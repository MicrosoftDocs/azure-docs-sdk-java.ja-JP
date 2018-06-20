---
title: Azure Storage Libraries for Java
description: ''
keywords: Azure, Java, SDK, API, Storage
author: douge
ms.author: douge
manager: douge
ms.date: 02/07/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: ec06e79374176b5a4795d27c5fbbb2260e65cd8c
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823655"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="88920-103">Azure Storage Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="88920-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="88920-104">概要</span><span class="sxs-lookup"><span data-stu-id="88920-104">Overview</span></span>

<span data-ttu-id="88920-105">Java アプリケーションから [Azure Storage](/azure/storage/storage-introduction) を使って、ファイル、BLOB (オブジェクト) データ、キーと値のペア、メッセージの読み取りと書き込みを行います。</span><span class="sxs-lookup"><span data-stu-id="88920-105">Read and write files, blob (object) data, key-value pairs, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="88920-106">Azure Storage の概要については、「[Java から BLOB ストレージを使用する方法](/azure/storage/storage-java-how-to-use-blob-storage)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88920-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/storage-java-how-to-use-blob-storage).</span></span>

## <a name="client-library"></a><span data-ttu-id="88920-107">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="88920-107">Client library</span></span>

<span data-ttu-id="88920-108">[接続文字列](/azure/storage/storage-create-storage-account#manage-your-storage-account)を使って Azure ストレージ アカウントに接続したうえで、クライアント ライブラリのクラスとメソッドを使って、BLOB、テーブル、ファイル、Queue Storage を操作します。</span><span class="sxs-lookup"><span data-stu-id="88920-108">Use [connection strings](/azure/storage/storage-create-storage-account#manage-your-storage-account) to connect to an Azure Storage account, then use the client libraries' classes and methods to work with blob, table, file, or queue storage.</span></span> 

<span data-ttu-id="88920-109">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="88920-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>7.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="88920-110">例</span><span class="sxs-lookup"><span data-stu-id="88920-110">Example</span></span>

<span data-ttu-id="88920-111">既にある Azure Storage BLOB コンテナーの新しい BLOB に対し、ローカル ファイル システムから画像ファイルを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="88920-111">Write a image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
String storageConnectionString = "DefaultEndpointsProtocol=https;" + 
"AccountName=fabrikamblobstorage;" + 
"AccountKey=keyvalue;EndpointSuffix=core.windows.net";

CloudStorageAccount account = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient serviceClient = account.createCloudBlobClient();
CloudBlobContainer container = serviceClient.getContainerReference(blobContainer);

// write a blob from a local filesystem path to the container as logo.png
CloudBlockBlob blob = container.getBlockBlobReference("logo.png");
blob.uploadFromFile("/Users/raisa/fabrikam.png");
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="88920-112">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="88920-112">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="88920-113">管理 API</span><span class="sxs-lookup"><span data-stu-id="88920-113">Management API</span></span>

<span data-ttu-id="88920-114">Azure ストレージ アカウントと接続キーの作成と管理は、Management API を使って行います。</span><span class="sxs-lookup"><span data-stu-id="88920-114">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="88920-115">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="88920-115">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="88920-116">例</span><span class="sxs-lookup"><span data-stu-id="88920-116">Example</span></span>

<span data-ttu-id="88920-117">自分のサブスクリプションに新しい Azure ストレージ アカウントを作成して、そのアクセス キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="88920-117">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="88920-118">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="88920-118">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="88920-119">サンプル</span><span class="sxs-lookup"><span data-stu-id="88920-119">Samples</span></span>

<span data-ttu-id="88920-120">[Azure ストレージ アカウントの管理](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span><span class="sxs-lookup"><span data-stu-id="88920-120">[Manage Azure Storage accounts](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span></span>  
<span data-ttu-id="88920-121">[Blob Storage を対象としたオブジェクトの読み取りと書き込み](https://github.com/Azure-Samples/storage-blob-java-getting-started) </span><span class="sxs-lookup"><span data-stu-id="88920-121">[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blob-java-getting-started) </span></span>  
<span data-ttu-id="88920-122">[キューを使ったメッセージの読み取りと書き込み](https://github.com/Azure-Samples/storage-queue-java-getting-started) </span><span class="sxs-lookup"><span data-stu-id="88920-122">[Read and write messages with queues](https://github.com/Azure-Samples/storage-queue-java-getting-started) </span></span>  
[<span data-ttu-id="88920-123">Web アプリで Blob Storage からファイルを読み取る</span><span class="sxs-lookup"><span data-stu-id="88920-123">Read files from blob storage in a web app</span></span>](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux)

<span data-ttu-id="88920-124">アプリで利用できる [Azure Storage のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=storage)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="88920-124">Explore more [sample Java code for Azure Storage](https://azure.microsoft.com/resources/samples/?platform=java&term=storage) you can use in your apps.</span></span>
