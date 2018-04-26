---
title: Azure Data Lake Store Libraries for Java
description: Java Data Lake Store ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, ビッグ データ, Data Lake
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: bcd1fd17759f7d171006d7b2126019d00d06d1db
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="azure-data-lake-store-libraries-for-java"></a>Azure Data Lake Store Libraries for Java

## <a name="overview"></a>概要

[Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview) を使用して、分析用にあらゆるサイズ、種類、取り込み速度のデータを 1 つの場所にキャプチャします。

Data Lake Store を導入するには、「[Java で Azure Data Lake Store の使用を開始する](/azure/data-lake-store/data-lake-store-get-started-java-sdk)」を参照してください。


## <a name="client-library"></a>クライアント ライブラリ

クライアント ライブラリを使用して、Data Lake Store でファイルの読み取りと書き込み、アクセス許可とメタデータの設定、ファイルとディレクトリの管理を行います。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a>例

完全修飾ドメイン名と OAuth2 アクセス トークンから Data Lake クライアントを作成した後、Data Lake にファイルを作成し、そのファイルに書き込みます。

```java
// AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

// create directory
client.createDirectory("/a/b/w");
        
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/datalakestore/client)


## <a name="management-api"></a>管理 API

Management API を使用して、Data Lake Store アカウント、ファイアウォール規則、信頼できる ID プロバイダーを管理します。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/datalakestore/management)

## <a name="samples"></a>サンプル

[Azure Data Lake の概要][1] 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

アプリで利用できる [Azure Data Lake Store のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=lake)を探しましょう。
