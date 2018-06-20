---
title: 仮想マシン スケール セットを Java で管理する | Microsoft Docs
description: Azure SDK for Java を使って Azure 仮想マシン スケール セットを管理するためのサンプル コード
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
ms.locfileid: "21931158"
---
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a>Azure 仮想マシン スケール セットを Java アプリケーションから管理する

[このサンプル](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets)では、[Java 管理ライブラリ](https://github.com/Azure/azure-sdk-for-java)を使って[仮想マシン スケール セット](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview)を作成します。 

## <a name="run-the-sample"></a>サンプルの実行

[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。 次に、以下を実行します。

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) を参照してください。

## <a name="authenticate-with-azure"></a>Azure での認証

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a>スケール セットに使用する仮想ネットワークの作成

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

仮想ネットワークとロード バランサーは、スケール セットの定義を作成する前に設定します。 スケール セットの初期構成に、これらのリソースが使用されます。

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a>スケール セット全体に負荷を分散するためのロード バランサーの作成

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

 このロード バランサーには、2 つのバックエンド ネットワーク アドレス プールが定義されています。1 つは HTTP の負荷を分散するためのプール (`backendPoolName1`)、もう 1 つは HTTPS の負荷を分散するためのプール (`backendPoolName2`) です。  ロード バランサーには、`defineHttpProbe()` メソッドによって[正常性プローブのエンドポイント](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)を設定します。 スケール セットの仮想マシンのポート 22 とポート 23 は、telnet アクセスと SSH アクセス用に NAT ルールで公開しています。

## <a name="create-a-scale-set"></a>スケール セットを作成する
 
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

前の手順で作成した仮想ネットワークの定義とロード バランサーの定義を使用して、それぞれ 3 つの Linux インスタンス (`withCapacity(3)`) と 3 つの 100 GB データ ディスクから成るスケール セットを作成します。 それぞれの VM には、`defineNewExtension()` メソッド チェーンによって Apache Web サーバーがインストールされます。

## <a name="list-virtual-machine-scale-set-network-interfaces"></a>仮想マシン スケール セットに存在するネットワーク インターフェイスの列挙

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a>各スケール セット仮想マシンの SSH 接続文字列の取得

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

仮想マシンの SSH ポート (22) と telnet ポート (23) は、先に作成しておいた NAT プールによってロード バランサー上のポートにマップされます。 このコードによって、各仮想マシンの SSH 接続文字列が作成されます。

## <a name="stop-the-virtual-machine-scale-set"></a>仮想マシン スケール セットの停止

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

仮想マシンに予約されたリソースは、その仮想マシンを停止しても消費され続けます。 仮想マシン上のオペレーティング システムを `deallocate()` で停止して、コンピューティング リソースを解放してください。

## <a name="deallocate-the-virtual-machine-scale-set"></a>仮想マシン スケール セットの割り当て解除

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

Deallocate() を実行すると、仮想マシン上のオペレーティング システムがシャットダウンされ、スケール セット インスタンスで使用されているコンピューティング リソースとネットワーク リソース (IP アドレスなど) が解放されます。 その後も、仮想マシンにアタッチされているすべてのディスク (OS も含む) のストレージ料金が課金されます。

## <a name="start-a-virtual-machine-scale-set"></a>仮想マシン スケール セットの起動

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a>スケール セットに含まれる仮想マシンのインスタンス数の更新
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

スケール セットに含まれる仮想マシンの数をスケーリングするには `withCapacity()` を使用します。また、それぞれの仮想マシンのキャパシティをスケーリングするには、`withSku()` を使用します。

## <a name="sample-explanation"></a>サンプルの説明

[このサンプル コード](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)ではまず、スケール セットの仮想マシン間の通信に使用する仮想ネットワークを作成し、トラフィックを仮想マシン間で分散するためのロード バランサーを作成します。 Apache Web サーバーを実行する 3 つの Linux インスタンスから成るスケール セットが、`azure.virtualMachineScaleSets().define()...create()` メソッド チェーンによって作成されます。    
   
| サンプルで使われているクラス | メモ
|-------|-------|
| [VirtualMachineScaleSet](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | スケール セットに含まれるすべての仮想マシンを対象に、照会、起動、停止、更新、削除を実行します。
| [VirtualMachineScaleSetVM](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | `virtualMachineScaleSet.virtualMachines().get()` または `list()` から取得して、スケール セットの仮想マシンを対象に、照会、起動、停止、構成、削除を実行できます。
| [VirtualMachineScaleSetNetworkInterface](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | スケール セットに含まれる仮想マシンの読み取り専用のネットワーク インターフェイスを表します。`virtualMachineScaleSet.listNetworkInterfaces()` から返されます。
| [VirtualMachineScaleSetSkuTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | スケール セットのメンバーが利用できるリソースの量を定義する[仮想マシン スケール セットのレベル](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/)を設定するための静的フィールドのクラスです。
| [VirtualMachineScaleSetNicIpConfiguration](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | スケール セットの仮想マシン上のネットワーク インターフェイスに関連付けられている IP 構成を照会する際に使用します。

## <a name="next-steps"></a>次のステップ

[!INCLUDE [next-steps](includes/java-next-steps.md)]