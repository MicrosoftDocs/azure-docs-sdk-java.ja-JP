---
title: Service Bus Libraries for Java
description: Service Bus 用 Java クライアントおよび管理ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, メッセージング, amqp, qpid, JMS, pubsub, pub-sub, メッセージ ブローカー
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: service-bus
ms.openlocfilehash: ed830b4f7ffa104174205f75ea2923235029ea80
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892783"
---
# <a name="service-bus-libraries-for-java"></a><span data-ttu-id="4af09-104">Service Bus Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="4af09-104">Service Bus libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="4af09-105">概要</span><span class="sxs-lookup"><span data-stu-id="4af09-105">Overview</span></span>

<span data-ttu-id="4af09-106">Service Bus は、エンタープライズ クラスのトランザクション型メッセージング プラットフォーム サービスであり、順次配信、セッション、パーティション分割、スケジューリング、複雑なサブスクリプションなどの高度な機能を持つ、信頼性の高いキューとパブリッシュ/サブスクライブ トピックや、ワークフローおよびトランザクションの処理を提供します。</span><span class="sxs-lookup"><span data-stu-id="4af09-106">Service Bus is an enterprise-class, transactional messaging platform service that provides highly reliable queues and publish/subscribe topics with deep feature capabilities such as ordered delivery, sessions, partitioning, scheduling, complex subscriptions, as well as workflow and transaction handling.</span></span>

<span data-ttu-id="4af09-107">Service Bus の機能は、従来のハイエンドのオンプレミス メッセージ ブローカーの機能と同等か、多くの場合、それを上回っています。</span><span class="sxs-lookup"><span data-stu-id="4af09-107">The Service Bus feature capabilities are comparable and often exceed those of high-end, on-premises legacy message brokers.</span></span> <span data-ttu-id="4af09-108">Service Bus の機能は、AMQP 1.0 や HTTPS のような標準ベースのプロトコルを通じて利用でき、すべてのプロトコル ジェスチャは完全に文書化されているため、広範な相互運用が可能です。</span><span class="sxs-lookup"><span data-stu-id="4af09-108">The Service Bus features are available via standards-based protocols like AMQP 1.0 and HTTPS and all protocol gestures are fully documented, allowing for broad interoperability.</span></span> 

<span data-ttu-id="4af09-109">Service Bus Premium は、可用性と信頼性が高い、耐久性のあるメッセージングを重視し、充実したローカル データセンター デプロイでも競争力のあるスループット パフォーマンスを提供します。しかも、ハードウェアの選択と入手プロセス、デプロイの計画と実行、終わりのないパフォーマンス最適化セッションは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="4af09-109">Focusing on highly available and reliable durable messaging, the Service Bus Premium provides competitive throughput performance even with substantial local datacenter deployments, but without hardware selection and acquisition processes, deployment planning and execution, and endless performance optimization sessions.</span></span> 

<span data-ttu-id="4af09-110">Service Bus Premium は、テナントごとに専用の容量が予約されている、フル マネージドのソリューションです。パフォーマンスを予測することができ、料金モデルは単純な容量ベースで、総コストは商用のオンプレミス ブローカーよりも大幅に低くなります。</span><span class="sxs-lookup"><span data-stu-id="4af09-110">Service Bus Premium is a fully managed offering with dedicated capacity reserved for each tenant that yields predictable performance with a simple, capacity-oriented pricing model and at extremely lower overall cost than commercial on-premises brokers.</span></span> <span data-ttu-id="4af09-111">多くの場合、Service Bus Premium は、付随するワークロードがクラウドで実行されない場合でも、現在の専用オンプレミス メッセージング クラスターの代わりとなります。</span><span class="sxs-lookup"><span data-stu-id="4af09-111">For many customers, Service Bus Premium can replace dedicated on-premises messaging clusters today, even if the attached workloads do not run in the cloud.</span></span> 

<span data-ttu-id="4af09-112">Service Bus の概念の詳細については、[メッセージングのドキュメントのセクション](https://docs.microsoft.com/azure/service-bus-messaging/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4af09-112">Learn more about Service Bus concepts [in the messaging documentation section](https://docs.microsoft.com/azure/service-bus-messaging/)</span></span> 

<span data-ttu-id="4af09-113">Service Bus は Java 開発者に、Microsoft がサポートするネイティブ API を提供します。また、Service Bus は Apache Qpid Proton の JMS プロバイダーなどの AMQP 1.0 に準拠しているライブラリと共に使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="4af09-113">For Java developers, Service Bus provides a Microsoft supported native API and Service Bus can also be used with AMQP 1.0 compliant libraries such as Apache Qpid Proton's JMS provider.</span></span>

## <a name="client-library"></a><span data-ttu-id="4af09-114">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="4af09-114">Client library</span></span>

<span data-ttu-id="4af09-115">公式な Service Bus クライアントは [GitHub でソース コード形式](https://github.com/azure/azure-service-bus-java)で入手でき、バイナリとパッケージ化されたソースは [Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22) で入手できます。</span><span class="sxs-lookup"><span data-stu-id="4af09-115">The official Service Bus client is available in [source code form on GitHub](https://github.com/azure/azure-service-bus-java) and binaries and packaged sources [are available on Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22).</span></span>

<span data-ttu-id="4af09-116">**[サンプル コード リポジトリ](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)には、次のサンプルが含まれます。**</span><span class="sxs-lookup"><span data-stu-id="4af09-116">**The [sample code repository](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/) contains samples for:**</span></span>
* <span data-ttu-id="4af09-117">[QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java) を使用する方法</span><span class="sxs-lookup"><span data-stu-id="4af09-117">How to use the [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java)</span></span>
* <span data-ttu-id="4af09-118">[TopicClient と SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java) を使用する方法</span><span class="sxs-lookup"><span data-stu-id="4af09-118">How to use the [TopicClient and SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java)</span></span>
* <span data-ttu-id="4af09-119">Service Bus の [MessageSender および MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) メッセージを使用する方法</span><span class="sxs-lookup"><span data-stu-id="4af09-119">How to use [MessageSender and MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) messages from Service Bus.</span></span>

<span data-ttu-id="4af09-120">独自のプロジェクトでライブラリを使用するには、Maven プロジェクトの `pom.xml` ファイルに依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="4af09-120">Add a dependency to your Maven project's `pom.xml` file to use the library in your own project.</span></span> <span data-ttu-id="4af09-121">必要に応じてバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="4af09-121">Specify the version as desired.</span></span>

<span data-ttu-id="4af09-122">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="4af09-122">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-servicebus</artifactId>
    <version>1.0.0</version>
</dependency>
```

```java
public class BasicSendReceiveWithQueueClient {
    // Connection String for the namespace can be obtained from the Azure portal under the
    // 'Shared Access policies' section.
    private static final String connectionString = "{connection string}";
    private static final String queueName = "{queue name}";
    private static IQueueClient queueClient;
    private static int totalSend = 100;
    private static int totalReceived = 0;

    public static void main(String[] args) throws Exception {

        Log.log("Starting BasicSendReceiveWithQueueClient sample");

        // create client
        Log.log("Create queue client.");
        queueClient = new QueueClient(new ConnectionStringBuilder(connectionString, queueName), ReceiveMode.PeekLock);

        // send and receive
        queueClient.registerMessageHandler(new MessageHandler(queueClient), new MessageHandlerOptions(1, false, Duration.ofMinutes(1)));
        for (int i = 0; i < totalSend; i++) {
            int j = i;
            Log.log("Sending message #%d.", j);
            queueClient.sendAsync(new Message("" + i)).thenRunAsync(() -> { Log.log("Sent message #%d.", j);});
        }

        while(totalReceived != totalSend) {
            Thread.sleep(1000);
        }

        Log.log("Received all messages, exiting the sample.");
        Log.log("Closing queue client.");
        queueClient.close();
    }

    static class MessageHandler implements IMessageHandler {
        private IQueueClient client;

        public MessageHandler(IQueueClient client) {
            this.client = client;
        }

        @Override
        public CompletableFuture<Void> onMessageAsync(IMessage iMessage) {
            Log.log("Received message with sq#: %d and lock token: %s.", iMessage.getSequenceNumber(), iMessage.getLockToken());
            return this.client.completeAsync(iMessage.getLockToken()).thenRunAsync(() -> {
                Log.log("Completed message sq#: %d and locktoken: %s", iMessage.getSequenceNumber(), iMessage.getLockToken());
                totalReceived++;
            });
        }

        @Override
        public void notifyException(Throwable throwable, ExceptionPhase exceptionPhase) {
            Log.log(exceptionPhase + "-" + throwable.getMessage());
        }
    }
}
```

> [!div class="nextstepaction"]
> <span data-ttu-id="4af09-123">[クライアント API を探す](/java/api/overview/azure/servicebus/client)
> [その他の例についてはこちらをご覧ください (詳細については上記も参照してください)](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)</span><span class="sxs-lookup"><span data-stu-id="4af09-123">[Explore the Client APIs](/java/api/overview/azure/servicebus/client)
[Find more examples here (See also above for more details)](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)</span></span>

## <a name="management-api"></a><span data-ttu-id="4af09-124">管理 API</span><span class="sxs-lookup"><span data-stu-id="4af09-124">Management API</span></span>

<span data-ttu-id="4af09-125">Management API で名前空間、トピック、キュー、およびサブスクリプションを作成および管理します。</span><span class="sxs-lookup"><span data-stu-id="4af09-125">Create and manage namespaces, topics, queues, and subscriptions with the management API.</span></span>

<span data-ttu-id="4af09-126">**例については以下をご覧ください。**</span><span class="sxs-lookup"><span data-stu-id="4af09-126">**Please find some examples here:**</span></span>
* [<span data-ttu-id="4af09-127">Service Bus キューの管理</span><span class="sxs-lookup"><span data-stu-id="4af09-127">Manage Service Bus queues</span></span>](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
* [<span data-ttu-id="4af09-128">Service Bus トピックの作成とそのトピックのサブスクライブ</span><span class="sxs-lookup"><span data-stu-id="4af09-128">Create and subscribe to Service Bus topics</span></span>](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)

<span data-ttu-id="4af09-129">**プロジェクトで管理 API を使用する方法:**
\\</span><span class="sxs-lookup"><span data-stu-id="4af09-129">**Use the Management API in your project:**
\\</span></span>
<span data-ttu-id="4af09-130">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="4af09-130">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-servicebus</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="4af09-131">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="4af09-131">Explore the Management APIs</span></span>](/java/api/overview/azure/servicebus/management)

<span data-ttu-id="4af09-132">アプリから利用できる [Azure Service Bus のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=bus)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="4af09-132">Explore more [sample Java code for Azure Service Bus](https://azure.microsoft.com/resources/samples/?platform=java&term=bus) you can use in your apps.</span></span>
