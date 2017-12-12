---
title: "Azure Toolkit for IntelliJ を使用して Linux コンテナーで Hello World Web アプリを実行する"
description: "Linux コンテナーに基本的な Hello World Web アプリを作成し、IntelliJ の Azure Toolkit を使用して Azure に発行する方法を説明します。"
services: app-service\web
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
ms.openlocfilehash: 421241b12d8bd9027d4bef8564e1c1ab5a01993a
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/06/2017
---
# <a name="run-a-hello-world-web-app-in-a-linux-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="e02c4-103">Azure Toolkit for IntelliJ を使用して Linux コンテナーで Hello World Web アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="e02c4-103">Run a Hello World web app in a Linux container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="e02c4-104">[Docker] コンテナーは、Web アプリケーションをデプロイするために広く使用されている方法です。</span><span class="sxs-lookup"><span data-stu-id="e02c4-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="e02c4-105">Docker コンテナーを使用すると、開発者は、すべてのプロジェクト ファイルと依存関係を、サーバーにデプロイするために 1 つのパッケージに統合できます。</span><span class="sxs-lookup"><span data-stu-id="e02c4-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="e02c4-106">Azure Toolkit for IntelliJ には、Microsoft Azure にデプロイする機能が追加されているため、Java 開発者はプロセスを簡略化できます。</span><span class="sxs-lookup"><span data-stu-id="e02c4-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="e02c4-107">この記事では、基本的な Hello World Web アプリを作成し、Azure Toolkit for IntelliJ を使用して Azure 用の Linux コンテナーに Web アプリを発行するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* <span data-ttu-id="e02c4-108">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="e02c4-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="e02c4-109">このチュートリアルの手順を実行するには、TLS を使用せず、ポート 2375 でデーモンを公開する [Docker] を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e02c4-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="e02c4-110">この設定は、Docker のインストール時、または、[Docker の設定] メニューから構成できます。</span><span class="sxs-lookup"><span data-stu-id="e02c4-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![[Docker の設定] メニュー][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="e02c4-112">新しい Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="e02c4-112">Create a new web app project</span></span>

1. <span data-ttu-id="e02c4-113">「Azure Toolkit for IntelliJ のサインイン手順」の記事に記載されている手順を使用して、IntelliJ を起動し Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-113">Start IntelliJ and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="e02c4-114">**[ファイル]** メニューの **[New]\(新規\)** をクリックし、**[プロジェクト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-114">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![新しいプロジェクトの作成][file-new-project]

1. <span data-ttu-id="e02c4-116">**[新しいプロジェクト]** ダイアログ ボックスで、 **[Maven]**、 **maven-archetype-webapp** の順に選択し、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-116">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![Maven アーキタイプ Web アプリの選択][maven-archetype-webapp]
   
1. <span data-ttu-id="e02c4-118">Web アプリの **[GroupId]** と **[ArtifactId]** を指定し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-118">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![GroupId と ArtifactId の指定][groupid-and-artifactid]

1. <span data-ttu-id="e02c4-120">Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-120">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![Maven 設定の指定][maven-options]

1. <span data-ttu-id="e02c4-122">プロジェクト名と場所を指定し、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-122">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![プロジェクト名の指定][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="e02c4-124">Azure Container Registry をプライベート Docker レジストリとして使用するために作成する</span><span class="sxs-lookup"><span data-stu-id="e02c4-124">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="e02c4-125">Azure Portal を使用して Azure Container Registry を作成する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-125">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="e02c4-126">Azure ポータルではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成][Create Docker Registry using Azure CLI]」の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e02c4-126">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="e02c4-127">[Azure ポータル]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-127">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="e02c4-128">Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-128">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="e02c4-129">**[+ 新規]** のメニュー アイコン、**[コンテナー]**、**[Azure Container Registry]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-129">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Azure Container Registry を新しく作成する][AR01]

1. <span data-ttu-id="e02c4-131">Azure Container Registry テンプレートの情報ページが表示されたら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-131">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Azure Container Registry を新しく作成する][AR02]

1. <span data-ttu-id="e02c4-133">**[コンテナー レジストリの作成]** ページが表示されたら、**[レジストリ名]** と **[リソース グループ]** を入力し、**[管理ユーザー]** に対して **[有効化]** を選択した後、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-133">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Azure Container Registry 設定を構成する][AR03]

1. <span data-ttu-id="e02c4-135">コンテナー レジストリが作成されたら、Azure Portal でコンテナー レジストリに移動して、**[アクセス キー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-135">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="e02c4-136">次の手順で使用するため、ユーザー名とパスワードをメモします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-136">Take note of the username and password for the next steps.</span></span>

   ![Azure Container Registry のアクセス キー][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="e02c4-138">Docker コンテナーに Web アプリを配置する</span><span class="sxs-lookup"><span data-stu-id="e02c4-138">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="e02c4-139">プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択し、**[Add Docker Support]\(Docker サポートの追加\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-139">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="e02c4-140">既定の構成の Docker ファイルが自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="e02c4-140">This will automatically create a Docker file with a default configuration.</span></span>

   ![Docker サポートの追加][add-docker-support]

1. <span data-ttu-id="e02c4-142">Docker のサポートを追加したら、プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択し、**[Run on Web App (Linux)]\(Web App を (Linux 上で) 実行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-142">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Run on Web App (Linux)**.</span></span>

   ![[Run on Web App]\(Web アプリで実行\)][run-on-web-app-linux]

1. <span data-ttu-id="e02c4-144">**[Run on Web App]\(Web アプリで実行\)** ダイアログ ボックスが表示されたら、必要な情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-144">When the **Run on Web App (Linux)** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="e02c4-145">**[名前]**: Azure Toolkit に表示されるフレンドリ名を指定します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-145">**Name**: This specifies the friendly name which is displayed in the Azure Toolkit.</span></span> 

   * <span data-ttu-id="e02c4-146">**[サーバーの URL]**: この記事の前のセクションで説明されている、コンテナー レジストリの URL を指定します。通常、"*registry*.azurecr.io" の構文が使用されます。</span><span class="sxs-lookup"><span data-stu-id="e02c4-146">**Server URL**: This specifies the URL for your container registry from the previous section of this article; typically this will use the following syntax: "*registry*.azurecr.io".</span></span> 

   * <span data-ttu-id="e02c4-147">**[ユーザー名]** と **[パスワード]**: この記事の前のセクションで説明されている、コンテナー レジストリのアクセス キーを指定します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-147">**Username** and **Password**: Specifies the access keys for your container registry from the previous section of this article.</span></span> 

   * <span data-ttu-id="e02c4-148">**[Image and tag]\(イメージとタグ\)**: コンテナー イメージ名を指定します。通常、"*registry*.azurecr.io/*appname*: latest" の構文が使用されます。ここで、</span><span class="sxs-lookup"><span data-stu-id="e02c4-148">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="e02c4-149">*registry* は、この記事の前のセクションで説明されているコンテナー レジストリを指します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-149">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="e02c4-150">*appname* は、Web アプリの名前です。</span><span class="sxs-lookup"><span data-stu-id="e02c4-150">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="e02c4-151">**[Use Existing Web App]\(既存の Web アプリを使用\)** または**[Create New Web App]\(新しい Web アプリの作成\)**: 既存の Web アプリへコンテナーを展開するか、または新しい Web アプリを作成するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-151">**Use Existing Web App** or **Create New Web App**: Specifies whether you will deploy your container to an existing web app or create a new web app.</span></span> 

   * <span data-ttu-id="e02c4-152">**[リソース グループ]** で、新しいリソース グループを作成するか、既存のリソース グループを使用するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-152">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="e02c4-153">**[App Service プラン]** では、新しいリソース グループを作成するか、既存のリソース グループを使用するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="e02c4-153">**App Service Plan**: Specifies whether you willuse an existing or create a new app service plan.</span></span> 

1. <span data-ttu-id="e02c4-154">上記の設定の構成が完了したら、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-154">When you have finished configuring the settings listed above, click **Run**.</span></span>

   ![Create Web App][create-web-app]

1. <span data-ttu-id="e02c4-156">Web アプリを発行すると、使用した設定が既定の設定として保存されます。ツール バーの緑色の矢印アイコンをクリックすると、Azure でアプリケーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="e02c4-156">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="e02c4-157">設定を変更するには、Web アプリのドロップダウン メニューをクリックし、**[構成の編集]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-157">You can modify these settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[構成の編集] メニュー][edit-configuration-menu]

1. <span data-ttu-id="e02c4-159">**[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e02c4-159">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[構成の編集] ダイアログ ボックス][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="e02c4-161">次のステップ</span><span class="sxs-lookup"><span data-stu-id="e02c4-161">Next steps</span></span>

<span data-ttu-id="e02c4-162">Docker の他のリソースについては、公式の [Docker の Web サイト]の [Docker] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e02c4-162">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure ポータル]: https://portal.azure.com/
[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[AR01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR01.png
[AR02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR02.png
[AR03]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR03.png
[AR04]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR04.png

[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[create-web-app]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-web-app.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
