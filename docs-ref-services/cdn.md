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
# <a name="azure-cdn-libraries-for-java"></a><span data-ttu-id="60b88-104">Azure CDN Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="60b88-104">Azure CDN libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="60b88-105">概要</span><span class="sxs-lookup"><span data-stu-id="60b88-105">Overview</span></span>

<span data-ttu-id="60b88-106">[Azure Content Delivery Network](/azure/cdn/cdn-overview) (CDN) を使用して、戦略的に配置された場所に静的 Web コンテンツをキャッシュし、ユーザーへのスループットを最大化します。</span><span class="sxs-lookup"><span data-stu-id="60b88-106">Cache static web content at strategically placed locations to provide maximum throughput for users with [Azure Content Delivery Network](/azure/cdn/cdn-overview) (CDN).</span></span>

<span data-ttu-id="60b88-107">Azure CDN を導入するには、「[Azure CDN の概要](/azure/cdn/cdn-create-new-endpoint)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60b88-107">To get started with Azure CDN, see [Getting started with Azure CDN](/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-api"></a><span data-ttu-id="60b88-108">Management API</span><span class="sxs-lookup"><span data-stu-id="60b88-108">Management API</span></span>

<span data-ttu-id="60b88-109">CDN プロファイルを作成して、エンドポイントを定義し、Management API を使用して CDN にコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="60b88-109">Create CDN profiles, define endpoints, and add content to the CDN using the management API.</span></span>

<span data-ttu-id="60b88-110">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="60b88-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.1.2</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="60b88-111">例</span><span class="sxs-lookup"><span data-stu-id="60b88-111">Example</span></span>

<span data-ttu-id="60b88-112">CDN プロファイルを作成して、エンドポイントを割り当て、コンテンツを CDN に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="60b88-112">Create a CDN profile, assign endpoints, and load content into the CDN.</span></span>

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
> [<span data-ttu-id="60b88-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="60b88-113">Explore the Management APIs</span></span>](/java/api/overview/azure/cdn/managementapi)

## <a name="samples"></a><span data-ttu-id="60b88-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="60b88-114">Samples</span></span>

[<span data-ttu-id="60b88-115">Java で CDN を管理する</span><span class="sxs-lookup"><span data-stu-id="60b88-115">Manage CDNs with Java</span></span>](https://github.com/Azure-Samples/cdn-java-manage-cdn)

<span data-ttu-id="60b88-116">アプリで利用できる [Azure CDN のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="60b88-116">Explore more [sample Java code for Azure CDN](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn) you can use in your apps.</span></span>