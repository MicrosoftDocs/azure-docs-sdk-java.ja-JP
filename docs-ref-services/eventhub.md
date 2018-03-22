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
# <a name="azure-event-hub-libraries-for-java"></a><span data-ttu-id="08a93-104">Azure Event Hub Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="08a93-104">Azure Event Hub libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="08a93-105">概要</span><span class="sxs-lookup"><span data-stu-id="08a93-105">Overview</span></span>

<span data-ttu-id="08a93-106">[Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs) を使用すると、インターネットに接続された IoT デバイスやアプリケーションから毎秒数百万のイベントを収集して管理することができます。</span><span class="sxs-lookup"><span data-stu-id="08a93-106">Collect and manage millions of events per second from connected IoT devices and applications with [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs).</span></span>

<span data-ttu-id="08a93-107">Azure Event Hubs の概要については、「[Java を使用して Azure Event Hubs からイベントを受信する](/azure/event-hubs/event-hubs-java-get-started-receive-eph)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="08a93-107">To get started with Azure Event Hubs, see [Receive events from Azure Event Hubs using Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span></span>


## <a name="client-library"></a><span data-ttu-id="08a93-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="08a93-108">Client library</span></span>

<span data-ttu-id="08a93-109">Azure イベント ハブにイベントを送信したりイベント ハブからイベントを取り込んで処理したりするには、Event Hubs クライアント ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="08a93-109">Send events to an Azure Event Hub and consume and process events from an Event Hub using the Event Hubs client library.</span></span>

<span data-ttu-id="08a93-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="08a93-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>0.14.3</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="08a93-111">例</span><span class="sxs-lookup"><span data-stu-id="08a93-111">Example</span></span>

<span data-ttu-id="08a93-112">イベント ハブにイベントを送信します。</span><span class="sxs-lookup"><span data-stu-id="08a93-112">Send an event to an event hub.</span></span>

```java
ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName,sasKeyName, sasKey);

byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);
EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
ehClient.sendSync(sendEvent);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="08a93-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="08a93-113">Explore the Client APIs</span></span>](/java/api/overview/azure/eventhub/client)


## <a name="samples"></a><span data-ttu-id="08a93-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="08a93-114">Samples</span></span>

<span data-ttu-id="08a93-115">[JMS 経由でのイベント ハブへの書き込みと Apache Storm からの読み取り][1]
[ハイブリッド .NET/Java トポロジを使用した EventHubs からの読み取りと EventHubs への書き込み][2]</span><span class="sxs-lookup"><span data-stu-id="08a93-115">[Write to Event Hub via JMS and read from Apache Storm][1]
[Read and write from EventHubs using a hybrid .NET/Java topology][2]</span></span> 

[1]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[2]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

<span data-ttu-id="08a93-116">アプリで利用できる [Azure Event Hubs のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=event)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="08a93-116">Explore more [sample Java code for Azure Event Hubs](https://azure.microsoft.com/resources/samples/?platform=java&term=event) you can use in your apps.</span></span>

