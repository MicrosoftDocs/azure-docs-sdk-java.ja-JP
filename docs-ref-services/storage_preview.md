---
title: Azure Storage Libraries for Java
description: ''
keywords: Azure, Java, SDK, API, Storage
author: douge
ms.author: douge
manager: douge
ms.date: 10/19/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: fba48dfa04f223dce72a0ee54da967565ebd3687
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235263"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="e2050-103">Azure Storage Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="e2050-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e2050-104">概要</span><span class="sxs-lookup"><span data-stu-id="e2050-104">Overview</span></span>

<span data-ttu-id="e2050-105">[Azure Storage](/azure/storage/storage-introduction) を使用して、Java アプリケーションから BLOB (オブジェクト) データ、ファイル、メッセージの読み取りと書き込みを行います。</span><span class="sxs-lookup"><span data-stu-id="e2050-105">Read and write blob (object) data, files, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="e2050-106">Azure Storage の概要については、「[Java から BLOB ストレージを使用する方法](/azure/storage/blobs/storage-quickstart-blobs-java-v10)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e2050-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/blobs/storage-quickstart-blobs-java-v10).</span></span>

## <a name="client-library"></a><span data-ttu-id="e2050-107">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="e2050-107">Client library</span></span>

<span data-ttu-id="e2050-108">Azure Active Directory から共有キー、SAS トークン、または OAuth トークンを使用して、Azure Storage サービスによる承認を行います。</span><span class="sxs-lookup"><span data-stu-id="e2050-108">Use a Shared Key, SAS token or an OAuth token from the Azure Active Directory to authorize with Azure Storage services.</span></span> <span data-ttu-id="e2050-109">次に、クライアント ライブラリのクラスとメソッドを使用して、Blob Storage、File Storage、または Queue Storage を操作します。</span><span class="sxs-lookup"><span data-stu-id="e2050-109">Then use the client libraries' classes and methods to work with blob, file, or queue storage.</span></span> 

<span data-ttu-id="e2050-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="e2050-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

<span data-ttu-id="e2050-111">**Blob service の依存関係**</span><span class="sxs-lookup"><span data-stu-id="e2050-111">**Dependency for Blob service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>10.1.0</version>
</dependency>
```

<span data-ttu-id="e2050-112">**Queue サービスの依存関係**</span><span class="sxs-lookup"><span data-stu-id="e2050-112">**Dependency for Queue service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-queue</artifactId>
    <version>10.0.0-Preview</version>
</dependency>
```


### <a name="example"></a><span data-ttu-id="e2050-113">例</span><span class="sxs-lookup"><span data-stu-id="e2050-113">Example</span></span>

<span data-ttu-id="e2050-114">ローカル ファイル システムから、既存の Azure Storage Blob コンテナー内の新しい BLOB に画像ファイルを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="e2050-114">Write an image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
// Retrieve the credentials and initialize SharedKeyCredentials
String accountName = System.getenv("AZURE_STORAGE_ACCOUNT");
String accountKey = System.getenv("AZURE_STORAGE_ACCESS_KEY");

// Create a BlockBlobURL to run operations on Block Blobs. Alternatively create a ServiceURL, or ContainerURL for operations on Blob service, and Blob containers
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);

// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final BlockBlobURL blobURL = new BlockBlobURL(
    new URL("https://" + accountName + ".blob.core.windows.net/mycontainer/myimage.jpg"), 
        StorageURL.createPipeline(creds, new PipelineOptions())
);

AsynchronousFileChannel fileChannel = AsynchronousFileChannel.open(Paths.get("myimage.jpg"));
TransferManager.uploadFileToBlockBlob(fileChannel, blobURL,0, null).blockingGet();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e2050-115">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="e2050-115">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="e2050-116">管理 API</span><span class="sxs-lookup"><span data-stu-id="e2050-116">Management API</span></span>

<span data-ttu-id="e2050-117">Azure ストレージ アカウントと接続キーの作成と管理は、Management API を使って行います。</span><span class="sxs-lookup"><span data-stu-id="e2050-117">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="e2050-118">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="e2050-118">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="e2050-119">例</span><span class="sxs-lookup"><span data-stu-id="e2050-119">Example</span></span>

<span data-ttu-id="e2050-120">自分のサブスクリプションに新しい Azure ストレージ アカウントを作成して、そのアクセス キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="e2050-120">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="e2050-121">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="e2050-121">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="e2050-122">サンプル</span><span class="sxs-lookup"><span data-stu-id="e2050-122">Samples</span></span>

<span data-ttu-id="e2050-123">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Blob Storage に対するオブジェクトの読み取りと書き込み](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span><span class="sxs-lookup"><span data-stu-id="e2050-123">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span></span>  
[<span data-ttu-id="e2050-124">キューを使ったメッセージの読み取りと書き込み</span><span class="sxs-lookup"><span data-stu-id="e2050-124">Read and write messages with queues</span></span>](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
