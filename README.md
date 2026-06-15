# Microsoft Bicep (microsoft-bicep)

Microsoft Bicep is a domain-specific language (DSL) that uses declarative syntax to deploy Azure resources. It provides a transparent abstraction over ARM templates and offers a more concise syntax, improved type safety, and better support for modularity and code reuse.

**APIs.json:** [https://github.com/Azure/bicep](https://github.com/Azure/bicep)

## Tags

- ARM Templates
- Azure
- Cloud
- Deployment
- DevOps
- Infrastructure as Code

## Timestamps

- **Created:** 2024-01-15
- **Modified:** 2026-05-19

## APIs

### Bicep CLI

Command-line interface for compiling and deploying Bicep files.

- **Human URL:** [https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/)
- **Base URL:** `https://github.com/Azure/bicep`

#### Properties

- [Documentation](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/)
- [GitHub Repository](https://github.com/Azure/bicep)
- [Installation  Guide](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/install)
- [Bicep  Playground](https://bicepdemo.z22.web.core.windows.net/)
- [Postman Collection](collections/microsoft-bicep-deployments.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/microsoft-bicep-deployments.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/microsoft-bicep-template-specs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/microsoft-bicep-template-specs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Bicep Language Server

Language server implementation for Bicep providing IntelliSense and validation.

- **Human URL:** [https://github.com/Azure/bicep/tree/main/src/Bicep.LangServer](https://github.com/Azure/bicep/tree/main/src/Bicep.LangServer)
- **Base URL:** `https://github.com/Azure/bicep`

#### Properties

- [Documentation](https://github.com/Azure/bicep/blob/main/docs/contributing/language-server.md)
- [GitHub Repository](https://github.com/Azure/bicep/tree/main/src/Bicep.LangServer)
- [V S  Code  Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep)
- [Postman Collection](collections/microsoft-bicep-deployments.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/microsoft-bicep-deployments.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/microsoft-bicep-template-specs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/microsoft-bicep-template-specs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Bicep Deployments REST API

Azure Resource Manager Deployments REST API used by Microsoft Bicep to deploy infrastructure as code templates. Provides operations for creating, validating, and managing ARM/Bicep template deployments at resource group, subscription, management group, and tenant scopes.

- **Human URL:** [https://learn.microsoft.com/en-us/rest/api/resources/deployments](https://learn.microsoft.com/en-us/rest/api/resources/deployments)
- **Base URL:** `https://management.azure.com`

#### Properties

- [Documentation](https://learn.microsoft.com/en-us/rest/api/resources/deployments)
- [OpenAPI](openapi/microsoft-bicep-deployments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/microsoft-bicep-deployments.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/microsoft-bicep-deployments.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Authentication](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/authenticate-multi-tenant)

### Bicep Template Specs REST API

Azure Resource Manager Template Specs REST API used by Microsoft Bicep for publishing and managing reusable infrastructure templates. Template Specs allow you to store ARM/Bicep templates as Azure resources for versioning, sharing, and access control across your organization.

- **Human URL:** [https://learn.microsoft.com/en-us/rest/api/resources/template-specs](https://learn.microsoft.com/en-us/rest/api/resources/template-specs)
- **Base URL:** `https://management.azure.com`

#### Properties

- [Documentation](https://learn.microsoft.com/en-us/rest/api/resources/template-specs)
- [OpenAPI](openapi/microsoft-bicep-template-specs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/microsoft-bicep-template-specs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/microsoft-bicep-template-specs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Authentication](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/authenticate-multi-tenant)

## Common Properties

- [GitHub Organization](https://github.com/Azure)
- [Blog](https://devblogs.microsoft.com/azure-sdk/)
- [Support](https://docs.microsoft.com/en-us/answers/topics/azure-bicep.html)
- [Learning  Resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/learn-bicep)
- [Bicep  Examples](https://github.com/Azure/bicep/tree/main/docs/examples)
- [Release Notes](https://github.com/Azure/bicep/releases)
- [Contributing  Guide](https://github.com/Azure/bicep/blob/main/CONTRIBUTING.md)
- [License](https://github.com/Azure/bicep/blob/main/LICENSE)
- [JSON-LD](json-ld/microsoft-bicep-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [JSON Schema](json-schema/microsoft-bicep-deployment-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/microsoft-bicep-template-spec-schema.json) — [JSON Schema](https://json-schema.org/specification)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
