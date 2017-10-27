---
title: "Azure Container Service で Spring Boot アプリケーションを Kubernetes にデプロイする"
description: "このチュートリアルでは、Microsoft Azure の Kubernetes クラスターに Spring Boot アプリケーションをデプロイする方法について説明します。"
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Kubernetes
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 10/11/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 44c20e9084d53fa366137fc191726aaa4be177f2
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a><span data-ttu-id="bd690-104">Azure Container Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="bd690-104">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>

<span data-ttu-id="bd690-105">**[Spring Framework]** は、Java 開発者による Web、モバイル、および API アプリケーションの作成を支援する一般的なオープンソース フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="bd690-105">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="bd690-106">このチュートリアルでは、[Spring Boot] (Spring の使用をすばやく開始するための規約駆動型手法) を使用して作成されたサンプル アプリを使用します。</span><span class="sxs-lookup"><span data-stu-id="bd690-106">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="bd690-107">**[Kubernetes]** と **[Docker]** は、開発者が、コンテナーで実行されるアプリケーションのデプロイ、スケーリング、および管理を自動化することを支援するオープン ソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="bd690-107">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="bd690-108">このチュートリアルでは、これら 2 つの一般的なオープンソース テクノロジを組み合わせて Spring Boot アプリケーションを開発し、Microsoft Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bd690-108">This tutorial walks you though combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="bd690-109">具体的には、アプリケーション開発で *[Spring Boot]* を、コンテナーのデプロイで *[Kubernetes]* を、アプリケーションのホストとして [Azure Container Service (ACS)] を使用します。</span><span class="sxs-lookup"><span data-stu-id="bd690-109">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Container Service (ACS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bd690-110">前提条件</span><span class="sxs-lookup"><span data-stu-id="bd690-110">Prerequisites</span></span>

* <span data-ttu-id="bd690-111">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="bd690-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="bd690-112">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="bd690-112">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="bd690-113">最新の [Java Developer Kit (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="bd690-113">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="bd690-114">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="bd690-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="bd690-115">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="bd690-115">A [Git] client.</span></span>
* <span data-ttu-id="bd690-116">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="bd690-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="bd690-117">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bd690-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="bd690-118">Spring Boot on Docker Getting Started Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="bd690-118">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="bd690-119">次の手順で、Spring Boot Web アプリケーションをビルドしてローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="bd690-119">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="bd690-120">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成し、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bd690-120">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="bd690-121">-- または --</span><span class="sxs-lookup"><span data-stu-id="bd690-121">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="bd690-122">[Spring Boot on Docker Getting Started] サンプル プロジェクトを、ディレクトリに複製します。</span><span class="sxs-lookup"><span data-stu-id="bd690-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="bd690-123">完成したプロジェクトにディレクトリを変更します。</span><span class="sxs-lookup"><span data-stu-id="bd690-123">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="bd690-124">Maven を使用してサンプル アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="bd690-124">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="bd690-125">http://localhost:8080 を参照するか次の `curl` コマンドを使用して、Web アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="bd690-125">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="bd690-126">次のメッセージが表示されるはずです。**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="bd690-126">You should see the following message displayed: **Hello Docker World**</span></span>

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="bd690-128">Azure CLI を使用して Azure Container Registry を作成する</span><span class="sxs-lookup"><span data-stu-id="bd690-128">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="bd690-129">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="bd690-129">Open a command prompt.</span></span>

1. <span data-ttu-id="bd690-130">Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="bd690-130">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="bd690-131">このチュートリアルで使用する Azure リソースのリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="bd690-131">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="bd690-132">リソース グループ内に、プライベートな Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="bd690-132">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="bd690-133">このチュートリアルでは、後の手順で、このレジストリに Docker イメージとしてサンプル アプリをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="bd690-133">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="bd690-134">`wingtiptoysregistry` を、レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd690-134">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="bd690-135">アプリをコンテナー レジストリにプッシュする</span><span class="sxs-lookup"><span data-stu-id="bd690-135">Push your app to the container registry</span></span>

1. <span data-ttu-id="bd690-136">Maven インストールの構成ディレクトリ (既定では ~/.m2/ または C:\Users\username\.m2) に移動し、*settings.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="bd690-136">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="bd690-137">Azure CLI からコンテナー レジストリのパスワードを取得します。</span><span class="sxs-lookup"><span data-stu-id="bd690-137">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="bd690-138">*settings.xml* ファイルの新しい `<server>` コレクションに Azure Container Registry の ID とパスワードを追加します。</span><span class="sxs-lookup"><span data-stu-id="bd690-138">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="bd690-139">`id` と `username` がレジストリの名前になります。</span><span class="sxs-lookup"><span data-stu-id="bd690-139">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="bd690-140">前のコマンドで取得した `password` の値を (引用符なしで) 使用します。</span><span class="sxs-lookup"><span data-stu-id="bd690-140">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="bd690-141">Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="bd690-141">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="bd690-142">*pom.xml*ファイル内の `<properties>` コレクションを、Azure Container Registry のログイン サーバーの値で更新します。</span><span class="sxs-lookup"><span data-stu-id="bd690-142">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="bd690-143">*pom.xml*ファイル内の `<plugins>` コレクションを、`<plugin>` にログイン サーバーのアドレスと Azure Container Registry のレジストリ名が含まれるように更新します。</span><span class="sxs-lookup"><span data-stu-id="bd690-143">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
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

1. <span data-ttu-id="bd690-144">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行して Docker コンテナーを作成し、そのイメージをレジストリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="bd690-144">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="bd690-145">Maven でイメージを Azure にプッシュすると、次のいずれかのようなエラー メッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="bd690-145">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="bd690-146">エラーが発生した場合は、Docker コマンド ラインから Azure にログインします。</span><span class="sxs-lookup"><span data-stu-id="bd690-146">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="bd690-147">その後、コンテナーをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="bd690-147">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a><span data-ttu-id="bd690-148">Azure CLI を使用して ACS で Kubernetes クラスターを作成する</span><span class="sxs-lookup"><span data-stu-id="bd690-148">Create a Kubernetes Cluster on ACS using the Azure CLI</span></span>

1. <span data-ttu-id="bd690-149">Azure Container Service で Kubernetes クラスターを作成します。</span><span class="sxs-lookup"><span data-stu-id="bd690-149">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="bd690-150">次のコマンドは、*kubernetes* クラスターを *wingtiptoys-kubernetes* リソース グループ内に作成します。クラスター名として *wingtiptoys-containerservice* を、DNS プレフィックスとして *wingtiptoys-kubernetes* を使用します。</span><span class="sxs-lookup"><span data-stu-id="bd690-150">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="bd690-151">このコマンドは、完了するまで時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="bd690-151">This command may take a while to complete.</span></span>

1. <span data-ttu-id="bd690-152">Azure CLI を使用して `kubectl` をインストールします。</span><span class="sxs-lookup"><span data-stu-id="bd690-152">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="bd690-153">Kubernetes CLI は `/usr/local/bin` にデプロイされるため、Linux ユーザーはこのコマンドの前に `sudo` を付けなければならない場合があります。</span><span class="sxs-lookup"><span data-stu-id="bd690-153">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="bd690-154">クラスター構成情報をダウンロードして、Kubernetes Web インターフェイスと `kubectl` からクラスターを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="bd690-154">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="bd690-155">イメージを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="bd690-155">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="bd690-156">このチュートリアルでは、`kubectl` を使用してアプリをデプロイします。これにより、Kubernetes Web インターフェイスを通してデプロイを調べることができます。</span><span class="sxs-lookup"><span data-stu-id="bd690-156">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="bd690-157">Kubernetes Web インターフェイスを使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="bd690-157">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="bd690-158">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="bd690-158">Open a command prompt.</span></span>

1. <span data-ttu-id="bd690-159">Kubernetes クラスターの構成 Web サイトを既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="bd690-159">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="bd690-160">Kubernetes 構成 Web サイトがブラウザーで開いたら、**コンテナー化されたアプリをデプロイする**ためのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bd690-160">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes 構成 Web サイト][KB01]

1. <span data-ttu-id="bd690-162">**[Deploy a containerized app]\(コンテナー化されたアプリのデプロイ\)** ページが表示されたら、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-162">When the **Deploy a containerized app** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="bd690-163">a.</span><span class="sxs-lookup"><span data-stu-id="bd690-163">a.</span></span> <span data-ttu-id="bd690-164">**[Specify app details below]\(アプリの詳細を指定する\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd690-164">Select **Specify app details below**.</span></span>

   <span data-ttu-id="bd690-165">b.</span><span class="sxs-lookup"><span data-stu-id="bd690-165">b.</span></span> <span data-ttu-id="bd690-166">Spring Boot アプリケーション名を **[App name]\(アプリ名\)** に入力します (例: "*gs-spring-boot-docker*")。</span><span class="sxs-lookup"><span data-stu-id="bd690-166">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="bd690-167">c.</span><span class="sxs-lookup"><span data-stu-id="bd690-167">c.</span></span> <span data-ttu-id="bd690-168">ログイン サーバーとコンテナー イメージを **[Container image]\(コンテナー イメージ\)** に入力します (例: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*")。</span><span class="sxs-lookup"><span data-stu-id="bd690-168">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="bd690-169">d.</span><span class="sxs-lookup"><span data-stu-id="bd690-169">d.</span></span> <span data-ttu-id="bd690-170">**[Service]\(サービス)\** で**[External]\(外部)\** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd690-170">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="bd690-171">e.</span><span class="sxs-lookup"><span data-stu-id="bd690-171">e.</span></span> <span data-ttu-id="bd690-172">外部ポートと内部ポートを **[Port]\(ポート)\** テキスト ボックスと **[Target port]\(ターゲット ポート\)** テキスト ボックスに指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-172">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes 構成 Web サイト][KB02]


1. <span data-ttu-id="bd690-174">**[Deploy]\(デプロイ)\** をクリックしてコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="bd690-174">Click **Deploy** to deploy the container.</span></span>

   ![コンテナーをデプロイする][KB05]

1. <span data-ttu-id="bd690-176">アプリケーションがデプロイされると、**[Services]\(サービス)\** の下に Spring Boot アプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bd690-176">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes サービス][KB06]

1. <span data-ttu-id="bd690-178">**[External endpoints]\(外部エンドポイント)\** のリンクをクリックすると、Spring Boot アプリケーションが Azure で実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="bd690-178">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes サービス][KB07]

   ![Azure でサンプル アプリを参照する][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="bd690-181">kubectl を使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="bd690-181">Deploy with kubectl</span></span>

1. <span data-ttu-id="bd690-182">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="bd690-182">Open a command prompt.</span></span>

1. <span data-ttu-id="bd690-183">`kubectl run` コマンドを使用して、Kubernetes クラスターのコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="bd690-183">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="bd690-184">Kubernetes でのアプリのサービス名と完全なイメージ名を指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-184">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="bd690-185">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bd690-185">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="bd690-186">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="bd690-186">In this command:</span></span>

   * <span data-ttu-id="bd690-187">コンテナー名 `gs-spring-boot-docker` は `run` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-187">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="bd690-188">`--image` パラメーターは、結合されたログイン サーバーとイメージの名前を `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest` として指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-188">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="bd690-189">`kubectl expose` コマンドを使用して、Kubernetes クラスターを外部に公開します。</span><span class="sxs-lookup"><span data-stu-id="bd690-189">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="bd690-190">サービス名、アプリにアクセスするために使用される公開 TCP ポート、およびアプリがリッスンする内部ターゲット ポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-190">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="bd690-191">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bd690-191">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="bd690-192">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="bd690-192">In this command:</span></span>

   * <span data-ttu-id="bd690-193">コンテナー名 `gs-spring-boot-docker` は `expose deployment` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-193">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="bd690-194">`--type` パラメーターは、クラスターでロード バランサーを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-194">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="bd690-195">`--port` パラメーターは、公開 TCP ポートとして 80 を指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-195">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="bd690-196">このポートでアプリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="bd690-196">You access the app on this port.</span></span>

   * <span data-ttu-id="bd690-197">`--target-port` パラメーターは、内部 TCP ポートとして 8080 を指定します。</span><span class="sxs-lookup"><span data-stu-id="bd690-197">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="bd690-198">ロード バランサーは、このポートでアプリに要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="bd690-198">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="bd690-199">クラスターにアプリがデプロイされたら、外部 IP アドレスを照会し、そのアドレスを Web ブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="bd690-199">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Azure でサンプル アプリを参照する][SB02]


## <a name="next-steps"></a><span data-ttu-id="bd690-201">次のステップ</span><span class="sxs-lookup"><span data-stu-id="bd690-201">Next steps</span></span>

<span data-ttu-id="bd690-202">Azure での Spring Boot の使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd690-202">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="bd690-203">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="bd690-203">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="bd690-204">Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする</span><span class="sxs-lookup"><span data-stu-id="bd690-204">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

<span data-ttu-id="bd690-205">Java での Azure の使用の詳細については、 [Azure Java デベロッパー センター] と[Java Tools for Visual Studio Team Services] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd690-205">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="bd690-206">Docker サンプル プロジェクトでの Spring Boot の詳細については、[Spring Boot on Docker Getting Started]に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd690-206">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="bd690-207">次のリンクは、Spring Boot アプリケーションの作成に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="bd690-207">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="bd690-208">単純な Spring Boot アプリケーションの作成の詳細については、Spring Initializr を参照してください (https://start.spring.io/)。</span><span class="sxs-lookup"><span data-stu-id="bd690-208">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="bd690-209">次のリンクは、Azure での Kubernetes の使用に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="bd690-209">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="bd690-210">Container Service で Kubernetes クラスターを使用する</span><span class="sxs-lookup"><span data-stu-id="bd690-210">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="bd690-211">Azure Container Service で Kubernetes Web UI を使用する</span><span class="sxs-lookup"><span data-stu-id="bd690-211">Using the Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="bd690-212">Kubernetes コマンド ライン インターフェイスの使用の詳細については、**kubectl** ユーザー ガイド (<https://kubernetes.io/docs/user-guide/kubectl/>) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd690-212">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="bd690-213">Kubernetes web サイトには、プライベート レジストリでのイメージの使用に関するさまざまな記事があります。</span><span class="sxs-lookup"><span data-stu-id="bd690-213">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="bd690-214">[Pods のサービス アカウントの構成]</span><span class="sxs-lookup"><span data-stu-id="bd690-214">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="bd690-215">[名前空間]</span><span class="sxs-lookup"><span data-stu-id="bd690-215">[Namespaces]</span></span>
* <span data-ttu-id="bd690-216">[プライベート レジストリからのイメージのプル]</span><span class="sxs-lookup"><span data-stu-id="bd690-216">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="bd690-217">Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd690-217">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java デベロッパー センター]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Pods のサービス アカウントの構成]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[名前空間]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[プライベート レジストリからのイメージのプル]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

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
