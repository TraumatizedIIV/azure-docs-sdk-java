---
title: Azure Core Experimental shared library for Java
keywords: Azure, java, SDK, API, azure-core-experimental, core
ms.date: 06/30/2025
ms.topic: reference
ms.devlang: java
ms.service: core
---
# Azure Core Experimental shared library for Java - version 1.0.0-beta.62 


[![Build Documentation](https://img.shields.io/badge/documentation-published-blue.svg)](https://azure.github.io/azure-sdk-for-java)

Azure Core Experimental contains types that are being evaluated and might eventually become part of Azure Core, this library will always stay in a preview version and might allow breaking changes.

## Getting started

### Prerequisites

- A [Java Development Kit (JDK)][jdk_link], version 8 or later.
  - Here are details about [Java 8 client compatibility with Azure Certificate Authority][java8_client_compatibility].

### Include the package

[//]: # ({x-version-update-start;com.azure:azure-core-experimental;current})
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-core-experimental</artifactId>
    <version>1.0.0-beta.62</version>
</dependency>
```
[//]: # ({x-version-update-end})

## Key concepts

Azure Core experimental is reserved for features that are actively being prototyped and may be merged into Azure Core
in the future.

## Examples

## Troubleshooting

### Enabling Logging

Azure SDKs for Java offer a consistent logging story to help aid in troubleshooting application errors and expedite
their resolution. The logs produced will capture the flow of an application before reaching the terminal state to help
locate the root issue. View the [logging][logging] wiki for guidance about enabling logging.

## Next steps

## Contributing

For details on contributing to this repository, see the [contributing guide](https://github.com/Azure/azure-sdk-for-java/blob/azure-core-experimental_1.0.0-beta.62/CONTRIBUTING.md).

1. Fork it
1. Create your feature branch (`git checkout -b my-new-feature`)
1. Commit your changes (`git commit -am 'Add some feature'`)
1. Push to the branch (`git push origin my-new-feature`)
1. Create new Pull Request

<!-- Links -->
[logging]: https://github.com/Azure/azure-sdk-for-java/wiki/Logging-in-Azure-SDK
[jdk_link]: https://learn.microsoft.com/java/azure/jdk/?view=azure-java-stable
[java8_client_compatibility]: https://learn.microsoft.com/azure/security/fundamentals/azure-ca-details?tabs=root-and-subordinate-cas-list#client-compatibility-for-public-pkis



