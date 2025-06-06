---
title: Azure Resource Manager DataFactory client library for Java
keywords: Azure, java, SDK, API, azure-resourcemanager-datafactory, datafactory
ms.date: 08/21/2024
ms.topic: reference
ms.devlang: java
ms.service: datafactory
---
# Azure Resource Manager DataFactory client library for Java - version 1.0.0-beta.30 


Azure Resource Manager DataFactory client library for Java.

This package contains Microsoft Azure SDK for DataFactory Management SDK. The Azure Data Factory V2 management API provides a RESTful set of web services that interact with Azure Data Factory V2 services. Package tag package-2018-06. For documentation on how to use this package, please see [Azure Management Libraries for Java](https://aka.ms/azsdk/java/mgmt).

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

[//]: # ({x-version-update-start;com.azure.resourcemanager:azure-resourcemanager-datafactory;current})
```xml
<dependency>
    <groupId>com.azure.resourcemanager</groupId>
    <artifactId>azure-resourcemanager-datafactory</artifactId>
    <version>1.0.0-beta.30</version>
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
DataFactoryManager manager = DataFactoryManager
    .authenticate(credential, profile);
```

The sample code assumes global Azure. Please change `AzureEnvironment.AZURE` variable if otherwise.

See [Authentication][authenticate] for more options.

## Key concepts

See [API design][design] for general introduction on design and key concepts on Azure Management Libraries.

## Examples

```java
// storage account
StorageAccount storageAccount = storageManager.storageAccounts().define(STORAGE_ACCOUNT)
    .withRegion(REGION)
    .withExistingResourceGroup(resourceGroup)
    .create();
final String storageAccountKey = storageAccount.getKeys().iterator().next().value();
final String connectionString = getStorageConnectionString(STORAGE_ACCOUNT, storageAccountKey, storageManager.environment());

// container
final String containerName = "adf";
storageManager.blobContainers().defineContainer(containerName)
    .withExistingStorageAccount(resourceGroup, STORAGE_ACCOUNT)
    .withPublicAccess(PublicAccess.NONE)
    .create();

// blob as input
BlobClient blobClient = new BlobClientBuilder()
    .connectionString(connectionString)
    .containerName(containerName)
    .blobName("input/data.txt")
    .buildClient();
blobClient.upload(BinaryData.fromString("data"));

// data factory
Factory dataFactory = manager.factories().define(DATA_FACTORY)
    .withRegion(REGION)
    .withExistingResourceGroup(resourceGroup)
    .create();

// linked service
final Map<String, String> connectionStringProperty = new HashMap<>();
connectionStringProperty.put("type", "SecureString");
connectionStringProperty.put("value", connectionString);

final String linkedServiceName = "LinkedService";
manager.linkedServices().define(linkedServiceName)
    .withExistingFactory(resourceGroup, DATA_FACTORY)
    .withProperties(new AzureStorageLinkedService()
        .withConnectionString(connectionStringProperty))
    .create();

// input dataset
final String inputDatasetName = "InputDataset";
manager.datasets().define(inputDatasetName)
    .withExistingFactory(resourceGroup, DATA_FACTORY)
    .withProperties(new AzureBlobDataset()
        .withLinkedServiceName(new LinkedServiceReference().withReferenceName(linkedServiceName))
        .withFolderPath(containerName)
        .withFileName("input/data.txt")
        .withFormat(new TextFormat()))
    .create();

// output dataset
final String outputDatasetName = "OutputDataset";
manager.datasets().define(outputDatasetName)
    .withExistingFactory(resourceGroup, DATA_FACTORY)
    .withProperties(new AzureBlobDataset()
        .withLinkedServiceName(new LinkedServiceReference().withReferenceName(linkedServiceName))
        .withFolderPath(containerName)
        .withFileName("output/data.txt")
        .withFormat(new TextFormat()))
    .create();

// pipeline
PipelineResource pipeline = manager.pipelines().define("CopyBlobPipeline")
    .withExistingFactory(resourceGroup, DATA_FACTORY)
    .withActivities(Collections.singletonList(new CopyActivity()
        .withName("CopyBlob")
        .withSource(new BlobSource())
        .withSink(new BlobSink())
        .withInputs(Collections.singletonList(new DatasetReference().withReferenceName(inputDatasetName)))
        .withOutputs(Collections.singletonList(new DatasetReference().withReferenceName(outputDatasetName)))))
    .create();

// run pipeline
CreateRunResponse createRun = pipeline.createRun();

// wait for completion
PipelineRun pipelineRun = manager.pipelineRuns().get(resourceGroup, DATA_FACTORY, createRun.runId());
String runStatus = pipelineRun.status();
while ("InProgress".equals(runStatus)) {
    sleepIfRunningAgainstService(10 * 1000);    // wait 10 seconds
    pipelineRun = manager.pipelineRuns().get(resourceGroup, DATA_FACTORY, createRun.runId());
    runStatus = pipelineRun.status();
}
```
[Code snippets and samples](https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-datafactory_1.0.0-beta.30/sdk/datafactory/azure-resourcemanager-datafactory/SAMPLE.md)


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
[azure_identity]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-datafactory_1.0.0-beta.30/sdk/identity/azure-identity
[azure_identity_credentials]: https://github.com/Azure/azure-sdk-for-java/tree/azure-resourcemanager-datafactory_1.0.0-beta.30/sdk/identity/azure-identity#credentials
[azure_core_http_netty]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-datafactory_1.0.0-beta.30/sdk/core/azure-core-http-netty
[authenticate]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-datafactory_1.0.0-beta.30/sdk/resourcemanager/docs/AUTH.md
[design]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-datafactory_1.0.0-beta.30/sdk/resourcemanager/docs/DESIGN.md
[cg]: https://github.com/Azure/azure-sdk-for-java/blob/azure-resourcemanager-datafactory_1.0.0-beta.30/CONTRIBUTING.md
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/



