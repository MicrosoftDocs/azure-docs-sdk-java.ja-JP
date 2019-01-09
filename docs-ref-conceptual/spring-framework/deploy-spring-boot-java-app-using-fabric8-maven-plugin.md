---
title: fabric8 Maven プラグインを使用して Spring Boot アプリをデプロイする
description: このチュートリアルでは、Microsoft Azure で Apache Maven 用 Fabric8 プラグインを使用して Spring Boot アプリケーションをデプロイする方法について説明します。
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
ms.openlocfilehash: 72eb49a764bdf15339e6cd17c6a7f997495dcf09
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991606"
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a><span data-ttu-id="6ea39-103">Fabric8 Maven プラグインを使用して Spring Boot アプリをデプロイする</span><span class="sxs-lookup"><span data-stu-id="6ea39-103">Deploy a Spring Boot app using the Fabric8 Maven Plugin</span></span>

<span data-ttu-id="6ea39-104">**[Fabric8]** は **[Kubernetes]** 上に構築されたオープン ソース ソリューションであり、開発者が Linux コンテナーでアプリケーションを作成するために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-104">**[Fabric8]** is an open-source solution that is built on **[Kubernetes]**, which helps developers create applications in Linux containers.</span></span>

<span data-ttu-id="6ea39-105">このチュートリアルでは、Maven 用 Fabric8 プラグインを使用してアプリケーションを開発し、[Azure Container Service (AKS)] の Linux ホストにデプロイする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-105">This tutorial walks you through using the Fabric8 plugin for Maven to develop to deploy an application to a Linux host in the [Azure Container Service (AKS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ea39-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="6ea39-106">Prerequisites</span></span>

<span data-ttu-id="6ea39-107">このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ea39-107">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="6ea39-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="6ea39-109">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="6ea39-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="6ea39-110">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="6ea39-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="6ea39-111">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="6ea39-112">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="6ea39-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="6ea39-113">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="6ea39-113">A [Git] client.</span></span>
* <span data-ttu-id="6ea39-114">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="6ea39-114">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="6ea39-115">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ea39-115">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="6ea39-116">Spring Boot on Docker Getting Started Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="6ea39-116">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="6ea39-117">次の手順で、Spring Boot Web アプリケーションをビルドしてローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="6ea39-117">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="6ea39-118">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成し、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-118">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   <span data-ttu-id="6ea39-119">-- または --</span><span class="sxs-lookup"><span data-stu-id="6ea39-119">-- or --</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. <span data-ttu-id="6ea39-120">[Docker での Spring Boot の使用開始] サンプル プロジェクトを、ディレクトリに複製します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-120">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="6ea39-121">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-121">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   <span data-ttu-id="6ea39-122">-- または --</span><span class="sxs-lookup"><span data-stu-id="6ea39-122">-- or --</span></span>
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. <span data-ttu-id="6ea39-123">Maven を使用してサンプル アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-123">Use Maven to build and run the sample app.</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```

1. <span data-ttu-id="6ea39-124">Web アプリをテストするには、 http://localhost:8080 を参照するか、次の `curl` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-124">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="6ea39-125">「**Hello Docker World**」というメッセージが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="6ea39-125">You should see a **Hello Docker World** message displayed.</span></span>

   ![サンプル アプリケーションをローカルで参照する][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a><span data-ttu-id="6ea39-127">Kubernetes コマンド ライン インターフェイスをインストールし、Azure CLI を使用して Azure リソース グループを作成します</span><span class="sxs-lookup"><span data-stu-id="6ea39-127">Install the Kubernetes command-line interface and create an Azure resource group using the Azure CLI</span></span>

1. <span data-ttu-id="6ea39-128">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-128">Open a command prompt.</span></span>

1. <span data-ttu-id="6ea39-129">次のコマンドを入力して、Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="6ea39-129">Type the following command to log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="6ea39-130">指示に従って、ログインを完了します</span><span class="sxs-lookup"><span data-stu-id="6ea39-130">Follow the instructions to complete the login process</span></span>

   <span data-ttu-id="6ea39-131">次のように、Azure CLI にアカウントの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-131">The Azure CLI will display a list of your accounts; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="6ea39-132">まだ Kubernetes コマンド ライン インターフェイス (`kubectl`) がインストールされていない場合、次のように、Azure CLI を使用してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-132">If you do not already have the Kubernetes command-line interface (`kubectl`) installed, you can install using the Azure CLI; for example:</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="6ea39-133">Kubernetes CLI は `/usr/local/bin` にデプロイされるため、Linux ユーザーはこのコマンドの前に `sudo` を付けなければならない場合があります。</span><span class="sxs-lookup"><span data-stu-id="6ea39-133">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   >
   > <span data-ttu-id="6ea39-134">`kubectl` がもうインストールされていて `kubectl` のバージョンが古すぎる場合、この記事の後半で説明する手順を完了しようとすると、次の例のようなエラー メッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="6ea39-134">If you already have `kubectl`) installed and your version of `kubectl` is too old, you may see an error message similar to the following example when you attempt to complete the steps listed later in this article:</span></span>
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > <span data-ttu-id="6ea39-135">この場合、`kubectl` を再インストールしてバージョンを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ea39-135">If this happens, you will need to reinstall `kubectl` to update your version.</span></span>
   >

1. <span data-ttu-id="6ea39-136">次のように、このチュートリアルで使用する Azure リソースのリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-136">Create a resource group for the Azure resources that you will use in this tutorial; for example:</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   <span data-ttu-id="6ea39-137">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="6ea39-137">Where:</span></span>  
      * <span data-ttu-id="6ea39-138">*wingtiptoys-kubernetes* は、リソース グループの一意な名前です</span><span class="sxs-lookup"><span data-stu-id="6ea39-138">*wingtiptoys-kubernetes* is a unique name for your resource group</span></span>  
      * <span data-ttu-id="6ea39-139">*westeurope* は、アプリケーションの適切な地理的な場所です</span><span class="sxs-lookup"><span data-stu-id="6ea39-139">*westeurope* is an appropriate geographic location for your application</span></span>  

   <span data-ttu-id="6ea39-140">次のように、Azure CLI に、リソース グループ作成の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-140">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a><span data-ttu-id="6ea39-141">Azure CLI を使用して Kubernetes クラスターを作成する</span><span class="sxs-lookup"><span data-stu-id="6ea39-141">Create a Kubernetes cluster using the Azure CLI</span></span>

1. <span data-ttu-id="6ea39-142">次のように、新しいリソース グループ内に Kubernetes クラスターを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-142">Create a Kubernetes cluster in your new resource group; for example:</span></span>  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   <span data-ttu-id="6ea39-143">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="6ea39-143">Where:</span></span>  
      * <span data-ttu-id="6ea39-144">*wingtiptoys-kubernetes* は、この記事の前半のリソース グループの名前です</span><span class="sxs-lookup"><span data-stu-id="6ea39-144">*wingtiptoys-kubernetes* is the name of your resource group from earlier in this article</span></span>  
      * <span data-ttu-id="6ea39-145">*wingtiptoys-cluster* は、Kubernetes クラスターの一意な名前です</span><span class="sxs-lookup"><span data-stu-id="6ea39-145">*wingtiptoys-cluster* is a unique name for your Kubernetes cluster</span></span>
      * <span data-ttu-id="6ea39-146">*wingtiptoys* は、アプリケーションの一意な DNS 名です</span><span class="sxs-lookup"><span data-stu-id="6ea39-146">*wingtiptoys* is a unique name DNS name for your application</span></span>

   <span data-ttu-id="6ea39-147">次のように、Azure CLI にクラスター作成の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-147">The Azure CLI will display the results of your cluster creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. <span data-ttu-id="6ea39-148">次のようにして、新しい Kubernetes クラスターの資格情報をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="6ea39-148">Download your credentials for your new Kubernetes cluster; for example:</span></span>  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. <span data-ttu-id="6ea39-149">次のコマンドで接続を検証します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-149">Verify your connection with the following command:</span></span>
   ```shell 
   kubectl get nodes
   ```

   <span data-ttu-id="6ea39-150">次の例のように、ノードと状態の一覧が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="6ea39-150">You should see a list of nodes and statuses like the following example:</span></span>

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="6ea39-151">Azure CLI を使用してプライベート Azure コンテナー レジストリを作成する</span><span class="sxs-lookup"><span data-stu-id="6ea39-151">Create a private Azure container registry using the Azure CLI</span></span>

1. <span data-ttu-id="6ea39-152">Docker イメージをホストするために、プライベート Azure コンテナー レジストリをリソース グループ内に作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-152">Create a private Azure container registry in your resource group to host your Docker image; for example:</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="6ea39-153">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="6ea39-153">Where:</span></span>

   | <span data-ttu-id="6ea39-154">パラメーター</span><span class="sxs-lookup"><span data-stu-id="6ea39-154">Parameter</span></span> | <span data-ttu-id="6ea39-155">説明</span><span class="sxs-lookup"><span data-stu-id="6ea39-155">Description</span></span> |
   |---|---|
   | `wingtiptoys-kubernetes` | <span data-ttu-id="6ea39-156">この記事の前半のリソース グループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-156">Specifies the name of your resource group from earlier in this article.</span></span> |
   | `wingtiptoysregistry` | <span data-ttu-id="6ea39-157">ご使用のプライベート レジストリの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-157">Specifies a unique name for your private registry.</span></span> |
   | `westeurope` | <span data-ttu-id="6ea39-158">アプリケーションの適切な地理的位置を指定します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-158">Specifies an appropriate geographic location for your application.</span></span> |

   <span data-ttu-id="6ea39-159">次のように、Azure CLI にレジストリ作成の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-159">The Azure CLI will display the results of your registry creation; for example:</span></span>  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

2. <span data-ttu-id="6ea39-160">Azure CLI からコンテナー レジストリのパスワードを取得します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-160">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   <span data-ttu-id="6ea39-161">次のように、Azure CLI にレジストリのパスワード結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-161">The Azure CLI will display the password for your registry; for example:</span></span>  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

3. <span data-ttu-id="6ea39-162">Maven インストールの構成ディレクトリ (既定では ~/.m2/ または C:\Users\username\.m2) に移動し、*settings.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-162">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

4. <span data-ttu-id="6ea39-163">*settings.xml* ファイルの新しい `<server>` コレクションに Azure Container Registry の URL、ユーザー名、およびパスワードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-163">Add your Azure Container Registry URL, username and password to a new `<server>` collection in the *settings.xml* file.</span></span>
   <span data-ttu-id="6ea39-164">`id` と `username` がレジストリの名前になります。</span><span class="sxs-lookup"><span data-stu-id="6ea39-164">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="6ea39-165">前のコマンドで取得した `password` の値を (引用符なしで) 使用します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-165">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

5. <span data-ttu-id="6ea39-166">Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-166">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

6. <span data-ttu-id="6ea39-167">*pom.xml*ファイル内の `<properties>` コレクションを、Azure Container Registry のログイン サーバーの値で更新します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-167">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

7. <span data-ttu-id="6ea39-168">*pom.xml*ファイル内の `<plugins>` コレクションを、`<plugin>` にログイン サーバーのアドレスと Azure Container Registry のレジストリ名が含まれるように更新します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-168">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

8. <span data-ttu-id="6ea39-169">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次の Maven コマンドを実行して Docker コンテナーを作成し、そのイメージをレジストリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="6ea39-169">Navigate to the completed project directory for your Spring Boot application, and run the following Maven command to build the Docker container and push the image to your registry:</span></span>

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   <span data-ttu-id="6ea39-170">次のように、Maven にビルドの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-170">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a><span data-ttu-id="6ea39-171">Fabric8 Maven プラグインを使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="6ea39-171">Configure your Spring Boot app to use the Fabric8 Maven plugin</span></span>

1. <span data-ttu-id="6ea39-172">Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-172">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="6ea39-173">*pom.xml* ファイル内の `<plugins>` コレクションを更新して、Fabric8 Maven プラグインを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-173">Update the `<plugins>` collection in the *pom.xml* file to add the Fabric8 Maven plugin:</span></span>

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="6ea39-174">Spring Boot アプリケーションのメイン ソース ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" や "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*" など) に移動し、"*fabric8*" という名前の新しいフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-174">Navigate to the main source directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*"), and create a new folder named "*fabric8*".</span></span>

1. <span data-ttu-id="6ea39-175">新しい *fabric8* フォルダー内に 3 つの YAML フラグメント ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-175">Create three YAML fragment files in the new *fabric8* folder:</span></span>

   <span data-ttu-id="6ea39-176">a.</span><span class="sxs-lookup"><span data-stu-id="6ea39-176">a.</span></span> <span data-ttu-id="6ea39-177">**deployment.yml** という名前のファイルを作成し、内容を次のようにします。</span><span class="sxs-lookup"><span data-stu-id="6ea39-177">Create a file named **deployment.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   <span data-ttu-id="6ea39-178">b.</span><span class="sxs-lookup"><span data-stu-id="6ea39-178">b.</span></span> <span data-ttu-id="6ea39-179">**secrets.yml** という名前のファイルを作成し、内容を次のようにします。</span><span class="sxs-lookup"><span data-stu-id="6ea39-179">Create a file named **secrets.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   <span data-ttu-id="6ea39-180">c.</span><span class="sxs-lookup"><span data-stu-id="6ea39-180">c.</span></span> <span data-ttu-id="6ea39-181">**service.yml** という名前のファイルを作成し、内容を次のようにします。</span><span class="sxs-lookup"><span data-stu-id="6ea39-181">Create a file named **service.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. <span data-ttu-id="6ea39-182">次の Maven コマンドを実行して、Kubernetes リソース リスト ファイルを構築します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-182">Run the following Maven command to build the Kubernetes resource list file:</span></span>

   ```shell
   mvn fabric8:resource
   ```

   <span data-ttu-id="6ea39-183">このコマンドは、*src/main/fabric8* フォルダー下のすべての Kubernetes リソース yaml ファイルを、Kubernetes リソース リストを含む YAML ファイルにマージします。このマージ後のファイルを Kubernetes クラスターに直接適用、または Helm グラフにエクスポートすることができます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-183">This command merges all Kubernetes resource yaml files under the *src/main/fabric8* folder to a YAML file that contains a Kubernetes resource list, which can be applied to Kubernetes cluster directly or export to a helm chart.</span></span>

   <span data-ttu-id="6ea39-184">次のように、Maven にビルドの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-184">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="6ea39-185">次の Maven コマンドを実行して、リソース リスト ファイルを Kubernetes クラスターに適用します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-185">Run the following Maven command to apply the resource list file to your Kubernetes cluster:</span></span>

   ```shell
   mvn fabric8:apply
   ```

   <span data-ttu-id="6ea39-186">次のように、Maven にビルドの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-186">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="6ea39-187">クラスターにアプリがデプロイされたら、次のように、`kubectl` アプリケーションを使用して外部 IP アドレスを照会します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-187">Once the app is deployed to the cluster, query the external IP address using the `kubectl` application; for example:</span></span>
   ```shell
   kubectl get svc -w
   ```

   <span data-ttu-id="6ea39-188">次のように、内部および外部の IP アドレスが `kubectl` に表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-188">`kubectl` will display your internal and external IP addresses; for example:</span></span>

   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```

   <span data-ttu-id="6ea39-189">外部 IP アドレスを使用して、Web ブラウザーでアプリケーションを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="6ea39-189">You can use the external IP address to open your application in a web browser.</span></span>

   ![サンプル アプリケーションを外部で参照する][SB02]

## <a name="delete-your-kubernetes-cluster"></a><span data-ttu-id="6ea39-191">Kubernetes クラスターの削除</span><span class="sxs-lookup"><span data-stu-id="6ea39-191">Delete your Kubernetes cluster</span></span>

<span data-ttu-id="6ea39-192">Kubernetes クラスターが不要になったら、`az group delete` コマンドを使用してリソース グループを削除することができます。これにより、その関連リソースがすべて削除されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="6ea39-192">When your Kubernetes cluster is no longer needed, you can use the `az group delete` command to remove the resource group, which will remove all of its related resources; for example:</span></span>

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a><span data-ttu-id="6ea39-193">次の手順</span><span class="sxs-lookup"><span data-stu-id="6ea39-193">Next steps</span></span>

<span data-ttu-id="6ea39-194">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-194">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6ea39-195">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="6ea39-195">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="6ea39-196">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="6ea39-196">Additional Resources</span></span>

<span data-ttu-id="6ea39-197">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-197">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="6ea39-198">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="6ea39-198">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="6ea39-199">Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする</span><span class="sxs-lookup"><span data-stu-id="6ea39-199">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)
* [<span data-ttu-id="6ea39-200">Azure Container Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="6ea39-200">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="6ea39-201">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-201">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="6ea39-202">Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-202">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="6ea39-203">独自の Spring Boot アプリケーションの使用開始に関するヘルプについては、「**Spring Initializr**」(<https://start.spring.io/>) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-203">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at <https://start.spring.io/>.</span></span>

<span data-ttu-id="6ea39-204">単純な Spring Boot アプリケーションの作成開始の詳細については、「Spring Initializr」(<https://start.spring.io/>) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-204">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at <https://start.spring.io/>.</span></span>

<span data-ttu-id="6ea39-205">Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea39-205">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Azure DevOps と Java の操作]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker での Spring Boot の使用開始]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB02.png
