---
title: "仮想マシン スケール セットを Java で管理する | Microsoft Docs"
description: "Azure SDK for Java を使って Azure 仮想マシン スケール セットを管理するためのサンプル コード"
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 4653726b387369c18942b6c11392f15b9f0351f3
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a><span data-ttu-id="ecf63-103">Azure 仮想マシン スケール セットを Java アプリケーションから管理する</span><span class="sxs-lookup"><span data-stu-id="ecf63-103">Manage Azure virtual machine scale sets from your Java applications</span></span>

<span data-ttu-id="ecf63-104">[このサンプル](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets)では、[Java 管理ライブラリ](https://github.com/Azure/azure-sdk-for-java)を使って[仮想マシン スケール セット](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview)を作成します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets) creates a  [virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="ecf63-105">サンプルの実行</span><span class="sxs-lookup"><span data-stu-id="ecf63-105">Run the sample</span></span>

<span data-ttu-id="ecf63-106">[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="ecf63-107">次に、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

<span data-ttu-id="ecf63-108">[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ecf63-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="ecf63-109">Azure での認証</span><span class="sxs-lookup"><span data-stu-id="ecf63-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a><span data-ttu-id="ecf63-110">スケール セットに使用する仮想ネットワークの作成</span><span class="sxs-lookup"><span data-stu-id="ecf63-110">Create a virtual network for the scale set</span></span>

```java
Network network = azure.networks().define(vnetName)
                    .withRegion(region)
                    .withNewResourceGroup(rgName)
                    .withAddressSpace("172.16.0.0/16")
                    .defineSubnet("Front-end")
                    .withAddressPrefix("172.16.1.0/24")
                    .attach()
                    .create();
```

<span data-ttu-id="ecf63-111">仮想ネットワークとロード バランサーは、スケール セットの定義を作成する前に設定します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-111">Set up a virtual network and load balancer before creating the scale set definition.</span></span> <span data-ttu-id="ecf63-112">スケール セットの初期構成に、これらのリソースが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-112">The scale set uses these resources for its initial configuration.</span></span>

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a><span data-ttu-id="ecf63-113">スケール セット全体に負荷を分散するためのロード バランサーの作成</span><span class="sxs-lookup"><span data-stu-id="ecf63-113">Create a load balancer to distribute load across the scale set</span></span>

```java
LoadBalancer loadBalancer1 = azure.loadBalancers().define(loadBalancerName1)
                    .withRegion(region)
                    .withExistingResourceGroup(rgName)
                    // assign a public IP address to the load balancer
                    .definePublicFrontend(frontendName)
                        .withExistingPublicIPAddress(publicIPAddress)
                        .attach()
                    // Add two backend address pools
                    .defineBackend(backendPoolName1)
                        .attach()
                    .defineBackend(backendPoolName2)
                        .attach()
                    // Add two health probes on 80 and 443
                    .defineHttpProbe(httpProbe)
                        .withRequestPath("/")
                        .withPort(80)
                        .attach()
                    .defineHttpProbe(httpsProbe)
                        .withRequestPath("/")
                        .withPort(443)
                        .attach()

                    // balance HTTP and HTTPs traffic
                    .defineLoadBalancingRule(httpLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(80)
                        .withProbe(httpProbe)
                        .withBackend(backendPoolName1)
                        .attach()
                    .defineLoadBalancingRule(httpsLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(443)
                        .withProbe(httpsProbe)
                        .withBackend(backendPoolName2)
                        .attach()

                    // Add NAT definitions to enable SSH and telnet to the VMs 
                    .defineInboundNatPool(natPool50XXto22)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(5000, 5099)
                        .withBackendPort(22)
                        .attach()
                    .defineInboundNatPool(natPool60XXto23)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(6000, 6099)
                        .withBackendPort(23)
                        .attach()
                    .create();
```

 <span data-ttu-id="ecf63-114">このロード バランサーには、2 つのバックエンド ネットワーク アドレス プールが定義されています。1 つは HTTP の負荷を分散するためのプール (`backendPoolName1`)、もう 1 つは HTTPS の負荷を分散するためのプール (`backendPoolName2`) です。</span><span class="sxs-lookup"><span data-stu-id="ecf63-114">The load balancer defines two backend network address pools-one to balance load across HTTP (`backendPoolName1`) and the other to balance load across HTTPS (`backendPoolName2`).</span></span>  <span data-ttu-id="ecf63-115">ロード バランサーには、`defineHttpProbe()` メソッドによって[正常性プローブのエンドポイント](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)を設定します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-115">The `defineHttpProbe()` methods set up [health probe endpoints](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) on the load balancers.</span></span> <span data-ttu-id="ecf63-116">スケール セットの仮想マシンのポート 22 とポート 23 は、telnet アクセスと SSH アクセス用に NAT ルールで公開しています。</span><span class="sxs-lookup"><span data-stu-id="ecf63-116">NAT rules expose ports 22 and 23 on the scale set virtual machines for telnet and SSH access.</span></span>

## <a name="create-a-scale-set"></a><span data-ttu-id="ecf63-117">スケール セットを作成する</span><span class="sxs-lookup"><span data-stu-id="ecf63-117">Create a scale set</span></span>
 
```java
 // Create a virtual machine scale set with three virtual machines
 // And, install Apache Web servers on them
VirtualMachineScaleSet virtualMachineScaleSet = azure.virtualMachineScaleSets()
       .define(vmssName)
                .withRegion(region)
                .withExistingResourceGroup(rgName)
                .withSku(VirtualMachineScaleSetSkuTypes.STANDARD_D3_V2)
                .withExistingPrimaryNetworkSubnet(network, "Front-end")
                .withExistingPrimaryInternetFacingLoadBalancer(loadBalancer1)
                .withPrimaryInternetFacingLoadBalancerBackends(backendPoolName1, backendPoolName2)
                .withPrimaryInternetFacingLoadBalancerInboundNatPools(natPool50XXto22, natPool60XXto23)
                .withoutPrimaryInternalLoadBalancer()
                .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                .withRootUsername(userName)
                .withSsh(sshKey)
                .withNewDataDisk(100)
                .withNewDataDisk(100, 1, CachingTypes.READ_WRITE)
                .withNewDataDisk(100, 2, 
                     CachingTypes.READ_WRITE, StorageAccountTypes.STANDARD_LRS)
                .withCapacity(3)
                // Use a VM extension to install Apache Web servers
                .defineNewExtension("CustomScriptForLinux")
                    .withPublisher("Microsoft.OSTCExtensions")
                    .withType("CustomScriptForLinux")
                    .withVersion("1.4")
                    .withMinorVersionAutoUpgrade()
                    .withPublicSetting("fileUris", fileUris)
                    .withPublicSetting("commandToExecute", installCommand)
                    .attach()
                .create();
```

<span data-ttu-id="ecf63-118">前の手順で作成した仮想ネットワークの定義とロード バランサーの定義を使用して、それぞれ 3 つの Linux インスタンス (`withCapacity(3)`) と 3 つの 100 GB データ ディスクから成るスケール セットを作成します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-118">Use the virtual network definition and load balancer definitions created in the previous step to create a scale set with three Linux instances (`withCapacity(3)`) and three 100GB data disks each.</span></span> <span data-ttu-id="ecf63-119">それぞれの VM には、`defineNewExtension()` メソッド チェーンによって Apache Web サーバーがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-119">The `defineNewExtension()` method chain installs the Apache web server on each VM.</span></span>

## <a name="list-virtual-machine-scale-set-network-interfaces"></a><span data-ttu-id="ecf63-120">仮想マシン スケール セットに存在するネットワーク インターフェイスの列挙</span><span class="sxs-lookup"><span data-stu-id="ecf63-120">List virtual machine scale set network interfaces</span></span>

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a><span data-ttu-id="ecf63-121">各スケール セット仮想マシンの SSH 接続文字列の取得</span><span class="sxs-lookup"><span data-stu-id="ecf63-121">Get SSH connection strings for each scale set virtual machine</span></span>

```java
for (VirtualMachineScaleSetVM instance : virtualMachineScaleSet.virtualMachines().list()) {
    System.out.println("Scale set virtual machine instance #" + instance.instanceId());
    System.out.println(instance.id());
    PagedList<VirtualMachineScaleSetNetworkInterface> networkInterfaces = 
         instance.listNetworkInterfaces();
    // Pick the first NIC on the instance and use its primary IP address
    VirtualMachineScaleSetNetworkInterface networkInterface = networkInterfaces.get(0);
    for (VirtualMachineScaleSetNicIPConfiguration ipConfig : networkInterface.ipConfigurations().values()) {
        if (ipConfig.isPrimary()) {
            List<LoadBalancerInboundNatRule> natRules = ipConfig.listAssociatedLoadBalancerInboundNatRules();
            for (LoadBalancerInboundNatRule natRule : natRules) {
                // find rule matching the inbound SSH port on the backend for the IP address
                // and return the SSH connection string to that port on the load balancer
                if (natRule.backendPort() == 22) {
                    System.out.println("SSH connection string: " + userName + 
                        "@" + publicIPAddress.fqdn() + ":" + natRule.frontendPort());
                break;
                }
            }
            break;
        }
    }
}
```

<span data-ttu-id="ecf63-122">仮想マシンの SSH ポート (22) と telnet ポート (23) は、先に作成しておいた NAT プールによってロード バランサー上のポートにマップされます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-122">The NAT pool created earlier mapped the SSH and telnet ports (22 and 23, respectively) on the virtual machines to ports on the load balancer.</span></span> <span data-ttu-id="ecf63-123">このコードによって、各仮想マシンの SSH 接続文字列が作成されます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-123">This code builds the SSH connection string for each virtual machine.</span></span>

## <a name="stop-the-virtual-machine-scale-set"></a><span data-ttu-id="ecf63-124">仮想マシン スケール セットの停止</span><span class="sxs-lookup"><span data-stu-id="ecf63-124">Stop the virtual machine scale set</span></span>

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

<span data-ttu-id="ecf63-125">仮想マシンに予約されたリソースは、その仮想マシンを停止しても消費され続けます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-125">Stopped virtual machines continue to consume reserved resources.</span></span> <span data-ttu-id="ecf63-126">仮想マシン上のオペレーティング システムを `deallocate()` で停止して、コンピューティング リソースを解放してください。</span><span class="sxs-lookup"><span data-stu-id="ecf63-126">Use `deallocate()` to stop the operating system on the virtual machines and release their compute resources.</span></span>

## <a name="deallocate-the-virtual-machine-scale-set"></a><span data-ttu-id="ecf63-127">仮想マシン スケール セットの割り当て解除</span><span class="sxs-lookup"><span data-stu-id="ecf63-127">Deallocate the virtual machine scale set</span></span>

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

<span data-ttu-id="ecf63-128">Deallocate() を実行すると、仮想マシン上のオペレーティング システムがシャットダウンされ、スケール セット インスタンスで使用されているコンピューティング リソースとネットワーク リソース (IP アドレスなど) が解放されます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-128">Deallocate() shuts down the operating system on the virtual machines and frees up the compute and network resources (such as IP addresses) used by the scale set instances.</span></span> <span data-ttu-id="ecf63-129">その後も、仮想マシンにアタッチされているすべてのディスク (OS も含む) のストレージ料金が課金されます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-129">You continue to accrue storage charges for any disks (including the OS) attached to the virtual machines.</span></span>

## <a name="start-a-virtual-machine-scale-set"></a><span data-ttu-id="ecf63-130">仮想マシン スケール セットの起動</span><span class="sxs-lookup"><span data-stu-id="ecf63-130">Start a virtual machine scale set</span></span>

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a><span data-ttu-id="ecf63-131">スケール セットに含まれる仮想マシンのインスタンス数の更新</span><span class="sxs-lookup"><span data-stu-id="ecf63-131">Update the number of virtual machines instances in the scale set</span></span>
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

<span data-ttu-id="ecf63-132">スケール セットに含まれる仮想マシンの数をスケーリングするには `withCapacity()` を使用します。また、それぞれの仮想マシンのキャパシティをスケーリングするには、`withSku()` を使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-132">Scale the number of virtual machines in the scale set using `withCapacity()` and scale the capacity of each virtual machine using `withSku()`.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="ecf63-133">サンプルの説明</span><span class="sxs-lookup"><span data-stu-id="ecf63-133">Sample explanation</span></span>

<span data-ttu-id="ecf63-134">[このサンプル コード](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)ではまず、スケール セットの仮想マシン間の通信に使用する仮想ネットワークを作成し、トラフィックを仮想マシン間で分散するためのロード バランサーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-134">[The sample code](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) first creates a virtual network for the scale set to communicate across and a load balancer to distribute traffic across the virtual machines.</span></span> <span data-ttu-id="ecf63-135">Apache Web サーバーを実行する 3 つの Linux インスタンスから成るスケール セットが、`azure.virtualMachineScaleSets().define()...create()` メソッド チェーンによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-135">The `azure.virtualMachineScaleSets().define()...create()` method chain creates the scale set with three Linux instances running the Apache web server.</span></span>    
   
| <span data-ttu-id="ecf63-136">サンプルで使われているクラス</span><span class="sxs-lookup"><span data-stu-id="ecf63-136">Class used in sample</span></span> | <span data-ttu-id="ecf63-137">メモ</span><span class="sxs-lookup"><span data-stu-id="ecf63-137">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="ecf63-138">VirtualMachineScaleSet</span><span class="sxs-lookup"><span data-stu-id="ecf63-138">VirtualMachineScaleSet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | <span data-ttu-id="ecf63-139">スケール セットに含まれるすべての仮想マシンを対象に、照会、起動、停止、更新、削除を実行します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-139">Query, start, stop, update and delete all virtual machines in the scale set.</span></span>
| [<span data-ttu-id="ecf63-140">VirtualMachineScaleSetVM</span><span class="sxs-lookup"><span data-stu-id="ecf63-140">VirtualMachineScaleSetVM</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | <span data-ttu-id="ecf63-141">`virtualMachineScaleSet.virtualMachines().get()` または `list()` から取得して、スケール セットの仮想マシンを対象に、照会、起動、停止、構成、削除を実行できます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-141">Retrieved from `virtualMachineScaleSet.virtualMachines().get()` or `list()`, allows you to query, start, stop, configure and delete virtual machines in the scale set.</span></span>
| [<span data-ttu-id="ecf63-142">VirtualMachineScaleSetNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="ecf63-142">VirtualMachineScaleSetNetworkInterface</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | <span data-ttu-id="ecf63-143">スケール セットに含まれる仮想マシンの読み取り専用のネットワーク インターフェイスを表します。`virtualMachineScaleSet.listNetworkInterfaces()` から返されます。</span><span class="sxs-lookup"><span data-stu-id="ecf63-143">Returned from `virtualMachineScaleSet.listNetworkInterfaces()`, read-only representation of a network interface on a virtual machine in a scale set.</span></span>
| [<span data-ttu-id="ecf63-144">VirtualMachineScaleSetSkuTypes</span><span class="sxs-lookup"><span data-stu-id="ecf63-144">VirtualMachineScaleSetSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | <span data-ttu-id="ecf63-145">スケール セットのメンバーが利用できるリソースの量を定義する[仮想マシン スケール セットのレベル](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/)を設定するための静的フィールドのクラスです。</span><span class="sxs-lookup"><span data-stu-id="ecf63-145">Class of static fields used to set the [virtual machine scale set tier](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) used to define how much resources scale set members can consume.</span></span>
| [<span data-ttu-id="ecf63-146">VirtualMachineScaleSetNicIpConfiguration</span><span class="sxs-lookup"><span data-stu-id="ecf63-146">VirtualMachineScaleSetNicIpConfiguration</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | <span data-ttu-id="ecf63-147">スケール セットの仮想マシン上のネットワーク インターフェイスに関連付けられている IP 構成を照会する際に使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf63-147">Used to query the IP configuration associated with a network interface on a scale set virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecf63-148">次のステップ</span><span class="sxs-lookup"><span data-stu-id="ecf63-148">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]