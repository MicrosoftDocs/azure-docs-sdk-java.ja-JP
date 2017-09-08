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
# <a name="azure-database-for-postgresql-libraries-for-java"></a>Azure Database for PostgreSQL Libraries for Java

## <a name="overview"></a>概要

[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) は、オープン ソースの [PostgreSQL](https://www.postgresql.org/) データベース エンジンのコミュニティ バージョンに基づいて開発者向けに構築された、Azure のリレーショナル データベース サービスです。

Azure Database for PostgreSQL の概要については、「[Java を使用した接続とデータの照会](/azure/postgresql/connect-java)」を参照してください。

## <a name="client-jdbc-driver"></a>クライアント JDBC ドライバー

Azure Database for PostgreSQL には、アプリケーションからオープン ソースの [PostgreSQL JDBC ドライバー](https://jdbc.postgresql.org/)を使用して接続します。 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使用してデータベースに直接接続できるほか、JDBC 経由でデータベースと対話するデータ アクセス フレームワーク ([Hibernate](http://hibernate.org/)) を使用することができます。

プロジェクトでクライアント JDBC ドライバーを使用するためには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.1.1</version>
</dependency>
```   

## <a name="example"></a>例

JDBC を使用して Azure Database for PostgreSQL に接続し、sales テーブル内の全レコードを選択します。 データベースの JDBC 接続文字列は、Azure Portal から取得できます。

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

## <a name="samples"></a>サンプル

[Azure CLI を使用して PostgreSQL データベースを設計する](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-cli) 

アプリで利用できる [Azure Database for PostgreSQL のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres)を探しましょう。
