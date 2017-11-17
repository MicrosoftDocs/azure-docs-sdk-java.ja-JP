---
title: "Azure Toolkit for Eclipse のサインイン手順"
description: "Azure Toolkit for Eclipse を使用して Microsoft Azure にサインインする方法について説明します。"
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
ms.openlocfilehash: 6c10d3e11dd75679fca0736e02d15de1d782dcec
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="3d905-103">Azure Toolkit for Eclipse の Azure サインイン手順</span><span class="sxs-lookup"><span data-stu-id="3d905-103">Azure Sign In Instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="3d905-104">Azure Toolkit for Eclipse には、Azure アカウントにサインインするための 2 つの方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="3d905-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="3d905-105">**自動** - この方法を使用する場合は、サービス プリンシパル データを含む資格情報ファイルを作成し、その後で、資格情報ファイルを使用して Azure アカウントに自動的にサインインできます。</span><span class="sxs-lookup"><span data-stu-id="3d905-105">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use the credentials file to automatically sign into your Azure account.</span></span>
  * <span data-ttu-id="3d905-106">**対話型** - この方法を使用する場合は、Azure アカウントにサインインするたびに Azure 資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="3d905-106">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>

<span data-ttu-id="3d905-107">以下のセクションの手順では、各方法の使用方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3d905-107">The steps in the following sections will describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a><span data-ttu-id="3d905-108">Azure アカウントに自動的にサインインし、今後使用する資格情報ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="3d905-108">Signing into your Azure account automatically and creating a credentials file to use in the future</span></span>

<span data-ttu-id="3d905-109">次の手順は、サービス プリンシパル データを格納する資格情報ファイルを作成する方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="3d905-109">The following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="3d905-110">これらの手順を完了すると、Eclipse は自動的に資格情報ファイルを使用し、プロジェクトを開くたびに Azure に自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="3d905-110">Once you have completed these steps, Eclipse will automatically use the credentials file to automatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="3d905-111">Eclipse でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="3d905-111">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="3d905-112">**[ツール]**、**[Azure]**、**[サインイン]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-112">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Azure サインイン用の Eclipse メニュー][A01]

1. <span data-ttu-id="3d905-114">**[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[自動]** を選択し、**[新規]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-114">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![[サインイン] ダイアログ ボックス][A02]

1. <span data-ttu-id="3d905-116">**[Azure Log In (Azure ログイン)]** ダイアログ ボックスが表示されたら、Azure 資格情報を入力し、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-116">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A03]

1. <span data-ttu-id="3d905-118">**[Create authentication files (認証ファイルの作成)]** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選んでから、**[開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-118">When the **Create authentication files** dialog box appears, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A04]

1. <span data-ttu-id="3d905-120">**[Service Principal Creatation Status (サービス プリンシパル作成ステータス)]** ダイアログ ボックスが表示されます。ファイルが正常に作成されたら **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-120">The **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![[Service Principal Creatation Status (サービス プリンシパル作成ステータス)] ダイアログ ボックス][A05]

1. <span data-ttu-id="3d905-122">**[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-122">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A06]

1. <span data-ttu-id="3d905-124">**[サブスクリプションの選択]** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-124">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="3d905-126">自動的にサインインした場合に Azure アカウントからサインアウトする</span><span class="sxs-lookup"><span data-stu-id="3d905-126">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="3d905-127">前のセクションの手順を構成した後、Eclipse を再起動するたびに Azure Toolkit によって Azure アカウントに自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="3d905-127">After you have configured the steps in the previous section, the Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="3d905-128">ただし、Azure アカウントからサインアウトし、Azure Toolkit が自動的にサインインしないようにするには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d905-128">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, use the following steps.</span></span>

1. <span data-ttu-id="3d905-129">Eclipse で、**[ツール]**、**[Azure]**、**[サインアウト]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-129">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Azure サインアウト用の Eclipse メニュー][L01]

1. <span data-ttu-id="3d905-131">**[Azure Sign Out (Azure サインアウト)]** ダイアログ ボックスが表示されたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-131">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![[サインアウト] ダイアログ ボックス][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="3d905-133">既に作成した資格情報ファイルを使用して Azure アカウントに自動的にサインインする</span><span class="sxs-lookup"><span data-stu-id="3d905-133">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="3d905-134">Eclipse を使用しているときに Azure からサインアウトする場合、Azure アカウントに自動的にサインインするには、資格情報ファイルを使用するように Azure Toolkit for Eclipse を再構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d905-134">If you sign out of Azure when you are using Eclipse, you will need to reconfigure the Azure Toolkit for Eclipse to use a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="3d905-135">次の手順は、既存の資格情報ファイルを使用するように Azure Toolkit を構成する方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="3d905-135">The following steps will walk you through configuring the Azure Toolkit to use an existing credentials file.</span></span>

1. <span data-ttu-id="3d905-136">Eclipse でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="3d905-136">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="3d905-137">**[ツール]**、**[Azure]**、**[サインイン]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-137">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Azure サインイン用の Eclipse メニュー][A01]

1. <span data-ttu-id="3d905-139">**[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[自動]** を選択し、**[参照]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-139">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![[サインイン] ダイアログ ボックス][A02]

1. <span data-ttu-id="3d905-141">**[Select Authenticated File (認証ファイルの選択)**] ダイアログ ボックスが表示されたら、前に作成した資格情報ファイルを選び、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-141">When the **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![[サインイン] ダイアログ ボックス][A08]

1. <span data-ttu-id="3d905-143">**[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-143">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A06]

1. <span data-ttu-id="3d905-145">**[サブスクリプションの選択]** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-145">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="3d905-147">Azure アカウントに対話形式でサインインする</span><span class="sxs-lookup"><span data-stu-id="3d905-147">Signing into your Azure account interactively</span></span>

<span data-ttu-id="3d905-148">次の手順は、Azure 資格情報を手動で入力して Azure にサインインする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="3d905-148">The following steps will illustrate how to sign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="3d905-149">Eclipse でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="3d905-149">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="3d905-150">**[ツール]**、**[Azure]**、**[サインイン]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-150">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Azure サインイン用の Eclipse メニュー][I01]

1. <span data-ttu-id="3d905-152">**[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[Interactive (対話型)]** を選択し、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-152">When the **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![[サインイン] ダイアログ ボックス][I02]

1. <span data-ttu-id="3d905-154">**[Azure Log In (Azure ログイン)]** ダイアログ ボックスが表示されたら、Azure 資格情報を入力し、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-154">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][I03]

1. <span data-ttu-id="3d905-156">**[サブスクリプションの選択]** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-156">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="3d905-158">対話形式でサインインした場合に Azure アカウントからサインアウトする</span><span class="sxs-lookup"><span data-stu-id="3d905-158">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="3d905-159">前のセクションの手順を構成した後、Eclipse を再起動するたびに Azure アカウントから自動的にサインアウトします。</span><span class="sxs-lookup"><span data-stu-id="3d905-159">After you have configured the steps in the previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="3d905-160">ただし、Eclipse を再起動せずに Azure アカウントからサインアウトするには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d905-160">However, if you want to sign out of your Azure account without restarting Eclipse, use the following steps.</span></span>

1. <span data-ttu-id="3d905-161">Eclipse で、**[ツール]**、**[Azure]**、**[サインアウト]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-161">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Azure サインアウト用の Eclipse メニュー][L01]

1. <span data-ttu-id="3d905-163">**[Azure Sign Out (Azure サインアウト)]** ダイアログ ボックスが表示されたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d905-163">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![[サインアウト] ダイアログ ボックス][L02]

## <a name="next-steps"></a><span data-ttu-id="3d905-165">次のステップ</span><span class="sxs-lookup"><span data-stu-id="3d905-165">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
