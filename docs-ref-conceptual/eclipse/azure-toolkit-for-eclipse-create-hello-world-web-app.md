---
title: Eclipse を使用して Azure 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for Eclipse を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
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
ms.openlocfilehash: c98f966eb17e3fbde877451c8f8fefb21e6bf686
ms.sourcegitcommit: dca98b953fa3149fb2e6aa49e27e843b6df0c6c2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2019
ms.locfileid: "57786891"
---
# <a name="create-a-hello-world-web-app-for-azure-using-eclipse"></a><span data-ttu-id="86d43-103">Eclipse を使用して Azure 用の Hello World Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="86d43-103">Create a Hello World web app for Azure using Eclipse</span></span>

<span data-ttu-id="86d43-104">このチュートリアルでは、[Azure Toolkit for Eclipse] を使用して、Web アプリとして基本的な Hello World アプリケーションを作成し、Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="86d43-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for Eclipse].</span></span>

> [!NOTE]
>
> <span data-ttu-id="86d43-105">この記事の [Azure Toolkit for IntelliJ] 使用バージョンについては、「[IntelliJ を使用して Azure 用の Hello World Web アプリを作成する][intellij-hello-world]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="86d43-105">For a version of this article that uses the [Azure Toolkit for IntelliJ], see [Create a Hello World web app for Azure using IntelliJ][intellij-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="86d43-106">Azure Toolkit for Eclipse は 2017 年 8 月に更新され、別のワークフローが導入されました。</span><span class="sxs-lookup"><span data-stu-id="86d43-106">The Azure Toolkit for Eclipse was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="86d43-107">この記事では、Azure Toolkit for Eclipse のバージョン 3.0.7 以降を使用して Hello World Web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="86d43-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="86d43-108">バージョン 3.0.6 以前のツールキットを使用している場合は、[レガシ ツールキットを使用した Eclipse での Azure 用 Hello World Web アプリの作成][Legacy Version]に関するページの手順に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="86d43-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in Eclipse using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="86d43-109">このチュートリアルを完了し、作成したアプリケーションを Web ブラウザーで開くと、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="86d43-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World アプリのプレビュー][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="86d43-111">新しい Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="86d43-111">Create a new web app project</span></span>

1. <span data-ttu-id="86d43-112">[Azure Toolkit for Eclipse の Azure サインイン手順][eclipse-sign-in-instructions] の説明に従って、Eclipse を起動し、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="86d43-112">Start Eclipse, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for Eclipse][eclipse-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="86d43-113">**[ファイル]**、**[新規]**、**[Dynamic Web Project]\(動的 Web プロジェクト\)** の順にクリックします </span><span class="sxs-lookup"><span data-stu-id="86d43-113">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="86d43-114">(**[ファイル]** と **[新規]** のクリック後、使用可能なプロジェクトとして **[Dynamic Web Project (動的 Web プロジェクト)]** が表示されない場合は、**[ファイル]**、**[新規]**、**[プロジェクト]** の順にクリックし、**[Web]** を展開して、**[Dynamic Web Project (動的 Web プロジェクト)]**、**[次へ]** の順にクリックします)。</span><span class="sxs-lookup"><span data-stu-id="86d43-114">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![新しい動的 Web プロジェクトの作成][file-new-dynamic-web-project]

2. <span data-ttu-id="86d43-116">このチュートリアルでは、プロジェクトに **MyWebApp**という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="86d43-116">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="86d43-117">画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="86d43-117">Your screen will appear similar to the following:</span></span>
   
   ![新しい動的 Web プロジェクトのプロパティ][dynamic-web-project-properties]

3. <span data-ttu-id="86d43-119">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86d43-119">Click **Finish**.</span></span>

4. <span data-ttu-id="86d43-120">Eclipse の Project Explorer ビューで、 **[MyWebApp]** を展開します。</span><span class="sxs-lookup"><span data-stu-id="86d43-120">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="86d43-121">**WebContent** を右クリックし、**[新規]**、**[JSP ファイル]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="86d43-121">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![新しい JSP ファイルの作成][create-new-jsp-file]

5. <span data-ttu-id="86d43-123">**[New JSP File (新しい JSP ファイル)]** ダイアログ ボックスで **index.jsp** ファイルに名前を付け、親フォルダーは **MyWebApp/WebContent** のままにして **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86d43-123">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![[New JSP File\(新しい JSP ファイル\)] ダイアログ ボックス][new-jsp-file-dialog]

6. <span data-ttu-id="86d43-125">**[Select JSP Template (JSP テンプレートの選択)]** ダイアログ ボックスで、このチュートリアルのために **[New JSP File (html) (新しい JSP ファイル (html))]** を選択し、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86d43-125">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![JSP テンプレートの選択][select-jsp-template]

7. <span data-ttu-id="86d43-127">index.jsp ファイルが Eclipse で開いたら、"**Hello World!**" を動的に表示するためのテキストを</span><span class="sxs-lookup"><span data-stu-id="86d43-127">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="86d43-128">既存の `<body>` 要素に追加します。</span><span class="sxs-lookup"><span data-stu-id="86d43-128">within the existing `<body>` element.</span></span> <span data-ttu-id="86d43-129">更新された `<body>` コンテンツは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="86d43-129">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="86d43-130">index.jsp を保存します。</span><span class="sxs-lookup"><span data-stu-id="86d43-130">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="86d43-131">Azure への Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="86d43-131">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="86d43-132">Eclipse のプロジェクト エクスプローラー ビューでプロジェクトを右クリックし、**[Azure]**、**[Publish as Azure Web App]\(Azure Web アプリとして発行\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="86d43-132">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![[Publish as Azure Web App (Azure Web アプリとして発行)]][publish-as-azure-web-app]

1. <span data-ttu-id="86d43-134">**[Deploy Web App]\(Web アプリのデプロイ\)** ダイアログ ボックスが表示されたら、次のいずれかのオプションを選択できます。</span><span class="sxs-lookup"><span data-stu-id="86d43-134">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="86d43-135">既存の Web アプリを選択します (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="86d43-135">Select an existing web app if one exists.</span></span>

      ![アプリ サービスの選択][select-app-service]

   * <span data-ttu-id="86d43-137">**[Create New Web App]\(新しい Web アプリを作成する\)** をクリックする。</span><span class="sxs-lookup"><span data-stu-id="86d43-137">Click **Create New Web App**.</span></span>

      ![App Service を作成する][create-app-service]

      <span data-ttu-id="86d43-139">**[App Service の作成]** ダイアログ ボックスで Web アプリに必要な情報を指定し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86d43-139">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      <span data-ttu-id="86d43-140">ここで、ランタイム環境、アプリ設定、サービス プラン、およびリソース グループを構成できます。</span><span class="sxs-lookup"><span data-stu-id="86d43-140">Here you can configure the runtime environment, app settings, service plan and resource group.</span></span>

      ![[App Service の作成] ダイアログ ボックス][create-app-service-dialog]

1. <span data-ttu-id="86d43-142">Web アプリを選択し、**[デプロイ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86d43-142">Select your web app and then click **Deploy**.</span></span>

   ![アプリ サービスのデプロイ][deploy-app-service]

1. <span data-ttu-id="86d43-144">ツールキットにより Web アプリが正常にデプロイされると、**[Azure の活動ログ]** タブに **[発行済み]** 状態として表示され、デプロイされた Web アプリの URL へのハイパーリンクが設定されます。</span><span class="sxs-lookup"><span data-stu-id="86d43-144">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![発行済み状態][publish-status]

1. <span data-ttu-id="86d43-146">ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="86d43-146">You can browse to your web app using the link provided in the status message.</span></span>

   ![Web アプリの参照][browse-web-app]

1. <span data-ttu-id="86d43-148">Web を Azure に発行した後、アプリを管理するには、それを右クリックし、コンテキスト メニューからオプションのいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="86d43-148">After you have published your web to Azure, you can manage your app by right-clicking on it and selecting one of the options on the context menu.</span></span> <span data-ttu-id="86d43-149">たとえば、Web アプリを**開始**、**停止**、または**削除**できます。</span><span class="sxs-lookup"><span data-stu-id="86d43-149">For example, you can **Start**, **Stop**, or **Delete** your web app.</span></span>

   ![アプリ サービスの管理][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="86d43-151">次の手順</span><span class="sxs-lookup"><span data-stu-id="86d43-151">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="86d43-152">Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="86d43-152">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

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
