---
title: "Write azd hooks in Python, JavaScript, TypeScript, or .NET"
url: "https://devblogs.microsoft.com/azure-sdk/azd-multi-language-hooks/"
date: "Wed, 22 Apr 2026 22:04:08 +0000"
author: "Kristen Womack"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p><em>Hooks are one of the most popular features in <code>azd</code>, and now you can write them in Python, JavaScript, TypeScript, or .NET, not just Bash and PowerShell.</em></p>
<h2>What&#8217;s new?</h2>
<p>The Azure Developer CLI (<code>azd</code>) hook system now supports four more languages beyond Bash and PowerShell. You can write hook scripts in <strong>Python</strong>, <strong>JavaScript</strong>, <strong>TypeScript</strong>, or <strong>.NET</strong>. <code>azd</code> automatically detects the language from the file extension, manages dependencies, and runs the script with no extra configuration required.</p>
<h2>Why it matters</h2>
<p>Hooks let you run custom logic at key points in the <code>azd</code> lifecycle before provisioning, after deployment, and more. Previously, hooks supported scripts written in Bash and PowerShell, which meant developers had to context-switch to a shell scripting language even if their project was entirely Python or TypeScript. Now you can write hooks in the same language as your application, reuse existing libraries, and skip the shell scripting.</p>
<h2>How to use it</h2>
<p>Point a hook at a script file in <code>azure.yaml</code>, and <code>azd</code> infers the language from the extension. If the extension is ambiguous or missing, you can specify the language explicitly with the <code>kind</code> field:</p>
<pre><code class="language-yaml">hooks:
  preprovision:
    run: ./hooks/setup
    run: ./hooks/setup.py
    kind: python    # explicit — overrides extension inference</code></pre>
<p>Here&#8217;s what a typical setup looks like:</p>
<pre><code class="language-yaml"># azure.yaml
hooks:
  preprovision:
    run: ./hooks/setup.py

  postdeploy:
    run: ./hooks/seed.ts

  postprovision:
    run: ./hooks/migrate.cs</code></pre>
<h3>Python hooks</h3>
<p>Place a <code>requirements.txt</code> or <code>pyproject.toml</code> in the same directory as your script or a parent directory. <code>azd</code> walks up the directory tree from the script location to find the nearest project file, creates a virtual environment, installs dependencies, and runs the script.</p>
<pre><code>hooks/
├── setup.py
└── requirements.txt</code></pre>
<p>Then reference the script in your <code>azure.yaml</code>:</p>
<pre><code class="language-yaml">hooks:
  preprovision:
    run: ./hooks/setup.py</code></pre>
<h3>JavaScript and TypeScript hooks</h3>
<p>Place a <code>package.json</code> in the same directory as your script or a parent directory. <code>azd</code> runs <code>npm install</code> (or the package manager specified in <code>config</code>) and executes the script. TypeScript scripts run via <code>npx tsx</code> with no compile step or <code>tsconfig.json</code> needed:</p>
<pre><code>hooks/
├── seed.ts
└── package.json</code></pre>
<p>Then reference the script in your <code>azure.yaml</code>:</p>
<pre><code class="language-yaml">hooks:
  postdeploy:
    run: ./hooks/seed.ts</code></pre>
<h3>.NET hooks</h3>
<p>Two modes are supported:</p>
<ul>
<li><strong>Project mode</strong>: If a <code>.csproj</code>, <code>.fsproj</code>, or <code>.vbproj</code> exists in the same directory as the script, <code>azd</code> runs <code>dotnet restore</code> and <code>dotnet build</code> automatically.</li>
<li><strong>Single-file mode</strong>: On .NET 10+, standalone <code>.cs</code> files run directly via <code>dotnet run script.cs</code> without a project file.</li>
</ul>
<pre><code>hooks/
├── migrate.cs
└── migrate.csproj   # optional — omit for single-file mode on .NET 10+</code></pre>
<p>Then reference the script in your <code>azure.yaml</code>:</p>
<pre><code class="language-yaml">hooks:
  postprovision:
    run: ./hooks/migrate.cs</code></pre>
<h3>Override the working directory</h3>
<p>Use the <code>dir</code> field to set the working directory for a hook. This configuration is useful when the project root differs from the script location:</p>
<pre><code class="language-yaml">hooks:
  preprovision:
    run: main.py
    dir: hooks/preprovision</code></pre>
<h3>Executor-specific configuration</h3>
<p>Each language supports an optional <code>config</code> block for executor-specific settings:</p>
<pre><code class="language-yaml">hooks:
  preprovision:
    run: ./hooks/setup.ts
    config:
      packageManager: pnpm    # npm | pnpm | yarn

  postdeploy:
    run: ./hooks/seed.py
    config:
      virtualEnvName: .venv   # override default naming

  postprovision:
    run: ./hooks/migrate.cs
    config:
      configuration: Release  # Debug | Release
      framework: net10.0      # target framework</code></pre>
<h3>Mixed formats</h3>
<p>You can mix single-hook and multi-hook formats in the same <code>hooks:</code> block, including platform-specific overrides:</p>
<pre><code class="language-yaml">hooks:
  preprovision:
    run: ./hooks/setup.py
  predeploy:
    windows:
      run: ./hooks/build.ps1
    posix:
      run: ./hooks/build.sh</code></pre>
<h2>Try it out</h2>
<p>To make sure you have this feature, update to the latest <code>azd</code> version:</p>
<pre><code class="language-bash">azd update</code></pre>
<p>For a fresh install, see <a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd">Install azd</a>.</p>
<p>We hope you like this new feature for writing <code>azd</code> hooks in your preferred language! Let us know what you think and how you&#8217;re using it.</p>
<h2>Feedback</h2>
<p>Have questions or ideas? File an issue or start a discussion on <a href="https://github.com/Azure/azure-dev">GitHub</a>. Want to help shape the future of <code>azd</code>? <a href="https://aka.ms/azd-user-research-signup">Sign up for user research</a>.</p>
<h2><img alt="🙋‍♀️" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f64b-200d-2640-fe0f.png" style="height: 1em;" /> New to azd?</h2>
<p>If you&#8217;re new to the Azure Developer CLI, <code>azd</code> is an open-source command-line tool that takes your application from local development environment to Azure. <code>azd</code> provides best practice, developer-friendly commands that map to key stages in your workflow, whether you&#8217;re working in the terminal, your editor or CI/CD.</p>
<ul>
<li><a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd">Install azd</a></li>
<li>Explore templates: Browse the <a href="https://azure.github.io/awesome-azd/">Awesome azd template gallery</a> and <a href="https://aka.ms/ai-gallery">AI App Templates</a></li>
<li>Learn more: Visit the <a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/">official documentation</a> and <a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/troubleshoot">troubleshooting guide</a></li>
<li>Get help: Visit the <a href="https://github.com/Azure/azure-dev">GitHub repository</a> to file issues or start discussions</li>
<li>Share your feedback: We&#8217;re interested in learning how you&#8217;re using <code>azd</code>! <a href="https://aka.ms/azd-user-research-signup">Sign up for user research</a> to help shape the future of the Azure Developer CLI</li>
</ul>
<hr />
<p><em>This feature was introduced across several PRs: <a href="https://github.com/Azure/azure-dev/pull/7451">#7451</a> (Python), <a href="https://github.com/Azure/azure-dev/pull/7626">#7626</a> (JS/TS), <a href="https://github.com/Azure/azure-dev/pull/7652">#7652</a> (.NET/C#/F#/VB.NET), <a href="https://github.com/Azure/azure-dev/pull/7690">#7690</a> (config), <a href="https://github.com/Azure/azure-dev/pull/7618">#7618</a> (mixed formats).</em></p>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/azd-multi-language-hooks/">Write azd hooks in Python, JavaScript, TypeScript, or .NET</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
