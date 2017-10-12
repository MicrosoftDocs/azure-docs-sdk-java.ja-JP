---
title: Azure SQL Database Libraries for Java
description: "Azure SQL Database に接続するには、JDBC ドライバーまたは管理 Azure SQL Database インスタンスと Management API を使用します。"
keywords: "Azure, Java, SDK, API, SQL, データベース , JDBC"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/05/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: sql-database
ms.openlocfilehash: 3ae4015ae57e5eac4dafb30f4a42881986501853
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2017
---
# <a name="azure-sql-database-libraries-for-java"></a>Azure SQL Database Libraries for Java

## <a name="overview"></a>概要

[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) は、テーブル データ、JSON データ、空間データ、XML データに対応した Microsoft SQL Server エンジンを使用するリレーショナル データベース サービスです。 

Azure SQL Database の概要については、「[Azure SQL Database: Java を使用して Azure SQL Database に照会する](/azure/sql-database/sql-database-connect-query-java)」を参照してください。

## <a name="client-jdbc-driver"></a>クライアント JDBC ドライバー

アプリケーションから Azure SQL Database に接続するには、[SQL Database JDBC ドライバー](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)を使用します。 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使用してデータベースに直接接続できるほか、JDBC 経由でデータベースと対話するデータ アクセス フレームワーク ([Hibernate](http://hibernate.org/)) を使用することができます。

プロジェクトでクライアント JDBC ドライバーを使用するためには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。


```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```   

### <a name="example"></a>例

JDBC を使用し、SQL データベースに接続してテーブル内のすべてのレコードを選択します。

```java
String connectionString = "jdbc:sqlserver://fabrikam.database.windows.net:1433;database=fiber;user=raisa;password=testpass;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
try {
    Connection conn = DriverManager.getConnection(connectionString);
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery("SELECT * FROM SALES");
}  
```

## <a name="management-api"></a>Management API

Management API を使用すると、ご利用のサブスクリプションの Azure SQL Database リソースを作成したり管理したりすることができます。   

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-sql</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/sql/managementapi)

### <a name="example"></a>例

SQL Database リソースを作成し、ファイアウォール規則を使って、特定の範囲の IP アドレスにアクセスを制限します。

```java
SqlServer sqlServer = azure.sqlServers().define(sqlDbName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(resourceGroupName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    .withNewFirewallRule("172.16.0.0", "172.31.255.255")
                    .create();
```

## <a name="samples"></a>サンプル

[!INCLUDE [java-sql-samples](../docs-ref-conceptual/includes/sql.md)]

アプリから利用できる [Azure SQL Database のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL)を探しましょう。