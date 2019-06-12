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
ms.openlocfilehash: 9ab781d27e8968ab867efc65f3ac422ac6253a6a
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270857"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a><span data-ttu-id="3365c-103">Azure Kubernetes Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="3365c-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Kubernetes Service</span></span>

<span data-ttu-id="3365c-104">**[Kubernetes]** と **[Docker]** は、開発者が、コンテナーで実行されるアプリケーションのデプロイ、スケーリング、管理を自動化することを支援するオープン ソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="3365c-104">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications running in containers.</span></span>

<span data-ttu-id="3365c-105">このチュートリアルでは、この 2 つの一般的なオープンソース テクノロジを組み合わせて Spring Boot アプリケーションを開発し、Microsoft Azure にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3365c-105">This tutorial walks you through combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="3365c-106">具体的には、アプリケーション開発に *[Spring Boot]* を、コンテナーのデプロイに *[Kubernetes]* を、アプリケーションのホストとして [Azure Kubernetes Service (AKS)] をそれぞれ使用します。</span><span class="sxs-lookup"><span data-stu-id="3365c-106">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Kubernetes Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3365c-107">前提条件</span><span class="sxs-lookup"><span data-stu-id="3365c-107">Prerequisites</span></span>

* <span data-ttu-id="3365c-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="3365c-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="3365c-109">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="3365c-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="3365c-110">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="3365c-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="3365c-111">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="3365c-112">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="3365c-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="3365c-113">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="3365c-113">A [Git] client.</span></span>
* <span data-ttu-id="3365c-114">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="3365c-114">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="3365c-115">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3365c-115">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="3365c-116">Spring Boot on Docker Getting Started Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="3365c-116">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="3365c-117">次の手順で、Spring Boot Web アプリケーションをビルドしてローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="3365c-117">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="3365c-118">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成し、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3365c-118">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="3365c-119">-- または --</span><span class="sxs-lookup"><span data-stu-id="3365c-119">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="3365c-120">[Docker での Spring Boot の使用開始] サンプル プロジェクトを、ディレクトリに複製します。</span><span class="sxs-lookup"><span data-stu-id="3365c-120">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="3365c-121">完成したプロジェクトにディレクトリを変更します。</span><span class="sxs-lookup"><span data-stu-id="3365c-121">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="3365c-122">Maven を使用してサンプル アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="3365c-122">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="3365c-123">Web アプリをテストするには、 `http://localhost:8080` を参照するか、次の `curl` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3365c-123">Test the web app by browsing to `http://localhost:8080`, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="3365c-124">次のメッセージが表示されます。**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="3365c-124">You should see the following message displayed: **Hello Docker World**</span></span>

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="3365c-126">Azure CLI を使用して Azure Container Registry を作成する</span><span class="sxs-lookup"><span data-stu-id="3365c-126">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="3365c-127">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="3365c-127">Open a command prompt.</span></span>

1. <span data-ttu-id="3365c-128">Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="3365c-128">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="3365c-129">Azure サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="3365c-129">Choose your Azure Subscription:</span></span>
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. <span data-ttu-id="3365c-130">このチュートリアルで使用する Azure リソースのリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="3365c-130">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="3365c-131">リソース グループ内に、プライベートな Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="3365c-131">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="3365c-132">このチュートリアルでは、後の手順で、このレジストリに Docker イメージとしてサンプル アプリをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="3365c-132">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="3365c-133">`wingtiptoysregistry` を、レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3365c-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a><span data-ttu-id="3365c-134">Jib を使用してアプリをコンテナー レジストリにプッシュする</span><span class="sxs-lookup"><span data-stu-id="3365c-134">Push your app to the container registry via Jib</span></span>

1. <span data-ttu-id="3365c-135">Azure CLI から Azure Container Registry にログインします。</span><span class="sxs-lookup"><span data-stu-id="3365c-135">Log in to your Azure Container Registry from the Azure CLI.</span></span>
   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. <span data-ttu-id="3365c-136">Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や " */users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="3365c-136">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="3365c-137">*pom.xml* ファイル内の `<properties>` コレクションを、Azure Container Registry のレジストリ名と [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) の最新バージョンで更新します。</span><span class="sxs-lookup"><span data-stu-id="3365c-137">Update the `<properties>` collection in the *pom.xml* file with the registry name for your Azure Container Registry and the latest version of [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin).</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="3365c-138">*pom.xml* ファイルの `<plugins>` コレクションを、`jib-maven-plugin` に `<plugin>` が含まれるように更新します。</span><span class="sxs-lookup"><span data-stu-id="3365c-138">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the `jib-maven-plugin`.</span></span>

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="3365c-139">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行してイメージを作成し、そのイメージをレジストリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="3365c-139">Navigate to the completed project directory for your Spring Boot application and run the following command to build the image and push the image to the registry:</span></span>

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> <span data-ttu-id="3365c-140">Azure Cli と Azure Container Registry のセキュリティ上の懸念により、`az acr login` によって作成された資格情報は 1 時間有効であり、"*401 権限がありません*" エラーが発生した場合は、もう一度 `az acr login -n <your registry name>` コマンドを実行して再認証できます。</span><span class="sxs-lookup"><span data-stu-id="3365c-140">Due to the security concern of Azure Cli and Azure Container Registry, the credential created by `az acr login` is valid for 1 hour, if you meet *401 Unauthorized* error, you can run the `az acr login -n <your registry name>` command again to reauthenticate.</span></span>
>

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="3365c-141">Azure CLI を使用して AKS で Kubernetes クラスターを作成する</span><span class="sxs-lookup"><span data-stu-id="3365c-141">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="3365c-142">Azure Kubernetes Service で Kubernetes クラスターを作成します。</span><span class="sxs-lookup"><span data-stu-id="3365c-142">Create a Kubernetes cluster in Azure Kubernetes Service.</span></span> <span data-ttu-id="3365c-143">次のコマンドでは、*kubernetes* クラスターを *wingtiptoys-kubernetes* リソース グループに作成します。クラスター名として *wingtiptoys-akscluster* を、DNS プレフィックスとして *wingtiptoys-kubernetes* を使用します。</span><span class="sxs-lookup"><span data-stu-id="3365c-143">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-akscluster* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   <span data-ttu-id="3365c-144">このコマンドは、完了するまで時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="3365c-144">This command may take a while to complete.</span></span>

1. <span data-ttu-id="3365c-145">Azure Kubernetes Service (AKS) で Azure Container Registry (ACR) を使用する場合は、Azure Kubernetes Service に Azure Container Registry へのプル アクセス権を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3365c-145">When you're using Azure Container Registry (ACR) with Azure Kubernetes Service (AKS), you need to grant Azure Kubernetes Service pull access to Azure Container Registry.</span></span> <span data-ttu-id="3365c-146">Azure Kubernetes Service の作成時に、既定のサービス プリンシパルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3365c-146">Azure creates a default service principal when you are creating Azure Kubernetes Service.</span></span> <span data-ttu-id="3365c-147">Bash または PowerShell で次のスクリプトを使用して、ACR へのアクセス権を AKS に付与します。詳細については、「[Azure Kubernetes Service から Azure Container Registry の認証を受ける]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-147">Please run the following scripts in bash or Powershell to grant AKS access to ACR, see more details at [Authenticate with Azure Container Registry from Azure Kubernetes Service].</span></span>

```bash
   # Get the id of the service principal configured for AKS
   CLIENT_ID=$(az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv)
   
   # Get the ACR registry resource id
   ACR_ID=$(az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv)
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

  <span data-ttu-id="3365c-148">-- または --</span><span class="sxs-lookup"><span data-stu-id="3365c-148">-- or --</span></span>

```PowerShell
   # Get the id of the service principal configured for AKS
   $CLIENT_ID = az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv
   
   # Get the ACR registry resource id
   $ACR_ID = az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

1. <span data-ttu-id="3365c-149">Azure CLI を使用して `kubectl` をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3365c-149">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="3365c-150">Kubernetes CLI は `/usr/local/bin` にデプロイされるため、Linux ユーザーはこのコマンドの前に `sudo` を付けなければならない場合があります。</span><span class="sxs-lookup"><span data-stu-id="3365c-150">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az aks install-cli
   ```

1. <span data-ttu-id="3365c-151">クラスター構成情報をダウンロードして、Kubernetes Web インターフェイスと `kubectl` からクラスターを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3365c-151">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="3365c-152">イメージを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="3365c-152">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="3365c-153">このチュートリアルでは、`kubectl` を使用してアプリをデプロイします。これにより、Kubernetes Web インターフェイスを通してデプロイを調べることができます。</span><span class="sxs-lookup"><span data-stu-id="3365c-153">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-kubectl"></a><span data-ttu-id="3365c-154">kubectl を使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="3365c-154">Deploy with kubectl</span></span>

1. <span data-ttu-id="3365c-155">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="3365c-155">Open a command prompt.</span></span>

1. <span data-ttu-id="3365c-156">`kubectl run` コマンドを使用して、Kubernetes クラスターのコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="3365c-156">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="3365c-157">Kubernetes でのアプリのサービス名と完全なイメージ名を指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-157">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="3365c-158">例:</span><span class="sxs-lookup"><span data-stu-id="3365c-158">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="3365c-159">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="3365c-159">In this command:</span></span>

   * <span data-ttu-id="3365c-160">コンテナー名 `gs-spring-boot-docker` は `run` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-160">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="3365c-161">`--image` パラメーターは、結合されたログイン サーバーとイメージの名前を `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest` として指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-161">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="3365c-162">`kubectl expose` コマンドを使用して、Kubernetes クラスターを外部に公開します。</span><span class="sxs-lookup"><span data-stu-id="3365c-162">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="3365c-163">サービス名、アプリにアクセスするために使用される公開 TCP ポート、およびアプリがリッスンする内部ターゲット ポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-163">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="3365c-164">例:</span><span class="sxs-lookup"><span data-stu-id="3365c-164">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="3365c-165">このコマンドの説明:</span><span class="sxs-lookup"><span data-stu-id="3365c-165">In this command:</span></span>

   * <span data-ttu-id="3365c-166">コンテナー名 `gs-spring-boot-docker` は `expose deployment` コマンドの直後に指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-166">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="3365c-167">`--type` パラメーターは、クラスターでロード バランサーを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-167">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="3365c-168">`--port` パラメーターは、公開 TCP ポートとして 80 を指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-168">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="3365c-169">このポートでアプリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="3365c-169">You access the app on this port.</span></span>

   * <span data-ttu-id="3365c-170">`--target-port` パラメーターは、内部 TCP ポートとして 8080 を指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-170">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="3365c-171">ロード バランサーは、このポートでアプリに要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="3365c-171">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="3365c-172">クラスターにアプリがデプロイされたら、外部 IP アドレスを照会し、そのアドレスを Web ブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="3365c-172">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=default
   ```

   ![Azure でサンプル アプリを参照する][SB02]



### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="3365c-174">Kubernetes Web インターフェイスを使用してデプロイする</span><span class="sxs-lookup"><span data-stu-id="3365c-174">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="3365c-175">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="3365c-175">Open a command prompt.</span></span>

1. <span data-ttu-id="3365c-176">Kubernetes クラスターの構成 Web サイトを既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="3365c-176">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. <span data-ttu-id="3365c-177">Kubernetes 構成 Web サイトがブラウザーで開いたら、**コンテナー化されたアプリをデプロイする**ためのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3365c-177">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes 構成 Web サイト][KB01]

1. <span data-ttu-id="3365c-179">**[Resource Creation]\(リソースの作成)** ページが表示されたら、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-179">When the **Resource Creation** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="3365c-180">a.</span><span class="sxs-lookup"><span data-stu-id="3365c-180">a.</span></span> <span data-ttu-id="3365c-181">**[CREATE AN APP]\(アプリの作成)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3365c-181">Select **CREATE AN APP**.</span></span>

   <span data-ttu-id="3365c-182">b.</span><span class="sxs-lookup"><span data-stu-id="3365c-182">b.</span></span> <span data-ttu-id="3365c-183">Spring Boot アプリケーション名を **[App name]\(アプリ名\)** に入力します (例: "*gs-spring-boot-docker*")。</span><span class="sxs-lookup"><span data-stu-id="3365c-183">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="3365c-184">c.</span><span class="sxs-lookup"><span data-stu-id="3365c-184">c.</span></span> <span data-ttu-id="3365c-185">ログイン サーバーとコンテナー イメージを **[Container image]\(コンテナー イメージ\)** に入力します (例: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*")。</span><span class="sxs-lookup"><span data-stu-id="3365c-185">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="3365c-186">d.</span><span class="sxs-lookup"><span data-stu-id="3365c-186">d.</span></span> <span data-ttu-id="3365c-187">**[Service]\(サービス)** で **[External]\(外部)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3365c-187">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="3365c-188">e.</span><span class="sxs-lookup"><span data-stu-id="3365c-188">e.</span></span> <span data-ttu-id="3365c-189">外部ポートと内部ポートを **[Port]\(ポート)** テキスト ボックスと **[Target port]\(ターゲット ポート\)** テキスト ボックスに指定します。</span><span class="sxs-lookup"><span data-stu-id="3365c-189">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes 構成 Web サイト][KB02]


1. <span data-ttu-id="3365c-191">**[Deploy]\(デプロイ)** をクリックしてコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3365c-191">Click **Deploy** to deploy the container.</span></span>

   ![Kubernetes デプロイ][KB05]

1. <span data-ttu-id="3365c-193">アプリケーションがデプロイされると、 **[Services]\(サービス)** の下に Spring Boot アプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3365c-193">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes サービス][KB06]

1. <span data-ttu-id="3365c-195">**[External endpoints]\(外部エンドポイント)\\** のリンクをクリックすると、Spring Boot アプリケーションが Azure で実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="3365c-195">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes サービス][KB07]

   ![Azure でサンプル アプリを参照する][SB02]

## <a name="next-steps"></a><span data-ttu-id="3365c-198">次の手順</span><span class="sxs-lookup"><span data-stu-id="3365c-198">Next steps</span></span>

<span data-ttu-id="3365c-199">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="3365c-199">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3365c-200">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="3365c-200">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="3365c-201">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="3365c-201">Additional Resources</span></span>

<span data-ttu-id="3365c-202">Azure での Spring Boot の使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-202">For more information about using Spring Boot on Azure, see the following article:</span></span>

* [<span data-ttu-id="3365c-203">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="3365c-203">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

<span data-ttu-id="3365c-204">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-204">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="3365c-205">Visual Studio Code を使用して Java アプリケーションを Kubernetes にデプロイする方法の詳細については、[Visual Studio Code Java チュートリアル]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-205">For more information about deploying a Java application to Kubernetes with Visual Studio Code, see [Visual Studio Code Java Tutorials].</span></span>

<span data-ttu-id="3365c-206">Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-206">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="3365c-207">次のリンクは、Spring Boot アプリケーションの作成に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="3365c-207">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="3365c-208">単純な Spring Boot アプリケーションの作成の詳細については、Spring Initializr (https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-208">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="3365c-209">次のリンクは、Azure での Kubernetes の使用に関する追加情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="3365c-209">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="3365c-210">Azure Kubernetes Service で Kubernetes クラスターを使用する</span><span class="sxs-lookup"><span data-stu-id="3365c-210">Get started with a Kubernetes cluster in Azure Kubernetes Service</span></span>](/azure/aks/intro-kubernetes)

<span data-ttu-id="3365c-211">Kubernetes コマンド ライン インターフェイスの使用方法の詳細については、**kubectl** ユーザー ガイド (<https://kubernetes.io/docs/user-guide/kubectl/>) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-211">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="3365c-212">Kubernetes web サイトには、プライベート レジストリでのイメージの使用に関するさまざまな記事があります。</span><span class="sxs-lookup"><span data-stu-id="3365c-212">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="3365c-213">[Pods のサービス アカウントの構成]</span><span class="sxs-lookup"><span data-stu-id="3365c-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="3365c-214">[名前空間]</span><span class="sxs-lookup"><span data-stu-id="3365c-214">[Namespaces]</span></span>
* <span data-ttu-id="3365c-215">[プライベート レジストリからのイメージのプル]</span><span class="sxs-lookup"><span data-stu-id="3365c-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="3365c-216">Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-216">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<span data-ttu-id="3365c-217">Azure Kubernetes Service (AKS) で直接 Azure Dev Spaces を使用してコンテナーの実行とデバッグを繰り返す場合の詳細については、「[Azure Dev Spaces での Java の使用]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3365c-217">For more information about iteratively running and debugging containers directly in Azure Kubernetes Service (AKS) with Azure Dev Spaces, see [Get started on Azure Dev Spaces with Java]</span></span>

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
[Azure Dev Spaces での Java の使用]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
[Get started on Azure Dev Spaces with Java]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
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
