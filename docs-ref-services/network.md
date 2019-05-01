---
title: Azure Network Libraries for Java
description: Java Azure Network 管理ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, ネットワーク, 負荷分散, vnet, サブネット
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: networking
ms.openlocfilehash: 4eb4f583da686508db9fb0c4346f2525502f669d
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593327"
---
# <a name="azure-network-libraries-for-java"></a>Azure Network Libraries for Java

## <a name="overview"></a>概要

[Azure ネットワーク](/azure/networking/networking-overview)で Azure リソースの接続、トラフィックのフィルター処理と分散、およびルーティングの管理を行います。

Azure ネットワークの使用を開始するには、「[最初の仮想ネットワークの作成](/azure/virtual-network/virtual-network-get-started-vnet-subnet)」を参照してください。

## <a name="management-api"></a>管理 API

Management API で Azure [仮想ネットワーク](/azure/virtual-network/virtual-networks-overview)、[ExpressRoutes](/azure/expressroute/)、[Application Gateway](/azure/application-gateway/) を作成し、管理します。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>例

1 つのサブネットを含む新しい仮想ネットワークを作成します。

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
> [Management API を探す](/java/api/overview/azure/networking/management)

## <a name="samples"></a>サンプル

[仮想ネットワークを管理する](https://github.com/Azure-Samples/network-java-manage-virtual-network)   
[ネットワーク インターフェイスを管理する](https://github.com/Azure-Samples/network-java-manage-network-interface)   
[Application Gateway を管理する](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways)   
[インターネットに接続するロード バランサーを管理する](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

アプリから利用できる [Azure ネットワークのサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=network)を探しましょう。
