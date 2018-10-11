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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892543"
---
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a><span data-ttu-id="5764e-103">Java アプリによる Azure 仮想ネットワークの作成と管理</span><span class="sxs-lookup"><span data-stu-id="5764e-103">Create and manage Azure virtual networks from your Java apps</span></span>

<span data-ttu-id="5764e-104">[このサンプル](https://github.com/Azure-Samples/network-java-manage-virtual-network)では、管理対象となるネットワーク セグメントに対し、Azure リソースを分離する[仮想ネットワーク](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)を作成します。</span><span class="sxs-lookup"><span data-stu-id="5764e-104">[This sample](https://github.com/Azure-Samples/network-java-manage-virtual-network) creates a [virtual network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) to isolate your Azure resources on network segment you control.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="5764e-105">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="5764e-105">Run the sample</span></span>

<span data-ttu-id="5764e-106">[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="5764e-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="5764e-107">次に、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="5764e-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

<span data-ttu-id="5764e-108">[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5764e-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="5764e-109">Azure での認証</span><span class="sxs-lookup"><span data-stu-id="5764e-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a><span data-ttu-id="5764e-110">インターネット トラフィックをブロックするネットワーク セキュリティ グループの作成</span><span class="sxs-lookup"><span data-stu-id="5764e-110">Create a network security group to block Internet traffic</span></span>

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

<span data-ttu-id="5764e-111">この[ネットワーク セキュリティ規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)は、受信方向と送信方向のパブリック インターネット トラフィックをどちらもブロックします。</span><span class="sxs-lookup"><span data-stu-id="5764e-111">This [network security rule](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) blocks both inbound and outbound public Internet traffic.</span></span> <span data-ttu-id="5764e-112">このネットワーク セキュリティ グループは、仮想ネットワーク内のサブネットに適用するまで作用しません。</span><span class="sxs-lookup"><span data-stu-id="5764e-112">This network security group will not have an effect until applied to a subnet in your virtual network.</span></span>

## <a name="create-a-virtual-network-with-two-subnets"></a><span data-ttu-id="5764e-113">2 つのサブネットから成る仮想ネットワークの作成</span><span class="sxs-lookup"><span data-stu-id="5764e-113">Create a virtual network with two subnets</span></span>

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

<span data-ttu-id="5764e-114">バックエンド サブネットは、ネットワーク セキュリティ グループに設定された規則に従ってインターネット アクセスを拒否します。</span><span class="sxs-lookup"><span data-stu-id="5764e-114">The backend subnet denies Internet access usingfollowing the rules set in the network security group.</span></span> <span data-ttu-id="5764e-115">フロント エンド サブネットには、インターネットへの送信トラフィックを許可する[既定の規則](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)が使用されます。</span><span class="sxs-lookup"><span data-stu-id="5764e-115">The front end subnet uses the [default rules](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) which allow outbound traffic to the Internet.</span></span>

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a><span data-ttu-id="5764e-116">受信方向の HTTP トラフィックを許可するネットワーク セキュリティ グループの作成</span><span class="sxs-lookup"><span data-stu-id="5764e-116">Create a network security group to allow inbound HTTP traffic</span></span>
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

<span data-ttu-id="5764e-117">このネットワーク セキュリティ規則は、パブリック インターネットからの受信トラフィックをポート 80 で開放し、ネットワーク内からパブリック インターネットに向かう送信方向のトラフィックはすべてブロックします。</span><span class="sxs-lookup"><span data-stu-id="5764e-117">This network security rule opens up inbound traffic on port 80 from the public Internet, and blocks all outbound traffic from inside the network to the public Internet.</span></span> 

## <a name="update-a-virtual-network"></a><span data-ttu-id="5764e-118">仮想ネットワークの更新</span><span class="sxs-lookup"><span data-stu-id="5764e-118">Update a virtual network</span></span>
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

<span data-ttu-id="5764e-119">前の手順で作成したネットワーク セキュリティ規則を使用し、受信方向の HTTP トラフィックを許可するようにフロント エンド サブネットを更新します。</span><span class="sxs-lookup"><span data-stu-id="5764e-119">Update the front end subnet to allow inbound HTTP traffic using the network security rule created in the previous step.</span></span>

## <a name="create-a-virtual-machine-on-a-subnet"></a><span data-ttu-id="5764e-120">サブネットへの仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="5764e-120">Create a virtual machine on a subnet</span></span>
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

<span data-ttu-id="5764e-121">`withExistingPrimaryNetwork()` と `withSubnet()` では、前の手順で作成した仮想ネットワークのフロントエンド サブネットを使用するように仮想マシンを構成しています。</span><span class="sxs-lookup"><span data-stu-id="5764e-121">`withExistingPrimaryNetwork()` and `withSubnet()` configure the virtual machine to use the front-end subnet on the virtual network created in the previous steps.</span></span>

## <a name="list-virtual-networks-in-a-resource-group"></a><span data-ttu-id="5764e-122">リソース グループに存在する仮想ネットワークの列挙</span><span class="sxs-lookup"><span data-stu-id="5764e-122">List virtual networks in a resource group</span></span>
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

<span data-ttu-id="5764e-123">外側のコレクションを使って `Network` オブジェクトを列挙しながら調べるか、この例のように for-each ループを入れ子にして、ネットワークごとにそれぞれの子リソースを反復処理することができます。</span><span class="sxs-lookup"><span data-stu-id="5764e-123">You can list and inspect `Network` object using the outer collection or iterate through each child resource for each network using the nested for-each loop as seen in this example.</span></span>

## <a name="delete-a-virtual-network"></a><span data-ttu-id="5764e-124">仮想ネットワークの削除</span><span class="sxs-lookup"><span data-stu-id="5764e-124">Delete a virtual network</span></span>
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

<span data-ttu-id="5764e-125">仮想ネットワークを削除すると、そのネットワーク上のサブネットも削除されます。ただし、そのサブネットに適用されていたネットワーク セキュリティ グループの規則は削除されません。</span><span class="sxs-lookup"><span data-stu-id="5764e-125">Removing a virtual network deletes the subnets on the network but does not delete the network security group rules applied to the subnets.</span></span> <span data-ttu-id="5764e-126">その定義を他のサブネットに再度適用することができます。</span><span class="sxs-lookup"><span data-stu-id="5764e-126">Those definitions can be reapplied to other subnets.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="5764e-127">サンプルの説明</span><span class="sxs-lookup"><span data-stu-id="5764e-127">Sample explanation</span></span>

<span data-ttu-id="5764e-128">このサンプルでは、それぞれ 1 つの仮想マシンを含んだ 2 つのサブネットから成る仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="5764e-128">This sample creates a virtual network with two subnets and with one virtual machine on each subnet.</span></span> <span data-ttu-id="5764e-129">バック サブネットは、パブリック インターネットから切り離します。</span><span class="sxs-lookup"><span data-stu-id="5764e-129">The back subnet is cut off from the public Internet.</span></span> <span data-ttu-id="5764e-130">外部に公開されたフロント サブネットは、インターネットからの受信方向の HTTP トラフィックを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="5764e-130">The front-facing subnet accepts inbound HTTP traffic from the Internet.</span></span> <span data-ttu-id="5764e-131">仮想ネットワーク内の 2 つの仮想マシンは、既定のネットワーク セキュリティ グループの規則に従って互いに通信を行います。</span><span class="sxs-lookup"><span data-stu-id="5764e-131">Both virtual machines in the virtual network communicate with each other through the default network security group rules.</span></span>

| <span data-ttu-id="5764e-132">サンプルで使われているクラス</span><span class="sxs-lookup"><span data-stu-id="5764e-132">Class used in sample</span></span> | <span data-ttu-id="5764e-133">メモ</span><span class="sxs-lookup"><span data-stu-id="5764e-133">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="5764e-134">ネットワーク</span><span class="sxs-lookup"><span data-stu-id="5764e-134">Network</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | <span data-ttu-id="5764e-135">`azure.networks().define()...create()` から作成された仮想ネットワークのローカル オブジェクトを表します。</span><span class="sxs-lookup"><span data-stu-id="5764e-135">Local object representation of the virtual network created from `azure.networks().define()...create()` .</span></span> <span data-ttu-id="5764e-136">`update()...apply()` という fluent チェーンを使用して、既存の仮想ネットワークを更新します。</span><span class="sxs-lookup"><span data-stu-id="5764e-136">Use the `update()...apply()` fluent chain to update an existing virtual network.</span></span>
| [<span data-ttu-id="5764e-137">サブネット</span><span class="sxs-lookup"><span data-stu-id="5764e-137">Subnet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | <span data-ttu-id="5764e-138">仮想ネットワーク上のサブネットは、そのネットワークを定義または更新するときに、`withSubnet()` を使って作成します。</span><span class="sxs-lookup"><span data-stu-id="5764e-138">Create subnets on the virtual network when defining or updating the network using `withSubnet()`.</span></span> <span data-ttu-id="5764e-139">サブネットのオブジェクト表現は、`Network.subnets().get()` または `Network.subnets().entrySet()` から取得します。</span><span class="sxs-lookup"><span data-stu-id="5764e-139">Get object representations of a subnet from `Network.subnets().get()` or `Network.subnets().entrySet()`.</span></span> <span data-ttu-id="5764e-140">これらのオブジェクトには、サブネットのプロパティを照会するためのメソッドが用意されています。</span><span class="sxs-lookup"><span data-stu-id="5764e-140">These objects have methods to query subnet properties.</span></span>
| [<span data-ttu-id="5764e-141">NetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="5764e-141">NetworkSecurityGroup</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | <span data-ttu-id="5764e-142">`azure.networkSecurityGroups().define()...create()` という fluent チェーンを使って作成され、仮想ネットワーク内のサブネットを更新または作成するときにサブネットに適用されます。</span><span class="sxs-lookup"><span data-stu-id="5764e-142">Created using the `azure.networkSecurityGroups().define()...create()` fluent chain and then applied to subnets through the updating or creating subnets in a virtual network.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5764e-143">次の手順</span><span class="sxs-lookup"><span data-stu-id="5764e-143">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]