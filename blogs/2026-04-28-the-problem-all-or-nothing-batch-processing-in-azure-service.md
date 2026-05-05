---
title: "The problem: All-or-nothing batch processing in Azure Service Bus"
url: "https://devblogs.microsoft.com/azure-sdk/per-message-settlement-azure-service-bus/"
date: "Tue, 28 Apr 2026 18:47:37 +0000"
author: "Swapnil Nagar"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>Azure Service Bus is one of the most widely used messaging services for building event-driven applications on Azure. When you use Azure Functions with a Service Bus trigger in batch mode, your function receives multiple messages at once for efficient, high-throughput processing.</p>
<p>But what happens when one message in the batch fails? Assume your function receives a batch of 50 Service Bus messages. Forty-nine process perfectly, and one fails.</p>
<p>In the default model, the entire batch fails. All 50 messages go back on the queue and get reprocessed, including the 49 that already succeeded. This leads to:</p>
<ul>
<li><strong>Duplicate processing</strong>—messages that were already handled successfully get processed again</li>
<li><strong>Wasted compute</strong>—you pay for re-executing work that already completed</li>
<li><strong>Infinite retry loops</strong>—if that one &#8220;poison&#8221; message keeps failing, it blocks the entire batch indefinitely</li>
<li><strong>Idempotency burden</strong>—your downstream systems must handle duplicates gracefully, adding complexity to every consumer</li>
</ul>
<p>This pattern is the classic all-or-nothing batch failure problem. Azure Functions solves it with per-message settlement.</p>
<h2>The solution: Per-message settlement for Azure Service Bus</h2>
<p>Azure Functions gives you direct control over how each individual message is settled in real time, as you process it. Instead of treating the batch as all-or-nothing, you settle each message independently based on its processing outcome.</p>
<p>With Service Bus message settlement actions in Azure Functions, you can:</p>
<table>
<thead>
<tr>
<th>Action</th>
<th>What It Does</th>
</tr>
</thead>
<tbody>
<tr>
<td>Complete</td>
<td>Remove the message from the queue (successfully processed)</td>
</tr>
<tr>
<td>Abandon</td>
<td>Release the lock so the message returns to the queue for retry, optionally modifying application properties</td>
</tr>
<tr>
<td>Dead-letter</td>
<td>Move the message to the dead-letter queue (poison message handling)</td>
</tr>
<tr>
<td>Defer</td>
<td>Keep the message in the queue but make it only retrievable by sequence number</td>
</tr>
</tbody>
</table>
<p>This capability means in a batch of 50 messages, you can:</p>
<ul>
<li>Complete 47 that processed successfully</li>
<li>Abandon 2 that hit a transient error (with updated retry metadata)</li>
<li>Dead-letter 1 that&#8217;s malformed and never succeeds</li>
</ul>
<p>All in a single function invocation. No reprocessing of successful messages. No building failure response objects. No all-or-nothing.</p>
<h2>Why this matters</h2>
<h3>1. Eliminates duplicate processing</h3>
<p>When you complete messages individually, successfully processed messages are immediately removed from the queue. There&#8217;s no chance of them being redelivered, even if other messages in the same batch fail.</p>
<h3>2. Enables granular error handling</h3>
<p>Different failures deserve different treatments. A malformed message should be dead-lettered immediately. A message that failed due to a transient database timeout should be abandoned for retry. A message that requires manual intervention should be deferred. Per-message settlement gives you this granularity.</p>
<h3>3. Implements exponential backoff without external infrastructure</h3>
<p>By combining abandon with modified application properties, you can track retry counts per message, and implement exponential backoff patterns directly in your function code. No extra queues or Durable Functions are required.</p>
<h3>4. Reduces cost</h3>
<p>You stop paying for redundant re-execution of already-successful work. In high-throughput systems processing millions of messages, this approach can be a material cost reduction.</p>
<h3>5. Simplifies idempotency requirements</h3>
<p>When successful messages are never redelivered, your downstream systems don&#8217;t need to guard against duplicates as aggressively. This change reduces architectural complexity and potential for bugs.</p>
<h2>Before: One message = one function invocation</h2>
<p>Before batch support, there was no cardinality option, Azure Functions processed each Service Bus message as a separate function invocation. If your queue had 50 messages, the runtime spun up 50 individual executions.</p>
<h3>Single-message processing (the old way)</h3>
<pre><code class="language-typescript">import { app, InvocationContext } from '@azure/functions';

async function processOrder(
    message: unknown,  // ← One message at a time, no batch
    context: InvocationContext
): Promise&lt;void&gt; {
    try {
        const order = message as Order;
        await processOrder(order);
    } catch (error) {
        context.error('Failed to process message:', error);
        // Message auto-complete by default.
        throw error;
    }
}

app.serviceBusQueue('processOrder', {
    connection: 'ServiceBusConnection',
    queueName: 'orders-queue',
    handler: processOrder,
});</code></pre>
<p>What this cost you:</p>
<table>
<thead>
<tr>
<th>50 messages on the queue</th>
<th>Old (single-message)</th>
<th>New (batch + settlement)</th>
</tr>
</thead>
<tbody>
<tr>
<td>Function invocations</td>
<td>50 separate invocations</td>
<td>One invocation</td>
</tr>
<tr>
<td>Connection overhead</td>
<td>50 separate DB/API connections</td>
<td>One connection, reused across batch</td>
</tr>
<tr>
<td>Compute cost</td>
<td>50× invocation overhead</td>
<td>1× invocation overhead</td>
</tr>
<tr>
<td>Settlement control</td>
<td>Binary: throw or don&#8217;t</td>
<td>Four actions per message</td>
</tr>
</tbody>
</table>
<p>Every message paid the full price of a function invocation, startup, connection setup, teardown. At scale (millions of messages/day), this overhead was a significant cost and latency penalty. And when a message failed, your only option was to throw (retry the whole message) or swallow the error (lose it silently).</p>
<h2>Code examples</h2>
<p>Let&#8217;s see how this looks across all three major Azure Functions language stacks.</p>
<h3>Node.js (TypeScript with @azure/functions-extensions-servicebus)</h3>
<pre><code class="language-typescript">import '@azure/functions-extensions-servicebus';
import { app, InvocationContext } from '@azure/functions';
import { ServiceBusMessageContext, messageBodyAsJson } from '@azure/functions-extensions-servicebus';

interface Order { id: string; product: string; amount: number; }

export async function processOrderBatch(
    sbContext: ServiceBusMessageContext,
    context: InvocationContext
): Promise&lt;void&gt; {
    const { messages, actions } = sbContext;

    for (const message of messages) {
        try {
            const order = messageBodyAsJson&lt;Order&gt;(message);
            await processOrder(order);
            await actions.complete(message);            // &#x2705; Done
        } catch (error) {
            context.error(`Failed ${message.messageId}:`, error);
            await actions.deadletter(message);          // &#x2620; Poison
        }
    }
}

app.serviceBusQueue('processOrderBatch', {
    connection: 'ServiceBusConnection',
    queueName: 'orders-queue',
    sdkBinding: true,
    autoCompleteMessages: false,
    cardinality: 'many',
    handler: processOrderBatch,
});</code></pre>
<p><strong>Key points:</strong></p>
<ul>
<li>Enable <code>sdkBinding: true</code> and <code>autoCompleteMessages: false</code> to gain manual settlement control</li>
<li><code>ServiceBusMessageContext</code> provides both the <code>messages</code> array and <code>actions</code> object</li>
<li>Settlement actions: <code>complete()</code>, <code>abandon()</code>, <code>deadletter()</code>, <code>defer()</code></li>
<li>Application properties can be passed to <code>abandon()</code> for retry tracking</li>
<li>Built-in helpers like <code>messageBodyAsJson&lt;T&gt;()</code> handle Buffer-to-object parsing</li>
</ul>
<p>Full sample: <a href="https://github.com/Azure/azure-functions-nodejs-extensions/tree/main/azure-functions-nodejs-extensions-servicebus/samples/serviceBusSampleWithComplete">serviceBusSampleWithComplete</a></p>
<h3>Python (V2 programming model)</h3>
<pre><code class="language-python">import json
import logging
from typing import List

import azure.functions as func
import azurefunctions.extensions.bindings.servicebus as servicebus

app = func.FunctionApp(http_auth_level=func.AuthLevel.FUNCTION)

@app.service_bus_queue_trigger(arg_name="messages",
                               queue_name="orders-queue",
                               connection="SERVICEBUS_CONNECTION",
                               auto_complete_messages=False,
                               cardinality="many")
def process_order_batch(messages: List[servicebus.ServiceBusReceivedMessage],
                        message_actions: servicebus.ServiceBusMessageActions):
    for message in messages:
        try:
            order = json.loads(message.body)
            process_order(order)
            message_actions.complete(message)              # &#x2705; Done
        except Exception as e:
            logging.error(f"Failed {message.message_id}: {e}")
            message_actions.dead_letter(message)           # &#x2620; Poison

def process_order(order):
    logging.info(f"Processing order: {order['id']}")</code></pre>
<p><strong>Key points:</strong></p>
<ul>
<li>Uses <code>azurefunctions.extensions.bindings.servicebus</code> for SDK-type bindings with <code>ServiceBusReceivedMessage</code></li>
<li>The extension supports both queue and topic triggers with <code>cardinality="many"</code> for batch processing</li>
<li>Each message exposes SDK properties like <code>body</code>, <code>enqueued_time_utc</code>, <code>lock_token</code>, <code>message_id</code>, and <code>sequence_number</code></li>
</ul>
<p>Full sample: <a href="https://github.com/Azure/azure-functions-python-extensions/tree/dev/azurefunctions-extensions-bindings-servicebus/samples/servicebus_samples_settlement">servicebus_samples_settlement</a></p>
<h3>.NET (C# Isolated Worker)</h3>
<pre><code class="language-csharp">using Azure.Messaging.ServiceBus;
using Microsoft.Azure.Functions.Worker;

public class ServiceBusBatchProcessor(ILogger&lt;ServiceBusBatchProcessor&gt; logger)
{
    [Function(nameof(ProcessOrderBatch))]
    public async Task ProcessOrderBatch(
        [ServiceBusTrigger("orders-queue", Connection = "ServiceBusConnection")]
        ServiceBusReceivedMessage[] messages,
        ServiceBusMessageActions messageActions)
    {
        foreach (var message in messages)
        {
            try
            {
                var order = message.Body.ToObjectFromJson&lt;Order&gt;();
                await ProcessOrder(order);
                await messageActions.CompleteMessageAsync(message);       // &#x2705; Done
            }
            catch (Exception ex)
            {
                logger.LogError(ex, "Failed {MessageId}", message.MessageId);
                await messageActions.DeadLetterMessageAsync(message);     // &#x2620; Poison
            }
        }
    }

    private Task ProcessOrder(Order order) =&gt; Task.CompletedTask;
}

public record Order(string Id, string Product, decimal Amount);</code></pre>
<p><strong>Key points:</strong></p>
<ul>
<li>Inject <code>ServiceBusMessageActions</code> directly alongside the message array</li>
<li>Each message is individually settled with <code>CompleteMessageAsync</code>, <code>DeadLetterMessageAsync</code>, or <code>AbandonMessageAsync</code></li>
<li>Application properties can be modified on abandon to track retry metadata</li>
</ul>
<p>Full sample: <a href="https://github.com/Azure/azure-functions-dotnet-worker/blob/main/samples/Extensions/ServiceBus/ServiceBusReceivedMessageFunctions.cs">ServiceBusReceivedMessageFunctions.cs</a></p>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/per-message-settlement-azure-service-bus/">The problem: All-or-nothing batch processing in Azure Service Bus</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
