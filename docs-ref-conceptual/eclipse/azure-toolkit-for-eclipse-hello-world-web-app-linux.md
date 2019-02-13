---
title: Azure Toolkit for Eclipse を使用して Linux コンテナーで実行されている Hello World Web アプリをクラウドにデプロイする
description: Linux コンテナーで基本的な Hello World Web アプリを実行し、Azure Toolkit for Eclipse を使用してクラウドにデプロイします。
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
ms.openlocfilehash: 799f21a282956f9a88aa35743157fc1292197569
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55649248"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="21506-103">Azure Toolkit for Eclipse を使用して Hello World Web アプリをクラウドの Linux コンテナーにデプロイする</span><span class="sxs-lookup"><span data-stu-id="21506-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="21506-104">[Docker] コンテナーは、Web アプリケーションをデプロイするために広く使用されている方法です。</span><span class="sxs-lookup"><span data-stu-id="21506-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="21506-105">Docker コンテナーを使用すると、開発者は、すべてのプロジェクト ファイルと依存関係を、サーバーにデプロイするために 1 つのパッケージに統合できます。</span><span class="sxs-lookup"><span data-stu-id="21506-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="21506-106">Azure Toolkit for Eclipse には、コンテナーを Microsoft Azure にデプロイするための機能が追加されているため、Java 開発者はこのプロセスを簡素化できます。</span><span class="sxs-lookup"><span data-stu-id="21506-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="21506-107">この記事では、基本的な Hello World Web アプリを作成し、Azure Toolkit for Eclipse を使用して Linux コンテナーの Web アプリを Azure に 発行するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="21506-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]
* <span data-ttu-id="21506-108">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="21506-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="21506-109">このチュートリアルの手順を実行するには、TLS を使用せず、ポート 2375 でデーモンを公開する [Docker] を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="21506-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="21506-110">この設定は、Docker のインストール時、または、[Docker の設定] メニューから構成できます。</span><span class="sxs-lookup"><span data-stu-id="21506-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![[Docker の設定] メニュー][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="21506-112">新しい Web アプリ プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="21506-112">Create a new web app project</span></span>

1. <span data-ttu-id="21506-113">Eclipse を起動し、[Azure Toolkit for Eclipse のサインイン手順](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)に関する記事の手順に従って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="21506-113">Start Eclipse and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="21506-114">**[File]\(ファイル\)** メニューをクリックし、**[New]\(新規作成\)**、**[Dynamic Web Project]\(動的 Web プロジェクト\)** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="21506-114">Click the **File** menu, then click **New**, and then click **Dynamic Web Project**.</span></span>
   
   ![新しいプロジェクトの作成][file-new-project]

1. <span data-ttu-id="21506-116">**[New Dynamic Web Project]\(新しい動的 Web プロジェクト\)** ダイアログ ボックスで、プロジェクト名と場所を指定し、**[Finish]\(完了\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="21506-116">In the **New Dynamic Web Project** dialog box, specify your project name and location, and then click **Finish**.</span></span>
   
   ![プロジェクト名の指定][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="21506-118">Azure Container Registry をプライベート Docker レジストリとして使用するために作成する</span><span class="sxs-lookup"><span data-stu-id="21506-118">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="21506-119">Azure Portal を使用して Azure Container Registry を作成する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="21506-119">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="21506-120">Azure ポータルではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成][Create Docker Registry using Azure CLI]」の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="21506-120">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="21506-121">[Azure ポータル]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="21506-121">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="21506-122">Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。</span><span class="sxs-lookup"><span data-stu-id="21506-122">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="21506-123">**[+ リソースの作成]** のメニュー アイコンをクリックし、**[コンテナー]**、**[コンテナー レジストリ]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="21506-123">Click the menu icon for **+ Create a resource**, then click **Containers**, and then click **Container Registry**.</span></span>
   
   ![Azure Container Registry を新しく作成する][create-container-registry-01]

1. <span data-ttu-id="21506-125">**[コンテナー レジストリの作成]** ページが表示されたら、**[レジストリ名]** と **[リソース グループ]** を入力し、**[管理ユーザー]** に対して **[有効化]** を選択した後、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="21506-125">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Azure Container Registry 設定を構成する][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="21506-127">Docker コンテナーに Web アプリを配置する</span><span class="sxs-lookup"><span data-stu-id="21506-127">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="21506-128">プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択し、**[Add Docker Support]\(Docker サポートの追加\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="21506-128">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="21506-129">既定の構成の Docker ファイルが自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="21506-129">This will automatically create a Docker file with a default configuration.</span></span>

   ![Docker サポートの追加][add-docker-support]

1. <span data-ttu-id="21506-131">Docker のサポートを追加したら、プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択して、**[Publish to Web App for Containers]\(Web App for Containers に発行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="21506-131">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Publish to Web App for Containers**.</span></span>

   ![[Publish to Web App for Containers]\(Web App for Containers に発行\)][run-on-web-app-for-containers]

1. <span data-ttu-id="21506-133">**[Run on Web App for Containers]\(Web App for Containers で実行\)** ダイアログ ボックスが表示されたら、必要な情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="21506-133">When the **Run on Web App for Containers** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="21506-134">**[Docker ファイル]**:Docker のサポートをプロジェクトに追加したときに作成された Docker ファイルのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="21506-134">**Docker File**: This specifies the path to the Docker file that was created when you added Docker support to your project.</span></span> 

   * <span data-ttu-id="21506-135">**[コンテナー レジストリ]**:この記事の前のセクションで作成したコンテナー レジストリをドロップダウン メニューから選択します。</span><span class="sxs-lookup"><span data-stu-id="21506-135">**Container Registry**: Choose the container registry from the drop-down menu that you created in the previous section of this article.</span></span> <span data-ttu-id="21506-136">**[サーバーの URL]**、**[ユーザー名]**、**[パスワード]** の各フィールドには、値が自動的に入力されます。</span><span class="sxs-lookup"><span data-stu-id="21506-136">The fields for **Server URL**, **Username**, and **Password** will be automatically populated.</span></span>

   * <span data-ttu-id="21506-137">**[Image and tag]\(イメージとタグ\)**:コンテナー イメージ名を指定します。通常は、"*registry*.azurecr.io/*appname*: latest" の構文が使用されます。</span><span class="sxs-lookup"><span data-stu-id="21506-137">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="21506-138">*registry* は、この記事の前のセクションで説明されているコンテナー レジストリを指します。</span><span class="sxs-lookup"><span data-stu-id="21506-138">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="21506-139">*appname* は、Web アプリの名前です。</span><span class="sxs-lookup"><span data-stu-id="21506-139">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="21506-140">**[Web App for Container]**:**[既存のものを使用]** または **[新規作成]** を選択して、コンテナーを既存の Web アプリに展開するか、新しい Web アプリを作成するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="21506-140">**Web App for Container**: Choose **Use Existing** or **Create New** to specify whether you will deploy your container to an existing web app or create a new web app.</span></span>  <span data-ttu-id="21506-141">指定した**アプリ名**を使用して Web アプリの URL が作成されます (例: *wingtiptoys.azurewebsites.net*)。</span><span class="sxs-lookup"><span data-stu-id="21506-141">The **App name** that you specify will create the URL for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   * <span data-ttu-id="21506-142">**リソース グループ**:既存のリソース グループを使用するか、新しいリソース グループを作成するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="21506-142">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="21506-143">**[App Service プラン]**:既存の App Service プランを使用するか、新しい App Service プランを作成するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="21506-143">**App Service Plan**: Specifies whether you will use an existing or create a new app service plan.</span></span> 

   ![[Run on Web App for Containers]\(Web App for Containers で実行\)][run-on-web-app-linux]

1. <span data-ttu-id="21506-145">上記の設定の構成が完了したら、**[OK]** をクリックして Web アプリを Azure に発行します。</span><span class="sxs-lookup"><span data-stu-id="21506-145">When you have finished configuring the settings listed above, click **OK** to publish your web app to Azure.</span></span>

1. <span data-ttu-id="21506-146">Web アプリが発行されたら、以前に指定した Web アプリの URL (例: *wingtiptoys.azurewebsites.net*) を参照できます。</span><span class="sxs-lookup"><span data-stu-id="21506-146">After your web app has been published, you can browse to the URL that specifed earlier for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   ![Web アプリの参照][browsing-to-web-app]

## <a name="next-steps"></a><span data-ttu-id="21506-148">次の手順</span><span class="sxs-lookup"><span data-stu-id="21506-148">Next steps</span></span>

<span data-ttu-id="21506-149">Docker の他のリソースについては、公式の [Docker の Web サイト]の [Docker] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="21506-149">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

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

[add-docker-support]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-eclipse-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/file-new-project.png
[project-name]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-linux.png
