---
title: IntelliJ を使用して Azure 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for IntelliJ を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
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
ms.openlocfilehash: cc68e16a6940a1f0f2b08f0b63c90c58ec6dbc4e
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892863"
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a><span data-ttu-id="3b139-103">IntelliJ を使用して Azure 用の Hello World Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="3b139-103">Create a Hello World web app for Azure using IntelliJ</span></span>

<span data-ttu-id="3b139-104">このチュートリアルでは、[Azure Toolkit for IntelliJ] を使用して、Web アプリとして基本的な Hello World アプリケーションを作成し、Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3b139-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
>
> <span data-ttu-id="3b139-105">この記事の [Azure Toolkit for Eclipse] 使用バージョンについては、「[Eclipse を使用して Azure 用の Hello World Web アプリを作成する][eclipse-hello-world]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b139-105">For a version of this article that uses the [Azure Toolkit for Eclipse], see [Create a Hello World web app for Azure using Eclipse][eclipse-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="3b139-106">Azure Toolkit for IntelliJ は 2017 年 8 月に更新され、別のワークフローが導入されました。</span><span class="sxs-lookup"><span data-stu-id="3b139-106">The Azure Toolkit for IntelliJ was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="3b139-107">この記事では、Azure Toolkit for IntelliJ バージョン 3.0.7 以降を使用して Hello World Web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3b139-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="3b139-108">バージョン 3.0.6 以前のツールキットを使用している場合は、[レガシ ツールキットを使用した IntelliJ での Azure 用 Hello World Web アプリの作成][Legacy Version]に関するページの手順に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b139-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in IntelliJ using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="3b139-109">このチュートリアルを完了し、作成したアプリケーションを Web ブラウザーで開くと、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="3b139-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World アプリのプレビュー][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="3b139-111">新しい Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="3b139-111">Create a new web app project</span></span>

1. <span data-ttu-id="3b139-112">[Azure Toolkit for IntelliJ の Azure サインイン手順][intelliJ-sign-in-instructions]に関する記事の説明に従って、IntelliJ を起動し、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="3b139-112">Start IntelliJ, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="3b139-113">**[ファイル]** メニューの **[New]\(新規\)** をクリックし、**[プロジェクト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-113">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![新しいプロジェクトの作成][file-new-project]

1. <span data-ttu-id="3b139-115">**[新しいプロジェクト]** ダイアログ ボックスで、 **[Maven]**、 **maven-archetype-webapp** の順に選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-115">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![Maven アーキタイプ Web アプリの選択][maven-archetype-webapp]
   
1. <span data-ttu-id="3b139-117">Web アプリの **[GroupId]** と **[ArtifactId]** を指定し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-117">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![GroupId と ArtifactId の指定][groupid-and-artifactid]

1. <span data-ttu-id="3b139-119">Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-119">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![Maven 設定の指定][maven-options]

1. <span data-ttu-id="3b139-121">プロジェクト名と場所を指定し、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-121">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![プロジェクト名の指定][project-name]

1. <span data-ttu-id="3b139-123">IntelliJ のプロジェクト エクスプローラー ビューで、**[src]**、**[main]**、**[webapp]** の順に展開し、**index.jsp** をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-123">Within IntelliJ's Project Explorer view, expand **src**, then **main**, then **webapp**, and then double-click **index.jsp**.</span></span>
   
   ![インデックス ページを開く][open-index-page]

1. <span data-ttu-id="3b139-125">index.jsp ファイルが IntelliJ で開いたら、"**Hello World!**" を動的に表示するためのテキストを</span><span class="sxs-lookup"><span data-stu-id="3b139-125">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="3b139-126">既存の `<body>` 要素に追加します。</span><span class="sxs-lookup"><span data-stu-id="3b139-126">within the existing `<body>` element.</span></span> <span data-ttu-id="3b139-127">更新された `<body>` コンテンツは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3b139-127">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![インデックス ページの編集][edit-index-page]

1. <span data-ttu-id="3b139-129">index.jsp を保存します。</span><span class="sxs-lookup"><span data-stu-id="3b139-129">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="3b139-130">Azure への Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="3b139-130">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="3b139-131">IntelliJ のプロジェクト エクスプローラー ビューでプロジェクトを右クリックし、**[Azure]**、**[Run on Web App]\(Web アプリで実行\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="3b139-131">Within IntelliJ's Project Explorer view, right-click your project, choose **Azure**, and then choose **Run on Web App**.</span></span>
   
   ![[Run on Web App]\(Web アプリで実行\) メニュー][run-on-web-app-menu]

1. <span data-ttu-id="3b139-133">[Run on Web App]\(Web アプリで実行\) ダイアログ ボックスで、次のいずれかのオプションを選択できます。</span><span class="sxs-lookup"><span data-stu-id="3b139-133">In the Run on Web App dialog box, you can choose one of the following options:</span></span>

   * <span data-ttu-id="3b139-134">既存の Web アプリ (存在する場合) を選択して **[Run]\(実行\)** をクリックする。</span><span class="sxs-lookup"><span data-stu-id="3b139-134">Choose an existing web app (if one exists), and then click **Run**.</span></span>

      ![[Run on Web App]\(Web アプリで実行\) ダイアログ ボックス][run-on-web-app-dialog]

   * <span data-ttu-id="3b139-136">**[Create New Web App]\(新しい Web アプリを作成する\)** をクリックする。</span><span class="sxs-lookup"><span data-stu-id="3b139-136">Click **Create New Web App**.</span></span> <span data-ttu-id="3b139-137">新しい Web アプリを作成する場合は、Web アプリに必要な情報を指定し、**[Run]\(実行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-137">If you choose to create a new web app, specify the requisite information for your web app, and then click **Run**.</span></span>

      ![新しい Web アプリの作成][create-new-web-app-dialog]

1. <span data-ttu-id="3b139-139">Web アプリが正常にデプロイされると、その Web アプリの URL を含むステータス メッセージがツールキットに表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b139-139">The toolkit will display a status message when it has successfully deployed your web app, which will also display the URL of your deployed web app.</span></span>

   ![デプロイに成功][successfully-deployed]

1. <span data-ttu-id="3b139-141">ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="3b139-141">You can browse to your web app using the link provided in the status message.</span></span>

   ![Web アプリの参照][browse-web-app]

1. <span data-ttu-id="3b139-143">Web アプリを発行した後、使用した設定が既定の設定として保存されます。ツール バーの緑色矢印のアイコンをクリックすることで、Azure でアプリケーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="3b139-143">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="3b139-144">設定を変更するには、Web アプリのドロップダウン メニューをクリックし、**[Edit Configurations]\(構成の編集\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-144">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[Edit Configurations]\(構成の編集\) メニュー][edit-configuration-menu]

1. <span data-ttu-id="3b139-146">**[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b139-146">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[構成の編集] ダイアログ ボックス][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="3b139-148">次の手順</span><span class="sxs-lookup"><span data-stu-id="3b139-148">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="3b139-149">Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b139-149">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

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

[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[run-on-web-app-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[run-on-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
