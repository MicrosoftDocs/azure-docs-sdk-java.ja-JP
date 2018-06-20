---
title: Azure 仮想ネットワークを Java で管理する | Microsoft Docs
description: Java コードで Azure 仮想ネットワークを管理するためのサンプル コード
author: rloutlaw
manager: douge
ms.assetid: 92736911-3df6-46e7-b751-25bb36bf89b9
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 3d21cdd890912c1fc58fc65a79ba972b8327edeb
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931088"
---
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a>Java アプリによる Azure 仮想ネットワークの作成と管理

[このサンプル](https://github.com/Azure-Samples/network-java-manage-virtual-network)では、管理対象となるネットワーク セグメントに対し、Azure リソースを分離する[仮想ネットワーク](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)を作成します。

## <a name="run-the-sample"></a>サンプルの実行

[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。 次に、以下を実行します。

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java) を参照してください。

## <a name="authenticate-with-azure"></a>Azure での認証

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a>インターネット トラフィックをブロックするネットワーク セキュリティ グループの作成

```java
// this NSG definition block traffic to and from the public Internet
NetworkSecurityGroup backEndSubnetNsg = azure.networkSecurityGroups()
              .define(vnet1BackEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .defineRule("DenyInternetInComing")
                        .denyInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

この[ネットワーク セキュリティ規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)は、受信方向と送信方向のパブリック インターネット トラフィックをどちらもブロックします。 このネットワーク セキュリティ グループは、仮想ネットワーク内のサブネットに適用するまで作用しません。

## <a name="create-a-virtual-network-with-two-subnets"></a>2 つのサブネットから成る仮想ネットワークの作成

```java
// create the a virtual network with two subnets
// assign the backend subnet a rule blocking public internet traffic
Network virtualNetwork1 = azure.networks().define(vnetName1)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withAddressSpace("192.168.0.0/16")
                    .withSubnet(vnet1FrontEndSubnetName, "192.168.1.0/24")
                    .defineSubnet(vnet1BackEndSubnetName)
                        .withAddressPrefix("192.168.2.0/24")
                        .withExistingNetworkSecurityGroup(backEndSubnetNsg)
                        .attach()
                    .create();
```

バックエンド サブネットは、ネットワーク セキュリティ グループに設定された規則に従ってインターネット アクセスを拒否します。 フロント エンド サブネットには、インターネットへの送信トラフィックを許可する[既定の規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)が使用されます。

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a>受信方向の HTTP トラフィックを許可するネットワーク セキュリティ グループの作成
```java
// create a rule that allows inbound HTTP and blocks outbound Internet traffic
NetworkSecurityGroup frontEndSubnetNsg = azure.networkSecurityGroups()
               .define(vnet1FrontEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .defineRule("AllowHttpInComing")
                        .allowInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toPort(80)
                        .withProtocol(SecurityRuleProtocol.TCP)
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

このネットワーク セキュリティ規則は、パブリック インターネットからの受信トラフィックをポート 80 で開放し、ネットワーク内からパブリック インターネットに向かう送信方向のトラフィックはすべてブロックします。 

## <a name="update-a-virtual-network"></a>仮想ネットワークの更新
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

前の手順で作成したネットワーク セキュリティ規則を使用し、受信方向の HTTP トラフィックを許可するようにフロント エンド サブネットを更新します。

## <a name="create-a-virtual-machine-on-a-subnet"></a>サブネットへの仮想マシンの作成
```java
// attach the new VM to the front end subnet on the virtual network
VirtualMachine frontEndVM = azure.virtualMachines().define(frontEndVmName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withExistingPrimaryNetwork(virtualNetwork1) 
                    .withSubnet(vnet1FrontEndSubnetName)
                    .withPrimaryPrivateIpAddressDynamic()
                    .withNewPrimaryPublicIpAddress(publicIpAddressLeafDnsForFrontEndVm)
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();
```

`withExistingPrimaryNetwork()` と `withSubnet()` では、前の手順で作成した仮想ネットワークのフロントエンド サブネットを使用するように仮想マシンを構成しています。

## <a name="list-virtual-networks-in-a-resource-group"></a>リソース グループに存在する仮想ネットワークの列挙
```java
// iterate over every virtual network in the resource group 
for (Network virtualNetwork : azure.networks().listByResourceGroup(rgName)) {
    // for each subnet on the virtual network, log the network address prefix 
    for (Map.Entry<String, Subnet> entry : virtualNetwork.subnets().entrySet()) {
        String subnetName = entry.getKey();
        Subnet subnet = entry.getValue();
        System.out.println("Address prefix for subnet " + subnetName + 
             " is " + subnet.addressPrefix());
    }
}
```       

外側のコレクションを使って `Network` オブジェクトを列挙しながら調べるか、この例のように for-each ループを入れ子にして、ネットワークごとにそれぞれの子リソースを反復処理することができます。

## <a name="delete-a-virtual-network"></a>仮想ネットワークの削除
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

仮想ネットワークを削除すると、そのネットワーク上のサブネットも削除されます。ただし、そのサブネットに適用されていたネットワーク セキュリティ グループの規則は削除されません。 その定義を他のサブネットに再度適用することができます。

## <a name="sample-explanation"></a>サンプルの説明

このサンプルでは、それぞれ 1 つの仮想マシンを含んだ 2 つのサブネットから成る仮想ネットワークを作成します。 バック サブネットは、パブリック インターネットから切り離します。 外部に公開されたフロント サブネットは、インターネットからの受信方向の HTTP トラフィックを受け入れます。 仮想ネットワーク内の 2 つの仮想マシンは、既定のネットワーク セキュリティ グループの規則に従って互いに通信を行います。

| サンプルで使われているクラス | メモ
|-------|-------|
| [ネットワーク](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | `azure.networks().define()...create()` から作成された仮想ネットワークのローカル オブジェクトを表します。 `update()...apply()` という fluent チェーンを使用して、既存の仮想ネットワークを更新します。
| [サブネット](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | 仮想ネットワーク上のサブネットは、そのネットワークを定義または更新するときに、`withSubnet()` を使って作成します。 サブネットのオブジェクト表現は、`Network.subnets().get()` または `Network.subnets().entrySet()` から取得します。 これらのオブジェクトには、サブネットのプロパティを照会するためのメソッドが用意されています。
| [NetworkSecurityGroup](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | `azure.networkSecurityGroups().define()...create()` という fluent チェーンを使って作成され、仮想ネットワーク内のサブネットを更新または作成するときにサブネットに適用されます。 

## <a name="next-steps"></a>次のステップ

[!INCLUDE [next-steps](includes/java-next-steps.md)]