---
title: Azure Event Hub Libraries for Java
description: Java Event Hub ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, イベント ハブ, IoT, ストリーム処理
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: event-hub
ms.openlocfilehash: 076906ff3cafcb4eba97b0a022e5214d7834517c
ms.sourcegitcommit: 02b70b9f5d34415c337601f0b818f7e0985fd884
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="azure-event-hub-libraries-for-java"></a>Azure Event Hub Libraries for Java

## <a name="overview"></a>概要

[Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs) を使用すると、インターネットに接続された IoT デバイスやアプリケーションから毎秒数百万のイベントを収集して管理することができます。

Azure Event Hubs の概要については、「[Java を使用して Azure Event Hubs からイベントを受信する](/azure/event-hubs/event-hubs-java-get-started-receive-eph)」を参照してください。


## <a name="client-library"></a>クライアント ライブラリ

Azure イベント ハブにイベントを送信したりイベント ハブからイベントを取り込んで処理したりするには、Event Hubs クライアント ライブラリを使用します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>0.14.3</version>
</dependency>
```   

## <a name="example"></a>例

イベント ハブにイベントを送信します。

```java
ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName,sasKeyName, sasKey);

byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);
EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
ehClient.sendSync(sendEvent);
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/eventhub/client)


## <a name="samples"></a>サンプル

[JMS 経由でのイベント ハブへの書き込みと Apache Storm からの読み取り][1]
[ハイブリッド .NET/Java トポロジを使用した EventHubs からの読み取りと EventHubs への書き込み][2] 

[1]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[2]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

アプリで利用できる [Azure Event Hubs のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=event)を探しましょう。

