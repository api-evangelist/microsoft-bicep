---
title: "MCP as Easy as 1-2-3: Introducing the Fluent API for MCP Apps"
url: "https://devblogs.microsoft.com/azure-sdk/mcp-as-easy-as-1-2-3-introducing-the-fluent-api-for-mcp-apps/"
date: "Tue, 07 Apr 2026 21:16:11 +0000"
author: "Lilian Kasem (she/her)"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>Earlier this year, we <a href="https://techcommunity.microsoft.com/blog/appsonazureblog/building-mcp-apps-with-azure-functions-mcp-extension/4496536">introduced MCP Apps</a> in the Azure Functions MCP extension. These are tools that go beyond text and render full UI experiences, serve static assets, and integrate seamlessly with AI agents. If you haven&#8217;t tried them yet, the <a href="https://learn.microsoft.com/azure/azure-functions/scenario-mcp-apps?tabs=bash%2Clinux&amp;pivots=programming-language-csharp">MCP Apps quickstart</a> is a great place to start.</p>
<p>Today, we&#8217;re making Model Context Protocol (MCP) Apps even easier to build. We&#8217;re introducing a fluent configuration API for the .NET isolated worker that lets you promote any MCP tool to a full MCP App complete with views, permissions, and security policies in just a few lines of code.</p>
<h2>What are MCP Apps?</h2>
<p>MCP Apps extend the <a href="https://modelcontextprotocol.io/introduction">Model Context Protocol</a> tool model by allowing individual tools to be configured as apps. Tools that come with their own UI views, static assets, and fine-grained security controls. Think of them as MCP tools that can present rich, interactive experiences to users while still being invocable by AI agents.</p>
<p>With MCP Apps, you can:</p>
<ul>
<li><strong>Attach HTML views</strong> to your tools, rendered by the MCP client.</li>
<li><strong>Serve static assets</strong> (HTML, CSS, JavaScript, images) alongside your tool.</li>
<li><strong>Control permissions</strong> like clipboard access, allowing your app to interact with the user&#8217;s environment in a controlled way.</li>
<li><strong>Define Content Security Policies (CSP)</strong> to lock down what your app can load and connect to.</li>
</ul>
<h2>Why a Fluent API?</h2>
<p>One of the key goals of the Fluent API is to abstract away the MCP spec so you don&#8217;t need to know the protocol details to build an app. In the MCP protocol, connecting a tool to a UI view requires precise coordination: a resource endpoint at a <code>ui://</code> URI, a special mime type (<code>text/html;profile=mcp-app</code>) that tells clients to render the content as an interactive app, and <code>_meta.ui</code> metadata on both the tool and resource to wire everything together. For example, the tool metadata carries a <code>resourceUri</code> and <code>visibility</code> so the client knows the tool has a UI, while the resource response carries the CSP, permissions, and rendering hints so the client knows how to display it securely.</p>
<p>The Fluent API handles all of this coordination for you. When you call <code>AsMcpApp</code>, the extension automatically generates the synthetic resource function, sets the correct mime type, and injects the metadata that connects your tool to its view. You just write your function, point it at an HTML file, and configure the security policies you need.</p>
<h2>Get started</h2>
<p>The Fluent API for MCP Apps is available as a preview in the <code>Microsoft.Azure.Functions.Worker.Extensions.Mcp</code> NuGet package. If you&#8217;re already building MCP tools with Azure Functions, you&#8217;re just a package update away:</p>
<pre><code class="language-bash">dotnet add package Microsoft.Azure.Functions.Worker.Extensions.Mcp --version 1.5.0-preview.1</code></pre>
<p>This preview package includes all the fluent configuration APIs covered in this post. Since this is a preview release, APIs may change based on your feedback before the stable release.</p>
<h2>The Fluent API</h2>
<p>Here&#8217;s the complete setup for a simple &#8220;Hello App&#8221;:</p>
<h3>Step 1: Define your function</h3>
<p>Start with a standard Azure Functions MCP tool. The <code>[McpToolTrigger]</code> attribute wires up your function as an MCP tool, just like before:</p>
<pre><code class="language-csharp">[Function(nameof(HelloApp))]
public string HelloApp(
    [McpToolTrigger("HelloApp", "A simple MCP App that says hello.")] ToolInvocationContext context)
{
    return "Hello from app";
}</code></pre>
<h3>Step 2: Configure it as an MCP App</h3>
<p>In your program startup, use the Fluent API to promote your tool to a full MCP App:</p>
<pre><code class="language-csharp">builder.ConfigureMcpTool("HelloApp")
    .AsMcpApp(app =&gt; app
        .WithView("assets/hello-app.html")
        .WithTitle("Hello App")
        .WithPermissions(McpAppPermissions.ClipboardWrite | McpAppPermissions.ClipboardRead)
        .WithCsp(csp =&gt;
        {
            csp.AllowBaseUri("https://www.microsoft.com")
               .ConnectTo("https://www.microsoft.com");
        }));</code></pre>
<h3>Step 3: Add your view</h3>
<p>Create an HTML file at <code>assets/hello-app.html</code> in your project. This is the UI that MCP clients render when your app tool is invoked. You have full control over the markup, styling, and client-side behavior.</p>
<p>That&#8217;s it. Three steps and your MCP tool is now an MCP App with a rich UI.</p>
<h2>Breaking down the API</h2>
<p>Let&#8217;s walk through each part of the fluent configuration.</p>
<h3><code>ConfigureMcpTool("HelloApp")</code></h3>
<p>Selects the MCP tool you want to configure. The name must match the function name registered with <code>[McpToolTrigger]</code>.</p>
<h3><code>.AsMcpApp(app =&gt; ...)</code></h3>
<p>Promotes the tool to an MCP App. Everything inside the lambda configures the app-specific behavior via the <code>IMcpAppBuilder</code> interface.</p>
<h3><code>.WithView(...)</code></h3>
<p>Sets the view for the app. You can provide a file path directly, or use one of the <code>McpViewSource</code> factory methods for more control:</p>
<pre><code class="language-csharp">// File on disk (relative to output directory)
app.WithView("assets/hello-app.html")

// Explicit file source
app.WithView(McpViewSource.FromFile("assets/hello-app.html"))

// Embedded resource from an assembly
app.WithView(McpViewSource.FromEmbeddedResource("MyApp.Resources.view.html"))</code></pre>
<p>The <code>McpViewSource</code> abstraction lets you choose where your HTML lives: on disk alongside your function, or embedded directly in your assembly for self-contained deployment.</p>
<h3><code>.WithTitle("Hello App")</code></h3>
<p>Sets a human-readable title for the view, displayed by MCP clients in their UI.</p>
<h3><code>.WithBorder()</code></h3>
<p>Hints to the MCP client that it should render a border around the view. Pass <code>false</code> to explicitly opt out, or omit it entirely to let the client decide:</p>
<pre><code class="language-csharp">.WithBorder()       // prefer border
.WithBorder(false)  // prefer no border</code></pre>
<h3><code>.WithDomain("myapp.example.com")</code></h3>
<p>Sets a domain hint for the view, used by the host to scope cookies and storage for your app.</p>
<h3><code>.WithPermissions(...)</code></h3>
<p>Controls what the app is allowed to do in the client environment. Permissions are defined as flags on <code>McpAppPermissions</code>:</p>
<table>
<thead>
<tr>
<th>Permission</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>ClipboardRead</code></td>
<td>Allows the app to read from the clipboard</td>
</tr>
<tr>
<td><code>ClipboardWrite</code></td>
<td>Allows the app to write to the clipboard</td>
</tr>
</tbody>
</table>
<p>Permissions are opt-in, so your app only gets the access it explicitly requests.</p>
<h3><code>.WithCsp(csp =&gt; ...)</code></h3>
<p>Defines the Content Security Policy for the app&#8217;s view. The CSP builder (<code>IMcpCspBuilder</code>) provides four methods, each mapping to standard CSP directives:</p>
<table>
<thead>
<tr>
<th>Method</th>
<th>CSP Directive</th>
<th>Purpose</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>ConnectTo(origin)</code></td>
<td><code>connect-src</code></td>
<td>Network requests (fetch, XMLHttpRequest, WebSocket)</td>
</tr>
<tr>
<td><code>LoadResourcesFrom(origin)</code></td>
<td><code>img-src</code>, <code>script-src</code>, <code>style-src</code>, <code>font-src</code>, <code>media-src</code></td>
<td>Static resources (scripts, images, styles, fonts, media)</td>
</tr>
<tr>
<td><code>AllowFrame(origin)</code></td>
<td><code>frame-src</code></td>
<td>Nested iframes</td>
</tr>
<tr>
<td><code>AllowBaseUri(origin)</code></td>
<td><code>base-uri</code></td>
<td>Base URI for the document</td>
</tr>
</tbody>
</table>
<pre><code class="language-csharp">.WithCsp(csp =&gt;
{
    csp.ConnectTo("https://api.example.com")
       .LoadResourcesFrom("https://cdn.example.com")
       .AllowFrame("https://youtube.com")
       .AllowBaseUri("https://www.microsoft.com");
})</code></pre>
<p>You can call <code>WithCsp</code> multiple times. Origins accumulate across calls, so you can compose CSP configuration from multiple sources. By default, the CSP is restrictive; you explicitly allowlist the origins your app needs, following the principle of least privilege.</p>
<h3><code>.WithVisibility(...)</code></h3>
<p>Controls who can see the tool. The <code>McpVisibility</code> flags enum has two values:</p>
<table>
<thead>
<tr>
<th>Visibility</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>Model</code></td>
<td>Visible to the large language model (LLM) during tool selection</td>
</tr>
<tr>
<td><code>App</code></td>
<td>Visible to the host UI for rendering</td>
</tr>
</tbody>
</table>
<p>By default, tools are visible to both (<code>Model | App</code>). You can restrict visibility if, for example, you want a tool that only renders UI and shouldn&#8217;t be invoked by the model directly:</p>
<pre><code class="language-csharp">.ConfigureApp()
.WithVisibility(McpVisibility.App)  // UI-only, hidden from the model</code></pre>
<h3><code>.WithStaticAssets(...)</code></h3>
<p>Configures a directory from which static assets (CSS, JS, images) are served alongside your view:</p>
<pre><code class="language-csharp">.ConfigureApp()
.WithStaticAssets("assets/dist")

// Or with options
.WithStaticAssets("assets/dist", options =&gt;
{
    options.IncludeSourceMaps = true;  // default: false, to avoid leaking internal paths
})</code></pre>
<p>By default, <code>.map</code> files are excluded from serving to avoid leaking internal paths and implementation details.</p>
<h3>Builder navigation</h3>
<p>The Fluent API uses three builder levels (tool, app, and view), and you can navigate between them:</p>
<pre><code class="language-csharp">builder.ConfigureMcpTool("Dashboard")
    .AsMcpApp(app =&gt; app
        .WithView("ui/dashboard.html")      // returns IMcpViewBuilder
        .WithTitle("Dashboard")
        .WithPermissions(McpAppPermissions.ClipboardRead)
        .ConfigureApp()                      // back to IMcpAppBuilder
        .WithStaticAssets("ui/dist")
        .WithVisibility(McpVisibility.Model | McpVisibility.App))
    .WithProperty("dataset", McpToolPropertyType.String, "The dataset to display");  // back to tool builder</code></pre>
<h2>Summary</h2>
<p>The new Fluent API for MCP Apps in the .NET isolated worker makes it straightforward to build MCP tools with rich UI experiences. With just a few lines of configuration, you can attach views, control permissions, and enforce security policies on your tools without needing to know the details of the MCP protocol. Under the covers, the extension handles synthetic resource generation, metadata wiring, mime type management, and secure asset serving, so you can focus on building great tool experiences for AI agents and users alike.</p>
<h2>Useful links</h2>
<ul>
<li><a href="https://techcommunity.microsoft.com/blog/appsonazureblog/building-mcp-apps-with-azure-functions-mcp-extension/4496536">Building MCP Apps with Azure Functions MCP Extension</a>: the original MCP Apps announcement blog post</li>
<li><a href="https://learn.microsoft.com/azure/azure-functions/scenario-mcp-apps?tabs=bash%2Clinux&amp;pivots=programming-language-csharp">MCP Apps quickstart</a>: step-by-step guide to building your first MCP App</li>
<li><a href="https://learn.microsoft.com/azure/azure-functions/functions-bindings-mcp">Azure Functions MCP extension documentation</a>: full reference for the MCP extension</li>
</ul>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/mcp-as-easy-as-1-2-3-introducing-the-fluent-api-for-mcp-apps/">MCP as Easy as 1-2-3: Introducing the Fluent API for MCP Apps</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
