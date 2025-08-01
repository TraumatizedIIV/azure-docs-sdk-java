---
title: Azure File Data Lake client library for Java
keywords: Azure, java, SDK, API, azure-storage-file-datalake, storage
ms.date: 07/30/2025
ms.topic: reference
ms.devlang: java
ms.service: storage
---
# Azure File Data Lake client library for Java - version 12.24.1 


Azure Data Lake Storage is Microsoft's optimized storage solution for for big
data analytics workloads. A fundamental part of Data Lake Storage Gen2 is the
addition of a hierarchical namespace to Blob storage. The hierarchical
namespace organizes objects/files into a hierarchy of directories for
efficient data access.

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
To learn more about the BOM, see the [AZURE SDK BOM README](https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-file-datalake_12.24.1/sdk/boms/azure-sdk-bom/README.md).

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
    <artifactId>azure-storage-file-datalake</artifactId>
  </dependency>
</dependencies>
```

#### Include direct dependency
If you want to take dependency on a particular version of the library that is not present in the BOM,
add the direct dependency to your project as follows.

[//]: # ({x-version-update-start;com.azure:azure-storage-file-datalake;current})
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-storage-file-datalake</artifactId>
    <version>12.24.1</version>
</dependency>
```
[//]: # ({x-version-update-end})

### Create a Storage Account
To create a Storage Account you can use the [Azure Portal][storage_account_create_portal] or [Azure CLI][storage_account_create_cli].
Note: To use data lake, your account must have hierarchical namespace enabled.

```bash
# Install the extension “Storage-Preview”
az extension add --name storage-preview
# Create the storage account
az storage account create -n my-storage-account-name -g my-resource-group --sku Standard_LRS --kind StorageV2 --hierarchical-namespace true
```

Your storage account URL, subsequently identified as `<your-storage-account-url>`, would be formatted as follows
`http(s)://<storage-account-name>.dfs.core.windows.net`

### Authenticate the client

In order to interact with the Storage Service you'll need to create an instance of the Service Client class.
To make this possible you'll need the Account SAS (shared access signature) string of the Storage Account. Learn more at [SAS Token][sas_token]

#### Get credentials

##### SAS Token

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

DataLake Storage Gen2 was designed to:
- Service multiple petabytes of information while sustaining hundreds of gigabits of throughput
- Allow you to easily manage massive amounts of data

Key Features of DataLake Storage Gen2 include:
- Hadoop compatible access
- A superset of POSIX permissions
- Cost effective in terms of low-cost storage capacity and transactions
- Optimized driver for big data analytics

A fundamental part of Data Lake Storage Gen2 is the addition of a hierarchical namespace to Blob storage. The hierarchical namespace organizes objects/files into a hierarchy of directories for efficient data access.

In the past, cloud-based analytics had to compromise in areas of performance, management, and security. Data Lake Storage Gen2 addresses each of these aspects in the following ways:
- Performance is optimized because you do not need to copy or transform data as a prerequisite for analysis. The hierarchical namespace greatly improves the performance of directory management operations, which improves overall job performance.
- Management is easier because you can organize and manipulate files through directories and subdirectories.
- Security is enforceable because you can define POSIX permissions on directories or individual files.
- Cost effectiveness is made possible as Data Lake Storage Gen2 is built on top of the low-cost Azure Blob storage. The additional features further lower the total cost of ownership for running big data analytics on Azure.

Data Lake Storage Gen2 offers two types of resources:

- The `_filesystem` used via 'DataLakeFileSystemClient'
- The `_path` used via 'DataLakeFileClient' or 'DataLakeDirectoryClient'

|ADLS Gen2                  | Blob       |
| --------------------------| ---------- |
|Filesystem                 | Container  |
|Path (File or Directory)   | Blob       |

Note: This client library does not support hierarchical namespace (HNS) disabled storage accounts.

### URL format
Paths are addressable using the following URL format:
The following URL addresses a file:
```
https://${myaccount}.dfs.core.windows.net/${myfilesystem}/${myfile}
```

#### Resource URI Syntax
For the storage account, the base URI for datalake operations includes the name of the account only:

```
https://${myaccount}.dfs.core.windows.net
```

For a file system, the base URI includes the name of the account and the name of the file system:

```
https://${myaccount}.dfs.core.windows.net/${myfilesystem}
```

For a file/directory, the base URI includes the name of the account, the name of the file system and the name of the path:

```
https://${myaccount}.dfs.core.windows.net/${myfilesystem}/${mypath}
```

Note that the above URIs may not hold for more advanced scenarios such as custom domain names.

## Examples

The following sections provide several code snippets covering some of the most common Azure Storage Blob tasks, including:

- [Create a `DataLakeServiceClient`](#create-a-datalakeserviceclient)
- [Create a `DataLakeFileSystemClient`](#create-a-datalakefilesystemclient)
- [Create a `DataLakeFileClient`](#create-a-datalakefileclient)
- [Create a `DataLakeDirectoryClient`](#create-a-datalakedirectoryclient)
- [Create a file system](#create-a-file-system)
- [Enumerate paths](#enumerate-paths)
- [Rename a file](#rename-a-file)
- [Rename a directory](#rename-a-directory)
- [Get file properties](#get-file-properties)
- [Get directory properties](#get-directory-properties)
- [Authenticate with Azure Identity](#authenticate-with-azure-identity)

### Create a `DataLakeServiceClient`

Create a `DataLakeServiceClient` using the [`sasToken`](#get-credentials) generated above.

```java readme-sample-getDataLakeServiceClient1
DataLakeServiceClient dataLakeServiceClient = new DataLakeServiceClientBuilder()
    .endpoint("<your-storage-account-url>")
    .sasToken("<your-sasToken>")
    .buildClient();
```

or

```java readme-sample-getDataLakeServiceClient2
// Only one "?" is needed here. If the sastoken starts with "?", please removing one "?".
DataLakeServiceClient dataLakeServiceClient = new DataLakeServiceClientBuilder()
    .endpoint("<your-storage-account-url>" + "?" + "<your-sasToken>")
    .buildClient();
```

### Create a `DataLakeFileSystemClient`

Create a `DataLakeFileSystemClient` using a `DataLakeServiceClient`.

```java readme-sample-getDataLakeFileSystemClient1
DataLakeFileSystemClient dataLakeFileSystemClient = dataLakeServiceClient.getFileSystemClient("myfilesystem");
```

or

Create a `DataLakeFileSystemClient` from the builder [`sasToken`](#get-credentials) generated above.

```java readme-sample-getDataLakeFileSystemClient2
DataLakeFileSystemClient dataLakeFileSystemClient = new DataLakeFileSystemClientBuilder()
    .endpoint("<your-storage-account-url>")
    .sasToken("<your-sasToken>")
    .fileSystemName("myfilesystem")
    .buildClient();
```

or

```java readme-sample-getDataLakeFileSystemClient3
// Only one "?" is needed here. If the sastoken starts with "?", please removing one "?".
DataLakeFileSystemClient dataLakeFileSystemClient = new DataLakeFileSystemClientBuilder()
    .endpoint("<your-storage-account-url>" + "/" + "myfilesystem" + "?" + "<your-sasToken>")
    .buildClient();
```

### Create a `DataLakeFileClient`

Create a `DataLakeFileClient` using a `DataLakeFileSystemClient`.

```java readme-sample-getFileClient1
DataLakeFileClient fileClient = dataLakeFileSystemClient.getFileClient("myfile");
```

or

Create a `FileClient` from the builder [`sasToken`](#get-credentials) generated above.

```java readme-sample-getFileClient2
DataLakeFileClient fileClient = new DataLakePathClientBuilder()
    .endpoint("<your-storage-account-url>")
    .sasToken("<your-sasToken>")
    .fileSystemName("myfilesystem")
    .pathName("myfile")
    .buildFileClient();
```

or

```java readme-sample-getFileClient3
// Only one "?" is needed here. If the sastoken starts with "?", please removing one "?".
DataLakeFileClient fileClient = new DataLakePathClientBuilder()
    .endpoint("<your-storage-account-url>" + "/" + "myfilesystem" + "/" + "myfile" + "?" + "<your-sasToken>")
    .buildFileClient();
```

### Create a `DataLakeDirectoryClient`

Get a `DataLakeDirectoryClient` using a `DataLakeFileSystemClient`.

```java readme-sample-getDirClient1
DataLakeDirectoryClient directoryClient = dataLakeFileSystemClient.getDirectoryClient("mydir");
```

or

Create a `DirectoryClient` from the builder [`sasToken`](#get-credentials) generated above.

```java readme-sample-getDirClient2
DataLakeDirectoryClient directoryClient = new DataLakePathClientBuilder()
    .endpoint("<your-storage-account-url>")
    .sasToken("<your-sasToken>")
    .fileSystemName("myfilesystem")
    .pathName("mydir")
    .buildDirectoryClient();
```

or

```java readme-sample-getDirClient3
// Only one "?" is needed here. If the sastoken starts with "?", please removing one "?".
DataLakeDirectoryClient directoryClient = new DataLakePathClientBuilder()
    .endpoint("<your-storage-account-url>" + "/" + "myfilesystem" + "/" + "mydir" + "?" + "<your-sasToken>")
    .buildDirectoryClient();
```

### Create a file system

Create a file system using a `DataLakeServiceClient`.

```java readme-sample-createDataLakeFileSystemClient1
dataLakeServiceClient.createFileSystem("myfilesystem");
```

or

Create a file system using a `DataLakeFileSystemClient`.

```java readme-sample-createDataLakeFileSystemClient2
dataLakeFileSystemClient.create();
```

### Enumerate paths

Enumerating all paths using a `DataLakeFileSystemClient`.

```java readme-sample-enumeratePaths
for (PathItem pathItem : dataLakeFileSystemClient.listPaths()) {
    System.out.println("This is the path name: " + pathItem.getName());
}
```

### Rename a file

Rename a file using a `DataLakeFileClient`.

```java readme-sample-renameFile
//Need to authenticate with azure identity and add role assignment "Storage Blob Data Contributor" to do the following operation.
DataLakeFileClient fileClient = dataLakeFileSystemClient.getFileClient("myfile");
fileClient.create();
fileClient.rename("new-file-system-name", "new-file-name");
```

### Rename a directory

Rename a directory using a `DataLakeDirectoryClient`.

```java readme-sample-renameDirectory
//Need to authenticate with azure identity and add role assignment "Storage Blob Data Contributor" to do the following operation.
DataLakeDirectoryClient directoryClient = dataLakeFileSystemClient.getDirectoryClient("mydir");
directoryClient.create();
directoryClient.rename("new-file-system-name", "new-directory-name");
```

### Get file properties

Get properties from a file using a `DataLakeFileClient`.

```java readme-sample-getPropertiesFile
DataLakeFileClient fileClient = dataLakeFileSystemClient.getFileClient("myfile");
fileClient.create();
PathProperties properties = fileClient.getProperties();
```

### Get directory properties

Get properties from a directory using a `DataLakeDirectoryClient`.

```java readme-sample-getPropertiesDirectory
DataLakeDirectoryClient directoryClient = dataLakeFileSystemClient.getDirectoryClient("mydir");
directoryClient.create();
PathProperties properties = directoryClient.getProperties();
```

### Authenticate with Azure Identity

The [Azure Identity library][identity] provides Azure Active Directory support for authenticating with Azure Storage.

```java readme-sample-authWithIdentity
DataLakeServiceClient storageClient = new DataLakeServiceClientBuilder()
    .endpoint("<your-storage-account-url>")
    .credential(new DefaultAzureCredentialBuilder().build())
    .buildClient();
```

## Troubleshooting

When interacting with data lake using this Java client library, errors returned by the service correspond to the same HTTP
status codes returned for [REST API][error_codes] requests. For example, if you try to retrieve a file system or path that
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

Several Storage datalake  Java SDK samples are available to you in the SDK's GitHub repository.

## Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a [Contributor License Agreement (CLA)][cla] declaring that you have the right to, and actually do, grant us the rights to use your contribution.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct][coc]. For more information see the [Code of Conduct FAQ][coc_faq] or contact [opencode@microsoft.com][coc_contact] with any additional questions or comments.

<!-- LINKS -->
[source]: https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-file-datalake_12.24.1/sdk/storage/azure-storage-file-datalake/src
[samples_readme]: src/samples/README.md
[docs]: https://azure.github.io/azure-sdk-for-java/
[rest_docs]: https://learn.microsoft.com/rest/api/storageservices/data-lake-storage-gen2
[product_docs]: https://learn.microsoft.com/azure/storage/blobs/data-lake-storage-introduction
[sas_token]: https://learn.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1
[jdk]: https://learn.microsoft.com/java/azure/jdk/
[azure_subscription]: https://azure.microsoft.com/free/
[storage_account]: https://learn.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal
[storage_account_create_cli]: https://learn.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli
[storage_account_create_portal]: https://learn.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal
[identity]: https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-file-datalake_12.24.1/sdk/identity/azure-identity/README.md
[samples]: https://github.com/Azure/azure-sdk-for-java/blob/azure-storage-file-datalake_12.24.1/sdk/storage/azure-storage-file-datalake/src/samples
[error_codes]: https://learn.microsoft.com/rest/api/storageservices/data-lake-storage-gen2
[cla]: https://cla.microsoft.com
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/
[coc_contact]: mailto:opencode@microsoft.com
[performance_tuning]: https://github.com/Azure/azure-sdk-for-java/wiki/Performance-Tuning



