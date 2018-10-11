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
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="8a48d-104">Azure Traffic Manager Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="8a48d-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="8a48d-105">概要</span><span class="sxs-lookup"><span data-stu-id="8a48d-105">Overview</span></span>

<span data-ttu-id="8a48d-106">[Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) を使用して、ユーザー トラフィックがさまざまなデータセンターのサービス エンドポイントに分散されるように制御します。</span><span class="sxs-lookup"><span data-stu-id="8a48d-106">Control the distribution of user traffic for service endpoints in different datacenters with [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview).</span></span>

<span data-ttu-id="8a48d-107">Azure Traffic Manager を導入するには、「[Traffic Manager プロファイルの作成](/azure/traffic-manager/traffic-manager-create-profile)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a48d-107">To get started with Azure Traffic Manager, see [Create a Traffic Manager profile](/azure/traffic-manager/traffic-manager-create-profile).</span></span>

## <a name="management-api"></a><span data-ttu-id="8a48d-108">Management API</span><span class="sxs-lookup"><span data-stu-id="8a48d-108">Management API</span></span>

<span data-ttu-id="8a48d-109">Management API を使用して、Traffic Manager プロファイルの作成、エンドポイントの定義、ルーティング方法の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="8a48d-109">Create Traffic Manager profiles, define endpoints, and change the routing method with the management API.</span></span> 

<span data-ttu-id="8a48d-110">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="8a48d-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.3.0</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="8a48d-111">例</span><span class="sxs-lookup"><span data-stu-id="8a48d-111">Example</span></span>

<span data-ttu-id="8a48d-112">Traffic Manager プロファイルを作成し、単一のエンドポイントを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="8a48d-112">Create a Traffic Manager profile and assign a single endpoint.</span></span>

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
> [<span data-ttu-id="8a48d-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="8a48d-113">Explore the Management APIs</span></span>](/java/api/overview/azure/trafficmanager/management)

## <a name="samples"></a><span data-ttu-id="8a48d-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="8a48d-114">Samples</span></span>

[<span data-ttu-id="8a48d-115">Web アプリのトラフィックを複数のリージョンに分散する</span><span class="sxs-lookup"><span data-stu-id="8a48d-115">Balance web app traffic across multiple regions</span></span>](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

<span data-ttu-id="8a48d-116">アプリで利用できる [Azure Traffic Manager のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="8a48d-116">Explore more [sample Java code for Azure Traffic Manager](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) you can use in your apps.</span></span>
