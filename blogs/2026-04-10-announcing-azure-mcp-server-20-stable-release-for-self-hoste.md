---
title: "Announcing Azure MCP Server 2.0 Stable Release for Self-Hosted Agentic Cloud Automation"
url: "https://devblogs.microsoft.com/azure-sdk/announcing-azure-mcp-server-2-0-stable-release/"
date: "Fri, 10 Apr 2026 17:16:10 +0000"
author: "Sandeep Sen"
feed_url: "https://devblogs.microsoft.com/azure-sdk/feed/"
---
<p>We&#8217;re excited to announce the stable release of Azure MCP Server 2.0, a significant milestone for building secure and flexible agentic workflows on Azure. Azure MCP Server is open-source software that implements the <a href="https://modelcontextprotocol.io/docs/getting-started/intro">Model Context Protocol</a> specification and enables AI agents and developer tools to interact with Azure resources through a consistent, standardized tool interface.</p>
<p>Azure MCP currently contains 276 MCP tools across 57 Azure services, enabling end-to-end scenarios that span provisioning, deployment, monitoring, and operational diagnostics within AI-assisted experiences. The defining advancement in 2.0 is the self-hosted, remote MCP server support. Azure MCP server can now run as a remote MCP server, so you can deploy it exactly where your team builds and operates.</p>
<h2>What is Azure MCP Server?</h2>
<p>Azure MCP Server is an MCP-compliant server that exposes Azure capabilities as structured, discoverable tools that agents can select and invoke. It&#8217;s designed to integrate into modern developer workflows and can be used flexibly across local development on IDEs, tool integrations, and centralized deployments, including operation as a <a href="https://aka.ms/azmcp/self-host/obo">self-hosted</a> remote MCP server for team and enterprise scenarios, which is the primary focus of the 2.0 release.</p>
<p>This flexibility lets you start small on a single machine and scale to centrally managed deployments with consistent policy, security controls, and configuration.</p>
<h2>Key updates in Azure MCP Server 2.0</h2>
<p>Azure MCP Server 2.0 represents a focused set of improvements that make the platform more suitable for shared deployments, stronger governance, and daily engineering workflows.</p>
<h3>Self-hosted remote MCP server</h3>
<p>Azure MCP Server 2.0 is designed for remote hosting. It strengthens HTTP-based transport to support authentication scenarios and safer operational behaviors in remote mode. This enables teams to deploy Azure MCP as a centrally managed internal service and apply consistent configuration and governance.</p>
<p>Remote Azure MCP also supports multiple authentication approaches so you can align access with your environment and security model. For example, you can use <a href="https://aka.ms/azmcp/self-host/foundry">managed identity when running alongside Microsoft Foundry</a>. Alternatively, use an <a href="https://aka.ms/azmcp/self-host/obo">On-Behalf-Of (OBO) flow</a>, also known as OpenID Connect delegation, to securely call Azure APIs using the signed-in user context.</p>
<p>Common scenarios include:</p>
<ul>
<li>Providing shared Azure MCP access to developers and internal agent systems</li>
<li>Operating Azure MCP within enterprise network and policy boundaries</li>
<li>Centrally managing configuration such as tenant context, subscription defaults, and telemetry policies</li>
<li>Integrating MCP-powered workflows into CI/CD and automation pipelines</li>
</ul>
<h3>Security hardening and operational safeguards</h3>
<p>Security and operational safety are central design priorities in 2.0. The release includes stronger validation and safeguards intended to reduce risk in both local development and remote hosting scenarios. These improvements span endpoint validation, protections against common injection patterns for query-oriented tools, and tighter isolation controls for development environments.</p>
<p>Collectively, these changes are intended to make Azure MCP safer to run locally and more appropriate to host as a remote shared service.</p>
<h3>Client compatibility and distribution options</h3>
<p>Azure MCP Server 2.0 continues to support a broad range of development environments and agent platforms, whether you&#8217;re working inside an IDE, interacting through a CLI, or running a standalone server. The release also expands distribution options to improve portability and simplify onboarding across MCP-compatible tools.</p>
<h3>Performance and reliability improvements</h3>
<p>Azure MCP Server 2.0 includes practical upgrades that improve reliability and responsiveness in day-to-day usage, particularly in scenarios that depend on multiple MCP toolsets. Container distribution updates also reduce image size and support more efficient deployment in containerized environments.</p>
<h3>Sovereign cloud readiness</h3>
<p>Azure MCP Server can be configured for <a href="https://aka.ms/azmcp/sovereign-cloud">sovereign cloud environments</a> such as Azure US Government and Azure operated by 21Vianet Cloud (Azure in China), enabling use in regulated deployments that require sovereign endpoints and stronger boundary controls. This capability complements the 2.0 emphasis on self-hosting by allowing teams to deploy Azure MCP close to their required cloud environment and compliance posture.</p>
<h2>Under the hood</h2>
<p>Azure MCP continues to evolve its tool ecosystem to improve usability and agent selection accuracy through clearer tool descriptions, more consistent validation logic, and consolidation of redundant operations where it improves discoverability. The intent is to provide a practical code-to-cloud operational interface that works consistently across a wide range of Azure scenarios without requiring service-specific integration patterns.</p>
<h2>Get started and choose your experience</h2>
<ul>
<li><a href="https://aka.ms/azmcp">GitHub Repo</a></li>
<li><a href="https://aka.ms/azmcp/download/docker">Docker Image</a></li>
<li><a href="https://aka.ms/azmcp/issues">Create an Issue</a></li>
</ul>
<h3>Choose your experience</h3>
<ul>
<li>Use Azure MCP as an IDE extension in the following tools for an integrated developer workflow:
<ul>
<li><a href="https://aka.ms/azmcp/download/vscode">Visual Studio Code</a></li>
<li><a href="https://aka.ms/azmcp/download/vs">Visual Studio</a></li>
<li><a href="https://aka.ms/azmcp/download/intellij">IntelliJ</a></li>
<li><a href="https://aka.ms/azmcp/download/eclipse">Eclipse</a></li>
<li><a href="https://aka.ms/azmcp/download/cursor">Cursor</a></li>
</ul>
</li>
<li>Use it with agent tools such as <a href="https://aka.ms/azmcp/download/github-copilot-cli">GitHub Copilot CLI</a> and Claude Code for command-line agentic scenarios.</li>
<li>Run it as a standalone local server when you want a simple, self-contained setup.</li>
<li>Self-host it as a <a href="https://aka.ms/azmcp/self-host">remote MCP server</a> when you need shared access, centralized configuration, and enterprise-ready controls, which is the key capability introduced in 2.0.</li>
</ul>
<h2>Thank you!</h2>
<p>Azure MCP Server 2.0 reflects continued collaboration with partners and the broader developer community. Thank you for the feedback, contributions, and real-world scenarios that shaped this release. We&#8217;re looking forward to what you build with 2.0, especially as more teams adopt self-hosted MCP servers to bring agentic workflows closer to their systems, policies, and day-to-day engineering practices.</p>
<p>The post <a href="https://devblogs.microsoft.com/azure-sdk/announcing-azure-mcp-server-2-0-stable-release/">Announcing Azure MCP Server 2.0 Stable Release for Self-Hosted Agentic Cloud Automation</a> appeared first on <a href="https://devblogs.microsoft.com/azure-sdk">Azure SDK Blog</a>.</p>
