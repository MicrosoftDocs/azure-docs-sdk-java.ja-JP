---
title: Azure Toolkit for Eclipse のサインイン手順
description: Azure Toolkit for Eclipse を使用して Microsoft Azure にサインインする方法について説明します。
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
ms.openlocfilehash: b4b13de38913ae6e7ae2bb09210ac742efc9d0ad
ms.sourcegitcommit: d18d9dce22b7f7af178f756bd341433d24e3c3b5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2019
ms.locfileid: "66575311"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="03ae8-103">Azure Toolkit for Eclipse のサインイン手順</span><span class="sxs-lookup"><span data-stu-id="03ae8-103">Sign-in instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="03ae8-104">Azure Toolkit for Eclipse には、Azure アカウントにサインインするための 2 つの方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="03ae8-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  - <span data-ttu-id="03ae8-105">[[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-device-login)</span><span class="sxs-lookup"><span data-stu-id="03ae8-105">[Sign in to your Azure account by Device Login](#sign-in-to-your-azure-account-by-device-login)</span></span>
  - <span data-ttu-id="03ae8-106">[[サービス プリンシパル] によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-service-principal)</span><span class="sxs-lookup"><span data-stu-id="03ae8-106">[Sign in to your Azure account by Service Principal](#sign-in-to-your-azure-account-by-service-principal)</span></span>

<span data-ttu-id="03ae8-107">[**サインアウト**](#sign-out-of-your-azure-account)方法も用意されています。</span><span class="sxs-lookup"><span data-stu-id="03ae8-107">[**Sign out**](#sign-out-of-your-azure-account) methods are also provided.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a><span data-ttu-id="03ae8-108">[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする</span><span class="sxs-lookup"><span data-stu-id="03ae8-108">Sign in to your Azure account by Device Login</span></span>

<span data-ttu-id="03ae8-109">[Device Login]\(デバイスのログイン\) によって Azure にサインインするには、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="03ae8-109">To sign in Azure by device login, do the following:</span></span>

1. <span data-ttu-id="03ae8-110">Eclipse でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="03ae8-110">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="03ae8-111">**[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="03ae8-112">![Azure サインイン用の Eclipse メニュー][I01]</span><span class="sxs-lookup"><span data-stu-id="03ae8-112">![Eclipse Menu for Azure Sign In][I01]</span></span>

3. <span data-ttu-id="03ae8-113">**[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、 **[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-113">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in**.</span></span>

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

4. <span data-ttu-id="03ae8-115">**[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-115">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

> [!NOTE]
>
> <span data-ttu-id="03ae8-117">ブラウザーが開かない場合は、Internet Explorer、Firefox、Chrome などの外部ブラウザーを使用するように Eclipse を構成します。</span><span class="sxs-lookup"><span data-stu-id="03ae8-117">If the browser doesn't open, configure Eclipse to use an external browser like Internet Explorer, Firefox, or Chrome:</span></span>
>
> 1. <span data-ttu-id="03ae8-118">[Preferences]\(設定\) - > [General]\(全般\) - > [Web Browser ]\(Web ブラウザー\) - > [Use external web browser in Eclipse]\(Eclipse で外部 Web ブラウザーを使用する\) の順に開きます。</span><span class="sxs-lookup"><span data-stu-id="03ae8-118">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="03ae8-119">使用するブラウザーを選択します</span><span class="sxs-lookup"><span data-stu-id="03ae8-119">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="03ae8-120">ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-120">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![デバイスのログイン ブラウザー][I04]

6. <span data-ttu-id="03ae8-122">最後に、 **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-122">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a><span data-ttu-id="03ae8-124">[サービス プリンシパル] によって Azure アカウントにサインインする</span><span class="sxs-lookup"><span data-stu-id="03ae8-124">Sign in to your Azure account by Service Principal</span></span>

<span data-ttu-id="03ae8-125">このセクションでは、サービス プリンシパル データを格納する資格情報ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="03ae8-125">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="03ae8-126">このプロセスが完了すると、Eclipse は資格情報ファイルを使用して、プロジェクトを開いたときにユーザーを自動的に Azure にサインインします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-126">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure when open your project.</span></span>

1. <span data-ttu-id="03ae8-127">Eclipse でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="03ae8-127">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="03ae8-128">**[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-128">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="03ae8-129">![Eclipse Azure サインイン コマンド][A01]</span><span class="sxs-lookup"><span data-stu-id="03ae8-129">![The Eclipse Azure Sign In command][A01]</span></span>

3. <span data-ttu-id="03ae8-130">**[Azure サインイン]** ウィンドウで、 **[サービス プリンシパル]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="03ae8-130">In the **Azure Sign In** window, select **Service Principal**.</span></span> <span data-ttu-id="03ae8-131">サービス プリンシパル認証ファイルがまだない場合は、 **[新規]** をクリックして作成します。</span><span class="sxs-lookup"><span data-stu-id="03ae8-131">If you do not have the service principal authentication file yet, click **New** to create one.</span></span> <span data-ttu-id="03ae8-132">そうでない場合は、 **[参照]** をクリックしてそれを開き、ステップ 8 に進むことができます。</span><span class="sxs-lookup"><span data-stu-id="03ae8-132">Otherwise you can click **Browse** to open it and jump to step 8.</span></span>

   ![[サービス プリンシパル] が選択されている [Azure サインイン] ウィンドウ][A02]

4. <span data-ttu-id="03ae8-134">**[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-134">Click **Copy&Open** in **Azure Device Login** dialog.</span></span>

   ![[Azure ログイン] ダイアログ ウィンドウ][A08]

> [!NOTE]
>
> <span data-ttu-id="03ae8-136">ブラウザーが開かない場合は、IE や Chrome などの外部ブラウザーを使用するように Eclipse を構成します。</span><span class="sxs-lookup"><span data-stu-id="03ae8-136">If the browser doesn't open, configure eclipse to use an external browser like IE or Chrome:</span></span>
>
> 1. <span data-ttu-id="03ae8-137">[Preferences]\(設定\) - > [General]\(全般\) - > [Web Browser ]\(Web ブラウザー\) - > [Use external web browser in Eclipse]\(Eclipse で外部 Web ブラウザーを使用する\) の順に開きます。</span><span class="sxs-lookup"><span data-stu-id="03ae8-137">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="03ae8-138">使用するブラウザーを選択します</span><span class="sxs-lookup"><span data-stu-id="03ae8-138">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="03ae8-139">ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-139">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![デバイスのログイン ブラウザー][A03]

6. <span data-ttu-id="03ae8-141">**[Create authentication files]\(認証ファイルの作成\)** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選択し、 **[開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-141">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![[Create authentication files]\(認証ファイルの作成\) ウィンドウ][A04]

7. <span data-ttu-id="03ae8-143">**[Service Principal Creatation Status]\(サービス プリンシパル作成状態\)** ダイアログ ボックスで、ファイルが正常に作成されたら **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-143">In the **Service Principal Creation Status** dialog box, click **OK** after your files have been created successfully.</span></span>

   ![[Service Principal Creatation Status]\(サービス プリンシパル作成ステータス\) ダイアログ ボックス][A05]

8. <span data-ttu-id="03ae8-145">作成されたファイルのアドレスは **[Azure サインイン]** ウィンドウに自動的に入力されるため、ここで **[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-145">Address of the created file will be automatically filled in the **Azure Sign In** window, now click **Sign in**.</span></span>

   ![Azure のログイン ダイアログ ボックス][A06]

9. <span data-ttu-id="03ae8-147">最後に、 **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-147">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="sign-out-of-your-azure-account"></a><span data-ttu-id="03ae8-149">Azureアカウントからサインアウトする</span><span class="sxs-lookup"><span data-stu-id="03ae8-149">Sign out of your Azure account</span></span>

<span data-ttu-id="03ae8-150">前の手順によってアカウントを構成すると、Eclipse を起動するたびに自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-150">After you have configured your account by preceding steps, you will be automatically signed in each time you start Eclipse.</span></span> <span data-ttu-id="03ae8-151">ただし、Azure アカウントからサインアウトするには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="03ae8-151">However, if you want to sign out of your Azure account, use the following steps.</span></span>

1. <span data-ttu-id="03ae8-152">Eclipse で、 **[ツール]** 、 **[Azure]** 、 **[サインアウト]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-152">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Azure サインアウト用の Eclipse メニュー][L01]

2. <span data-ttu-id="03ae8-154">**[Azure Sign Out (Azure サインアウト)]** ダイアログ ボックスが表示されたら、 **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03ae8-154">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![[サインアウト] ダイアログ ボックス][L02]

## <a name="next-steps"></a><span data-ttu-id="03ae8-156">次の手順</span><span class="sxs-lookup"><span data-stu-id="03ae8-156">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

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
