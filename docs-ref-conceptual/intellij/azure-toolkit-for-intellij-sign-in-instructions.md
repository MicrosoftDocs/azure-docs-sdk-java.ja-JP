---
title: "Azure Toolkit for IntelliJ のサインイン手順"
description: "Azure Toolkit for IntelliJ を使用して Microsoft Azure にサインインする方法について説明します。"
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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 25c25e58b079c1e08d62feff389b899b26e82b5e
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/18/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a><span data-ttu-id="e11e9-103">Azure Toolkit for IntelliJ のサインイン手順</span><span class="sxs-lookup"><span data-stu-id="e11e9-103">Sign-in instructions for the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="e11e9-104">Azure Toolkit for IntelliJ には、Azure アカウントにサインインするための 2 つの方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="e11e9-104">The Azure Toolkit for IntelliJ provides two methods for signing in to your Azure account:</span></span>

  * <span data-ttu-id="e11e9-105">**自動**: Azure アカウントに自動的にサインインするために使用できる資格情報ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="e11e9-105">**Automated**: You create a credentials file that you can use to automatically sign in to your Azure account.</span></span>
  * <span data-ttu-id="e11e9-106">**対話型**: Azure アカウントにサインインするたびに、Azure 資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="e11e9-106">**Interactive**: You enter your Azure credentials each time you sign in to your Azure account.</span></span>

<span data-ttu-id="e11e9-107">以下のセクションで、各方法の使用方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e11e9-107">The following sections describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-automatically"></a><span data-ttu-id="e11e9-108">Azure アカウントに自動的にサインインする</span><span class="sxs-lookup"><span data-stu-id="e11e9-108">Sign in to your Azure account automatically</span></span>

<span data-ttu-id="e11e9-109">このセクションでは、サービス プリンシパル データを格納する資格情報ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="e11e9-109">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="e11e9-110">このプロセスを完了すると、プロジェクトを開くたびに、Eclipse によって資格情報ファイルが自動的に使用されて、Azure に自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-110">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure each time you open your project.</span></span>

1. <span data-ttu-id="e11e9-111">IntelliJ IDEA でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="e11e9-111">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="e11e9-112">**[ツール]** メニューの **[Azure]** をポイントし、**[Azure サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-112">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![IntelliJ Azure サインイン コマンド][A01]

1. <span data-ttu-id="e11e9-114">**[Azure サインイン]** ダイアログ ボックスで、**[自動]** を選択し、**[新規]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-114">In the **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![[自動] が選択されている [Azure サインイン] ウィンドウ][A02]

1. <span data-ttu-id="e11e9-116">**[Azure ログイン]** ダイアログ ウィンドウで、Azure 資格情報を入力し、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-116">In the **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![[Azure ログイン] ダイアログ ウィンドウ][A03]

1. <span data-ttu-id="e11e9-118">**[Create authentication files]\(認証ファイルの作成\)** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選択し、**[開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-118">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![[Create authentication files]\(認証ファイルの作成\) ウィンドウ][A04]

1. <span data-ttu-id="e11e9-120">**[Service Principal Creatation Status]\(サービス プリンシパル作成ステータス\)** ダイアログ ボックスで、ファイルが正常に作成されたら **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-120">In the **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![[Service Principal Creatation Status]\(サービス プリンシパル作成ステータス\) ダイアログ ボックス][A05]

1. <span data-ttu-id="e11e9-122">**[Azure サインイン]** ウィンドウで、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-122">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![[Azure ログイン] ダイアログ ボックス][A06]

1. <span data-ttu-id="e11e9-124">**[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-124">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="e11e9-126">自動的にサインインした後で Azure アカウントからサインアウトする</span><span class="sxs-lookup"><span data-stu-id="e11e9-126">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="e11e9-127">前の手順を使用してアカウントを構成すると、IntelliJ IDEA を再起動するたびに Azure Toolkit によって Azure アカウントに自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-127">After you have configured your account by using the preceding steps, the Azure Toolkit automatically signs you in to your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="e11e9-128">ただし、Azure アカウントからサインアウトし、Azure Toolkit による自動的なサインインが行われないようにするには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="e11e9-128">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, do the following:</span></span>

1. <span data-ttu-id="e11e9-129">IntelliJ IDEA で、**[ツール]** メニューの **[Azure]** をポイントし、**[Azure サインアウト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-129">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![IntelliJ Azure サインアウト コマンド][L01]

1. <span data-ttu-id="e11e9-131">**[Azure サインアウト]** 確認ウィンドウで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-131">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![[Azure サインアウト] 確認ウィンドウ][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="e11e9-133">既存の資格情報ファイルを使用して Azure アカウントに自動的にサインインする</span><span class="sxs-lookup"><span data-stu-id="e11e9-133">Sign in to your Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="e11e9-134">IntelliJ IDEA を使用しているときに Azure アカウントからサインアウトした場合は、既存の資格情報ファイルを使用してアカウントに自動的にサインインしなおす必要があります。</span><span class="sxs-lookup"><span data-stu-id="e11e9-134">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file to automatically sign back in to the account.</span></span> <span data-ttu-id="e11e9-135">既存の視覚情報ファイルを使用するように Azure Toolkit for Eclipse を構成するには、以下の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e11e9-135">To configure the Azure Toolkit for Eclipse to use an existing credentials file, do the following:</span></span>

1. <span data-ttu-id="e11e9-136">IntelliJ IDEA でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="e11e9-136">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="e11e9-137">**[ツール]** メニューの **[Azure]** をポイントし、**[Azure サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-137">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![IntelliJ Azure サインイン コマンド][A01]

1. <span data-ttu-id="e11e9-139">**[Azure サインイン]** ウィンドウで、**[自動]** を選択し、**[参照]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-139">In the **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![[自動] が選択されている [Azure サインイン] ウィンドウ][A02]

1. <span data-ttu-id="e11e9-141">**[Select Authentication File]\(認証ファイルの選択\)** ダイアログ ボックスで、作成済みの資格情報ファイルを選択し、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-141">In the **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![[Select Authentication File]\(認証ファイルの選択\) ダイアログ ボックス][A08]

1. <span data-ttu-id="e11e9-143">**[Azure サインイン]** ウィンドウで、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-143">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![[自動] が選択されている [Azure サインイン] ウィンドウ][A06]

1. <span data-ttu-id="e11e9-145">**[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-145">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="sign-in-to-your-azure-account-interactively"></a><span data-ttu-id="e11e9-147">Azure アカウントに対話形式でサインインする</span><span class="sxs-lookup"><span data-stu-id="e11e9-147">Sign in to your Azure account interactively</span></span>

<span data-ttu-id="e11e9-148">Azure 資格情報を手動で入力して Azure にサインインするには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e11e9-148">To sign in to Azure by manually entering your Azure credentials, do the following:</span></span>

1. <span data-ttu-id="e11e9-149">IntelliJ IDEA でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="e11e9-149">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="e11e9-150">**[ツール]** をクリックし、**[Azure]** をポイントし、**[Azure サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-150">Click **Tools**, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![IntelliJ Azure サインイン コマンド][I01]

1. <span data-ttu-id="e11e9-152">**[Azure サインイン]** ウィンドウで、**[対話型]** を選択し、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-152">In the **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![[対話型] が選択されている [Azure サインイン] ウィンドウ][I02]

1. <span data-ttu-id="e11e9-154">**[Azure ログイン]** ダイアログ ボックスが表示されたら、Azure 資格情報を入力し、**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-154">In the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

1. <span data-ttu-id="e11e9-156">**[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-156">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="e11e9-158">対話形式でサインインした後で Azure アカウントからサインアウトする</span><span class="sxs-lookup"><span data-stu-id="e11e9-158">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="e11e9-159">前の手順を使用してアカウントを構成すると、IntelliJ IDEA を再起動するたびに Azure アカウントから自動的にサインアウトします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-159">After you have configured your account by using the preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="e11e9-160">ただし、IntelliJ IDEA を*再起動せずに* Azure アカウントからサインアウトするには、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="e11e9-160">However, if you want to sign out of your Azure account *without* restarting IntelliJ IDEA, do the following.</span></span>

1. <span data-ttu-id="e11e9-161">IntelliJ IDEA で、**[ツール]** メニューの **[Azure]** をポイントし、**[Azure サインアウト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-161">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![IntelliJ Azure サインアウト コマンド][L01]

1. <span data-ttu-id="e11e9-163">**[Azure サインアウト]** 確認ウィンドウで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e11e9-163">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![[Azure サインアウト] 確認ウィンドウ][L02]

## <a name="next-steps"></a><span data-ttu-id="e11e9-165">次のステップ</span><span class="sxs-lookup"><span data-stu-id="e11e9-165">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
