---
title: Java 用 Azure Event Grid ライブラリ
description: Azure Event Grid Java ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, Event Grid, メッセージング, イベント ドリブン
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 07/11/2017
ms.topic: managed-reference
ms.devlang: java
ms.service: event-grid
ms.openlocfilehash: 35acbdeede2fbc541128029ad43f149209a057c4
ms.sourcegitcommit: db36eeb667a0bfe6d113953bb0583240db61b0f4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134630"
---
# <a name="azure-event-grid-libraries-for-java"></a>Java 用 Azure Event Grid ライブラリ

Azure Event Grid で簡単な HTTP ベースのイベント処理を使用して、Azure サービスやカスタム ソースのイベントをリッスンして対応するイベント ドリブン アプリケーションを構築します。

Azure Event Grid の[詳細を確認](/azure/event-grid/overview)し、[Azure Blob Storage イベントのチュートリアル](/azure/storage/blobs/storage-blob-event-quickstart)を開始してください。 

## <a name="client-sdk"></a>クライアント SDK

Azure Event Grid クライアント SDK を使用して、イベントの作成、認証、トピックへの投稿を行います。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventgrid</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="publish-events"></a>イベントの発行

次のコードでは、Azure で認証し、カスタム型の `EventGridEvent` イベント (この例では `Contoso.Items.ItemsReceived`) の `List` をトピックに発行します。 このサンプルで使用されているトピック キーとエンドポイント アドレスは、Azure CLI から取得できます。

```azurecli-interactive
az eventgrid topic show -g gridResourceGroup -n topicName
az eventgrid topic key list -g gridResourceGroup -n topicName
```

```java
// Create an event grid client.
TopicCredentials topicCredentials = new TopicCredentials(System.getenv("EVENTGRID_TOPIC_KEY"));
EventGridClient client = new EventGridClientImpl(topicCredentials);

// Publish custom events to the EventGrid.
System.out.println("Publish custom events to the EventGrid");
List<EventGridEvent> eventsList = new ArrayList<>();
for (int i = 0; i < 5; i++) {
    eventsList.add(new EventGridEvent(
        UUID.randomUUID().toString(),
        String.format("Door%d", i),
        new ContosoItemReceivedEventData("Contoso Item SKU #1"),
        "Contoso.Items.ItemReceived",
        DateTime.now(),
        "2.0"
    ));
}

String eventGridEndpoint = String.format("https://%s/", new URI(System.getenv("EVENTGRID_TOPIC_ENDPOINT")).getHost());

client.publishEvents(eventGridEndpoint, eventsList);
```

> [!div class="nextstepaction"]
> [Event Grid クライアント API を探す](/java/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a>管理 SDK

Event Grid 管理 SDK を使用して、Event Grid のインスタンス、トピック、サブスクリプションを作成、更新、削除します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.16.0</version>
</dependency>
```   

次の例では、[EventGrid Java サンプル](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)から Event Grid サブスクリプションを作成します

```java
// Create an event grid subscription.
//
System.out.println("Creating an Azure EventGrid Subscription");

EventSubscription eventSubscription = eventGridManager.eventSubscriptions().define(eventSubscriptionName)
    .withScope(eventGridTopic.id())
    .withDestination(new EventHubEventSubscriptionDestination()
        .withResourceId(eventHub.id()))
    .withFilter(new EventSubscriptionFilter()
        .withIsSubjectCaseSensitive(false)
        .withSubjectBeginsWith("")
        .withSubjectEndsWith(""))
    .create();
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/eventgrid/management)