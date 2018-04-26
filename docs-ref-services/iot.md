---
title: Azure IoT Hub Libraries for Java
description: Java Azure IoT Hub ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, イベント, IoT, ストリーム, デバイス, iot hub
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: iot-hub
ms.openlocfilehash: 5e6a102b062b2fff6b297c7e3dda423d1448bcb0
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="azure-iot-libraries-for-java"></a>Azure IoT Libraries for Java

モノのインターネット資産の接続、監視、制御は、[Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub) で行います。

Azure IoT Hub の概要については、「[Java を使用してデバイスを IoT ハブに接続する](/azure/iot-hub/iot-hub-java-java-getstarted)」を参照してください。

## <a name="iot-service-library"></a>IoT サービス ライブラリ

デバイスを登録してクラウドから登録済みデバイスにメッセージを送信するには、IoT サービス ライブラリを使用します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a>IoT デバイス ライブラリ

クラウドにメッセージを送信したりデバイスでメッセージを受信したりするには、IoT デバイス ライブラリを使用します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/iot/client)   

## <a name="example"></a>例

Azure IoT Hub からデバイスにメッセージを送信します。

```java
Message messageToSend = new Message(messageText);
messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
messageToSend.setMessageId(java.util.UUID.randomUUID().toString());

// set message properties
Map<String, String> propertiesToSend = new HashMap<String, String>();
propertiesToSend.put(messagePropertyKey,messagePropertyKey);
messageToSend.setProperties(propertiesToSend);

CompletableFuture<Void> future = serviceClient.sendAsync(deviceId, messageToSend);
try {
    future.get();
}
catch (ExecutionException e) {
    System.out.println("Exception : " + e.getMessage());
}
```


## <a name="samples"></a>サンプル

[IoT デバイスのサンプル](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)     
[IoT サービスのサンプル](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

アプリで利用できる [Azure IoT のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=iot)を探しましょう。
