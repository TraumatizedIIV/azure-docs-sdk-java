---
title: Azure Resource Manager Batch client library for Java
keywords: Azure, java, SDK, API, azure-resourcemanager-batch, batch
ms.date: 09/26/2024
ms.topic: reference
ms.devlang: java
ms.service: batch
---
# Azure Resource Manager Batch client library for Java - version 2.0.0 


Azure Resource Manager Batch client library for Java.

This package contains Microsoft Azure SDK for Batch Management SDK. Batch Client. Package tag package-2024-07. For documentation on how to use this package, please see [Azure Management Libraries for Java](https://aka.ms/azsdk/java/mgmt).

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

[//]: # ({x-version-update-start;com.azure.resourcemanager:azure-resourcemanager-batch;current})
```xml
<dependency>
    <groupId>com.azure.resourcemanager</groupId>
    <artifactId>azure-resourcemanager-batch</artifactId>
    <version>2.0.0</version>
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
AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);
TokenCredential credential = new DefaultAzureCredentialBuilder()
    .authorityHost(profile.getEnvironment().getActiveDirectoryEndpoint())
    .build();
BatchManager manager = BatchManager
    .authenticate(credential, profile);
```

The sample code assumes global Azure. Please change `AzureEnvironment.AZURE` variable if otherwise.

See [Authentication][authenticate] for more options.

## Key concepts

See [API design][design] for general introduction on design and key concepts on Azure Management Libraries.

## Examples

```java
// batch account
account = batchManager
    .batchAccounts()
    .define(batchAccountName)
    .withRegion(REGION)
    .withExistingResourceGroup(resourceGroup)
    .create();

// batch pool create
pool = batchManager.pools()
    .define(poolName)
    .withExistingBatchAccount(resourceGroup, batchAccountName)
    .withDisplayName(poolDisplayName)
    .withDeploymentConfiguration(
        new DeploymentConfiguration()
            .withVirtualMachineConfiguration(
                new VirtualMachineConfiguration()
                    .withImageReference(new ImageReference().withPublisher("Canonical")
                        .withOffer("UbuntuServer").withSku("18.04-LTS").withVersion("latest"))
                    .withNodeAgentSkuId("batch.node.ubuntu 18.04")))
    .withScaleSettings(
        new ScaleSettings()
            .withFixedScale(
                new FixedScaleSettings()
                    .withResizeTimeout(Duration.parse("PT8M"))
                    .withTargetDedicatedNodes(1)
                    .withTargetLowPriorityNodes(1)
                    .withNodeDeallocationOption(ComputeNodeDeallocationOption.TASK_COMPLETION)))
    .withVmSize("Standard_D1")
    .create();
```
[Code snippets and samples](https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-batch_2.0.0/sdk/batch/azure-resourcemanager-batch/SAMPLE.md)


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
[azure_identity]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-batch_2.0.0/sdk/identity/azure-identity
[azure_identity_credentials]: https://github.com/Azure/azure-sdk-for-java/tree/azure-resourcemanager-batch_2.0.0/sdk/identity/azure-identity#credentials
[azure_core_http_netty]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-batch_2.0.0/sdk/core/azure-core-http-netty
[authenticate]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-batch_2.0.0/sdk/resourcemanager/docs/AUTH.md
[design]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-batch_2.0.0/sdk/resourcemanager/docs/DESIGN.md
[cg]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-batch_2.0.0/CONTRIBUTING.md
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/



