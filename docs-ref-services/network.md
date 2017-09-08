---
title: Azure Network Libraries for Java
description: "Java Azure Network 管理ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, ネットワーク, 負荷分散, vnet, サブネット"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: networking
ms.openlocfilehash: 7be23ba896887cc88ccdf0fb049cb5f579d496c3
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-network-libraries-for-java"></a><span data-ttu-id="55270-104">Azure Network Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="55270-104">Azure Network libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="55270-105">概要</span><span class="sxs-lookup"><span data-stu-id="55270-105">Overview</span></span>

<span data-ttu-id="55270-106">[Azure ネットワーク](/azure/networking/networking-overview)で Azure リソースの接続、トラフィックのフィルター処理と分散、およびルーティングの管理を行います。</span><span class="sxs-lookup"><span data-stu-id="55270-106">Connect Azure resources, filter and balance traffic, and manage routing with [Azure Networking](/azure/networking/networking-overview).</span></span>

<span data-ttu-id="55270-107">Azure ネットワークの使用を開始するには、「[最初の仮想ネットワークの作成](/azure/virtual-network/virtual-network-get-started-vnet-subnet)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="55270-107">To get started with Azure Networking, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-api"></a><span data-ttu-id="55270-108">Management API</span><span class="sxs-lookup"><span data-stu-id="55270-108">Management API</span></span>

<span data-ttu-id="55270-109">Management API で Azure [仮想ネットワーク](/azure/virtual-network/virtual-networks-overview)、[ExpressRoutes](/azure/expressroute/)、[Application Gateway](/azure/application-gateway/) を作成し、管理します。</span><span class="sxs-lookup"><span data-stu-id="55270-109">Create and manage Azure [virtual networks](/azure/virtual-network/virtual-networks-overview) , [ExpressRoutes](/azure/expressroute/) , and [Application Gateways](/azure/application-gateway/) with the management API.</span></span>

<span data-ttu-id="55270-110">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="55270-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.1.2</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="55270-111">例</span><span class="sxs-lookup"><span data-stu-id="55270-111">Example</span></span>

<span data-ttu-id="55270-112">1 つのサブネットを含む新しい仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="55270-112">Create a new virtual network with a single subnet.</span></span>

```java
Network virtualNetwork1 = azure.networks().define(vnetName1)
        .withRegion(Region.US_EAST)
        .withExistingResourceGroup("myResourceGroup")
        .withAddressSpace("192.168.0.0/16")
        .defineSubnet("myVirtualNetwork")
            .withAddressPrefix("192.168.2.0/24")
            .attach()
        .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="55270-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="55270-113">Explore the Management APIs</span></span>](/java/api/overview/azure/networking/managementapi)

## <a name="samples"></a><span data-ttu-id="55270-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="55270-114">Samples</span></span>

<span data-ttu-id="55270-115">[仮想ネットワークを管理する](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span><span class="sxs-lookup"><span data-stu-id="55270-115">[Manage virtual networks](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span></span>  
<span data-ttu-id="55270-116">[ネットワーク インターフェイスを管理する](https://github.com/Azure-Samples/network-java-manage-network-interface) </span><span class="sxs-lookup"><span data-stu-id="55270-116">[Manage network interfaces](https://github.com/Azure-Samples/network-java-manage-network-interface) </span></span>  
<span data-ttu-id="55270-117">[Application Gateway を管理する](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span><span class="sxs-lookup"><span data-stu-id="55270-117">[Manage Application Gateways](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span></span>  
[<span data-ttu-id="55270-118">インターネットに接続するロード バランサーを管理する</span><span class="sxs-lookup"><span data-stu-id="55270-118">Manage internet facing load balancers</span></span>](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

<span data-ttu-id="55270-119">アプリから利用できる [Azure ネットワークのサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=network)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="55270-119">Explore more [sample Java code for Azure Networking](https://azure.microsoft.com/resources/samples/?platform=java&term=network) you can use in your apps.</span></span>