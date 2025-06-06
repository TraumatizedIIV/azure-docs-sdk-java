---
title: Azure Template client library for Java
keywords: Azure, java, SDK, API, azure-sdk-template, template
ms.date: 04/15/2025
ms.topic: reference
ms.devlang: java
ms.service: template
---
# Azure Template client library for Java - version 1.2.1-beta.4762296 


Use the guidelines in each section of this template to ensure consistency and readability of your README. 
The README resides in your package's GitHub repository at the root of its directory within the repo. 
It's also used as the package distribution page (NuGet, PyPi, npm, etc.) and as a Quickstart on learn.microsoft.com. 

**Title**: The H1 of your README should be in the format: `# [Product Name] client library for [Language]`

* All headings, including the H1, should use **sentence-style capitalization**. Refer to the [Microsoft Style Guide][style-guide-msft].
* Example: `# Azure Batch client library for Java - version 1.2.1-beta.4762296 
`

**Introduction**: The introduction appears directly under the title (H1) of your README.

* **DO NOT** use an "Introduction" or "Overview" heading (H2) for this section.
* First sentence: **Describe the service** briefly. You can usually use the first line of the service's docs landing page 
  for this (Example: [Cosmos DB docs landing page](https://learn.microsoft.com/azure/cosmos-db/)).
* Next, add a **bulleted list** of the **most common tasks** supported by the package or library, prefaced with 
  "Use the client library for [Product Name] to:". Then, provide code snippets for these tasks in the [Examples](#examples) 
  section later in the document. Keep the task list short but include those tasks most developers need to perform with your package.

> TIP: Your README should be as **brief** as possible but **no more brief** than necessary to get a developer new to Azure, 
> the service, or the package up and running quickly. Keep it brief, but include everything a developer needs to make 
> their first API call successfully.

## Getting started

This section should include everything a developer needs to do to install and create their first client connection *very quickly*.

### Install the package

First, provide instruction for obtaining and installing the package or library. This section might include only a single
line of code, like `pip install package-name`, but should enable a developer to successfully install the package from 
NuGet, pip, npm, Maven, or even cloning a GitHub repository.

Include a **Prerequisites** line after the install command that details any requirements that must be satisfied before 
a developer can [authenticate](#authenticate-the-client) and test all the snippets in the [Examples](#examples) section. 
For example, for Cosmos DB:

**Prerequisites**: You must have an [Azure subscription](https://azure.microsoft.com/free/), [Cosmos DB account](https://learn.microsoft.com/azure/cosmos-db/account-overview) (SQL API), and [Java Development Kit (JDK) with version 8 or above][jdk] to use this package.

> TODO: Once the library has GA'ed include the instructions on how to include the BOM file directly. And the benefit of using the BOM file over adding a direct dependency to the project.

### Authenticate the client

If your library requires authentication for use, such as for Azure services, include instructions and example code 
needed for initializing and authenticating.

For example, include details on obtaining an account key and endpoint URI, setting environment variables for each, and 
initializing the client object.

## Key concepts

The *Key concepts* section should describe the functionality of the main classes. Point out the most important and 
useful classes in the package (with links to their reference pages) and explain how those classes work together. Feel 
free to use bulleted lists, tables, code blocks, or even diagrams for clarity.

## Examples

Include code snippets and short descriptions for each task you listed in the [Introduction](#introduction) (the bulleted list). 
Briefly explain each operation, but include enough clarity to explain complex or otherwise tricky operations.

If possible, use the same example snippets that your in-code documentation uses. For example, use the snippets in your 
`ReadmeSamples.java` that `codesnippet-maven-plugin` ingests via its [README syntax](https://github.com/Azure/azure-sdk-tools/tree/main/packages/java-packages/codesnippet-maven-plugin#injecting-codesnippets-into-readmes). 
The `ReadmeSamples.java` file containing the snippets should reside alongside your package's code, and should be 
validated in an automated fashion.

Each example in the *Examples* section starts with an H3 that describes the example. At the top of this section, just 
under the *Examples* H2, add a bulleted list linking to each example H3. Each example should deep-link to the types 
and/or members used in the example.

* [Create the thing](#create-the-thing)
* [Get the thing](#get-the-thing)
* [List the things](#list-the-things)

### Create the thing

Use the `createThing` method to create a Thing reference; this method does not make a network call. To persist the 
Thing in the service, call `Thing.save`.

```java
Thing thing = client.createThing(id, name);
thing.save();
```

### Get the thing

The `getThing` method retrieves a Thing from the service. The `id` parameter is the unique ID of the Thing, not its 
"name" property.

```java
Thing thing = client.getThing(id);
```

### List the things

Use `listThings` to get one or more Thing objects from the service. If there are no Things available, a `404` exception 
is thrown (see [Troubleshooting](#troubleshooting) for details on handling exceptions).

```java
List<Thing> things = client.listThings();
```

## Troubleshooting

Describe common errors and exceptions, how to "unpack" them if necessary, and include guidance for graceful handling and recovery.

Provide information to help developers avoid throttling or other service-enforced errors they might encounter. For example, 
provide guidance and examples for using retry or connection policies in the API.

If the package, or a related package supports it, include tips for logging or enabling instrumentation to help them debug their code.

### Enabling Logging

Azure SDKs for Java provide a consistent logging story to help aid in troubleshooting application errors and expedite
their resolution. The logs produced will capture the flow of an application before reaching the terminal state to help
locate the root issue. View the [logging][logging] wiki for guidance about enabling logging.

### Default HTTP Client

By default, a Netty based HTTP client will be used. The [HTTP clients wiki](https://learn.microsoft.com/azure/developer/java/sdk/http-client-pipeline#http-clients)
provides more information on configuring or changing the HTTP client.

## Next steps

* Provide a link to additional code examples, ideally to those sitting alongside the README in the package's `/samples` directory.
* If appropriate, point users to other packages that might be useful.
* If you think there's a good chance that developers might stumble across your package in error (because they're searching 
  for specific functionality and mistakenly think the package provides that functionality), point them to the packages 
  they might be looking for.
  
* After adding the new SDK, you need to include the package in the following locations
1. `version_client.txt` - include the package with the version.
2. parent pom - `<enlistmentroot>\pom.xml` - Multiple places in the file.

## Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a
[Contributor License Agreement (CLA)][cla] declaring that you have the right to, and actually do, grant us the rights
to use your contribution.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate
the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to
do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct][coc]. For more information see the
[Code of Conduct FAQ][coc_faq] or contact [opencode@microsoft.com][coc_contact] with any additional questions or comments.

<!-- LINKS -->
[style-guide-msft]: https://learn.microsoft.com/style-guide/capitalization
[jdk]: https://learn.microsoft.com/java/azure/jdk/?view=azure-java-stable
[logging]: https://github.com/Azure/azure-sdk-for-java/wiki/Logging-in-Azure-SDK
[cla]: https://cla.microsoft.com
[coc]: https://opensource.microsoft.com/codeofconduct/
[coc_faq]: https://opensource.microsoft.com/codeofconduct/faq/
[coc_contact]: mailto:opencode@microsoft.com



