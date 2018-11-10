---
title: Azure Storage Libraries for Java
description: ''
keywords: Azure, Java, SDK, API, Storage
author: douge
ms.author: seguler
manager: dineshm
ms.date: 10/29/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: 68a5fdbf8e2fbfe83f9ea09112d64488e0c11d08
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235253"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="c35b4-103">Azure Storage Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="c35b4-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="c35b4-104">概要</span><span class="sxs-lookup"><span data-stu-id="c35b4-104">Overview</span></span>

<span data-ttu-id="c35b4-105">[Azure Storage](/azure/storage/storage-introduction) を使用して、Java アプリケーションから BLOB (オブジェクト) データ、ファイル、メッセージの読み取りと書き込みを行います。</span><span class="sxs-lookup"><span data-stu-id="c35b4-105">Read and write blob (object) data, files, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="c35b4-106">Azure Storage の概要については、[Java SDK v7 から Blob Storage を使用する方法](/azure/storage/blobs/storage-quickstart-blobs-java)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c35b4-106">To get started with Azure Storage, see [How to use Blob storage from Java SDK v7](/azure/storage/blobs/storage-quickstart-blobs-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="c35b4-107">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="c35b4-107">Client library</span></span>

<span data-ttu-id="c35b4-108">Azure Active Directory から共有キー、SAS トークン、または OAuth トークンを使用して、Azure Storage サービスによる承認を行います。</span><span class="sxs-lookup"><span data-stu-id="c35b4-108">Use a Shared Key, SAS token or an OAuth token from the Azure Active Directory to authorize with Azure Storage services.</span></span> <span data-ttu-id="c35b4-109">次に、クライアント ライブラリのクラスとメソッドを使用して、Blob Storage、File Storage、または Queue Storage を操作します。</span><span class="sxs-lookup"><span data-stu-id="c35b4-109">Then use the client libraries' classes and methods to work with blob, file, or queue storage.</span></span> 

<span data-ttu-id="c35b4-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="c35b4-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

<span data-ttu-id="c35b4-111">**従来の Azure Storage SDK v7 の依存関係**:</span><span class="sxs-lookup"><span data-stu-id="c35b4-111">**Dependency for the legacy Azure Storage SDK v7**:</span></span>
```XML
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>8.0.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="c35b4-112">例</span><span class="sxs-lookup"><span data-stu-id="c35b4-112">Example</span></span>

<span data-ttu-id="c35b4-113">ローカル ファイル システムから、既存の Azure Storage Blob コンテナー内の新しい BLOB に画像ファイルを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="c35b4-113">Write an image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
// Parse the connection string and create a blob client to interact with Blob storage
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();
CloudBlobContainer container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());         
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c35b4-114">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="c35b4-114">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="c35b4-115">管理 API</span><span class="sxs-lookup"><span data-stu-id="c35b4-115">Management API</span></span>

<span data-ttu-id="c35b4-116">Azure ストレージ アカウントと接続キーの作成と管理は、Management API を使って行います。</span><span class="sxs-lookup"><span data-stu-id="c35b4-116">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="c35b4-117">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="c35b4-117">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="c35b4-118">例</span><span class="sxs-lookup"><span data-stu-id="c35b4-118">Example</span></span>

<span data-ttu-id="c35b4-119">自分のサブスクリプションに新しい Azure ストレージ アカウントを作成して、そのアクセス キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="c35b4-119">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

```java
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
        .withRegion(Region.US_EAST)
        .withNewResourceGroup(rgName)
        .create();

// get a list of storage account keys related to the account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c35b4-120">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="c35b4-120">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="c35b4-121">サンプル</span><span class="sxs-lookup"><span data-stu-id="c35b4-121">Samples</span></span>

<span data-ttu-id="c35b4-122">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Blob Storage に対するオブジェクトの読み取りと書き込み](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span><span class="sxs-lookup"><span data-stu-id="c35b4-122">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span></span>  
[<span data-ttu-id="c35b4-123">キューを使ったメッセージの読み取りと書き込み</span><span class="sxs-lookup"><span data-stu-id="c35b4-123">Read and write messages with queues</span></span>](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
