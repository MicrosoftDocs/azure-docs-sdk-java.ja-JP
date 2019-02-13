---
title: Azure Toolkit for IntelliJ を使用して Linux コンテナーで実行されている Hello World Web アプリをクラウドでデプロイする
description: Linux コンテナーで基本的な Hello World Web アプリを実行し、Azure Toolkit for IntelliJ を使用してクラウドにデプロイします。
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/20/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: fdff8dc2bd7a29473314d5c0bc99b7bcda369156
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55648726"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="0ab40-103">Azure Toolkit for IntelliJ を使用して Hello World Web アプリをクラウドの Linux コンテナーにデプロイする</span><span class="sxs-lookup"><span data-stu-id="0ab40-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="0ab40-104">[Docker] コンテナーは、Web アプリケーションをデプロイするために広く使用されている方法です。</span><span class="sxs-lookup"><span data-stu-id="0ab40-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="0ab40-105">Docker コンテナーを使用すると、開発者は、すべてのプロジェクト ファイルと依存関係を、サーバーにデプロイするために 1 つのパッケージに統合できます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="0ab40-106">Azure Toolkit for IntelliJ には、Microsoft Azure にデプロイする機能が追加されているため、Java 開発者はプロセスを簡略化できます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="0ab40-107">この記事では、基本的な Hello World Web アプリを作成し、Azure Toolkit for IntelliJ を使用して Azure 用の Linux コンテナーに Web アプリを発行するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* <span data-ttu-id="0ab40-108">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="0ab40-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0ab40-109">このチュートリアルの手順を実行するには、TLS を使用せず、ポート 2375 でデーモンを公開する [Docker] を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ab40-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="0ab40-110">この設定は、Docker のインストール時、または、[Docker の設定] メニューから構成できます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![[Docker の設定] メニュー][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="0ab40-112">新しい Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="0ab40-112">Create a new web app project</span></span>

1. <span data-ttu-id="0ab40-113">「[Azure Toolkit for IntelliJ のサインイン手順](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions)」の記事に記載されている手順を使用して、IntelliJ を起動し Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-113">Start IntelliJ and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="0ab40-114">**[ファイル]** メニューの **[New]\(新規\)** をクリックし、**[プロジェクト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-114">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![新しいプロジェクトの作成][file-new-project]

1. <span data-ttu-id="0ab40-116">**[新しいプロジェクト]** ダイアログ ボックスで、 **[Maven]**、 **maven-archetype-webapp** の順に選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-116">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![Maven アーキタイプ Web アプリの選択][maven-archetype-webapp]
   
1. <span data-ttu-id="0ab40-118">Web アプリの **[GroupId]** と **[ArtifactId]** を指定し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-118">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![GroupId と ArtifactId の指定][groupid-and-artifactid]

1. <span data-ttu-id="0ab40-120">Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-120">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![Maven 設定の指定][maven-options]

1. <span data-ttu-id="0ab40-122">プロジェクト名と場所を指定し、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-122">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![プロジェクト名の指定][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="0ab40-124">Azure Container Registry をプライベート Docker レジストリとして使用するために作成する</span><span class="sxs-lookup"><span data-stu-id="0ab40-124">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="0ab40-125">Azure Portal を使用して Azure Container Registry を作成する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-125">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0ab40-126">Azure ポータルではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成][Create Docker Registry using Azure CLI]」の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="0ab40-126">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="0ab40-127">[Azure ポータル]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-127">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="0ab40-128">Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-128">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="0ab40-129">**[+ リソースの作成]** のメニュー アイコンをクリックし、**[コンテナー]**、**[コンテナー レジストリ]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-129">Click the menu icon for **+ Create a resource**, then click **Containers**, and then click **Container Registry**.</span></span>
   
   ![Azure Container Registry を新しく作成する][create-container-registry-01]

1. <span data-ttu-id="0ab40-131">**[コンテナー レジストリの作成]** ページが表示されたら、**[レジストリ名]** と **[リソース グループ]** を入力し、**[管理ユーザー]** に対して **[有効化]** を選択した後、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-131">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Azure Container Registry 設定を構成する][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="0ab40-133">Docker コンテナーに Web アプリを配置する</span><span class="sxs-lookup"><span data-stu-id="0ab40-133">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="0ab40-134">プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択し、**[Add Docker Support]\(Docker サポートの追加\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-134">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="0ab40-135">既定の構成の Docker ファイルが自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-135">This will automatically create a Docker file with a default configuration.</span></span>

   ![Docker サポートの追加][add-docker-support]

1. <span data-ttu-id="0ab40-137">Docker のサポートを追加したら、プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択して、**[Run on Web App for Containers]\(Web App for Containers で実行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-137">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Run on Web App for Containers**.</span></span>

   ![[Run on Web App for Containers]\(Web App for Containers で実行\)][run-on-web-app-for-containers]

1. <span data-ttu-id="0ab40-139">**[Run on Web App for Containers]\(Web App for Containers で実行\)** ダイアログ ボックスが表示されたら、必要な情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-139">When the **Run on Web App for Containers** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="0ab40-140">**[名前]**:Azure Toolkit に表示されるフレンドリ名を指定します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-140">**Name**: This specifies the friendly name which is displayed in the Azure Toolkit.</span></span> 

   * <span data-ttu-id="0ab40-141">**[コンテナー レジストリ]**:この記事の前のセクションで作成したコンテナー レジストリをドロップダウン メニューから選択します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-141">**Container Registry**: Choose the container registry from the drop-down menu that you created in the previous section of this article.</span></span> <span data-ttu-id="0ab40-142">**[サーバーの URL]**、**[ユーザー名]**、**[パスワード]** の各フィールドには、値が自動的に入力されます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-142">The fields for **Server URL**, **Username**, and **Password** will be automatically populated.</span></span>

   * <span data-ttu-id="0ab40-143">**[Image and tag]\(イメージとタグ\)**:コンテナー イメージ名を指定します。通常は、"*registry*.azurecr.io/*appname*: latest" の構文が使用されます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-143">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="0ab40-144">*registry* は、この記事の前のセクションで説明されているコンテナー レジストリを指します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-144">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="0ab40-145">*appname* は、Web アプリの名前です。</span><span class="sxs-lookup"><span data-stu-id="0ab40-145">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="0ab40-146">**[Use Existing Web App]\(既存の Web アプリを使用\)** または **[Create New Web App]\(新しい Web アプリの作成\)**:コンテナーを既存の Web アプリに展開するか、新しい Web アプリを作成するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-146">**Use Existing Web App** or **Create New Web App**: Specifies whether you will deploy your container to an existing web app or create a new web app.</span></span> <span data-ttu-id="0ab40-147">指定した**アプリ名**を使用して Web アプリの URL が作成されます (例: *wingtiptoys.azurewebsites.net*)。</span><span class="sxs-lookup"><span data-stu-id="0ab40-147">The **App name** that you specify will create the URL for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   * <span data-ttu-id="0ab40-148">**リソース グループ**:既存のリソース グループを使用するか、新しいリソース グループを作成するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-148">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="0ab40-149">**[App Service プラン]**:既存の App Service プランを使用するか、新しい App Service プランを作成するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="0ab40-149">**App Service Plan**: Specifies whether you will use an existing or create a new app service plan.</span></span> 

   ![[Run on Web App for Containers]\(Web App for Containers で実行\)][run-on-web-app-linux]

1. <span data-ttu-id="0ab40-151">上記の設定の構成が完了したら、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-151">When you have finished configuring the settings listed above, click **Run**.</span></span> <span data-ttu-id="0ab40-152">Web アプリが正常にデプロイされると、**[実行]** ウィンドウに状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-152">When your web app has been successfully deployed, the status will be displayed in the **Run** window.</span></span>

   ![正常にデプロイされた Web アプリ][successfully-deployed]

1. <span data-ttu-id="0ab40-154">Web アプリが発行されたら、以前に指定した Web アプリの URL (例: *wingtiptoys.azurewebsites.net*) を参照できます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-154">After your web app has been published, you can browse to the URL that specifed earlier for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   ![Web アプリの参照][browsing-to-web-app]

## <a name="optional-modify-your-web-app-publish-settings"></a><span data-ttu-id="0ab40-156">省略可能:Web アプリの発行設定を変更する</span><span class="sxs-lookup"><span data-stu-id="0ab40-156">Optional: Modify your web app publish settings</span></span>

1. <span data-ttu-id="0ab40-157">Web アプリを発行すると、使用した設定が既定の設定として保存されます。ツール バーの緑色の矢印アイコンをクリックすると、Azure でアプリケーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="0ab40-157">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="0ab40-158">設定を変更するには、Web アプリのドロップダウン メニューをクリックし、**[構成の編集]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-158">You can modify these settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![[構成の編集] メニュー][edit-configuration-menu]

1. <span data-ttu-id="0ab40-160">**[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0ab40-160">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![[構成の編集] ダイアログ ボックス][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="0ab40-162">次の手順</span><span class="sxs-lookup"><span data-stu-id="0ab40-162">Next steps</span></span>

<span data-ttu-id="0ab40-163">Docker の他のリソースについては、公式の [Docker の Web サイト]の [Docker] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0ab40-163">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure ポータル]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-intellij-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
