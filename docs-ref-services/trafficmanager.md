---
title: Azure Traffic Manager Libraries for Java
description: Java Traffic Manager 管理ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, 負荷分散, 負荷の分散, ネットワーク, Traffic Manager
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: traffic-manager
ms.openlocfilehash: fd78402f50df16ad7d57c0ca67815bfad5b18d51
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892873"
---
# <a name="azure-traffic-manager-libraries-for-java"></a>Azure Traffic Manager Libraries for Java

## <a name="overview"></a>概要

[Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) を使用して、ユーザー トラフィックがさまざまなデータセンターのサービス エンドポイントに分散されるように制御します。

Azure Traffic Manager を導入するには、「[Traffic Manager プロファイルの作成](/azure/traffic-manager/traffic-manager-create-profile)」を参照してください。

## <a name="management-api"></a>Management API

Management API を使用して、Traffic Manager プロファイルの作成、エンドポイントの定義、ルーティング方法の変更を行います。 

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.3.0</version>
</dependency>
```   

## <a name="example"></a>例

Traffic Manager プロファイルを作成し、単一のエンドポイントを割り当てます。

```java
TrafficManagerProfile tmProfile = azure.trafficManagerProfiles().define("testTMProfile")
        .withNewResourceGroup(Region.US_EAST)
        .withLeafDomainLabel("testTMProfile")
        .withPriorityBasedRouting()
        .defineAzureTargetEndpoint("endpoint")
        .toResourceId(webAppId)
        .withRoutingPriority(1)
        .attach()
        .create();
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/trafficmanager/management)

## <a name="samples"></a>サンプル

[Web アプリのトラフィックを複数のリージョンに分散する](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

アプリで利用できる [Azure Traffic Manager のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)を探しましょう。
