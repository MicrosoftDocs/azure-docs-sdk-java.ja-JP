---
title: Azure Database for MySQL Libraries for Java
description: Azure Database for MySQL 用 Java クライアント ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, SQL, データベース, PostGres, MySQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: mysql
ms.openlocfilehash: 72c94ef4bdad7adeae63da2efda93b52a9afef53
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931018"
---
# <a name="azure-database-for-mysql-libraries-for-java"></a><span data-ttu-id="87c6c-104">Azure Database for MySQL Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="87c6c-104">Azure Database for MySQL libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="87c6c-105">概要</span><span class="sxs-lookup"><span data-stu-id="87c6c-105">Overview</span></span>

<span data-ttu-id="87c6c-106">[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) は、オープン ソースの [MySQL](https://www.mysql.com/) Server エンジンに基づいたリレーショナル データベース サービスです。</span><span class="sxs-lookup"><span data-stu-id="87c6c-106">[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) is a relational database service based on the open source [MySQL](https://www.mysql.com/) Server engine.</span></span> 

<span data-ttu-id="87c6c-107">Azure Database for MySQL の概要については、「[Java を使用した接続とデータの照会](/azure/mysql/connect-java)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87c6c-107">To get started with Azure Database for MySQL, see [Use Java to connect and query data](/azure/mysql/connect-java).</span></span>

## <a name="client-jbdc-driver"></a><span data-ttu-id="87c6c-108">クライアント JDBC ドライバー</span><span class="sxs-lookup"><span data-stu-id="87c6c-108">Client JBDC driver</span></span>

<span data-ttu-id="87c6c-109">Azure Database for MySQL には、アプリケーションからオープン ソースの [MySQL JDBC ドライバー](https://dev.mysql.com/downloads/connector/j/)を使用して接続します。</span><span class="sxs-lookup"><span data-stu-id="87c6c-109">Connect to Azure Database for MySQL from your applications using the open-source [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).</span></span> <span data-ttu-id="87c6c-110">[Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使用してデータベースに直接接続できるほか、JDBC 経由でデータベースと対話するデータ アクセス フレームワーク ([Hibernate](http://hibernate.org/)) を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="87c6c-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect to the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="87c6c-111">プロジェクトでクライアント JDBC ドライバーを使用するためには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="87c6c-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="87c6c-112">例</span><span class="sxs-lookup"><span data-stu-id="87c6c-112">Example</span></span>

<span data-ttu-id="87c6c-113">JDBC を使用して Azure Database for MySQL に接続し、sales テーブル内の全レコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="87c6c-113">Connect to Azure Database for MySQL using JDBC and select all records in the sales table.</span></span> <span data-ttu-id="87c6c-114">データベースの JDBC 接続文字列は、Azure Portal から取得できます。</span><span class="sxs-lookup"><span data-stu-id="87c6c-114">You can get the JDBC connection string for the database from the Azure Portal.</span></span>

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a><span data-ttu-id="87c6c-115">サンプル</span><span class="sxs-lookup"><span data-stu-id="87c6c-115">Samples</span></span>

<span data-ttu-id="87c6c-116">[Java と MySQL Web アプリを構築する](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span><span class="sxs-lookup"><span data-stu-id="87c6c-116">[Build a Java and MySQL web app](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span></span>  
[<span data-ttu-id="87c6c-117">Azure CLI を使用して MySQL データベースを設計する</span><span class="sxs-lookup"><span data-stu-id="87c6c-117">Design a MySQL database using the Azure CLI</span></span>](/azure/mysql/tutorial-design-database-using-cli)   

<span data-ttu-id="87c6c-118">アプリで利用できる [Azure Database for MySQL のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="87c6c-118">Explore more [sample Java code for Azure Database for MySQL](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql) you can use in your apps.</span></span>
