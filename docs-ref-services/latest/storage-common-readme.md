---
title: Azure Storage Common shared library for Java
keywords: Azure, java, SDK, API, azure-storage-common, storage
ms.date: 07/30/2025
ms.topic: reference
ms.devlang: java
ms.service: storage
---
# Azure Storage Common shared library for Java - version 12.30.1 

Azure Storage Common is a library that has common dependencies for all Azure Storage packages.

[Source code][source] | [API reference documentation][docs]

## Getting started

### Prerequisites

- [Java Development Kit (JDK)][jdk] with version 8 or above
  - Here are details about [Java 8 client compatibility with Azure Certificate Authority](https://learn.microsoft.com/azure/security/fundamentals/azure-ca-details?tabs=root-and-subordinate-cas-list#client-compatibility-for-public-pkis).
- [Azure Subscription][azure_subscription]
- [Create Storage Account][storage_account]

### Include the package

#### Include the BOM file

Please include the azure-sdk-bom to your project to take dependency on GA version of the library. In the following snippet, replace the {bom_version_to_target} placeholder with the version number.
To learn more about the BOM, see the [AZURE SDK BOM README](https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-common_12.30.1/sdk/boms/azure-sdk-bom/README.md).

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.azure</groupId>
            <artifactId>azure-sdk-bom</artifactId>
            <version>{bom_version_to_target}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
and then include the direct dependency in the dependencies section without the version tag.

```xml
<dependencies>
  <dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-storage-common</artifactId>
  </dependency>
</dependencies>
```

#### Include direct dependency
If you want to take dependency on a particular version of the library that is not present in the BOM,
add the direct dependency to your project as follows.


[//]: # ({x-version-update-start;com.azure:azure-storage-common;current})
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-storage-common</artifactId>
    <version>12.30.1</version>
</dependency>
```
[//]: # ({x-version-update-end})

## Key concepts


## Examples


## Troubleshooting

### Default HTTP Client
All client libraries by default use the Netty HTTP client. Adding the above dependency will automatically configure
the client library to use the Netty HTTP client. Configuring or changing the HTTP client is detailed in the
[HTTP clients wiki](https://learn.microsoft.com/azure/developer/java/sdk/http-client-pipeline#http-clients).

### Default SSL library
All client libraries, by default, use the Tomcat-native Boring SSL library to enable native-level performance for SSL
operations. The Boring SSL library is an uber jar containing native libraries for Linux / macOS / Windows, and provides
better performance compared to the default SSL implementation within the JDK. For more information, including how to
reduce the dependency size, refer to the [performance tuning](https://github.com/Azure/azure-sdk-for-java/wiki/Performance-Tuning) section of the wiki.

## Next steps

## Next steps Samples

## Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a [Contributor License Agreement (CLA)][cla] declaring that you have the right to, and actually do, grant us the rights to use your contribution.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct][coc]. For more information see the [Code of Conduct FAQ][coc_faq] or contact [opencode@microsoft.com][coc_contact] with any additional questions or comments.

<!-- LINKS -->
[source]: https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-common_12.30.1/sdk/storage/azure-storage-common/src
[samples_readme]: src/samples/README.md
[docs]: https://azure.github.io/azure-sdk-for-java/
[sas_token]: https://learn.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1
[jdk]: https://learn.microsoft.com/java/azure/jdk/
[azure_subscription]: https://azure.microsoft.com/free/
[storage_account]: https://learn.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal
[cla]: https://cla.microsoft.com
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/
[coc_contact]: mailto:opencode@microsoft.com



