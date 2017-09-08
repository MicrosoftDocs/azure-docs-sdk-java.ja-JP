---
title: Azure Database for PostgreSQL Libraries for Java
description: "Azure Database for PostgreSQL 用 Java クライアント ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, SQL, データベース, PostGres, PostgreSQL"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: postgresql
ms.openlocfilehash: d6fa2acb9a2f44b157ae52ba6e41f6777a43b574
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-database-for-postgresql-libraries-for-java"></a><span data-ttu-id="dde2d-104">Azure Database for PostgreSQL Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="dde2d-104">Azure Database for PostgreSQL libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="dde2d-105">概要</span><span class="sxs-lookup"><span data-stu-id="dde2d-105">Overview</span></span>

<span data-ttu-id="dde2d-106">[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) は、オープン ソースの [PostgreSQL](https://www.postgresql.org/) データベース エンジンのコミュニティ バージョンに基づいて開発者向けに構築された、Azure のリレーショナル データベース サービスです。</span><span class="sxs-lookup"><span data-stu-id="dde2d-106">[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) is a relational database service in Azure built for developers based on the community version of open source [PostgreSQL](https://www.postgresql.org/) database engine.</span></span>

<span data-ttu-id="dde2d-107">Azure Database for PostgreSQL の概要については、「[Java を使用した接続とデータの照会](/azure/postgresql/connect-java)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="dde2d-107">To get started with Azure Database for PostgreSQL, see [Use Java to connect and query data](/azure/postgresql/connect-java).</span></span>

## <a name="client-jdbc-driver"></a><span data-ttu-id="dde2d-108">クライアント JDBC ドライバー</span><span class="sxs-lookup"><span data-stu-id="dde2d-108">Client JDBC driver</span></span>

<span data-ttu-id="dde2d-109">Azure Database for PostgreSQL には、アプリケーションからオープン ソースの [PostgreSQL JDBC ドライバー](https://jdbc.postgresql.org/)を使用して接続します。</span><span class="sxs-lookup"><span data-stu-id="dde2d-109">Connect to Azure Database for PostgreSQL from your applications using the open-source [PostgreSQL JDBC driver](https://jdbc.postgresql.org/).</span></span> <span data-ttu-id="dde2d-110">[Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使用してデータベースに直接接続できるほか、JDBC 経由でデータベースと対話するデータ アクセス フレームワーク ([Hibernate](http://hibernate.org/)) を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dde2d-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect to the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="dde2d-111">プロジェクトでクライアント JDBC ドライバーを使用するためには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="dde2d-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>  

```XML
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.1.1</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="dde2d-112">例</span><span class="sxs-lookup"><span data-stu-id="dde2d-112">Example</span></span>

<span data-ttu-id="dde2d-113">JDBC を使用して Azure Database for PostgreSQL に接続し、sales テーブル内の全レコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="dde2d-113">Connect to Azure Database for PostgreSQL using JDBC and select all records in the sales table.</span></span> <span data-ttu-id="dde2d-114">データベースの JDBC 接続文字列は、Azure Portal から取得できます。</span><span class="sxs-lookup"><span data-stu-id="dde2d-114">You can get the JDBC connection string for the database from the Azure Portal.</span></span>

```java
String url = String.format("jdbc:postgresql://mypostgresdb.postgres.database.azure.com:5432/mydb?user=frank@mypostgresdb&password=AbCdEfGhIjK&ssl=true");
Connection connection = null;
try {
    connection = DriverManager.getConnection(url);
    String selectSql = "SELECT * FROM SALES";
    Statement statement = connection.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a><span data-ttu-id="dde2d-115">サンプル</span><span class="sxs-lookup"><span data-stu-id="dde2d-115">Samples</span></span>

[<span data-ttu-id="dde2d-116">Azure CLI を使用して PostgreSQL データベースを設計する</span><span class="sxs-lookup"><span data-stu-id="dde2d-116">Design a PostgreSQL database using the Azure CLI</span></span>](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-cli) 

<span data-ttu-id="dde2d-117">アプリで利用できる [Azure Database for PostgreSQL のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="dde2d-117">Explore more [sample Java code for Azure Database for PostgreSQL](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres) you can use in your apps.</span></span>
