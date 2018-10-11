---
title: Azure Virtual Machine Libraries for Java
description: ''
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
ms.openlocfilehash: a54bc40e1d28ba6ee1d8b0638cb259adbb69d78d
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893043"
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="9e836-103">Azure 仮想マシン ライブラリ</span><span class="sxs-lookup"><span data-stu-id="9e836-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="9e836-104">概要</span><span class="sxs-lookup"><span data-stu-id="9e836-104">Overview</span></span>

<span data-ttu-id="9e836-105">Linux または Windows を実行するオンデマンドのスケーラブルなコンピューティング リソースです。</span><span class="sxs-lookup"><span data-stu-id="9e836-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="9e836-106">Azure 仮想マシンの概要については、「[Azure Portal で Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9e836-106">To get started with Azure virtual machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="9e836-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="9e836-107">Management API</span></span>

<span data-ttu-id="9e836-108">Azure の Windows 仮想マシンと Linux 仮想マシンは、コードから Management API を使って作成、構成、スケールアウトすることができます。</span><span class="sxs-lookup"><span data-stu-id="9e836-108">Create, configure, and scale out Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="9e836-109">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="9e836-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-compute</artifactId>
    <version>1.3.0</version>
</dependency>
```   


## <a name="example"></a><span data-ttu-id="9e836-110">例</span><span class="sxs-lookup"><span data-stu-id="9e836-110">Example</span></span>

<span data-ttu-id="9e836-111">新しい Azure リソース グループに新しい Linux 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="9e836-111">Create a new Linux virtual machine in a new Azure resource group.</span></span>

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
> [<span data-ttu-id="9e836-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="9e836-112">Explore the Management APIs</span></span>](/java/api/overview/azure/virtualmachines/management)


## <a name="samples"></a><span data-ttu-id="9e836-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="9e836-113">Samples</span></span>

<span data-ttu-id="9e836-114">[仮想マシンの管理][1] </span><span class="sxs-lookup"><span data-stu-id="9e836-114">[Manage virtual machines][1] </span></span>  
<span data-ttu-id="9e836-115">[仮想ネットワークの管理][6] </span><span class="sxs-lookup"><span data-stu-id="9e836-115">[Manage virtual networks][6] </span></span>  
<span data-ttu-id="9e836-116">[カスタム イメージからの仮想マシンの作成][2] </span><span class="sxs-lookup"><span data-stu-id="9e836-116">[Create a virtual machine from a custom image][2] </span></span>  
<span data-ttu-id="9e836-117">[複数のリージョンに対して同時に仮想マシンを作成する][5]  </span><span class="sxs-lookup"><span data-stu-id="9e836-117">[Create virtual machines across regions in parallel][5]  </span></span>  
<span data-ttu-id="9e836-118">[ロード バランサーを使って仮想マシン スケール セットを作成する][7]</span><span class="sxs-lookup"><span data-stu-id="9e836-118">[Create a virtual machine scale set with a load balancer][7]</span></span>    

[1]: ../docs-ref-conceptual/java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[5]: ../docs-ref-conceptual/java-sdk-virtual-machines-in-parallel.md
[6]: ../docs-ref-conceptual/java-sdk-manage-virtual-networks.md
[7]: ../docs-ref-conceptual/java-sdk-manage-vm-scalesets.md

<span data-ttu-id="9e836-119">アプリで利用できる [Azure 仮想マシンのサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=VM)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="9e836-119">Explore more [sample Java code for Azure virtual machines](https://azure.microsoft.com/resources/samples/?platform=java&term=VM) you can use in your apps.</span></span>