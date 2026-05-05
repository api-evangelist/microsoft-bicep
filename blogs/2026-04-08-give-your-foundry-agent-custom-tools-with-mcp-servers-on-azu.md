---
title: "Give your Foundry Agent Custom Tools with MCP Servers on Azure Functions"
url: "https://devblogs.microsoft.com/azure-sdk/give-your-foundry-agent-custom-tools-with-mcp-servers-on-azure-functions/"
date: "Wed, 08 Apr 2026 15:00:50 +0000"
author: "Lily Ma"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>This blog post is for developers who have an MCP server deployed to Azure Functions and want to connect it to Microsoft Foundry agents. It walks through why you&#8217;d want to do this, the different authentication options available, and how to get your agent calling your MCP tools.</p>
<p><img alt="Connecting to Foundry agent through OAuth Identity Passthrough demo" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/04-06-foundry_oauth_id_passthrough.gif" /></p>
<h2>Connect your MCP server on Azure Functions to Foundry Agent</h2>
<p>Azure Functions is a great place to host remote MCP servers with scalable infrastructure, built-in auth, and serverless billing. But hosting an MCP server is only half the picture. The real value comes when something actually uses those tools.</p>
<p>Microsoft Foundry lets you build AI agents that can reason, plan, and take actions. By connecting your MCP server to an agent, you&#8217;re giving it access to your custom tools, whether that&#8217;s querying a database, calling an API, or running some business logic. The agent discovers your tools, decides when to call them, and uses the results to respond to the user.</p>
<h3>Why connect MCP servers to Foundry agents?</h3>
<p>You might already have an MCP server that works great with VS Code, Visual Studio, Cursor, or other MCP clients. Connecting that same server to a Foundry agent means you can reuse those tools in a completely different context. For example, in an enterprise AI agent that your team or customers interact with. No need to rebuild anything. Your MCP server stays the same; you&#8217;re just adding another consumer.</p>
<h3>Prerequisites</h3>
<p>Before proceeding, make sure you have the following:</p>
<ol>
<li>An MCP server deployed to Azure Functions. If you don&#8217;t have one yet, you can deploy one quickly by following one of the samples:
<ul>
<li><a href="https://github.com/Azure-Samples/remote-mcp-functions-python">Python</a></li>
<li><a href="https://github.com/Azure-Samples/remote-mcp-functions-typescript">TypeScript</a></li>
<li><a href="https://github.com/Azure-Samples/remote-mcp-functions-dotnet">.NET</a></li>
<li><a href="https://github.com/Azure-Samples/remote-mcp-functions-java">Java</a></li>
</ul>
</li>
<li>A <a href="https://learn.microsoft.com/azure/foundry/tutorials/quickstart-create-foundry-resources?view=foundry&amp;tabs=portal">Foundry project with a deployed model</a> and a <a href="https://learn.microsoft.com/azure/foundry/quickstarts/get-started-code?view=foundry&amp;tabs=portal">Foundry agent</a></li>
</ol>
<h3>Authentication options</h3>
<p>Depending on where you are in development, you can pick what makes sense and upgrade later. Here&#8217;s a summary:</p>
<table>
<thead>
<tr>
<th>Method</th>
<th>Description</th>
<th>Use case</th>
</tr>
</thead>
<tbody>
<tr>
<td>Key-based (default)</td>
<td>Agent authenticates by passing a shared <a href="https://learn.microsoft.com/azure/azure-functions/function-keys-how-to">function access key</a> in the request header. This method is the default authentication for HTTP endpoints in Functions.</td>
<td>Use during development or when the MCP server doesn&#8217;t require Microsoft Entra authentication.</td>
</tr>
<tr>
<td>Microsoft Entra</td>
<td>Agent authenticates using either its own identity (<em>agent identity</em>) or the shared identity of the Foundry project (<em>project managed identity</em>).</td>
<td>Use agent identity for production scenarios, but limit shared identity to development.</td>
</tr>
<tr>
<td>OAuth identity passthrough</td>
<td>Agent prompts users to sign in and authorize access, using the provided token to authenticate.</td>
<td>Use in production when each user must authenticate with their own identity and user context must be persisted.</td>
</tr>
<tr>
<td>Unauthenticated access</td>
<td>Agent makes unauthenticated calls.</td>
<td>Use during development or when your MCP server accesses only public information.</td>
</tr>
</tbody>
</table>
<h3>Connect your MCP server to your Foundry agent</h3>
<p>If your server uses key-based auth or is unauthenticated, it should be relatively straightforward to set up the connection from a Foundry agent.</p>
<p>The Microsoft Entra and OAuth identity passthrough are options that require extra steps to set up. Check out <a href="https://learn.microsoft.com/azure/azure-functions/functions-mcp-foundry-tools?tabs=entra%2Cmcp-extension%2Cfoundry">detailed step-by-step instructions</a> for each authentication method.</p>
<p>At a high level, the process looks like this:</p>
<ol>
<li><strong>Enable <a href="https://learn.microsoft.com/azure/azure-functions/functions-mcp-tutorial?tabs=mcp-extension&amp;pivots=programming-language-python#enable-built-in-server-authorization-and-authentication">built-in MCP authentication</a></strong>: When you deploy a server to Azure Functions, key-based auth is the default. You&#8217;ll need to disable that and enable built-in MCP auth instead. If you deployed one of the sample servers in the <em>Prerequisites</em> section, this step is already done for you.</li>
<li><strong>Get your MCP server endpoint URL</strong>: For MCP extension-based servers, it&#8217;s <code>https://&lt;FUNCTION_APP_NAME&gt;.azurewebsites.net/runtime/webhooks/mcp</code>.</li>
<li><strong>Get your credentials based on your chosen auth method</strong>: A managed identity configuration, OAuth credentials, or a function key.</li>
<li><strong>Add the MCP server as a tool in the Foundry portal</strong> by navigating to your agent, adding a new MCP tool, and providing the endpoint and credentials.</li>
</ol>
<p><strong>Microsoft Entra connection required fields:</strong></p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>Name</td>
<td>A unique identifier for your MCP server. You can use your function app name.</td>
<td><code>contoso-mcp-tools</code></td>
</tr>
<tr>
<td>Remote MCP Server endpoint</td>
<td>The URL endpoint for your MCP server.</td>
<td><code>https://contoso-mcp-tools.azurewebsites.net/runtime/webhooks/mcp</code></td>
</tr>
<tr>
<td>Authentication</td>
<td>The authentication method to use.</td>
<td><code>Microsoft Entra</code></td>
</tr>
<tr>
<td>Type</td>
<td>The identity type the agent uses to authenticate.</td>
<td><code>Project Managed Identity</code></td>
</tr>
<tr>
<td>Audience</td>
<td>The Application ID URI of your function app&#8217;s Entra registration. This value tells the identity provider which app the token is intended for.</td>
<td><code>api://00001111-aaaa-2222-bbbb-3333cccc4444</code></td>
</tr>
</tbody>
</table>
<p><strong>OAuth identity passthrough required fields:</strong></p>
<table style="width: 100%;">
<thead>
<tr>
<th style="width: 9.06367%;">Field</th>
<th style="width: 60.7491%;">Description</th>
<th style="width: 29.3633%;">Example</th>
</tr>
</thead>
<tbody>
<tr>
<td style="width: 9.06367%;">Name</td>
<td style="width: 60.7491%;">A unique identifier for your MCP server. You can use your function app name.</td>
<td style="width: 29.3633%;"><code>contoso-mcp-tools</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Remote MCP Server endpoint</td>
<td style="width: 60.7491%;">The URL endpoint for your MCP server.</td>
<td style="width: 29.3633%;"><code>https://contoso-mcp-tools.azurewebsites.net/runtime/webhooks/mcp</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Authentication</td>
<td style="width: 60.7491%;">The authentication method to use.</td>
<td style="width: 29.3633%;"><code>OAuth Identity Passthrough</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Client ID</td>
<td style="width: 60.7491%;">The client ID of your function app Entra registration.</td>
<td style="width: 29.3633%;"><code>00001111-aaaa-2222-bbbb-3333cccc4444</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Client secret</td>
<td style="width: 60.7491%;">The client secret of your function app Entra registration.</td>
<td style="width: 29.3633%;"><code>abcEFGhijKLMNopqRST</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Token URL</td>
<td style="width: 60.7491%;">The endpoint your server app calls to exchange an authorization code or credential for an access token.</td>
<td style="width: 29.3633%;"><code>https://login.microsoftonline.com/&lt;TENANT_ID&gt;/oauth2/v2.0/token</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Auth URL</td>
<td style="width: 60.7491%;">The endpoint where users are redirected to authenticate and grant authorization to your server app.</td>
<td style="width: 29.3633%;"><code>https://login.microsoftonline.com/&lt;TENANT_ID&gt;/oauth2/v2.0/authorize</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Refresh URL</td>
<td style="width: 60.7491%;">The endpoint used to obtain a new access token when the current one expires.</td>
<td style="width: 29.3633%;"><code>https://login.microsoftonline.com/&lt;TENANT_ID&gt;/oauth2/v2.0/token</code></td>
</tr>
<tr>
<td style="width: 9.06367%;">Scopes</td>
<td style="width: 60.7491%;">The specific permissions or resource access levels your server app requests from the authorization server.</td>
<td style="width: 29.3633%;"><code>api://00001111-aaaa-2222-bbbb-3333cccc4444/user_impersonation</code></td>
</tr>
</tbody>
</table>
<p>Once the server is configured as a tool, test it in the Agent Builder playground by sending a prompt that triggers one of your MCP tools.</p>
<h2>Closing thoughts</h2>
<p>What I find exciting about this is the composability. You build your MCP server once and it works everywhere: VS Code, Visual Studio, Cursor, ChatGPT, and now Foundry agents. The MCP protocol is becoming the universal interface for tool use in AI, and Azure Functions makes it easy to host these servers at scale and with security.</p>
<p>Are you building agents with Foundry? Have you connected your MCP servers to other clients? I&#8217;d love to hear what tools you&#8217;re exposing and how you&#8217;re using them. Share with us your thoughts!</p>
<h3>What&#8217;s next</h3>
<p>In the next blog post, we&#8217;ll go deeper into other MCP topics and cover new MCP features and developments in Azure Functions. Stay tuned!</p>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/give-your-foundry-agent-custom-tools-with-mcp-servers-on-azure-functions/">Give your Foundry Agent Custom Tools with MCP Servers on Azure Functions</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
