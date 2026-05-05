---
title: "GitHub Copilot meets Azure Developer CLI: AI-assisted project setup and error troubleshooting"
url: "https://devblogs.microsoft.com/azure-sdk/azd-copilot-integration/"
date: "Tue, 21 Apr 2026 00:38:49 +0000"
author: "Kristen Womack"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>The Azure Developer CLI (<code>azd</code>) now integrates with GitHub Copilot in two ways: AI-assisted project scaffolding during <code>azd init</code> and intelligent error troubleshooting when commands fail. Both features keep you in the terminal and use Copilot&#8217;s reasoning to reduce manual work.</p>
<h2>Prerequisites</h2>
<p>To use these features, you need:</p>
<ul>
<li><strong>azd 1.23.11 or later</strong>—Run <code>azd version</code> to check, or <code>azd update</code> to get the latest</li>
<li><strong>GitHub Copilot access</strong>—An active GitHub Copilot subscription (Individual, Business, or Enterprise)</li>
<li><strong>GitHub CLI (<code>gh</code>)</strong>—<code>azd</code> automatically checks and prompts for login if needed</li>
</ul>
<h2>Copilot-powered project setup with <code>azd init</code></h2>
<p>Running <code>azd init</code> now presents a &#8220;Set up with GitHub Copilot (Preview)&#8221; option. Select it, and Copilot analyzes your codebase to scaffold the project, which includes: generating <code>azure.yaml</code>, infrastructure templates, and deployment configuration based on your code&#8217;s language, framework, and dependencies.</p>
<p>This approach works whether you&#8217;re starting fresh or bringing an existing app to Azure. Building a new project? Copilot can help you create it from scratch with the right Azure infrastructure. Have an existing project you want to deploy? Copilot can analyze your codebase and generate the configuration needed to deploy it with <code>azd</code> with no need to rewrite or restructure your project.</p>
<p>Before making any changes, the flow runs preflight checks. It verifies your git working directory is clean so no uncommitted work is at risk. It also prompts for Model Context Protocol (MCP) server tool consent upfront, so you know exactly what Copilot can access before it starts.</p>
<p>To use Copilot-powered init:</p>
<pre><code class="language-bash">azd init
# Select: "Set up with GitHub Copilot (Preview)"</code></pre>
<p>The Copilot agent examines your project structure, proposes an <code>azure.yaml</code> configuration, and generates the necessary infrastructure files. You review and approve the changes before anything is written to disk.</p>
<h3>Example: scaffolding a Node.js app</h3>
<p>Say you have an Express API with a <code>package.json</code>, a <code>src/</code> directory, and a PostgreSQL dependency. Normally you&#8217;d need to:</p>
<ol>
<li>Reference the docs to determine the right <code>host</code> type (Container App vs App Service vs Function)</li>
<li>Write an <code>azure.yaml</code> with the correct <code>language</code>, <code>host</code>, and <code>build</code> settings</li>
<li>Author Bicep modules for the app, the database, and any networking</li>
</ol>
<p>With Copilot-powered init, Copilot detects your Express framework and PostgreSQL dependency, then generates the <code>azure.yaml</code>, a Bicep module for Azure Container Apps, and a Bicep module for Azure Database for PostgreSQL, which is then ready for you to review.</p>
<h2>AI-assisted error troubleshooting</h2>
<p>Deployment errors happen. A missing parameter, a permissions issue, a misconfigured resource, and, in these cases, the error message doesn&#8217;t always make the fix obvious.</p>
<h3>Troubleshooting with and without Copilot</h3>
<p>Without Copilot, when <code>azd provision</code> or <code>azd up</code> fails, the steps to fix it look something like this:</p>
<ol>
<li>Copy the error message from the terminal</li>
<li>Search Azure docs or Stack Overflow for the error code</li>
<li>Read through troubleshooting guides to find the relevant fix</li>
<li>Run one or more <code>az</code> CLI commands to apply the fix</li>
<li>Rerun the original <code>azd</code> command and hope it works</li>
</ol>
<p>With Copilot, <code>azd</code> can handle that loop for you. When any <code>azd</code> command fails, it offers an interactive troubleshooting flow powered by Copilot. You can jump straight to a fix or explore the error first. You choose from four options:</p>
<ul>
<li><strong>Explain</strong>—Get a plain-language explanation of what went wrong</li>
<li><strong>Guidance</strong>—Receive step-by-step instructions to fix the issue</li>
<li><strong>Diagnose and Guide</strong>—Get troubleshooting steps on what happened, why the error happened, and how to fix it. Let Copilot apply a fix (with your approval), then optionally retry the failed command</li>
<li><strong>Skip</strong>—Dismiss and handle it yourself</li>
</ul>
<p>The troubleshooting runs entirely in your terminal. There&#8217;s no need to switch to a browser, search docs, or paste error messages into a chat window. Copilot has context about your project configuration, the command that failed, and the error details, so its suggestions are specific to your situation.</p>
<h3>Set a default error handling behavior</h3>
<p>If you find yourself always choosing the same option, you can skip the interactive prompt by setting a default with <code>azd config</code>:</p>
<pre><code class="language-bash">azd config set copilot.errorHandling.category troubleshoot</code></pre>
<p>Available values for <code>copilot.errorHandling.category</code>:</p>
<table>
<thead>
<tr>
<th>Value</th>
<th>Behavior</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>explain</code></td>
<td>Automatically get a plain-language explanation</td>
</tr>
<tr>
<td><code>guidance</code></td>
<td>Automatically get step-by-step fix instructions</td>
</tr>
<tr>
<td><code>troubleshoot</code></td>
<td>Automatically diagnose and guide</td>
</tr>
<tr>
<td><code>fix</code></td>
<td>Automatically apply a fix</td>
</tr>
<tr>
<td><code>skip</code></td>
<td>Always skip Copilot troubleshooting</td>
</tr>
</tbody>
</table>
<p>You can also enable auto-fix and retry, so Copilot applies the fix and reruns the failed command automatically:</p>
<pre><code class="language-bash">azd config set copilot.errorHandling.fix allow</code></pre>
<p>To reset to the default interactive prompt, unset the config:</p>
<pre><code class="language-bash">azd config unset copilot.errorHandling.category</code></pre>
<h3>Common Azure deployment errors where Copilot helps</h3>
<p>Here are a few common Azure errors that developers run into and how Copilot-assisted troubleshooting turns a wall of text into an actionable fix.</p>
<h4><code>MissingSubscriptionRegistration</code>—resource provider not registered</h4>
<p>A first-time deployment to a subscription often fails with:</p>
<pre><code>ERROR: deployment failed: MissingSubscriptionRegistration:
The subscription is not registered to use namespace 'Microsoft.App'.</code></pre>
<p>Azure requires resource providers to be registered before you can create certain resource types. If this Container App deployment is the first deployment in a given subscription, <code>Microsoft.App</code> isn&#8217;t registered yet. Copilot&#8217;s <strong>Troubleshoot</strong> option can register the provider for you and run the deployment automatically.</p>
<h4><code>SkuNotAvailable</code> / <code>OperationNotAllowed</code>—SKU or quota limits</h4>
<p>You pick a region and hit:</p>
<pre><code>ERROR: deployment failed: SkuNotAvailable:
The requested VM size 'Standard_D2s_v3' is not available in location 'westus'.</code></pre>
<p>Or a quota variant:</p>
<pre><code>ERROR: deployment failed: OperationNotAllowed:
Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.</code></pre>
<p>These errors are common when a region is capacity-constrained or your subscription hits its vCPU limit. Copilot&#8217;s <strong>Explain</strong> option clarifies which SKU or quota is blocked, and <strong>Guidance</strong> suggests alternative regions or Virtual Machine (VM) sizes that are available or shows you how to request a quota increase.</p>
<h4><code>StorageAccountAlreadyTaken</code>—globally unique name collision</h4>
<pre><code>ERROR: deployment failed: StorageAccountAlreadyTaken:
The storage account named 'myappstorage' is already taken.</code></pre>
<p>Storage account names must be unique across all of Azure. Copilot suggests updating your Bicep parameter or <code>azure.yaml</code> environment variable with a unique name, often by appending your environment name or a random suffix, and then retrying the deployment.</p>
<h2>What&#8217;s next</h2>
<p>This integration is just the beginning of Copilot features in <code>azd</code>. The team is working on deeper agent capabilities including Copilot-assisted infrastructure customization and smarter multi-service orchestration. If you want to influence what gets built next, <a href="https://aka.ms/azd-user-research-signup">sign up for user research</a>, reach out to us by filing an issue, or start a discussion in the repo.</p>
<h2>Try it out</h2>
<p>To check your current version, run:</p>
<pre><code class="language-bash">azd version</code></pre>
<p>To update to the latest version:</p>
<pre><code class="language-bash">azd update</code></pre>
<p>For a fresh install, see <a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd">Install azd</a>. For more details on the March 2026 release, read the <a href="https://devblogs.microsoft.com/azure-sdk/azure-developer-cli-azd-march-2026/">full release blog post</a>.</p>
<h2>Feedback</h2>
<p>Have questions or ideas? File an issue or start a discussion on <a href="https://github.com/Azure/azure-dev">GitHub</a>. Want to help shape the future of <code>azd</code>? <a href="https://aka.ms/azd-user-research-signup">Sign up for user research</a>.</p>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/azd-copilot-integration/">GitHub Copilot meets Azure Developer CLI: AI-assisted project setup and error troubleshooting</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
