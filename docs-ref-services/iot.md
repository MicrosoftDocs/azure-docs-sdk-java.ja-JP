---
title: Azure IoT Hub Libraries for Java
description: "Java Azure IoT Hub ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, イベント, IoT, ストリーム, デバイス, iot hub"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: iot-hub
ms.openlocfilehash: b386612741e222fcaeb7b6c38753d0cb7d616239
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-iot-libraries-for-java"></a><span data-ttu-id="2a8a7-104">Azure IoT Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="2a8a7-104">Azure IoT libraries for Java</span></span>

<span data-ttu-id="2a8a7-105">モノのインターネット資産の接続、監視、制御は、[Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-what-is-iot-hub) で行います。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-105">Connect, monitor, and control Internet of Things assets with [Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-what-is-iot-hub).</span></span>

<span data-ttu-id="2a8a7-106">Azure IoT Hub の概要については、「[Java を使用してデバイスを IoT ハブに接続する](/azure/iot-hub/iot-hub-java-java-getstarted)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-106">To get started with Azure IoT Hub, see [Connect your device to your IoT hub using Java](/azure/iot-hub/iot-hub-java-java-getstarted).</span></span>

## <a name="iot-service-library"></a><span data-ttu-id="2a8a7-107">IoT サービス ライブラリ</span><span class="sxs-lookup"><span data-stu-id="2a8a7-107">IoT Service library</span></span>

<span data-ttu-id="2a8a7-108">デバイスを登録してクラウドから登録済みデバイスにメッセージを送信するには、IoT サービス ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-108">Register devices and send messages from the cloud to registered devices using the IoT Service library.</span></span>

<span data-ttu-id="2a8a7-109">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a><span data-ttu-id="2a8a7-110">IoT デバイス ライブラリ</span><span class="sxs-lookup"><span data-stu-id="2a8a7-110">Iot Device library</span></span>

<span data-ttu-id="2a8a7-111">クラウドにメッセージを送信したりデバイスでメッセージを受信したりするには、IoT デバイス ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-111">Send messages to the cloud and receive messages on devices using the IoT Device library.</span></span>

<span data-ttu-id="2a8a7-112">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-112">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="2a8a7-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="2a8a7-113">Explore the Client APIs</span></span>](/java/api/overview/azure/iot/clientlibrary)   

## <a name="example"></a><span data-ttu-id="2a8a7-114">例</span><span class="sxs-lookup"><span data-stu-id="2a8a7-114">Example</span></span>

<span data-ttu-id="2a8a7-115">Azure IoT Hub からデバイスにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-115">Send a message from Azure IoT Hub to a device.</span></span>

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


## <a name="samples"></a><span data-ttu-id="2a8a7-116">サンプル</span><span class="sxs-lookup"><span data-stu-id="2a8a7-116">Samples</span></span>

<span data-ttu-id="2a8a7-117">[IoT デバイスのサンプル](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span><span class="sxs-lookup"><span data-stu-id="2a8a7-117">[IoT Device samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span></span>  
[<span data-ttu-id="2a8a7-118">IoT サービスのサンプル</span><span class="sxs-lookup"><span data-stu-id="2a8a7-118">IoT Service samples</span></span>](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

<span data-ttu-id="2a8a7-119">アプリで利用できる [Azure IoT のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=iot)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="2a8a7-119">Explore more [sample Java code for Azure IoT](https://azure.microsoft.com/resources/samples/?platform=java&term=iot) you can use in your apps.</span></span>
