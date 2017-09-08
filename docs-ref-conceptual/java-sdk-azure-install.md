---
title: "Azure for Java 開発者向け Microsoft ドキュメント"
description: "Azure 向けの Java SDK と API リファレンス"
keywords: "Azure Java, Azure Java API リファレンス, Azure Java クラス ライブラリ, Azure SDK"
author: routlaw
manager: douge
ms.assetid: 7b92e776-959b-4632-8b1d-047ce1417616
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: 10073a1b2250a37347128dd9c8faf1375b2ab6ae
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="80214-104">Java 用 Azure ライブラリ</span><span class="sxs-lookup"><span data-stu-id="80214-104">Azure libraries for Java</span></span>

<span data-ttu-id="80214-105">Azure のライブラリを使用すると、Java アプリでネイティブ インターフェイスを使用して Azure サービスを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="80214-105">Azure libraries help you consume Azure services in your Java apps using native interfaces.</span></span> <span data-ttu-id="80214-106">それぞれのライブラリは独立しており、他のライブラリと切り離して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="80214-106">Each library is independent and can be used separately from the others another.</span></span>

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [<span data-ttu-id="80214-107">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="80214-107">Azure Storage</span></span>](#azure-storage) | [<span data-ttu-id="80214-108">SQL Database</span><span class="sxs-lookup"><span data-stu-id="80214-108">SQL Database</span></span>](#sql-database)  | [<span data-ttu-id="80214-109">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="80214-109">Redis Cache</span></span>](#redis-cache)   | [<span data-ttu-id="80214-110">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="80214-110">DocumentDB</span></span>](#documentdb) |
| [<span data-ttu-id="80214-111">Service Bus</span><span class="sxs-lookup"><span data-stu-id="80214-111">Service Bus</span></span>](#servicebus)  | [<span data-ttu-id="80214-112">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="80214-112">Azure Active Directory</span></span>](#azuread) | [<span data-ttu-id="80214-113">Key Vault</span><span class="sxs-lookup"><span data-stu-id="80214-113">Key Vault</span></span>](#keyvault)  | [<span data-ttu-id="80214-114">イベント ハブ</span><span class="sxs-lookup"><span data-stu-id="80214-114">Event Hub</span></span>](#eventhub)
| [<span data-ttu-id="80214-115">IoT サービス</span><span class="sxs-lookup"><span data-stu-id="80214-115">IoT Service</span></span>](#iotservice) | [<span data-ttu-id="80214-116">IoT デバイス</span><span class="sxs-lookup"><span data-stu-id="80214-116">IoT Device</span></span>](#iotdevice) | [<span data-ttu-id="80214-117">Data Lake</span><span class="sxs-lookup"><span data-stu-id="80214-117">Data Lake</span></span>](#datalake)  | [<span data-ttu-id="80214-118">AppInsights</span><span class="sxs-lookup"><span data-stu-id="80214-118">AppInsights</span></span>](#appinsights) | 
| [<span data-ttu-id="80214-119">Batch</span><span class="sxs-lookup"><span data-stu-id="80214-119">Batch</span></span>](#batch) | [<span data-ttu-id="80214-120">Azure のリソースを管理する</span><span class="sxs-lookup"><span data-stu-id="80214-120">Manage Azure resources</span></span>](#management) |

## <a name="install-with-maven"></a><span data-ttu-id="80214-121">Maven でのインストール</span><span class="sxs-lookup"><span data-stu-id="80214-121">Install with Maven</span></span>

<span data-ttu-id="80214-122">依存関係のエントリを `pom.xml` に追加して、[Maven](https://maven.apache.org) プロジェクトにライブラリをインポートします。</span><span class="sxs-lookup"><span data-stu-id="80214-122">Add a dependency entry in your `pom.xml` to import a library into your [Maven](https://maven.apache.org) project.</span></span>

<span data-ttu-id="80214-123">たとえば、最新バージョンの [Azure Management Libraries for Java](#management) を含めるには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="80214-123">For example, to include the latest version of the [Azure management libraries for Java](#management):</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

<span data-ttu-id="80214-124">その他の Java ビルド ツール (Gradle など) もサポートされますが、この記事ではインストール手順を紹介していません。</span><span class="sxs-lookup"><span data-stu-id="80214-124">Other Java build tools like Gradle are supported but the install steps are not provided in this article.</span></span> <span data-ttu-id="80214-125">Maven インポートの実行方法については、ご利用のビルド ツールのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="80214-125">Review the documentation for your build tool on how to consume Maven imports.</span></span>

## <a name="azure-service-libraries"></a><span data-ttu-id="80214-126">Azure サービス ライブラリ</span><span class="sxs-lookup"><span data-stu-id="80214-126">Azure service libraries</span></span>

<span data-ttu-id="80214-127">これらのライブラリを使ってアプリに機能を追加するには、Azure サービスを統合します。</span><span class="sxs-lookup"><span data-stu-id="80214-127">Integrate Azure services to add functionality to your apps using these libraries.</span></span> <span data-ttu-id="80214-128">Azure サービスを使ってアプリをビルドする方法については、[Java デベロッパー センター](https://azure.microsoft.com/develop/java)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="80214-128">Learn more about building apps with Azure services at the [Java developer center](https://azure.microsoft.com/develop/java).</span></span>

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[<span data-ttu-id="80214-129">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="80214-129">Azure Storage</span></span>](/azure/storage/storage-introduction)  

<span data-ttu-id="80214-130">アプリケーションのデータ ストレージとメッセージング。</span><span class="sxs-lookup"><span data-stu-id="80214-130">Data storage and messaging for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

[<span data-ttu-id="80214-131">サンプル](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [リファレンス](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-131">Samples](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [Reference](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[<span data-ttu-id="80214-132">SQL Database</span><span class="sxs-lookup"><span data-stu-id="80214-132">SQL Database</span></span>](/azure/sql-database/sql-database-technical-overview)

<span data-ttu-id="80214-133">Azure SQL Database 用 JDBC ドライバー。</span><span class="sxs-lookup"><span data-stu-id="80214-133">JDBC driver for Azure SQL Database.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

[<span data-ttu-id="80214-134">サンプル](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [リファレンス](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-134">Samples](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [Reference](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [Release Notes</span></span>](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[<span data-ttu-id="80214-135">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="80214-135">Redis Cache</span></span>](https://azure.microsoft.com/services/cache/)

<span data-ttu-id="80214-136">待ち時間の短いハイパフォーマンスのキー/値ストア。</span><span class="sxs-lookup"><span data-stu-id="80214-136">Low-latency, high-performance key-value store.</span></span>

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

[<span data-ttu-id="80214-137">サンプル](/azure/redis-cache/cache-java-get-started) | [リファレンス](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-137">Samples](/azure/redis-cache/cache-java-get-started) | [Reference](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [Release Notes</span></span>](https://github.com/xetorthio/jedis/releases)  

<a name="documentdb"></a>

### <a name="cosmos-dbazuredocumentdbdocumentdb-introduction"></a>[<span data-ttu-id="80214-138">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="80214-138">Cosmos DB</span></span>](/azure/documentdb/documentdb-introduction)

<span data-ttu-id="80214-139">SQL または JavaScript のクエリ構文と JSON ドキュメントを使ったスケーラブルな NoSQL データベース。</span><span class="sxs-lookup"><span data-stu-id="80214-139">Scalable NoSQL database with JSON documents and a SQL or JavaScript query syntax.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

[<span data-ttu-id="80214-140">サンプル](/azure/documentdb/documentdb-java-application) | [リファレンス](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-140">Samples](/azure/documentdb/documentdb-java-application) | [Reference](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [Release Notes</span></span>](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="80214-141">ServiceBus</span><span class="sxs-lookup"><span data-stu-id="80214-141">ServiceBus</span></span>](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 <span data-ttu-id="80214-142">Service Bus は、エンタープライズ クラスのトランザクション メッセージング プラットフォーム サービスです。</span><span class="sxs-lookup"><span data-stu-id="80214-142">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 [<span data-ttu-id="80214-143">サンプル](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [リファレンス](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-143">Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-service-bus-java)   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[<span data-ttu-id="80214-144">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="80214-144">Azure Active Directory</span></span>](/azure/active-directory/active-directory-whatis)   

<span data-ttu-id="80214-145">アプリケーションのための ID 管理と安全なサインイン。</span><span class="sxs-lookup"><span data-stu-id="80214-145">Identity management and secure sign-in for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
[<span data-ttu-id="80214-146">サンプル](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [リファレンス](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-146">Samples](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [Reference](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [Release Notes</span></span>](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[<span data-ttu-id="80214-147">Key Vault</span><span class="sxs-lookup"><span data-stu-id="80214-147">Key Vault</span></span>](/azure/key-vault) 

<span data-ttu-id="80214-148">アプリケーションからキーとシークレットに安全にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="80214-148">Safely access keys and secrets from your applications.</span></span> 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

[<span data-ttu-id="80214-149">サンプル](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [リファレンス](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-149">Samples](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [Reference](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [Release Notes</span></span>](https://github.com/Azure/azure-keyvault-java) 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[<span data-ttu-id="80214-150">イベント ハブ</span><span class="sxs-lookup"><span data-stu-id="80214-150">Event Hub</span></span>](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
<span data-ttu-id="80214-151">インストルメンテーションまたは IoT のシナリオに対応する高スループットのイベントとテレメトリ処理。</span><span class="sxs-lookup"><span data-stu-id="80214-151">High-throughput event and telemetry handling for your instrumentation or IoT scenarios.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

[<span data-ttu-id="80214-152">サンプル](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [リファレンス](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-152">Samples](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [Reference](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-event-hubs-java)

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[<span data-ttu-id="80214-153">IoT サービス</span><span class="sxs-lookup"><span data-stu-id="80214-153">IoT Service</span></span>](/azure/iot-hub/)

<span data-ttu-id="80214-154">IoT ハブに登録済みのデバイスとの間で、ID を管理したり、メッセージを送信したり、フィードバックを取得したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="80214-154">Manage identities, send messages, and get feedback from devices registered with your IoT hub.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
[<span data-ttu-id="80214-155">サンプル](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [リファレンス](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-155">Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes</span></span>](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[<span data-ttu-id="80214-156">IoT デバイス</span><span class="sxs-lookup"><span data-stu-id="80214-156">IoT Device</span></span>](/azure/iot-hub/iot-hub-devguide)

<span data-ttu-id="80214-157">デバイスから IoT ハブにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="80214-157">Send a message to an IoT hub from your device.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

[<span data-ttu-id="80214-158">サンプル](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [リファレンス](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-158">Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes</span></span>](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[<span data-ttu-id="80214-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="80214-159">Data Lake Store</span></span>](/azure/data-lake-store/data-lake-store-overview)   
   
<span data-ttu-id="80214-160">あらゆるサイズのデータをキャプチャし、1 つの場所にまとめて分析に利用することができます。</span><span class="sxs-lookup"><span data-stu-id="80214-160">Capture data of any size and shape into a single location for performing analytics.</span></span>    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

[<span data-ttu-id="80214-161">サンプル](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [リファレンス](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-161">Samples](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [Reference](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [Release Notes</span></span>](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[<span data-ttu-id="80214-162">AppInsights</span><span class="sxs-lookup"><span data-stu-id="80214-162">AppInsights</span></span>](/azure/application-insights/app-insights-overview)

<span data-ttu-id="80214-163">使用状況を追跡したり、テレメトリを追加したり、Web アプリを監視したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="80214-163">Track usage, add telemetry, and monitor your web apps.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

[<span data-ttu-id="80214-164">サンプル](/azure/application-insights/app-insights-java-get-started) | [リファレンス](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-164">Samples](/azure/application-insights/app-insights-java-get-started) | [Reference](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [Release Notes</span></span>](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)

<a name="batch"></a>

### <a name="batchazurebatch"></a>[<span data-ttu-id="80214-165">Batch</span><span class="sxs-lookup"><span data-stu-id="80214-165">Batch</span></span>](/azure/batch)

<span data-ttu-id="80214-166">大規模な並列コンピューティングやハイパフォーマンス コンピューティングのアプリケーションをクラウドで効率的に実行できます。</span><span class="sxs-lookup"><span data-stu-id="80214-166">Run large-scale parallel and high-performance computing applications efficiently in the cloud.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

[<span data-ttu-id="80214-167">サンプル](https://github.com/azure/azure-batch-samples) | [リファレンス](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-167">Samples](https://github.com/azure/azure-batch-samples) | [Reference](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [Release Notes</span></span>](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)

<a name="management"></a> 

## <a name="manage-azure-resources"></a><span data-ttu-id="80214-168">Azure のリソースを管理する</span><span class="sxs-lookup"><span data-stu-id="80214-168">Manage Azure resources</span></span>

<span data-ttu-id="80214-169">Azure リソースの作成、更新、削除をアプリケーション コードから行います。</span><span class="sxs-lookup"><span data-stu-id="80214-169">Create, update, and delete Azure resources from your application code.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

[<span data-ttu-id="80214-170">サンプル](https://github.com/Azure/azure-sdk-for-java#sample-code) | [リファレンス](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-170">Samples](https://github.com/Azure/azure-sdk-for-java#sample-code) | [Reference](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [Release Notes</span></span>](java-sdk-azure-release-notes.md)

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomen-usazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="80214-171">ServiceBus</span><span class="sxs-lookup"><span data-stu-id="80214-171">ServiceBus</span></span>](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) 
   
<span data-ttu-id="80214-172">Service Bus は、エンタープライズ クラスのトランザクション メッセージング プラットフォーム サービスです。</span><span class="sxs-lookup"><span data-stu-id="80214-172">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

[<span data-ttu-id="80214-173">サンプル](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [リファレンス](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [リリース ノート</span><span class="sxs-lookup"><span data-stu-id="80214-173">Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes</span></span>](https://github.com/Azure/azure-service-bus-java)

