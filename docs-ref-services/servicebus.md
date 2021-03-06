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
ms.openlocfilehash: 2099695784a59bf539aeae745df1fe38ec84f511
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593277"
---
# <a name="service-bus-libraries-for-java"></a>Service Bus Libraries for Java

## <a name="overview"></a>概要

Service Bus は、エンタープライズ クラスのトランザクション型メッセージング プラットフォーム サービスであり、順次配信、セッション、パーティション分割、スケジューリング、複雑なサブスクリプションなどの高度な機能を持つ、信頼性の高いキューとパブリッシュ/サブスクライブ トピックや、ワークフローおよびトランザクションの処理を提供します。

Service Bus の機能は、従来のハイエンドのオンプレミス メッセージ ブローカーの機能と同等か、多くの場合、それを上回っています。 Service Bus の機能は、AMQP 1.0 や HTTPS のような標準ベースのプロトコルを通じて利用でき、すべてのプロトコル ジェスチャは完全に文書化されているため、広範な相互運用が可能です。 

Service Bus Premium は、可用性と信頼性が高い、耐久性のあるメッセージングを重視し、充実したローカル データセンター デプロイでも競争力のあるスループット パフォーマンスを提供します。しかも、ハードウェアの選択と入手プロセス、デプロイの計画と実行、終わりのないパフォーマンス最適化セッションは必要ありません。 

Service Bus Premium は、テナントごとに専用の容量が予約されている、フル マネージドのソリューションです。パフォーマンスを予測することができ、料金モデルは単純な容量ベースで、総コストは商用のオンプレミス ブローカーよりも大幅に低くなります。 多くの場合、Service Bus Premium は、付随するワークロードがクラウドで実行されない場合でも、現在の専用オンプレミス メッセージング クラスターの代わりとなります。 

Service Bus の概念の詳細については、[メッセージングのドキュメントのセクション](https://docs.microsoft.com/azure/service-bus-messaging/)を参照してください。 

Service Bus は Java 開発者に、Microsoft がサポートするネイティブ API を提供します。また、Service Bus は Apache Qpid Proton の JMS プロバイダーなどの AMQP 1.0 に準拠しているライブラリと共に使用することもできます。

## <a name="client-library"></a>クライアント ライブラリ

公式な Service Bus クライアントは [GitHub でソース コード形式](https://github.com/azure/azure-service-bus-java)で入手でき、バイナリとパッケージ化されたソースは [Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22) で入手できます。

**[サンプル コード リポジトリ](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)には、次のサンプルが含まれます。**
* [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java) を使用する方法
* [TopicClient と SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java) を使用する方法
* Service Bus の [MessageSender および MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) メッセージを使用する方法

独自のプロジェクトでライブラリを使用するには、Maven プロジェクトの `pom.xml` ファイルに依存関係を追加します。 必要に応じてバージョンを指定します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

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
> [クライアント API を探す](/java/api/overview/azure/servicebus/client)
> [その他の例についてはこちらをご覧ください (詳細については上記も参照してください)](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)

## <a name="management-api"></a>管理 API

Management API で名前空間、トピック、キュー、およびサブスクリプションを作成および管理します。

**例については以下をご覧ください。**
* [Service Bus キューの管理](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
* [Service Bus トピックの作成とそのトピックのサブスクライブ](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)

**プロジェクトで管理 API を使用する方法:**
\
プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-servicebus</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/servicebus/management)

アプリから利用できる [Azure Service Bus のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=bus)を探しましょう。
