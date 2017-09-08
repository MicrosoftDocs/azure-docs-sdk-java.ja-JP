---
title: Azure Storage Libraries for Java
description: 
keywords: Azure, Java, SDK, API, Storage
author: douge
ms.author: douge
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: 7c8b6958d5e2892f35af8771d9d53eda28c430ef
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-storage-libraries-for-java"></a>Azure Storage Libraries for Java

## <a name="overview"></a>概要

Java アプリケーションから [Azure Storage](/azure/storage/storage-introduction) を使って、ファイル、BLOB (オブジェクト) データ、キーと値のペア、メッセージの読み取りと書き込みを行います。

Azure Storage の概要については、「[Java から BLOB ストレージを使用する方法](/azure/storage/storage-java-how-to-use-blob-storage)」を参照してください。

## <a name="client-library"></a>クライアント ライブラリ

[接続文字列](/azure/storage/storage-create-storage-account#manage-your-storage-account)を使って Azure ストレージ アカウントに接続したうえで、クライアント ライブラリのクラスとメソッドを使って、BLOB、テーブル、ファイル、Queue Storage を操作します。 

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.3.1</version>
</dependency>
```   

### <a name="example"></a>例

既にある Azure Storage BLOB コンテナーの新しい BLOB に対し、ローカル ファイル システムから画像ファイルを書き込みます。


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
> [クライアント API を探す](/java/api/overview/azure/storage/clientlibrary)

## <a name="management-api"></a>Management API

Azure ストレージ アカウントと接続キーの作成と管理は、Management API を使って行います。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.1.2</version>
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
> [Management API を探す](/java/api/overview/azure/storage/managementapi)


## <a name="samples"></a>サンプル

[Azure ストレージ アカウントの管理](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)    
[Blob Storage を対象としたオブジェクトの読み取りと書き込み](https://github.com/Azure-Samples/storage-blob-java-getting-started)   
[キューを使ったメッセージの読み取りと書き込み](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
[Web アプリで Blob Storage からファイルを読み取る](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux)

アプリで利用できる [Azure Storage のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=storage)を探しましょう。