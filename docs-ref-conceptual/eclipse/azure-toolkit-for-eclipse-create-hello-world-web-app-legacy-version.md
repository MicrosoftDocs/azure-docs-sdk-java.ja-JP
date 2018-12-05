---
title: ''
description: このチュートリアルでは、Azure Toolkit for Eclipse バージョン 3.0.6 以前を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
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
ms.openlocfilehash: b05dcd52f36524ab17652f83c6ced4006f874365
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338716"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-eclipse"></a><span data-ttu-id="5d7b2-102">Eclipse 用のレガシ ツールキットを使用して Azure 用の Hello World Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="5d7b2-102">Create a Hello World web app for Azure using the legacy toolkit for Eclipse</span></span>

<span data-ttu-id="5d7b2-103">このチュートリアルでは、[Azure Toolkit for Eclipse] バージョン 3.0.6 以前を使用して、Web アプリとして基本的な Hello World アプリケーションを作成し、Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-103">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using version 3.0.6 (or earlier) of the [Azure Toolkit for Eclipse].</span></span>

> [!NOTE]
>
> <span data-ttu-id="5d7b2-104">この記事の [Azure Toolkit for IntelliJ] 使用バージョンについては、「[IntelliJ を使用して Azure 用の Hello World Web アプリを作成する][intellij-hello-world]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-104">For a version of this article that uses the [Azure Toolkit for IntelliJ], see [Create a Hello World web app for Azure using IntelliJ][intellij-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="5d7b2-105">Azure Toolkit for Eclipse は 2017 年 8 月に更新され、別のワークフローが導入されました。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-105">The Azure Toolkit for Eclipse was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="5d7b2-106">この記事では、Azure Toolkit for Eclipse のバージョン 3.0.6 以前を使用して Hello World Web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-106">This article illustrates creating a Hello World web app by using version 3.0.6 (or earlier) of the Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="5d7b2-107">バージョン 3.0.7 以降のツールキットを使用している場合は、[Eclipse での Azure 用 Hello World Web アプリの作成][Updated Version]に関するページの手順に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-107">If you are using the version 3.0.7 (or later) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in Eclipse][Updated Version].</span></span>
>

<span data-ttu-id="5d7b2-108">このチュートリアルを完了し、作成したアプリケーションを Web ブラウザーで開くと、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-108">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World アプリのプレビュー][01]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="5d7b2-110">新しい Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="5d7b2-110">Create a new web app project</span></span>

1. <span data-ttu-id="5d7b2-111">「[Azure Toolkit for Eclipse の Azure サインイン手順][eclipse-sign-in-instructions]」にの説明に従って、Eclipse を起動し、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-111">Start Eclipse, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for Eclipse][eclipse-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="5d7b2-112">**[ファイル]**、**[新規]**、**[Dynamic Web Project]\(動的 Web プロジェクト\)** の順にクリックします </span><span class="sxs-lookup"><span data-stu-id="5d7b2-112">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="5d7b2-113">(**[ファイル]** と **[新規]** のクリック後、使用可能なプロジェクトとして **[Dynamic Web Project (動的 Web プロジェクト)]** が表示されない場合は、**[ファイル]**、**[新規]**、**[プロジェクト]** の順にクリックし、**[Web]** を展開して、**[Dynamic Web Project (動的 Web プロジェクト)]**、**[次へ]** の順にクリックします)。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-113">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

2. <span data-ttu-id="5d7b2-114">このチュートリアルでは、プロジェクトに **MyWebApp**という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-114">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="5d7b2-115">画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-115">Your screen will appear similar to the following:</span></span>
   
   ![新しい動的 Web プロジェクトの作成][02]

3. <span data-ttu-id="5d7b2-117">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-117">Click **Finish**.</span></span>

4. <span data-ttu-id="5d7b2-118">Eclipse の**プロジェクト エクスプローラー** ビューで、**MyWebApp** を展開します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-118">Within Eclipse's **Project Explorer** view, expand **MyWebApp**.</span></span> <span data-ttu-id="5d7b2-119">**WebContent** を右クリックし、**[新規]**、**[JSP ファイル]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-119">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

5. <span data-ttu-id="5d7b2-120">**[New JSP File (新しい JSP ファイル)]** ダイアログ ボックスで **index.jsp** ファイルに名前を付け、親フォルダーは **MyWebApp/WebContent** のままにして **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-120">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

6. <span data-ttu-id="5d7b2-121">**[Select JSP Template (JSP テンプレートの選択)]** ダイアログ ボックスで、このチュートリアルのために **[New JSP File (html) (新しい JSP ファイル (html))]** を選択し、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-121">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

7. <span data-ttu-id="5d7b2-122">index.jsp ファイルが Eclipse で開いたら、"**Hello World!**" を動的に表示するためのテキストを</span><span class="sxs-lookup"><span data-stu-id="5d7b2-122">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="5d7b2-123">既存の `<body>` 要素に追加します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-123">within the existing `<body>` element.</span></span> <span data-ttu-id="5d7b2-124">更新された `<body>` コンテンツは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-124">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="5d7b2-125">index.jsp を保存します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-125">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="5d7b2-126">Azure への Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="5d7b2-126">Deploy your web app to Azure</span></span>

<span data-ttu-id="5d7b2-127">Java Web アプリケーションを Azure にデプロイする方法はいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-127">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="5d7b2-128">このチュートリアルでは、アプリケーションを Azure Web アプリ コンテナーにデプロイするという最も簡単な方法について説明します。特殊なプロジェクトの種類や追加のツールは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-128">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="5d7b2-129">JDK と Web コンテナー ソフトウェアは Azure から提供されるので、自分でアップロードする必要はありません。必要なものは Java Web アプリのみです。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-129">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="5d7b2-130">結果として、アプリケーションの発行プロセスにかかる時間は分単位ではなく、秒単位になります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-130">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="5d7b2-131">Eclipse の Project Explorer で **[MyWebApp]** を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-131">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>

2. <span data-ttu-id="5d7b2-132">コンテキスト メニューの **[Azure]**、**[Publish as Azure Web App (Azure Web アプリとして発行)]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-132">In the context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
   ![[Publish as Azure Web App (Azure Web アプリとして発行)]][03]
   
   <span data-ttu-id="5d7b2-134">または、Project Explorer で Web アプリケーション プロジェクトが選択されている状態で、ツール バーの **[発行]** ドロップダウン ボタンをクリックし、そこから **[Publish as Azure Web App (Azure Web アプリとして発行)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-134">Alternatively, while your web application project is selected in the Project Explorer, you can click the **Publish** dropdown button on the toolbar and select **Publish as Azure Web App** from there:</span></span>
   
   ![[Publish as Azure Web App (Azure Web アプリとして発行)]][14]

3. <span data-ttu-id="5d7b2-136">まだ Eclipse から Azure にサインインしていない場合、Azure アカウントにサインインするように求められます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-136">If you have not already signed into Azure from Eclipse, you will be prompted to sign into your Azure account:</span></span>
   
   ![Azure サインイン ダイアログ ボックス][04]
   
   <span data-ttu-id="5d7b2-138">複数の Azure アカウントがある場合、サインイン プロセス中に同じようなプロンプトが何度も表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-138">If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="5d7b2-139">このような状況の場合、次のサインイン手順を続行します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-139">When this happens, continue following the sign in instructions.</span></span>

4. <span data-ttu-id="5d7b2-140">Azure アカウントに正常にサインインすると、 **[サブスクリプションの管理]** ダイアログ ボックスに、資格情報に関連付けられたサブスクリプションの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-140">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="5d7b2-141">複数のサブスクリプションが表示された場合、その一部のみを使用するには、使用しないサブスクリプションのチェックボックスを必要に応じてオフにします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-141">If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the ones you do want to use.</span></span> <span data-ttu-id="5d7b2-142">サブスクリプションを選択したら、 **[閉じる]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-142">When you have selected your subscriptions, click **Close**.</span></span>
   
   ![[サブスクリプションの管理] ダイアログ ボックス][05]

5. <span data-ttu-id="5d7b2-144">**[Deploy to Azure Web App Container (Azure Web アプリ コンテナーにデプロイ)]** ダイアログ ボックスを開くと、以前に作成した Web アプリ コンテナーがすべて表示されます。コンテナーを作成していない場合、一覧は空欄です。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-144">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![[Deploy to Azure Web App Container (Azure Web アプリ コンテナーにデプロイ)] ダイアログ ボックス][06]

6. <span data-ttu-id="5d7b2-146">以前に Azure Web アプリ コンテナーを作成していない場合、またはアプリケーションを新しいコンテナーに発行する場合は、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-146">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="5d7b2-147">作成済みの場合は、既存の Web アプリ コンテナーを選択し、以下の手順 7 に進みます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-147">Otherwise, select an existing Web App Container and skip to step 7 below.</span></span>
   
   <span data-ttu-id="5d7b2-148">a.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-148">a.</span></span> <span data-ttu-id="5d7b2-149"> *\*[New (新規)]**</span><span class="sxs-lookup"><span data-stu-id="5d7b2-149">Click **New...**</span></span>
      
      ![[Deploy to Azure Web App Container (Azure Web アプリ コンテナーにデプロイ)] ダイアログ ボックス][15]

   <span data-ttu-id="5d7b2-151">b.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-151">b.</span></span> <span data-ttu-id="5d7b2-152">**[New Web App Container (新しい Web アプリ コンテナー)]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-152">The **New Web App Container** dialog box will be displayed:</span></span>
      
      ![[New Web App Container (新しい Web アプリ コンテナー)] ダイアログ ボックス][07a]

   <span data-ttu-id="5d7b2-154">c.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-154">c.</span></span> <span data-ttu-id="5d7b2-155">Web アプリ コンテナーの **[DNS Label (DNS ラベル)]** を入力します。これで、Azure の Web アプリケーションについて、ホスト URL のリーフ DNS ラベルが構成されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-155">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="5d7b2-156">(この名前は使用可能であり、Azure Web アプリの名前付け要件に準拠している必要があります。)</span><span class="sxs-lookup"><span data-stu-id="5d7b2-156">(Note that the name must be available and conform to Azure Web App naming requirements.)</span></span>

   <span data-ttu-id="5d7b2-157">d.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-157">d.</span></span> <span data-ttu-id="5d7b2-158">**[Web Container (Web コンテナー)]** ドロップダウン メニューで、アプリケーションに適したソフトウェアを選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-158">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="5d7b2-159">現在、Tomcat 8、Tomcat 7 または Jetty 9 から選択することができます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-159">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="5d7b2-160">選択したソフトウェアの最新ディストリビューションが Azure で提供され、Azure で提供される JDK の最新ディストリビューションで実行されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-160">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of the JDK provided by Azure.</span></span>

   <span data-ttu-id="5d7b2-161">e.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-161">e.</span></span> <span data-ttu-id="5d7b2-162">**[サブスクリプション]** ドロップダウン メニューで、このデプロイに使用するサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-162">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="5d7b2-163">f.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-163">f.</span></span> <span data-ttu-id="5d7b2-164">**[Resource Group (リソース グループ)]** ドロップダウン メニューで、Web アプリに関連付けるリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-164">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="5d7b2-165">(Azure リソース グループを使用すると、関連リソースをグループ化できるため、たとえば、リソースをまとめて削除できます。)</span><span class="sxs-lookup"><span data-stu-id="5d7b2-165">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="5d7b2-166">(所有している場合は) 既存のリソース グループを選択して、下記のステップ g にスキップするか、以下のステップに従って、新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-166">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following these steps to create a new Resource Group:</span></span>
      
   * <span data-ttu-id="5d7b2-167"> *\*[New (新規)]**</span><span class="sxs-lookup"><span data-stu-id="5d7b2-167">Click **New...**</span></span>
   * <span data-ttu-id="5d7b2-168">**[New Resource Group (新しいリソース グループ)]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-168">The **New Resource Group** dialog box will be displayed:</span></span>
        
       ![[New Resource Group (新しいリソース グループ)] ダイアログ ボックス][08]
   * <span data-ttu-id="5d7b2-170">**[Name (名前)]** テキスト ボックスに、新しいリソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-170">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
   * <span data-ttu-id="5d7b2-171">**[Region (リージョン)]** ドロップダウン メニューで、リソース グループに適した Azure データ センターの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-171">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
   * <span data-ttu-id="5d7b2-172">オプション: 既定では、Java 8 の最新の配布は、Azure によって Web アプリ コンテナーに JVM として自動的にデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically to your web app container as your JVM.</span></span> <span data-ttu-id="5d7b2-173">ただし、Web アプリで必要な場合は、JVM の別のバージョンと配布を指定できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-173">However, you can specify a different version and distribution of the JVM if your Web App requires it.</span></span> <span data-ttu-id="5d7b2-174">Web アプリの JDK を指定するには、 **[JDK]** タブをクリックし、次のオプションのいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-174">To specify the JDK for your Web App, click the **JDK** tab, and select one of the following options:</span></span>
     * <span data-ttu-id="5d7b2-175">**[Deploy the default JDK offered by Azure Web Apps service (Azure Web Apps サービスによって提供される既定の JDK をデプロイする)]**: Java の最新の配布がデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-175">**Deploy the default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java.</span></span>
     * <span data-ttu-id="5d7b2-176">**[Deploy a 3rd party JDK available on Azure (Azure で利用できるサード パーティの JDK をデプロイする)]**: Microsoft Azure によって提供される JDK のリストから選択できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-176">**Deploy a 3rd party JDK available on Azure**: This option allows you to choose from the list of JDKs which are provided by Microsoft Azure.</span></span>
     * <span data-ttu-id="5d7b2-177">**[Deploy my own JDK from this download location (このダウンロード場所から独自の JDK をデプロイする)]**: 独自の JDK 配布を指定できます。JDK 配布は、ZIP ファイルとしてパッケージ化し、公開されているダウンロード場所またはアクセスできる Azure Storage アカウントにアップロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-177">**Deploy my own JDK from this download location**: This option allows you to specify your own JDK distribution, which must be packaged as a ZIP file and uploaded to either a publicly available download location or an Azure storage account for which you have access.</span></span>
          
       ![[New Web App Container (新しい Web アプリ コンテナー)] ダイアログ ボックス][07b]

   <span data-ttu-id="5d7b2-179">g.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-179">g.</span></span> <span data-ttu-id="5d7b2-180">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-180">Click **OK**.</span></span>

   <span data-ttu-id="5d7b2-181">h.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-181">h.</span></span> <span data-ttu-id="5d7b2-182">**[App Service Plan (App Service プラン)]** ドロップダウン メニューには、選択したリソース グループに関連付けられた App Service プランが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-182">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="5d7b2-183">(App Service プランでは、Web アプリの場所、価格レベル、コンピューティング インスタンス サイズなどの情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-183">(App Service Plans specify information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="5d7b2-184">単一の App Service プランを複数の Web Apps に使用できます。そのため、App Service プランは、特定の Web アプリのデプロイとは別に保持されます。)</span><span class="sxs-lookup"><span data-stu-id="5d7b2-184">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:
      
      * <span data-ttu-id="5d7b2-185"> *\*[New (新規)]**</span><span class="sxs-lookup"><span data-stu-id="5d7b2-185">Click **New...**</span></span>
      * <span data-ttu-id="5d7b2-186">**[New App Service Plan (新しい App Service プラン)]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-186">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![[新しい App Service プラン] ダイアログ ボックス][09]
      * <span data-ttu-id="5d7b2-188">**[Name (名前)]** ボックスに、新しい App Service プランの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-188">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="5d7b2-189">**[Location (場所)]** ドロップダウン メニューで、プランに適した Azure データ センターの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-189">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="5d7b2-190">**[Pricing Tier (価格レベル)]** ドロップダウン メニューで、プランに適した価格を選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-190">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="5d7b2-191">テスト目的の場合は、 **[Free]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-191">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="5d7b2-192">**[Instance Size (インスタンス サイズ)]** ドロップダウン メニューで、プランに適したインスタンス サイズを選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-192">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="5d7b2-193">テスト目的の場合は、 **[Small]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-193">For testing purposes you can choose **Small**.</span></span>

   <span data-ttu-id="5d7b2-194">i.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-194">i.</span></span> <span data-ttu-id="5d7b2-195">これらの手順をすべて完了すると、[New Web App Container] \(新しい Web アプリ コンテナー) ダイアログ ボックスは次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-195">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![[New Web App Container (新しい Web アプリ コンテナー)] ダイアログ ボックス][10]

   <span data-ttu-id="5d7b2-197">j.</span><span class="sxs-lookup"><span data-stu-id="5d7b2-197">j.</span></span> <span data-ttu-id="5d7b2-198">**[OK]** をクリックすると、新しい Web アプリ コンテナーの作成が完了します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-198">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="5d7b2-199">数秒待つと Web アプリ コンテナーの一覧が更新されます。一覧で新しく作成した Web アプリ コンテナーが選択されています。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-199">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

7. <span data-ttu-id="5d7b2-200">以上で、初めて Web アプリを Azure にデプロイする処理を完了できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-200">You are now ready to complete the initial deployment of your Web App to Azure:</span></span>
   
   ![[Deploy to Azure Web App Container (Azure Web アプリ コンテナーにデプロイ)] ダイアログ ボックス][11]
   
   <span data-ttu-id="5d7b2-202">**[OK]** をクリックして、Java アプリケーションを選択した Web アプリ コンテナーにデプロイします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-202">Click **OK** to deploy your Java application to the selected Web App container.</span></span>
   
   <span data-ttu-id="5d7b2-203">既定では、アプリケーションはアプリケーション サーバーのサブディレクトリとしてデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-203">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="5d7b2-204">ルート アプリケーションとしてデプロイする場合、**[Deploy to root (ルートにデプロイ)]** チェック ボックスをオンにして **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-204">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>

8. <span data-ttu-id="5d7b2-205">**[Azure Activity Log (Azure アクティビティ ログ)]** ビューが開き、Web アプリのデプロイの状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-205">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![[Azure Activity Log (Azure アクティビティ ログ)]][12]
   
   <span data-ttu-id="5d7b2-207">Web アプリを Azure にデプロイするプロセスは、わずか数秒で完了します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-207">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="5d7b2-208">アプリケーションの準備ができると、 **[Published (発行済み)]** in the **[Published (発行済み)]** というリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-208">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="5d7b2-209">リンクをクリックすると、デプロイした Web アプリのホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-209">When you click the link, it will take you to your deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="5d7b2-210">Web アプリの更新</span><span class="sxs-lookup"><span data-stu-id="5d7b2-210">Updating your web app</span></span>

<span data-ttu-id="5d7b2-211">既存の実行中の Azure Web アプリを更新するプロセスは短時間で簡単です。更新には 2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-211">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="5d7b2-212">既存の Java Web アプリのデプロイを更新できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-212">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="5d7b2-213">同じ Web アプリ コンテナーに追加の Java アプリケーションを発行できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-213">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="5d7b2-214">いずれの場合でもプロセスは同じで、かかる時間は数秒です。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-214">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="5d7b2-215">Eclipse の Project Explorer で、更新する Java アプリケーションを右クリックするか、既存の Web アプリ コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-215">In the Eclipse project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>

2. <span data-ttu-id="5d7b2-216">コンテキスト メニューが表示されたら、**[Azure]**、**[Publish as Azure Web App (Azure Web アプリとして発行)]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-216">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>

3. <span data-ttu-id="5d7b2-217">既にログインしているので、既存の Web アプリ コンテナーの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-217">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="5d7b2-218">Java アプリケーションを発行または再発行するコンテナーを選択し、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-218">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="5d7b2-219">数秒後、**[Azure Activity Log (Azure アクティビティ ログ)]** ビューに更新されたデプロイが **[Published (発行済み)]** と表示され、Web ブラウザーで更新されたアプリケーションを確認できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-219">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="5d7b2-220">既存の Web アプリの起動、停止、再起動</span><span class="sxs-lookup"><span data-stu-id="5d7b2-220">Starting, stopping, or restarting an existing web app</span></span>

<span data-ttu-id="5d7b2-221">既存の Azure Web アプリ コンテナー (コンテナー内にデプロイされているすべての Java アプリケーションを含む) を起動または停止するには、 **[Azure Explorer]** (Azure Explorer) ビューを使用できます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-221">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="5d7b2-222">**Azure 用エクスプローラー** ビューがまだ開いていない場合、Eclipse の **[ウィンドウ]** メニュー、**[Show View (ビューの表示)]**、**[Other (その他)]**、**[Azure]**、**[Azure Explorer (Azure 用エクスプローラー)]** の順にクリックして開きます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-222">If the **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="5d7b2-223">まだログインしていない場合は、ログインするように求められます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-223">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="5d7b2-224">**[Azure Explorer]** (Azure Explorer) ビューが表示されたら、次の手順に従って Web アプリを起動また停止します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-224">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="5d7b2-225">**[Azure]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-225">Expand the **Azure** node.</span></span>

2. <span data-ttu-id="5d7b2-226">**[Web Apps (Web アプリ)]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-226">Expand the **Web Apps** node.</span></span> 

3. <span data-ttu-id="5d7b2-227">目的の Web App を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-227">Right-click the desired Web App.</span></span>

4. <span data-ttu-id="5d7b2-228">コンテキスト メニューが表示されたら、**[Start (起動)]\*\*\*\*[Stop (停止)]**、または **[Restart (再起動)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-228">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="5d7b2-229">メニュー項目はコンテキストに依存します。つまり、Web アプリが実行しているときは停止操作のみ、Web アプリが現在実行されていないときは起動操作のみを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-229">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![既存の Web アプリを停止する][13]

## <a name="next-steps"></a><span data-ttu-id="5d7b2-231">次の手順</span><span class="sxs-lookup"><span data-stu-id="5d7b2-231">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="5d7b2-232">Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5d7b2-232">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Updated Version]: azure-toolkit-for-eclipse-create-hello-world-web-app.md
[eclipse-sign-in-instructions]: azure-toolkit-for-eclipse-sign-in-instructions.md


<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/01-Web-Page.png
[02]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/02-Dynamic-Web-Project.png
[03]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/03-Context-Menu.png
[04]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/04-Log-In-Dialog.png
[05]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/05-Manage-Subscriptions-Dialog.png
[06]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/07b-New-Web-App-Container-Dialog.png
[08]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/08-New-Resource-Group-Dialog.png
[09]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/09-New-Service-Plan-Dialog.png
[10]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/11-Completed-Deploy-Dialog.png
[12]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/12-Activity-Log-View.png
[13]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/13-Azure-Explorer-Web-App.png
[14]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/14-publishDropdownButton.png
[15]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/15-New-Azure-Web-Container.png
