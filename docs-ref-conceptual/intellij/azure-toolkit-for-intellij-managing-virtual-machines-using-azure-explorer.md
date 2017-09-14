---
title: "Azure Explorer for IntelliJ を使用して仮想マシンを管理する"
description: "Azure Explorer for IntelliJ を使用して Azure 仮想マシンを管理する方法について説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: fb175a44235a8ff27675dd42905237a8826f91fe
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="73ec0-103">Azure Explorer for IntelliJ を使用して仮想マシンを管理する</span><span class="sxs-lookup"><span data-stu-id="73ec0-103">Manage virtual machines by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="73ec0-104">Azure Toolkit for IntelliJ の一部である Azure Explorer は、IntelliJ 統合開発環境 (IDE) 内から Azure アカウントの仮想マシンを管理するための使いやすいソリューションを Java 開発者に提供します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a><span data-ttu-id="73ec0-105">IntelliJ で仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="73ec0-105">Create a virtual machine in IntelliJ</span></span>

<span data-ttu-id="73ec0-106">Azure Explorer を使用して仮想マシンを作成するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span> 

1. <span data-ttu-id="73ec0-107">「[Azure Toolkit for IntelliJ のサインイン手順]」に従って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-107">Sign in to your Azure account by using the steps in the [Sign-in instructions for the Azure Toolkit for IntelliJ] article.</span></span>

2. <span data-ttu-id="73ec0-108">**Azure Explorer** ビューで、**[Azure]** ノードを展開し、**[仮想マシン]** を右クリックし、**[VM の作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span> 

   <span data-ttu-id="73ec0-109">![[VM の作成] コマンド][CR01]</span><span class="sxs-lookup"><span data-stu-id="73ec0-109">![The Create VM command][CR01]</span></span>  
    <span data-ttu-id="73ec0-110">**[新しい仮想マシンの作成]** ウィザードが開きます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="73ec0-111">**[サブスクリプションの選択]** ウィンドウで、サブスクリプションを選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span> 

   ![[サブスクリプションの選択] ウィンドウ][CR02]

4. <span data-ttu-id="73ec0-113">**[仮想マシン イメージの選択]** ウィンドウで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="73ec0-114">**[場所]**: 仮想マシンを作成する場所を指定します ("*米国西部*" など)。</span><span class="sxs-lookup"><span data-stu-id="73ec0-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span> 

   * <span data-ttu-id="73ec0-115">**[Recommended Image]\(推奨イメージ\)**: よく使用されるイメージの省略版リストからイメージを選択することを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-115">**Recommended image**: Specifies that you will choose an image from an abbreviated list of commonly used images.</span></span>

   * <span data-ttu-id="73ec0-116">**[カスタム イメージ]**: 次の情報を指定することでカスタム イメージを選択することを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-116">**Custom image**: Specifies that you will choose a custom image by providing the following information:</span></span>

      * <span data-ttu-id="73ec0-117">**[発行者]**: 仮想マシンを作成するために使用するイメージを作成した発行元を指定します (*Microsoft* など)。</span><span class="sxs-lookup"><span data-stu-id="73ec0-117">**Publisher**: Specifies the publisher that created the image that you will use for your virtual machine (for example, *Microsoft*).</span></span>

      * <span data-ttu-id="73ec0-118">**[プラン]**: 選択した発行元の、仮想マシンで使用するプランを指定します (*JDK* など)。</span><span class="sxs-lookup"><span data-stu-id="73ec0-118">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

      * <span data-ttu-id="73ec0-119">**[SKU]**: 選択したプランで使用する在庫保管単位 (SKU) を指定します (*JDK_8* など)。</span><span class="sxs-lookup"><span data-stu-id="73ec0-119">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

      * <span data-ttu-id="73ec0-120">**[Version #]\(バージョン番号\)**: 選択した SKU で使用するバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-120">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![[仮想マシン イメージの選択] ウィンドウ][CR03]

5. <span data-ttu-id="73ec0-122">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-122">Click **Next**.</span></span> 

6. <span data-ttu-id="73ec0-123">**[仮想マシンの基本設定]** ウィンドウで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-123">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="73ec0-124">**[仮想マシン名]**: 新しい仮想マシンの名前を指定します。英字、数字、ハイフンのみ使用でき、先頭には英字を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73ec0-124">**Virtual machine name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="73ec0-125">**[サイズ]**: 仮想マシンに割り当てるコアとメモリの数を指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-125">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="73ec0-126">**[ユーザー名]**: 仮想マシンを管理するために作成する管理者アカウントを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-126">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="73ec0-127">**[パスワード]** と **[確認]**: 管理者アカウントのパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-127">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![[仮想マシンの基本設定] ウィンドウ][CR04]

7. <span data-ttu-id="73ec0-129">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-129">Click **Next**.</span></span> 

8. <span data-ttu-id="73ec0-130">**[関連するリソース]** ウィンドウで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-130">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="73ec0-131">**[リソース グループ]**: 仮想マシン用のリソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-131">**Resource group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="73ec0-132">次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-132">Select one of the following options:</span></span>
      * <span data-ttu-id="73ec0-133">**[新規作成]**: 新しいリソース グループを作成することを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-133">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="73ec0-134">**[Use Existing]\(既存の使用\)**: Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-134">**Use existing**: Specifies that you want to select from a list of resource groups that are associated with your Azure account.</span></span>

       ![[関連するリソース] ウィンドウ][CR07]

   * <span data-ttu-id="73ec0-136">**[ストレージ アカウント]**: 仮想マシンを格納するために使用するストレージ アカウントを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-136">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="73ec0-137">既存のストレージ アカウントを使用するか、新しいストレージ アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-137">You can choose an existing storage account or create a new account.</span></span> <span data-ttu-id="73ec0-138">**[新規作成]** を選択した場合は、次のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-138">If you choose **Create New**, the following dialog box appears:</span></span>

      ![[ストレージ アカウントの作成] ダイアログ ボックス][CR05]

   * <span data-ttu-id="73ec0-140">**[仮想ネットワーク]** と **[サブネット]**: 仮想マシンを接続する仮想ネットワークとサブネットを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-140">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="73ec0-141">既存のネットワークとサブネットを使用するか、新しいネットワークとサブネットを作成できます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-141">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="73ec0-142">**[新規作成]** を選択した場合は、次のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-142">If you select **Create new**, the following dialog box appears:</span></span>

      ![[仮想ネットワークの作成] ダイアログ ボックス][CR06]

   * <span data-ttu-id="73ec0-144">**[パブリック IP アドレス]**: 仮想マシンの外部接続 IP アドレスを指定します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-144">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="73ec0-145">新しい IP アドレスを作成できます。仮想マシンのパブリック IP アドレスがない場合は、**[(なし)]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-145">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span> 

   * <span data-ttu-id="73ec0-146">**[ネットワーク セキュリティ グループ]**: 仮想マシンのネットワーク ファイアウォールを指定します (省略可能)。</span><span class="sxs-lookup"><span data-stu-id="73ec0-146">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="73ec0-147">既存のファイアウォールを選択できます。仮想マシンでネットワーク ファイアウォールを使用しない場合は、**[(なし)]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-147">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span> 

   * <span data-ttu-id="73ec0-148">**[可用性セット]**: 仮想マシンが属することができる可用性セットを指定します (省略可能)。</span><span class="sxs-lookup"><span data-stu-id="73ec0-148">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="73ec0-149">既存の可用性セットを指定するか、新しい可用性セットを作成できます。仮想マシンが可用性セットに属さない場合は、**[(なし)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-149">You can select an existing availability set, create a new availability set or, if your virtual machine will not belong to an availability set, select **(None)**.</span></span>

9. <span data-ttu-id="73ec0-150">**[完了]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-150">Click **Finish**.</span></span>  
    <span data-ttu-id="73ec0-151">新しい仮想マシンが Azure エクスプローラーのツール ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="73ec0-151">Your new virtual machine appears in the Azure Explorer tool window.</span></span> 

   ![Azure エクスプローラー ビューの新しい仮想マシン][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a><span data-ttu-id="73ec0-153">IntelliJ で仮想マシンを再起動する</span><span class="sxs-lookup"><span data-stu-id="73ec0-153">Restart a virtual machine in IntelliJ</span></span>

<span data-ttu-id="73ec0-154">IntelliJ で Azure Explorer を使用して仮想マシンを再起動するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-154">To restart a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="73ec0-155">**Azure Explorer** ビューで、仮想マシンを右クリックし、**[再起動]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-155">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![仮想マシンの [再起動] コマンド][RE01]

2. <span data-ttu-id="73ec0-157">確認ウィンドウで **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-157">In the confirmation window, click **Yes**.</span></span> 

   ![仮想マシンの再起動確認ウィンドウ][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a><span data-ttu-id="73ec0-159">IntelliJ で仮想マシンをシャットダウンする</span><span class="sxs-lookup"><span data-stu-id="73ec0-159">Shut down a virtual machine in IntelliJ</span></span>

<span data-ttu-id="73ec0-160">IntelliJ でAzure Explorer を使用して実行中の仮想マシンをシャットダウンするには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-160">To shut down a running virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="73ec0-161">**Azure Explorer** ビューで、仮想マシンを右クリックし、**[シャットダウン]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-161">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![仮想マシンの [シャットダウン] コマンド][SH01]

2. <span data-ttu-id="73ec0-163">確認ウィンドウで **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-163">In the confirmation window, click **Yes**.</span></span> 

   ![仮想マシンのシャットダウン確認ウィンドウ][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a><span data-ttu-id="73ec0-165">IntelliJ で仮想マシンを削除する</span><span class="sxs-lookup"><span data-stu-id="73ec0-165">Delete a virtual machine in IntelliJ</span></span>

<span data-ttu-id="73ec0-166">IntelliJ で Azure Explorer を使用して仮想マシンを削除するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-166">To delete a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="73ec0-167">**Azure Explorer** ビューで、仮想マシンを右クリックし、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ec0-167">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![仮想マシンの [削除] コマンド][DE01]

2. <span data-ttu-id="73ec0-169">確認ウィンドウで **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73ec0-169">In the confirmation window, click **Yes**.</span></span> 

   ![仮想マシンの削除確認ウィンドウ][DE02]

## <a name="next-steps"></a><span data-ttu-id="73ec0-171">次のステップ</span><span class="sxs-lookup"><span data-stu-id="73ec0-171">Next steps</span></span>

<span data-ttu-id="73ec0-172">Azure 仮想マシンのサイズと料金について詳しくは、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="73ec0-172">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="73ec0-173">Azure 仮想マシンのサイズ</span><span class="sxs-lookup"><span data-stu-id="73ec0-173">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="73ec0-174">[Azure の Windows 仮想マシンのサイズ]</span><span class="sxs-lookup"><span data-stu-id="73ec0-174">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="73ec0-175">[Azure の Linux 仮想マシンのサイズ]</span><span class="sxs-lookup"><span data-stu-id="73ec0-175">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="73ec0-176">Azure 仮想マシンの料金</span><span class="sxs-lookup"><span data-stu-id="73ec0-176">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="73ec0-177">[Windows 仮想マシンの料金]</span><span class="sxs-lookup"><span data-stu-id="73ec0-177">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="73ec0-178">[Linux 仮想マシンの料金]</span><span class="sxs-lookup"><span data-stu-id="73ec0-178">[Linux virtual-machine pricing]</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Toolkit for IntelliJ のサインイン手順]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Azure の Windows 仮想マシンのサイズ]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure の Linux 仮想マシンのサイズ]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 仮想マシンの料金]: /pricing/details/virtual-machines/windows/
[Linux 仮想マシンの料金]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
