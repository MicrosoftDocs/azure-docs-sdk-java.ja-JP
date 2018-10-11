---
title: 複数リージョンに対して同時に VM を作成する | Microsoft Docs
description: Azure SDK for Java を使って複数の Azure リージョンに対して同時に仮想マシンを作成するためのサンプル コード
author: rloutlaw
manager: douge
ms.assetid: e5a36699-2d96-4571-84f9-a6af13f3c067
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: e20feb555c3a360eceae60c1569af9a00a5cd027
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893213"
---
# <a name="create-virtual-machines-across-multiple-regions-from-your-java-applications"></a>Java アプリケーションから複数のリージョンに対して仮想マシンを作成する

[このサンプル](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)では、[Azure Management Libraries for Java](https://github.com/Azure/azure-sdk-for-java) を使って複数の Azure リージョンに対して同時に仮想マシンを作成します。

> [!IMPORTANT]
> 4 つのリージョンに対し、[STANDARD_DS3_V2 サイズ](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes)の Ubuntu 16.04 LTS を実行する合計 48 の VM を作成します。 これらの仮想マシンは、終了前にサンプル コードによって削除されます。 このサンプルを既定の VM 数で実行する前に必ず、[サービスの制限とクォータをチェック](http://docs.microsoft.com/azure/azure-subscription-service-limits)してください。

## <a name="run-the-sample"></a>サンプルを実行する

[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。 次に、以下を実行します。

```
git clone https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel.git
cd compute-java-create-virtual-machines-across-regions-in-parallel
mvn clean compile exec:java
```

[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java) を参照してください。

## <a name="authenticate-with-azure"></a>Azure での認証

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="set-locations-and-counts-for-the-virtual-machines"></a>仮想マシンの場所と数の設定

```java
// use a Map to define how where and how many VMs to create 
Map<Region, Integer> virtualMachinesByLocation = new HashMap<Region, Integer>();

// create 12 virtual machines in four regions
virtualMachinesByLocation.put(Region.US_EAST, 12);
virtualMachinesByLocation.put(Region.US_SOUTH_CENTRAL, 12);
virtualMachinesByLocation.put(Region.US_WEST, 12);
virtualMachinesByLocation.put(Region.US_NORTH_CENTRAL, 12);
```

この `Map` は、後で各リージョンへの VM の分配を設定するためにサンプルの中で使用します。

## <a name="create-a-resource-group"></a>リソース グループの作成 

```java
// logically associate the resources in the sample into a randomly named resource group
final String rgName = SdkContext.randomResourceName("rgCOPD", 24);
ResourceGroup resourceGroup = azure.resourceGroups().define(rgName)
                .withRegion(Region.US_EAST)
                .create();
```

サンプルで使う各リソースは、このリソース グループによって管理されます。 そのため後でリソース グループを削除すれば、簡単にリソースをクリーンアップすることができます。

## <a name="define-the-virtual-machines"></a>仮想マシンの定義
```java
// list to store the VirtualMachine definitions
List<Creatable<VirtualMachine>> creatableVirtualMachines = new ArrayList<>();
    
// outer loop: iterate through each region included in the map
for (Map.Entry<Region, Integer> entry : virtualMachinesByLocation.entrySet()) {
    Region region = entry.getKey();
    Integer vmCount = entry.getValue();

    // Define one virtual network Creatable per region for the VMs to share
    String networkName = SdkContext.randomResourceName("vnetCOPD-", 20);
    Creatable<Network> networkCreatable = azure.networks().define(networkName)
           .withRegion(region)
           .withExistingResourceGroup(resourceGroup)
           .withAddressSpace("172.16.0.0/16");

    // Define one storage account Creatable per region for storing VM disks
    String storageAccountName = SdkContext.randomResourceName("stgcopd", 20);
    Creatable<StorageAccount> storageAccountCreatable = azure.storageAccounts()
        .define(storageAccountName)
              .withRegion(region)
              .withExistingResourceGroup(resourceGroup);

    // generate a common prefix for every VM name
    String linuxVMNamePrefix = SdkContext.randomResourceName("vm-", 15);

    // inner loop: iterate once for every VM instance in the region
    for (int i = 1; i <= vmCount; i++) {

        // Create one public IP address Creatable for each VM
        Creatable<PublicIpAddress> publicIpAddressCreatable = azure.publicIpAddresses()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withLeafDomainLabel(SdkContext.randomResourceName("pip", 10));

        publicIpCreatableKeys.add(publicIpAddressCreatable.key());

        // Create one virtual machine Creatable 
        Creatable<VirtualMachine> virtualMachineCreatable = azure.virtualMachines()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withNewPrimaryNetwork(networkCreatable)
             .withPrimaryPrivateIpAddressDynamic()
             .withNewPrimaryPublicIpAddress(publicIpAddressCreatable)
             .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
             .withRootUsername(userName)
             .withSsh(sshKey)
             .withSize(VirtualMachineSizeTypes.STANDARD_DS3_V2)
             .withNewStorageAccount(storageAccountCreatable);
        // add the virtual machine Creatable to the list     
        creatableVirtualMachines.add(virtualMachineCreatable); 
     }
}
```

このコードに記述されている外側の `for` ループは、各リージョンを反復処理しながら、対応するリージョンのすべての仮想マシンで使用する仮想ネットワークの Creatable とストレージ アカウントの Creatable を定義しています。 管理ライブラリを使用しているとき、実際に必要になったタイミングでリソースを作成するには、[Creatables](java-sdk-azure-concepts.md#Creatables) を使います。詳細については、リンク先のページを参照してください。

内側の `for` ループでは、仮想マシンに使うパブリック IP アドレスの Creatable を取得した後、あらかじめ定義しておいた仮想ネットワーク、ストレージ アカウント、パブリック IP アドレスの Creatable を使って仮想マシンの Creatable を定義しています。  その後、この VirtualMachine Creatable を `creatableVirtualMachines` リストに追加します。

## <a name="create-the-virtual-machines"></a>仮想マシンの作成

```java
// create all virtual machines defined in the list, return all Creatable objects used
// including networks, public IP addresses, and storage accounts
CreatedResources<VirtualMachine> virtualMachines = azure.virtualMachines().create(creatableVirtualMachines);

// list the IDs of each virtual machine created 
for (VirtualMachine virtualMachine : virtualMachines.values()) {
    System.out.println(virtualMachine.id());
}

// call createdRelatedResource(key) to get the resources used to define the virtual machines. 
// Save the key at the time you define the Creatable to use CreatedResources like this
for (String publicIpCreatableKey : publicIpCreatableKeys) {
    PublicIPAddress pip = 
         (PublicIPAddress) virtualMachines.createdRelatedResource(publicIpCreatableKey);
}
```

`azure.virtualMachines().create(creatableVirtualMachines)` の呼び出しによって、`creatableVirtualMachines` リストに定義されているすべての仮想マシンが複数のリージョンに対して同時に作成されます。

これによって取得した `CreatedResources<VirtualMachine>` オブジェクトを使用すると、返された `VirtualMachine` 型だけでなく、`create()` メソッドの実行時に Azure サブスクリプションに作成されたすべてのリソースにアクセスできます。 返された値は、`createdRelatedResources()` から適切な型にキャストします。 

`Creatable<T>` と `CreatedResources` の使い方について詳しくは、[ライブラリの概念に関する記事](java-sdk-azure-concepts.md)を参照してください。

## <a name="delete-the-resource-group"></a>リソース グループを削除します 

```java
// finally block deletes the resource group before the code exits
// deleting a resource group deletes all resources created in it
finally {
    try {
        System.out.println("Deleting Resource Group: " + rgName);
        azure.resourceGroups().deleteByName(rgName);
        System.out.println("Deleted Resource Group: " + rgName);
    } catch (NullPointerException npe) {
        System.out.println("Did not create any resources in Azure. No clean up is necessary");
    } catch (Exception g) {
        g.printStackTrace();
    }
}
```

サンプルで作成されたリソースは、その終了前にこのブロックで削除されます。

## <a name="sample-explanation"></a>サンプルの説明

[完全なサンプル コードについては GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) を参照してください。

このサンプルでは、仮想マシンがホストされるリージョンごとに、`Creatable` オブジェクトを使って仮想ネットワークとストレージ アカウントを定義しています。 その後、それぞれの仮想マシンには、パブリック IP アドレスの `Creatable` オブジェクトが定義されます。 これらの `Creatable` オブジェクトを使って仮想マシンが定義され、VM の定義が `virtualMachineCreatable` リストに追加されます。

すべての仮想マシンの定義をリストに追加したら、`azure.virtualMachines().create(creatableVirtualMachines)` で、個々の仮想マシンを Azure に対して同時に作成します。

その後、返された CreatedResources オブジェクトから、作成したすべての仮想マシンの IP アドレスを取得し、それらの仮想マシンに負荷を分散させる [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) を作成します。 

万一エラーが発生した場合でも、`finally` ブロックで、Azure サブスクリプションからリソースが削除されます。

| サンプルで使われているクラス | メモ
|-------|-------|
| [VirtualMachine](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | 仮想マシンのプロパティを照会したり、仮想マシンの状態を管理したりすることができます。 `azure.virtualMachines().list()` からリスト形式で取得するか、`azure.virtualMachines().getByResourceGroup()` から名前または ID で取得します。
| [VirtualMachineSizeTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | [仮想マシン サイズのオプション](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)に対応する静的な値です。仮想マシンを定義するときに、`withSize()` に渡すパラメーターとして使用されます。
| [PublicIpAddress](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._public_i_p_address) | 仮想マシンごとに `azure.publicIpAddresses().define()` を使用して定義されます (ただしすぐには作成されません)。 `Creatable` ごとにキーを保存しておいて、後から `createdRelatedResource()` で取得します。
| [KnownLinuxVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | Linux 仮想マシンの一連のオプションです。仮想マシンを定義するときに、`withPopularLinuxImage()` メソッドに渡すパラメーターとして使用されます。
| [ネットワーク](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | このサンプルでは、各リージョンにつき 1 つの仮想ネットワークを `azure.networks().define()` で定義しています。 

## <a name="next-steps"></a>次の手順

[!INCLUDE [next-steps](includes/java-next-steps.md)]