---
title: Azure Kubernetes Service で Spring Boot アプリを Kubernetes にデプロイする
description: このチュートリアルでは、Microsoft Azure の Kubernetes クラスターに Spring Boot アプリケーションをデプロイする方法について説明します。
services: container-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 87bbf46fe5b22c4a147d6010d3813334caa774fb
ms.sourcegitcommit: 1c1412ad5d8960975c3fc7fd3d1948152ef651ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57335415"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a><span data-ttu-id="5c7fa-103">Azure Kubernetes Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="5c7fa-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Kubernetes Service</span></span>

<span data-ttu-id="5c7fa-104">**[Kubernetes]** と **[Docker]** は、開発者が、コンテナーで実行されるアプリケーションのデプロイ、スケーリング、管理を自動化することを支援するオープン ソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-104">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications running in containers.</span></span>

<span data-ttu-id="5c7fa-105">このチュートリアルでは、この 2 つの一般的なオープンソース テクノロジを組み合わせて Spring Boot アプリケーションを開発し、Microsoft Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-105">This tutorial walks you through combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="5c7fa-106">具体的には、アプリケーション開発に *[Spring Boot]* を、コンテナーのデプロイに *[Kubernetes]* を、アプリケーションのホストとして [Azure Kubernetes Service (AKS)] をそれぞれ使用します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-106">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Kubernetes Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5c7fa-107">前提条件</span><span class="sxs-lookup"><span data-stu-id="5c7fa-107">Prerequisites</span></span>

* <span data-ttu-id="5c7fa-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="5c7fa-109">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="5c7fa-110">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="5c7fa-111">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="5c7fa-112">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="5c7fa-113">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-113">A [Git] client.</span></span>
* <span data-ttu-id="5c7fa-114">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-114">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5c7fa-115">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-115">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="5c7fa-116">Spring Boot on Docker Getting Started Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="5c7fa-116">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="5c7fa-117">次の手順で、Spring Boot Web アプリケーションをビルドしてローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-117">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="5c7fa-118">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成し、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-118">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="5c7fa-119">-- または --</span><span class="sxs-lookup"><span data-stu-id="5c7fa-119">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="5c7fa-120">[Docker での Spring Boot の使用開始] サンプル プロジェクトを、ディレクトリに複製します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-120">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="5c7fa-121">完成したプロジェクトにディレクトリを変更します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-121">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="5c7fa-122">Maven を使用してサンプル アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-122">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="5c7fa-123">Web アプリをテストするには、 `http://localhost:8080` を参照するか、次の `curl` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-123">Test the web app by browsing to `http://localhost:8080`, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="5c7fa-124">次のメッセージが表示されます。**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="5c7fa-124">You should see the following message displayed: **Hello Docker World**</span></span>

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="5c7fa-126">Azure CLI を使用して Azure Container Registry を作成する</span><span class="sxs-lookup"><span data-stu-id="5c7fa-126">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="5c7fa-127">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-127">Open a command prompt.</span></span>

1. <span data-ttu-id="5c7fa-128">Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-128">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="5c7fa-129">Azure サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-129">Choose your Azure Subscription:</span></span>
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. <span data-ttu-id="5c7fa-130">このチュートリアルで使用する Azure リソースのリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-130">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="5c7fa-131">リソース グループ内に、プライベートな Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-131">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="5c7fa-132">このチュートリアルでは、後の手順で、このレジストリに Docker イメージとしてサンプル アプリをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-132">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="5c7fa-133">`wingtiptoysregistry` を、レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="5c7fa-134">アプリをコンテナー レジストリにプッシュする</span><span class="sxs-lookup"><span data-stu-id="5c7fa-134">Push your app to the container registry</span></span>

1. <span data-ttu-id="5c7fa-135">Maven インストールの構成ディレクトリ (既定では ~/.m2/ または C:\Users\username\.m2) に移動し、*settings.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-135">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="5c7fa-136">Azure CLI からコンテナー レジストリのパスワードを取得します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-136">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="5c7fa-137">*settings.xml* ファイルの新しい `<server>` コレクションに Azure Container Registry の ID とパスワードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-137">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="5c7fa-138">`id` と `username` がレジストリの名前になります。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-138">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="5c7fa-139">前のコマンドで取得した `password` の値を (引用符なしで) 使用します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-139">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="5c7fa-140">Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-140">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="5c7fa-141">*pom.xml*ファイル内の `<properties>` コレクションを、Azure Container Registry のログイン サーバーの値で更新します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-141">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="5c7fa-142">*pom.xml*ファイル内の `<plugins>` コレクションを、`<plugin>` にログイン サーバーのアドレスと Azure Container Registry のレジストリ名が含まれるように更新します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-142">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <buildArgs>
            <JAR_FILE>target/${project.build.finalName}.jar</JAR_FILE>
         </buildArgs>
         <baseImage>java</baseImage>
         <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. <span data-ttu-id="5c7fa-143">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行して Docker コンテナーを作成し、そのイメージをレジストリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-143">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package dockerfile:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="5c7fa-144">Maven でイメージを Azure にプッシュすると、次のいずれかのようなエラー メッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-144">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="5c7fa-145">エラーが発生した場合は、Docker コマンド ラインから Azure にログインします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-145">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="5c7fa-146">その後、コンテナーをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-146">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="5c7fa-147">Azure CLI を使用して AKS で Kubernetes クラスターを作成する</span><span class="sxs-lookup"><span data-stu-id="5c7fa-147">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="5c7fa-148">Azure Kubernetes Service で Kubernetes クラスターを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-148">Create a Kubernetes cluster in Azure Kubernetes Service.</span></span> <span data-ttu-id="5c7fa-149">次のコマンドでは、*kubernetes* クラスターを *wingtiptoys-kubernetes* リソース グループに作成します。クラスター名として *wingtiptoys-akscluster* を、DNS プレフィックスとして *wingtiptoys-kubernetes* を使用します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-149">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-akscluster* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   <span data-ttu-id="5c7fa-150">このコマンドは、完了するまで時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-150">This command may take a while to complete.</span></span>

1. <span data-ttu-id="5c7fa-151">Azure Kubernetes Service (AKS) で Azure Container Registry (ACR) を使用する場合は、認証メカニズムを確立する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-151">When you're using Azure Container Registry (ACR) with Azure Kubernetes Service (AKS), an authentication mechanism needs to be established.</span></span> <span data-ttu-id="5c7fa-152">「[Azure Kubernetes Service から Azure Container Registry の認証を受ける]」に記載された手順に従って、ACR へのアクセス許可を AKS に付与します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-152">Follow the steps in [Authenticate with Azure Container Registry from Azure Kubernetes Service] to grant AKS access to ACR.</span></span>


1. <span data-ttu-id="5c7fa-153">Azure CLI を使用して `kubectl` をインストールします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-153">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="5c7fa-154">Kubernetes CLI は `/usr/local/bin` にデプロイされるため、Linux ユーザーはこのコマンドの前に `sudo` を付けなければならない場合があります。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-154">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az aks install-cli
   ```

1. <span data-ttu-id="5c7fa-155">クラスター構成情報をダウンロードして、Kubernetes Web インターフェイスと `kubectl` からクラスターを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-155">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="5c7fa-156">イメージを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="5c7fa-156">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="5c7fa-157">このチュートリアルでは、`kubectl` を使用してアプリをデプロイします。これにより、Kubernetes Web インターフェイスを通してデプロイを調べることができます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-157">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="5c7fa-158">Kubernetes Web インターフェイスを使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="5c7fa-158">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="5c7fa-159">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-159">Open a command prompt.</span></span>

1. <span data-ttu-id="5c7fa-160">Kubernetes クラスターの構成 Web サイトを既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-160">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. <span data-ttu-id="5c7fa-161">Kubernetes 構成 Web サイトがブラウザーで開いたら、**コンテナー化されたアプリをデプロイする**ためのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-161">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes 構成 Web サイト][KB01]

1. <span data-ttu-id="5c7fa-163">**[Resource Creation]\(リソースの作成)** ページが表示されたら、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-163">When the **Resource Creation** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="5c7fa-164">a.</span><span class="sxs-lookup"><span data-stu-id="5c7fa-164">a.</span></span> <span data-ttu-id="5c7fa-165">**[CREATE AN APP]\(アプリの作成)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-165">Select **CREATE AN APP**.</span></span>

   <span data-ttu-id="5c7fa-166">b.</span><span class="sxs-lookup"><span data-stu-id="5c7fa-166">b.</span></span> <span data-ttu-id="5c7fa-167">Spring Boot アプリケーション名を **[App name]\(アプリ名\)** に入力します (例: "*gs-spring-boot-docker*")。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-167">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="5c7fa-168">c.</span><span class="sxs-lookup"><span data-stu-id="5c7fa-168">c.</span></span> <span data-ttu-id="5c7fa-169">ログイン サーバーとコンテナー イメージを **[Container image]\(コンテナー イメージ\)** に入力します (例: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*")。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-169">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="5c7fa-170">d.[Tableau Server return URL]: Tableau Server ユーザーがアクセスする URL。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-170">d.</span></span> <span data-ttu-id="5c7fa-171">**[Service]\(サービス)** で **[External]\(外部)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-171">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="5c7fa-172">e.</span><span class="sxs-lookup"><span data-stu-id="5c7fa-172">e.</span></span> <span data-ttu-id="5c7fa-173">外部ポートと内部ポートを **[Port]\(ポート)** テキスト ボックスと **[Target port]\(ターゲット ポート\)** テキスト ボックスに指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-173">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes 構成 Web サイト][KB02]


1. <span data-ttu-id="5c7fa-175">**[Deploy]\(デプロイ)** をクリックしてコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-175">Click **Deploy** to deploy the container.</span></span>

   ![Kubernetes デプロイ][KB05]

1. <span data-ttu-id="5c7fa-177">アプリケーションがデプロイされると、**[Services]\(サービス)** の下に Spring Boot アプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-177">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes サービス][KB06]

1. <span data-ttu-id="5c7fa-179">\**[External endpoints]\(外部エンドポイント)\** のリンクをクリックすると、Spring Boot アプリケーションが Azure で実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-179">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes サービス][KB07]

   ![Azure でサンプル アプリを参照する][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="5c7fa-182">kubectl を使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="5c7fa-182">Deploy with kubectl</span></span>

1. <span data-ttu-id="5c7fa-183">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-183">Open a command prompt.</span></span>

1. <span data-ttu-id="5c7fa-184">`kubectl run` コマンドを使用して、Kubernetes クラスターのコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-184">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="5c7fa-185">Kubernetes でのアプリのサービス名と完全なイメージ名を指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-185">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="5c7fa-186">例: </span><span class="sxs-lookup"><span data-stu-id="5c7fa-186">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="5c7fa-187">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="5c7fa-187">In this command:</span></span>

   * <span data-ttu-id="5c7fa-188">コンテナー名 `gs-spring-boot-docker` は `run` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-188">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="5c7fa-189">`--image` パラメーターは、結合されたログイン サーバーとイメージの名前を `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest` として指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-189">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="5c7fa-190">`kubectl expose` コマンドを使用して、Kubernetes クラスターを外部に公開します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-190">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="5c7fa-191">サービス名、アプリにアクセスするために使用される公開 TCP ポート、およびアプリがリッスンする内部ターゲット ポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-191">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="5c7fa-192">例: </span><span class="sxs-lookup"><span data-stu-id="5c7fa-192">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="5c7fa-193">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="5c7fa-193">In this command:</span></span>

   * <span data-ttu-id="5c7fa-194">コンテナー名 `gs-spring-boot-docker` は `expose deployment` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-194">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="5c7fa-195">`--type` パラメーターは、クラスターでロード バランサーを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-195">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="5c7fa-196">`--port` パラメーターは、公開 TCP ポートとして 80 を指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-196">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="5c7fa-197">このポートでアプリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-197">You access the app on this port.</span></span>

   * <span data-ttu-id="5c7fa-198">`--target-port` パラメーターは、内部 TCP ポートとして 8080 を指定します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-198">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="5c7fa-199">ロード バランサーは、このポートでアプリに要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-199">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="5c7fa-200">クラスターにアプリがデプロイされたら、外部 IP アドレスを照会し、そのアドレスを Web ブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-200">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Azure でサンプル アプリを参照する][SB02]


## <a name="next-steps"></a><span data-ttu-id="5c7fa-202">次の手順</span><span class="sxs-lookup"><span data-stu-id="5c7fa-202">Next steps</span></span>

<span data-ttu-id="5c7fa-203">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-203">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5c7fa-204">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="5c7fa-204">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="5c7fa-205">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5c7fa-205">Additional Resources</span></span>

<span data-ttu-id="5c7fa-206">Azure での Spring Boot の使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-206">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="5c7fa-207">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="5c7fa-207">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="5c7fa-208">Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする</span><span class="sxs-lookup"><span data-stu-id="5c7fa-208">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

<span data-ttu-id="5c7fa-209">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-209">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="5c7fa-210">Visual Studio Code を使用して Java アプリケーションを Kubernetes にデプロイする方法の詳細については、[Visual Studio Code Java チュートリアル]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-210">For more information about deploying a Java application to Kubernetes with Visual Studio Code, see [Visual Studio Code Java Tutorials].</span></span>

<span data-ttu-id="5c7fa-211">Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-211">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="5c7fa-212">次のリンクは、Spring Boot アプリケーションの作成に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-212">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="5c7fa-213">単純な Spring Boot アプリケーションの作成の詳細については、Spring Initializr (https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-213">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="5c7fa-214">次のリンクは、Azure での Kubernetes の使用に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-214">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="5c7fa-215">Azure Kubernetes Service で Kubernetes クラスターを使用する</span><span class="sxs-lookup"><span data-stu-id="5c7fa-215">Get started with a Kubernetes cluster in Azure Kubernetes Service</span></span>](/azure/aks/intro-kubernetes)

<span data-ttu-id="5c7fa-216">Kubernetes コマンド ライン インターフェイスの使用方法の詳細については、**kubectl** ユーザー ガイド (<https://kubernetes.io/docs/user-guide/kubectl/>) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-216">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="5c7fa-217">Kubernetes web サイトには、プライベート レジストリでのイメージの使用に関するさまざまな記事があります。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-217">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="5c7fa-218">[Pods のサービス アカウントの構成]</span><span class="sxs-lookup"><span data-stu-id="5c7fa-218">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="5c7fa-219">[名前空間]</span><span class="sxs-lookup"><span data-stu-id="5c7fa-219">[Namespaces]</span></span>
* <span data-ttu-id="5c7fa-220">[プライベート レジストリからのイメージのプル]</span><span class="sxs-lookup"><span data-stu-id="5c7fa-220">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="5c7fa-221">Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c7fa-221">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Kubernetes Service (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Azure DevOps と Java の操作]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker での Spring Boot の使用開始]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Pods のサービス アカウントの構成]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[名前空間]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[プライベート レジストリからのイメージのプル]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[Azure Kubernetes Service から Azure Container Registry の認証を受ける]: /azure/container-registry/container-registry-auth-aks/
[Authenticate with Azure Container Registry from Azure Kubernetes Service]: /azure/container-registry/container-registry-auth-aks/
[Visual Studio Code Java チュートリアル]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Visual Studio Code Java Tutorials]: https://code.visualstudio.com/docs/java/java-kubernetes/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
