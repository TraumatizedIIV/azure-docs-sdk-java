---
title: Azure Storage Blobs Cryptography client library for Java
keywords: Azure, java, SDK, API, azure-storage-blob-cryptography, storage
ms.date: 07/30/2025
ms.topic: reference
ms.devlang: java
ms.service: storage
---
# Azure Storage Blobs Cryptography client library for Java - version 12.30.1 


Azure Blob storage is Microsoft's object storage solution for the cloud. Blob
storage is optimized for storing massive amounts of unstructured data.
Unstructured data is data that does not adhere to a particular data model or
definition, such as text or binary data.
This package supports client side encryption for blob storage.

[Source code][source] | [API reference documentation][docs] | [REST API documentation][rest_docs] | [Product documentation][product_docs] | [Samples][samples]

## Getting started

### Prerequisites

- [Java Development Kit (JDK)][jdk] with version 8 or above
  - Here are details about [Java 8 client compatibility with Azure Certificate Authority](https://learn.microsoft.com/azure/security/fundamentals/azure-ca-details?tabs=root-and-subordinate-cas-list#client-compatibility-for-public-pkis).
- [Azure Subscription][azure_subscription]
- [Create Storage Account][storage_account]

### Include the package

#### Include the BOM file

Please include the azure-sdk-bom to your project to take dependency on GA version of the library. In the following snippet, replace the {bom_version_to_target} placeholder with the version number.
To learn more about the BOM, see the [AZURE SDK BOM README](https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-blob-cryptography_12.30.1/sdk/boms/azure-sdk-bom/README.md).

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
    <artifactId>azure-storage-blob-cryptography</artifactId>
  </dependency>
</dependencies>
```

#### Include direct dependency
If you want to take dependency on a particular version of the library that is not present in the BOM,
add the direct dependency to your project as follows.


[//]: # ({x-version-update-start;com.azure:azure-storage-blob-cryptography;current})
```xml
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-storage-blob-cryptography</artifactId>
  <version>12.30.1</version>
</dependency>
```
[//]: # ({x-version-update-end})

### Create a Storage Account
To create a Storage Account you can use the [Azure Portal][storage_account_create_portal] or [Azure CLI][storage_account_create_cli].

```bash
az storage account create \
    --resource-group <resource-group-name> \
    --name <storage-account-name> \
    --location <location>
```

### Authenticate the client

In order to interact with the Storage service (Blob, Queue, Message, MessageId, File) you'll need to create an instance of the Service Client class.
To make this possible you'll need the Account SAS (shared access signature) string of Storage account. Learn more at [SAS Token][sas_token]

#### Get credentials

- **SAS Token**

a. Use the Azure CLI snippet below to get the SAS token from the Storage Account.

```bash
az storage blob generate-sas \
    --account-name {Storage Account name} \
    --container-name {container name} \
    --name {blob name} \
    --permissions {permissions to grant} \
    --expiry {datetime to expire the SAS token} \
    --services {storage services the SAS allows} \
    --resource-types {resource types the SAS allows}
```

Example:

```bash
CONNECTION_STRING=<connection-string>

az storage blob generate-sas \
    --account-name MyStorageAccount \
    --container-name MyContainer \
    --name MyBlob \
    --permissions racdw \
    --expiry 2020-06-15
```

b. Alternatively, get the Account SAS Token from the Azure Portal.

1. Go to your Storage Account
2. Select `Shared access signature` from the menu on the left
3. Click on `Generate SAS and connection string` (after setup)

##### **Shared Key Credential**

a. Use Account name and Account key. Account name is your Storage Account name.

1. Go to your Storage Account
2. Select `Access keys` from the menu on the left
3. Under `key1`/`key2` copy the contents of the `Key` field

or

b. Use the connection string.

1. Go to your Storage Account
2. Select `Access keys` from the menu on the left
3. Under `key1`/`key2` copy the contents of the `Connection string` field

## Key concepts

Blob storage is designed for:

- Serving images or documents directly to a browser.
- Storing files for distributed access.
- Streaming video and audio.
- Writing to log files.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.

## Examples

Note: The usage of the `EncryptedBlobClient` is the same as the equivalent `BlobClient`, the only difference being client construction.
Please refer to `azure-storage-blob` for common use cases of the `BlobClient`

The following sections provide several code snippets covering some of the most common Azure Storage Blob cryptography creation tasks, including:

- [Create an `EncryptedBlobClient` from a `BlobClient`](#create-an-encryptedblobclient-from-a-blobclient)
- [Create an `EncryptedBlobClient`](#create-an-encryptedblobclient)
- [Use a `LocalKeyEncryptionKey`](#use-a-local-keyencryptionkey)
- [Use a `KeyVaultKey`](#use-a-keyvaultkey)

### Create an `EncryptedBlobClient` from a `BlobClient`

Create an `EncryptedBlobClient` using a `BlobClient`. `BlobClient` construction is explained in the `azure-storage-blob` README.

```java readme-sample-getEncryptedBlobClientBlobClient
EncryptedBlobClient client = new EncryptedBlobClientBuilder()
    .key(key, keyWrapAlgorithm)
    .keyResolver(keyResolver)
    .blobClient(blobClient)
    .buildEncryptedBlobClient();
```

### Create an `EncryptedBlobClient`

Create a `BlobServiceClient` using a connection string.

```java readme-sample-getEncryptedBlobClient
EncryptedBlobClient client = new EncryptedBlobClientBuilder()
    .key(key, keyWrapAlgorithm)
    .keyResolver(keyResolver)
    .connectionString(connectionString)
    .containerName(containerName)
    .blobName(blobName)
    .buildEncryptedBlobClient();
```

### Use a local `KeyEncryptionKey`

```java readme-sample-getClientLocalKey
JsonWebKey localKey = JsonWebKey.fromAes(new SecretKeySpec(keyBytes, secretKeyAlgorithm),
    Arrays.asList(KeyOperation.WRAP_KEY, KeyOperation.UNWRAP_KEY))
    .setId("my-id");
AsyncKeyEncryptionKey akek = new KeyEncryptionKeyClientBuilder()
    .buildAsyncKeyEncryptionKey(localKey).block();

EncryptedBlobClient client = new EncryptedBlobClientBuilder()
    .key(akek, keyWrapAlgorithm)
    .connectionString(connectionString)
    .containerName(containerName)
    .blobName(blobName)
    .buildEncryptedBlobClient();
```

### Use a `KeyVaultKey`

```java readme-sample-getClientKeyVaultKey
KeyClient keyClient = new KeyClientBuilder()
    .vaultUrl(keyVaultUrl)
    .credential(tokenCredential)
    .buildClient();
KeyVaultKey rsaKey = keyClient.createRsaKey(new CreateRsaKeyOptions(keyName)
    .setExpiresOn(OffsetDateTime.now().plusYears(1))
    .setKeySize(2048));
AsyncKeyEncryptionKey akek = new KeyEncryptionKeyClientBuilder()
    .credential(tokenCredential)
    .buildAsyncKeyEncryptionKey(rsaKey.getId())
    .block();

EncryptedBlobClient client = new EncryptedBlobClientBuilder()
    .key(akek, keyWrapAlgorithm)
    .connectionString(connectionString)
    .containerName(containerName)
    .blobName(blobName)
    .buildEncryptedBlobClient();
```

## Troubleshooting

When interacting with blobs using this Java client library, errors returned by the service correspond to the same HTTP
status codes returned for [REST API][error_codes] requests. For example, if you try to retrieve a container or blob that
doesn't exist in your Storage Account, a `404` error is returned, indicating `Not Found`.

### Default HTTP Client
All client libraries by default use the Netty HTTP client. Adding the above dependency will automatically configure
the client library to use the Netty HTTP client. Configuring or changing the HTTP client is detailed in the
[HTTP clients wiki](https://learn.microsoft.com/azure/developer/java/sdk/http-client-pipeline#http-clients).

### Default SSL library
All client libraries, by default, use the Tomcat-native Boring SSL library to enable native-level performance for SSL
operations. The Boring SSL library is an uber jar containing native libraries for Linux / macOS / Windows, and provides
better performance compared to the default SSL implementation within the JDK. For more information, including how to
reduce the dependency size, refer to the [performance tuning][performance_tuning] section of the wiki.

## Next steps

## Contributing
This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the Code of Conduct FAQ or contact opencode@microsoft.com with any additional questions or comments.



[jdk]: https://learn.microsoft.com/java/azure/jdk/
[source]: https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-blob-cryptography_12.30.1/sdk/storage/azure-storage-blob-cryptography/src
[docs]: https://azure.github.io/azure-sdk-for-java/
[rest_docs]: https://learn.microsoft.com/rest/api/storageservices/blob-service-rest-api
[product_docs]: https://learn.microsoft.com/azure/storage/blobs/storage-blobs-overview
[samples]: https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-blob-cryptography_12.30.1/sdk/storage/azure-storage-blob-cryptography/src/samples
[azure_subscription]: https://azure.microsoft.com/free/
[storage_account]: https://learn.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal
[storage_account_create_cli]: https://learn.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli
[storage_account_create_portal]: https://learn.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal
[sas_token]: https://learn.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1
[source]: https://github.com/Azure/azure-sdk-for-java/tree/azure-storage-blob-cryptography_12.30.1/sdk/storage/azure-storage-blob-cryptography/src/main/java
[docs]: https://azure.github.io/azure-sdk-for-java/
[rest_docs]: https://learn.microsoft.com/rest/api/storageservices/blob-service-rest-api
[product_docs]: https://learn.microsoft.com/azure/storage/blobs/storage-blobs-overview
[samples]: https://github.com/Azure/azure-sdk-for-java/tree/azure-storage-blob-cryptography_12.30.1/sdk/storage/azure-storage-blob-cryptography/src/samples
[error_codes]: https://learn.microsoft.com/rest/api/storageservices/blob-service-error-codes
[performance_tuning]: https://github.com/Azure/azure-sdk-for-java/wiki/Performance-Tuning

