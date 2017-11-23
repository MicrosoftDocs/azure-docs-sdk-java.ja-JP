---
title: "Azure Toolkit for IntelliJ を使用して Spring Boot アプリを Docker コンテナーとして発行する"
description: "Azure Toolkit for IntelliJ を使用して、Web アプリを Docker コンテナーとして Microsoft Azure に発行する方法について説明します。"
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
ms.openlocfilehash: 65fbdc32824c2b6312929f4888844d1673101ac8
ms.sourcegitcommit: 062e07cbd42cda74f02c82b933ce90da646a50a0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="82521-103">Azure Toolkit for IntelliJ を使用して Spring Boot アプリを Docker コンテナーとして発行する</span><span class="sxs-lookup"><span data-stu-id="82521-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="82521-104">[Spring Framework] は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="82521-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="82521-105">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="82521-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="82521-106">[Docker] は、開発者が、コンテナーで実行されるアプリケーションのデプロイ、スケーリング、および管理を自動化することを支援するオープン ソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="82521-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="82521-107">このチュートリアルでは、Azure Toolkit for IntelliJ を使用して Spring Boot アプリケーションを Docker コンテナーとして Microsoft Azure にデプロイする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="82521-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a><span data-ttu-id="82521-108">既定の Spring Boot Docker リポジトリの複製</span><span class="sxs-lookup"><span data-stu-id="82521-108">Clone the default Spring Boot Docker repo</span></span>

<span data-ttu-id="82521-109">次の手順では、IntelliJ を使用して、Spring Boot Docker リポジトリを複製する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="82521-109">The following steps walk you through cloning the Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="82521-110">コマンド ラインの使用が適している場合は、「[Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする][Deploy Spring Boot on Linux in AKS]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="82521-110">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS].</span></span>

1. <span data-ttu-id="82521-111">IntelliJ を開きます。</span><span class="sxs-lookup"><span data-stu-id="82521-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="82521-112">[ようこそ] 画面で、**[Check out from Version Control]\(バージョン管理からチェックアウト\)** 一覧の **[GitHub]** オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="82521-112">On the welcome screen, select the **GitHub** option in the **Check out from Version Control** list.</span></span>

   ![バージョン コントロールの GitHub オプション][CL01]

1. <span data-ttu-id="82521-114">ログインを求められたら、資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="82521-114">Enter your credentials if you are prompted to log in.</span></span>

   * <span data-ttu-id="82521-115">ユーザー名/パスワードを使用して GitHub にログインする場合:</span><span class="sxs-lookup"><span data-stu-id="82521-115">If you are using a username/password to log in to GitHub:</span></span> 

      ![GitHub ユーザー名とパスワードを入力するためのダイアログ ボックス][CL02a]

   * <span data-ttu-id="82521-117">トークンを使用して GitHub にログインする場合:</span><span class="sxs-lookup"><span data-stu-id="82521-117">If you are using a token to log in to GitHub:</span></span> 

      ![GitHub トークンを入力するためのダイアログ ボックス][CL02b]

1. <span data-ttu-id="82521-119">リポジトリ URL に「**https://github.com/spring-guides/gs-spring-boot-docker.git**」と入力し、ローカル パスとフォルダー情報を指定して、**[複製]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for the repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![リポジトリの複製ダイアログ ボックス][CL03]

1. <span data-ttu-id="82521-121">IntelliJ プロジェクトの作成を求められた場合は、**[いいえ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="82521-121">When you're prompted to create an IntelliJ project, select **No**.</span></span>

   ![IntelliJ プロジェクトの作成を拒否][CL04]

1. <span data-ttu-id="82521-123">ホーム ページで **[プロジェクトのインポート]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-123">On the welcome page, click **Import Project**.</span></span>

   ![[プロジェクトのインポート] の選択内容][CL05]

1. <span data-ttu-id="82521-125">Spring Boot リポジトリを複製したパスを見つけて、ルートの下の **complete** フォルダーを選択して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-125">Locate the path where you cloned the Spring Boot repo, select the **complete** folder under the root, and then click **OK**.</span></span>

   ![インポート用フォルダーの選択][CL06]

1. <span data-ttu-id="82521-127">メッセージが表示されたら、**[Create project from existing sources]\(既存のソースからプロジェクトを作成\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="82521-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![[Create project from existing sources]\(既存のソースからプロジェクトを作成\)][CL07]

1. <span data-ttu-id="82521-129">プロジェクト名を指定するか、既定値を受け入れて、**complete** フォルダーの正確なパスを確認し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-129">Specify your project name or accept the default, verify the correct path to the **complete** folder, and then click **Next**.</span></span>

   ![プロジェクト名の指定][CL08]

1. <span data-ttu-id="82521-131">インポート用にディレクトリをカスタマイズし、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![ディレクトリの選択][CL09]

1. <span data-ttu-id="82521-133">インポートするライブラリを確認し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-133">Review the libraries to import, and then click **Next**.</span></span>

   ![プロジェクト ライブラリの確認][CL10]

1. <span data-ttu-id="82521-135">モジュールの構造を確認し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-135">Review the module structure, and then click **Next**.</span></span>

   ![モジュールの構造を確認][CL11]

1. <span data-ttu-id="82521-137">JDK を指定し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-137">Specify your JDK, and then click **Next**.</span></span>

   ![JDK の指定][CL12]

1. <span data-ttu-id="82521-139">**[完了]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-139">Click **Finish**.</span></span>

   ![[完了] ボタン][CL13]

<span data-ttu-id="82521-141">IntelliJ により Spring Boot アプリがプロジェクトとしてインポートされ、インポートが完了すると構造が表示されます。</span><span class="sxs-lookup"><span data-stu-id="82521-141">IntelliJ imports the Spring Boot app as a project and displays the structure when the import has finished.</span></span>

![IntelliJ での Spring Boot アプリ][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="82521-143">Spring Boot アプリのビルド</span><span class="sxs-lookup"><span data-stu-id="82521-143">Build your Spring Boot app</span></span>

### <a name="build-the-app-by-using-the-maven-pom"></a><span data-ttu-id="82521-144">Maven POM を使用したアプリのビルド</span><span class="sxs-lookup"><span data-stu-id="82521-144">Build the app by using the Maven POM</span></span>

1. <span data-ttu-id="82521-145">Maven ツール ウィンドウがまだ開かれていない場合は、Maven ツール ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="82521-145">Open the Maven tool window if it is not already opened.</span></span> <span data-ttu-id="82521-146">**[表示]** > **[ツール ウィンドウ]** > **[Maven Projects]\(Maven プロジェクト\)** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![ツール ウィンドウおよび Maven プロジェクト コマンド][BU01]

1. <span data-ttu-id="82521-148">Maven ツール ウィンドウで **[パッケージ]** を右クリックして、**[Run Maven Build]\(Maven ビルドの実行\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="82521-148">In the Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="82521-149">(Maven プロジェクトが自動的に表示されない場合は、Maven ツール バーで **[Reimport]\(再インポート\)** アイコンをクリックします。)</span><span class="sxs-lookup"><span data-stu-id="82521-149">(If your Maven project does not show up automatically, click the **Reimport** icon on the Maven toolbar.)</span></span>

   ![Maven ビルド コマンドの実行][BU02]

1. <span data-ttu-id="82521-151">Spring Boot アプリが正常に作成されると、IntelliJ により、**BUILD SUCCESS** メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="82521-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![ビルド成功のメッセージ][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="82521-153">デプロイの準備ができたアーティファクトの作成</span><span class="sxs-lookup"><span data-stu-id="82521-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="82521-154">Spring Boot アプリを発行するには、デプロイの準備ができたアーティファクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82521-154">To publish your Spring Boot app, you need to create a deployment-ready artifact.</span></span> <span data-ttu-id="82521-155">次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="82521-155">Use the following steps:</span></span>

1. <span data-ttu-id="82521-156">IntelliJ で、Web アプリ プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="82521-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="82521-157">**[ファイル]** をクリックし、**[プロジェクトの構造]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-157">Click **File**, and then click **Project Structure**.</span></span>

   ![[プロジェクトの構造] コマンド][ART01]

1. <span data-ttu-id="82521-159">緑色のプラス (**+**) 記号をクリックしてアーティファクトを追加し、**[JAR]** をクリックして、**[Empty]\(空にする\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-159">Click the green plus (**+**) symbol to add an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![アーティファクトの追加][ART02]

1. <span data-ttu-id="82521-161">".jar" 拡張子を追加しないようにしてアーティファクトに名前を付け、Maven の出力用のターゲット フォルダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="82521-161">Name your artifact while making sure not to add the ".jar" extension, and then specify the target folder for the Maven output.</span></span>

   ![アーティファクトのプロパティを指定][ART03]

1. <span data-ttu-id="82521-163">アーティファクトのマニフェストを作成します (省略可能)。</span><span class="sxs-lookup"><span data-stu-id="82521-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="82521-164">a.</span><span class="sxs-lookup"><span data-stu-id="82521-164">a.</span></span> <span data-ttu-id="82521-165">**[Create Manifest]\(マニフェストの作成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-165">Click **Create Manifest**.</span></span>

      ![[マニフェストの作成] ボタンをクリック][ART04a]

   <span data-ttu-id="82521-167">b.</span><span class="sxs-lookup"><span data-stu-id="82521-167">b.</span></span> <span data-ttu-id="82521-168">アーティファクトの既定のパスを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-168">Choose the default path for the artifact, and then click **OK**.</span></span>

      ![アーティファクトのパスを指定][ART04b]

   <span data-ttu-id="82521-170">c.</span><span class="sxs-lookup"><span data-stu-id="82521-170">c.</span></span> <span data-ttu-id="82521-171">省略記号 (**...**) をクリックして、メイン クラスを検索します。</span><span class="sxs-lookup"><span data-stu-id="82521-171">Click the ellipsis (**...**) to locate the main class.</span></span>

      ![メイン クラスの検索][ART04c]

   <span data-ttu-id="82521-173">d.</span><span class="sxs-lookup"><span data-stu-id="82521-173">d.</span></span> <span data-ttu-id="82521-174">メイン クラスを選択して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-174">Choose your main class, and then click **OK**.</span></span>

      ![メイン クラスの指定][ART04d]

1. <span data-ttu-id="82521-176">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-176">Click **OK**.</span></span>

   ![[プロジェクト構造] ダイアログ ボックスを閉じる][ART05]

> [!NOTE]
> <span data-ttu-id="82521-178">IntelliJ のアーティファクトの作成について詳しくは、JetBrains の Web サイトの「[Configuring Artifacts (アーティファクトの構成)]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="82521-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on the JetBrains website.</span></span>
>

### <a name="build-the-artifact-for-deployment"></a><span data-ttu-id="82521-179">デプロイ用のアーティファクトのビルド</span><span class="sxs-lookup"><span data-stu-id="82521-179">Build the artifact for deployment</span></span>

1. <span data-ttu-id="82521-180">**[Build]\(ビルド\)** をクリックし、**[Artifacts]\(アーティファクト\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![[Build Artifacts]\(ビルド アーティファクト\) コマンド][BU04]

1. <span data-ttu-id="82521-182">**[Build Artifacts]\(ビルド アーティファクト\)** コンテキスト メニューが表示されたら、**[Build]\(ビルド\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-182">When the **Build Artifact** context menu appears, click **Build**.</span></span>

   ![[Build Artifacts]\(ビルド アーティファクト\) コンテキスト メニュー][BU05]

<span data-ttu-id="82521-184">IntelliJ でプロジェクト ツール ウィンドウに、Spring Boot アプリの完成したアーティファクトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="82521-184">IntelliJ should display the completed artifact for your Spring Boot app in the project tool window.</span></span>

   ![作成されたアーティファクト][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="82521-186">Docker コンテナーを使用して Web アプリを Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="82521-186">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="82521-187">Azure アカウントにサインインしていない場合は、「[Azure Toolkit for IntelliJ のサインイン手順][Azure Sign In for IntelliJ]」の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="82521-187">If you have not signed in to your Azure account, follow the steps in [Sign-in instructions for the Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="82521-188">プロジェクト エクスプローラー ツール ウィンドウで、プロジェクトを右クリックし、**[Azure]** > **[Publish as Docker Container]\(Docker コンテナーとして発行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-188">In the Project Explorer tool window, right-click the project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![[Publish as Docker Container]\(Docker コンテナーとして発行\) コマンド][PU01]

1. <span data-ttu-id="82521-190">**[Deploy Docker Container on Azure]\(Azure への Docker コンテナーのデプロイ\)** ダイアログ ボックスが表示され、既存の Docker ホストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="82521-190">When the **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="82521-191">既存のホストへのデプロイを選択する場合は、スキップして手順 4 に進みます。</span><span class="sxs-lookup"><span data-stu-id="82521-191">If you choose to deploy to an existing host, you can skip to step 4.</span></span> <span data-ttu-id="82521-192">そうでない場合は、次の手順に従ってホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="82521-192">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="82521-193">a.</span><span class="sxs-lookup"><span data-stu-id="82521-193">a.</span></span> <span data-ttu-id="82521-194">緑色のプラス ("**+**") 記号をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-194">Click the green plus (**+**) symbol.</span></span>

      ![新しい Docker ホストの追加][PU02]

   <span data-ttu-id="82521-196">b.</span><span class="sxs-lookup"><span data-stu-id="82521-196">b.</span></span> <span data-ttu-id="82521-197">**[Create Docker Host]\(Docker ホストの作成\)** ダイアログ ボックスが表示されたら、新しい Docker ホストに既定値をそのまま使用するか、カスタム設定を指定できます </span><span class="sxs-lookup"><span data-stu-id="82521-197">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="82521-198">(さまざまな設定について詳しくは、「[Azure Toolkit for IntelliJ を使用して Web アプリを Docker コンテナーとして発行する][Publish Container with Azure Toolkit]」をご覧ください)。使用する設定を指定したら、**[Next]\(次へ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-198">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Docker ホスト オプションの指定][PU03a]

   <span data-ttu-id="82521-200">c.</span><span class="sxs-lookup"><span data-stu-id="82521-200">c.</span></span> <span data-ttu-id="82521-201">Azure Key Vault の既存のログイン資格情報を使用することも、新しい Docker ログイン資格情報を入力することもできます。</span><span class="sxs-lookup"><span data-stu-id="82521-201">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="82521-202">オプションを指定したら、**[Finish]\(完了\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-202">Click **Finish** when you have specified your options.</span></span>

      ![Docker ホストの資格情報の指定][PU03b]

1. <span data-ttu-id="82521-204">Docker ホストを選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-204">Select your Docker host, and then click **Next**.</span></span>

   ![使用する Docker ホストの選択][PU04]

1. <span data-ttu-id="82521-206">**[Deploy Docker Container on Azure]\(Azure への Docker コンテナーのデプロイ\)** ダイアログ ボックスの最後のページで、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="82521-206">On the last page of the **Deploy Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="82521-207">a.</span><span class="sxs-lookup"><span data-stu-id="82521-207">a.</span></span> <span data-ttu-id="82521-208">Docker コンテナーをホストするコンテナーのカスタム名を指定するか、既定値をそのまま使用することができます。</span><span class="sxs-lookup"><span data-stu-id="82521-208">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="82521-209">b.</span><span class="sxs-lookup"><span data-stu-id="82521-209">b.</span></span> <span data-ttu-id="82521-210">"*[外部ポート]*:*[内部ポート]*" という構文で、Docker ホストの TCP ポートを入力します。</span><span class="sxs-lookup"><span data-stu-id="82521-210">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="82521-211">たとえば "**80:8080**" では、外部ポート "80" と既定の内部 Spring Boot ポート "8080" が指定されます。</span><span class="sxs-lookup"><span data-stu-id="82521-211">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="82521-212">内部ポートをカスタマイズした場合 (application.yml ファイルを編集するなどして)、Azure で正しいルーティングが実現するようポート番号を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82521-212">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="82521-213">c.</span><span class="sxs-lookup"><span data-stu-id="82521-213">c.</span></span> <span data-ttu-id="82521-214">これらのオプションを構成したら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82521-214">After you have configured these options, click **Finish**.</span></span>

   ![Azure への Docker コンテナーのデプロイ][PU05]

1. <span data-ttu-id="82521-216">Azure Toolkit による発行が完了したら、Azure Activity Log の状態が**発行済み**になります。</span><span class="sxs-lookup"><span data-stu-id="82521-216">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![正常にデプロイされた Docker ホスト][PU06]

## <a name="next-steps"></a><span data-ttu-id="82521-218">次のステップ</span><span class="sxs-lookup"><span data-stu-id="82521-218">Next steps</span></span>

<span data-ttu-id="82521-219">IntelliJ を使用して Spring Boot アプリを作成するための他の方法については、JetBrains Web サイトの [Spring Boot プロジェクトの作成](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html)に関する記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="82521-219">To learn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on the JetBrains website.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Configuring Artifacts (アーティファクトの構成)]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in AKS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
