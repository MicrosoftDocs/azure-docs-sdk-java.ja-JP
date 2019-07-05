---
title: Docker と Azure を使用して MicroProfile アプリをクラウドにデプロイする
description: Docker および Azure Container Instances を使用して MicroProfile アプリをクラウドにデプロイする方法について説明します。
services: container-instances;container-registry
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
ms.openlocfilehash: 6ba12bb183969103676fa988199603df6cf36bba
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533604"
---
# <a name="deploy-a-microprofile-app-to-the-cloud-by-using-docker-and-azure"></a><span data-ttu-id="aadb0-103">Docker と Azure を使用して MicroProfile アプリをクラウドにデプロイする</span><span class="sxs-lookup"><span data-stu-id="aadb0-103">Deploy a MicroProfile app to the cloud by using Docker and Azure</span></span>

<span data-ttu-id="aadb0-104">この記事では、[MicroProfile.io] アプリケーションを Docker コンテナーにパックし、Azure Container Instances で実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
> <span data-ttu-id="aadb0-105">この手順は、Docker コンテナー イメージが自己実行可能である (つまり、イメージにランタイムが含まれている) 限り、任意の MicroProfile.io の実装で機能します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-105">This procedure works with any implementation of MicroProfile.io, as long as the Docker container image is self-executable (that is, the image includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aadb0-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="aadb0-106">Prerequisites</span></span>

<span data-ttu-id="aadb0-107">このチュートリアルを完了するには、次の前提条件を用意しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="aadb0-107">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="aadb0-108">Azure サブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="aadb0-108">An Azure subscription.</span></span> <span data-ttu-id="aadb0-109">Azure サブスクリプションをまだ取得していない場合は、[無料の Azure アカウント]にサインアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="aadb0-109">If you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="aadb0-110">[Azure CLI] がインストールされていること。</span><span class="sxs-lookup"><span data-stu-id="aadb0-110">The [Azure CLI], installed.</span></span>
* <span data-ttu-id="aadb0-111">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="aadb0-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="aadb0-112">Azure で開発するときに使用可能な JDK の詳細については、「[Azure および Azure Stack の Java 長期サポート](https://aka.ms/azure-jdks)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aadb0-112">For more information about the JDKs that are available to use when you develop on Azure, see [Java long-term support for Azure and Azure Stack](https://aka.ms/azure-jdks).</span></span>
* <span data-ttu-id="aadb0-113">[Apache Maven] ビルド ツール (バージョン 3 以降)。</span><span class="sxs-lookup"><span data-stu-id="aadb0-113">The [Apache Maven] build tool (version 3 or later).</span></span>
* <span data-ttu-id="aadb0-114">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="aadb0-114">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="aadb0-115">MicroProfile Hello Azure サンプル</span><span class="sxs-lookup"><span data-stu-id="aadb0-115">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="aadb0-116">この記事では、[MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) サンプルを使用します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-116">In this article, you use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample.</span></span> <span data-ttu-id="aadb0-117">それを次のコマンドを使用して複製、ビルドしてローカルに実行します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-117">Clone, build, and run it locally by using the following commands:</span></span>

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

<span data-ttu-id="aadb0-118">`curl` を呼び出すか、[ブラウザー](http://localhost:8080/api/hello)を使用してアプリケーションをテストできます。</span><span class="sxs-lookup"><span data-stu-id="aadb0-118">You can test the application by calling `curl` or by using a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="aadb0-119">Azure にアプリケーションをデプロイする</span><span class="sxs-lookup"><span data-stu-id="aadb0-119">Deploy the app to Azure</span></span>

<span data-ttu-id="aadb0-120">ここで [Azure Container Instances] および [Azure Container Registry] サービスを使用して、このアプリケーションを Azure に移行しましょう。</span><span class="sxs-lookup"><span data-stu-id="aadb0-120">Now bring this application to Azure by using the [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="aadb0-121">Docker イメージをビルドする</span><span class="sxs-lookup"><span data-stu-id="aadb0-121">Build a Docker image</span></span>

<span data-ttu-id="aadb0-122">サンプル プロジェクトには、使用できる Dockerfile が用意されています。</span><span class="sxs-lookup"><span data-stu-id="aadb0-122">The sample project provides a Dockerfile that you can use.</span></span> <span data-ttu-id="aadb0-123">ただし、クラウドでのイメージのビルドには Azure Container Registry Build 機能を使用するため、Docker をインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="aadb0-123">You don't need to have Docker installed, though, because you'll use the Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="aadb0-124">イメージをビルドしてそれを Azure で実行するための準備をするには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="aadb0-124">To build the image and prepare to run it on Azure, do the following:</span></span>

1. <span data-ttu-id="aadb0-125">Azure CLI をインストールしてサインインします。</span><span class="sxs-lookup"><span data-stu-id="aadb0-125">Install and sign in to the Azure CLI.</span></span>
1. <span data-ttu-id="aadb0-126">Azure リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-126">Create an Azure resource group.</span></span>
1. <span data-ttu-id="aadb0-127">Azure コンテナー レジストリ インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-127">Create an Azure container registry instance.</span></span>
1. <span data-ttu-id="aadb0-128">Docker イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="aadb0-128">Build a Docker image.</span></span>
1. <span data-ttu-id="aadb0-129">Docker イメージを以前に作成したコンテナー レジストリ インスタンスに発行します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-129">Publish the Docker image to the previously created container registry instance.</span></span>
1. <span data-ttu-id="aadb0-130">(省略可能) 1 つのコマンドでイメージをビルドして、コンテナー レジストリ インスタンスに発行します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-130">(Optional) Build and publish the image to the container registry instance in one command.</span></span>


#### <a name="set-up-the-azure-cli"></a><span data-ttu-id="aadb0-131">Azure CLI のセットアップ</span><span class="sxs-lookup"><span data-stu-id="aadb0-131">Set up the Azure CLI</span></span>

<span data-ttu-id="aadb0-132">Azure のサブスクリプションを所有していること、[Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) がインストールされていること、お使いのアカウントでご自身が認証されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-132">Make sure that you have an Azure subscription, that you've installed [the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you're authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="aadb0-133">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="aadb0-133">Create a resource group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-a-container-registry-instance"></a><span data-ttu-id="aadb0-134">コンテナー レジストリ インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="aadb0-134">Create a container registry instance</span></span>

<span data-ttu-id="aadb0-135">このコマンドによって、基本的な名前とランダムな数値を使用して、グローバルに一意のコンテナー レジストリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="aadb0-135">This command should create a globally unique container registry instance with a basic name and a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="aadb0-136">Docker イメージを構築する</span><span class="sxs-lookup"><span data-stu-id="aadb0-136">Build the Docker image</span></span>

<span data-ttu-id="aadb0-137">Docker イメージは、Docker そのものを使用してローカルで簡単にビルドできますが、クラウドでのビルドを検討する理由はいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="aadb0-137">Although you can easily build the Docker image locally by using Docker itself, you might consider building it in the cloud for few reasons:</span></span>

* <span data-ttu-id="aadb0-138">Docker をローカルにインストールする必要がない。</span><span class="sxs-lookup"><span data-stu-id="aadb0-138">You don't have to install Docker locally.</span></span>
* <span data-ttu-id="aadb0-139">ビルドが別の場所で行われるため、はるかに高速 (コンテキストのアップロード時間を除く)。</span><span class="sxs-lookup"><span data-stu-id="aadb0-139">It's much faster, because the build happens elsewhere (except for context upload time).</span></span>
* <span data-ttu-id="aadb0-140">クラウド内のプロセスはより高速にインターネットにアクセスできるため、ダウンロードがさらに速くなる。</span><span class="sxs-lookup"><span data-stu-id="aadb0-140">The process in the cloud has faster access to the internet and, therefore, faster downloads.</span></span>
* <span data-ttu-id="aadb0-141">イメージがコンテナー レジストリ インスタンスに直接移動する。</span><span class="sxs-lookup"><span data-stu-id="aadb0-141">The image goes directly into the container registry instance.</span></span>

<span data-ttu-id="aadb0-142">これらの理由により、[Azure Container Registry Build] 機能を使用してイメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="aadb0-142">For these reasons, you build the image by using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-the-docker-image-from-the-azure-container-registry-instance-to-container-instances"></a><span data-ttu-id="aadb0-143">Docker イメージを Azure コンテナー レジストリ インスタンスから Container Instances にデプロイする</span><span class="sxs-lookup"><span data-stu-id="aadb0-143">Deploy the Docker image from the Azure container registry instance to Container Instances</span></span>

<span data-ttu-id="aadb0-144">これで、イメージがお使いのコンテナー レジストリ インスタンスで使用できるようになったので、Container Instances でコンテナー インスタンスをプッシュしてインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-144">Now that the image is available on your container registry instance, push and instantiate a container instance on Container Instances.</span></span> <span data-ttu-id="aadb0-145">しかし、まずコンテナー レジストリ インスタンスに認証できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-145">But first, make sure that you can authenticate into the container registry instance:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="aadb0-146">デプロイされた MicroProfile アプリケーションのテスト</span><span class="sxs-lookup"><span data-stu-id="aadb0-146">Test your deployed MicroProfile application</span></span>

<span data-ttu-id="aadb0-147">これでご自身のアプリケーションが起動されて実行されます。</span><span class="sxs-lookup"><span data-stu-id="aadb0-147">Your application should now be up and running.</span></span> <span data-ttu-id="aadb0-148">コマンド ライン インターフェイスからこれをテストするには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="aadb0-148">To test it from the command-line interface, use the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="aadb0-149">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="aadb0-149">Congratulations!</span></span> <span data-ttu-id="aadb0-150">MicroProfile アプリケーションを Docker コンテナーとして適切にビルドし、Azure にデプロイできました。</span><span class="sxs-lookup"><span data-stu-id="aadb0-150">You've successfully built a MicroProfile application as a Docker container and deployed it to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aadb0-151">次の手順</span><span class="sxs-lookup"><span data-stu-id="aadb0-151">Next steps</span></span>

<span data-ttu-id="aadb0-152">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="aadb0-152">For more information about the various technologies discussed in this article, see:</span></span>

* [<span data-ttu-id="aadb0-153">Azure CLI を使用して Azure にサインインする</span><span class="sxs-lookup"><span data-stu-id="aadb0-153">Sign in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure CLI]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Apache Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry
