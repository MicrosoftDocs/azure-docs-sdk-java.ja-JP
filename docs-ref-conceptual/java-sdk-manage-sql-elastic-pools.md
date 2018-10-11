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
ms.openlocfilehash: 9ec0cf3083b8659fa850b798ca0ecf18b2757234
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893573"
---
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a><span data-ttu-id="59cb2-103">Java アプリケーションからエラスティック プールの Azure SQL データベースを管理する</span><span class="sxs-lookup"><span data-stu-id="59cb2-103">Manage Azure SQL databases in elastic pools from your Java applications</span></span>

<span data-ttu-id="59cb2-104">[このサンプル](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)では、1 つのプランで複数のデータベースのリソースを管理したりスケーリングしたりできる[エラスティック プール](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)を使って SQL データベース サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="59cb2-104">[This sample](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool) creates a SQL database server with an [elastic pool](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to manage and scale resources for mulitple databases in a single plan.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="59cb2-105">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="59cb2-105">Run the sample</span></span>

<span data-ttu-id="59cb2-106">[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="59cb2-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="59cb2-107">次に、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="59cb2-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

[<span data-ttu-id="59cb2-108">GitHub で完全なコード サンプルを見る</span><span class="sxs-lookup"><span data-stu-id="59cb2-108">View the complete code sample on GitHub</span></span>](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)

## <a name="authenticate-with-azure"></a><span data-ttu-id="59cb2-109">Azure での認証</span><span class="sxs-lookup"><span data-stu-id="59cb2-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a><span data-ttu-id="59cb2-110">エラスティック プールを使用した SQL Database サーバーの作成</span><span class="sxs-lookup"><span data-stu-id="59cb2-110">Create a SQL database server with an elastic pool</span></span>

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

<span data-ttu-id="59cb2-111">現在のエディションの値については、[ElasticPoolEditions クラスのリファレンス](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59cb2-111">See the [ElasticPoolEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) for current edition values.</span></span> <span data-ttu-id="59cb2-112">エディションごとのリソース特性を比較するには、[SQL データベースのエラスティック プールに関するドキュメント](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59cb2-112">Review the [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to compare edition resource characteristics.</span></span> 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a><span data-ttu-id="59cb2-113">エラスティック プールのデータベース トランザクション ユニット (DTU) 設定の変更</span><span class="sxs-lookup"><span data-stu-id="59cb2-113">Change Database Transaction Unit (DTU) settings in an elastic pool</span></span>

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

<span data-ttu-id="59cb2-114">SQL データベースへのリソース割り当ての詳細については、[DTU と eDTU のドキュメント](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59cb2-114">Review the [DTUs and eDTUs documentation](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu) to learn more about allocating resources to SQL databases.</span></span>

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a><span data-ttu-id="59cb2-115">新しいデータベースの作成とエラスティック プールへの追加</span><span class="sxs-lookup"><span data-stu-id="59cb2-115">Create a new database and add it to an elastic pool</span></span>

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

<span data-ttu-id="59cb2-116">この API は、先頭のステートメントで、[S0 レベル](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)の `anotherDatabase` を作成しています。</span><span class="sxs-lookup"><span data-stu-id="59cb2-116">The API creates `anotherDatabase` at [S0 tier](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) in the first statement.</span></span> <span data-ttu-id="59cb2-117">`anotherDatabase` をエラスティック プールに移動すると、そのプールの設定に基づいて、データベース リソースが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-117">Moving `anotherDatabase` to the elastic pool assigns the database resources based on the pool settings.</span></span>

## <a name="remove-a-database-from-an-elastic-pool"></a><span data-ttu-id="59cb2-118">エラスティック プールからのデータベースの削除</span><span class="sxs-lookup"><span data-stu-id="59cb2-118">Remove a database from an elastic pool</span></span>
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

<span data-ttu-id="59cb2-119">`withEdition()` に渡す値については、[DatabaseEditions クラスのリファレンス](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59cb2-119">See the [DatabaseEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) for values to pass to `withEdition()`.</span></span>

## <a name="list-current-database-activities-in-an-elastic-pool"></a><span data-ttu-id="59cb2-120">エラスティック プールにおける現在のデータベース アクティビティの列挙</span><span class="sxs-lookup"><span data-stu-id="59cb2-120">List current database activities in an elastic pool</span></span>
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

<span data-ttu-id="59cb2-121">データベース アクティビティには、エラスティック プール内のデータベースの追加、削除、更新が含まれます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-121">Database activities include moving a database in or out of an elastic pool and updating databases in an elastic pool.</span></span>


## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="59cb2-122">エラスティック プール内のデータベースの一覧表示</span><span class="sxs-lookup"><span data-stu-id="59cb2-122">List databases in an elastic pool</span></span>
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

<span data-ttu-id="59cb2-123">データベースを照会するための各種メソッドについて詳しくは、[com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59cb2-123">Review the methods in [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) to query the databases in more detail.</span></span>

## <a name="delete-an-elastic-pool"></a><span data-ttu-id="59cb2-124">エラスティック プールの削除</span><span class="sxs-lookup"><span data-stu-id="59cb2-124">Delete an elastic pool</span></span>
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

<span data-ttu-id="59cb2-125">エラスティック プールを削除するには、あらかじめそのエラスティック プールを空にしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="59cb2-125">The elastic pool must be empty before deleting it.</span></span>

## <a name="sample-code-summary"></a><span data-ttu-id="59cb2-126">サンプル コードの概要</span><span class="sxs-lookup"><span data-stu-id="59cb2-126">Sample code summary.</span></span>

<span data-ttu-id="59cb2-127">このサンプルは、単一のエラスティック プールで管理される 2 つのデータベースを含んだ SQL サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="59cb2-127">The sample creates a SQL server with two databases managed in a single elasic pool.</span></span> <span data-ttu-id="59cb2-128">エラスティック プール リソースの制限を更新したうえで、別のデータベースをプールに追加しています。</span><span class="sxs-lookup"><span data-stu-id="59cb2-128">The elastic pool resource limits are updated, then additional databases are added to the pool.</span></span> <span data-ttu-id="59cb2-129">その後、エラスティック プールの構成と状態を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-129">The code then reads the elastic pool's configuration and state.</span></span> 

<span data-ttu-id="59cb2-130">このサンプルで作成したリソースは、終了前にすべて削除されます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-130">The sample deletes all resources it created before exiting.</span></span>

| <span data-ttu-id="59cb2-131">サンプルで使われているクラス</span><span class="sxs-lookup"><span data-stu-id="59cb2-131">Class used in sample</span></span> | <span data-ttu-id="59cb2-132">メモ</span><span class="sxs-lookup"><span data-stu-id="59cb2-132">Notes</span></span> |
|-------|-------|
| [<span data-ttu-id="59cb2-133">SqlServer</span><span class="sxs-lookup"><span data-stu-id="59cb2-133">SqlServer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_server) | <span data-ttu-id="59cb2-134">Azure における SQL DB サーバー。`azure.sqlServers().define()...create()` という fluent チェーンによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-134">SQL DB server in Azure created by `azure.sqlServers().define()...create()` fluent chain.</span></span> <span data-ttu-id="59cb2-135">エラスティック プールやデータベースを作成したり操作したりするためのメソッドが備わっています。</span><span class="sxs-lookup"><span data-stu-id="59cb2-135">Provides methods to create and work with elastic pools and databases.</span></span> 
| [<span data-ttu-id="59cb2-136">SqlDatabase</span><span class="sxs-lookup"><span data-stu-id="59cb2-136">SqlDatabase</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) | <span data-ttu-id="59cb2-137">SQL データベースを表すクライアント側オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="59cb2-137">Client side object representing a SQL database.</span></span> <span data-ttu-id="59cb2-138">`sqlServer().define()...create()` を使って作成します。</span><span class="sxs-lookup"><span data-stu-id="59cb2-138">Created through `sqlServer().define()...create()`.</span></span> 
| [<span data-ttu-id="59cb2-139">DatabaseEditions</span><span class="sxs-lookup"><span data-stu-id="59cb2-139">DatabaseEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) | <span data-ttu-id="59cb2-140">エラスティック プールの外にデータベースを作成したり、エラスティック プールからデータベースを移動したりする際に、データベース リソースを設定するための静的な定数フィールド。</span><span class="sxs-lookup"><span data-stu-id="59cb2-140">Constant static fields used to set database resources when creating a database outside of an elastic pool or when moving a database out of an elastic pool</span></span>  
| [<span data-ttu-id="59cb2-141">SqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="59cb2-141">SqlElasticPool</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_elastic_pool) | <span data-ttu-id="59cb2-142">Azure に SqlServer を作成した fluent チェーンの `withNewElasticPool()` セクションから作成されます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-142">Created from the `withNewElasticPool()` section of the fluent chain that created the SqlServer in Azure.</span></span> <span data-ttu-id="59cb2-143">エラスティック プールで実行されるデータベース (またはエラスティック プールそのもの) のリソース制限を設定するためのメソッドが備わっています。</span><span class="sxs-lookup"><span data-stu-id="59cb2-143">Provides methods to set resource limits for databases running in the elastic pool and for the elastic pool itself.</span></span> 
| [<span data-ttu-id="59cb2-144">ElasticPoolEditions</span><span class="sxs-lookup"><span data-stu-id="59cb2-144">ElasticPoolEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) | <span data-ttu-id="59cb2-145">エラスティック プールで利用可能なリソースを定義する定数フィールドのクラス。</span><span class="sxs-lookup"><span data-stu-id="59cb2-145">Class of constant fields defining the resources available to an elastic pool.</span></span> <span data-ttu-id="59cb2-146">詳細については、[SQL データベースのエラスティック プールに関するドキュメント](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59cb2-146">See [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) for tier details.</span></span> 
| [<span data-ttu-id="59cb2-147">ElasticPoolDatabaseActivity</span><span class="sxs-lookup"><span data-stu-id="59cb2-147">ElasticPoolDatabaseActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_database_activity) | <span data-ttu-id="59cb2-148">`SqlElasticPool.listDatabaseActivities()` から取得されます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-148">Retreived from `SqlElasticPool.listDatabaseActivities()`.</span></span> <span data-ttu-id="59cb2-149">この型のオブジェクトはそれぞれ、エラスティック プール内のデータベースに対して実行されるアクティビティを表します。</span><span class="sxs-lookup"><span data-stu-id="59cb2-149">Each object of this type represents an activity performed on a database in the elastic pool.</span></span>
| [<span data-ttu-id="59cb2-150">ElasticPoolActivity</span><span class="sxs-lookup"><span data-stu-id="59cb2-150">ElasticPoolActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_activity) | <span data-ttu-id="59cb2-151">`SqlElasticPool.listActivities()` からリスト形式で取得されます。</span><span class="sxs-lookup"><span data-stu-id="59cb2-151">Retrieved in a List from `SqlElasticPool.listActivities()`.</span></span> <span data-ttu-id="59cb2-152">リストに含まれる各オブジェクトは、(エラスティック プール内のデータベースではなく) エラスティック プールに対して実行されるアクティビティを表します。</span><span class="sxs-lookup"><span data-stu-id="59cb2-152">Each of object in the list represents an activity performed on the elastic pool (not the databases in the elastic pool).</span></span>

## <a name="next-steps"></a><span data-ttu-id="59cb2-153">次の手順</span><span class="sxs-lookup"><span data-stu-id="59cb2-153">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]