---
title: Azure SQL Database Libraries for Java
description: Azure SQL Database に接続するには、JDBC ドライバーまたは管理 Azure SQL Database インスタンスと Management API を使用します。
keywords: Azure, Java, SDK, API, SQL, データベース , JDBC
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/05/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: sql-database
ms.openlocfilehash: 37f7d3caf10e6b709cee2452c63a543d49e0ead8
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893313"
---
# <a name="azure-sql-database-libraries-for-java"></a><span data-ttu-id="d9f2a-104">Azure SQL Database Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="d9f2a-104">Azure SQL Database libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d9f2a-105">概要</span><span class="sxs-lookup"><span data-stu-id="d9f2a-105">Overview</span></span>

<span data-ttu-id="d9f2a-106">[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) は、テーブル データ、JSON データ、空間データ、XML データに対応した Microsoft SQL Server エンジンを使用するリレーショナル データベース サービスです。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-106">[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) is a relational database service using the Microsoft SQL Server engine that supports table, JSON, spatial, and XML data.</span></span> 

<span data-ttu-id="d9f2a-107">Azure SQL Database の概要については、「[Azure SQL Database: Java を使用して Azure SQL Database に照会する](/azure/sql-database/sql-database-connect-query-java)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-107">To get started with Azure SQL Database, see [Azure SQL Database: Use Java to connect and query data](/azure/sql-database/sql-database-connect-query-java).</span></span>

## <a name="client-jdbc-driver"></a><span data-ttu-id="d9f2a-108">クライアント JDBC ドライバー</span><span class="sxs-lookup"><span data-stu-id="d9f2a-108">Client JDBC driver</span></span>

<span data-ttu-id="d9f2a-109">アプリケーションから Azure SQL Database に接続するには、[SQL Database JDBC ドライバー](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)を使用します。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-109">Connect to Azure SQL Database from your applications using the [SQL Database JDBC driver](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server).</span></span> <span data-ttu-id="d9f2a-110">[Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使用してデータベースに直接接続できるほか、JDBC 経由でデータベースと対話するデータ アクセス フレームワーク ([Hibernate](http://hibernate.org/)) を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect with the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="d9f2a-111">プロジェクトでクライアント JDBC ドライバーを使用するためには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="d9f2a-112">例</span><span class="sxs-lookup"><span data-stu-id="d9f2a-112">Example</span></span>

<span data-ttu-id="d9f2a-113">JDBC を使用し、SQL データベースに接続してテーブル内のすべてのレコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-113">Connect to SQL database and select all records in a table using JDBC.</span></span>

```java
String connectionString = "jdbc:sqlserver://fabrikam.database.windows.net:1433;database=fiber;user=raisa;password=testpass;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
try {
    Connection conn = DriverManager.getConnection(connectionString);
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery("SELECT * FROM SALES");
}  
```

## <a name="management-api"></a><span data-ttu-id="d9f2a-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="d9f2a-114">Management API</span></span>

<span data-ttu-id="d9f2a-115">Management API を使用すると、ご利用のサブスクリプションの Azure SQL Database リソースを作成したり管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-115">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span>   

<span data-ttu-id="d9f2a-116">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-sql</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d9f2a-117">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="d9f2a-117">Explore the Management APIs</span></span>](/java/api/overview/azure/sql/management)

### <a name="example"></a><span data-ttu-id="d9f2a-118">例</span><span class="sxs-lookup"><span data-stu-id="d9f2a-118">Example</span></span>

<span data-ttu-id="d9f2a-119">SQL Database リソースを作成し、ファイアウォール規則を使って、特定の範囲の IP アドレスにアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-119">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlDbName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(resourceGroupName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    .withNewFirewallRule("172.16.0.0", "172.31.255.255")
                    .create();
```

## <a name="samples"></a><span data-ttu-id="d9f2a-120">サンプル</span><span class="sxs-lookup"><span data-stu-id="d9f2a-120">Samples</span></span>

[!INCLUDE [java-sql-samples](../docs-ref-conceptual/includes/sql.md)]

<span data-ttu-id="d9f2a-121">アプリから利用できる [Azure SQL Database のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="d9f2a-121">Explore more [sample Java code for Azure SQL Database](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL) you can use in your apps.</span></span>