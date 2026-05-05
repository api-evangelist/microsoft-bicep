---
title: "Azure SDK Release (April 2026)"
url: "https://devblogs.microsoft.com/azure-sdk/azure-sdk-release-april-2026/"
date: "Tue, 21 Apr 2026 23:13:33 +0000"
author: "Ronnie Geraghty"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>Thank you for your interest in the new Azure SDKs! We release new features, improvements, and bug fixes every month. Subscribe to our <a href="https://devblogs.microsoft.com/azure-sdk/feed/">Azure SDK Blog RSS Feed</a> to get notified when a new release is available.</p>
<p>You can find links to packages, code, and docs on our <a href="https://aka.ms/azsdk/releases">Azure SDK Releases page</a>.</p>
<h2>Release highlights</h2>
<h3><a href="https://central.sonatype.com/artifact/com.azure/azure-cosmos/4.79.0">Cosmos DB 4.79.0</a></h3>
<p>The Java Cosmos DB library includes a critical security fix for a Remote Code Execution (RCE) vulnerability (CWE-502). Java deserialization was replaced with JSON-based serialization in <code>CosmosClientMetadataCachesSnapshot</code>, <code>AsyncCache</code>, and <code>DocumentCollection</code>, eliminating the entire class of Java deserialization attacks. This release also adds support for N-Region synchronous commit, a Query Advisor feature, and <code>CosmosFullTextScoreScope</code> for controlling BM25 statistics scope in hybrid search queries.</p>
<h3><a href="https://www.nuget.org/packages/Azure.AI.Projects/2.0.0">AI Foundry 2.0.0</a></h3>
<p>The <code>Azure.AI.Projects</code> NuGet package ships its 2.0.0 stable release with significant architectural changes. Evaluations and memory operations moved to separate <code>Azure.AI.Projects.Evaluation</code> and <code>Azure.AI.Projects.Memory</code> namespaces. Many types were renamed for consistency, including <code>Insights</code> → <code>ProjectInsights</code>, <code>Schedules</code> → <code>ProjectSchedules</code>, <code>Evaluators</code> → <code>ProjectEvaluators</code>, and <code>Trigger</code> → <code>ScheduleTrigger</code>. Boolean properties now follow the <code>Is*</code> naming convention, and several internal types were made internal.</p>
<h3><a href="https://central.sonatype.com/artifact/com.azure/azure-ai-agents/2.0.0">AI Agents 2.0.0</a></h3>
<p>The Java Azure AI Agents library reaches general availability with version 2.0.0. This release includes breaking changes to improve API consistency:</p>
<ul>
<li>Several enum types were converted from standard Java <code>enum</code> to <code>ExpandableStringEnum</code>-based classes.</li>
<li><code>*Param</code> model classes were renamed to <code>*Parameter</code>.</li>
<li><code>MCPToolConnectorId</code> now uses consistent casing as <code>McpToolConnectorId</code>.</li>
<li>A new convenience overload for <code>beginUpdateMemories</code> was added.</li>
</ul>
<h3>Initial stable releases</h3>
<ul>
<li><strong>Client Libraries for .NET</strong>
<ul>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.Network/1.0.0">Provisioning &#8211; Network 1.0.0</a></li>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.PrivateDns/1.0.0">Provisioning &#8211; Private DNS 1.0.0</a></li>
</ul>
</li>
<li><strong>Management Library for Java</strong>
<ul>
<li><a href="https://central.sonatype.com/artifact/com.azure.resourcemanager/azure-resourcemanager-azurestackhci/1.0.0">Resource Management &#8211; Azure Stack HCI 1.0.0</a></li>
</ul>
</li>
<li><strong>Management Library for Go</strong>
<ul>
<li><a href="https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/sdk/resourcemanager/resources/armpolicy@v1.0.0">Resource Management &#8211; Policy 1.0.0</a></li>
</ul>
</li>
</ul>
<h3>Initial beta releases</h3>
<ul>
<li><strong>Client Libraries for .NET</strong>
<ul>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.ApiManagement/1.0.0-beta.1">Provisioning &#8211; API Management 1.0.0-beta.1</a></li>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.Batch/1.0.0-beta.1">Provisioning &#8211; Batch 1.0.0-beta.1</a></li>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.Compute/1.0.0-beta.1">Provisioning &#8211; Compute 1.0.0-beta.1</a></li>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.Monitor/1.0.0-beta.1">Provisioning &#8211; Monitor 1.0.0-beta.1</a></li>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.MySql/1.0.0-beta.1">Provisioning &#8211; MySQL 1.0.0-beta.1</a></li>
<li><a href="https://www.nuget.org/packages/Azure.Provisioning.SecurityCenter/1.0.0-beta.1">Provisioning &#8211; Security Center 1.0.0-beta.1</a></li>
</ul>
</li>
<li><strong>Management Libraries for .NET</strong>
<ul>
<li><a href="https://www.nuget.org/packages/Azure.ResourceManager.AppNetwork/1.0.0-beta.1">Resource Management &#8211; App Network 1.0.0-beta.1</a></li>
<li><a href="https://www.nuget.org/packages/Azure.ResourceManager.DomainRegistration/1.0.0-beta.1">Resource Management &#8211; Domain Registration 1.0.0-beta.1</a></li>
<li><a href="https://www.nuget.org/packages/Azure.ResourceManager.Resources.Policy/1.0.0-beta.1">Resource Management &#8211; Resources.Policy 1.0.0-beta.1</a></li>
</ul>
</li>
<li><strong>Management Libraries for Java</strong>
<ul>
<li><a href="https://central.sonatype.com/artifact/com.azure.resourcemanager/azure-resourcemanager-containerregistry-tasks/1.0.0-beta.1">Resource Management &#8211; Container Registry Tasks 1.0.0-beta.1</a></li>
<li><a href="https://central.sonatype.com/artifact/com.azure.resourcemanager/azure-resourcemanager-servicegroups/1.0.0-beta.1">Resource Management &#8211; Service Groups 1.0.0-beta.1</a></li>
</ul>
</li>
<li><strong>Client Library for JavaScript</strong>
<ul>
<li><a href="https://www.npmjs.com/package/@azure/ai-speech-transcription/v/1.0.0-beta.1">AI Speech Transcription 1.0.0-beta.1</a></li>
</ul>
</li>
<li><strong>Management Libraries for JavaScript</strong>
<ul>
<li><a href="https://www.npmjs.com/package/@azure/arm-containerregistrytasks/v/1.0.0-beta.1">Resource Management &#8211; Container Registry Tasks 1.0.0-beta.1</a></li>
<li><a href="https://www.npmjs.com/package/@azure/arm-marketplace/v/1.0.0-beta.1">Resource Management &#8211; Marketplace 1.0.0-beta.1</a></li>
</ul>
</li>
<li><strong>Management Libraries for Go</strong>
<ul>
<li><a href="https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/sdk/resourcemanager/alertprocessingrules/armalertprocessingrules@v0.1.0">Resource Management &#8211; Alert Processing Rules 0.1.0</a></li>
<li><a href="https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/sdk/resourcemanager/alertrulerecommendations/armalertrulerecommendations@v0.1.0">Resource Management &#8211; Alert Rule Recommendations 0.1.0</a></li>
<li><a href="https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/sdk/resourcemanager/appnetwork/armappnetwork@v0.1.0">Resource Management &#8211; App Network 0.1.0</a></li>
<li><a href="https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/sdk/resourcemanager/previewalertrule/armpreviewalertrule@v0.1.0">Resource Management &#8211; Preview Alert Rule 0.1.0</a></li>
<li><a href="https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/sdk/resourcemanager/prometheusrulegroups/armprometheusrulegroups@v0.1.0">Resource Management &#8211; Prometheus Rule Groups 0.1.0</a></li>
<li><a href="https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/sdk/resourcemanager/tenantactivitylogalerts/armtenantactivitylogalerts@v0.1.0">Resource Management &#8211; Tenant Activity Log Alerts 0.1.0</a></li>
</ul>
</li>
</ul>
<h2>Release notes</h2>
<ul>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/index.html">All languages</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/dotnet.html">.NET</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/java.html">Java</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/js.html">JavaScript/TypeScript</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/python.html">Python</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/go.html">Go</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/rust.html">Rust</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/cpp.html">C++</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/c.html">Embedded C</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/android.html">Android</a></li>
<li><a href="https://azure.github.io/azure-sdk/releases/2026-04/ios.html">iOS</a></li>
</ul>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/azure-sdk-release-april-2026/">Azure SDK Release (April 2026)</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
