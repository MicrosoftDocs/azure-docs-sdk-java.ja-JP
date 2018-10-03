---
title: Azure DevOps を使用した MicroProfile アプリケーションの CI/CD
description: Azure DevOps を使用して、MicroProfile アプリケーションを Azure Web App for Containers インスタンスにデプロイするための CI/CD リリース サイクルを設定する方法について説明します。
services: Azure DevOps
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/14/2018
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: c2b6bf3370982d26d8d23fede370e0105a70b734
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506591"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a><span data-ttu-id="37442-103">Azure DevOps を使用した MicroProfile アプリケーションの CI/CD</span><span class="sxs-lookup"><span data-stu-id="37442-103">CI/CD for MicroProfile applications using Azure DevOps</span></span>

<span data-ttu-id="37442-104">このチュートリアルでは、Java EE 開発者が、Azure DevOps (公式には VSTS として知られています) を使用して、[MicroProfile](http://microprofile.io) アプリケーションを Azure Web App for Containers にデプロイするための CI/CD リリース サイクルを簡単に設定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="37442-104">This tutorial will show how Java EE developers can easily setup a CI/CD release cycle to deploy their [MicroProfile](http://microprofile.io) applications to an Azure Web App for Containers using Azure DevOps (formally known as VSTS).</span></span>  <span data-ttu-id="37442-105">この例では、[Payara Micro](https://www.payara.fish/payara_micro) を基本イメージとして使用する MicroProfile アプリケーションを使用します。</span><span class="sxs-lookup"><span data-stu-id="37442-105">In this example, we’ll be using a MicroProfile application that uses a [Payara Micro](https://www.payara.fish/payara_micro) as a base image.</span></span>   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
<span data-ttu-id="37442-106">Docker イメージをビルドし、コンテナー イメージを Azure Container Registry にプッシュして、Azure DevOps コンテナー化プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="37442-106">We will start the Azure DevOps containerize process by building a Docker image and pushing the container image to an Azure Container Register.</span></span>  <span data-ttu-id="37442-107">次に、Azure DevOps リリース パイプラインを使用して、コンテナー イメージを Web アプリに展開します。</span><span class="sxs-lookup"><span data-stu-id="37442-107">Then complete with a Azure DevOps release pipeline to deploy the container image to a Web App.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37442-108">前提条件</span><span class="sxs-lookup"><span data-stu-id="37442-108">Prerequisites</span></span>
- <span data-ttu-id="37442-109">[GitHub](https://github.com/Azure-Samples/microprofile-hello-azure) から Git URL をコピーして保存します。</span><span class="sxs-lookup"><span data-stu-id="37442-109">Copy and save the Git url from [Github](https://github.com/Azure-Samples/microprofile-hello-azure)</span></span>
- <span data-ttu-id="37442-110">[Azure DevOps](https://dev.azure.com) アカウントに登録またはログインします。</span><span class="sxs-lookup"><span data-stu-id="37442-110">Register or Log into your [Azure DevOps](https://dev.azure.com) account</span></span>
- <span data-ttu-id="37442-111">新しい [Azure DevOps プロジェクト](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav)を作成し、上記の Git URL を使用して**リポジトリをインポート**します。</span><span class="sxs-lookup"><span data-stu-id="37442-111">Create a new [Azure DevOps project](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) and use the above Git url to **import a repository**</span></span>
- <span data-ttu-id="37442-112">[Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR) を作成します。</span><span class="sxs-lookup"><span data-stu-id="37442-112">Create an [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span></span>
- <span data-ttu-id="37442-113">Azure Web App for Containers を作成します。</span><span class="sxs-lookup"><span data-stu-id="37442-113">Create an Azure Web App for Container</span></span>
> [!NOTE]
>
> <span data-ttu-id="37442-114">Web アプリ インスタンスをプロビジョニングするときに、コンテナーの設定で [クイック スタート] を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-114">Select "Quickstart" in the Container Settings when provisioning the Web App instance</span></span>


## <a name="create-a-build-definition"></a><span data-ttu-id="37442-115">ビルド定義を作成する</span><span class="sxs-lookup"><span data-stu-id="37442-115">Create a Build definition</span></span>

<span data-ttu-id="37442-116">Azure DevOps のビルド定義により、Java EE アプリケーションのソース アプリケーションでコミットが発生するたびに、ビルド内のすべてのタスクが自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="37442-116">The build definition in Azure DevOps automatically executes all the tasks in the build each time there’s a commit in Java EE application source application.</span></span>  <span data-ttu-id="37442-117">この例では、Azure DevOps で Maven を使用して Java MicroProfile プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="37442-117">In this example, Azure DevOps will use Maven to build the Java MicroProfile project.</span></span>

1. <span data-ttu-id="37442-118">Azure DevOps プロジェクト ページの上部にある [Build and Release]\(ビルドとリリース\) タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-118">Click on the "Build and Release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="37442-119">次に、**[ビルド]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-119">Then, select the **Builds** link</span></span> 

<img src="media/VSTS/Buid-and-Release1.png">

2. <span data-ttu-id="37442-120">**[新しいパイプライン]** をクリックし、**[続行]** をクリックしてビルド タスクの定義を開始します。</span><span class="sxs-lookup"><span data-stu-id="37442-120">Click on the **New Pipeline** button, and then **Continue** to start defining your build tasks</span></span>
3. <span data-ttu-id="37442-121">テンプレートの一覧から [Maven] を選択し、**[適用]** をクリックして Java プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="37442-121">Select "Maven" from the list of templates, then click on the **Apply** button to build your Java project</span></span>
4. <span data-ttu-id="37442-122">[エージェント プール] フィールドのドロップダウン メニューで、**[Hosted Linux Preview]\(ホストされている Linux プレビュー\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-122">Use the drop-down menu for the Agent pool field to select **Hosted Linux Preview** option.</span></span>
> [!NOTE]
>
> <span data-ttu-id="37442-123">これにより、使用するビルド サーバーが Azure DevOps に通知されます。</span><span class="sxs-lookup"><span data-stu-id="37442-123">This informs Azure DevOps which build server to use.</span></span>  <span data-ttu-id="37442-124">カスタマイズされたプライベート ビルド サーバーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="37442-124">You can use your private customized build server</span></span>

5. <span data-ttu-id="37442-125">継続的インテグレーション用にビルドを構成するには、**[トリガー]** タブを選択し、**[継続的インテグレーションを有効にする]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="37442-125">To configure your build for continuous integration, select the **Triggers** tab and check the **Enable continuous integration** checkbox.</span></span>  

<img src="media/VSTS/Build-Triggers2.png"> 
 
6. <span data-ttu-id="37442-126">**[タスク]** タブを選択して、ビルド パイプラインのメイン ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="37442-126">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
7. <span data-ttu-id="37442-127">**[保存してキューに登録]** ドロップダウン メニューで、**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-127">Use the **Save & queue** drop-down menu to select the **Save** option</span></span>
 

## <a name="create-a-docker-build-image"></a><span data-ttu-id="37442-128">Docker ビルド イメージを作成する</span><span class="sxs-lookup"><span data-stu-id="37442-128">Create a Docker Build Image</span></span>

<span data-ttu-id="37442-129">このタスクでは、Azure DevOps で Payara Micro の基本イメージを指定した Dockerfile を使用して Docker イメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="37442-129">In this task, Azure DevOps uses a Dockerfile with a base image from Payara Micro to create a Docker image.</span></span>  

1. <span data-ttu-id="37442-130">**[タスク]** タブを選択して、ビルド パイプラインのメイン ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="37442-130">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
2. <span data-ttu-id="37442-131">**[+]** アイコンをクリックして、ビルド定義に新しいタスクを追加します。</span><span class="sxs-lookup"><span data-stu-id="37442-131">Click on the **+** icon to add new task to the build definition</span></span>
 
<img src="media/VSTS/Tasks-add4.png">
 
3. <span data-ttu-id="37442-132">テンプレートの一覧から [Docker] を選択し、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-132">Select "Docker" from the list of templates, then click on the **Add** button</span></span>
4. <span data-ttu-id="37442-133">**[表示名]** フィールドにわかりやすい名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="37442-133">Enter a description name for the **Display name** field</span></span>
5. <span data-ttu-id="37442-134">**[コンテナー レジストリの種類]** のドロップダウン メニューで **[Azure Container Registry]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="37442-134">Verify that **Azure Container Registry** is selected in the drop-down menu for **Container registry type**.</span></span>
> [!NOTE]
>
>  <span data-ttu-id="37442-135">Docker Hub または別のレジストリを使用する場合は、代わりに [Container Registry] を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-135">If you are using Docker Hub or another registry, select "Container Registry" instead.</span></span>  <span data-ttu-id="37442-136">次に、[+ 新規] をクリックして、資格情報と接続情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="37442-136">Then click on the "+ New" button to provide the credentials and connection information for it.</span></span> <span data-ttu-id="37442-137">その後、[コマンド] セクションに進んで続行します。</span><span class="sxs-lookup"><span data-stu-id="37442-137">Then skip to the Commands section to continue.</span></span>

6. <span data-ttu-id="37442-138">**[Azure サブスクリプション]** ドロップダウン メニューで、Azure サブスクリプション ID を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-138">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>  <span data-ttu-id="37442-139">次に、**[承認]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-139">Then click on the **Authorize** button</span></span>
7. <span data-ttu-id="37442-140">**[Azure Container Registry]** ドロップダウン メニューで、Azure で作成したレジストリ名を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-140">In the **Azure container registry** drop-down menu, select registry name you created in Azure.</span></span>
8. <span data-ttu-id="37442-141">**[コマンド]** ドロップダウン メニューで **[build]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="37442-141">Verify that **build** option is selected in the **Command** drop-down menu.</span></span>
9. <span data-ttu-id="37442-142">**Dockerfile** については、テキスト ボックスの横のパス ナビゲーション アイコンをクリックして、GitHub プロジェクトから Dockerfile を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-142">For the **Dockerfile**, click on the path navigation icon next to the textbox to select the Dockerfile from the github project.</span></span>  <span data-ttu-id="37442-143">次に、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-143">Then click the **OK** button.</span></span>

<img src="media/VSTS/Dockerfile5.png">

10. <span data-ttu-id="37442-144">**[イメージ名]** で、**[最終のタグを含める]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="37442-144">Under the **Image name**, check the **Include latest tag** checkbox.</span></span> 
11. <span data-ttu-id="37442-145">**[保存してキューに登録]** ドロップダウン メニューで、**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-145">Use the **Save & queue** drop-down menu to select the **Save** option.</span></span>

## <a name="push-docker-image-to-acr"></a><span data-ttu-id="37442-146">Docker イメージを ACR にプッシュする</span><span class="sxs-lookup"><span data-stu-id="37442-146">Push Docker Image to ACR</span></span>

<span data-ttu-id="37442-147">このタスクでは、Azure DevOps で Docker イメージを Azure Container Registry にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="37442-147">In this task, Azure DevOps will push the docker image to your Azure Container Registry.</span></span>  <span data-ttu-id="37442-148">これは、MicroProfile API アプリケーションを、コンテナー化された Java Web アプリとして実行するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="37442-148">This will be used to run the MicroProfile API application as a containerized Java web app.</span></span>

1. <span data-ttu-id="37442-149">Azure DevOps で Docker を使用しているので、前述の「**Docker ビルド イメージを作成する**」の手順 1. から 7. を繰り返して、新しい Docker テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="37442-149">Since we are using Docker in Azure DevOps, create a new Docker template by repeating steps 1 - 7 above in the **Create a Docker Build Image** section.</span></span>
2. <span data-ttu-id="37442-150">**[コマンド]** ドロップダウン メニューで **[push]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-150">Select **push** in the **Command** drop-down menu.</span></span>
3. <span data-ttu-id="37442-151">**[保存してキューに登録]** タブをクリックし、**[保存してキューに登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-151">Click on the **Save & queue** tab, then select **Save & queue** option.</span></span>
4. <span data-ttu-id="37442-152">ポップアップ ウィンドウのエージェント プールに、**[Hosted Linux Preview]\(ホストされている Linux プレビュー\)** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="37442-152">Verify that the **Hosted Linux Preview** is select for the Agent pool on the pop-up window.</span></span>  <span data-ttu-id="37442-153">次に、**[保存してキューに登録]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-153">Then click on the **Save & queue** button.</span></span>
5. <span data-ttu-id="37442-154">ビルド番号をクリックして、Java プロジェクトのビルド パイプラインが正常に完了したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="37442-154">Click on the build number to verify that the build pipeline for the Java project completed successfully.</span></span>

<img src="media/VSTS/Build-Number6.png">
 

## <a name="create-a-release-definition-for-a-java-app"></a><span data-ttu-id="37442-155">Java アプリのリリース定義を作成する</span><span class="sxs-lookup"><span data-stu-id="37442-155">Create a release definition for a Java app</span></span>

<span data-ttu-id="37442-156">Azure DevOps のリリース パイプラインでは、ビルド プロセスが正常に完了するとすぐに、Azure などのターゲット環境へのビルド成果物の配置が自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="37442-156">The release pipeline in Azure DevOps automatically triggers the deployment of build artifacts to a target environment like Azure as soon as the Build process completes successfully.</span></span>   <span data-ttu-id="37442-157">開発、テスト、ステージング、運用の各環境用のリリース パイプラインを作成できます。</span><span class="sxs-lookup"><span data-stu-id="37442-157">The release pipeline can be created for dev, test, staging or production environments.</span></span>

1. <span data-ttu-id="37442-158">Azure DevOps プロジェクト ページの上部にある [Build and Release]\(ビルドとリリース\) タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-158">Click on the "Build and release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="37442-159">次に、**[リリース]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-159">Then, select the **Releases** link.</span></span>

<img src="media/VSTS/Release-new-pipeline7.png">
 
2. <span data-ttu-id="37442-160">[新しいパイプライン] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-160">Click on the "New pipeline\*\* button</span></span>
3. <span data-ttu-id="37442-161">テンプレートの一覧で **[Java アプリを Azure App Service に配置]** を選択し、**[適用]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-161">Select the **Deploy a Java app to Azure App Service** in the list of templates, then click on the **Apply** button.</span></span>

<img src="media/VSTS/deploy-java-template8.png">
 
4. <span data-ttu-id="37442-162">**ステージ名** (例: 開発、テスト、ステージング、運用) を設定します。</span><span class="sxs-lookup"><span data-stu-id="37442-162">Set a **Stage name** (e.g Dev, Test, Staging or Production).</span></span>  <span data-ttu-id="37442-163">次に、**[X]** ボタンをクリックしてポップアップ ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="37442-163">Then click on the **X** button to close the pop-up window</span></span>
5. <span data-ttu-id="37442-164">[成果物] セクションの **[+ 追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-164">Click on the **+ Add** button in the Artifacts section.</span></span>  <span data-ttu-id="37442-165">これにより、ビルド定義からこのリリース定義に成果物がリンクされます。</span><span class="sxs-lookup"><span data-stu-id="37442-165">This will link artifacts from the build definition to this release definition.</span></span>  
6. <span data-ttu-id="37442-166">**[Source (build pipeline)]\(ソース (ビルド パイプライン)\)** のドロップダウン メニューでビルド定義を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-166">Use the drop-down menu for the **Source (build pipeline)** to select your build definition.</span></span> <span data-ttu-id="37442-167">次に、**[追加]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="37442-167">Then click the **Add** button to continue.</span></span>

<img src="media/VSTS/add-artifact9.png">
 
7. <span data-ttu-id="37442-168">パイプラインの **[タスク]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-168">Click on the **Tasks** tab on the pipeline.</span></span>  <span data-ttu-id="37442-169">次に、ステージ名を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-169">Then, select your stage name.</span></span>
 
<img src="media/VSTS/release-stage10.png">

8. <span data-ttu-id="37442-170">**[Azure サブスクリプション]** ドロップダウン メニューで、Azure サブスクリプション ID を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-170">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>
9. <span data-ttu-id="37442-171">**[アプリの種類]** ドロップダウン メニューで、**[Linux アプリ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-171">Select **Linux App** from the **App type** drop-down menu</span></span>
10. <span data-ttu-id="37442-172">**[App Service の名前]** ドロップダウン メニューから、上記で作成した Web App for Containers インスタンスの名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-172">Select the name of the Web App for Container instance you created above in the **App service name** drop-down menu</span></span>
11. <span data-ttu-id="37442-173">**[Registry or Namespaces]\(レジストリまたは名前空間\)** フィールドに、Azure コンテナー レジストリの名前を入力します </span><span class="sxs-lookup"><span data-stu-id="37442-173">Enter the name of your azure container registry in the **Registry or Namespaces** field.</span></span>  <span data-ttu-id="37442-174">(例: **myregistry.azure.io**)。</span><span class="sxs-lookup"><span data-stu-id="37442-174">E.g **myregistry.azure.io**</span></span>
12. <span data-ttu-id="37442-175">**[リポジトリ]** フィールドにレジストリ名を入力します。</span><span class="sxs-lookup"><span data-stu-id="37442-175">Enter the registry name in the **Repository** field</span></span>
13. <span data-ttu-id="37442-176">**[Deploy Azure App Service]\(Azure App Service の配置\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-176">Click on **Deploy Azure App Service**.</span></span>  <span data-ttu-id="37442-177">**[タグ]** テキスト ボックスに、コンテナー イメージのタグを入力します。</span><span class="sxs-lookup"><span data-stu-id="37442-177">Enter the tag for the container image in the **Tag** textbox</span></span> 
14. <span data-ttu-id="37442-178">**[エージェントで実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-178">Click on **Run on agent**.</span></span>  <span data-ttu-id="37442-179">エージェント プールのドロップダウン メニューで、**[Hosted Linux Preview]\(ホストされている Linux プレビュー\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-179">Select **Hosted Linux Preview** in the Agent pool drop-down menu</span></span> 

## <a name="setup-environment-variables"></a><span data-ttu-id="37442-180">環境変数を設定する</span><span class="sxs-lookup"><span data-stu-id="37442-180">Setup Environment Variables</span></span>

1. <span data-ttu-id="37442-181">**[変数]** タブをクリックします。次に、**[+ 追加]** をクリックして環境変数を定義します。</span><span class="sxs-lookup"><span data-stu-id="37442-181">Click on the **Variables** tab.  Then click on the **+ Add** button to define your environment variables</span></span>
2. <span data-ttu-id="37442-182">コンテナー レジストリの URL、ユーザー名、パスワードの変数名と値を追加します。</span><span class="sxs-lookup"><span data-stu-id="37442-182">Add the variable name and values for your container registry url, username and password.</span></span>   <span data-ttu-id="37442-183">セキュリティを確保するために、ロック アイコンをクリックして、パスワード値を非表示にしておきます。</span><span class="sxs-lookup"><span data-stu-id="37442-183">For security, click on the lock icon to keep the password value hidden.</span></span>

<span data-ttu-id="37442-184">例: </span><span class="sxs-lookup"><span data-stu-id="37442-184">For example:</span></span>
- <span data-ttu-id="37442-185">registry.password</span><span class="sxs-lookup"><span data-stu-id="37442-185">registry.password</span></span>
- <span data-ttu-id="37442-186">registry.url</span><span class="sxs-lookup"><span data-stu-id="37442-186">registry.url</span></span>
- <span data-ttu-id="37442-187">registry.username</span><span class="sxs-lookup"><span data-stu-id="37442-187">registry.username</span></span>

<img src="media/VSTS/environment-variables12.png">

3. <span data-ttu-id="37442-188">**[タスク]** タブをクリックして、リリース パイプライン定義のメイン ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="37442-188">Click on the **Tasks** tab to return to the main release pipeline definition page</span></span>
4. <span data-ttu-id="37442-189">**[Deploy Azure App Service]\(Azure App Service の配置\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-189">Click on **Deploy Azure App Service**.</span></span> 
5. <span data-ttu-id="37442-190">**[Application and Configuration Settings]\(アプリケーションと構成の設定\)** セクションを展開し、**[アプリケーション設定]** フィールドのナビゲーション パスをクリックして、配置時にコンテナー レジストリに接続するための環境変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="37442-190">Expand the **Application and Configuration Settings** section, then click on the navigation path for the **App Settings** field to add environments variable to connect to the container registry during deployment.</span></span>
6. <span data-ttu-id="37442-191">**[+ 追加]** をクリックして、次のアプリ設定を定義し、環境変数を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="37442-191">Click on the \*\* + Add\*\* button to define the following app settings and assign the environment variables</span></span>
- <span data-ttu-id="37442-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span><span class="sxs-lookup"><span data-stu-id="37442-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span></span>
- <span data-ttu-id="37442-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span><span class="sxs-lookup"><span data-stu-id="37442-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span></span>
- <span data-ttu-id="37442-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span><span class="sxs-lookup"><span data-stu-id="37442-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span></span>

<img src="media/VSTS/environment-variables14.png">
 
7. <span data-ttu-id="37442-195">**[OK]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="37442-195">Click on the **OK** button to continue</span></span>

## <a name="setup-continious-deployment--deploy-java-application"></a><span data-ttu-id="37442-196">継続的配置を設定し、Java アプリケーションを配置する</span><span class="sxs-lookup"><span data-stu-id="37442-196">Setup Continious Deployment & Deploy Java Application</span></span>

1. <span data-ttu-id="37442-197">継続的配置を有効にするには、**[パイプライン]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-197">To enable continuous deployment, click the **Pipelines** tab</span></span>
2. <span data-ttu-id="37442-198">[成果物] セクションで稲妻アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-198">In the Artifacts section, click on the lightening icon.</span></span>  <span data-ttu-id="37442-199">次に、**[継続的配置トリガー]** を [有効] に設定します。</span><span class="sxs-lookup"><span data-stu-id="37442-199">Then set the **Continuous deployment trigger** to Enabled.</span></span>

<img src="media/VSTS/release-enable-CD.png">
 
3. <span data-ttu-id="37442-200">**[保存]** をクリックし、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-200">Click on the **Save** button, then the **OK** button</span></span> 
4. <span data-ttu-id="37442-201">**[+ リリース]** ドロップダウン メニューをクリックし、**[Create a release]\(リリースの作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="37442-201">Click on the **+ Release** drop-down menu, then select **Create a release** link</span></span>
5. <span data-ttu-id="37442-202">**[Stages for a trigger change from automated to manual]\(トリガーを自動から手動に変更するステージ\)** ドロップダウン メニューで、目的のステージ名のチェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="37442-202">Use the **Stages for a trigger change from automated to manual** drop-down menu to select the checkbox for your stage name</span></span>
6. <span data-ttu-id="37442-203">**[作成]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="37442-203">Click the **Create** button to continue</span></span>
7. <span data-ttu-id="37442-204">リリース番号をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-204">Click on the release number.</span></span>  <span data-ttu-id="37442-205">次に、ステージ名をポイントし、**[Deploy]\(配置\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37442-205">Then hover your mouse cursor over the stage name and click on the **Deploy** button</span></span>
8. <span data-ttu-id="37442-206">ポップアップ ウィンドウの **[Deploy]\(配置\)** をクリックして、Azure へのデプロイ プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="37442-206">The click on the **Deploy** button on the pop-up window to start the deployment process to Azure</span></span>


## <a name="test-the-java-web-application"></a><span data-ttu-id="37442-207">Java Web アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="37442-207">Test the Java Web Application</span></span>
1. <span data-ttu-id="37442-208">Web ブラウザーで Web アプリの URL を実行します。</span><span class="sxs-lookup"><span data-stu-id="37442-208">Run the web app url in web browser:</span></span>  
<span data-ttu-id="37442-209">https://{your-app-service-name}.azurewebsites.net/api/hello</span><span class="sxs-lookup"><span data-stu-id="37442-209">https://{your-app-service-name}.azurewebsites.net/api/hello</span></span>

 
<img src="media/VSTS/web-app16.png">

2. <span data-ttu-id="37442-210">Web ページに "**Hello Azure!**" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="37442-210">The web page should say **Hello Azure!**</span></span>
 
<img src="media/VSTS/web-api17.png">
