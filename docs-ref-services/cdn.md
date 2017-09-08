---
title: Azure CDN Libraries for Java
description: "Java CDN 管理ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, コンテンツ, 配信, ネットワーク, CDN"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cdn
ms.openlocfilehash: e4605b1a7dd7fe34d191337acb2f20cbca2e7037
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-cdn-libraries-for-java"></a>Azure CDN Libraries for Java

## <a name="overview"></a>概要

[Azure Content Delivery Network](/azure/cdn/cdn-overview) (CDN) を使用して、戦略的に配置された場所に静的 Web コンテンツをキャッシュし、ユーザーへのスループットを最大化します。

Azure CDN を導入するには、「[Azure CDN の概要](/azure/cdn/cdn-create-new-endpoint)」を参照してください。

## <a name="management-api"></a>Management API

CDN プロファイルを作成して、エンドポイントを定義し、Management API を使用して CDN にコンテンツを追加します。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.1.2</version>
</dependency>
```   

### <a name="example"></a>例

CDN プロファイルを作成して、エンドポイントを割り当て、コンテンツを CDN に読み込みます。

```java
CdnProfile profile = azure.cdnProfiles().define("testCDN")
        .withRegion(Region.US_EAST)
        .withNewResourceGroup()
        .withStandardVerizonSku()
        .withNewEndpoint("webAppHostname1")
        .withNewEndpoint("webAppHostname2")
        .create();

List<String> contentList = new ArrayList<String>();
contentList.add("/client.js");
contentList.add("/media/fabrikam_logo.png");

for (CdnEndpoint endpoint : profile.endpoints().values()) {
    endpoint.loadContent(contentList);
}
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/cdn/managementapi)

## <a name="samples"></a>サンプル

[Java で CDN を管理する](https://github.com/Azure-Samples/cdn-java-manage-cdn)

アプリで利用できる [Azure CDN のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn)を探しましょう。