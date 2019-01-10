---
title: IntelliJ 用のレガシ ツールキットを使用して Azure 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for IntelliJ バージョン 3.0.6 以前を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/13/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 4a1d9ee79fdc4284dff65f6b026ec103b3d623ce
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338976"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-intellij"></a><span data-ttu-id="d800f-103">IntelliJ 用のレガシ ツールキットを使用して Azure 用の Hello World Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="d800f-103">Create a Hello World web app for Azure using the legacy toolkit for IntelliJ</span></span>

<span data-ttu-id="d800f-104">このチュートリアルでは、[Azure Toolkit for IntelliJ] バージョン 3.0.6 以前を使用して、Web アプリとして基本的な Hello World アプリケーションを作成し、Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d800f-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using version 3.0.6 (or earlier) of the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
>
> <span data-ttu-id="d800f-105">この記事の [Azure Toolkit for Eclipse] 使用バージョンについては、「[Eclipse を使用して Azure 用の Hello World Web アプリを作成する][eclipse-hello-world]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d800f-105">For a version of this article that uses the [Azure Toolkit for Eclipse], see [Create a Hello World web app for Azure using Eclipse][eclipse-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="d800f-106">Azure Toolkit for IntelliJ は 2017 年 8 月に更新され、別のワークフローが導入されました。</span><span class="sxs-lookup"><span data-stu-id="d800f-106">The Azure Toolkit for IntelliJ was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="d800f-107">この記事では、Azure Toolkit for IntelliJ のバージョン 3.0.6 以前を使用して Hello World Web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d800f-107">This article illustrates creating a Hello World web app by using version 3.0.6 (or earlier) of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="d800f-108">バージョン 3.0.7 以降のツールキットを使用している場合は、[IntelliJ での Azure 用の Hello World Web アプリの作成][Updated Version]に関するページの手順に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="d800f-108">If you are using the version 3.0.7 (or later) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in IntelliJ][Updated Version].</span></span>
>

<span data-ttu-id="d800f-109">このチュートリアルを完了し、作成したアプリケーションを Web ブラウザーで開くと、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="d800f-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World アプリのプレビュー][01]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="d800f-111">新しい Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="d800f-111">Create a new web app project</span></span>

1. <span data-ttu-id="d800f-112">[Azure Toolkit for IntelliJ の Azure サインイン手順][intelliJ-sign-in-instructions]に関する記事の説明に従って、IntelliJ を起動し、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="d800f-112">Start IntelliJ, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="d800f-113">**[ファイル]** メニューの **[New]\(新規\)** をクリックし、**[プロジェクト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-113">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![[File (ファイル)]、[New (新規)]、[Project (プロジェクト)]][02]

2. <span data-ttu-id="d800f-115">**[New Project]\(新しいプロジェクトの作成\)** ダイアログ ボックスで、**[Java]**、**[Web Application]\(Web アプリケーション\)**、**[Next]\(次へ\)** の順にクリックし、Project SDK を追加します。</span><span class="sxs-lookup"><span data-stu-id="d800f-115">In the **New Project** dialog box, select **Java**, then **Web Application**, and then click **New** to add a Project SDK.</span></span>
   
   ![[新しいプロジェクト] ダイアログ][03a]
   
3. <span data-ttu-id="d800f-117">[Select Home Directory for JDK (JDK のホーム ディレクトリの選択)] ダイアログ ボックスで JDK がインストールされているフォルダーを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-117">In the Select Home Directory for JDK dialog box, select the folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="d800f-118">[New Project (新しいプロジェクト)] ダイアログ ボックスで **[Next (次へ)]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="d800f-118">Click **Next** in the New Project dialog box to continue.</span></span>
   
   ![JDK ホーム ディレクトリを指定する][03b]

4. <span data-ttu-id="d800f-120">このチュートリアルでは、プロジェクトに **Java-Web-App-On-Azure** という名前を付け、**[Finish (完了)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-120">For purposes of this tutorial, name the project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
   ![[新しいプロジェクト] ダイアログ][04]

5. <span data-ttu-id="d800f-122">IntelliJ の [Project Explorer (プロジェクト エクスプローラー)] ビューで、**[Java-Web-App-On-Azure]**、**[Web]** の順に展開し、**index.jsp** をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-122">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
   ![インデックスを開くページ][05c]

6. <span data-ttu-id="d800f-124">index.jsp ファイルが IntelliJ で開いたら、"**Hello World!**" を動的に表示するためのテキストを</span><span class="sxs-lookup"><span data-stu-id="d800f-124">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="d800f-125">既存の `<body>` 要素に追加します。</span><span class="sxs-lookup"><span data-stu-id="d800f-125">within the existing `<body>` element.</span></span> <span data-ttu-id="d800f-126">更新された `<body>` コンテンツは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d800f-126">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. <span data-ttu-id="d800f-127">index.jsp を保存します。</span><span class="sxs-lookup"><span data-stu-id="d800f-127">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="d800f-128">Azure への Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="d800f-128">Deploy your web app to Azure</span></span>

<span data-ttu-id="d800f-129">Java Web アプリを Azure にデプロイする方法はいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="d800f-129">There are several ways by which you can deploy a Java web app to Azure.</span></span> <span data-ttu-id="d800f-130">このチュートリアルでは、アプリケーションを Azure Web アプリ コンテナーにデプロイするという最も簡単な方法について説明します。特殊なプロジェクトの種類や追加のツールは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d800f-130">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="d800f-131">JDK と Web コンテナー ソフトウェアは Azure から提供されるので、自分でアップロードする必要はありません。必要なものは Java Web アプリのみです。</span><span class="sxs-lookup"><span data-stu-id="d800f-131">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="d800f-132">結果として、アプリケーションの発行プロセスにかかる時間は分単位ではなく、秒単位になります。</span><span class="sxs-lookup"><span data-stu-id="d800f-132">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="d800f-133">アプリケーションを発行する前に、まずモジュール設定を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d800f-133">Before you publish your application, you first need to configure your module settings.</span></span> <span data-ttu-id="d800f-134">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="d800f-134">To do so, use the following steps:</span></span>

1. <span data-ttu-id="d800f-135">IntelliJ の Project Explorer で、 **Java-Web-App-On-Azure** プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-135">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="d800f-136">コンテキスト メニューが表示されたら、**[Open Module Settings (モジュール設定を開く)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-136">When the context menu appears, click **Open Module Settings**.</span></span>

   ![モジュール設定を開く][05a]

2. <span data-ttu-id="d800f-138">[プロジェクト構造] ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-138">When the Project Structure dialog box appears:</span></span>

   <span data-ttu-id="d800f-139">a.</span><span class="sxs-lookup"><span data-stu-id="d800f-139">a.</span></span> <span data-ttu-id="d800f-140">**[プロジェクト設定]** の一覧で、**[Artifacts (アーティファクト)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-140">Click **Artifacts** in the list of **Project Settings**.</span></span>

   <span data-ttu-id="d800f-141">b.</span><span class="sxs-lookup"><span data-stu-id="d800f-141">b.</span></span> <span data-ttu-id="d800f-142">**[名前]** ボックスのアーティファクトの名前を、空白または特殊文字が含まれないように変更します。この名前が Uniform Resource Identifier (URI) で使用されるため、変更が必要になります。</span><span class="sxs-lookup"><span data-stu-id="d800f-142">Change the artifact name in the **Name** box so that it doesn't contain whitespace or special characters; this is necessary since the name will be used in the Uniform Resource Identifier (URI).</span></span>

   <span data-ttu-id="d800f-143">c.</span><span class="sxs-lookup"><span data-stu-id="d800f-143">c.</span></span> <span data-ttu-id="d800f-144">**[タイプ]** を **[Web Application: Archive (Web アプリケーション: アーカイブ)]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="d800f-144">Change the **Type** to **Web Application: Archive**.</span></span>

   <span data-ttu-id="d800f-145">d.</span><span class="sxs-lookup"><span data-stu-id="d800f-145">d.</span></span> <span data-ttu-id="d800f-146">**[OK]** をクリックして、[プロジェクト構造] ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="d800f-146">Click **OK** to close the Project Structure dialog box.</span></span>

   ![モジュール設定を開く][05b]

<span data-ttu-id="d800f-148">モジュール設定を構成したら、次の手順を使用してアプリケーションを Azure に発行できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-148">When you have configured your module settings, you can publish your application to Azure by using the following steps:</span></span>

1. <span data-ttu-id="d800f-149">IntelliJ の Project Explorer で、 **Java-Web-App-On-Azure** プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-149">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="d800f-150">コンテキスト メニューが表示されたら、**[Azure]**、**[Publish as Azure Web App (Azure Web アプリとして発行)]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-150">When the context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
   ![Azure に発行するためのコンテキスト メニュー][06]

2. <span data-ttu-id="d800f-152">まだ IntelliJ から Azure にサインインしていない場合、Azure アカウントにサインインするように求められます。</span><span class="sxs-lookup"><span data-stu-id="d800f-152">If you have not already signed in to Azure from IntelliJ, you will be prompted to sign in to your Azure account.</span></span> <span data-ttu-id="d800f-153">(複数の Azure アカウントがある場合、サインイン プロセス中に同じようなプロンプトが何度も表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="d800f-153">(If you have multiple Azure accounts, some of the prompts during the sign-in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="d800f-154">このような状況の場合、次のサインイン手順を続行します。)</span><span class="sxs-lookup"><span data-stu-id="d800f-154">When this happens, continue to follow the sign-in instructions.)</span></span>
   
   ![Azure のログイン ダイアログ][07]

3. <span data-ttu-id="d800f-156">Azure アカウントに正常にサインインすると、**[サブスクリプションの管理]** ダイアログ ボックスに、資格情報に関連付けられたサブスクリプションの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-156">After you have successfully signed in to your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="d800f-157">(複数のサブスクリプションが表示された場合、その一部のみを使うには、使わないサブスクリプションのチェックボックスを必要に応じてオフにします)。サブスクリプションを選択したら、 **[閉じる]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-157">(If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the subscriptions you don't want to use.) When you have selected your subscriptions, click **Close**.</span></span>
   
   ![サブスクリプションの管理][08]

4. <span data-ttu-id="d800f-159">**[Deploy to Azure Web App Container (Azure Web アプリ コンテナーにデプロイ)]** ダイアログ ボックスを開くと、以前に作成した Web アプリ コンテナーがすべて表示されます。コンテナーを作成していない場合、一覧は空欄です。</span><span class="sxs-lookup"><span data-stu-id="d800f-159">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![アプリ コンテナー][09]

5. <span data-ttu-id="d800f-161">以前に Azure Web アプリ コンテナーを作成していない場合、またはアプリケーションを新しいコンテナーに発行する場合は、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="d800f-161">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="d800f-162">作成済みの場合は、既存の Web アプリ コンテナーを選択し、以下の手順 6. に進みます。</span><span class="sxs-lookup"><span data-stu-id="d800f-162">Otherwise, select an existing Web App Container and skip to step 6 below.</span></span>
   
   <span data-ttu-id="d800f-163">a.</span><span class="sxs-lookup"><span data-stu-id="d800f-163">a.</span></span> <span data-ttu-id="d800f-164">**+** 記号をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-164">Click the **+** sign.</span></span>
      
      ![アプリ コンテナーの追加][10]

   <span data-ttu-id="d800f-166">b.</span><span class="sxs-lookup"><span data-stu-id="d800f-166">b.</span></span> <span data-ttu-id="d800f-167">**[New Web App Container (新しい Web アプリ コンテナー)]** ダイアログ ボックスが表示されます。このコンテナーは、後続のいくつかの手順で使用します。</span><span class="sxs-lookup"><span data-stu-id="d800f-167">The **New Web App Container** dialog box will be displayed, which will be used for the next several steps.</span></span>
      
      ![新しいアプリ コンテナー][11a]
   
   <span data-ttu-id="d800f-169">c.</span><span class="sxs-lookup"><span data-stu-id="d800f-169">c.</span></span> <span data-ttu-id="d800f-170">Web アプリ コンテナーの **[DNS Label (DNS ラベル)]** を入力します。これで、Azure の Web アプリケーションについて、ホスト URL のリーフ DNS ラベルが構成されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-170">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="d800f-171">この名前は使用可能であり、Azure Web アプリの名前付け要件に準拠している必要があります。</span><span class="sxs-lookup"><span data-stu-id="d800f-171">Note that the name must be available and conform to Azure Web App naming requirements.</span></span>

   <span data-ttu-id="d800f-172">d.</span><span class="sxs-lookup"><span data-stu-id="d800f-172">d.</span></span> <span data-ttu-id="d800f-173">**[Web Container (Web コンテナー)]** ドロップダウン メニューで、アプリケーションに適したソフトウェアを選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-173">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="d800f-174">現在、Tomcat 8、Tomcat 7 または Jetty 9 から選択することができます。</span><span class="sxs-lookup"><span data-stu-id="d800f-174">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="d800f-175">選択したソフトウェアの最新ディストリビューションが Azure で提供され、Azure で提供される JDK の最新ディストリビューションで実行されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-175">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of the JDK provided by Azure.</span></span>

   <span data-ttu-id="d800f-176">e.</span><span class="sxs-lookup"><span data-stu-id="d800f-176">e.</span></span> <span data-ttu-id="d800f-177">**[サブスクリプション]** ドロップダウン メニューで、このデプロイに使用するサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-177">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="d800f-178">f.</span><span class="sxs-lookup"><span data-stu-id="d800f-178">f.</span></span> <span data-ttu-id="d800f-179">**[Resource Group (リソース グループ)]** ドロップダウン メニューで、Web アプリに関連付けるリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-179">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="d800f-180">(Azure リソース グループを使用すると、関連リソースをグループ化できるため、たとえば、リソースをまとめて削除できます。)</span><span class="sxs-lookup"><span data-stu-id="d800f-180">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="d800f-181">(所有している場合は) 既存のリソース グループを選択して、下記のステップ g にスキップするか、以下のステップに従って、新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="d800f-181">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="d800f-182">**[リソース グループ]** ドロップダウン メニューから、**&lt;&lt;新規リソース グループの作成&gt;&gt;** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-182">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in the **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="d800f-183">**[New Resource Group (新しいリソース グループ)]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-183">The **New Resource Group** dialog box will be displayed:</span></span>
        
         ![New Resource Group][12]

      * <span data-ttu-id="d800f-185">**[Name]\(名前\)** テキスト ボックスに、新しいリソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="d800f-185">In the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="d800f-186">**[Region]\(リージョン\)** ドロップダウン メニューで、リソース グループに適した Azure データ センターの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-186">In the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="d800f-187">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="d800f-187">Click **OK**.</span></span>

   <span data-ttu-id="d800f-188">g.</span><span class="sxs-lookup"><span data-stu-id="d800f-188">g.</span></span> <span data-ttu-id="d800f-189">**[App Service Plan (App Service プラン)]** ドロップダウン メニューには、選択したリソース グループに関連付けられた App Service プランが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-189">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="d800f-190">(App Service プランでは、Web アプリの場所、価格レベル、およびコンピューティング インスタンス サイズなどの情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="d800f-190">(An App Service Plan specifies information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="d800f-191">単一の App Service プランを複数の Web Apps に使用できます。そのため、App Service プランは、特定の Web アプリのデプロイとは別に保持されます。)</span><span class="sxs-lookup"><span data-stu-id="d800f-191">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
      <span data-ttu-id="d800f-192">(所有している場合は) 既存の App Service プランを選択して、下記のステップ h にスキップするか、以下のステップに従って、新しい App Service プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="d800f-192">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="d800f-193">**[App Service プラン]** ドロップダウン メニューから、**&lt;&lt;新しい App Service プランの作成&gt;&gt;** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-193">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in the **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="d800f-194">**[New App Service Plan (新しい App Service プラン)]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-194">The **New App Service Plan** dialog box will be displayed:</span></span>
        
         ![新しい App Service プラン][13]

      * <span data-ttu-id="d800f-196">**[Name]\(名前\)** ボックスに、新しい App Service プランの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="d800f-196">In the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="d800f-197">**[Location]\(場所\)** ドロップダウン メニューで、プランに適した Azure データ センターの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-197">In the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="d800f-198">**[Pricing Tier]\(価格レベル\)** ドロップダウン メニューで、プランに適した価格を選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-198">In the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="d800f-199">テスト目的の場合は、 **[Free]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-199">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="d800f-200">**[Instance Size]\(インスタンス サイズ\)** ドロップダウン メニューで、プランに適したインスタンス サイズを選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-200">In the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="d800f-201">テスト目的の場合は、 **[Small]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-201">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="d800f-202">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="d800f-202">Click **OK**.</span></span>

   <span data-ttu-id="d800f-203">h.</span><span class="sxs-lookup"><span data-stu-id="d800f-203">h.</span></span> <span data-ttu-id="d800f-204">(オプション) 既定では、Java 8 の最新のディストリビューションは、Azure によって Web アプリ コンテナーに JVM として自動的にデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="d800f-204">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure to your web app container.</span></span> <span data-ttu-id="d800f-205">ただし、JVM の別のバージョンおよびディストリビューションを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="d800f-205">However, you can select a different version and distribution of the JVM.</span></span> <span data-ttu-id="d800f-206">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="d800f-206">To do so, use the following steps:</span></span>
      
   * <span data-ttu-id="d800f-207">**[New Web App Container (新しい Web アプリ コンテナー)]** ダイアログ ボックスの **[JDK]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-207">Click the **JDK** tab in the **New Web App Container** dialog box.</span></span>
   * <span data-ttu-id="d800f-208">次のオプションのいずれかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-208">You can choose from one of the following options:</span></span>
        
      * <span data-ttu-id="d800f-209">Azure によって提供される既定の JDK をデプロイする</span><span class="sxs-lookup"><span data-stu-id="d800f-209">Deploy the default JDK which is offered by Azure</span></span>
      * <span data-ttu-id="d800f-210">Azure で利用可能な追加の JDK のドロップダウン リストからサード パーティの JDK をデプロイする</span><span class="sxs-lookup"><span data-stu-id="d800f-210">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
      * <span data-ttu-id="d800f-211">カスタム JDK をデプロイする。カスタム JDK は、ZIP ファイルとしてパッケージ化され、一般に入手可能であるか、Azure ストレージ アカウントに格納されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="d800f-211">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
     ![[New Web App Container (新しい Web アプリ コンテナー)] の [JDK] タブ][11b]

   <span data-ttu-id="d800f-213">i.</span><span class="sxs-lookup"><span data-stu-id="d800f-213">i.</span></span> <span data-ttu-id="d800f-214">これらの手順をすべて完了すると、[New Web App Container] \(新しい Web アプリ コンテナー) ダイアログ ボックスは次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="d800f-214">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![新しいアプリ コンテナー][14]
   
   <span data-ttu-id="d800f-216">j.</span><span class="sxs-lookup"><span data-stu-id="d800f-216">j.</span></span> <span data-ttu-id="d800f-217">**[OK]** をクリックすると、新しい Web アプリ コンテナーの作成が完了します。</span><span class="sxs-lookup"><span data-stu-id="d800f-217">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="d800f-218">数秒待つと Web アプリ コンテナーの一覧が更新されます。一覧で新しく作成した Web アプリ コンテナーが選択されています。</span><span class="sxs-lookup"><span data-stu-id="d800f-218">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

6. <span data-ttu-id="d800f-219">これで、Azure への Web アプリの初期デプロイを完了する準備ができました。**[OK]** をクリックして、Java アプリケーションを選択した Web アプリ コンテナーにデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d800f-219">You are now ready to complete the initial deployment of your Web App to Azure; click **OK** to deploy your Java application to the selected Web App container.</span></span> <span data-ttu-id="d800f-220">既定では、アプリケーションはアプリケーション サーバーのサブディレクトリとしてデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="d800f-220">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="d800f-221">ルート アプリケーションとしてデプロイする場合、**[Deploy to root (ルートにデプロイ)]** チェック ボックスをオンにして **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-221">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
   
   ![Azure へのデプロイ][15]

7. <span data-ttu-id="d800f-223">**[Azure Activity Log (Azure アクティビティ ログ)]** ビューが開き、Web アプリのデプロイの状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-223">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![進捗状況インジケーター][16]
   
   <span data-ttu-id="d800f-225">Web アプリを Azure にデプロイするプロセスは、わずか数秒で完了します。</span><span class="sxs-lookup"><span data-stu-id="d800f-225">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="d800f-226">アプリケーションの準備ができると、 **[Status]\(状態\)** 列に **[Published]\(発行済み\)** というリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-226">When your application is ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="d800f-227">リンクをクリックすると、デプロイされた Web アプリのホーム ページに移動します。また、次のセクションの手順を使用して Web アプリを参照することもできます。</span><span class="sxs-lookup"><span data-stu-id="d800f-227">When you click the link, it will take you to your deployed Web App's home page, or you can use the steps in the following section to browse to your web app.</span></span>

## <a name="browsing-to-your-web-app-on-azure"></a><span data-ttu-id="d800f-228">Azure の Web アプリの参照</span><span class="sxs-lookup"><span data-stu-id="d800f-228">Browsing to your Web App on Azure</span></span>

<span data-ttu-id="d800f-229">**Azure Explorer** ビューを使用すると、Azure の Web アプリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-229">To browse to your Web App on Azure, you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="d800f-230">**[Azure Explorer (Azure エクスプローラー)]** ビューがまだ開いていない場合、IntelliJ の **[View (ビュー)]** メニュー、**[Tool Windows (ツール ウィンドウ)]**、**[Service Explorer (サービス エクスプローラー)]** の順にクリックして開きます。</span><span class="sxs-lookup"><span data-stu-id="d800f-230">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="d800f-231">まだログインしていない場合は、ログインするように求められます。</span><span class="sxs-lookup"><span data-stu-id="d800f-231">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="d800f-232">**[Azure Explorer]** ビューが表示されたら、次の手順に従って Web アプリを参照します。</span><span class="sxs-lookup"><span data-stu-id="d800f-232">When the **Azure Explorer** view is displayed, follow these steps to browse to your Web App:</span></span> 

1. <span data-ttu-id="d800f-233">**[Azure]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="d800f-233">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="d800f-234">**[Web Apps (Web アプリ)]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="d800f-234">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="d800f-235">目的の Web App を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-235">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="d800f-236">コンテキスト メニューが表示されたら、 **[Open in Browser]**(ブラウザーで開く) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-236">When the context menu appears, click **Open in Browser**.</span></span>
   
   ![Web アプリの参照][17]

## <a name="updating-your-web-app"></a><span data-ttu-id="d800f-238">Web アプリの更新</span><span class="sxs-lookup"><span data-stu-id="d800f-238">Updating your web app</span></span>

<span data-ttu-id="d800f-239">既存の実行中の Azure Web アプリを更新するプロセスは短時間で簡単です。更新には 2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="d800f-239">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="d800f-240">既存の Java Web アプリのデプロイを更新できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-240">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="d800f-241">同じ Web アプリ コンテナーに追加の Java アプリケーションを発行できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-241">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="d800f-242">いずれの場合でもプロセスは同じで、かかる時間は数秒です。</span><span class="sxs-lookup"><span data-stu-id="d800f-242">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="d800f-243">IntelliJ の Project Explorer で、更新する Java アプリケーションを右クリックするか、既存の Web アプリ コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="d800f-243">In the IntelliJ project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="d800f-244">コンテキスト メニューが表示されたら、**[Azure]**、**[Publish as Azure Web App (Azure Web アプリとして発行)]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d800f-244">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="d800f-245">既にログインしているので、既存の Web アプリ コンテナーの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d800f-245">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="d800f-246">Java アプリケーションを発行または再発行するコンテナーを選択し、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-246">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="d800f-247">数秒後、**[Azure Activity Log (Azure アクティビティ ログ)]** ビューに更新されたデプロイが **[Published (発行済み)]** と表示され、Web ブラウザーで更新されたアプリケーションを確認できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-247">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="d800f-248">既存の Web アプリの起動、停止、再起動</span><span class="sxs-lookup"><span data-stu-id="d800f-248">Starting, stopping, or restarting an existing web app</span></span>

<span data-ttu-id="d800f-249">既存の Azure Web アプリ コンテナー (コンテナー内にデプロイされているすべての Java アプリケーションを含む) を起動または停止するには、 **[Azure Explorer]** (Azure Explorer) ビューを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d800f-249">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="d800f-250">**[Azure Explorer (Azure エクスプローラー)]** ビューがまだ開いていない場合、IntelliJ の **[View (ビュー)]** メニュー、**[Tool Windows (ツール ウィンドウ)]**、**[Service Explorer (サービス エクスプローラー)]** の順にクリックして開きます。</span><span class="sxs-lookup"><span data-stu-id="d800f-250">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="d800f-251">まだログインしていない場合は、ログインするように求められます。</span><span class="sxs-lookup"><span data-stu-id="d800f-251">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="d800f-252">**[Azure Explorer]** ビューが表示されたら、次の手順に従って Web アプリを起動また停止します。</span><span class="sxs-lookup"><span data-stu-id="d800f-252">When the **Azure Explorer** view is displayed, follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="d800f-253">**[Azure]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="d800f-253">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="d800f-254">**[Web Apps (Web アプリ)]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="d800f-254">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="d800f-255">目的の Web App を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-255">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="d800f-256">コンテキスト メニューが表示されたら、**[Start (起動)]** **[Stop (停止)]**、または **[Restart (再起動)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d800f-256">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="d800f-257">メニュー項目はコンテキストに依存します。つまり、Web アプリが実行しているときは停止操作のみ、Web アプリが現在実行されていないときは起動操作のみを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="d800f-257">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![Web アプリの停止][18]

## <a name="next-steps"></a><span data-ttu-id="d800f-259">次の手順</span><span class="sxs-lookup"><span data-stu-id="d800f-259">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="d800f-260">Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d800f-260">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Updated Version]: azure-toolkit-for-intellij-create-hello-world-web-app.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/01-Web-Page.png
[02]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/02-File-New-Project.png
[03a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-Dialog.png
[03b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-SDK-Dialog.png
[04]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/04-New-Project-Dialog.png
[05a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Module-Settings.png
[05b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Project-Structure-Dialog.png
[05c]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Index-Page.png
[06]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/06-Azure-Publish-Context-Menu.png
[07]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/07-Azure-Log-In-Dialog.png
[08]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/08-Manage-Subscriptions.png
[09]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/09-App-Containers.png
[10]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/10-Add-App-Container.png
[11a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container.png
[11b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container-JDK-Tab.png
[12]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/12-New-Resource-Group.png
[13]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/13-New-App-Service-Plan.png
[14]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/14-New-App-Container.png
[15]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/15-Deploy-To-Azure.png
[16]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/16-Progress-Indicator.png
[17]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/17-Browse-Web-App.png
[18]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/18-Stop-Web-App.png
