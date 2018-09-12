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
ms.date: 07/30/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 336af51bbdf5d2f843c3868ebc2358e128daaeaa
ms.sourcegitcommit: 280d13b43cef94177d95e03879a5919da234a23c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2018
ms.locfileid: "43324328"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a><span data-ttu-id="e0262-103">Docker と Azure を使用して MicroProfile アプリケーションをクラウドにデプロイする</span><span class="sxs-lookup"><span data-stu-id="e0262-103">Deploy a MicroProfile application to the cloud with Docker and Azure</span></span>

<span data-ttu-id="e0262-104">この記事では、[MicroProfile.io] アプリケーションを Docker コンテナーにパックし、Azure Container Instances で実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e0262-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
>
> <span data-ttu-id="e0262-105">この手順は、Docker コンテナー イメージが自己実行可能である場合 (つまり、ランタイムが含まれる場合) に、任意の MicroProfile.io の実装で機能します。</span><span class="sxs-lookup"><span data-stu-id="e0262-105">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0262-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="e0262-106">Prerequisites</span></span>

<span data-ttu-id="e0262-107">このチュートリアルの手順を実行するには、次の前提条件を満たしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0262-107">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="e0262-108">Azure サブスクリプション: Azure サブスクリプションをまだ取得していない場合は、[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="e0262-108">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="e0262-109">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="e0262-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="e0262-110">最新の [Java Development Kit (JDK)] (バージョン 1.8 以降)。</span><span class="sxs-lookup"><span data-stu-id="e0262-110">An up-to-date [Java Development Kit (JDK)], version 1.8 or later.</span></span>
* <span data-ttu-id="e0262-111">Apache の [Maven] ビルド ツール (バージョン 3 以上)。</span><span class="sxs-lookup"><span data-stu-id="e0262-111">Apache's [Maven] build tool (version 3+).</span></span>
* <span data-ttu-id="e0262-112">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="e0262-112">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="e0262-113">MicroProfile Hello Azure サンプル</span><span class="sxs-lookup"><span data-stu-id="e0262-113">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="e0262-114">この記事では、[MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) サンプルを使用します。</span><span class="sxs-lookup"><span data-stu-id="e0262-114">For this article, we will use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample:</span></span>

### <a name="clone-build-and-run-locally"></a><span data-ttu-id="e0262-115">ローカルで複製、ビルドし、および実行する</span><span class="sxs-lookup"><span data-stu-id="e0262-115">Clone, build, and run locally</span></span>

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

<span data-ttu-id="e0262-116">アプリケーションをテストするには、`curl` を呼び出すか、[ブラウザー](http://localhost:8080/api/hello)を使用してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="e0262-116">You can test the application by calling `curl` or visiting through a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a><span data-ttu-id="e0262-117">[Deploy to Azure (Azure へのデプロイ)]</span><span class="sxs-lookup"><span data-stu-id="e0262-117">Deploy to Azure</span></span>

<span data-ttu-id="e0262-118">ここで [Azure Container Instances] および [Azure Container Registry] サービスを使用して、このアプリケーションをクラウドに移行しましょう。</span><span class="sxs-lookup"><span data-stu-id="e0262-118">Now let's bring this application to the cloud using [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="e0262-119">Docker イメージをビルドする</span><span class="sxs-lookup"><span data-stu-id="e0262-119">Build a Docker image</span></span>

<span data-ttu-id="e0262-120">サンプル プロジェクトには、使用できる Docker ファイルが既に用意されています。</span><span class="sxs-lookup"><span data-stu-id="e0262-120">The sample project already provides a Dockerfile you can use.</span></span> <span data-ttu-id="e0262-121">ただし、クラウドでのイメージのビルドには Azure Container Registry Build 機能を使用するため、Docker をインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e0262-121">You don't need Docker installed though, as we will use Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="e0262-122">イメージをビルドして Azure で実行できるようにするには、次の手順に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0262-122">To build the image and be ready to run on Azure, you will have to follow these steps:</span></span>

1. <span data-ttu-id="e0262-123">Azure CLI でインストールおよびログインする</span><span class="sxs-lookup"><span data-stu-id="e0262-123">Install and log in with Azure CLI</span></span>
1. <span data-ttu-id="e0262-124">Azure リソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="e0262-124">Create an Azure Resource Group</span></span>
1. <span data-ttu-id="e0262-125">Azure Container Registry (ACR) を作成する</span><span class="sxs-lookup"><span data-stu-id="e0262-125">Create an Azure Container Registry (ACR)</span></span>
1. <span data-ttu-id="e0262-126">Docker イメージを構築する</span><span class="sxs-lookup"><span data-stu-id="e0262-126">Build the Docker image</span></span>
1. <span data-ttu-id="e0262-127">前に作成した ACR に Docker イメージを発行する</span><span class="sxs-lookup"><span data-stu-id="e0262-127">Publish the Docker image to the ACR created before</span></span>
1. <span data-ttu-id="e0262-128">(省略可能) 1 つのコマンドでビルドして ACR に発行する</span><span class="sxs-lookup"><span data-stu-id="e0262-128">(Optionally) Build and publish to ACR in one command</span></span>


#### <a name="set-up-azure-cli"></a><span data-ttu-id="e0262-129">Azure CLI をセットアップする</span><span class="sxs-lookup"><span data-stu-id="e0262-129">Set up Azure CLI</span></span>

<span data-ttu-id="e0262-130">Azure のサブスクリプションを所有していること、[Azure CLI がインストールされている](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)こと、およびお使いのアカウントにご自身が認証されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e0262-130">Make sure you have an Azure subscription, [Azure CLI installed](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you are authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="e0262-131">リソース グループを作成します</span><span class="sxs-lookup"><span data-stu-id="e0262-131">Create a Resource Group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="e0262-132">Azure Container Registry インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="e0262-132">Create an Azure Container Registry instance</span></span>

<span data-ttu-id="e0262-133">このコマンドによって、ランダムな数値を持つ基本的な名前を使用して、(うまくいけば) グローバルに一意のコンテナー レジストリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e0262-133">This command will create a globally unique (hopefully) container registry using a basic name with a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="e0262-134">Docker イメージを構築する</span><span class="sxs-lookup"><span data-stu-id="e0262-134">Build the Docker image</span></span>

<span data-ttu-id="e0262-135">Docker 自体を使用して Docker イメージをローカルで簡単にビルドできますが、クラウドでのビルドを検討する理由はいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="e0262-135">While you can easily build the Docker image locally using Docker itself, you may want to consider building it in the Cloud for few reasons:</span></span>

1. <span data-ttu-id="e0262-136">Docker をローカルにインストールする必要がない</span><span class="sxs-lookup"><span data-stu-id="e0262-136">No need to install Docker locally</span></span>
1. <span data-ttu-id="e0262-137">ビルドが別の場所で行われるため、はるかに高速 (コンテキストのアップロード時間を除く)</span><span class="sxs-lookup"><span data-stu-id="e0262-137">Much faster since build will happen elsewhere (except for context upload time)</span></span>
1. <span data-ttu-id="e0262-138">クラウドのプロセスでより高速なインターネットにアクセスできるため、ダウンロードがさらに速くなる</span><span class="sxs-lookup"><span data-stu-id="e0262-138">Process in the Cloud has access to faster Internet, therefore faster downloads</span></span>
1. <span data-ttu-id="e0262-139">イメージが Container Registry に直接移動する</span><span class="sxs-lookup"><span data-stu-id="e0262-139">Image goes directly into the Container Registry</span></span>

<span data-ttu-id="e0262-140">これらの理由により、[Azure Container Registry Build] 機能を使用してイメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e0262-140">Because of these reasons, we will build the image using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a><span data-ttu-id="e0262-141">Docker イメージを Azure Container Registry (ACR) から Azure Container Instances (ACI) にデプロイする</span><span class="sxs-lookup"><span data-stu-id="e0262-141">Deploy Docker Image from Azure Container Registry (ACR) into Container Instances (ACI)</span></span>

<span data-ttu-id="e0262-142">ACR でイメージを使用できるようになったので、次はコンテナー インスタンスを ACI にプッシュしてインスタンス化してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e0262-142">Now that the image is available on your ACR, let's push and instanciate a container instance on ACI.</span></span> <span data-ttu-id="e0262-143">ただし、最初に、ACR に認証できることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0262-143">But first, we need to make sure we can authenticate into the ACR:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="e0262-144">デプロイされた MicroProfile アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="e0262-144">Test Your Deployed MicroProfile Application</span></span>

<span data-ttu-id="e0262-145">これでご自身のアプリケーションが起動されて実行されます。</span><span class="sxs-lookup"><span data-stu-id="e0262-145">Your application should now be up and running.</span></span> <span data-ttu-id="e0262-146">コマンド ラインからこれをテストするには、次のコマンドを実行してみます。</span><span class="sxs-lookup"><span data-stu-id="e0262-146">To test it from the command-line, try the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="e0262-147">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="e0262-147">Congratulations!</span></span> <span data-ttu-id="e0262-148">MicroProfile アプリケーションを Docker コンテナーとして適切にビルドし、Microsoft Azure にデプロイできました。</span><span class="sxs-lookup"><span data-stu-id="e0262-148">You have successfuly built and deployed a MicroProfile application as a Docker container onto Microsoft Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0262-149">次の手順</span><span class="sxs-lookup"><span data-stu-id="e0262-149">Next steps</span></span>

<span data-ttu-id="e0262-150">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e0262-150">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* [<span data-ttu-id="e0262-151">Azure CLI から Azure へのログイン</span><span class="sxs-lookup"><span data-stu-id="e0262-151">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

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
[Java Development Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/index.html
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry