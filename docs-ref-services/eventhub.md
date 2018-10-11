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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893473"
---
# <a name="azure-event-hub-libraries-for-java"></a><span data-ttu-id="d6dd1-104">Azure Event Hub Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="d6dd1-104">Azure Event Hub libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d6dd1-105">概要</span><span class="sxs-lookup"><span data-stu-id="d6dd1-105">Overview</span></span>

<span data-ttu-id="d6dd1-106">[Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs) を使用すると、インターネットに接続された IoT デバイスやアプリケーションから毎秒数百万のイベントを収集して管理することができます。</span><span class="sxs-lookup"><span data-stu-id="d6dd1-106">Collect and manage millions of events per second from connected IoT devices and applications with [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs).</span></span>

<span data-ttu-id="d6dd1-107">Azure Event Hubs の概要については、「[Java を使用して Azure Event Hubs からイベントを受信する](/azure/event-hubs/event-hubs-java-get-started-receive-eph)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d6dd1-107">To get started with Azure Event Hubs, see [Receive events from Azure Event Hubs using Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span></span>


## <a name="client-library"></a><span data-ttu-id="d6dd1-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d6dd1-108">Client library</span></span>

<span data-ttu-id="d6dd1-109">Azure イベント ハブにイベントを送信したりイベント ハブからイベントを取り込んで処理したりするには、Event Hubs クライアント ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="d6dd1-109">Send events to an Azure Event Hub and consume and process events from an Event Hub using the Event Hubs client library.</span></span>

<span data-ttu-id="d6dd1-110">プロジェクトで[クライアント ライブラリ](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs)を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="d6dd1-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the [client library](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs) in your project.</span></span>
 

## <a name="example"></a><span data-ttu-id="d6dd1-111">例</span><span class="sxs-lookup"><span data-stu-id="d6dd1-111">Example</span></span>

<span data-ttu-id="d6dd1-112">イベント ハブにイベントを送信します。</span><span class="sxs-lookup"><span data-stu-id="d6dd1-112">Send an event to an event hub.</span></span>

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
> [<span data-ttu-id="d6dd1-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="d6dd1-113">Explore the Client APIs</span></span>](/java/api/overview/azure/eventhubs/client)



## <a name="samples"></a><span data-ttu-id="d6dd1-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="d6dd1-114">Samples</span></span>

<span data-ttu-id="d6dd1-115">[サンプルを使用した Event Hub data-plane API の確認][1]</span><span class="sxs-lookup"><span data-stu-id="d6dd1-115">[Explore Event Hub data-plane API using samples][1]</span></span>

<span data-ttu-id="d6dd1-116">[JMS を使用したイベント ハブへの書き込みと Apache Storm からの読み取り][2]</span><span class="sxs-lookup"><span data-stu-id="d6dd1-116">[Write to Event Hub via JMS and read from Apache Storm][2]</span></span>

<span data-ttu-id="d6dd1-117">[ハイブリッド .NET/Java トポロジを使用した Event Hubs の読み取りと書き込み][3]</span><span class="sxs-lookup"><span data-stu-id="d6dd1-117">[Read and write from EventHubs using a hybrid .NET/Java topology][3]</span></span> 

[1]: https://github.com/Azure/azure-event-hubs/tree/master/samples/Java
[2]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[3]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

<span data-ttu-id="d6dd1-118">アプリで利用できる [Azure Event Hubs のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=event)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="d6dd1-118">Explore more [sample Java code for Azure Event Hubs](https://azure.microsoft.com/resources/samples/?platform=java&term=event) you can use in your apps.</span></span>

