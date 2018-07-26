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
ms.openlocfilehash: b6646ef27edace4247090e749c9a52cd6a33a82c
ms.sourcegitcommit: 3d3460289ab6b9165c2cf6a3dd56eafd0692501e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
ms.locfileid: "34283026"
---
# <a name="azure-event-hub-libraries-for-java"></a>Azure Event Hub Libraries for Java

## <a name="overview"></a>概要

[Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs) を使用すると、インターネットに接続された IoT デバイスやアプリケーションから毎秒数百万のイベントを収集して管理することができます。

Azure Event Hubs の概要については、「[Java を使用して Azure Event Hubs からイベントを受信する](/azure/event-hubs/event-hubs-java-get-started-receive-eph)」を参照してください。


## <a name="client-library"></a>クライアント ライブラリ

Azure イベント ハブにイベントを送信したりイベント ハブからイベントを取り込んで処理したりするには、Event Hubs クライアント ライブラリを使用します。

プロジェクトで[クライアント ライブラリ](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs)を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。
 

## <a name="example"></a>例

イベント ハブにイベントを送信します。

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                                            .setNamespaceName(namespaceName)
                                            .setEventHubName(eventHubName)
                                            .setSasKeyName(sasKeyName)
                                            .setSasKey(sasKey);
final EventHubClient ehClient = EventHubClient.createSync(connStr.toString());

final byte[] payloadBytes = "Test AMQP message".getBytes("UTF-8");
final EventData sendEvent = new EventData(payloadBytes);

ehClient.sendSync(sendEvent);
```


> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/eventhubs/client)



## <a name="samples"></a>サンプル

[サンプルを使用した Event Hub data-plane API の確認][1]

[JMS を使用したイベント ハブへの書き込みと Apache Storm からの読み取り][2]

[ハイブリッド .NET/Java トポロジを使用した Event Hubs の読み取りと書き込み][3] 

[1]: https://github.com/Azure/azure-event-hubs/tree/master/samples/Java
[2]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[3]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

アプリで利用できる [Azure Event Hubs のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=event)を探しましょう。

