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
# <a name="azure-storage-libraries-for-java"></a>Azure Storage Libraries for Java

## <a name="overview"></a>概要

[Azure Storage](/azure/storage/storage-introduction) を使用して、Java アプリケーションから BLOB (オブジェクト) データ、ファイル、メッセージの読み取りと書き込みを行います。

Azure Storage の概要については、[Java SDK v7 から Blob Storage を使用する方法](/azure/storage/blobs/storage-quickstart-blobs-java)に関するページをご覧ください。

## <a name="client-library"></a>クライアント ライブラリ

Azure Active Directory から共有キー、SAS トークン、または OAuth トークンを使用して、Azure Storage サービスによる承認を行います。 次に、クライアント ライブラリのクラスとメソッドを使用して、Blob Storage、File Storage、または Queue Storage を操作します。 

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。   

**従来の Azure Storage SDK v7 の依存関係**:
```XML
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>8.0.0</version>
</dependency>
```

### <a name="example"></a>例

ローカル ファイル システムから、既存の Azure Storage Blob コンテナー内の新しい BLOB に画像ファイルを書き込みます。


```java
// Parse the connection string and create a blob client to interact with Blob storage
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();
CloudBlobContainer container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());         
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/storage/client)

## <a name="management-api"></a>管理 API

Azure ストレージ アカウントと接続キーの作成と管理は、Management API を使って行います。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a>例

自分のサブスクリプションに新しい Azure ストレージ アカウントを作成して、そのアクセス キーを取得します。

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
> [Management API を探す](/java/api/overview/azure/storage/management)


## <a name="samples"></a>サンプル

[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Blob Storage に対するオブジェクトの読み取りと書き込み](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart)   
[キューを使ったメッセージの読み取りと書き込み](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
