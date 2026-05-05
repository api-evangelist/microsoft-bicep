---
title: "Stop juggling package managers—just run `azd update`"
url: "https://devblogs.microsoft.com/azure-sdk/azd-update/"
date: "Tue, 14 Apr 2026 19:24:08 +0000"
author: "Kristen Womack"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p><em>Updating <code>azd</code> used to mean remembering which package manager you installed it with. Now one command handles it on every platform.</em></p>
<h2>What&#8217;s new?</h2>
<p>The <code>azd update</code> command updates the Azure Developer CLI (azd) to the latest version. It works on Windows, macOS, and Linux regardless of how you originally installed <code>azd</code>—winget, Chocolatey, Homebrew, or install script. No more juggling platform-specific upgrade commands.</p>
<h2>Why it matters</h2>
<p>Every time the &#8220;A new version of azd is available&#8221; prompt appeared, it was easy to think &#8220;I&#8217;ll do that later&#8221; and forget. Then you&#8217;d hit a bug that was already fixed. When you can&#8217;t remember whether you used winget, Homebrew, or a curl script, even a simple upgrade becomes a detour. <code>azd update</code> removes that friction so you stay current with one command.</p>
<h2>How to use it</h2>
<p>To update to the latest stable release, run:</p>
<pre><code class="language-bash">azd update</code></pre>
<p>To switch to the daily insiders build for early access to new features, use the <code>--channel</code> flag:</p>
<pre><code class="language-bash">azd update --channel daily
azd update --channel stable</code></pre>
<h2>Try it out</h2>
<p>The <code>azd update</code> command is available starting with azd 1.23.x. To check your current version, run <code>azd version</code>. If you&#8217;re on an older version, do one last manual update using your original installation method. After that, <code>azd update</code> handles everything going forward. For a fresh install, see <a href="https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd">Install azd</a>.</p>
<p>For a deeper dive, check out this post from Jon Gallant <a href="https://blog.jongallant.com/2026/04/azd-update">azd update—Stop Juggling Package Managers</a>.</p>
<h2>Feedback</h2>
<p>Have questions or ideas? File an issue or start a discussion on <a href="https://github.com/Azure/azure-dev">GitHub</a>. Want to help shape the future of <code>azd</code>? <a href="https://aka.ms/azd-user-research-signup">Sign up for user research</a>.</p>
<hr />
<p><em>This feature was introduced in <a href="https://github.com/Azure/azure-dev/pull/6942">PR #6942</a>, based on <a href="https://github.com/Azure/azure-dev/issues/6673">Issue #6673</a>.</em></p>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/azd-update/">Stop juggling package managers—just run `azd update`</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
