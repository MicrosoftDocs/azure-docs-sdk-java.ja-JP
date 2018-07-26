---
title: Azure Cosmos DB Libraries for Java
description: Azure Cosmos DB 用 Java クライアント ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, SQL, データベース, MongoDB, Cosmos DB, NoSQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/10/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cosmosdb
ms.openlocfilehash: 6fc9f90cb3c8130aa82b20554a94a8b5ab78c083
ms.sourcegitcommit: 33c70f921f1524c8bdb69ad5a1a3c1b4b1de97ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026794"
---
# <a name="azure-cosmos-db-libraries-for-java"></a>Azure Cosmos DB Libraries for Java

## <a name="overview"></a>概要

[Azure Cosmos DB](/azure/cosmos-db/introduction) を使用して、キーと値、JSON ドキュメント、グラフ、列指向データを、グローバル分散データベースに格納し、照会します。

Azure Cosmos DB を使い始めるには、Azure Cosmos DB の [Java と Azure Portal を使用した API アプリの構築](/azure/cosmos-db/create-sql-api-java)に関する記事をご覧ください。

## <a name="client-library"></a>クライアント ライブラリ

[SQL クエリ構文](/azure/cosmos-db/sql-api-sql-query)で JSON データを操作するには、[SQL API](/azure/cosmos-db/sql-api-introduction) クライアント ライブラリを使用して Azure Cosmos DB に接続します。

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
> [クライアント API を探す](/java/api/overview/azure/cosmosdb/client)


## <a name="samples"></a>サンプル

[Azure Cosmos DB MongoDB API を使用して Java アプリを開発する][2]   
[Azure Cosmos DB Graph API を使用して Java アプリを開発する][3]   
[Azure Cosmos DB SQL API を使用して Java アプリを開発する][4]        

アプリで使用できる [Azure Cosmos DB 用の Java コードのサンプル](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos)を参照してください。

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started
