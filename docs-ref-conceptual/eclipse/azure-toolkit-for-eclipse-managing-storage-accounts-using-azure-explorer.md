---
title: Azure Explorer for Eclipse を使用してストレージ アカウントを管理する
description: Azure Explorer for Eclipse を使用して Azure ストレージ アカウントを管理する方法について説明します。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 310d95436189af09f794154f4c9f0e71c47d88c8
ms.sourcegitcommit: 3d3460289ab6b9165c2cf6a3dd56eafd0692501e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="388a1-103">Azure Explorer for Eclipse を使用してストレージ アカウントを管理する</span><span class="sxs-lookup"><span data-stu-id="388a1-103">Manage storage accounts by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="388a1-104">Azure Toolkit for Eclipse の一部である Azure Explorer は、Eclipse 統合開発環境 (IDE) 内から Azure アカウントのストレージ アカウントを管理するための使いやすいソリューションを Java 開発者に提供します。</span><span class="sxs-lookup"><span data-stu-id="388a1-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-eclipse"></a><span data-ttu-id="388a1-105">Eclipse でストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="388a1-105">Create a storage account in Eclipse</span></span>

<span data-ttu-id="388a1-106">Azure Explorer を使用してストレージ アカウントを作成するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="388a1-106">To create a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="388a1-107">「[Azure Toolkit for Eclipse のサインイン手順](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)」を使用して Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="388a1-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).</span></span>

1. <span data-ttu-id="388a1-108">**Azure Explorer** ビューで、**[Azure]** ノードを展開し、**[ストレージ アカウント]** を右クリックし、**[ストレージ アカウントの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![[ストレージ アカウントの作成] コマンド][CS01]

1. <span data-ttu-id="388a1-110">**[ストレージ アカウントの作成]** ダイアログ ボックスで、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-110">In the **Create Storage Account** dialog box, specify the following options:</span></span>

   ![[新しいストレージ アカウントの作成] ダイアログ ボックス][CS02]

   * <span data-ttu-id="388a1-112">**[名前]**: 新しいストレージ アカウントの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-112">**Name**: Specifies the name for the new storage account.</span></span>

   * <span data-ttu-id="388a1-113">**[サブスクリプション]**: 新しいストレージ アカウントに使用する Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-113">**Subscription**: Specifies the Azure subscription that you want to use for the new storage account.</span></span>

   * <span data-ttu-id="388a1-114">**[リソース グループ]**: 仮想マシン用のリソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-114">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="388a1-115">次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="388a1-115">Select one of the following options:</span></span>
      * <span data-ttu-id="388a1-116">**[新規作成]**: 新しいリソース グループを作成することを指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-116">**Create New**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="388a1-117">**[既存のものを使用]**: Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-117">**Use Existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

   * <span data-ttu-id="388a1-118">**[リージョン]**: ストレージ アカウントが作成される場所 ("米国西部" など) を指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-118">**Region**: Specifies the location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="388a1-119">**[アカウントの種類]**: 作成するストレージ アカウントの種類 ("Blob Storage" など) を指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-119">**Account kind**: Specifies the type of storage account to create (for example, "Blob storage").</span></span> <span data-ttu-id="388a1-120">詳細については、「[Azure ストレージ アカウントについて]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="388a1-120">For more information, see [About Azure storage accounts].</span></span>

   * <span data-ttu-id="388a1-121">**[パフォーマンス]**: 選択された発行元からどのストレージ アカウント サービスを使用するかを指定します ("Premium" など)。</span><span class="sxs-lookup"><span data-stu-id="388a1-121">**Performance**: Specifies which storage account offering to use from the selected publisher (for example, "Premium").</span></span> <span data-ttu-id="388a1-122">詳細については、「[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="388a1-122">For more information, see [Azure storage scalability and performance targets].</span></span>

   * <span data-ttu-id="388a1-123">**[レプリケーション]**: ストレージ アカウントのレプリケーション ("ゾーン冗長" など) を指定します。</span><span class="sxs-lookup"><span data-stu-id="388a1-123">**Replication**: Specifies the replication for the storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="388a1-124">詳細については、「[Azure Storage のレプリケーション]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="388a1-124">For more information, see [Azure storage replication].</span></span>

1. <span data-ttu-id="388a1-125">上記のオプションをすべて指定したら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-125">When you have specified all of the preceding options, click **Create**.</span></span>

## <a name="create-a-storage-container-in-eclipse"></a><span data-ttu-id="388a1-126">Eclipse でストレージ コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="388a1-126">Create a storage container in Eclipse</span></span>

<span data-ttu-id="388a1-127">Azure Explorer を使用してストレージ コンテナーを作成するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="388a1-127">To create a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="388a1-128">**Azure Explorer** ビューで、コンテナーを作成するストレージ アカウントを右クリックし、**[BLOB コンテナーを作成する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-128">In the **Azure Explorer** view, right-click the storage account where you want to create a container, and then click **Create blob container**.</span></span>

   ![[BLOB コンテナーを作成する] コマンド][CC01]

1. <span data-ttu-id="388a1-130">**[BLOB コンテナーを作成する]** ダイアログ ボックスで、コンテナーの名前を指定し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-130">In the **Create blob container** dialog box, specify the name for your container, and then click **OK**.</span></span> <span data-ttu-id="388a1-131">ストレージ コンテナーの名前付けの詳細については、「[コンテナー、BLOB、メタデータの名前付けと参照]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="388a1-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![[BLOB コンテナーを作成する] ダイアログ ボックス][CC02]

## <a name="delete-a-storage-container-in-eclipse"></a><span data-ttu-id="388a1-133">Eclipse でストレージ コンテナーを削除する</span><span class="sxs-lookup"><span data-stu-id="388a1-133">Delete a storage container in Eclipse</span></span>

<span data-ttu-id="388a1-134">Azure Explorer を使用してストレージ コンテナーを削除するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="388a1-134">To delete a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="388a1-135">**Azure Explorer** ビューで、ストレージ コンテナーを右クリックし、**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-135">In the **Azure Explorer** view, right-click the storage container, and then click **Delete**.</span></span>

   ![[ストレージ コンテナーの削除] コマンド][DC01]

1. <span data-ttu-id="388a1-137">確認ウィンドウで、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-137">In the confirmation window, click **OK**.</span></span>

   ![[ストレージ コンテナーの削除] 確認ウィンドウ][DC02]

## <a name="delete-a-storage-account-in-eclipse"></a><span data-ttu-id="388a1-139">Eclipse でストレージ アカウントを削除する</span><span class="sxs-lookup"><span data-stu-id="388a1-139">Delete a storage account in Eclipse</span></span>

<span data-ttu-id="388a1-140">Azure Explorer を使用してストレージ アカウントを削除するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="388a1-140">To delete a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="388a1-141">**Azure Explorer** ビューで、ストレージ アカウントを右クリックし、**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-141">In the **Azure Explorer** view, right-click the storage account, and then click **Delete**.</span></span>

   ![[Delete storage account] (ストレージ アカウントの削除) コマンド][DS01]

1. <span data-ttu-id="388a1-143">確認ウィンドウで、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="388a1-143">In the confirmation window, click **OK**.</span></span>

   ![[Delete storage account] (ストレージ アカウントの削除) 確認ウィンドウ][DS02]

## <a name="next-steps"></a><span data-ttu-id="388a1-145">次の手順</span><span class="sxs-lookup"><span data-stu-id="388a1-145">Next steps</span></span>

<span data-ttu-id="388a1-146">Azure ストレージ アカウント、サイズ、および料金の詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="388a1-146">For more information about Azure storage accounts, sizes, and pricing, see the following resources:</span></span>

* <span data-ttu-id="388a1-147">[Microsoft Azure Storage の概要]</span><span class="sxs-lookup"><span data-stu-id="388a1-147">[Introduction to Microsoft Azure Storage]</span></span>
* <span data-ttu-id="388a1-148">[Azure ストレージ アカウントについて]</span><span class="sxs-lookup"><span data-stu-id="388a1-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="388a1-149">Azure ストレージ アカウントのサイズ</span><span class="sxs-lookup"><span data-stu-id="388a1-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="388a1-150">[Azure の Windows ストレージ アカウントのサイズ]</span><span class="sxs-lookup"><span data-stu-id="388a1-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="388a1-151">[Azure の Linux ストレージ アカウントのサイズ]</span><span class="sxs-lookup"><span data-stu-id="388a1-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="388a1-152">Azure ストレージ アカウントの料金</span><span class="sxs-lookup"><span data-stu-id="388a1-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="388a1-153">[Windows ストレージ アカウントの料金]</span><span class="sxs-lookup"><span data-stu-id="388a1-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="388a1-154">[Linux ストレージ アカウントの料金]</span><span class="sxs-lookup"><span data-stu-id="388a1-154">[Linux storage-account pricing]</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Microsoft Azure Storage の概要]: /azure/storage/storage-introduction
[Introduction to Microsoft Azure Storage]: /azure/storage/storage-introduction
[Azure ストレージ アカウントについて]: /azure/storage/storage-create-storage-account
[About Azure storage accounts]: /azure/storage/storage-create-storage-account
[Azure Storage のレプリケーション]: /azure/storage/storage-redundancy
[Azure storage replication]: /azure/storage/storage-redundancy
[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]: /azure/storage/storage-scalability-targets
[Azure storage scalability and Performance Targets]: /azure/storage/storage-scalability-targets
[コンテナー、BLOB、メタデータの名前付けと参照]: http://go.microsoft.com/fwlink/?LinkId=255555
[Naming and referencing containers, blobs, and metadata]: http://go.microsoft.com/fwlink/?LinkId=255555

[Azure の Windows ストレージ アカウントのサイズ]: /azure/virtual-machines/virtual-machines-windows-sizes
[Sizes for Windows storage accounts in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure の Linux ストレージ アカウントのサイズ]: /azure/virtual-machines/virtual-machines-linux-sizes
[Sizes for Linux storage accounts in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows ストレージ アカウントの料金]: /pricing/details/virtual-machines/windows/
[Windows storage-account pricing]: /pricing/details/virtual-machines/windows/
[Linux ストレージ アカウントの料金]: /pricing/details/virtual-machines/linux/
[Linux storage-account pricing]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png
