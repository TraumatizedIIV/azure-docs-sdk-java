---
title: Azure Device Update for IoT Hub client library for Java
keywords: Azure, java, SDK, API, azure-iot-deviceupdate, deviceupdate
ms.date: 07/30/2025
ms.topic: reference
ms.devlang: java
ms.service: deviceupdate
---
# Azure Device Update for IoT Hub client library for Java - version 1.0.28 


The library provides access to the Device Update for IoT Hub service that enables customers to publish updates for their IoT devices to the cloud, and then deploy these updates to their devices (approve updates to groups of devices managed and provisioned in IoT Hub).

  [Source code](https://github.com/Azure/azure-sdk-for-java/tree/azure-iot-deviceupdate_1.0.28/sdk) | [Product documentation](/azure/iot-hub-device-update/understand-device-update)

## Getting started

The complete Microsoft Azure SDK can be downloaded from the [Microsoft Azure Downloads](https://azure.microsoft.com/downloads/?sdk=java) page and ships with support for building deployment packages, integrating with tooling, rich command line tooling, and more.

For the best development experience, developers should use the official Microsoft NuGet packages for libraries. NuGet packages are regularly updated with new functionality and hotfixes.

### Prerequisites

- A [Java Development Kit (JDK)][jdk_link], version 8 or later.
- [Azure Subscription][azure_subscription]
- Device Update for IoT Hub instance
- Azure IoT Hub instance

### Include the Package

[//]: # ({x-version-update-start;com.azure:azure-iot-deviceupdate;current})
```xml
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-iot-deviceupdate</artifactId>
  <version>1.0.28</version>
</dependency>
```
[//]: # ({x-version-update-end})

### Authenticate the client

In order to interact with the Device Update for IoT Hub service, you will need to create an instance of a [TokenCredential class](/java/api/com.azure.core.credential.tokencredential?view=azure-java-stable) and pass it to the constructor of your `DeviceUpdateClientBuilder` class.

Please refer to [Java SDK Get Started document](/azure/developer/java/sdk/get-started#set-up-authentication) for more authentication configuration.

## Key concepts

Device Update for IoT Hub is a managed service that enables you to deploy over-the-air updates for your IoT devices. The client library has two main components:

- **DeviceUpdate**: update management (import, enumerate, delete, etc.)
- **DeviceManagement**: device management (enumerate devices and retrieve device properties), deployment management (start and monitor update deployments to a set of devices)

You can learn more about Device Update for IoT Hub by visiting [Device Update for IoT Hub](https://github.com/azure/iot-hub-device-update).

## Examples

You can familiarize yourself with different APIs using [Samples](https://github.com/Azure/azure-sdk-for-java/tree/azure-iot-deviceupdate_1.0.28/sdk/deviceupdate/azure-iot-deviceupdate/src/samples).

## Troubleshooting

All Device Update for IoT Hub service operations will throw a ErrorResponseException on failure with helpful ErrorCodes.

For example, if you use the `getUpdateWithResponse` operation and the model you are looking for doesn't exist, you can catch that specific HttpStatusCode to decide the operation that follows in that case.


``` java com.azure.iot.deviceupdate.DeviceUpdateClient.notfound
try {
    Response<BinaryData> response = client.getUpdateWithResponse("foo", "bar", "0.0.0.1",
            null);
} catch (HttpResponseException e) {
    if (e.getResponse().getStatusCode() == 404) {
        // update does not exist
        System.out.println("update does not exist");
    }
}
```

## Next steps

Get started with our [Device Update for IoT Hub samples](https://github.com/Azure/azure-sdk-for-java/tree/azure-iot-deviceupdate_1.0.28/sdk/deviceupdate/azure-iot-deviceupdate/src/samples)

## Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a [Contributor License Agreement (CLA)][cla] declaring that you have the right to, and actually do, grant us the rights to use your contribution.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct][coc]. For more information see the [Code of Conduct FAQ][coc_faq] or contact [opencode@microsoft.com][coc_contact] with any additional questions or comments.

<!-- LINKS -->
[azure_subscription]: https://azure.microsoft.com/free
[jdk_link]: /java/azure/jdk/?view=azure-java-stable
[cla]: https://cla.microsoft.com
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/
[coc_contact]: mailto:opencode@microsoft.com

![Impressions](https://azure-sdk-impressions.azurewebsites.net/api/impressions/azure-sdk-for-java%2Fsdk%2Fdeviceupdate%2Fazure-iot-deviceupdate%2FREADME.png)

