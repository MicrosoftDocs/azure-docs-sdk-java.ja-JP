---
title: SQL データベースのエラスティック プールを Java で管理する | Microsoft Docs
description: Azure SDK for Java を使って Azure SQL データベースを作成して構成するためのサンプル コード
author: rloutlaw
manager: douge
ms.assetid: 9b461de8-46bc-4650-8e9e-59531f4e2a53
ms.topic: article
ms.service: Azure
ms.devlang: java
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 59d2601ef21b91a364887fe0548cb95023c79b5e
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592687"
---
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a>Java アプリケーションからエラスティック プールの Azure SQL データベースを管理する

[このサンプル](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)では、1 つのプランで複数のデータベースのリソースを管理したりスケーリングしたりできる[エラスティック プール](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)を使って SQL データベース サーバーを作成します。

## <a name="run-the-sample"></a>サンプルを実行する

[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。 次に、以下を実行します。

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

[GitHub で完全なコード サンプルを見る](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)

## <a name="authenticate-with-azure"></a>Azure での認証

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a>エラスティック プールを使用した SQL Database サーバーの作成

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    // Use ElasticPoolEditions.STANDARD as the edition
                    // and create two databases
                    .withNewElasticPool(elasticPoolName, ElasticPoolEditions.STANDARD, 
                        database1Name, database2Name)
                    .create();
```

現在のエディションの値については、[ElasticPoolEditions クラスのリファレンス](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions)を参照してください。 エディションごとのリソース特性を比較するには、[SQL データベースのエラスティック プールに関するドキュメント](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)を参照してください。 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a>エラスティック プールのデータベース トランザクション ユニット (DTU) 設定の変更

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

SQL データベースへのリソース割り当ての詳細については、[DTU と eDTU のドキュメント](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)を参照してください。

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a>新しいデータベースの作成とエラスティック プールへの追加

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

この API は、先頭のステートメントで、[S0 レベル](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)の `anotherDatabase` を作成しています。 `anotherDatabase` をエラスティック プールに移動すると、そのプールの設定に基づいて、データベース リソースが割り当てられます。

## <a name="remove-a-database-from-an-elastic-pool"></a>エラスティック プールからのデータベースの削除
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

`withEdition()` に渡す値については、[DatabaseEditions クラスのリファレンス](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions)を参照してください。

## <a name="list-current-database-activities-in-an-elastic-pool"></a>エラスティック プールにおける現在のデータベース アクティビティの列挙
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

データベース アクティビティには、エラスティック プール内のデータベースの追加、削除、更新が含まれます。


## <a name="list-databases-in-an-elastic-pool"></a>エラスティック プール内のデータベースの一覧表示
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

データベースを照会するための各種メソッドについて詳しくは、[com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) を参照してください。

## <a name="delete-an-elastic-pool"></a>エラスティック プールの削除
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

エラスティック プールを削除するには、あらかじめそのエラスティック プールを空にしておく必要があります。

## <a name="sample-code-summary"></a>サンプル コードの概要

このサンプルは、単一のエラスティック プールで管理される 2 つのデータベースを含んだ SQL サーバーを作成します。 エラスティック プール リソースの制限を更新したうえで、別のデータベースをプールに追加しています。 その後、エラスティック プールの構成と状態を読み込みます。 

このサンプルで作成したリソースは、終了前にすべて削除されます。

| サンプルで使われているクラス | メモ |
|-------|-------|
| [SqlServer](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_server) | Azure における SQL DB サーバー。`azure.sqlServers().define()...create()` という fluent チェーンによって作成されます。 エラスティック プールやデータベースを作成したり操作したりするためのメソッドが備わっています。 
| [SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) | SQL データベースを表すクライアント側オブジェクト。 `sqlServer().define()...create()` を使って作成します。 
| [DatabaseEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) | エラスティック プールの外にデータベースを作成したり、エラスティック プールからデータベースを移動したりする際に、データベース リソースを設定するための静的な定数フィールド。  
| [SqlElasticPool](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_elastic_pool) | Azure に SqlServer を作成した fluent チェーンの `withNewElasticPool()` セクションから作成されます。 エラスティック プールで実行されるデータベース (またはエラスティック プールそのもの) のリソース制限を設定するためのメソッドが備わっています。 
| [ElasticPoolEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) | エラスティック プールで利用可能なリソースを定義する定数フィールドのクラス。 詳細については、[SQL データベースのエラスティック プールに関するドキュメント](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)を参照してください。 
| [ElasticPoolDatabaseActivity](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_database_activity) | `SqlElasticPool.listDatabaseActivities()` から取得されます。 この型のオブジェクトはそれぞれ、エラスティック プール内のデータベースに対して実行されるアクティビティを表します。
| [ElasticPoolActivity](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_activity) | `SqlElasticPool.listActivities()` からリスト形式で取得されます。 リストに含まれる各オブジェクトは、(エラスティック プール内のデータベースではなく) エラスティック プールに対して実行されるアクティビティを表します。

## <a name="next-steps"></a>次の手順

[!INCLUDE [next-steps](includes/java-next-steps.md)]