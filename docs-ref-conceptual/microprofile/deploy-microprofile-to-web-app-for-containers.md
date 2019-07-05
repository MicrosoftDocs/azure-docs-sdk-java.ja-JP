---
title: Java ベースの MicroProfile サービスを Azure Web App for Containers にデプロイする
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
ms.openlocfilehash: 4ecfbf92bc30bc491c991e30111cce8375da7f1a
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533598"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="d217c-103">Java ベースの MicroProfile サービスを Azure Web App for Containers にデプロイする</span><span class="sxs-lookup"><span data-stu-id="d217c-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="d217c-104">[Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/) などのサービスにすばやく簡単にデプロイできる、小さな Java アプリケーションを構築するには、MicroProfile が最適です。</span><span class="sxs-lookup"><span data-stu-id="d217c-104">MicroProfile is a great way to build exceedingly small Java applications that you can quickly and easily deploy to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="d217c-105">このチュートリアルでは、Docker コンテナーにコンテナー化するシンプルな MicroProfile ベースのマイクロサービスを作成し、[Azure コンテナー レジストリ インスタンス](https://azure.microsoft.com/services/container-registry/) にデプロイした後、Azure Web App for Containers を使ってホストします。</span><span class="sxs-lookup"><span data-stu-id="d217c-105">In this tutorial, you create a simple, MicroProfile-based microservice that you containerize into a Docker container, deploy to an [Azure container registry instance](https://azure.microsoft.com/services/container-registry/), and then host by using Azure Web App for Containers.</span></span>

> [!NOTE]
> <span data-ttu-id="d217c-106">この手順は、Docker コンテナー イメージが自己実行可能である場合 (つまり、イメージにランタイムが含まれている場合) に、任意の MicroProfile.io の実装で機能します。</span><span class="sxs-lookup"><span data-stu-id="d217c-106">This procedure works with any implementation of MicroProfile.io, as long the Docker container image is self-executable (that is, the image includes the runtime).</span></span>

<span data-ttu-id="d217c-107">このサンプルでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile 1.3](https://microprofile.io/) を使用して、小規模な (約 5,000 バイト) Java web アプリ要件 (WAR) ファイルを作成し、それを約 174 メガバイト (MB) の Docker イメージにパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="d217c-107">In this sample, you use [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a small (approximately 5,000 bytes) Java web application requirement (WAR) file, and then package it into a Docker image of approximately 174 megabytes (MB).</span></span> <span data-ttu-id="d217c-108">この Docker イメージには、Web アプリの完全にコンテナー化されたデプロイに必要なすべてのものが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d217c-108">The Docker image contains everything necessary for a fully containerized deployment of the web app.</span></span>

<span data-ttu-id="d217c-109">アプリケーションのソース コードが変更されるたびに、174 MB の Docker イメージ全体を再デプロイする必要はありません。Docker によって (ずっと小さい) 差分のみがアップロードされるためです。</span><span class="sxs-lookup"><span data-stu-id="d217c-109">An entire 174 MB Docker image doesn't need to be redeployed whenever the application source code is changed, because Docker uploads only the differences (which are significantly smaller).</span></span> <span data-ttu-id="d217c-110">その結果、CI/CD パイプライン経由での新しい MicroProfile アプリケーション リリースの実行プロセスが非常に効率的かつ迅速になり、煩雑さが軽減され、開発の反復処理の高速化が実現します。</span><span class="sxs-lookup"><span data-stu-id="d217c-110">Consequently, the process of executing a new release of a MicroProfile application via a CI/CD pipeline is extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="d217c-111">このチュートリアルでは、最初にローカルでコードを作成して実行し、その後、そのコードを Azure で Web アプリとしてデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d217c-111">You'll work through this tutorial by first creating and running the code locally and then deploying it as a web app on Azure.</span></span> <span data-ttu-id="d217c-112">いずれのフェーズでも、Docker を使用して、作業を簡略化および標準化できます。</span><span class="sxs-lookup"><span data-stu-id="d217c-112">In both phases, you can depend on Docker to simplify and standardize your efforts.</span></span> <span data-ttu-id="d217c-113">開始する前に、Docker コンテナーを格納するための Azure コンテナー レジストリ インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d217c-113">Before you begin, you'll create an Azure container registry instance for storing your Docker containers.</span></span>

## <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="d217c-114">Azure コンテナー レジストリ インスタンスを作成する</span><span class="sxs-lookup"><span data-stu-id="d217c-114">Create an Azure container registry instance</span></span>

<span data-ttu-id="d217c-115">Azure コンテナー レジストリ インスタンスは [Azure portal](http://portal.azure.com) を使用して作成できますが、Azure CLI など他の選択肢もあります。</span><span class="sxs-lookup"><span data-stu-id="d217c-115">You can use the [Azure portal](http://portal.azure.com) to create the Azure container registry instance, but there are other choices also, such as the Azure CLI.</span></span> <span data-ttu-id="d217c-116">新しい Azure コンテナー レジストリ インスタンスを作成するには、次の手順を行ってください。</span><span class="sxs-lookup"><span data-stu-id="d217c-116">To create a new Azure container registry instance, do the following:</span></span>

1. <span data-ttu-id="d217c-117">[Azure portal](http://portal.azure.com) にサインインし、新しい Azure コンテナー レジストリ リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="d217c-117">Sign in to the [Azure portal](http://portal.azure.com) and create a new Azure container registry resource.</span></span> <span data-ttu-id="d217c-118">レジストリの名前を指定します (この名前は *pom.xml* ファイルの `docker.registry` プロパティとして設定されています)。</span><span class="sxs-lookup"><span data-stu-id="d217c-118">Provide a registry name (this name should be set as the `docker.registry` property in the *pom.xml* file).</span></span> <span data-ttu-id="d217c-119">必要に応じて既定値を変更し、 **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d217c-119">Change the defaults as you want, and then select **Create**.</span></span>

1. <span data-ttu-id="d217c-120">約 30 秒で、コンテナー レジストリ インスタンスがライブになったら、コンテナー レジストリ インスタンスを選択し、左側のウィンドウで、 **[アクセス キー]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="d217c-120">In about 30 seconds, when the container registry instance is live, select the container registry instance and then, in the left pane, select the **Access keys** link.</span></span> 

    <span data-ttu-id="d217c-121">お使いのコンピューターからこのコンテナー レジストリ インスタンスにアクセスして Docker コンテナーをプッシュできるように、*管理者ユーザー*設定が有効になっていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d217c-121">Be sure to enable the *admin user* setting, so that you can access this container registry instance from your machines and push Docker containers into it.</span></span> <span data-ttu-id="d217c-122">そうすることで、この後設定する Azure Web App for Containers インスタンスからもアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="d217c-122">Doing so also enables access from the Azure Web App for Containers instance that you'll set up soon.</span></span>

1. <span data-ttu-id="d217c-123">**[アクセス キー]** ウィンドウで、**ユーザー名**と**パスワード**の値をコピーして、グローバル Maven *settings.xml* ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d217c-123">In the **Access keys** pane, copy the **username** and **password** values and paste them into your global Maven *settings.xml* file.</span></span> <span data-ttu-id="d217c-124">Maven 設定の詳細については、[Apache Maven Project](https://maven.apache.org/settings.html) の Web サイトをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d217c-124">For more information about Maven settings, go to the [Apache Maven Project](https://maven.apache.org/settings.html) website.</span></span> 

   <span data-ttu-id="d217c-125">参考のため、 *${user.home}/.m2/settings.xml* ファイルの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d217c-125">For your reference, here's an example of the *${user.home}/.m2/settings.xml* file:</span></span>

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

<span data-ttu-id="d217c-126">コンテナー レジストリ インスタンスを作成したので、MicroProfile アプリケーションのローカルでのビルドと実行に移ることができます。</span><span class="sxs-lookup"><span data-stu-id="d217c-126">Now that you've created your container registry instance, you can move on to building and running your MicroProfile application locally.</span></span>

## <a name="create-your-microprofile-application"></a><span data-ttu-id="d217c-127">MicroProfile アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="d217c-127">Create your MicroProfile application</span></span>

<span data-ttu-id="d217c-128">このコード例は、GitHub で入手可能なサンプル アプリケーションに基づいています。</span><span class="sxs-lookup"><span data-stu-id="d217c-128">This example code is based on a sample application that's available on GitHub.</span></span> <span data-ttu-id="d217c-129">コンピューターにコードを複製するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d217c-129">To clone the code onto your machine, enter the following commands:</span></span>

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

<span data-ttu-id="d217c-130">このディレクトリには、Maven ビルド ツールで使用されている形式でプロジェクトを指定する *pom.xml* ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d217c-130">This directory contains a *pom.xml* file that you use to specify the project in the format that's used by the Maven build tool.</span></span> <span data-ttu-id="d217c-131">このファイルは各自のニーズに合わせて編集できます。</span><span class="sxs-lookup"><span data-stu-id="d217c-131">You can edit the file to suit your own needs.</span></span> <span data-ttu-id="d217c-132">具体的には、`docker.registry` プロパティと `docker.name` プロパティを、Azure コンテナー レジストリ インスタンスの設定時に作成された `docker.registry` プロパティと `docker.name` プロパティに変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d217c-132">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` properties that were created when you set up the Azure container registry instance.</span></span>

<span data-ttu-id="d217c-133">また、このディレクトリの Dockerfile にも注目します。これを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="d217c-133">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="d217c-134">Dockerfile によって、Payara Micro Docker コンテナーに基づく新しい Docker コンテナーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d217c-134">The Dockerfile creates a new Docker container that's based on the Payara Micro Docker Container.</span></span> <span data-ttu-id="d217c-135">これはビルド プロセスの一部として作成された WAR ファイルでコピーされます。</span><span class="sxs-lookup"><span data-stu-id="d217c-135">It copies in the WAR file that was created as part of your build process.</span></span> <span data-ttu-id="d217c-136">また、サービスが Docker コンテナー内で実行されるようになったら、サービスにアクセスできるようにポート 8080 も公開されます。</span><span class="sxs-lookup"><span data-stu-id="d217c-136">It also exposes port 8080 so that you can access the service after it's up and running within a Docker container.</span></span>

<span data-ttu-id="d217c-137">*src* ディレクトリを開くときに、最終的にはここに再現されている `Application` クラスが見つかります。</span><span class="sxs-lookup"><span data-stu-id="d217c-137">When you open the *src* directory, you'll eventually discover the `Application` class that's reproduced here:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="d217c-138">*@ApplicationPath("/api")* 注釈は、マイクロサービスのベース エンドポイントを指定します。</span><span class="sxs-lookup"><span data-stu-id="d217c-138">The *@ApplicationPath("/api")* annotation specifies the base endpoint for this microservice.</span></span> <span data-ttu-id="d217c-139">つまり、すべてのエンドポイントに対し、特定の REST エンドポイントにアクセスするために必要な URL の残りの部分より前に */api* を配置します。</span><span class="sxs-lookup"><span data-stu-id="d217c-139">That is, for all endpoints, */api* precedes the rest of the URL that's required to access any specific REST endpoint.</span></span>

<span data-ttu-id="d217c-140">*api* パッケージ内には `API` という名前のクラスがあり、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d217c-140">Inside the *api* package is a class named `API`, which contains the following code:</span></span>

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

<span data-ttu-id="d217c-141">*@Path("/helloworld")* 注釈を使用すると、この REST エンドポイントは、`Application` クラスで指定された */api* と組み合わされた場合に、 */api/helloworld* になることがわかります。</span><span class="sxs-lookup"><span data-stu-id="d217c-141">Through the use of the *@Path("/helloworld")* annotation, you can see that this REST endpoint, when it's combined with the */api* that's specified in the `Application` class, will be */api/helloworld*.</span></span> <span data-ttu-id="d217c-142">HTTP GET 要求を使用してこのエンドポイントを呼び出すと、メソッドによって text/html が生成されることを確認できますが、実際のところ、これは単にハード コーディングされた文字列 "Hello, world!" にすぎません。</span><span class="sxs-lookup"><span data-stu-id="d217c-142">When you call this endpoint by using an HTTP GET request, you can see that the method produces text/html and, in fact, it's simply a hard-coded string, "Hello, world!"</span></span>

<span data-ttu-id="d217c-143">ここまで、この記事では、MicroProfile を使用してマイクロサービスを作成するために必要なすべてのコードについて説明してきました。</span><span class="sxs-lookup"><span data-stu-id="d217c-143">So far, this article has covered all the code that's required for you to create a microservice by using MicroProfile.</span></span> <span data-ttu-id="d217c-144">これで Maven を使用してこれをビルドし、Docker コンテナーにコンテナー化して、次の操作を行ってローカルで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="d217c-144">You can now use Maven to build it, containerize it into a Docker container, and run it locally by doing the following:</span></span>

1. <span data-ttu-id="d217c-145">`mvn clean package` を実行し、完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="d217c-145">Run `mvn clean package`, and wait until it is complete.</span></span>

1. <span data-ttu-id="d217c-146">`docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest` を実行します。</span><span class="sxs-lookup"><span data-stu-id="d217c-146">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`.</span></span> <span data-ttu-id="d217c-147">たとえば、*docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest* の場合、 *\<docker.registry>* は *jogilescr.azurecr.io* で、 *\<docker.name>* は *samples/docker-helloworld* です。</span><span class="sxs-lookup"><span data-stu-id="d217c-147">For example, *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*, where *\<docker.registry>* is *jogilescr.azurecr.io* and *\<docker.name>* is *samples/docker-helloworld*.</span></span>

1. <span data-ttu-id="d217c-148">Web ブラウザーで [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) および [http://localhost:8080/health](http://localhost:8080/health) にアクセスしてみます。</span><span class="sxs-lookup"><span data-stu-id="d217c-148">Try to access [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="d217c-149">想定どおりの "Hello, world!"</span><span class="sxs-lookup"><span data-stu-id="d217c-149">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="d217c-150">応答 (および [/health](http://localhost:8080/health) エンドポイントの正常性に関する情報) が表示された場合、MicroProfile アプリケーションはローカル コンピューターに正常に展開されています。</span><span class="sxs-lookup"><span data-stu-id="d217c-150">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you've successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="push-the-container-to-the-azure-container-registry-instance"></a><span data-ttu-id="d217c-151">Azure コンテナー レジストリ インスタンスにコンテナーをプッシュする</span><span class="sxs-lookup"><span data-stu-id="d217c-151">Push the container to the Azure container registry instance</span></span>

<span data-ttu-id="d217c-152">ローカル コンピューターで MicroProfile アプリケーションを正常にビルドして実行したので、このコンテナーをご自分のコンテナー レジストリ インスタンスにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="d217c-152">Now that you've successfully built and run your MicroProfile application on your local machine, push this container to your container registry instance.</span></span> 

> [!NOTE]
> <span data-ttu-id="d217c-153">この記事では、Azure コンテナー レジストリ インスタンスを使用していますが、関連する場所をポイントするように *pom.xml* ファイルを編集すれば、どのコンテナー レジストリ インスタンスでも機能します。</span><span class="sxs-lookup"><span data-stu-id="d217c-153">Although this article uses an Azure container registry instance, any container registry instance should work, as long as the *pom.xml* file is edited to point to the relevant location.</span></span>

1. <span data-ttu-id="d217c-154">ローカル Docker イメージを消去、コンパイル、作成するには、`mvn clean package` を実行します。</span><span class="sxs-lookup"><span data-stu-id="d217c-154">To clean, compile, and create a local Docker image, run `mvn clean package`.</span></span>
2. <span data-ttu-id="d217c-155">コンテナーを Azure コンテナー レジストリ インスタンスにプッシュするには、`mvn dockerfile:push` を実行します。</span><span class="sxs-lookup"><span data-stu-id="d217c-155">To push the container to the Azure container registry instance, run `mvn dockerfile:push`.</span></span>

<span data-ttu-id="d217c-156">これで Docker コンテナー イメージが Azure コンテナー イメージ インスタンスにアップロードされました。</span><span class="sxs-lookup"><span data-stu-id="d217c-156">You now have your Docker container image uploaded to the Azure container registry instance.</span></span> <span data-ttu-id="d217c-157">しかし、まだ実行されていません。</span><span class="sxs-lookup"><span data-stu-id="d217c-157">However, it's not yet running.</span></span> <span data-ttu-id="d217c-158">今度はこれを Azure Web App for Containers インスタンスにデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d217c-158">You now have to deploy it to an Azure Web App for Containers instance.</span></span> 

## <a name="create-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="d217c-159">Web App for Containers インスタンスを作成する</span><span class="sxs-lookup"><span data-stu-id="d217c-159">Create an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="d217c-160">[Azure portal](http://portal.azure.com) の左側のウィンドウで、 **[Web + モバイル]** を選択し、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="d217c-160">In the [Azure portal](http://portal.azure.com), in the left pane, select **Web + Mobile**, and then do the following:</span></span>

   <span data-ttu-id="d217c-161">a.</span><span class="sxs-lookup"><span data-stu-id="d217c-161">a.</span></span> <span data-ttu-id="d217c-162">名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="d217c-162">Specify a name.</span></span> <span data-ttu-id="d217c-163">この名前は Web アプリのパブリック URL になるため、覚えやすい名前を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d217c-163">The name will become the public URL of the web app, so it's a good idea to pick a name that you can easily remember.</span></span> <span data-ttu-id="d217c-164">必要に応じて、カスタム ドメイン名を後で追加することができます。</span><span class="sxs-lookup"><span data-stu-id="d217c-164">You can add a custom domain name later, if you want.</span></span>

   <span data-ttu-id="d217c-165">b.</span><span class="sxs-lookup"><span data-stu-id="d217c-165">b.</span></span> <span data-ttu-id="d217c-166">**[コンテナーの構成]** セクションの **[イメージ ソース]** で、 **[Azure Container Registry]** を選択してから、ドロップダウン リストで適切なイメージを選択します。</span><span class="sxs-lookup"><span data-stu-id="d217c-166">In the **Configure container** section, under **Image source**, select **Azure Container Registry** and then, in the drop-down list, select the correct image.</span></span>

   <span data-ttu-id="d217c-167">c.</span><span class="sxs-lookup"><span data-stu-id="d217c-167">c.</span></span> <span data-ttu-id="d217c-168">**[スタートアップ ファイル]** フィールドに値を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d217c-168">You don't need to specify a value in the **Startup File** field.</span></span>

1. <span data-ttu-id="d217c-169">インスタンスを作成したら、それを選択し、 **[アプリケーションの設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d217c-169">After you've created the instance, select it, and then select **Application Settings**.</span></span> <span data-ttu-id="d217c-170">**[キー]** に「**WEBSITES_PORT**」と入力し、 **[値]** に「**8080**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d217c-170">For **Key**, enter **WEBSITES_PORT**, and for **Value**, enter **8080**.</span></span> <span data-ttu-id="d217c-171">これらの設定では、コンテナー内で公開するポートと、それを外部からポート 80 にマップすることが Azure に指示されます。</span><span class="sxs-lookup"><span data-stu-id="d217c-171">These settings tell Azure which port to expose in the container and to map it to port 80 externally.</span></span>

1. <span data-ttu-id="d217c-172">(省略可能) **[Docker コンテナー]** リンクを選択してから、 **[継続的なデプロイ]** を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d217c-172">(Optional) Select the **Docker Container** link, and then enable **Continuous Deployment**.</span></span> <span data-ttu-id="d217c-173">これにより、Azure コンテナー レジストリ インスタンスのイメージを更新するたびに Azure Web App for Containers インスタンスで自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="d217c-173">By doing so, whenever you update the Azure container registry instance image, it's automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="d217c-174">これで、Azure でホストされるインスタンスに `http://<appname>.azurewebsites.net/microprofile/api/helloworld` および `http://<appname>.azurewebsites.net/health` でアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="d217c-174">You should now be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="d217c-175">まとめ</span><span class="sxs-lookup"><span data-stu-id="d217c-175">Summary</span></span>

<span data-ttu-id="d217c-176">このチュートリアルでは、シンプルな MicroProfile ベースのマイクロサービスを作成し、それを Docker コンテナーにコンテナー化した後、ローカルで実行し、Azure に発行するプロセスについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="d217c-176">In this tutorial, you've stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, run it locally, and published it to Azure.</span></span> <span data-ttu-id="d217c-177">その他の便利な機能を提供するようにマイクロサービスを拡張することができます。</span><span class="sxs-lookup"><span data-stu-id="d217c-177">You can extend your microservice to provide additional useful functionality.</span></span> <span data-ttu-id="d217c-178">詳細については、[MicroProfile.io](https://microprofile.io/) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d217c-178">For more information, go to [MicroProfile.io](https://microprofile.io/).</span></span>
