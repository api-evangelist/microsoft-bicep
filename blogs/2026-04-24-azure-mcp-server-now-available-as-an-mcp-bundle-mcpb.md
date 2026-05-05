---
title: "Azure MCP Server now available as an MCP Bundle (.mcpb)"
url: "https://devblogs.microsoft.com/azure-sdk/azure-mcp-server-mcpb-support/"
date: "Fri, 24 Apr 2026 20:32:49 +0000"
author: "Victor Colin Amador"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>We&#8217;re excited to announce that the <a href="https://aka.ms/azmcp">Azure MCP Server</a> is now available as an <a href="https://github.com/modelcontextprotocol/mcpb">MCP Bundle</a> (<code>.mcpb</code>). This means you can install the Azure MCP Server into <a href="https://claude.com/download">Claude Desktop</a> and other MCP-compatible clients with minimum setup—no Node.js, Python, or .NET runtime required.</p>
<h2>What are MCP Bundles?</h2>
<p>MCP Bundles are a portable packaging format for MCP servers. Think of them like browser extensions (<code>.crx</code>) or VS Code extensions (<code>.vsix</code>), but for <a href="https://modelcontextprotocol.io">Model Context Protocol</a> servers. Each bundle is a ZIP archive containing:</p>
<ul>
<li>A <code>manifest.json</code> file describing the server&#8217;s metadata, tools, and runtime requirements.</li>
<li>The server binary and all of its dependencies—everything needed to run the server on a specific platform.</li>
</ul>
<p>The key benefit is simplicity. End users don&#8217;t need to install any runtimes, manage dependencies, or write configuration files. You download a <code>.mcpb</code> file, open it in a supported client, and the server is ready to use.</p>
<h2>Why MCP Bundles matter for Azure MCP Server users</h2>
<p>Until now, using the Azure MCP Server required one of the following runtimes:</p>
<table>
<thead>
<tr>
<th>Method</th>
<th>Runtime required</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>npm/npx</strong></td>
<td>Node.js</td>
</tr>
<tr>
<td><strong>pip/uvx</strong></td>
<td>Python</td>
</tr>
<tr>
<td><strong>dotnet</strong></td>
<td>.NET SDK</td>
</tr>
<tr>
<td><strong>Docker</strong></td>
<td>Docker Engine</td>
</tr>
</tbody>
</table>
<p>MCP Bundles change this paradigm by providing a self-contained binary that <strong>doesn&#8217;t require any additional runtime</strong>. This format is one of the easiest ways to get started with the Azure MCP Server, especially for users who aren&#8217;t developers or don&#8217;t want to manage runtimes.</p>
<h2>Get started in three steps</h2>
<h3>1. Download the MCP Bundle for your platform</h3>
<p>Go to the <strong>MCP Bundles</strong> section of the latest release post <a href="https://github.com/microsoft/mcp/releases?q=Azure.Mcp.Server">on GitHub</a> page. To download the corresponding <code>.mcpb</code> file, select the link that matches your operating system and architecture.</p>
<p><img alt="Download page" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-download.png" /></p>
<h3>2. Install in Claude Desktop</h3>
<h4>Drag and drop installation (recommended)</h4>
<p>The easiest way to install is to <strong>drag and drop the <code>.mcpb</code> file into the Claude Desktop window</strong>:</p>
<ol>
<li>Open the hamburger menu (☰) in the top left of Claude Desktop.
<p><img alt="Hamburger menu" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-hamburger.png" /></li>
<li>Go to <strong>File</strong> &gt; <strong>Settings</strong> &gt; <strong>Extensions</strong>.
<p><img alt="File and settings" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-file-settings.png" />
<img alt="Extensions page" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-extensions.png" /></li>
<li>Drag and drop the downloaded bundle into the <strong>Extensions</strong> page to install.
<p><img alt="Drag and drop" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-drag-drop.png" /></li>
<li>Review the server details and select <strong>Install</strong>.
<p><img alt="Review server details" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-review.png" /></li>
<li>Select <strong>Install</strong> again in the pop-up dialog.
<p><img alt="Install dialog" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-install.png" /></li>
<li>Once the server is installed, the <strong>Install</strong> button in the details pane changes to <strong>Uninstall</strong> and the server shows as enabled.
<p><img alt="Installation successful" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-success.png" /></li>
</ol>
<h4>Manual installation</h4>
<p>If you prefer a manual installation, follow these steps instead:</p>
<ol>
<li>From the <strong>Extensions</strong> page, go to <strong>Advanced Settings</strong> &gt; <strong>Install Extension</strong>.
<p><img alt="Advanced Settings" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-advanced-settings.png" />
<img alt="Install Extension button" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-install-extension.png" /></li>
<li>Select the downloaded <code>.mcpb</code> file and select <strong>Preview</strong>.
<p><img alt="Select file" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-select-file.png" /></li>
<li>Review the server details and select <strong>Install</strong>.
<p><img alt="Review server details" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-review.png" /></li>
<li>Select <strong>Install</strong> again in the pop-up dialogue.
<p><img alt="Install dialogue" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-install.png" /></li>
<li>Once the server is installed, the <strong>Install</strong> button in the details pane changes to <strong>Uninstall</strong> and the server shows as enabled.
<p><img alt="Installation successful" src="https://devblogs.microsoft.com/azure-sdk/wp-content/uploads/sites/58/2026/04/05-04-azure-mcp-server-support-mcpb-success.png" /></li>
</ol>
<blockquote><p><strong>Tip:</strong> You can also set Claude Desktop as the default app for <code>.mcpb</code> files, then simply double-click the bundle to install it.</p></blockquote>
<h3>3. Authenticate to Azure</h3>
<p>The Azure MCP Server uses your Azure credentials, so make sure you&#8217;re signed in before using Azure tools. The easiest way is to run the following <a href="https://learn.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest">Azure CLI</a> command in your terminal:</p>
<pre><code class="language-bash">az login</code></pre>
<p>For more authentication options (managed identities, service principals, etc.), see the <a href="https://github.com/microsoft/mcp/blob/main/docs/Authentication.md">Authentication guide</a>.</p>
<h2>What you can do</h2>
<p>Once installed, you have access to the full set of Azure MCP Server capabilities directly from your favorite client, including:</p>
<ul>
<li><strong>100+ Azure service tools</strong>—from Cosmos DB and Storage to Key Vault, App Service, and Microsoft Foundry</li>
<li><strong>Azure CLI command generation</strong>—get the right <code>az</code> commands for any task</li>
<li><strong>Infrastructure guidance</strong>—Bicep and Terraform template generation</li>
<li><strong>Architecture design</strong>—cloud architecture recommendations based on your requirements</li>
<li><strong>Diagnostics</strong>—resource health, monitoring, and troubleshooting</li>
</ul>
<p>Try prompts like:</p>
<ul>
<li>&#8220;List all resource groups in my subscription&#8221;</li>
<li>&#8220;Show me the secrets in my Key Vault named my-vault&#8221;</li>
<li>&#8220;Generate a Bicep template for a web app with a SQL database&#8221;</li>
<li>&#8220;What Cosmos DB databases do I have?&#8221;</li>
</ul>
<h2>How is the MCP Bundle different from the VS Code extension?</h2>
<p>Both provide access to the same Azure MCP Server and its tools. The difference is the client:</p>
<table>
<thead>
<tr>
<th>Option</th>
<th>Client</th>
<th>Best for</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>MCP Bundle</strong> (<code>.mcpb</code>)</td>
<td>Claude Desktop</td>
<td>Claude users who want a zero-config install</td>
</tr>
<tr>
<td><strong>VS Code Extension</strong></td>
<td>VS Code + GitHub Copilot</td>
<td>Developers working in VS Code</td>
</tr>
<tr>
<td><strong>npm/npx</strong></td>
<td>Any MCP client</td>
<td>Developers who already have Node.js</td>
</tr>
<tr>
<td><strong>pip/uvx</strong></td>
<td>Any MCP client</td>
<td>Python developers</td>
</tr>
<tr>
<td><strong>Docker</strong></td>
<td>Any MCP client</td>
<td>CI/CD pipelines and containerized environments</td>
</tr>
</tbody>
</table>
<p>Choose whichever method fits your workflow. The same tools and capabilities are available regardless of how you install the server.</p>
<h2>Troubleshooting</h2>
<p>If you run into issues installing the bundle:</p>
<ol>
<li><strong>Make sure Claude Desktop is up to date</strong>—download the latest version from <a href="https://claude.com/download">claude.com/download</a>.</li>
<li><strong>Verify you downloaded the correct platform</strong>—for example, use the <code>osx-arm64</code> bundle on Apple Silicon Macs, not <code>osx-x64</code>.</li>
<li><strong>Reinstall if needed</strong>—in Claude Desktop, go to <strong>File</strong> &gt; <strong>Settings</strong> &gt; <strong>Extensions</strong>, uninstall the Azure MCP Server, and install the bundle again.</li>
</ol>
<p>For more information, see the <a href="https://github.com/microsoft/mcp/blob/main/servers/Azure.Mcp.Server/TROUBLESHOOTING.md#mcpb-mcp-bundle-installation-issues">Troubleshooting guide</a>.</p>
<h2>Get started today</h2>
<ul>
<li><strong>Download:</strong> <a href="https://github.com/microsoft/mcp/releases?q=Azure.Mcp.Server-">GitHub Releases</a></li>
<li><strong>GitHub Repo:</strong> <a href="https://aka.ms/azmcp">aka.ms/azmcp</a></li>
<li><strong>Documentation:</strong> <a href="https://aka.ms/azmcp/docs">aka.ms/azmcp/docs</a></li>
<li><strong>VS Code Extension:</strong> <a href="https://aka.ms/azmcp/download/vscode">aka.ms/azmcp/download/vscode</a></li>
<li><strong>Create an Issue:</strong> <a href="https://aka.ms/azmcp/issues">aka.ms/azmcp/issues</a></li>
</ul>
<h2>Summary</h2>
<p>The Azure MCP Server is now available as an MCP Bundle, making it easier than ever to connect Claude Desktop to over 100 Azure services. Download the <code>.mcpb</code> for your platform, drag it into Claude Desktop, and start managing your Azure resources through natural language—no runtimes, no configuration files, no friction. Try it out and <a href="https://aka.ms/azmcp/issues">let us know what you think</a>!</p>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/azure-mcp-server-mcpb-support/">Azure MCP Server now available as an MCP Bundle (.mcpb)</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
