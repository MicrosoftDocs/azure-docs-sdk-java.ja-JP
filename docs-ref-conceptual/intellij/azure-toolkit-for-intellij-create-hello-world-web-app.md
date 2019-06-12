---
title: IntelliJ を使用して Azure App Service 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for IntelliJ を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
keywords: java, intellij, web アプリ, azure app service, hello world, クイック スタート
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ae0749ce1ddab971f1a83e2e5e58492fd8ccb287
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626119"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a><span data-ttu-id="0a2f2-104">IntelliJ を使用して Azure App Service 用の Hello World Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="0a2f2-104">Create a Hello World web app for Azure App Service using IntelliJ</span></span>

<span data-ttu-id="0a2f2-105">オープンソースの [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) プラグインを使用すれば、数分で、基本的な Hello World アプリケーション作成して Web アプリとして Azure App Service にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-105">Using open sourced [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0a2f2-106">Eclipse を使用したい場合は、[Eclipse 用の同様のチュートリアル][eclipse-hello-world]をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-106">If you prefer using Eclipse, check out our [similar tutorial for Eclipse][eclipse-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="0a2f2-107">このチュートリアルを完了したら、忘れずにリソースをクリーンアップしてください。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="0a2f2-108">その場合、このガイドの実行によって無料アカウントのクォータが超過することはありません。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="0a2f2-109">インストールとサインイン</span><span class="sxs-lookup"><span data-stu-id="0a2f2-109">Installation and Sign-in</span></span>

1. <span data-ttu-id="0a2f2-110">IntelliJ IDEA の [設定/環境設定] ダイアログ (Ctrl+Alt+S) で、 **[プラグイン]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-110">In IntelliJ IDEA's Settings/Preferences dialog (Ctrl+Alt+S), select **Plugins**.</span></span> <span data-ttu-id="0a2f2-111">次に、 **[Marketplace]** で **[Azure Toolkit for IntelliJ]** を見つけ、 **[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-111">Then, find the **Azure Toolkit for IntelliJ** in the **Marketplace** and click **Install**.</span></span> <span data-ttu-id="0a2f2-112">インストール後、 **[再起動]** をクリックしてプラグインをアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-112">After installed, click **Restart** to activate the plugin.</span></span> 

   ![Marketplace の Azure Toolkit for IntelliJ プラグイン][marketplace]

2. <span data-ttu-id="0a2f2-114">Azure アカウントにサインインするには、サイドバーの **Azure Explorer** を開き、上部のバーにある **[Azure サインイン]** アイコンをクリックします (または [IDEA] メニューから **[ツール]/[Azure]/ [Azure サインイン]** をクリックします)。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-114">To sign in to your Azure account, open sidebar **Azure Explorer**, and then click the **Azure Sign In** icon in the bar on top (or from IDEA menu **Tools/Azure/Azure Sign in**).</span></span>

   ![IntelliJ Azure サインイン コマンド][I01]

3. <span data-ttu-id="0a2f2-116">**[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、次に **[サインイン]** をクリックします ([他のサインイン オプション](azure-toolkit-for-intellij-sign-in-instructions.md))。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-116">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign in options](azure-toolkit-for-intellij-sign-in-instructions.md)).</span></span>

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

4. <span data-ttu-id="0a2f2-118">**[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-118">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

5. <span data-ttu-id="0a2f2-120">ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-120">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![デバイスのログイン ブラウザー][I04]

6. <span data-ttu-id="0a2f2-122">**[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-122">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="0a2f2-124">Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="0a2f2-124">Creating web app project</span></span>

1. <span data-ttu-id="0a2f2-125">IntelliJ で、 **[ファイル]** メニュー、 **[新規作成]** 、 **[プロジェクト]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-125">In IntelliJ, click the **File** menu, then click **New**, and then click **Project**.</span></span>

   ![新しいプロジェクトの作成][file-new-project]

2. <span data-ttu-id="0a2f2-127">**[新しいプロジェクト]** ダイアログ ボックスで、 **[Maven]** 、 **maven-archetype-webapp** の順に選択し、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-127">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>

   ![Maven アーキタイプ Web アプリの選択][maven-archetype-webapp]

3. <span data-ttu-id="0a2f2-129">Web アプリの **[GroupId]** と **[ArtifactId]** を指定し、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-129">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>

   ![GroupId と ArtifactId の指定][groupid-and-artifactid]

4. <span data-ttu-id="0a2f2-131">Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-131">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>

   ![Maven 設定の指定][maven-options]

5. <span data-ttu-id="0a2f2-133">プロジェクト名と場所を指定し、 **[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-133">Specify your project name and location, and then click **Finish**.</span></span>

   ![プロジェクト名の指定][project-name]

6. <span data-ttu-id="0a2f2-135">Project Explorer ビューで、**src/main/webapp/index.jsp** ファイルを開き、次のように編集して**変更を保存します**。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-135">Under Project Explorer view, open and edit the file **src/main/webapp/index.jsp** as following and **save the changes**:</span></span>

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![インデックス ページの編集][edit-index-page]

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="0a2f2-137">Azure への Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="0a2f2-137">Deploying web app to Azure</span></span>

1. <span data-ttu-id="0a2f2-138">Project Explorer ビューでプロジェクトを右クリックし、 **[Azure]** を展開して、次に **[Deploy to Azure]\(Azure へのデプロイ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-138">Under Project Explorer view, right-click your project, expand **Azure**, then click **Deploy to Azure**.</span></span>

   ![[Deploy to Azure]\(Azure へのデプロイ\) メニュー][deploy-to-azure-menu]

1. <span data-ttu-id="0a2f2-140">[Deploy to Azure]\(Azure へのデプロイ\) ダイアログ ボックスで、既存の Tomcat Web アプリがある場合はそれにアプリケーションを直接デプロイできますが、それ以外の場合は、最初に新しいものを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-140">In the Deploy to Azure dialog box, you can directly deploy the application to an existing Tomcat webapp if you already have one, otherwise you should create a new one first.</span></span>
   1. <span data-ttu-id="0a2f2-141">**[No Available webapp, click to create a new one]\(使用可能な Web アプリがありません。クリックして新しいものを作成してください\)** リンクをクリックし、新しい Web アプリを作成します。お使いのサブスクリプションに既存の Web アプリがある場合は、WebApp ドロップダウンから **[新しい Web アプリを作成する]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-141">Click the link **No Available webapp, click to create a new one** to crete a new web app, you could choose **Create New WebApp** from WebApp dropdown if there are existing webapps in your subscription.</span></span>

      ![[Deploy to Azure]\(Azure へのデプロイ\) ダイアログ ボックス][deploy-to-azure-dialog]

   1. <span data-ttu-id="0a2f2-143">ポップアップ ダイアログ ボックスで、[Web コンテナー] として **TOMCAT 8.5-jre8** を選択し、その他の必要な情報を指定し、 **[OK]** をクリックして Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-143">In the pop-up dialog box, chose **TOMCAT 8.5-jre8** as Web Container and specify other required information, then click **OK** to create the webapp.</span></span>

      ![新しい Web アプリの作成][create-new-web-app-dialog]

   1. <span data-ttu-id="0a2f2-145">WebApp ドロップダウンから Web アプリを選択し、 **[実行]** をクリックします。(既存の Web アプリにデプロイする場合は、ここから開始できます)</span><span class="sxs-lookup"><span data-stu-id="0a2f2-145">Choose the web app from WebApp drop down, and then click **Run**.(You could start from here if you want deploy to an existing webapp)</span></span>

      ![既存の Web アプリへのデプロイ][deploy-to-existing-webapp]

1. <span data-ttu-id="0a2f2-147">ツールキットにより Web アプリが正常にデプロイされた場合、状態メッセージと、デプロイされた Web アプリの URL (成功した場合) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-147">The toolkit will display a status message when it has successfully deployed your web app, along with the URL of your deployed web app if succeed.</span></span>

   ![デプロイに成功][successfully-deployed]

1. <span data-ttu-id="0a2f2-149">ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-149">You can browse to your web app using the link provided in the status message.</span></span>

   ![Web アプリの参照][browse-web-app]

## <a name="managing-deploy-configurations"></a><span data-ttu-id="0a2f2-151">デプロイ構成の管理</span><span class="sxs-lookup"><span data-stu-id="0a2f2-151">Managing deploy configurations</span></span>

1. <span data-ttu-id="0a2f2-152">Web アプリを発行すると、使用した設定が既定値として保存され、ツール バーの緑色の矢印アイコンをクリックすることでデプロイを実行できます。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-152">After you have published your web app, your settings will be saved as the default, and you can run the deployment by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="0a2f2-153">設定を変更するには、Web アプリのドロップダウン メニューをクリックし、 **[Edit Configurations]\(構成の編集\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-153">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[Edit Configurations]\(構成の編集\) メニュー][edit-configuration-menu]

1. <span data-ttu-id="0a2f2-155">**[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-155">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[構成の編集] ダイアログ ボックス][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a><span data-ttu-id="0a2f2-157">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="0a2f2-157">Cleaning up resources</span></span>

1. <span data-ttu-id="0a2f2-158">Azure Explorer での Web アプリ の削除</span><span class="sxs-lookup"><span data-stu-id="0a2f2-158">Deleting Web Apps in Azure Explorer</span></span>

     ![リソースのクリーンアップ][clean-resources]

## <a name="next-steps"></a><span data-ttu-id="0a2f2-160">次の手順</span><span class="sxs-lookup"><span data-stu-id="0a2f2-160">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="0a2f2-161">Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0a2f2-161">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->
[marketplace]:./media/azure-toolkit-for-intellij-create-hello-world-web-app/marketplace.png
[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/clean-resource.png
[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png
