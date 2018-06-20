---
title: Azure 仮想マシンを Java で管理する | Microsoft Docs
description: Azure SDK for Java を使って Azure 仮想マシンを管理するためのサンプル コード
author: rloutlaw
manager: douge
ms.assetid: 88629aee-6279-433e-a08b-4f8e290446d0
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: e3048b3317477f4b1fb8edf93e4bebad6b7fafce
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931168"
---
# <a name="manage-azure-virtual-machines-from-your-java-applications"></a><span data-ttu-id="77bd2-103">Azure 仮想マシンを Java アプリケーションから管理する</span><span class="sxs-lookup"><span data-stu-id="77bd2-103">Manage Azure virtual machines from your Java applications</span></span>

<span data-ttu-id="77bd2-104">[このサンプル](https://github.com/Azure-Samples/compute-java-manage-vm/)では、[Azure Management Libraries for Java](https://github.com/Azure/azure-sdk-for-java) を使って Azure 仮想マシンの作成と操作を行います。</span><span class="sxs-lookup"><span data-stu-id="77bd2-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-vm/) uses the [Azure management libraries for Java](https://github.com/Azure/azure-sdk-for-java) to create and work with Azure virtual machines.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="77bd2-105">サンプルの実行</span><span class="sxs-lookup"><span data-stu-id="77bd2-105">Run the sample</span></span>

<span data-ttu-id="77bd2-106">[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="77bd2-107">次に、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-vm.git
cd compute-java-manage-vm
mvn clean compile exec:java
```

<span data-ttu-id="77bd2-108">[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="77bd2-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="77bd2-109">Azure での認証</span><span class="sxs-lookup"><span data-stu-id="77bd2-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-windows-virtual-machine"></a><span data-ttu-id="77bd2-110">Windows 仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="77bd2-110">Create a Windows virtual machine</span></span>

```java
// Prepare a data disk for VM
Disk dataDisk = azure.disks().define(SdkContext.randomResourceName("dsk", 30))
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withData()
            .withSizeInGB(50)
            .create();

// create the windows virtual machine with the data disk            
VirtualMachine windowsVM = azure.virtualMachines().define(windowsVmName)
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularWindowsImage(KnownWindowsVirtualMachineImage.WINDOWS_SERVER_2012_R2_DATACENTER)
            .withAdminUsername(userName)
            .withAdminPassword(password)
            .withNewDataDisk(10)
            .withNewDataDisk(dataDiskCreatable)
            .withExistingDataDisk(dataDisk)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

<span data-ttu-id="77bd2-111">このコードは次のことを行っています。</span><span class="sxs-lookup"><span data-stu-id="77bd2-111">This code:</span></span>   

0. <span data-ttu-id="77bd2-112">仮想マシンで使用する、50 GB のサイズの `Disk` Creatable をランダムな名前で定義します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-112">Defines a `Disk` Creatable with a 50GB size and random name for use with a virtual machine.</span></span>
0. <span data-ttu-id="77bd2-113">`azure.virtualMachines().define()..create()` チェーンを使って、Windows Server 2012 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-113">Uses the `azure.virtualMachines().define()..create()` chain to create the Windows Server 2012 virtual machine.</span></span> <span data-ttu-id="77bd2-114">この API によって、前の手順で定義した `Disk` が仮想マシンと同時に作成されます。</span><span class="sxs-lookup"><span data-stu-id="77bd2-114">The API creates the `Disk` defined in the previous step the same time as the virtual machine.</span></span> <span data-ttu-id="77bd2-115">仮想マシンには、`withNewDataDisk(10)` を通じて 10 GB のデータ ディスクがアタッチされます。</span><span class="sxs-lookup"><span data-stu-id="77bd2-115">A 10GB data disk is also attached to the virtual machine through `withNewDataDisk(10)`.</span></span>

<span data-ttu-id="77bd2-116">[Creatable<T> オブジェクト](java-sdk-azure-concepts.md#Creatables)を使うと、リソースのローカルな表現を定義しておいて、実際に他の Azure リソースから必要とされたタイミングで作成することができます。詳細については、リンク先のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="77bd2-116">Learn more about using [Creatable<T> objects](java-sdk-azure-concepts.md#Creatables) to define local representations of resources and create them just as other Azure resources need them.</span></span>

## <a name="stop-start-and-restart-a-virtual-machine"></a><span data-ttu-id="77bd2-117">仮想マシンの停止、開始、再起動</span><span class="sxs-lookup"><span data-stu-id="77bd2-117">Stop, start, and restart a virtual machine</span></span>

```java
// look up a virtual machine by its ID and then restart, stop, and start it
azureVM = azure.getVirtualMachine.getById(windowsVM.id());

azureVM.restart();
azureVM.powerOff();
azureVM.start();
```

<span data-ttu-id="77bd2-118">`powerOff()` は、仮想マシン オペレーティング システムを停止するだけで、そのリソースの割り当ては解除しません。</span><span class="sxs-lookup"><span data-stu-id="77bd2-118">`powerOff()` stops the virtual machine operating system but does not deallocate its resources.</span></span>

## <a name="add-a-virtual-machine-to-an-existing-network"></a><span data-ttu-id="77bd2-119">既存のネットワークへの仮想マシンの追加</span><span class="sxs-lookup"><span data-stu-id="77bd2-119">Add a virtual machine to an existing network</span></span>

```java
// Get the virtual network the current virtual machine is using
Network network = windowsVM.getPrimaryNetworkInterface().primaryIPConfiguration().getNetwork();

// Create a Linux VM in the same subnet
VirtualMachine linuxVM = azure.virtualMachines().define(linuxVmName)
           .withRegion(region)
           .withExistingResourceGroup(rgName)
           .withExistingPrimaryNetwork(network)
           .withSubnet("subnet1") // default subnet name when no name specified at creation
           .withPrimaryPrivateIPAddressDynamic()
           .withoutPrimaryPublicIPAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withRootPassword(password)
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
```

<span data-ttu-id="77bd2-120">Windows VM ではなく Linux VM を定義するには、`withPopularLinuxImage` を使用します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-120">Use `withPopularLinuxImage` to define a Linux VM instead of a Windows one.</span></span>


## <a name="list-virtual-machines"></a><span data-ttu-id="77bd2-121">仮想マシンの列挙</span><span class="sxs-lookup"><span data-stu-id="77bd2-121">List virtual machines</span></span>

```java
// get a list of VMs in the same resource group as an existing VM
String resourceGroupName = windowsVM.resourceGroupName();
PagedList<VirtualMachine> resourceGroupVMs = azure.virtualMachines()
        .listByResourceGroup(resourceGroupName); 

// for each vitual machine in the resource group, log their name and plan
for (VirtualMachine virtualMachine : azure.virtualMachines().listByResourceGroup(resourceGroupName)) {
    System.out.println("VM " + virtualMachine.computerName() + 
        " has plan " + virtualMachine.plan());
}
```

<span data-ttu-id="77bd2-122">`azure.virtualMachines().list()` を使ってサブスクリプションのすべての仮想マシンを列挙し、リソース グループごとに、`tags()` から返された Map を反復処理しながら、タグ付けされた仮想マシンのコレクションを管理します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-122">List all virtual machines for a subscription using `azure.virtualMachines().list()` and iterate through the Map returned by `tags()` to manage tagged collections of virtual machines across resource groups.</span></span>

## <a name="update-a-virtual-machine"></a><span data-ttu-id="77bd2-123">仮想マシンの更新</span><span class="sxs-lookup"><span data-stu-id="77bd2-123">Update a virtual machine</span></span>

```java
// add a 10GB data disk to the virtual machine
windowsVM.update()
     .withNewDataDisk(10)
     .apply();
```

<span data-ttu-id="77bd2-124">`update()...apply()` を使用して仮想マシンの構成を更新します。`define()...create()` で仮想マシンを構成するときに使用したものと同じメソッドを使用しています。</span><span class="sxs-lookup"><span data-stu-id="77bd2-124">Update the virtual machine configuration using `update()...apply()` and the same methods used to configure the virtual machine when created through `define()...create()`.</span></span>

## <a name="delete-a-virtual-machine"></a><span data-ttu-id="77bd2-125">仮想マシンの削除</span><span class="sxs-lookup"><span data-stu-id="77bd2-125">Delete a virtual machine</span></span>

```java
// delete by ID if you already are working with the VM object
azure.virtualMachines().deleteById(windowsVM.id());

// delete by resource group and name
azure.virtualMachines().deleteByResourceGroup(rgName,windowsVmName);
```

## <a name="sample-explanation"></a><span data-ttu-id="77bd2-126">サンプルの説明</span><span class="sxs-lookup"><span data-stu-id="77bd2-126">Sample explanation</span></span>

<span data-ttu-id="77bd2-127">[サンプル コード](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java)では、50 GB のデータ ディスクを使って Windows 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-127">[The sample code](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) creates a Windows virtual machine with a 50GB data disk.</span></span> <span data-ttu-id="77bd2-128">その後、10 GB のデータ ディスクを別途作成し、それをこの Windows 仮想マシンにアタッチします。</span><span class="sxs-lookup"><span data-stu-id="77bd2-128">The sample then creates a second 10GB data disk and attaches it to this Windows virtual machine.</span></span>
<span data-ttu-id="77bd2-129">そのうえで、この Windows 仮想マシンと同じ仮想ネットワークに Linux 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-129">Then the sample creates a Linux virtual machine in the same virtual network as the Windows virtual machine.</span></span>

<span data-ttu-id="77bd2-130">サンプルでは、この 2 つの仮想マシンについての情報をログに記録します。どちらのログも完了前に削除されます。</span><span class="sxs-lookup"><span data-stu-id="77bd2-130">The sample logs information about both virtual machines and deletes them both before completing.</span></span>

| <span data-ttu-id="77bd2-131">サンプルで使われているクラス</span><span class="sxs-lookup"><span data-stu-id="77bd2-131">Class used in sample</span></span> | <span data-ttu-id="77bd2-132">メモ</span><span class="sxs-lookup"><span data-stu-id="77bd2-132">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="77bd2-133">VirtualMachine</span><span class="sxs-lookup"><span data-stu-id="77bd2-133">VirtualMachine</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | <span data-ttu-id="77bd2-134">仮想マシンのプロパティを照会したり、仮想マシンの状態を管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="77bd2-134">Query properties and manage state of virtual machines.</span></span> <span data-ttu-id="77bd2-135">`azure.virtualMachines().list()` を使ってリスト形式で取得するか、`azure.virtualMachines().getByResourceGroup()` で名前または ID を使って取得します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-135">Retrieved in list form  with`azure.virtualMachines().list()` or by name or ID `azure.virtualMachines().getByResourceGroup()`</span></span>
| [<span data-ttu-id="77bd2-136">VirtualMachineSizeTypes</span><span class="sxs-lookup"><span data-stu-id="77bd2-136">VirtualMachineSizeTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | <span data-ttu-id="77bd2-137">[仮想マシン サイズのオプション](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)に対応した静的な値を保持するクラスです。VM に割り当てるリソースを `withSize()`メソッドで定義する際に使用します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-137">Class with static values that map to [virtual machine size options](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), used by the `withSize()` method to define the resources allocated to the VM.</span></span>
| [<span data-ttu-id="77bd2-138">ディスク</span><span class="sxs-lookup"><span data-stu-id="77bd2-138">Disk</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk) | <span data-ttu-id="77bd2-139">ディスクを定義する際、データを格納するためのディスクは `withData()` で作成し、オペレーティング システム イメージを格納するためのディスクは、`withLinux` または `withWindows` のどちらか適切なメソッドで作成します。</span><span class="sxs-lookup"><span data-stu-id="77bd2-139">Create a disk to store data using `withData()` or operating system image using the appropriate `withLinux` or `withWindows` method when defining the disk.</span></span> <span data-ttu-id="77bd2-140">仮想マシンへのディスクのアタッチは、作成時に (`using withNewDataDisk` または `withExistingDataDisk`) 行うか、作成後に VirtualMachine オブジェクトの `update()..apply()` で行います。</span><span class="sxs-lookup"><span data-stu-id="77bd2-140">Attach disks to virtual machines either at the time of creation (`using withNewDataDisk` or `withExistingDataDisk`) or after creation by `update()..apply()` on the VirtualMachine object.</span></span>
| [<span data-ttu-id="77bd2-141">DiskSkuTypes</span><span class="sxs-lookup"><span data-stu-id="77bd2-141">DiskSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk_sku_types) | <span data-ttu-id="77bd2-142">Standard または [Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) のストレージ プランでディスクを定義するための静的な値を保持するクラスです。</span><span class="sxs-lookup"><span data-stu-id="77bd2-142">Class with static values to define a disk with a standard or [premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) storage plan.</span></span>
| [<span data-ttu-id="77bd2-143">KnownLinuxVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="77bd2-143">KnownLinuxVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | <span data-ttu-id="77bd2-144">仮想マシンを定義するときに `withPopularLinuxImage()` メソッドで使用する一連の Linux 仮想マシン オプションを保持するクラスです。</span><span class="sxs-lookup"><span data-stu-id="77bd2-144">Class with a set of Linux virtual machine options for use with the `withPopularLinuxImage()` method when defining a virtual machine.</span></span>
| [<span data-ttu-id="77bd2-145">KnownWindowsVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="77bd2-145">KnownWindowsVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_windows_virtual_machine_image) | <span data-ttu-id="77bd2-146">仮想マシンを定義するときに `withPopularWindowsImage()` メソッドで使用する一連の Windows 仮想マシン イメージ オプションを保持するクラスです。</span><span class="sxs-lookup"><span data-stu-id="77bd2-146">Class with a set of Windows virtual machine image options for use with the `withPopularWindowsImage()` method when defining a virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77bd2-147">次のステップ</span><span class="sxs-lookup"><span data-stu-id="77bd2-147">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]