---
title: Azure Database for MySQL Libraries for Java
description: Azure Database for MySQL 用 Java クライアント ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, SQL, データベース, PostGres, MySQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.devlang: java
ms.service: mysql
ms.openlocfilehash: f3c0b84a8c6577e5a844c4084b3d9cfaf3a52323
ms.sourcegitcommit: 1b22376e4ceb3d2f2734c8fc80823a44cc5fe8fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2018
ms.locfileid: "42703341"
---
# <a name="azure-database-for-mysql-libraries-for-java"></a>Azure Database for MySQL Libraries for Java

## <a name="overview"></a>概要

[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) は、オープン ソースの [MySQL](https://www.mysql.com/) Server エンジンに基づいたリレーショナル データベース サービスです。 

Azure Database for MySQL の概要については、「[Java を使用した接続とデータの照会](/azure/mysql/connect-java)」を参照してください。

## <a name="client-jbdc-driver"></a>クライアント JDBC ドライバー

Azure Database for MySQL には、アプリケーションからオープン ソースの [MySQL JDBC ドライバー](https://dev.mysql.com/downloads/connector/j/)を使用して接続します。 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使用してデータベースに直接接続できるほか、JDBC 経由でデータベースと対話するデータ アクセス フレームワーク ([Hibernate](http://hibernate.org/)) を使用することができます。

プロジェクトでクライアント JDBC ドライバーを使用するためには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a>例

JDBC を使用して Azure Database for MySQL に接続し、sales テーブル内の全レコードを選択します。 データベースの JDBC 接続文字列は、Azure Portal から取得できます。

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a>サンプル

[Java と MySQL Web アプリを構築する](/azure/app-service-web/app-service-web-tutorial-java-mysql)   
[Azure CLI を使用して MySQL データベースを設計する](/azure/mysql/tutorial-design-database-using-cli)   

アプリで利用できる [Azure Database for MySQL のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql)を探しましょう。
