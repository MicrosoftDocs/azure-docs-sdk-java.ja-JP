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
ms.openlocfilehash: a38db9a0d92d7dedc5ff49b83ad2658aa61729dd
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2017
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="5e217-104">Azure Traffic Manager Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="5e217-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="5e217-105">概要</span><span class="sxs-lookup"><span data-stu-id="5e217-105">Overview</span></span>

<span data-ttu-id="5e217-106">[Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) を使用して、ユーザー トラフィックがさまざまなデータセンターのサービス エンドポイントに分散されるように制御します。</span><span class="sxs-lookup"><span data-stu-id="5e217-106">Control the distribution of user traffic for service endpoints in different datacenters with [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview).</span></span>

<span data-ttu-id="5e217-107">Azure Traffic Manager を導入するには、「[Traffic Manager プロファイルの作成](/azure/traffic-manager/traffic-manager-create-profile)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5e217-107">To get started with Azure Traffic Manager, see [Create a Traffic Manager profile](/azure/traffic-manager/traffic-manager-create-profile).</span></span>

## <a name="management-api"></a><span data-ttu-id="5e217-108">Management API</span><span class="sxs-lookup"><span data-stu-id="5e217-108">Management API</span></span>

<span data-ttu-id="5e217-109">Management API を使用して、Traffic Manager プロファイルの作成、エンドポイントの定義、ルーティング方法の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="5e217-109">Create Traffic Manager profiles, define endpoints, and change the routing method with the management API.</span></span> 

<span data-ttu-id="5e217-110">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="5e217-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.2.1</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="5e217-111">例</span><span class="sxs-lookup"><span data-stu-id="5e217-111">Example</span></span>

<span data-ttu-id="5e217-112">Traffic Manager プロファイルを作成し、単一のエンドポイントを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="5e217-112">Create a Traffic Manager profile and assign a single endpoint.</span></span>

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
> [<span data-ttu-id="5e217-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="5e217-113">Explore the Management APIs</span></span>](/java/api/overview/azure/trafficmanager/managementapi)

## <a name="samples"></a><span data-ttu-id="5e217-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="5e217-114">Samples</span></span>

[<span data-ttu-id="5e217-115">Web アプリのトラフィックを複数のリージョンに分散する</span><span class="sxs-lookup"><span data-stu-id="5e217-115">Balance web app traffic across multiple regions</span></span>](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

<span data-ttu-id="5e217-116">アプリで利用できる [Azure Traffic Manager のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="5e217-116">Explore more [sample Java code for Azure Traffic Manager](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) you can use in your apps.</span></span>
