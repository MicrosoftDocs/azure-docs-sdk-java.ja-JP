---
title: Azure Virtual Machine Libraries for Java
description: 
keywords: Azure, Java, SDK, API, Compute , Virtual Machines
author: douge
ms.author: douge
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: compute
ms.openlocfilehash: b414f4dc9289958c2562ddc6d41133279f47a45e
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2017
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="21283-103">Azure 仮想マシン ライブラリ</span><span class="sxs-lookup"><span data-stu-id="21283-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="21283-104">概要</span><span class="sxs-lookup"><span data-stu-id="21283-104">Overview</span></span>

<span data-ttu-id="21283-105">Linux または Windows を実行するオンデマンドのスケーラブルなコンピューティング リソースです。</span><span class="sxs-lookup"><span data-stu-id="21283-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="21283-106">Azure 仮想マシンの概要については、「[Azure Portal で Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="21283-106">To get started with Azure virtual machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="21283-107">Management API</span><span class="sxs-lookup"><span data-stu-id="21283-107">Management API</span></span>

<span data-ttu-id="21283-108">Azure の Windows 仮想マシンと Linux 仮想マシンは、コードから Management API を使って作成、構成、スケールアウトすることができます。</span><span class="sxs-lookup"><span data-stu-id="21283-108">Create, configure, and scale out Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="21283-109">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="21283-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-compute</artifactId>
    <version>1.2.1</version>
</dependency>
```   


## <a name="example"></a><span data-ttu-id="21283-110">例</span><span class="sxs-lookup"><span data-stu-id="21283-110">Example</span></span>

<span data-ttu-id="21283-111">新しい Azure リソース グループに新しい Linux 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="21283-111">Create a new Linux virtual machine in a new Azure resource group.</span></span>

```java
VirtualMachine newLinuxVm = azure.virtualMachines().define(linuxVmName)
            .withRegion(Region.US_EAST)
            .withNewResourceGroup("myResourceGroup")
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
            .withRootUsername(userName)
            .withSshKey(key)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="21283-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="21283-112">Explore the Management APIs</span></span>](/java/api/overview/azure/virtualmachines/managementapi)


## <a name="samples"></a><span data-ttu-id="21283-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="21283-113">Samples</span></span>

<span data-ttu-id="21283-114">[仮想マシンの管理][1] </span><span class="sxs-lookup"><span data-stu-id="21283-114">[Manage virtual machines][1] </span></span>  
<span data-ttu-id="21283-115">[仮想ネットワークの管理][6] </span><span class="sxs-lookup"><span data-stu-id="21283-115">[Manage virtual networks][6] </span></span>  
<span data-ttu-id="21283-116">[カスタム イメージからの仮想マシンの作成][2] </span><span class="sxs-lookup"><span data-stu-id="21283-116">[Create a virtual machine from a custom image][2] </span></span>  
<span data-ttu-id="21283-117">[複数のリージョンに対して同時に仮想マシンを作成する][5]  </span><span class="sxs-lookup"><span data-stu-id="21283-117">[Create virtual machines across regions in parallel][5]  </span></span>  
<span data-ttu-id="21283-118">[ロード バランサーを使って仮想マシン スケール セットを作成する][7]</span><span class="sxs-lookup"><span data-stu-id="21283-118">[Create a virtual machine scale set with a load balancer][7]</span></span>    

[1]: ../docs-ref-conceptual/java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[5]: ../docs-ref-conceptual/java-sdk-virtual-machines-in-parallel.md
[6]: ../docs-ref-conceptual/java-sdk-manage-virtual-networks.md
[7]: ../docs-ref-conceptual/java-sdk-manage-vm-scalesets.md

<span data-ttu-id="21283-119">アプリで利用できる [Azure 仮想マシンのサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=VM)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="21283-119">Explore more [sample Java code for Azure virtual machines](https://azure.microsoft.com/resources/samples/?platform=java&term=VM) you can use in your apps.</span></span>