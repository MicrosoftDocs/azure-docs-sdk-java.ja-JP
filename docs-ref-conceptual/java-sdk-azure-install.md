---
title: Azure for Java 開発者向け Microsoft ドキュメント
description: Azure 向けの Java SDK と API リファレンス
keywords: Azure Java, Azure Java API リファレンス, Azure Java クラス ライブラリ, Azure SDK
author: routlaw
manager: douge
ms.assetid: 7b92e776-959b-4632-8b1d-047ce1417616
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: 5c8bb4b81080461285551573eefc0d76b47b2d3d
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892533"
---
# <a name="azure-libraries-for-java"></a>Java 用 Azure ライブラリ

Azure のライブラリを使用すると、Java アプリでネイティブ インターフェイスを使用して Azure サービスを実行することができます。 それぞれのライブラリは独立しており、他のライブラリと切り離して使用することができます。

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [Azure Storage](#azure-storage) | [SQL Database](#sql-database)  | [Redis Cache](#redis-cache)   | [Azure Cosmos DB](#cosmos-db) |
| [Service Bus](#servicebus)  | [Azure Active Directory](#azuread) | [Key Vault](#keyvault)  | [イベント ハブ](#eventhub)
| [IoT サービス](#iotservice) | [IoT デバイス](#iotdevice) | [Data Lake](#datalake)  | [AppInsights](#appinsights) | 
| [Batch](#batch) | [Azure のリソースを管理する](#management) |

## <a name="install-with-maven"></a>Maven でのインストール

依存関係のエントリを `pom.xml` に追加して、[Maven](https://maven.apache.org) プロジェクトにライブラリをインポートします。

たとえば、最新バージョンの [Azure Management Libraries for Java](#management) を含めるには、次のようにします。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

その他の Java ビルド ツール (Gradle など) もサポートされますが、この記事ではインストール手順を紹介していません。 Maven インポートの実行方法については、ご利用のビルド ツールのドキュメントを参照してください。

## <a name="azure-service-libraries"></a>Azure サービス ライブラリ

これらのライブラリを使ってアプリに機能を追加するには、Azure サービスを統合します。 Azure サービスを使ってアプリをビルドする方法については、[Java デベロッパー センター](https://azure.microsoft.com/develop/java)を参照してください。

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[Azure Storage](/azure/storage/storage-introduction)  

アプリケーションのデータ ストレージとメッセージング。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

[サンプル](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [リファレンス](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [リリース ノート](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[SQL Database](/azure/sql-database/sql-database-technical-overview)

Azure SQL Database 用 JDBC ドライバー。

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

[サンプル](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [リファレンス](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [リリース ノート](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[Redis Cache](https://azure.microsoft.com/services/cache/)

待ち時間の短いハイパフォーマンスのキー/値ストア。

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

[サンプル](/azure/redis-cache/cache-java-get-started) | [リファレンス](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [リリース ノート](https://github.com/xetorthio/jedis/releases)  

<a name="cosmos-db"></a>

### <a name="azure-cosmos-dbazurecosmos-dbintroduction"></a>[Azure Cosmos DB](/azure/cosmos-db/introduction)

SQL または JavaScript のクエリ構文と JSON ドキュメントを使ったスケーラブルな NoSQL データベース。   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

[サンプル](/azure/cosmos-db/sql-api-java-application) | [リファレンス](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [リリース ノート](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[ServiceBus](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 Service Bus は、エンタープライズ クラスのトランザクション メッセージング プラットフォーム サービスです。
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 [サンプル](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [リファレンス](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [リリース ノート](https://github.com/Azure/azure-service-bus-java)   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[Azure Active Directory](/azure/active-directory/active-directory-whatis)   

アプリケーションのための ID 管理と安全なサインイン。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
[サンプル](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [リファレンス](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [リリース ノート](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[Key Vault](/azure/key-vault) 

アプリケーションからキーとシークレットに安全にアクセスできます。 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

[サンプル](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [リファレンス](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [リリース ノート](https://github.com/Azure/azure-keyvault-java) 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[イベント ハブ](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
インストルメンテーションまたは IoT のシナリオに対応する高スループットのイベントとテレメトリ処理。

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

[サンプル](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [リファレンス](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [リリース ノート](https://github.com/Azure/azure-event-hubs-java)

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[IoT サービス](/azure/iot-hub/)

IoT ハブに登録済みのデバイスとの間で、ID を管理したり、メッセージを送信したり、フィードバックを取得したりすることができます。

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
[サンプル](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [リファレンス](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [リリース ノート](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[IoT デバイス](/azure/iot-hub/iot-hub-devguide)

デバイスから IoT ハブにメッセージを送信します。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

[サンプル](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [リファレンス](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [リリース ノート](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[Data Lake Store](/azure/data-lake-store/data-lake-store-overview)   
   
あらゆるサイズのデータをキャプチャし、1 つの場所にまとめて分析に利用することができます。    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

[サンプル](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [リファレンス](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [リリース ノート](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[AppInsights](/azure/application-insights/app-insights-overview)

使用状況を追跡したり、テレメトリを追加したり、Web アプリを監視したりすることができます。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

[サンプル](/azure/application-insights/app-insights-java-get-started) | [リファレンス](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [リリース ノート](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)

<a name="batch"></a>

### <a name="batchazurebatch"></a>[Batch](/azure/batch)

大規模な並列コンピューティングやハイパフォーマンス コンピューティングのアプリケーションをクラウドで効率的に実行できます。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

[サンプル](https://github.com/azure/azure-batch-samples) | [リファレンス](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [リリース ノート](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)

<a name="management"></a> 

## <a name="manage-azure-resources"></a>Azure のリソースを管理する

Azure リソースの作成、更新、削除をアプリケーション コードから行います。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

[サンプル](https://github.com/Azure/azure-sdk-for-java#sample-code) | [リファレンス](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [リリース ノート](java-sdk-azure-release-notes.md)

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomazureservice-bus-messagingservice-bus-messaging-overview"></a>[ServiceBus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) 
   
Service Bus は、エンタープライズ クラスのトランザクション メッセージング プラットフォーム サービスです。

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

[サンプル](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [リファレンス](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [リリース ノート](https://github.com/Azure/azure-service-bus-java)

