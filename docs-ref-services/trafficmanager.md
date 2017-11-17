---
title: Azure Traffic Manager Libraries for Java
description: "Java Traffic Manager 管理ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, 負荷分散, 負荷の分散, ネットワーク, Traffic Manager"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: traffic-manager
ms.openlocfilehash: 9e13f97c6ddb763fb162b3de0c8d09c77eae1ccb
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2017
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="f92b5-104">Azure Traffic Manager Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="f92b5-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="f92b5-105">概要</span><span class="sxs-lookup"><span data-stu-id="f92b5-105">Overview</span></span>

<span data-ttu-id="f92b5-106">[Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) を使用して、ユーザー トラフィックがさまざまなデータセンターのサービス エンドポイントに分散されるように制御します。</span><span class="sxs-lookup"><span data-stu-id="f92b5-106">Control the distribution of user traffic for service endpoints in different datacenters with [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview).</span></span>

<span data-ttu-id="f92b5-107">Azure Traffic Manager を導入するには、「[Traffic Manager プロファイルの作成](/azure/traffic-manager/traffic-manager-create-profile)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f92b5-107">To get started with Azure Traffic Manager, see [Create a Traffic Manager profile](/azure/traffic-manager/traffic-manager-create-profile).</span></span>

## <a name="management-api"></a><span data-ttu-id="f92b5-108">Management API</span><span class="sxs-lookup"><span data-stu-id="f92b5-108">Management API</span></span>

<span data-ttu-id="f92b5-109">Management API を使用して、Traffic Manager プロファイルの作成、エンドポイントの定義、ルーティング方法の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="f92b5-109">Create Traffic Manager profiles, define endpoints, and change the routing method with the management API.</span></span> 

<span data-ttu-id="f92b5-110">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="f92b5-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.3.0</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="f92b5-111">例</span><span class="sxs-lookup"><span data-stu-id="f92b5-111">Example</span></span>

<span data-ttu-id="f92b5-112">Traffic Manager プロファイルを作成し、単一のエンドポイントを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="f92b5-112">Create a Traffic Manager profile and assign a single endpoint.</span></span>

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
> [<span data-ttu-id="f92b5-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="f92b5-113">Explore the Management APIs</span></span>](/java/api/overview/azure/trafficmanager/managementapi)

## <a name="samples"></a><span data-ttu-id="f92b5-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="f92b5-114">Samples</span></span>

[<span data-ttu-id="f92b5-115">Web アプリのトラフィックを複数のリージョンに分散する</span><span class="sxs-lookup"><span data-stu-id="f92b5-115">Balance web app traffic across multiple regions</span></span>](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

<span data-ttu-id="f92b5-116">アプリで利用できる [Azure Traffic Manager のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="f92b5-116">Explore more [sample Java code for Azure Traffic Manager](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) you can use in your apps.</span></span>
