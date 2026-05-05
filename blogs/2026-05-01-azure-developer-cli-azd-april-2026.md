---
title: "Azure Developer CLI (azd) – April 2026"
url: "https://devblogs.microsoft.com/azure-sdk/azure-developer-cli-azd-april-2026/"
date: "Fri, 01 May 2026 00:53:08 +0000"
author: "Kristen Womack"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>The Azure Developer CLI (<code>azd</code>) shipped five releases in April 2026. The biggest theme this month is multi-language hook support: write <code>azd</code> hooks in Python, JavaScript, TypeScript, or .NET alongside the existing Bash and PowerShell options. Here&#8217;s what&#8217;s in versions <a href="https://github.com/Azure/azure-dev/releases/tag/azure-dev-cli_1.23.14">1.23.14</a>, <a href="https://github.com/Azure/azure-dev/releases/tag/azure-dev-cli_1.23.15">1.23.15</a>, <a href="https://github.com/Azure/azure-dev/releases/tag/azure-dev-cli_1.24.0">1.24.0</a>, <a href="https://github.com/Azure/azure-dev/releases/tag/azure-dev-cli_1.24.1">1.24.1</a>, and <a href="https://github.com/Azure/azure-dev/releases/tag/azure-dev-cli_1.24.2">1.24.2</a>.</p>
<p>To share your feedback and questions, join the <a href="https://github.com/Azure/azure-dev/discussions/7934">April release discussion on GitHub</a>.</p>
<p><strong>Highlights:</strong></p>
<ul>
<li>Security improvements including Windows MSI code-signing verification and an environment variable leak fix in extension commands (update azd version)</li>
<li>Write hooks in Python, JavaScript, TypeScript, and .NET with automatic dependency management</li>
<li>AI model quota preflight check catches quota issues before provisioning</li>
<li><code>azd update</code> graduates to public preview, so you can update with a single command on any platform</li>
<li>Custom provisioning providers let extension authors plug in alternative infrastructure backends</li>
<li>Standardized <code>--no-prompt</code> behavior for reliable automation in CI/CD pipelines and AI agents</li>
</ul>
<h2>New features</h2>
<h3><img alt="🪝" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1fa9d.png" style="height: 1em;" /> Multi-language hooks</h3>
<p>Hooks in <code>azure.yaml</code> now support Python, JavaScript, TypeScript, and .NET scripts, alongside the existing Bash and PowerShell options. Each language gets automatic dependency installation and runtime management. Read more in <a href="https://devblogs.microsoft.com/azure-sdk/azd-multi-language-hooks/">Write azd hooks in Python, JavaScript, TypeScript, or .NET</a>.</p>
<ul>
<li><strong>Python hooks</strong>: Point a hook to a <code>.py</code> script and <code>azd</code> detects it automatically. When <code>requirements.txt</code> or <code>pyproject.toml</code> is present, a virtual environment is created and dependencies are installed before execution. <a href="https://github.com/Azure/azure-dev/pull/7451">[#7451]</a></li>
<li><strong>JavaScript and TypeScript hooks</strong>: Point a hook to a <code>.js</code> or <code>.ts</code> script for automatic detection. When <code>package.json</code> is present, <code>azd</code> runs <code>npm install</code> first. TypeScript scripts execute through <code>npx tsx</code> with no compile step required. <a href="https://github.com/Azure/azure-dev/pull/7626">[#7626]</a></li>
<li><strong>.NET hooks</strong>: Point a hook to a <code>.cs</code> file and <code>azd</code> executes it with <code>dotnet run</code>, with automatic project discovery and support for single-file scripts on .NET 10+. <a href="https://github.com/Azure/azure-dev/pull/7652">[#7652]</a></li>
<li><strong>Executor-specific configuration</strong>: A new <code>config:</code> block for hooks in <code>azure.yaml</code> lets you configure <code>packageManager</code> for JavaScript/TypeScript hooks, <code>virtualEnvName</code> for Python hooks, and <code>configuration</code>/<code>framework</code> for .NET hooks. <a href="https://github.com/Azure/azure-dev/pull/7690">[#7690]</a></li>
<li><strong>Per-layer hooks</strong>: Hooks defined under <code>infra.layers[].hooks</code> now execute during <code>azd provision</code>, and <code>azd hooks run</code> supports a new <code>--layer</code> flag for targeted execution. <a href="https://github.com/Azure/azure-dev/pull/7382">[#7382]</a></li>
</ul>
<h3><img alt="🔌" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f50c.png" style="height: 1em;" /> Extension Framework</h3>
<p>The Extension Framework (currently in <strong>Beta</strong>, while <code>azd</code> itself is GA) gains new capabilities for extension authors and a smoother upgrade experience for users.</p>
<ul>
<li><strong>Custom provisioning providers</strong>: Extension authors can register alternative infrastructure providers through <code>WithProvisioningProvider("name", factory)</code> on the <code>ExtensionHost</code>. Users set <code>infra: { provider: name }</code> in <code>azure.yaml</code> to use them. <a href="https://github.com/Azure/azure-dev/pull/7482">[#7482]</a></li>
<li><strong>Smarter extension upgrades</strong>: <code>azd extension upgrade</code> now resolves the installed source by default, automatically promotes extensions from a dev registry to the main registry when a newer version is available, and processes <code>--all</code> or <code>--no-prompt</code> batch upgrades non-interactively. <a href="https://github.com/Azure/azure-dev/pull/7841">[#7841]</a></li>
<li><strong>Key Vault secret resolver</strong>: The extension framework automatically resolves <code>@Microsoft.KeyVault(...)</code> references in environment variables before passing them to extensions. <a href="https://github.com/Azure/azure-dev/pull/7043">[#7043]</a></li>
</ul>
<h3><img alt="🤖" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f916.png" style="height: 1em;" /> AI and Copilot</h3>
<ul>
<li><strong>AI model quota preflight check</strong>: <code>azd provision</code> now detects Azure Cognitive Services model deployments in the Bicep snapshot and validates quota availability before provisioning, warning on exceeded quota or unrecognized model names. <a href="https://github.com/Azure/azure-dev/pull/7672">[#7672]</a></li>
<li><strong>&#8220;Fix this error&#8221; in Copilot troubleshooting</strong>: The Copilot-assisted error troubleshooting flow adds a &#8220;Fix this error&#8221; option, allowing the agent to directly apply a fix and collect user feedback. Read more in <a href="https://devblogs.microsoft.com/azure-sdk/azd-copilot-integration/">GitHub Copilot meets Azure Developer CLI: AI-assisted project setup and error troubleshooting</a>. <a href="https://github.com/Azure/azure-dev/pull/7401">[#7401]</a></li>
<li><strong>Improved AI model capacity resolution</strong>: The <code>PromptLocation</code> extension framework API adds an <code>allowed_locations</code> filter, and AI model capacity resolution falls back to the highest valid capacity within remaining quota. <a href="https://github.com/Azure/azure-dev/pull/7397">[#7397]</a></li>
</ul>
<h3><img alt="🚀" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f680.png" style="height: 1em;" /> Project setup and initialization</h3>
<ul>
<li><strong><code>.azdignore</code> for templates</strong>: Template authors can create a <code>.azdignore</code> file in the template root to exclude contributor-only files (such as <code>SECURITY.md</code>, <code>.github/</code>) from being copied to consumer projects. <a href="https://github.com/Azure/azure-dev/pull/7685">[#7685]</a></li>
<li><strong><code>.azdxignore</code> for watch mode</strong>: Create a <code>.azdxignore</code> file in the project root (gitignore syntax) to exclude directories such as <code>node_modules/</code> and <code>dist/</code> from triggering unnecessary rebuilds during <code>azd x watch</code>. <a href="https://github.com/Azure/azure-dev/pull/7697">[#7697]</a></li>
</ul>
<h3><img alt="⚙" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2699.png" style="height: 1em;" /> Preflight validation</h3>
<ul>
<li><strong>Reserved-name preflight check</strong>: <code>azd provision</code> now warns before deploying when predicted resource names contain Azure reserved words (such as names with <code>MICROSOFT</code>, <code>WINDOWS</code>, or prefixed with <code>LOGIN</code>), with color-highlighted output. The check correctly skips Azure Resource Manager (ARM) allow-listed resource types (such as Private Link DNS zones, resource groups, and role assignments) that accept reserved names server-side. <a href="https://github.com/Azure/azure-dev/pull/7746">[#7746]</a> <a href="https://github.com/Azure/azure-dev/pull/7819">[#7819]</a></li>
</ul>
<h3><img alt="🔧" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f527.png" style="height: 1em;" /> Developer experience</h3>
<ul>
<li><strong><code>azd update</code> public preview</strong>: <code>azd update</code> no longer requires an alpha feature flag and displays a preview notice on first use. Read more in <a href="https://devblogs.microsoft.com/azure-sdk/azd-update/">Stop juggling package managers, run <code>azd update</code></a>. <a href="https://github.com/Azure/azure-dev/pull/7489">[#7489]</a></li>
<li><strong><code>--non-interactive</code> flag alias</strong>: <code>--non-interactive</code> is now a global alias for <code>--no-prompt</code>, and the <code>AZD_NON_INTERACTIVE</code> environment variable enables non-interactive mode. <a href="https://github.com/Azure/azure-dev/pull/7392">[#7392]</a></li>
<li><strong>Standardized <code>--no-prompt</code> failures</strong>: <code>--no-prompt</code> now consistently fails with a structured error when required input (subscription, location, or resource group) can&#8217;t be resolved automatically, enabling reliable non-interactive use in continuous integration and continuous deployment (CI/CD) pipelines and AI agents. <a href="https://github.com/Azure/azure-dev/pull/7825">[#7825]</a></li>
<li><strong><code>docker.network</code> option</strong>: A new <code>docker.network</code> field in <code>azure.yaml</code> service configuration passes <code>--network</code> to <code>docker build</code> for services that require host networking (such as builds behind corporate proxies). <a href="https://github.com/Azure/azure-dev/pull/7361">[#7361]</a></li>
<li><strong>Raw <code>azd auth token</code> output</strong>: <code>azd auth token</code> now prints the raw access token by default. Use <code>--output json</code> for structured output including expiration metadata. <a href="https://github.com/Azure/azure-dev/pull/7384">[#7384]</a></li>
<li><strong>Reduced pipeline prompts</strong>: <code>azd pipeline config</code> no longer prompts for parameters that are outputs of earlier provisioning layers, reducing unnecessary prompts in multi-layer deployments. <a href="https://github.com/Azure/azure-dev/pull/7296">[#7296]</a></li>
</ul>
<h2>Breaking changes</h2>
<h3><code>azd init -t &lt;template&gt;</code> creates a new directory</h3>
<p>Running <code>azd init -t &lt;template&gt;</code> now creates a project directory named after the template (similar to <code>git clone</code>) instead of initializing in the current directory. Pass an optional <code>[directory]</code> positional argument to override the name, or pass <code>.</code> to restore the previous in-place behavior:</p>
<pre><code class="language-bash">azd init -t &lt;template&gt; .</code></pre>
<p><a href="https://github.com/Azure/azure-dev/pull/7290">[#7290]</a></p>
<h3>App Service slot targeting</h3>
<p>The automatic slot-selection heuristics for App Service deployments are replaced with explicit slot targeting. Set <code>AZD_DEPLOY_{SERVICE}_SLOT_NAME=production</code> to deploy to the main app, or <code>AZD_DEPLOY_{SERVICE}_SLOT_NAME=&lt;name&gt;</code> for a specific slot.</p>
<p>The previous auto pick behavior (single slot present, no <code>SLOT_NAME</code> set, <code>--no-prompt</code>) and first-deploy push-to-all-slots behavior are removed. When slots are present and <code>SLOT_NAME</code> isn&#8217;t set, <code>azd deploy</code> prompts interactively or errors in non-interactive mode. <a href="https://github.com/Azure/azure-dev/pull/7630">[#7630]</a></p>
<h2><img alt="🪲" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1fab2.png" style="height: 1em;" /> Bugs fixed</h2>
<h3>Security</h3>
<p>This release includes several security-related fixes. Users are encouraged to upgrade (<code>azd update</code>).</p>
<ul>
<li><strong>Code-signing verification for Windows MSI installs</strong> performed via <code>azd update</code>. The installer now refactors how MSI install script arguments are constructed for stable and daily channels, and rejects MSI packages that aren&#8217;t signed by the expected publisher. This closes a path where a tampered or substituted MSI could be silently installed during an in-place update on Windows. <a href="https://github.com/Azure/azure-dev/pull/7298">[#7298]</a></li>
<li><strong>Environment variable leak fix between <code>azd</code> and extension subprocesses.</strong> Because extension commands set <code>DisableFlagParsing: true</code>, cobra never parsed the <code>-e/--environment</code> flag, so the DI resolver fell back to the default environment and extensions could receive secrets or configuration from the wrong <code>azd</code> environment. The fix introduces a <code>globalOptions</code> parser that runs before cobra and is used consistently by both the DI resolver and extension invocation. The same change restores broken <code>--debug</code>, <code>--cwd</code>, and <code>-e/--environment</code> flag propagation to extension commands that regressed in an earlier revert. Validation of environment names is intentionally lenient so that extensions reusing <code>-e</code> for non-env-name values (such as URLs) continue to work. <a href="https://github.com/Azure/azure-dev/pull/7314">[#7314]</a></li>
<li><strong>Removed an unsafe global <code>os.Chdir</code> call</strong> from Aspire server initialization. The previous implementation mutated the process-wide working directory in <code>InitializeAsync</code>, an ambient-authority pattern that could cause concurrency issues when other goroutines or concurrent operations relied on the working directory. <code>rootPath</code> was already passed explicitly to every consumer, so the call was unnecessary as well as unsafe. <a href="https://github.com/Azure/azure-dev/pull/7362">[#7362]</a></li>
</ul>
<h3>Authentication and identity</h3>
<ul>
<li>Fix error handling for <code>AADSTS530084</code> token protection errors to display clear guidance and documentation links instead of an opaque failure message. <a href="https://github.com/Azure/azure-dev/pull/7797">[#7797]</a></li>
<li>Fix tenant-specific authentication guidance for <code>AADSTS70043</code> and <code>AADSTS700082</code> errors; <code>azd</code> now returns guidance targeting the correct subscription tenant when a credential fails due to a stale refresh token. <a href="https://github.com/Azure/azure-dev/pull/7578">[#7578]</a></li>
<li>Fix <code>AZURE_PRINCIPAL_ID</code> resolution for guest and business-to-business users by resolving the principal identity in the subscription&#8217;s resource tenant, and prefer the ARM token <code>oid</code> claim over a Microsoft Graph call to avoid incorrect role assignments. <a href="https://github.com/Azure/azure-dev/pull/7549">[#7549]</a></li>
<li>Fix panic when <code>azd auth token</code> is called with an unsupported <code>--output</code> format. <a href="https://github.com/Azure/azure-dev/pull/7356">[#7356]</a></li>
<li>Fix <code>azd auth token</code> being cancelled by the background update check when invoked as a subprocess by extension credential providers. Fast-exit commands now skip the update check entirely. <a href="https://github.com/Azure/azure-dev/pull/7629">[#7629]</a></li>
</ul>
<h3>Hooks and configuration</h3>
<ul>
<li>Fix <code>azure.yaml</code> hook parsing failure when mixing single-hook (map) and multi-hook (list) formats in the same <code>hooks:</code> block. <a href="https://github.com/Azure/azure-dev/pull/7618">[#7618]</a></li>
<li>Fix service-level hooks referencing shared scripts through relative paths (such as <code>../../hooks/script.ps1</code>) failing with &#8220;hook script path escapes project root.&#8221; The containment boundary is now the project root instead of the service directory (regression in 1.23.15). <a href="https://github.com/Azure/azure-dev/pull/7689">[#7689]</a></li>
<li>Fix service names containing spaces in <code>azure.yaml</code> generating invalid environment variable names (such as <code>SERVICE_API AND FRONTEND_IMAGE_NAME</code> → <code>SERVICE_API_AND_FRONTEND_IMAGE_NAME</code>). <a href="https://github.com/Azure/azure-dev/pull/7723">[#7723]</a></li>
<li>Fix <code>docker.path</code> and <code>docker.context</code> in <code>azure.yaml</code> being resolved relative to the service directory instead of the project root when user-specified values are provided. <a href="https://github.com/Azure/azure-dev/pull/7600">[#7600]</a></li>
<li>Fix nil pointer panic when <code>azure.yaml</code> contains services, resources, or hooks with empty definitions. Reports all issues in a single actionable error message. <a href="https://github.com/Azure/azure-dev/pull/7343">[#7343]</a></li>
</ul>
<h3>Extensions and deployment</h3>
<ul>
<li>Fix extension lifecycle event handlers being silently dropped when multiple extensions subscribe to the same lifecycle event. <a href="https://github.com/Azure/azure-dev/pull/7562">[#7562]</a></li>
<li>Fix subscription-scope deployments incorrectly treating preexisting resource groups as deployment-owned during cleanup. Only resource groups explicitly created by the deployment are now returned. <a href="https://github.com/Azure/azure-dev/pull/7698">[#7698]</a></li>
<li>Fix Azure Kubernetes Service postprovision hook to skip gracefully when the cluster isn&#8217;t provisioned yet in a multi-phase workflow, instead of failing fatally. <a href="https://github.com/Azure/azure-dev/pull/7501">[#7501]</a></li>
<li>Fix <code>azd pipeline config --provider azdo</code> failing when no agent queue named &#8220;Default&#8221; exists. <code>azd</code> now queries available queues, automatically selects when only one is present, and prompts the user to choose when multiple queues are available. <a href="https://github.com/Azure/azure-dev/pull/7707">[#7707]</a></li>
</ul>
<h3>Copilot and AI</h3>
<ul>
<li>Fix Copilot error-handling saved preference (<code>copilot.errorHandling.fix=allow</code>) to automatically retry the failed command after applying a fix, instead of only applying the fix without retrying. <a href="https://github.com/Azure/azure-dev/pull/7768">[#7768]</a></li>
<li>Fix Copilot error troubleshooting to skip AI analysis for timeout errors, mark Bicep missing-input and <code>azure.yaml</code> config validation errors as not fixable, and apply a 5-minute guard timeout to AI analysis requests. <a href="https://github.com/Azure/azure-dev/pull/7555">[#7555]</a></li>
</ul>
<h3>User experience</h3>
<ul>
<li>Fix arrow keys printing escape sequence characters (<code>[A</code>, <code>[B</code>, <code>[C</code>, <code>[D</code>) in the filter text of select and multi-select prompts when running <code>azd</code> in PowerShell. <a href="https://github.com/Azure/azure-dev/pull/7642">[#7642]</a></li>
<li>Fix <code>azd update</code> on Windows failing when PowerShell 7 and 5.1 are both installed. Reset <code>PSModulePath</code> before invoking the installer to prevent module path conflicts. <a href="https://github.com/Azure/azure-dev/pull/7703">[#7703]</a></li>
<li>Improve <code>azd update</code> error message when the installation is managed by an administrator, with guidance to suppress update notifications through <code>AZD_SKIP_UPDATE_CHECK=1</code>. <a href="https://github.com/Azure/azure-dev/pull/7417">[#7417]</a></li>
</ul>
<h2>Other changes</h2>
<ul>
<li>Improve <code>azd up</code> and <code>azd deploy</code> performance with connection pooling, adaptive ARM poll frequency (5 seconds for deployments, 15 seconds for WhatIf/Validate), per-registry Azure Container Registry login caching, and Container App revision poll frequency (5 seconds). <a href="https://github.com/Azure/azure-dev/pull/7600">[#7600]</a></li>
<li>Update bundled Bicep CLI to v0.42.1. <a href="https://github.com/Azure/azure-dev/pull/7557">[#7557]</a></li>
<li>Update bundled GitHub CLI to v2.89.0. <a href="https://github.com/Azure/azure-dev/pull/7456">[#7456]</a></li>
<li>Update the &#8220;update available&#8221; banner to a shorter, more actionable format that includes a link to release notes (stable channel) or recent changes (daily channel). <a href="https://github.com/Azure/azure-dev/pull/7767">[#7767]</a></li>
<li>Update <code>azd update</code> success message to a shorter, more actionable format. <a href="https://github.com/Azure/azure-dev/pull/7591">[#7591]</a></li>
<li>Filter deprecated AI model versions and retired stock-keeping units from model selection prompts in the AI model service. <a href="https://github.com/Azure/azure-dev/pull/7536">[#7536]</a></li>
<li>Fix <code>copilot consent list</code> and <code>copilot consent revoke</code> <code>--action</code> flag to display correct valid values (<code>all</code>, <code>readonly</code>) in shell completion suggestions. <a href="https://github.com/Azure/azure-dev/pull/7588">[#7588]</a></li>
</ul>
<h2>New docs</h2>
<ul>
<li><strong><a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/hooks-multi-language">Multi-language hooks</a></strong> (April 24): New documentation covering Python, JavaScript, TypeScript, and .NET hook support in <code>azure.yaml</code>, including dependency management and executor configuration.</li>
<li><strong><a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/azd-copilot-integration">Copilot integration</a></strong> (April 24): New documentation for GitHub Copilot integration with <code>azd</code>, covering AI-assisted project setup and error troubleshooting flows.</li>
<li><strong><a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/custom-prompts">Custom prompts</a></strong> (April 14): New documentation on customizing prompts in <code>azd</code> workflows.</li>
<li><strong><a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/azd-schema">Azure.yaml schema</a></strong> (April 23): Updated schema documentation covering new fields for hooks, Docker configuration, and infrastructure providers.</li>
<li><strong><a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd">Updated installation instructions</a></strong> (April 14): Refreshed installation instructions for v1.23.14.</li>
</ul>
<h2>New templates</h2>
<p>Community-driven templates help you get started faster, solve real-world scenarios, and showcase best practices for deploying solutions with Azure Developer CLI.</p>
<p>The Azure Developer CLI template gallery continues to grow with exciting new contributions from the community. Thank you!</p>
<h3>awesome-azd</h3>
<ul>
<li><strong><a href="https://github.com/Azure-Samples/functions-quickstart-python-azd-service-bus">Azure Functions with Service Bus and VNet (Python)</a></strong> by <a href="https://github.com/anthonychu">Anthony Chu</a>: End-to-end Python sample demonstrating secure triggering of a Flex Consumption function app from a Service Bus instance secured in a virtual network, using managed identity and VNet integration.</li>
<li><strong>Azure Functions EventHub templates</strong> (Java, JavaScript, PowerShell): Multiple EventHub-triggered Azure Functions templates added April 10.</li>
<li><strong><a href="https://github.com/massimobonanni/AZD-ToDoList">ToDo list: Razor Pages + Azure Functions + Storage Table (Managed Identity access)</a></strong> by <a href="https://github.com/massimobonanni">Massimo Bonanni</a>: A complete ToDo app with front-end in Razor pages hosted in App Service, backend in Azure Functions hosted in a function app, and data in Azure Table Storage. Uses managed identity to access data.</li>
<li><strong>Host Model Context Protocol SDK Server on Azure Functions</strong>, with four language variants added April 3:
<ul>
<li><strong><a href="https://github.com/Azure-Samples/mcp-sdk-functions-hosting-python">Python</a></strong></li>
<li><strong><a href="https://github.com/Azure-Samples/mcp-sdk-functions-hosting-node">TypeScript</a></strong></li>
<li><strong><a href="https://github.com/Azure-Samples/mcp-sdk-functions-hosting-dotnet">.NET/C#</a></strong></li>
<li><strong><a href="https://github.com/Azure-Samples/mcp-sdk-functions-hosting-java">Java/Quarkus</a></strong></li>
</ul>
</li>
</ul>
<h3>AI gallery</h3>
<ul>
<li><strong><a href="https://github.com/microsoft/Prior-Authorization-Multi-Agent-Solution-Accelerator">Prior-Authorization-Multi-Agent-Solution-Accelerator</a></strong>, added April 8.</li>
</ul>
<h2><img alt="🙋‍♀️" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f64b-200d-2640-fe0f.png" style="height: 1em;" /> New to azd?</h2>
<p>If you&#8217;re new to the Azure Developer CLI, <code>azd</code> is an open-source command-line tool that accelerates the time it takes to get your application from local development environment to Azure. <code>azd</code> provides best practice, developer-friendly commands that map to key stages in your workflow, whether you&#8217;re working in the terminal, your editor or CI/CD.</p>
<ul>
<li><a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd">Install azd</a></li>
<li><strong>Explore templates:</strong> Browse the <a href="https://azure.github.io/awesome-azd/">Awesome azd template gallery</a> and <a href="https://aka.ms/ai-gallery">AI App Templates</a></li>
<li><strong>Learn more:</strong> Visit the <a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/">official documentation</a> and <a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/troubleshoot">troubleshooting guide</a></li>
<li><strong>Get help:</strong> Visit the <a href="https://github.com/Azure/azure-dev">GitHub repository</a> to file issues or start discussions</li>
</ul>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/azure-developer-cli-azd-april-2026/">Azure Developer CLI (azd) &#8211; April 2026</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
