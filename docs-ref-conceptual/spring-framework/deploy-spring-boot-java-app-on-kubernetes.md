---
title: Azure Kubernetes Service で Spring Boot アプリを Kubernetes にデプロイする
description: このチュートリアルでは、Microsoft Azure の Kubernetes クラスターに Spring Boot アプリケーションをデプロイする方法について説明します。
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 07/05/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 546aa2dc18143ca173d72198ea8e6c30bda3c97f
ms.sourcegitcommit: e017de4677c5bedd6ef88c8c1b6da279dc973efe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2018
ms.locfileid: "45639725"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a><span data-ttu-id="4fb2a-103">Azure Kubernetes Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="4fb2a-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Kubernetes Service</span></span>

<span data-ttu-id="4fb2a-104">**[Kubernetes]** と **[Docker]** は、開発者が、コンテナーで実行されるアプリケーションのデプロイ、スケーリング、管理を自動化することを支援するオープン ソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-104">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications running in containers.</span></span>

<span data-ttu-id="4fb2a-105">このチュートリアルでは、この 2 つの一般的なオープンソース テクノロジを組み合わせて Spring Boot アプリケーションを開発し、Microsoft Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-105">This tutorial walks you through combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="4fb2a-106">具体的には、アプリケーション開発に *[Spring Boot]* を、コンテナーのデプロイに *[Kubernetes]* を、アプリケーションのホストとして [Azure Kubernetes Service (AKS)] をそれぞれ使用します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-106">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Kubernetes Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4fb2a-107">前提条件</span><span class="sxs-lookup"><span data-stu-id="4fb2a-107">Prerequisites</span></span>

* <span data-ttu-id="4fb2a-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="4fb2a-109">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="4fb2a-110">最新の [Java Developer Kit (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-110">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="4fb2a-111">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-111">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="4fb2a-112">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-112">A [Git] client.</span></span>
* <span data-ttu-id="4fb2a-113">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-113">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="4fb2a-114">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-114">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="4fb2a-115">Spring Boot on Docker Getting Started Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="4fb2a-115">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="4fb2a-116">次の手順で、Spring Boot Web アプリケーションをビルドしてローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-116">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="4fb2a-117">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成し、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-117">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="4fb2a-118">-- または --</span><span class="sxs-lookup"><span data-stu-id="4fb2a-118">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="4fb2a-119">[Docker での Spring Boot の使用開始] サンプル プロジェクトを、ディレクトリに複製します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-119">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="4fb2a-120">完成したプロジェクトにディレクトリを変更します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-120">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="4fb2a-121">Maven を使用してサンプル アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-121">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="4fb2a-122">http://localhost:8080 を参照するか、次の `curl` コマンドを使用して、Web アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-122">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="4fb2a-123">次のメッセージが表示されるはずです。**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="4fb2a-123">You should see the following message displayed: **Hello Docker World**</span></span>

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="4fb2a-125">Azure CLI を使用して Azure Container Registry を作成する</span><span class="sxs-lookup"><span data-stu-id="4fb2a-125">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="4fb2a-126">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-126">Open a command prompt.</span></span>

1. <span data-ttu-id="4fb2a-127">Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-127">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="4fb2a-128">Azure サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-128">Choose your Azure Subscription:</span></span>
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. <span data-ttu-id="4fb2a-129">このチュートリアルで使用する Azure リソースのリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-129">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="4fb2a-130">リソース グループ内に、プライベートな Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-130">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="4fb2a-131">このチュートリアルでは、後の手順で、このレジストリに Docker イメージとしてサンプル アプリをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-131">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="4fb2a-132">`wingtiptoysregistry` を、レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-132">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="4fb2a-133">アプリをコンテナー レジストリにプッシュする</span><span class="sxs-lookup"><span data-stu-id="4fb2a-133">Push your app to the container registry</span></span>

1. <span data-ttu-id="4fb2a-134">Maven インストールの構成ディレクトリ (既定では ~/.m2/ または C:\Users\username\.m2) に移動し、*settings.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-134">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="4fb2a-135">Azure CLI からコンテナー レジストリのパスワードを取得します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-135">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="4fb2a-136">*settings.xml* ファイルの新しい `<server>` コレクションに Azure Container Registry の ID とパスワードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-136">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="4fb2a-137">`id` と `username` がレジストリの名前になります。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-137">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="4fb2a-138">前のコマンドで取得した `password` の値を (引用符なしで) 使用します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-138">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="4fb2a-139">Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-139">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="4fb2a-140">*pom.xml*ファイル内の `<properties>` コレクションを、Azure Container Registry のログイン サーバーの値で更新します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-140">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="4fb2a-141">*pom.xml*ファイル内の `<plugins>` コレクションを、`<plugin>` にログイン サーバーのアドレスと Azure Container Registry のレジストリ名が含まれるように更新します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-141">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

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

1. <span data-ttu-id="4fb2a-142">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行して Docker コンテナーを作成し、そのイメージをレジストリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-142">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="4fb2a-143">Maven でイメージを Azure にプッシュすると、次のいずれかのようなエラー メッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-143">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="4fb2a-144">エラーが発生した場合は、Docker コマンド ラインから Azure にログインします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-144">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="4fb2a-145">その後、コンテナーをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-145">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="4fb2a-146">Azure CLI を使用して AKS で Kubernetes クラスターを作成する</span><span class="sxs-lookup"><span data-stu-id="4fb2a-146">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="4fb2a-147">Azure Kubernetes Service で Kubernetes クラスターを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-147">Create a Kubernetes cluster in Azure Kubernetes Service.</span></span> <span data-ttu-id="4fb2a-148">次のコマンドでは、*kubernetes* クラスターを *wingtiptoys-kubernetes* リソース グループに作成します。クラスター名として *wingtiptoys-akscluster* を、DNS プレフィックスとして *wingtiptoys-kubernetes* を使用します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-148">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-akscluster* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   <span data-ttu-id="4fb2a-149">このコマンドは、完了するまで時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-149">This command may take a while to complete.</span></span>

1. <span data-ttu-id="4fb2a-150">Azure Kubernetes Service (AKS) で Azure Container Registry (ACR) を使用する場合は、認証メカニズムを確立する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-150">When you're using Azure Container Registry (ACR) with Azure Kubernetes Service (AKS), an authentication mechanism needs to be established.</span></span> <span data-ttu-id="4fb2a-151">「[Azure Kubernetes Service から Azure Container Registry の認証を受ける]」に記載された手順に従って、ACR へのアクセス許可を AKS に付与します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-151">Follow the steps in [Authenticate with Azure Container Registry from Azure Kubernetes Service] to grant AKS access to ACR.</span></span>


1. <span data-ttu-id="4fb2a-152">Azure CLI を使用して `kubectl` をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-152">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="4fb2a-153">Kubernetes CLI は `/usr/local/bin` にデプロイされるため、Linux ユーザーはこのコマンドの前に `sudo` を付けなければならない場合があります。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-153">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az aks install-cli
   ```

1. <span data-ttu-id="4fb2a-154">クラスター構成情報をダウンロードして、Kubernetes Web インターフェイスと `kubectl` からクラスターを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-154">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="4fb2a-155">イメージを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="4fb2a-155">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="4fb2a-156">このチュートリアルでは、`kubectl` を使用してアプリをデプロイします。これにより、Kubernetes Web インターフェイスを通してデプロイを調べることができます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-156">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="4fb2a-157">Kubernetes Web インターフェイスを使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="4fb2a-157">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="4fb2a-158">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-158">Open a command prompt.</span></span>

1. <span data-ttu-id="4fb2a-159">Kubernetes クラスターの構成 Web サイトを既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-159">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. <span data-ttu-id="4fb2a-160">Kubernetes 構成 Web サイトがブラウザーで開いたら、**コンテナー化されたアプリをデプロイする**ためのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-160">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes 構成 Web サイト][KB01]

1. <span data-ttu-id="4fb2a-162">**[Resource Creation]\(リソースの作成)** ページが表示されたら、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-162">When the **Resource Creation** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="4fb2a-163">a.</span><span class="sxs-lookup"><span data-stu-id="4fb2a-163">a.</span></span> <span data-ttu-id="4fb2a-164">**[CREATE AN APP]\(アプリの作成)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-164">Select **CREATE AN APP**.</span></span>

   <span data-ttu-id="4fb2a-165">b.</span><span class="sxs-lookup"><span data-stu-id="4fb2a-165">b.</span></span> <span data-ttu-id="4fb2a-166">Spring Boot アプリケーション名を **[App name]\(アプリ名)** に入力します (例: "*gs-spring-boot-docker*")。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-166">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="4fb2a-167">c.</span><span class="sxs-lookup"><span data-stu-id="4fb2a-167">c.</span></span> <span data-ttu-id="4fb2a-168">ログイン サーバーとコンテナー イメージを **[Container image]\(コンテナー イメージ)** に入力します (例: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*")。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-168">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="4fb2a-169">d.</span><span class="sxs-lookup"><span data-stu-id="4fb2a-169">d.</span></span> <span data-ttu-id="4fb2a-170">**[Service]\(サービス)** で **[External]\(外部)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-170">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="4fb2a-171">e.</span><span class="sxs-lookup"><span data-stu-id="4fb2a-171">e.</span></span> <span data-ttu-id="4fb2a-172">外部ポートと内部ポートを **[Port]\(ポート)** テキスト ボックスと **[Target port]\(ターゲット ポート)** テキスト ボックスに指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-172">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes 構成 Web サイト][KB02]


1. <span data-ttu-id="4fb2a-174">**[Deploy]\(デプロイ)** をクリックしてコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-174">Click **Deploy** to deploy the container.</span></span>

   ![Kubernetes デプロイ][KB05]

1. <span data-ttu-id="4fb2a-176">アプリケーションがデプロイされると、**[Services]\(サービス)** の下に Spring Boot アプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-176">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes サービス][KB06]

1. <span data-ttu-id="4fb2a-178">**[External endpoints]\(外部エンドポイント)** のリンクをクリックすると、Spring Boot アプリケーションが Azure で実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-178">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes サービス][KB07]

   ![Azure でサンプル アプリを参照する][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="4fb2a-181">kubectl を使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="4fb2a-181">Deploy with kubectl</span></span>

1. <span data-ttu-id="4fb2a-182">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-182">Open a command prompt.</span></span>

1. <span data-ttu-id="4fb2a-183">`kubectl run` コマンドを使用して、Kubernetes クラスターのコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-183">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="4fb2a-184">Kubernetes でのアプリのサービス名と完全なイメージ名を指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-184">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="4fb2a-185">例: </span><span class="sxs-lookup"><span data-stu-id="4fb2a-185">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="4fb2a-186">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="4fb2a-186">In this command:</span></span>

   * <span data-ttu-id="4fb2a-187">コンテナー名 `gs-spring-boot-docker` は `run` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-187">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="4fb2a-188">`--image` パラメーターは、結合されたログイン サーバーとイメージの名前を `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest` として指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-188">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="4fb2a-189">`kubectl expose` コマンドを使用して、Kubernetes クラスターを外部に公開します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-189">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="4fb2a-190">サービス名、アプリにアクセスするために使用される公開 TCP ポート、およびアプリがリッスンする内部ターゲット ポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-190">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="4fb2a-191">例: </span><span class="sxs-lookup"><span data-stu-id="4fb2a-191">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="4fb2a-192">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="4fb2a-192">In this command:</span></span>

   * <span data-ttu-id="4fb2a-193">コンテナー名 `gs-spring-boot-docker` は `expose deployment` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-193">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="4fb2a-194">`--type` パラメーターは、クラスターでロード バランサーを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-194">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="4fb2a-195">`--port` パラメーターは、公開 TCP ポートとして 80 を指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-195">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="4fb2a-196">このポートでアプリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-196">You access the app on this port.</span></span>

   * <span data-ttu-id="4fb2a-197">`--target-port` パラメーターは、内部 TCP ポートとして 8080 を指定します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-197">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="4fb2a-198">ロード バランサーは、このポートでアプリに要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-198">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="4fb2a-199">クラスターにアプリがデプロイされたら、外部 IP アドレスを照会し、そのアドレスを Web ブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-199">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Azure でサンプル アプリを参照する][SB02]


## <a name="next-steps"></a><span data-ttu-id="4fb2a-201">次の手順</span><span class="sxs-lookup"><span data-stu-id="4fb2a-201">Next steps</span></span>

<span data-ttu-id="4fb2a-202">Azure での Spring Boot の使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-202">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="4fb2a-203">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="4fb2a-203">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="4fb2a-204">Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする</span><span class="sxs-lookup"><span data-stu-id="4fb2a-204">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

<span data-ttu-id="4fb2a-205">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-205">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="4fb2a-206"><!-- Newly added --> Visual Studio Code を使用して Java アプリケーションを Kubernetes にデプロイする方法の詳細については、[Visual Studio Code Java チュートリアル]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-206"><!-- Newly added --> For more information about deploying a Java application to Kubernetes with Visual Studio Code, see [Visual Studio Code Java Tutorials].</span></span>

<span data-ttu-id="4fb2a-207">Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-207">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="4fb2a-208">次のリンクは、Spring Boot アプリケーションの作成に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-208">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="4fb2a-209">単純な Spring Boot アプリケーションの作成の詳細については、Spring Initializr (https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-209">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="4fb2a-210">次のリンクは、Azure での Kubernetes の使用に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-210">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="4fb2a-211">Azure Kubernetes Service で Kubernetes クラスターを使用する</span><span class="sxs-lookup"><span data-stu-id="4fb2a-211">Get started with a Kubernetes cluster in Azure Kubernetes Service</span></span>](https://docs.microsoft.com/azure/aks/intro-kubernetes)

<span data-ttu-id="4fb2a-212">Kubernetes コマンド ライン インターフェイスの使用方法の詳細については、**kubectl** ユーザー ガイド (<https://kubernetes.io/docs/user-guide/kubectl/>) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-212">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="4fb2a-213">Kubernetes web サイトには、プライベート レジストリでのイメージの使用に関するさまざまな記事があります。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-213">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="4fb2a-214">[Pods のサービス アカウントの構成]</span><span class="sxs-lookup"><span data-stu-id="4fb2a-214">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="4fb2a-215">[名前空間]</span><span class="sxs-lookup"><span data-stu-id="4fb2a-215">[Namespaces]</span></span>
* <span data-ttu-id="4fb2a-216">[プライベート レジストリからのイメージのプル]</span><span class="sxs-lookup"><span data-stu-id="4fb2a-216">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="4fb2a-217">Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4fb2a-217">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Kubernetes Service (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
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

<!-- Newly added -->
[Azure Kubernetes Service から Azure Container Registry の認証を受ける]: https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks/
[Authenticate with Azure Container Registry from Azure Kubernetes Service]: https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks/
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
