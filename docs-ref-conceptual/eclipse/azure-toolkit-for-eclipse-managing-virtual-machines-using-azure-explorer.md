---
title: Azure Explorer for Eclipse を使用して仮想マシンを管理する
description: Azure Explorer for Eclipse を使用して Azure 仮想マシンを管理する方法について説明します。
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
ms.openlocfilehash: c04f5225f0bb99898f69b26a4782aa57d75c4f22
ms.sourcegitcommit: 0ed7c5af0152125322ff1d265c179f35028f3c15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38074567"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="9a2ec-103">Azure Explorer for Eclipse を使用して仮想マシンを管理する</span><span class="sxs-lookup"><span data-stu-id="9a2ec-103">Manage virtual machines by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="9a2ec-104">Azure Toolkit for Eclipse の一部である Azure Explorer は、Eclipse 統合開発環境 (IDE) 内から Azure アカウントの仮想マシンを管理するための使いやすいソリューションを Java 開発者に提供します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9a2ec-105">Eclipse で仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="9a2ec-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="9a2ec-106">Azure Explorer を使用して仮想マシンを作成するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="9a2ec-107">「[Azure Toolkit for Eclipse のサインイン手順](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)」に従って Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).</span></span>

2. <span data-ttu-id="9a2ec-108">**Azure Explorer** ビューで、**[Azure]** ノードを展開し、**[仮想マシン]** を右クリックし、**[VM の作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   ![[VM の作成] コマンド][CR01]  

   <span data-ttu-id="9a2ec-110">**[新しい仮想マシンの作成]** ウィザードが開きます。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="9a2ec-111">**[サブスクリプションの選択]** ウィンドウで、サブスクリプションを選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![[サブスクリプションの選択] ウィンドウ][CR02]

4. <span data-ttu-id="9a2ec-113">**[仮想マシン イメージの選択]** ウィンドウで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="9a2ec-114">**[場所]**: 仮想マシンを作成する場所を指定します ("*米国西部*" など)。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="9a2ec-115">**[発行者]**: 仮想マシンの作成に使用するイメージを作成した発行元を指定します (*Microsoft* など)。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-115">**Publisher**: Specifies the publisher that created the image you'll use to create your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="9a2ec-116">
  **[プラン]**: 選択した発行元の、仮想マシンで使用するプランを指定します (*JDK* など)。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-116">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="9a2ec-117">
  **[SKU]**: 選択したプランで使用する在庫保管単位 (SKU) を指定します (*JDK_8* など)。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-117">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="9a2ec-118">**[Version #]\(バージョン番号\)**: 選択した SKU で使用するバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-118">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![[仮想マシン イメージの選択] ウィンドウ][CR03]

5. <span data-ttu-id="9a2ec-120">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-120">Click **Next**.</span></span>

6. <span data-ttu-id="9a2ec-121">**[仮想マシンの基本設定]** ウィンドウで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-121">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="9a2ec-122">**[仮想マシン名]**: 新しい仮想マシンの名前を指定します。英字、数字、ハイフンのみ使用でき、先頭には英字を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-122">**Virtual Machine Name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="9a2ec-123">**[サイズ]**: 仮想マシンに割り当てるコアとメモリの数を指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-123">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="9a2ec-124">**[ユーザー名]**: 仮想マシンを管理するために作成する管理者アカウントを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-124">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="9a2ec-125">**[パスワード]** と **[確認]**: 管理者アカウントのパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-125">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![[仮想マシンの基本設定] ウィンドウ][CR04]

7. <span data-ttu-id="9a2ec-127">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-127">Click **Next**.</span></span>

8. <span data-ttu-id="9a2ec-128">**[新しいストレージ アカウントの作成]** ウィンドウで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-128">In the **Create New Storage Account** window, enter the following information:</span></span>

   * <span data-ttu-id="9a2ec-129">**[リソース グループ]**: 仮想マシン用のリソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-129">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="9a2ec-130">次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-130">Select one of the following options:</span></span>
     * <span data-ttu-id="9a2ec-131">**[新規作成]**: 新しいリソース グループを作成することを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-131">**Create new**: Specifies that you want to create a new resource group.</span></span>
     * <span data-ttu-id="9a2ec-132">**[既存のものを使用]**: Azure アカウントに既に関連付けられているリソース グループの一覧から選択することを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-132">**Use existing**: Specifies that you want to select a resource group that is already associated with your Azure account.</span></span>

       ![[新しいストレージ アカウントの作成] ダイアログ ボックス][CR05]

   * <span data-ttu-id="9a2ec-134">**[ストレージ アカウント]**: 仮想マシンを格納するために使用するストレージ アカウントを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-134">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="9a2ec-135">既存のストレージ アカウントを使用するか、新しいアカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="9a2ec-136">**[仮想ネットワーク]** と **[サブネット]**: 仮想マシンを接続する仮想ネットワークとサブネットを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-136">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="9a2ec-137">既存のネットワークとサブネットを使用するか、新しいネットワークとサブネットを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="9a2ec-138">**[新規作成]** を選択した場合は、次のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-138">If you select **Create new**, the following dialog box is displayed:</span></span>

      ![[新しい仮想ネットワークの作成] ダイアログ ボックス][CR06]

9. <span data-ttu-id="9a2ec-140">**[関連するリソース]** ウィンドウで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-140">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="9a2ec-141">**[パブリック IP アドレス]**: 仮想マシンの外部接続 IP アドレスを指定します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="9a2ec-142">新しい IP アドレスを作成できます。仮想マシンのパブリック IP アドレスがない場合は、**[(なし)]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-142">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="9a2ec-143">**[ネットワーク セキュリティ グループ]**: 仮想マシンのネットワーク ファイアウォールを指定します (省略可能)。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="9a2ec-144">既存のファイアウォールを選択できます。仮想マシンでネットワーク ファイアウォールを使用しない場合は、**[(なし)]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="9a2ec-145">**[可用性セット]**: 仮想マシンが属することができる可用性セットを指定します (省略可能)。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="9a2ec-146">既存の可用性セットを指定するか、新しい可用性セットを作成できます。仮想マシンが可用性セットに属さない場合は、**[(なし)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong to an availability set, you can select **(None)**.</span></span>

   ![[関連するリソース] ウィンドウ][CR07]

10. <span data-ttu-id="9a2ec-148">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-148">Click **Finish**.</span></span>  

    <span data-ttu-id="9a2ec-149">新しい仮想マシンが Azure エクスプローラーのツール ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-149">Your new virtual machine is displayed in the Azure Explorer tool window.</span></span>

    ![新しい仮想マシン][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9a2ec-151">Eclipse で仮想マシンを再起動する</span><span class="sxs-lookup"><span data-stu-id="9a2ec-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="9a2ec-152">Eclipse で Azure Explorer を使用して仮想マシンを再起動するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-152">To restart a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="9a2ec-153">**Azure Explorer** ビューで、仮想マシンを右クリックし、**[再起動]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-153">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![仮想マシンの [再起動] コマンド][RE01]

1. <span data-ttu-id="9a2ec-155">確認ウィンドウで **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-155">In the confirmation window, click **Yes**.</span></span>

   ![再起動の確認ウィンドウ][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9a2ec-157">Eclipse で仮想マシンをシャットダウンする</span><span class="sxs-lookup"><span data-stu-id="9a2ec-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="9a2ec-158">Eclipse で Azure Explorer を使用して実行中の仮想マシンをシャットダウンするには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-158">To shut down a running virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="9a2ec-159">**Azure Explorer** ビューで、仮想マシンを右クリックし、**[シャットダウン]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-159">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![仮想マシンの [シャットダウン] コマンド][SH01]

1. <span data-ttu-id="9a2ec-161">確認ウィンドウで **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-161">In the confirmation window, click **Yes**.</span></span>

   ![仮想マシンのシャットダウン確認ウィンドウ][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="9a2ec-163">Eclipse で仮想マシンを削除する</span><span class="sxs-lookup"><span data-stu-id="9a2ec-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="9a2ec-164">Eclipse で Azure Explorer を使用して仮想マシンを削除するには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-164">To delete a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="9a2ec-165">**Azure Explorer** ビューで、仮想マシンを右クリックし、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-165">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![仮想マシンの [削除] コマンド][DE01]

1. <span data-ttu-id="9a2ec-167">確認ウィンドウで **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-167">In the confirmation window, click **Yes**.</span></span>

   ![仮想マシンの削除確認ウィンドウ][DE02]

## <a name="next-steps"></a><span data-ttu-id="9a2ec-169">次の手順</span><span class="sxs-lookup"><span data-stu-id="9a2ec-169">Next steps</span></span>

<span data-ttu-id="9a2ec-170">Azure 仮想マシンのサイズと料金について詳しくは、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9a2ec-170">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="9a2ec-171">Azure 仮想マシンのサイズ</span><span class="sxs-lookup"><span data-stu-id="9a2ec-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="9a2ec-172">[Azure の Windows 仮想マシンのサイズ]</span><span class="sxs-lookup"><span data-stu-id="9a2ec-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="9a2ec-173">[Azure の Linux 仮想マシンのサイズ]</span><span class="sxs-lookup"><span data-stu-id="9a2ec-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="9a2ec-174">Azure 仮想マシンの料金</span><span class="sxs-lookup"><span data-stu-id="9a2ec-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="9a2ec-175">[Windows 仮想マシンの料金]</span><span class="sxs-lookup"><span data-stu-id="9a2ec-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="9a2ec-176">[Linux 仮想マシンの料金]</span><span class="sxs-lookup"><span data-stu-id="9a2ec-176">[Linux virtual-machine pricing]</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure の Windows 仮想マシンのサイズ]: /azure/virtual-machines/virtual-machines-windows-sizes
[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure の Linux 仮想マシンのサイズ]: /azure/virtual-machines/virtual-machines-linux-sizes
[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 仮想マシンの料金]: /pricing/details/virtual-machines/windows/
[Windows virtual-machine pricing]: /pricing/details/virtual-machines/windows/
[Linux 仮想マシンの料金]: /pricing/details/virtual-machines/linux/
[Linux virtual-machine pricing]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
