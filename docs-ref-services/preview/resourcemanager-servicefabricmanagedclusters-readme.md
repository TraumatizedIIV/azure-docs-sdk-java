---
title: Azure Resource Manager Service Fabric Managed Clusters client library for Java
keywords: Azure, java, SDK, API, azure-resourcemanager-servicefabricmanagedclusters, servicefabricmanagedclusters
ms.date: 06/26/2025
ms.topic: reference
ms.devlang: java
ms.service: servicefabricmanagedclusters
---
# Azure Resource Manager Service Fabric Managed Clusters client library for Java - version 1.1.0-beta.1 


Azure Resource Manager Service Fabric Managed Clusters client library for Java.

This package contains Microsoft Azure SDK for Service Fabric Managed Clusters Management SDK. Service Fabric Managed Clusters Management Client. Package api-version 2025-03-01-preview. For documentation on how to use this package, please see [Azure Management Libraries for Java](https://aka.ms/azsdk/java/mgmt).

## We'd love to hear your feedback

We're always working on improving our products and the way we communicate with our users. So we'd love to learn what's working and how we can do better.

If you haven't already, please take a few minutes to [complete this short survey][survey] we have put together.

Thank you in advance for your collaboration. We really appreciate your time!

## Documentation

Various documentation is available to help you get started

- [API reference documentation][docs]

## Getting started

### Prerequisites

- [Java Development Kit (JDK)][jdk] with version 8 or above
- [Azure Subscription][azure_subscription]

### Adding the package to your product

[//]: # ({x-version-update-start;com.azure.resourcemanager:azure-resourcemanager-servicefabricmanagedclusters;current})
```xml
<dependency>
    <groupId>com.azure.resourcemanager</groupId>
    <artifactId>azure-resourcemanager-servicefabricmanagedclusters</artifactId>
    <version>1.1.0-beta.1</version>
</dependency>
```
[//]: # ({x-version-update-end})

### Include the recommended packages

Azure Management Libraries require a `TokenCredential` implementation for authentication and an `HttpClient` implementation for HTTP client.

[Azure Identity][azure_identity] and [Azure Core Netty HTTP][azure_core_http_netty] packages provide the default implementation.

### Authentication

Microsoft Entra ID token authentication relies on the [credential class][azure_identity_credentials] from [Azure Identity][azure_identity] package.

Azure subscription ID can be configured via `AZURE_SUBSCRIPTION_ID` environment variable.

Assuming the use of the `DefaultAzureCredential` credential class, the client can be authenticated using the following code:

```java
AzureProfile profile = new AzureProfile(AzureCloud.AZURE_PUBLIC_CLOUD);
TokenCredential credential = new DefaultAzureCredentialBuilder()
    .authorityHost(profile.getEnvironment().getActiveDirectoryEndpoint())
    .build();
ServiceFabricManagedClustersManager manager = ServiceFabricManagedClustersManager
    .authenticate(credential, profile);
```

The sample code assumes global Azure. Please change the `AzureCloud.AZURE_PUBLIC_CLOUD` variable if otherwise.

See [Authentication][authenticate] for more options.

## Key concepts

See [API design][design] for general introduction on design and key concepts on Azure Management Libraries.

## Examples

```java
managedCluster = serviceFabricManagedClustersManager.managedClusters()
    .define(clusterName)
    .withRegion(REGION)
    .withExistingResourceGroup(resourceGroupName)
    .withSku(new Sku().withName(SkuName.STANDARD))
    .withAdminUsername(adminUser)
    .withAdminPassword(adminPassWord)
    .withDnsName(clusterName)
    .withClientConnectionPort(19000)
    .withHttpGatewayConnectionPort(19080)
    .create();
```
[Code snippets and samples](https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-servicefabricmanagedclusters_1.1.0-beta.1/sdk/servicefabricmanagedclusters/azure-resourcemanager-servicefabricmanagedclusters/SAMPLE.md)


## Troubleshooting

## Next steps

## Contributing

For details on contributing to this repository, see the [contributing guide][cg].

This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit <https://cla.microsoft.com>.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repositories using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct][coc]. For more information see the [Code of Conduct FAQ][coc_faq] or contact <opencode@microsoft.com> with any additional questions or comments.

<!-- LINKS -->
[survey]: https://microsoft.qualtrics.com/jfe/form/SV_ehN0lIk2FKEBkwd?Q_CHL=DOCS
[docs]: https://azure.github.io/azure-sdk-for-java/
[jdk]: https://learn.microsoft.com/azure/developer/java/fundamentals/
[azure_subscription]: https://azure.microsoft.com/free/
[azure_identity]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-servicefabricmanagedclusters_1.1.0-beta.1/sdk/identity/azure-identity
[azure_identity_credentials]: https://github.com/Azure/azure-sdk-for-java/tree/azure-resourcemanager-servicefabricmanagedclusters_1.1.0-beta.1/sdk/identity/azure-identity#credentials
[azure_core_http_netty]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-servicefabricmanagedclusters_1.1.0-beta.1/sdk/core/azure-core-http-netty
[authenticate]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-servicefabricmanagedclusters_1.1.0-beta.1/sdk/resourcemanager/docs/AUTH.md
[design]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-servicefabricmanagedclusters_1.1.0-beta.1/sdk/resourcemanager/docs/DESIGN.md
[cg]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-servicefabricmanagedclusters_1.1.0-beta.1/CONTRIBUTING.md
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/

