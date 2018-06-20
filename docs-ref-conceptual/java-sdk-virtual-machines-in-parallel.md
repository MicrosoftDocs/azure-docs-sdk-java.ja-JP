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
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931198"
---
# <a name="create-virtual-machines-across-multiple-regions-from-your-java-applications"></a><span data-ttu-id="541a0-103">Java アプリケーションから複数のリージョンに対して仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="541a0-103">Create virtual machines across multiple regions from your Java applications</span></span>

<span data-ttu-id="541a0-104">[このサンプル](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)では、[Azure Management Libraries for Java](https://github.com/Azure/azure-sdk-for-java) を使って複数の Azure リージョンに対して同時に仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="541a0-104">[This sample](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) creates virtual machines in parallel across different Azure regions using the [Azure management libraries for Java](https://github.com/Azure/azure-sdk-for-java).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="541a0-105">4 つのリージョンに対し、[STANDARD_DS3_V2 サイズ](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes)の Ubuntu 16.04 LTS を実行する合計 48 の VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="541a0-105">The sample creates a total of 48 VMs running Ubuntu 16.04 LTS of [size STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes) across four regions.</span></span> <span data-ttu-id="541a0-106">これらの仮想マシンは、終了前にサンプル コードによって削除されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-106">The sample code deletes these virtual machines before exiting.</span></span> <span data-ttu-id="541a0-107">このサンプルを既定の VM 数で実行する前に必ず、[サービスの制限とクォータをチェック](http://docs.microsoft.com/azure/azure-subscription-service-limits)してください。</span><span class="sxs-lookup"><span data-stu-id="541a0-107">Make sure to [check your service limits and quota](http://docs.microsoft.com/azure/azure-subscription-service-limits) before running this sample with the default number of VMs.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="541a0-108">サンプルの実行</span><span class="sxs-lookup"><span data-stu-id="541a0-108">Run the sample</span></span>

<span data-ttu-id="541a0-109">[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="541a0-109">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="541a0-110">次に、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="541a0-110">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel.git
cd compute-java-create-virtual-machines-across-regions-in-parallel
mvn clean compile exec:java
```

<span data-ttu-id="541a0-111">[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="541a0-111">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="541a0-112">Azure での認証</span><span class="sxs-lookup"><span data-stu-id="541a0-112">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="set-locations-and-counts-for-the-virtual-machines"></a><span data-ttu-id="541a0-113">仮想マシンの場所と数の設定</span><span class="sxs-lookup"><span data-stu-id="541a0-113">Set locations and counts for the virtual machines</span></span>

```java
// use a Map to define how where and how many VMs to create 
Map<Region, Integer> virtualMachinesByLocation = new HashMap<Region, Integer>();

// create 12 virtual machines in four regions
virtualMachinesByLocation.put(Region.US_EAST, 12);
virtualMachinesByLocation.put(Region.US_SOUTH_CENTRAL, 12);
virtualMachinesByLocation.put(Region.US_WEST, 12);
virtualMachinesByLocation.put(Region.US_NORTH_CENTRAL, 12);
```

<span data-ttu-id="541a0-114">この `Map` は、後で各リージョンへの VM の分配を設定するためにサンプルの中で使用します。</span><span class="sxs-lookup"><span data-stu-id="541a0-114">This `Map` is used later in the sample to set the distrubtion of the VMs worldwide.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="541a0-115">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="541a0-115">Create a resource group</span></span> 

```java
// logically associate the resources in the sample into a randomly named resource group
final String rgName = SdkContext.randomResourceName("rgCOPD", 24);
ResourceGroup resourceGroup = azure.resourceGroups().define(rgName)
                .withRegion(Region.US_EAST)
                .create();
```

<span data-ttu-id="541a0-116">サンプルで使う各リソースは、このリソース グループによって管理されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-116">Each resource in the sample is managed by this resource group.</span></span> <span data-ttu-id="541a0-117">そのため後でリソース グループを削除すれば、簡単にリソースをクリーンアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="541a0-117">This makes the resources easy to clean up later by deleting the resource group.</span></span>

## <a name="define-the-virtual-machines"></a><span data-ttu-id="541a0-118">仮想マシンの定義</span><span class="sxs-lookup"><span data-stu-id="541a0-118">Define the virtual machines</span></span>
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

<span data-ttu-id="541a0-119">このコードに記述されている外側の `for` ループは、各リージョンを反復処理しながら、対応するリージョンのすべての仮想マシンで使用する仮想ネットワークの Creatable とストレージ アカウントの Creatable を定義しています。</span><span class="sxs-lookup"><span data-stu-id="541a0-119">The outer `for` loop above iterates through each region, defining a virtual network Creatable and storage account Creatable for use by all virtual machines in that region.</span></span> <span data-ttu-id="541a0-120">管理ライブラリを使用しているとき、実際に必要になったタイミングでリソースを作成するには、[Creatables](java-sdk-azure-concepts.md#Creatables) を使います。詳細については、リンク先のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="541a0-120">Learn more about using [Creatables](java-sdk-azure-concepts.md#Creatables) to create resources only as needed when using the management libraries.</span></span>

<span data-ttu-id="541a0-121">内側の `for` ループでは、仮想マシンに使うパブリック IP アドレスの Creatable を取得した後、あらかじめ定義しておいた仮想ネットワーク、ストレージ アカウント、パブリック IP アドレスの Creatable を使って仮想マシンの Creatable を定義しています。</span><span class="sxs-lookup"><span data-stu-id="541a0-121">The inner `for` loop gets a public IP address Creatable for the virtual machine and then defines a virtual machine Creatable using the Creatables for the virtual network, storage account, and public IP address defined previously.</span></span>  <span data-ttu-id="541a0-122">その後、この VirtualMachine Creatable を `creatableVirtualMachines` リストに追加します。</span><span class="sxs-lookup"><span data-stu-id="541a0-122">This VirtualMachine Creatable is then added to the `creatableVirtualMachines` list.</span></span>

## <a name="create-the-virtual-machines"></a><span data-ttu-id="541a0-123">仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="541a0-123">Create the virtual machines</span></span>

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

<span data-ttu-id="541a0-124">`azure.virtualMachines().create(creatableVirtualMachines)` の呼び出しによって、`creatableVirtualMachines` リストに定義されているすべての仮想マシンが複数のリージョンに対して同時に作成されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-124">The `azure.virtualMachines().create(creatableVirtualMachines)` call creates all of the virtual machines defined in the `creatableVirtualMachines` List in parallel across the regions.</span></span>

<span data-ttu-id="541a0-125">これによって取得した `CreatedResources<VirtualMachine>` オブジェクトを使用すると、返された `VirtualMachine` 型だけでなく、`create()` メソッドの実行時に Azure サブスクリプションに作成されたすべてのリソースにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="541a0-125">Use the returned `CreatedResources<VirtualMachine>` object to access any resources created in the Azure subscription during the the `create()` method, not just the returned `VirtualMachine` type.</span></span> <span data-ttu-id="541a0-126">返された値は、`createdRelatedResources()` から適切な型にキャストします。</span><span class="sxs-lookup"><span data-stu-id="541a0-126">Cast the returned value from `createdRelatedResources()` to the correct type.</span></span> 

<span data-ttu-id="541a0-127">`Creatable<T>` と `CreatedResources` の使い方について詳しくは、[ライブラリの概念に関する記事](java-sdk-azure-concepts.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="541a0-127">Learn more about working with `Creatable<T>` and `CreatedResources` in our [library concepts article](java-sdk-azure-concepts.md).</span></span>

## <a name="delete-the-resource-group"></a><span data-ttu-id="541a0-128">リソース グループを削除します</span><span class="sxs-lookup"><span data-stu-id="541a0-128">Delete the resource group</span></span> 

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

<span data-ttu-id="541a0-129">サンプルで作成されたリソースは、その終了前にこのブロックで削除されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-129">This block deletes resources created in the sample before the sample exits.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="541a0-130">サンプルの説明</span><span class="sxs-lookup"><span data-stu-id="541a0-130">Sample explanation</span></span>

<span data-ttu-id="541a0-131">[完全なサンプル コードについては GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="541a0-131">View the complete sample code on [Github](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel).</span></span>

<span data-ttu-id="541a0-132">このサンプルでは、仮想マシンがホストされるリージョンごとに、`Creatable` オブジェクトを使って仮想ネットワークとストレージ アカウントを定義しています。</span><span class="sxs-lookup"><span data-stu-id="541a0-132">The sample uses `Creatable` objects to define a virtual network and storage account for each region hosting the virtual machines.</span></span> <span data-ttu-id="541a0-133">その後、それぞれの仮想マシンには、パブリック IP アドレスの `Creatable` オブジェクトが定義されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-133">`Creatable` objects are then defined for the public IP address for each virtual machine.</span></span> <span data-ttu-id="541a0-134">これらの `Creatable` オブジェクトを使って仮想マシンが定義され、VM の定義が `virtualMachineCreatable` リストに追加されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-134">The sample defines the virtual machines using these `Creatable` objects, and sample adds the VM definition to the `virtualMachineCreatable` list.</span></span>

<span data-ttu-id="541a0-135">すべての仮想マシンの定義をリストに追加したら、`azure.virtualMachines().create(creatableVirtualMachines)` で、個々の仮想マシンを Azure に対して同時に作成します。</span><span class="sxs-lookup"><span data-stu-id="541a0-135">After the code adds every virtual machine definition to the list, `azure.virtualMachines().create(creatableVirtualMachines)` creates each virtual machine in parallel in Azure.</span></span>

<span data-ttu-id="541a0-136">その後、返された CreatedResources オブジェクトから、作成したすべての仮想マシンの IP アドレスを取得し、それらの仮想マシンに負荷を分散させる [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) を作成します。</span><span class="sxs-lookup"><span data-stu-id="541a0-136">The sample code then gets the IP addresses for all of the created virtual machines from the returned CreatedResources object to create a [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) to distribute load across the virtual machines.</span></span> 

<span data-ttu-id="541a0-137">万一エラーが発生した場合でも、`finally` ブロックで、Azure サブスクリプションからリソースが削除されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-137">The `finally` block deletes the resources from your Azure subscription even in the case of an error.</span></span>

| <span data-ttu-id="541a0-138">サンプルで使われているクラス</span><span class="sxs-lookup"><span data-stu-id="541a0-138">Class used in sample</span></span> | <span data-ttu-id="541a0-139">メモ</span><span class="sxs-lookup"><span data-stu-id="541a0-139">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="541a0-140">VirtualMachine</span><span class="sxs-lookup"><span data-stu-id="541a0-140">VirtualMachine</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | <span data-ttu-id="541a0-141">仮想マシンのプロパティを照会したり、仮想マシンの状態を管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="541a0-141">Query properties and manage state of virtual machines.</span></span> <span data-ttu-id="541a0-142">`azure.virtualMachines().list()` からリスト形式で取得するか、`azure.virtualMachines().getByResourceGroup()` から名前または ID で取得します。</span><span class="sxs-lookup"><span data-stu-id="541a0-142">Retrieved in list form from `azure.virtualMachines().list()` or by name or ID `azure.virtualMachines().getByResourceGroup()`</span></span>
| [<span data-ttu-id="541a0-143">VirtualMachineSizeTypes</span><span class="sxs-lookup"><span data-stu-id="541a0-143">VirtualMachineSizeTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | <span data-ttu-id="541a0-144">[仮想マシン サイズのオプション](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)に対応する静的な値です。仮想マシンを定義するときに、`withSize()` に渡すパラメーターとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-144">Static values that map to [virtual machine size options](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) for use as a parameter to `withSize()` when defining a virtual machine.</span></span>
| [<span data-ttu-id="541a0-145">PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="541a0-145">PublicIpAddress</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._public_i_p_address) | <span data-ttu-id="541a0-146">仮想マシンごとに `azure.publicIpAddresses().define()` を使用して定義されます (ただしすぐには作成されません)。</span><span class="sxs-lookup"><span data-stu-id="541a0-146">Defined, but not immediately created, for each virtual machine through `azure.publicIpAddresses().define()`.</span></span> <span data-ttu-id="541a0-147">`Creatable` ごとにキーを保存しておいて、後から `createdRelatedResource()` で取得します。</span><span class="sxs-lookup"><span data-stu-id="541a0-147">Store the key for each `Creatable` and retrieve later through `createdRelatedResource()`</span></span>
| [<span data-ttu-id="541a0-148">KnownLinuxVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="541a0-148">KnownLinuxVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | <span data-ttu-id="541a0-149">Linux 仮想マシンの一連のオプションです。仮想マシンを定義するときに、`withPopularLinuxImage()` メソッドに渡すパラメーターとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="541a0-149">Set of Linux virtual machine options used as a parameter to `withPopularLinuxImage()` method when defining a virtual machine.</span></span>
| [<span data-ttu-id="541a0-150">ネットワーク</span><span class="sxs-lookup"><span data-stu-id="541a0-150">Network</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | <span data-ttu-id="541a0-151">このサンプルでは、各リージョンにつき 1 つの仮想ネットワークを `azure.networks().define()` で定義しています。</span><span class="sxs-lookup"><span data-stu-id="541a0-151">The sample defines one virtual network for each region through  `azure.networks().define()` .</span></span> 

## <a name="next-steps"></a><span data-ttu-id="541a0-152">次のステップ</span><span class="sxs-lookup"><span data-stu-id="541a0-152">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]