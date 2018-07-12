---
title: Azure Toolkit for Eclipse を使用して Spring Boot アプリを Docker コンテナーとして発行する
description: Azure Toolkit for Eclipse を使用して、Web アプリを Docker コンテナーとして Microsoft Azure に発行する方法について説明します。
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
ms.openlocfilehash: c116e0712afd8e48983f946f43eddfd0c79c0ba8
ms.sourcegitcommit: 0ed7c5af0152125322ff1d265c179f35028f3c15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38075474"
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="758cf-103">Azure Toolkit for Eclipse を使用して Spring Boot アプリを Docker コンテナーとして発行する</span><span class="sxs-lookup"><span data-stu-id="758cf-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="758cf-104">[Spring Framework] は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="758cf-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="758cf-105">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="758cf-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="758cf-106">[Docker] は、開発者が、コンテナーで実行されるアプリケーションのデプロイ、スケーリング、管理を自動化することを支援するオープン ソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="758cf-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="758cf-107">このチュートリアルでは、Azure Toolkit for Eclipse を使用して Spring Boot アプリケーションを Docker コンテナーとして Microsoft Azure にデプロイする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="758cf-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="758cf-108">既定の Spring Boot Docker リポジトリの複製</span><span class="sxs-lookup"><span data-stu-id="758cf-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="758cf-109">パブリック リポジトリのインポート</span><span class="sxs-lookup"><span data-stu-id="758cf-109">Import the public repository</span></span>

<span data-ttu-id="758cf-110">次の手順では、IntelliJ を使用して、ローカル コンピューターに Spring Boot Docker リポジトリを複製する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="758cf-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="758cf-111">コマンド ラインの使用が適している場合は、「[Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする][Deploy Spring Boot on Linux in AKS]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="758cf-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS].</span></span>

1. <span data-ttu-id="758cf-112">Eclipse を開きます。</span><span class="sxs-lookup"><span data-stu-id="758cf-112">Open Eclipse.</span></span>

1. <span data-ttu-id="758cf-113">**[ファイル]** > **[インポート]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-113">Click **File** > **Import**.</span></span>

   ![[ファイル] の [インポート] メニュー][CL01]

1. <span data-ttu-id="758cf-115">**[インポート]** ダイアログ ボックスが開いたら、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="758cf-116">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-116">a.</span></span> <span data-ttu-id="758cf-117">**[Git]** を展開します。</span><span class="sxs-lookup"><span data-stu-id="758cf-117">Expand **Git**.</span></span>

   <span data-ttu-id="758cf-118">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-118">b.</span></span> <span data-ttu-id="758cf-119">**[Projects from Git]\(Git のプロジェクト\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="758cf-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="758cf-120">c.</span><span class="sxs-lookup"><span data-stu-id="758cf-120">c.</span></span> <span data-ttu-id="758cf-121">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-121">Click **Next**.</span></span>

   ![[インポート] ダイアログ ボックス][CL02]

1. <span data-ttu-id="758cf-123">**[Select Repository Source]\(リポジトリ ソースの選択\)** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="758cf-124">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-124">a.</span></span> <span data-ttu-id="758cf-125">**[Clone URI]\(URI の複製\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="758cf-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="758cf-126">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-126">b.</span></span> <span data-ttu-id="758cf-127">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-127">Click **Next**.</span></span>

   ![リポジトリ ソース ページの選択][CL03]

1. <span data-ttu-id="758cf-129">**[Source Git Repository]\(ソース Git リポジトリ\)** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="758cf-130">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-130">a.</span></span> <span data-ttu-id="758cf-131">**[URI]** に「`https://github.com/spring-guides/gs-spring-boot-docker.git`」と入力します。</span><span class="sxs-lookup"><span data-stu-id="758cf-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="758cf-132">これで **[Host]\(ホスト\)** と **[Repository path]\(リポジトリ パス\)** の各フィールドに正しい値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="758cf-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="758cf-133">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-133">b.</span></span> <span data-ttu-id="758cf-134">Spring Boot リポジトリは公開されるため、Git のユーザー名とパスワードを入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="758cf-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="758cf-135">c.</span><span class="sxs-lookup"><span data-stu-id="758cf-135">c.</span></span> <span data-ttu-id="758cf-136">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-136">Click **Next**.</span></span>

   ![[Source Git Repository]\(ソース Git リポジトリ\) ページ][CL04]

1. <span data-ttu-id="758cf-138">**[Branch Selection]\(ブランチの選択\)** ページで **[Next]\(次へ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![[Branch Selection]\(ブランチの選択\) ページ][CL05]

1. <span data-ttu-id="758cf-140">**[Local Destination]\(ローカルの保存先\)** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="758cf-141">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-141">a.</span></span> <span data-ttu-id="758cf-142">ローカル リポジトリを保存するローカル フォルダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="758cf-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="758cf-143">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-143">b.</span></span> <span data-ttu-id="758cf-144">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-144">Click **Next**.</span></span>

   ![[Local Destination]\(ローカルの保存先\) ページ][CL06]

1. <span data-ttu-id="758cf-146">**[Select a wizard to use for importing projects]\(プロジェクトのインポートに使用するウィザードの選択\)** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="758cf-147">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-147">a.</span></span> <span data-ttu-id="758cf-148">**[Import as a general project]\(一般的なプロジェクトのインポート\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="758cf-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="758cf-149">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-149">b.</span></span> <span data-ttu-id="758cf-150">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-150">Click **Next**.</span></span>

   ![[Select a wizard to use for importing projects]\(プロジェクトのインポートに使用するウィザードの選択\) ページ][CL07]

1. <span data-ttu-id="758cf-152">**[Import Projects]\(プロジェクトのインポート\)** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="758cf-153">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-153">a.</span></span> <span data-ttu-id="758cf-154">プロジェクト名を指定します。</span><span class="sxs-lookup"><span data-stu-id="758cf-154">Specify your project name.</span></span>
   
   <span data-ttu-id="758cf-155">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-155">b.</span></span> <span data-ttu-id="758cf-156">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-156">Click **Finish**.</span></span>

   ![[Import Projects]\(プロジェクトのインポート\) ページ][CL08]

1. <span data-ttu-id="758cf-158">リポジトリの複製が完了すると、Eclipse にすべてのファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="758cf-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![ローカル リポジトリ][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="758cf-160">ローカル リポジトリからの Maven プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="758cf-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="758cf-161">Spring Boot Docker リポジトリには、このチュートリアルで使用する完成した Maven プロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="758cf-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="758cf-162">**[ファイル]** > **[インポート]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-162">Click **File** > **Import**.</span></span>

   ![[ファイル] メニューの [インポート] コマンド][CL01]

1. <span data-ttu-id="758cf-164">**[インポート]** ダイアログ ボックスが開いたら、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="758cf-165">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-165">a.</span></span> <span data-ttu-id="758cf-166">**[Maven]** を展開します。</span><span class="sxs-lookup"><span data-stu-id="758cf-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="758cf-167">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-167">b.</span></span> <span data-ttu-id="758cf-168">**[Existing Maven Projects]\(既存の Maven プロジェクト\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="758cf-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="758cf-169">c.</span><span class="sxs-lookup"><span data-stu-id="758cf-169">c.</span></span> <span data-ttu-id="758cf-170">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-170">Click **Next**.</span></span>

   ![[インポート] ダイアログ ボックス][MV01]

1. <span data-ttu-id="758cf-172">**[Maven Projects]\(Maven プロジェクト\)** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="758cf-173">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-173">a.</span></span> <span data-ttu-id="758cf-174">**[Root Directory]\(ルート ディレクトリ\)** に、ローカル リポジトリの **complete** フォルダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="758cf-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="758cf-175">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-175">b.</span></span> <span data-ttu-id="758cf-176">**[Advanced]\(詳細\)** セクションを展開し、**[Name template]\(テンプレートの名前\)** にカスタムの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="758cf-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="758cf-177">c.</span><span class="sxs-lookup"><span data-stu-id="758cf-177">c.</span></span> <span data-ttu-id="758cf-178">プロジェクトの **pom.xml** ファイルのボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="758cf-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="758cf-179">d.</span><span class="sxs-lookup"><span data-stu-id="758cf-179">d.</span></span> <span data-ttu-id="758cf-180">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-180">Click **Finish**.</span></span>

   ![[Maven Projects]\(Maven プロジェクト\) ページ][MV02]

1. <span data-ttu-id="758cf-182">Maven プロジェクトが正常に開くと、Eclipse に 2 つ目のプロジェクトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="758cf-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![ローカルの Maven プロジェクト][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="758cf-184">Maven での Spring Boot アプリのビルド</span><span class="sxs-lookup"><span data-stu-id="758cf-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="758cf-185">Eclipse Project Explorer で、Maven プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="758cf-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="758cf-186">**[実行]** > **[Run As]\(プログラム名を指定して実行\)** > **[Maven build]\(Maven ビルド\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![[Run As]\(プログラム名を指定して実行\) の [Maven build]\(Maven ビルド\) コマンド][BU01]

1. <span data-ttu-id="758cf-188">アプリケーションが正常にビルドされると、コンソール ウィンドウに状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="758cf-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Maven ビルドの成功][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="758cf-190">Docker コンテナーを使用して Web アプリを Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="758cf-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="758cf-191">Eclipse Project Explorer で、Maven プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="758cf-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="758cf-192">Azure の **[Publish]\(発行\)** メニューをクリックし、**[Publish as Docker container]\(Docker コンテナーとして発行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![[Publish as Docker Container]\(Docker コンテナーとして発行\) コマンド][PU01]

1. <span data-ttu-id="758cf-194">**[Deploy Docker Container on Azure]\(Azure への Docker コンテナーのデプロイ\)** ダイアログ ボックスが表示されたら、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="758cf-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="758cf-195">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-195">a.</span></span> <span data-ttu-id="758cf-196">カスタムの Docker イメージ名を入力します。</span><span class="sxs-lookup"><span data-stu-id="758cf-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="758cf-197">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-197">b.</span></span> <span data-ttu-id="758cf-198">**[Artifact to deploy]\(デプロイするアーティファクト\)** に、ビルドした **gs-spring-boot-docker-0.1.0.jar** ファイルのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="758cf-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Docker オプションの指定][PU02]

   <span data-ttu-id="758cf-200">既存の Docker ホストがすべて表示されます。</span><span class="sxs-lookup"><span data-stu-id="758cf-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="758cf-201">既存のホストへのデプロイを選択する場合は、スキップして手順 5 に進みます。</span><span class="sxs-lookup"><span data-stu-id="758cf-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="758cf-202">そうでない場合は、次の手順に従ってホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="758cf-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="758cf-203">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-203">a.</span></span> <span data-ttu-id="758cf-204">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-204">Click **Add**.</span></span>

      ![新しい Docker ホストの追加][PU03]

   <span data-ttu-id="758cf-206">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-206">b.</span></span> <span data-ttu-id="758cf-207">**[Create Docker Host]\(Docker ホストの作成\)** ダイアログ ボックスが表示されたら、新しい Docker ホストに既定値をそのまま使用するか、カスタム設定を指定できます </span><span class="sxs-lookup"><span data-stu-id="758cf-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="758cf-208">(さまざまな設定について詳しくは、「[Azure Toolkit for IntelliJ を使用して Web アプリを Docker コンテナーとして発行する][Publish Container with Azure Toolkit]」をご覧ください)。使用する設定を指定したら、**[Next]\(次へ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Docker ホスト オプションの指定][PU04]

   <span data-ttu-id="758cf-210">c.</span><span class="sxs-lookup"><span data-stu-id="758cf-210">c.</span></span> <span data-ttu-id="758cf-211">Azure Key Vault の既存のログイン資格情報を使用することも、新しい Docker ログイン資格情報を入力することもできます。</span><span class="sxs-lookup"><span data-stu-id="758cf-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="758cf-212">オプションを指定したら、**[Finish]\(完了\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-212">Click **Finish** when you have specified your options.</span></span>

      ![Docker ホストの資格情報の指定][PU05]

1. <span data-ttu-id="758cf-214">Docker ホストを選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-214">Select your Docker host, and then click **Next**.</span></span>

   ![使用する Docker ホストの選択][PU06]

1. <span data-ttu-id="758cf-216">**[Deploying Docker Container on Azure]\(Azure への Docker コンテナーのデプロイ\)** ダイアログ ボックスの最後のページで、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="758cf-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="758cf-217">a.</span><span class="sxs-lookup"><span data-stu-id="758cf-217">a.</span></span> <span data-ttu-id="758cf-218">Docker コンテナーをホストするコンテナーのカスタム名を指定するか、既定値をそのまま使用することができます。</span><span class="sxs-lookup"><span data-stu-id="758cf-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="758cf-219">b.</span><span class="sxs-lookup"><span data-stu-id="758cf-219">b.</span></span> <span data-ttu-id="758cf-220">"*[外部ポート]*:*[内部ポート]*" という構文で、Docker ホストの TCP ポートを入力します。</span><span class="sxs-lookup"><span data-stu-id="758cf-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="758cf-221">たとえば "**80:8080**" では、外部ポート "80" と既定の内部 Spring Boot ポート "8080" が指定されます。</span><span class="sxs-lookup"><span data-stu-id="758cf-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="758cf-222">内部ポートをカスタマイズした場合 (application.yml ファイルを編集するなどして)、Azure で正しいルーティングが実現するようポート番号を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="758cf-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="758cf-223">c.</span><span class="sxs-lookup"><span data-stu-id="758cf-223">c.</span></span> <span data-ttu-id="758cf-224">これらのオプションを構成したら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="758cf-224">After you configure these options, click **Finish**.</span></span>

   ![Azure への Docker コンテナーのデプロイ][PU07]

1. <span data-ttu-id="758cf-226">Azure Toolkit による発行が完了したら、Azure Activity Log の状態が**発行済み**になります。</span><span class="sxs-lookup"><span data-stu-id="758cf-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![正常にデプロイされた Docker ホスト][PU08]

## <a name="next-steps"></a><span data-ttu-id="758cf-228">次の手順</span><span class="sxs-lookup"><span data-stu-id="758cf-228">Next steps</span></span>

<span data-ttu-id="758cf-229">Docker の他のリソースについては、公式の [Docker の Web サイト](https://www.docker.com/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="758cf-229">For additional resources for Docker, see the official [Docker website](https://www.docker.com/).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in AKS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: azure-toolkit-for-eclipse-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
