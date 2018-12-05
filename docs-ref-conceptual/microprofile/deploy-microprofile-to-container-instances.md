---
title: Docker と Azure を使用して MicroProfile アプリをクラウドにデプロイする
description: Docker および Azure Container Instances を使用して MicroProfile アプリをクラウドにデプロイする方法について説明します。
services: container-instances;container-retistry
documentationcenter: java
author: brunoborges
manager: routlaw
editor: brunoborges
ms.assetid: ''
ms.author: brborges
ms.date: 11/21/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 22870b7ba32f115e7270c63d1bf42cbfc6531d7e
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338786"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a><span data-ttu-id="8b7f5-103">Docker と Azure を使用して MicroProfile アプリケーションをクラウドにデプロイする</span><span class="sxs-lookup"><span data-stu-id="8b7f5-103">Deploy a MicroProfile application to the cloud with Docker and Azure</span></span>

<span data-ttu-id="8b7f5-104">この記事では、[MicroProfile.io] アプリケーションを Docker コンテナーにパックし、Azure Container Instances で実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
>
> <span data-ttu-id="8b7f5-105">この手順は、Docker コンテナー イメージが自己実行可能である場合 (つまり、ランタイムが含まれる場合) に、任意の MicroProfile.io の実装で機能します。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-105">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b7f5-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="8b7f5-106">Prerequisites</span></span>

<span data-ttu-id="8b7f5-107">このチュートリアルの手順を実行するには、次の前提条件を満たしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-107">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="8b7f5-108">Azure サブスクリプション: Azure サブスクリプションをまだ取得していない場合は、[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-108">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="8b7f5-109">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="8b7f5-110">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="8b7f5-111">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="8b7f5-112">Apache の [Maven] ビルド ツール (バージョン 3 以上)。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-112">Apache's [Maven] build tool (version 3+).</span></span>
* <span data-ttu-id="8b7f5-113">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-113">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="8b7f5-114">MicroProfile Hello Azure サンプル</span><span class="sxs-lookup"><span data-stu-id="8b7f5-114">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="8b7f5-115">この記事では、[MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) サンプルを使用します。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-115">For this article, we will use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample:</span></span>

### <a name="clone-build-and-run-locally"></a><span data-ttu-id="8b7f5-116">ローカルで複製、ビルドし、および実行する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-116">Clone, build, and run locally</span></span>

```bash
$ git clone https://github.com/Azure-Samples/microprofile-hello-azure.git
$ mvn clean package
...
[INFO] BUILD SUCCESS
...
$ mvn payara-micro:start
...
[2018-07-30T13:34:51.553-0700] [] [INFO] [] [PayaraMicro] [tid: _ThreadID=1 _ThreadName=main] [timeMillis: 1532982891553] [levelValue: 800] Payara Micro  5.182 #badassmicrofish (build 303) ready in 10,304 (ms)
...
```

<span data-ttu-id="8b7f5-117">アプリケーションをテストするには、`curl` を呼び出すか、[ブラウザー](http://localhost:8080/api/hello)を使用してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-117">You can test the application by calling `curl` or visiting through a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a><span data-ttu-id="8b7f5-118">[Deploy to Azure (Azure へのデプロイ)]</span><span class="sxs-lookup"><span data-stu-id="8b7f5-118">Deploy to Azure</span></span>

<span data-ttu-id="8b7f5-119">ここで [Azure Container Instances] および [Azure Container Registry] サービスを使用して、このアプリケーションをクラウドに移行しましょう。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-119">Now let's bring this application to the cloud using [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="8b7f5-120">Docker イメージをビルドする</span><span class="sxs-lookup"><span data-stu-id="8b7f5-120">Build a Docker image</span></span>

<span data-ttu-id="8b7f5-121">サンプル プロジェクトには、使用できる Docker ファイルが既に用意されています。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-121">The sample project already provides a Dockerfile you can use.</span></span> <span data-ttu-id="8b7f5-122">ただし、クラウドでのイメージのビルドには Azure Container Registry Build 機能を使用するため、Docker をインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-122">You don't need Docker installed though, as we will use Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="8b7f5-123">イメージをビルドして Azure で実行できるようにするには、次の手順に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-123">To build the image and be ready to run on Azure, you will have to follow these steps:</span></span>

1. <span data-ttu-id="8b7f5-124">Azure CLI でインストールおよびログインする</span><span class="sxs-lookup"><span data-stu-id="8b7f5-124">Install and log in with Azure CLI</span></span>
1. <span data-ttu-id="8b7f5-125">Azure リソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-125">Create an Azure Resource Group</span></span>
1. <span data-ttu-id="8b7f5-126">Azure Container Registry (ACR) を作成する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-126">Create an Azure Container Registry (ACR)</span></span>
1. <span data-ttu-id="8b7f5-127">Docker イメージを構築する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-127">Build the Docker image</span></span>
1. <span data-ttu-id="8b7f5-128">前に作成した ACR に Docker イメージを発行する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-128">Publish the Docker image to the ACR created before</span></span>
1. <span data-ttu-id="8b7f5-129">(省略可能) 1 つのコマンドでビルドして ACR に発行する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-129">(Optionally) Build and publish to ACR in one command</span></span>


#### <a name="set-up-azure-cli"></a><span data-ttu-id="8b7f5-130">Azure CLI をセットアップする</span><span class="sxs-lookup"><span data-stu-id="8b7f5-130">Set up Azure CLI</span></span>

<span data-ttu-id="8b7f5-131">Azure のサブスクリプションを所有していること、[Azure CLI がインストールされている](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)こと、およびお使いのアカウントにご自身が認証されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-131">Make sure you have an Azure subscription, [Azure CLI installed](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you are authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="8b7f5-132">リソース グループを作成します</span><span class="sxs-lookup"><span data-stu-id="8b7f5-132">Create a Resource Group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="8b7f5-133">Azure Container Registry インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="8b7f5-133">Create an Azure Container Registry instance</span></span>

<span data-ttu-id="8b7f5-134">このコマンドによって、ランダムな数値を持つ基本的な名前を使用して、(うまくいけば) グローバルに一意のコンテナー レジストリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-134">This command will create a globally unique (hopefully) container registry using a basic name with a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="8b7f5-135">Docker イメージを構築する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-135">Build the Docker image</span></span>

<span data-ttu-id="8b7f5-136">Docker 自体を使用して Docker イメージをローカルで簡単にビルドできますが、クラウドでのビルドを検討する理由はいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-136">While you can easily build the Docker image locally using Docker itself, you may want to consider building it in the Cloud for few reasons:</span></span>

1. <span data-ttu-id="8b7f5-137">Docker をローカルにインストールする必要がない</span><span class="sxs-lookup"><span data-stu-id="8b7f5-137">No need to install Docker locally</span></span>
1. <span data-ttu-id="8b7f5-138">ビルドが別の場所で行われるため、はるかに高速 (コンテキストのアップロード時間を除く)</span><span class="sxs-lookup"><span data-stu-id="8b7f5-138">Much faster since build will happen elsewhere (except for context upload time)</span></span>
1. <span data-ttu-id="8b7f5-139">クラウドのプロセスでより高速なインターネットにアクセスできるため、ダウンロードがさらに速くなる</span><span class="sxs-lookup"><span data-stu-id="8b7f5-139">Process in the Cloud has access to faster Internet, therefore faster downloads</span></span>
1. <span data-ttu-id="8b7f5-140">イメージが Container Registry に直接移動する</span><span class="sxs-lookup"><span data-stu-id="8b7f5-140">Image goes directly into the Container Registry</span></span>

<span data-ttu-id="8b7f5-141">これらの理由により、[Azure Container Registry Build] 機能を使用してイメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-141">Because of these reasons, we will build the image using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a><span data-ttu-id="8b7f5-142">Docker イメージを Azure Container Registry (ACR) から Azure Container Instances (ACI) にデプロイする</span><span class="sxs-lookup"><span data-stu-id="8b7f5-142">Deploy Docker Image from Azure Container Registry (ACR) into Container Instances (ACI)</span></span>

<span data-ttu-id="8b7f5-143">ACR でイメージを使用できるようになったので、次はコンテナー インスタンスを ACI にプッシュしてインスタンス化してみましょう。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-143">Now that the image is available on your ACR, let's push and instanciate a container instance on ACI.</span></span> <span data-ttu-id="8b7f5-144">ただし、最初に、ACR に認証できることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-144">But first, we need to make sure we can authenticate into the ACR:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="8b7f5-145">デプロイされた MicroProfile アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="8b7f5-145">Test Your Deployed MicroProfile Application</span></span>

<span data-ttu-id="8b7f5-146">これでご自身のアプリケーションが起動されて実行されます。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-146">Your application should now be up and running.</span></span> <span data-ttu-id="8b7f5-147">コマンド ラインからこれをテストするには、次のコマンドを実行してみます。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-147">To test it from the command-line, try the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="8b7f5-148">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-148">Congratulations!</span></span> <span data-ttu-id="8b7f5-149">MicroProfile アプリケーションを Docker コンテナーとして適切にビルドし、Microsoft Azure にデプロイできました。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-149">You have successfuly built and deployed a MicroProfile application as a Docker container onto Microsoft Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b7f5-150">次の手順</span><span class="sxs-lookup"><span data-stu-id="8b7f5-150">Next steps</span></span>

<span data-ttu-id="8b7f5-151">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8b7f5-151">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* [<span data-ttu-id="8b7f5-152">Azure CLI から Azure へのログイン</span><span class="sxs-lookup"><span data-stu-id="8b7f5-152">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry