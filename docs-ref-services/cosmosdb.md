---
title: Azure Cosmos DB Libraries for Java
description: "Azure Cosmos DB 用 Java クライアント ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, SQL, データベース, MongoDB, Cosmos DB, NoSQL, DocumentDB"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/10/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cosmosdb
ms.openlocfilehash: 393f57df0ea2076c6ee7045b56883ee088716fad
ms.sourcegitcommit: 93107ca9ed76a29573a5faf8f39737c85e6bbaff
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
# <a name="azure-cosmos-db-libraries-for-java"></a>Azure Cosmos DB Libraries for Java

## <a name="overview"></a>概要

[Cosmos DB](/azure/cosmos-db/introduction) を使用して、キーと値、JSON ドキュメント、グラフ、および列指向データを、グローバルに分散したデータベースに格納したり照会したりします。

Cosmos DB を使い始めるには、Azure Cosmos DB の [Java と Azure Portal による API アプリの構築](/azure/cosmos-db/create-documentdb-java)に関するページを参照してください。

## <a name="client-library"></a>クライアント ライブラリ

[SQL クエリ構文](/azure/cosmos-db/documentdb-sql-query)で JSON データを操作するには、[DocumentDB](/azure/cosmos-db/documentdb-introduction) クライアント ライブラリを使用して Cosmos DB に接続します。

プロジェクトで Cosmos DB クライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a>例

SQL クエリ構文を使用して、Cosmos DB 内で一致する JSON ドキュメントを選択します。

```java
DocumentClient client = new DocumentClient("https://contoso.documents.azure.com:443",
                "contosoCosmosDBKey", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);

List<Document> results = client.queryDocuments("dbs/" + DATABASE_ID + "/colls/" + COLLECTION_ID,
        "SELECT * FROM myCollection WHERE myCollection.email = 'allen [at] contoso.com'",
        null)
    .getQueryIterable()
    .toList();

```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/cosmosdb/clientlibrary)


## <a name="samples"></a>サンプル

[Azure Cosmos DB MongoDB API を使用して Java アプリを開発する][2]   
[Azure Cosmos DB Graph API を使用して Java アプリを開発する][3]   
[Azure Cosmos DB DocumentDB API を使用して Java アプリを開発する][4]        

アプリで使用できる [Azure Cosmos DB 用の Java コードのサンプル](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos)を参照してください。

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started