---
title: Eclipse を使用して Azure App Service 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for Eclipse を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
keywords: java, eclipse, web アプリ, azure app service, hello world, クイック スタート
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 7e88298afaf0b4601d85d6063b7096c79e677421
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625841"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a><span data-ttu-id="9427e-104">Eclipse を使用して Azure App Service 用の Hello World Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="9427e-104">Create a Hello World web app for Azure App Service using Eclipse</span></span>

<span data-ttu-id="9427e-105">オープンソースの [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) プラグインを使用すれば、数分で、基本的な Hello World アプリケーション作成して Web アプリとして Azure App Service にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="9427e-105">Using open sourced [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9427e-106">IntelliJ IDEA を使用したい場合は、[IntelliJ 用の同様のチュートリアル][intellij-hello-world]をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9427e-106">If you prefer using IntelliJ IDEA, check out our [similar tutorial for IntelliJ][intellij-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="9427e-107">このチュートリアルを完了したら、忘れずにリソースをクリーンアップしてください。</span><span class="sxs-lookup"><span data-stu-id="9427e-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="9427e-108">その場合、このガイドの実行によって無料アカウントのクォータが超過することはありません。</span><span class="sxs-lookup"><span data-stu-id="9427e-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="9427e-109">インストールとサインイン</span><span class="sxs-lookup"><span data-stu-id="9427e-109">Installation and sign-in</span></span>

1. <span data-ttu-id="9427e-110">次のボタンを実行中の Eclipse ワークスペースにドラッグして、Azure Toolkit for Eclipse プラグインをインストールします ([他のインストール オプション](azure-toolkit-for-eclipse-installation.md))。</span><span class="sxs-lookup"><span data-stu-id="9427e-110">Drag the following button to your running Eclipse workspace to install the Azure Toolkit for Eclipse plugin ([other installation options](azure-toolkit-for-eclipse-installation.md)).</span></span>

    <span data-ttu-id="9427e-111">[![実行中の Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "実行中の Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要")</span><span class="sxs-lookup"><span data-stu-id="9427e-111">[![Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client")</span></span>

1. <span data-ttu-id="9427e-112">Azure アカウントにサインインするには、 **[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-112">To sign in to your Azure account, click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="9427e-113">![Azure サインイン用の Eclipse メニュー][I01]</span><span class="sxs-lookup"><span data-stu-id="9427e-113">![Eclipse Menu for Azure Sign In][I01]</span></span>

1. <span data-ttu-id="9427e-114">**[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、次に **[サインイン]** をクリックします ([他のサインイン オプション](azure-toolkit-for-eclipse-sign-in-instructions.md))。</span><span class="sxs-lookup"><span data-stu-id="9427e-114">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign-in options](azure-toolkit-for-eclipse-sign-in-instructions.md)).</span></span>

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

1. <span data-ttu-id="9427e-116">**[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-116">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

1. <span data-ttu-id="9427e-118">ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-118">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![デバイスのログイン ブラウザー][I04]

1. <span data-ttu-id="9427e-120">最後に、 **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-120">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="9427e-122">Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="9427e-122">Creating web app project</span></span>

1. <span data-ttu-id="9427e-123">**[ファイル]** 、 **[新規]** 、 **[Dynamic Web Project]\(動的 Web プロジェクト\)** の順にクリックします</span><span class="sxs-lookup"><span data-stu-id="9427e-123">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="9427e-124">( **[ファイル]** と **[新規]** のクリック後、使用可能なプロジェクトとして **[Dynamic Web Project (動的 Web プロジェクト)]** が表示されない場合は、 **[ファイル]** 、 **[新規]** 、 **[プロジェクト]** の順にクリックし、 **[Web]** を展開して、 **[Dynamic Web Project (動的 Web プロジェクト)]** 、 **[次へ]** の順にクリックします)。</span><span class="sxs-lookup"><span data-stu-id="9427e-124">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![新しい動的 Web プロジェクトの作成][file-new-dynamic-web-project]

2. <span data-ttu-id="9427e-126">このチュートリアルでは、プロジェクトに **MyWebApp**という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="9427e-126">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="9427e-127">画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9427e-127">Your screen will appear similar to the following:</span></span>
   
   ![新しい動的 Web プロジェクトのプロパティ][dynamic-web-project-properties]

3. <span data-ttu-id="9427e-129">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-129">Click **Finish**.</span></span>

4. <span data-ttu-id="9427e-130">Eclipse の Project Explorer ビューで、 **[MyWebApp]** を展開します。</span><span class="sxs-lookup"><span data-stu-id="9427e-130">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="9427e-131">**WebContent** を右クリックし、 **[新規]** 、 **[JSP ファイル]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-131">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![新しい JSP ファイルの作成][create-new-jsp-file]

5. <span data-ttu-id="9427e-133">**[New JSP File (新しい JSP ファイル)]** ダイアログ ボックスで **index.jsp** ファイルに名前を付け、親フォルダーは **MyWebApp/WebContent** のままにして **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-133">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![[New JSP File\(新しい JSP ファイル\)] ダイアログ ボックス][new-jsp-file-dialog]

6. <span data-ttu-id="9427e-135">**[Select JSP Template (JSP テンプレートの選択)]** ダイアログ ボックスで、このチュートリアルのために **[New JSP File (html) (新しい JSP ファイル (html))]** を選択し、 **[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-135">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![JSP テンプレートの選択][select-jsp-template]

7. <span data-ttu-id="9427e-137">index.jsp ファイルが Eclipse で開いたら、"**Hello World!** " を動的に表示するためのテキストを</span><span class="sxs-lookup"><span data-stu-id="9427e-137">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="9427e-138">既存の `<body>` 要素に追加します。</span><span class="sxs-lookup"><span data-stu-id="9427e-138">within the existing `<body>` element.</span></span> <span data-ttu-id="9427e-139">更新された `<body>` コンテンツは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9427e-139">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="9427e-140">index.jsp を保存します。</span><span class="sxs-lookup"><span data-stu-id="9427e-140">Save index.jsp.</span></span>

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="9427e-141">Azure への Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="9427e-141">Deploying web app to Azure</span></span>

1. <span data-ttu-id="9427e-142">Eclipse のプロジェクト エクスプローラー ビューでプロジェクトを右クリックし、 **[Azure]** 、 **[Publish as Azure Web App]\(Azure Web アプリとして発行\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="9427e-142">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![[Publish as Azure Web App (Azure Web アプリとして発行)]][publish-as-azure-web-app]

1. <span data-ttu-id="9427e-144">**[Deploy Web App]\(Web アプリのデプロイ\)** ダイアログ ボックスが表示されたら、次のいずれかのオプションを選択できます。</span><span class="sxs-lookup"><span data-stu-id="9427e-144">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="9427e-145">既存の Web アプリを選択します (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="9427e-145">Select an existing web app if one exists.</span></span>

      ![アプリ サービスの選択][select-app-service]

   * <span data-ttu-id="9427e-147">**[Create New Web App]\(新しい Web アプリを作成する\)** をクリックする。</span><span class="sxs-lookup"><span data-stu-id="9427e-147">Click **Create New Web App**.</span></span>

      ![App Service を作成する][create-app-service]

      <span data-ttu-id="9427e-149">**[App Service の作成]** ダイアログ ボックスで Web アプリに必要な情報を指定し、 **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-149">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      <span data-ttu-id="9427e-150">ここで、ランタイム環境、アプリ設定、サービス プラン、およびリソース グループを構成できます。</span><span class="sxs-lookup"><span data-stu-id="9427e-150">Here you can configure the runtime environment, app settings, service plan and resource group.</span></span>

      ![[App Service の作成] ダイアログ ボックス][create-app-service-dialog]

1. <span data-ttu-id="9427e-152">Web アプリを選択し、 **[デプロイ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9427e-152">Select your web app and then click **Deploy**.</span></span>

   ![アプリ サービスのデプロイ][deploy-app-service]

1. <span data-ttu-id="9427e-154">ツールキットにより Web アプリが正常にデプロイされると、 **[Azure の活動ログ]** タブに **[発行済み]** 状態として表示され、デプロイされた Web アプリの URL へのハイパーリンクが設定されます。</span><span class="sxs-lookup"><span data-stu-id="9427e-154">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![発行済み状態][publish-status]

1. <span data-ttu-id="9427e-156">ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="9427e-156">You can browse to your web app using the link provided in the status message.</span></span>

   ![Web アプリの参照][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="cleaning-up-resources"></a><span data-ttu-id="9427e-158">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="9427e-158">Cleaning up resources</span></span>

1. <span data-ttu-id="9427e-159">Web アプリを Azure に発行した後、それを管理するには、Azure Explorer で右クリックし、コンテキスト メニューからオプションのいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="9427e-159">After you have published your web app to Azure, you can manage it by right-clicking in Azure Explorer and selecting one of the options in the context menu.</span></span> <span data-ttu-id="9427e-160">たとえば、ここで Web アプリを **[削除]** して、このチュートリアルのリソースをクリーンアップできます。</span><span class="sxs-lookup"><span data-stu-id="9427e-160">For example, you can **Delete** your web app here to clean up the resource for this tutorial.</span></span>

   ![アプリ サービスの管理][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="9427e-162">次の手順</span><span class="sxs-lookup"><span data-stu-id="9427e-162">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="9427e-163">Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9427e-163">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->
[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

[browse-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/browse-web-app.png
[file-new-dynamic-web-project]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/file-new-dynamic-web-project.png
[dynamic-web-project-properties]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/dynamic-web-project-properties.png
[create-new-jsp-file]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-new-jsp-file.png
[new-jsp-file-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/new-jsp-file-dialog.png
[select-jsp-template]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-jsp-template.png
[publish-as-azure-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-as-azure-web-app.png
[deploy-web-app-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-web-app-dialog.png
[select-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-app-service.png
[create-app-service-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service-dialog.png
[publish-status]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-status.png
[create-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service.png
[deploy-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-app-service.png
[manage-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/manage-app-service.png
