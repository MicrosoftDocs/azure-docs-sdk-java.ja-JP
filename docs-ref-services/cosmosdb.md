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
# <a name="azure-cosmos-db-libraries-for-java"></a><span data-ttu-id="ebfde-104">Azure Cosmos DB Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="ebfde-104">Azure Cosmos DB libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="ebfde-105">概要</span><span class="sxs-lookup"><span data-stu-id="ebfde-105">Overview</span></span>

<span data-ttu-id="ebfde-106">[Cosmos DB](/azure/cosmos-db/introduction) を使用して、キーと値、JSON ドキュメント、グラフ、および列指向データを、グローバルに分散したデータベースに格納したり照会したりします。</span><span class="sxs-lookup"><span data-stu-id="ebfde-106">Store and query key-value, JSON document, graph, and columnar data in a globally distributed database with [Cosmos DB](/azure/cosmos-db/introduction).</span></span>

<span data-ttu-id="ebfde-107">Cosmos DB を使い始めるには、Azure Cosmos DB の [Java と Azure Portal による API アプリの構築](/azure/cosmos-db/create-documentdb-java)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ebfde-107">To get started with Cosmos DB, see [Azure Cosmos DB: Build an API app with Java and the Azure portal](/azure/cosmos-db/create-documentdb-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="ebfde-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="ebfde-108">Client library</span></span>

<span data-ttu-id="ebfde-109">[SQL クエリ構文](/azure/cosmos-db/documentdb-sql-query)で JSON データを操作するには、[DocumentDB](/azure/cosmos-db/documentdb-introduction) クライアント ライブラリを使用して Cosmos DB に接続します。</span><span class="sxs-lookup"><span data-stu-id="ebfde-109">Connect to Cosmos DB using the [DocumentDB](/azure/cosmos-db/documentdb-introduction) client library to work with JSON data with [SQL query syntax](/azure/cosmos-db/documentdb-sql-query).</span></span>

<span data-ttu-id="ebfde-110">プロジェクトで Cosmos DB クライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="ebfde-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the Cosmos DB client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="ebfde-111">例</span><span class="sxs-lookup"><span data-stu-id="ebfde-111">Example</span></span>

<span data-ttu-id="ebfde-112">SQL クエリ構文を使用して、Cosmos DB 内で一致する JSON ドキュメントを選択します。</span><span class="sxs-lookup"><span data-stu-id="ebfde-112">Select matching JSON documents in Cosmos DB using SQL query syntax.</span></span>

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
> [<span data-ttu-id="ebfde-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="ebfde-113">Explore the Client APIs</span></span>](/java/api/overview/azure/cosmosdb/clientlibrary)


## <a name="samples"></a><span data-ttu-id="ebfde-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="ebfde-114">Samples</span></span>

<span data-ttu-id="ebfde-115">[Azure Cosmos DB MongoDB API を使用して Java アプリを開発する][2] </span><span class="sxs-lookup"><span data-stu-id="ebfde-115">[Develop a Java app using Azure Cosmos DB MongoDB API][2] </span></span>  
<span data-ttu-id="ebfde-116">[Azure Cosmos DB Graph API を使用して Java アプリを開発する][3] </span><span class="sxs-lookup"><span data-stu-id="ebfde-116">[Develop a Java app using Azure Cosmos DB Graph API][3] </span></span>  
<span data-ttu-id="ebfde-117">[Azure Cosmos DB DocumentDB API を使用して Java アプリを開発する][4]</span><span class="sxs-lookup"><span data-stu-id="ebfde-117">[Develop a Java app using Azure Cosmos DB DocumentDB API][4]</span></span>        

<span data-ttu-id="ebfde-118">アプリで使用できる [Azure Cosmos DB 用の Java コードのサンプル](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ebfde-118">Explore more [sample Java code for Azure Cosmos DB](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos) you can use in your apps.</span></span>

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started