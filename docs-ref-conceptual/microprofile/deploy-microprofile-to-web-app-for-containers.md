---
title: Java ベースの MicroProfile サービスを Web App for Containers にデプロイする
description: Docker および Azure Web App for Containers を使用して MicroProfile サービスをデプロイする方法について説明します
services: container-registry;app-service
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: container-registry;app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 323ae247c3df8c7d7b180d9d60b9014e4e2d7382
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240962"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="2211a-103">Java ベースの MicroProfile サービスを Web App for Containers にデプロイする</span><span class="sxs-lookup"><span data-stu-id="2211a-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="2211a-104">[Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/) などのサービスにすばやく簡単にデプロイできる、非常に小さな Java アプリケーションを構築するには、MicroProfile が最適です。</span><span class="sxs-lookup"><span data-stu-id="2211a-104">MicroProfile is a great way to build exceedingly tiny Java applications that can be quickly and easily deployed to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="2211a-105">このチュートリアルでは、シンプルな MicroProfile ベースのマイクロサービスを作成します。このマイクロサービスは Docker コンテナーにコンテナー化され、[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) にデプロイされた後、Azure Web App for Containers を使ってホストされます。</span><span class="sxs-lookup"><span data-stu-id="2211a-105">In this tutorial we will create a simple MicroProfile-based microservice that is then containerized into a Docker container, deployed into an [Azure Container Registry](https://azure.microsoft.com/services/container-registry/), and then hosted using Azure Web App for Containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="2211a-106">この手順は、Docker コンテナー イメージが自己実行可能である場合 (つまり、ランタイムが含まれる場合) に、任意の MicroProfile.io の実装で機能します。</span><span class="sxs-lookup"><span data-stu-id="2211a-106">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

<span data-ttu-id="2211a-107">具体的には、このサンプルでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile 1.3](https://microprofile.io/) を使用して、小さな Java WAR ファイル (作成者マシン上では 5,085 バイト) を作成し、Docker イメージ (約 174 メガバイト) にパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="2211a-107">More concretely, this sample makes use of [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a tiny Java war file (5,085 bytes on the authors machine), and then packages it up into a Docker image (which is approximately 174 megabytes).</span></span> <span data-ttu-id="2211a-108">この Docker イメージには、この Web アプリの完全にコンテナー化されたデプロイに必要なすべてのものが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2211a-108">This Docker image contains everything necessary for a fully-containerised deployment of this webapp.</span></span>

<span data-ttu-id="2211a-109">Docker のしくみのせいで、多くの場合、アプリケーションのソース コードが変更されるたびに、174 メガバイトの Docker イメージ全体を再デプロイする必要はありません。これは、Docker によって (大幅に小さな) 差分のみがアップロードされるためです。</span><span class="sxs-lookup"><span data-stu-id="2211a-109">Because of the way Docker works, it is often the case that the entire 174 megabyte Docker image does not need to be redeployed whenever the application source code is changed, as Docker will only upload the differences (which is significantly smaller).</span></span> <span data-ttu-id="2211a-110">これにより、CI/CD パイプライン経由での新しい MicroProfile アプリケーション リリースの実行プロセスが非常に効率的かつ迅速になり、煩雑さが軽減され、開発の反復処理の高速化が実現します。</span><span class="sxs-lookup"><span data-stu-id="2211a-110">This makes the process of executing a new release of a MicroProfile application via a CI/CD pipeline extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="2211a-111">このチュートリアルでは、最初にローカルでコードを作成および実行し、その後、そのコードを Azure で Web アプリとしてデプロイします。</span><span class="sxs-lookup"><span data-stu-id="2211a-111">We will work through this tutorial firstly by creating and running the code locally, and then we will deploy this as a web app on Azure.</span></span> <span data-ttu-id="2211a-112">いずれの場合も、Docker を使用して、作業を簡略化および標準化します。</span><span class="sxs-lookup"><span data-stu-id="2211a-112">In both cases we will depend on Docker to simplify and standardize our efforts.</span></span> <span data-ttu-id="2211a-113">まずは、Docker コンテナーを格納する Azure Container Registry を作成します。</span><span class="sxs-lookup"><span data-stu-id="2211a-113">Before we begin, we will create an Azure Container Registry to store our Docker containers in.</span></span>

## <a name="creating-an-azure-container-registry"></a><span data-ttu-id="2211a-114">Azure Container Registry の作成</span><span class="sxs-lookup"><span data-stu-id="2211a-114">Creating an Azure Container Registry</span></span>

<span data-ttu-id="2211a-115">Azure Container Registry の作成には [Azure portal](http://portal.azure.com) を使用しますが、Azure CLI などの代替手段があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2211a-115">We will use the [Azure Portal](http://portal.azure.com) for creating the Azure Container Registry, but note that there are alternate choices such as the Azure CLI.</span></span> <span data-ttu-id="2211a-116">新しい Azure Container Registry を作成するには、次の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="2211a-116">Follow the steps below to create a new Azure Container Registry:</span></span>

1. <span data-ttu-id="2211a-117">[Azure portal](http://portal.azure.com) にログインし、新しい Azure Container Registry リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2211a-117">Log in to the [Azure Portal](http://portal.azure.com) and create a new Azure Container Registry resource.</span></span> <span data-ttu-id="2211a-118">レジストリの名前を指定します (これは `pom.xml` で `docker.registry` プロパティとして設定されている名前です)。</span><span class="sxs-lookup"><span data-stu-id="2211a-118">Provide a registry name (note that this is the name that should be set as the `docker.registry` property in `pom.xml`).</span></span> <span data-ttu-id="2211a-119">必要に応じて既定値を変更し、[作成] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2211a-119">Change the defaults as you wish, and then click 'create'.</span></span>

1. <span data-ttu-id="2211a-120">コンテナー レジストリがライブになったら ([作成] をクリックしてから約 30 秒かかります)、コンテナー レジストリをクリックし、左側のメニュー領域にある [アクセス キー] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2211a-120">Once the container registry is live (which is about 30 seconds after clicking 'create'), click on the container registry, and click on the 'Access keys' link in the left-menu area.</span></span> <span data-ttu-id="2211a-121">ここで、"管理者ユーザー" 設定を有効にする必要があります。これにより、(Docker コンテナーがプッシュされた) このコンテナー レジストリに、お使いのマシンからアクセスできるようになります。また、この後に設定する Azure Web App for Containers インスタンスからのアクセスも有効にします。</span><span class="sxs-lookup"><span data-stu-id="2211a-121">In here, you need to enable the 'admin user' setting, so that this container registry can be accessed from our machines (to push docker containers into), and also to enable access from the Azure Web Apps for Containers instance we will setup soon.</span></span>

1. <span data-ttu-id="2211a-122">[アクセス キー] 領域では、`username` 値と `password` 値に気を付けてください。</span><span class="sxs-lookup"><span data-stu-id="2211a-122">Whilst you are in the 'Access keys' area, note the `username` and `password` values.</span></span> <span data-ttu-id="2211a-123">これらをコピーして、グローバル Maven `settings.xml` ファイルに貼り付けます (Maven 設定の詳細については、[Apache Maven プロジェクト](https://maven.apache.org/settings.html)の Web サイトをご覧ください)。</span><span class="sxs-lookup"><span data-stu-id="2211a-123">We will copy / paste these into our global Maven `settings.xml` file  (for more information on Maven settings, refer to the [Apache Maven Project](https://maven.apache.org/settings.html) website).</span></span> <span data-ttu-id="2211a-124">参照用として、この作成者システムには `${user.home}/.m2/settings.xml` ファイルの難読化バージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="2211a-124">For reference, here is an obfuscated version of the `${user.home}/.m2/settings.xml` file on the authors system:</span></span>

    ```xml
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
          <server>
            <id>jogilescr.azurecr.io</id>
            <username>jogilescr</username>
            <password>ojoirshois.this-isn't-real.hrihslirhlishrglih</password>
          </server>
        </servers>
    </settings>
    ```

<span data-ttu-id="2211a-125">これが完了したら、次は MicroProfile アプリケーションをローカルで構築して実行します。</span><span class="sxs-lookup"><span data-stu-id="2211a-125">Now that this is complete, we can move on with building and running our MicroProfile application locally.</span></span>

## <a name="creating-our-microprofile-application"></a><span data-ttu-id="2211a-126">MicroProfile アプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="2211a-126">Creating our MicroProfile application</span></span>

<span data-ttu-id="2211a-127">この例は、GitHub で入手できるサンプル アプリケーションに基づいています。そこで、この例を複製して、コードをステップ実行します。</span><span class="sxs-lookup"><span data-stu-id="2211a-127">This example is based on a sample application available on GitHub, so we will clone that and then step through the code.</span></span> <span data-ttu-id="2211a-128">お使いのマシンにコードを複製するには、次の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="2211a-128">Follow the steps below to get the code cloned onto your machine:</span></span>

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

<span data-ttu-id="2211a-129">このディレクトリには `pom.xml` ファイルがあります。このファイルは、Maven ビルド ツールで使用される形式でプロジェクトを指定するときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="2211a-129">In this directory there is a `pom.xml` file that is used to specify the project in the format used by the Maven build tool.</span></span> <span data-ttu-id="2211a-130">このファイルは、独自のニーズに合わせて編集できます。</span><span class="sxs-lookup"><span data-stu-id="2211a-130">This file can be edited to suit your own needs.</span></span> <span data-ttu-id="2211a-131">具体的には、`docker.registry` プロパティと `docker.name` プロパティは、Azure Container Registry の設定時に作成された `docker.registry` と `docker.name` に変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2211a-131">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` created when the Azure Container Registry was setup.</span></span>

<span data-ttu-id="2211a-132">また、このディレクトリの Dockerfile にも注目します。これを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="2211a-132">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="2211a-133">この Dockerfile では、Payara Micro Docker コンテナーに基づいて新しい Docker コンテナーが作成され、ビルド プロセスの一環として作成された .war ファイルにコピーされるだけです。</span><span class="sxs-lookup"><span data-stu-id="2211a-133">This Dockerfile simply creates a new Docker container based on the Payara Micro Docker Container, and copies in the .war file that is created as part of our build process.</span></span> <span data-ttu-id="2211a-134">また、Docker コンテナーで実行された時点でサービスにアクセスできるように、ポート 8080 が公開されます。</span><span class="sxs-lookup"><span data-stu-id="2211a-134">It also exposes port 8080 so that we may access the service once it is up and running within a Docker container.</span></span>

<span data-ttu-id="2211a-135">`src` ディレクトリを詳しく見ていくと、最終的には以下のように `Application` クラスが見つかります。</span><span class="sxs-lookup"><span data-stu-id="2211a-135">Diving into the `src` directory, we will eventually discover the `Application` class reproduced below:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="2211a-136">`@ApplicationPath("/api")` 注釈により、このマイクロサービス用の基本エンドポイントが指定されます。つまり、すべてのエンドポイントで、特定の REST エンドポイントへのアクセスに必要な残りの URL の前に `/api` が付きます。</span><span class="sxs-lookup"><span data-stu-id="2211a-136">The `@ApplicationPath("/api")` annotation specifies the base endpoint for this microservice - that is, that all endpoints will have `/api` preceed the rest of the URL required to access any specific REST endpoint.</span></span>

<span data-ttu-id="2211a-137">`api` パッケージ内には `API` という名前のクラスがあり、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2211a-137">Inside the `api` package is a class named `API`, which contains the following code:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld.api;

import javax.enterprise.context.ApplicationScoped;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

import static javax.ws.rs.core.MediaType.TEXT_HTML;

@ApplicationScoped
@Path("/")
public class API {

    @GET
    @Path("/helloworld")
    @Produces(TEXT_HTML)
    public String info() {
        return "Hello, world!";
    }
}
```

<span data-ttu-id="2211a-138">`@Path("/helloworld")` 注釈を使用すると、この REST エンドポイントは、`Application` クラスで指定された `/api` と組み合わされた場合に、`/api/helloworld` になることがわかります。</span><span class="sxs-lookup"><span data-stu-id="2211a-138">Through the use of the `@Path("/helloworld")` annotation, we can see that this REST endpoint, when combined with the `/api` specified in the `Application` class, will be `/api/helloworld`.</span></span> <span data-ttu-id="2211a-139">このエンドポイントが HTTP GET 要求を使用して呼び出されると、メソッドによって text/html が生成されることを確認できますが、実際のところ、これは単にハード コーディングされた文字列 "Hello, world!" にすぎません。</span><span class="sxs-lookup"><span data-stu-id="2211a-139">When this endpoint is called using an HTTP GET request, we can see that the method will produce text/html, and in fact it is simply a hard-coded string "Hello, world!".</span></span>

<span data-ttu-id="2211a-140">MicroProfile を使用してマイクロサービスを作成するのに必要なコードについては、すべて説明しました。</span><span class="sxs-lookup"><span data-stu-id="2211a-140">We have now covered all the code required to create a microservice using MicroProfile.</span></span> <span data-ttu-id="2211a-141">これで Maven を使用して作成し、Docker コンテナーにコンテナー化して、ローカルで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="2211a-141">We can now use Maven to build it, containerize it into a Docker container, and run it locally.</span></span> <span data-ttu-id="2211a-142">これを行うには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="2211a-142">We can do that with the following steps:</span></span>

1. <span data-ttu-id="2211a-143">`mvn clean package` を実行し、正常に完了するまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="2211a-143">Run `mvn clean package` and wait until it successfully completes.</span></span>

1. <span data-ttu-id="2211a-144">`docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest` を実行します。たとえば、`docker.registry` が `jogilescr.azurecr.io`、`docker.name` が `samples/docker-helloworld` の場合は `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest` となります。</span><span class="sxs-lookup"><span data-stu-id="2211a-144">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`, for example, `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`, if your `docker.registry` is `jogilescr.azurecr.io` and `docker.name` is `samples/docker-helloworld`.</span></span>

1. <span data-ttu-id="2211a-145">Web ブラウザーで [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) および [http://localhost:8080/health](http://localhost:8080/health) にアクセスしてみます。</span><span class="sxs-lookup"><span data-stu-id="2211a-145">Try accessing [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="2211a-146">想定どおりの "Hello, world!" </span><span class="sxs-lookup"><span data-stu-id="2211a-146">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="2211a-147">応答 (および [/health](http://localhost:8080/health) エンドポイントの正常性に関する情報) が表示された場合、MicroProfile アプリケーションはローカル コンピューターに正常に展開されています。</span><span class="sxs-lookup"><span data-stu-id="2211a-147">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you have successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="pushing-to-the-azure-container-registry"></a><span data-ttu-id="2211a-148">Azure Container Registry へのプッシュ</span><span class="sxs-lookup"><span data-stu-id="2211a-148">Pushing to the Azure Container Registry</span></span>

<span data-ttu-id="2211a-149">MicroProfile アプリケーションが正常に作成され、ローカル コンピューターで実行されました。次は、このコンテナーをコンテナー レジストリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="2211a-149">Now that we have successfully built and run our MicroProfile application on our local machine, the next step is to push this container into our container registry.</span></span> <span data-ttu-id="2211a-150">このチュートリアルでは Azure Container Registry を使用していますが、(関連する場所をポイントするように `pom.xml` ファイルを編集できれば) 任意のコンテナー レジストリを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2211a-150">In this tutorial we are using the Azure Container Registry, but any container registry will work (as long as the `pom.xml` file is edited to point to the relevant location).</span></span>

1. <span data-ttu-id="2211a-151">`mvn clean package` を実行して、ローカル Docker イメージをクリーニング、コンパイル、および作成します。</span><span class="sxs-lookup"><span data-stu-id="2211a-151">Run `mvn clean package` to clean, compile, and create a local docker image.</span></span>
2. <span data-ttu-id="2211a-152">`mvn dockerfile:push` を実行して、Azure Container Repository にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="2211a-152">Run `mvn dockerfile:push` to push to the Azure Container Repository.</span></span>

<span data-ttu-id="2211a-153">この時点で、ご自身の Docker コンテナー イメージは Azure Container Registry にアップロードされていますが、Azure Web App for Containers インスタンスにデプロイする必要があるため、まだ実行されていません。</span><span class="sxs-lookup"><span data-stu-id="2211a-153">At this stage you now have your docker container image uploaded to the Azure Container Registry, but it is not yet running as we now have to deploy it into an Azure Web App for Containers instance.</span></span> <span data-ttu-id="2211a-154">次は、この操作を行います。</span><span class="sxs-lookup"><span data-stu-id="2211a-154">We will now do that.</span></span>

## <a name="creating-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="2211a-155">Web App for Containers インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="2211a-155">Creating an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="2211a-156">[Azure portal](http://portal.azure.com) に戻り、(メニューの [Web + モバイル] 見出しの下で) 新しい Web App for Containers インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2211a-156">Return to the [Azure Portal](http://portal.azure.com) and create a new Web App for Containers instance (located under the 'Web + Mobile' heading in the menu).</span></span> <span data-ttu-id="2211a-157">ポイントがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="2211a-157">A few pointers:</span></span>

   1. <span data-ttu-id="2211a-158">ここで指定する名前は Web アプリのパブリック URL になります (ただし、必要に応じて、後でカスタム ドメインを追加できます)。このため、覚えやすい名前を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2211a-158">The name you specify here will be the public URL of the web app (although a custom domain can be added later if desired), so it is a good idea to pick a name that you can easily remember.</span></span>

   1. <span data-ttu-id="2211a-159">[コンテナーの構成] セクションに移動したら、[イメージ ソース] として [Azure Container Registry] を選択し、ドロップダウン リストから適切なイメージを選択できます。</span><span class="sxs-lookup"><span data-stu-id="2211a-159">When you get to the 'Configure container' section, you can select 'Azure Container Registry' for the 'Image source', and then select the correct image from the drop-down lists.</span></span>

   1. <span data-ttu-id="2211a-160">[スタートアップ ファイル] フィールドに値を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2211a-160">You do not need to specify any value in the 'Startup File' field.</span></span>

1. <span data-ttu-id="2211a-161">インスタンスが作成されたら (ここでも非常に迅速です)、そのインスタンスをクリックし、[アプリケーション設定] メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2211a-161">Once the instance is created (again, it is very quick), click on it and then click on the 'Application Settings' menu item.</span></span> <span data-ttu-id="2211a-162">ここでは、新しいアプリケーション設定を追加する必要があります。ここで、キーは `WEBSITES_PORT`、値は `8080` です。</span><span class="sxs-lookup"><span data-stu-id="2211a-162">In here you need to add a new application setting, where the key is `WEBSITES_PORT` and the value is `8080`.</span></span> <span data-ttu-id="2211a-163">これにより、コンテナーで公開するポートが Azure に通知され、外部的にはポート 80 にマップされます。</span><span class="sxs-lookup"><span data-stu-id="2211a-163">This tells Azure which port you want to expose in the container, and it will be mapped to port 80 externally.</span></span>

1. <span data-ttu-id="2211a-164">必要に応じて、[Docker コンテナー] リンクをクリックし、[継続的なデプロイ] を有効にします。これにより、Azure Container Registry イメージを更新するたびに、Azure Web App for Containers インスタンスでも自動的に更新されるようになります。</span><span class="sxs-lookup"><span data-stu-id="2211a-164">Optionally, click on the 'Docker Container' link, and enable 'Continuous Deployment', so that whenever you update the Azure Container Registry image it is automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="2211a-165">これで、Azure でホストされるインスタンスに `http://<appname>.azurewebsites.net/microprofile/api/helloworld` および `http://<appname>.azurewebsites.net/health` でアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="2211a-165">You should be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="2211a-166">まとめ</span><span class="sxs-lookup"><span data-stu-id="2211a-166">Summary</span></span>

<span data-ttu-id="2211a-167">このチュートリアルでは、シンプルな MicroProfile ベースのマイクロサービスを作成し、Docker コンテナーにコンテナー化した後、ローカルで実行し、Azure に発行するプロセスについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="2211a-167">Through this tutorial we have stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, and we have run it locally and published it to Azure.</span></span> <span data-ttu-id="2211a-168">マイクロサービスを拡張して、より便利な機能を提供する方法については、このチュートリアルの範囲外ですが、インターネット上には MicroProfile に関するチュートリアルとアドバイスが豊富に用意されています。[MicroProfile.io](https://microprofile.io/) の詳細が記載されたコンテンツをご覧になることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2211a-168">Extending our microservice to provide more useful functionality is outside the scope of this tutorial, but there is a wealth of tutorials and advice on MicroProfile on the internet, and readers are encouraged to review [MicroProfile.io](https://microprofile.io/) for more content.</span></span>