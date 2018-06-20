---
title: Azure Container Service で Spring Boot Web アプリを Linux にデプロイする
description: このチュートリアルでは、Microsoft Azure の Linux Web アプリとして Spring Boot アプリケーションをデプロイする方法について説明します。
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: container-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: c3a10a0d6029ba31b13f82907311d9182f98acb6
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954753"
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a><span data-ttu-id="07b15-103">Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする</span><span class="sxs-lookup"><span data-stu-id="07b15-103">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>

<span data-ttu-id="07b15-104">このチュートリアルでは、[Docker] を使用して [Spring Boot] アプリケーションを開発し、[Azure Container Service (AKS)] の Linux ホストにデプロイする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="07b15-104">This tutorial walks you through using [Docker] to develop and deploy a [Spring Boot] application to a Linux host in the [Azure Container Service (AKS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07b15-105">前提条件</span><span class="sxs-lookup"><span data-stu-id="07b15-105">Prerequisites</span></span>

<span data-ttu-id="07b15-106">このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="07b15-106">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="07b15-107">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="07b15-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="07b15-108">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="07b15-108">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="07b15-109">最新の [Java Developer Kit (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="07b15-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="07b15-110">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="07b15-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="07b15-111">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="07b15-111">A [Git] client.</span></span>
* <span data-ttu-id="07b15-112">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="07b15-112">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="07b15-113">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="07b15-113">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="07b15-114">最初に使用する Docker での Spring Boot Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="07b15-114">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="07b15-115">次の手順では、単純な Spring Boot Web アプリケーションを作成し、それをローカルにテストするために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="07b15-115">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="07b15-116">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成して、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-116">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="07b15-117">-- または --</span><span class="sxs-lookup"><span data-stu-id="07b15-117">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="07b15-118">[Docker での Spring Boot の使用開始]のサンプル プロジェクトを今作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-118">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="07b15-119">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-119">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="07b15-120">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-120">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="07b15-121">Web アプリが作成されたら、JAR ファイルがある `target` ディレクトリにディレクトリを変更し、Web アプリを起動します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-121">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="07b15-122">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="07b15-122">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="07b15-123">たとえば curl が使用でき、Tomcat サーバーをポート 80 で実行されるように構成した場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="07b15-123">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="07b15-124">次のメッセージが表示されるはずです。**Hello Docker World!**</span><span class="sxs-lookup"><span data-stu-id="07b15-124">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="07b15-126">Azure Container Registry をプライベート Docker レジストリとして使用するために作成する</span><span class="sxs-lookup"><span data-stu-id="07b15-126">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="07b15-127">Azure Portal を使用して Azure Container Registry を作成する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="07b15-127">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="07b15-128">Azure Portal ではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成](/azure/container-registry/container-registry-get-started-azure-cli)」の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="07b15-128">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
>

1. <span data-ttu-id="07b15-129">[Azure ポータル]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="07b15-129">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="07b15-130">Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。</span><span class="sxs-lookup"><span data-stu-id="07b15-130">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="07b15-131">**[+ 新規]** のメニュー アイコン、**[コンテナー]**、**[Azure Container Registry]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-131">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Azure Container Registry を新しく作成する][AR01]

1. <span data-ttu-id="07b15-133">Azure Container Registry テンプレートの情報ページが表示されたら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-133">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Azure Container Registry を新しく作成する][AR02]

1. <span data-ttu-id="07b15-135">**[コンテナー レジストリの作成]** ページが表示されたら、**[レジストリ名]** と **[リソース グループ]** を入力し、**[管理ユーザー]** に対して **[有効化]** を選択した後、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-135">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Azure Container Registry 設定を構成する][AR03]

1. <span data-ttu-id="07b15-137">コンテナー レジストリが作成されたら、Azure Portal でコンテナー レジストリに移動して、**[アクセス キー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-137">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="07b15-138">次の手順で使用するため、ユーザー名とパスワードをメモします。</span><span class="sxs-lookup"><span data-stu-id="07b15-138">Take note of the username and password for the next steps.</span></span>

   ![Azure Container Registry のアクセス キー][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="07b15-140">Azure Container Registry のアクセス キーを使用するために Maven を構成する</span><span class="sxs-lookup"><span data-stu-id="07b15-140">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="07b15-141">Maven インストールの構成ディレクトリに移動し、*settings.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="07b15-141">Navigate to the configuration directory for your Maven installation and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="07b15-142">このチュートリアルの前のセクションから、Azure Container Registry のアクセス設定を *settings.xml* ファイルの `<servers>` コレクションに追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-142">Add your Azure Container Registry access settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="07b15-143">Spring Boot アプリケーションの完了プロジェクト ディレクトリ (例: "*C:\SpringBoot\gs-spring-boot-docker\complete*" または "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="07b15-143">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="07b15-144">*pom.xml* ファイル内の `<properties>` コレクションを、このチュートリアルの前のセクションにあった Azure Container Registry のログイン サーバー値で更新します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-144">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="07b15-145">`<plugin>` にこのチュートリアルの前のセクションにあった Azure Container Registry のログイン サーバー アドレスとレジストリ名が含まれるように、*pom.xml* ファイル内の `<plugins>` コレクションを更新します。</span><span class="sxs-lookup"><span data-stu-id="07b15-145">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="07b15-146">例: </span><span class="sxs-lookup"><span data-stu-id="07b15-146">For example:</span></span>

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

1. <span data-ttu-id="07b15-147">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行してアプリケーションをリビルドし、コンテナーを Azure Container Registry にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="07b15-147">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="07b15-148">Docker コンテナーを Azure にプッシュすると、Docker コンテナーが正しく作成されていても、次のいずれかのようなエラー メッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="07b15-148">When you are pushing your Docker container to Azure, you may receive an error message that is similar to one of the following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="07b15-149">これが発生した場合は、Docker コマンド ラインから Azure アカウントにサインインしなければならない場合があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="07b15-149">If this happens, you may need to sign in to your Azure account from the Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="07b15-150">これで、次のようにしてコマンド ラインからコンテナーをプッシュできるようになります。</span><span class="sxs-lookup"><span data-stu-id="07b15-150">You can then push your container from the command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="07b15-151">コンテナー イメージを使用して Azure App Service で Linux に Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="07b15-151">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="07b15-152">[Azure ポータル]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="07b15-152">Browse to the [Azure portal] and sign in.</span></span>

1. <span data-ttu-id="07b15-153">**[+ 新規]** のメニュー アイコン、**[Web + Mobile]**、**[Web App on Linux]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-153">Click the menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![Azure ポータルで Web アプリを新しく作成する][LX01]

1. <span data-ttu-id="07b15-155">**[Web App on Linux]** ページが表示されたら、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="07b15-155">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="07b15-156">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="07b15-156">a.</span></span> <span data-ttu-id="07b15-157">**[App name]\(アプリ名\)** に一意の名前 (例: "*wingtiptoyslinux*") を入力します。</span><span class="sxs-lookup"><span data-stu-id="07b15-157">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="07b15-158">b.</span><span class="sxs-lookup"><span data-stu-id="07b15-158">b.</span></span> <span data-ttu-id="07b15-159">**[サブスクリプション]** ボックスの一覧で、サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="07b15-159">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="07b15-160">c.</span><span class="sxs-lookup"><span data-stu-id="07b15-160">c.</span></span> <span data-ttu-id="07b15-161">**[リソース グループ]** ボックスの一覧で、既存のリソース グループを選択するか、新しいリソース グループの名前を指定して作成します。</span><span class="sxs-lookup"><span data-stu-id="07b15-161">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="07b15-162">d.</span><span class="sxs-lookup"><span data-stu-id="07b15-162">d.</span></span> <span data-ttu-id="07b15-163">**[コンテナーの構成]** をクリックして、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="07b15-163">Click **Configure container** and enter the following information:</span></span>

      * <span data-ttu-id="07b15-164">**[プライベート レジストリ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="07b15-164">Choose **Private registry**.</span></span>

      * <span data-ttu-id="07b15-165">**[イメージとオプションのタグ]**: 先に設定したコンテナー名 ("*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*" など) を指定します。</span><span class="sxs-lookup"><span data-stu-id="07b15-165">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

      * <span data-ttu-id="07b15-166">**[サーバーの URL]**: 先に設定したレジストリの URL ("*https://wingtiptoysregistry.azurecr.io*" など) を指定します。</span><span class="sxs-lookup"><span data-stu-id="07b15-166">**Server URL**: Specify your registry URL from earlier; for example: "*https://wingtiptoysregistry.azurecr.io*"</span></span>

      * <span data-ttu-id="07b15-167">**[ログイン ユーザー名]** と **[パスワード]**: 前の手順で使用した**アクセス キー**から、ログイン資格情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="07b15-167">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="07b15-168">e.</span><span class="sxs-lookup"><span data-stu-id="07b15-168">e.</span></span> <span data-ttu-id="07b15-169">上記の情報をすべて入力したら、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-169">Once you have entered all of the above information, click **OK**.</span></span>

   ![Web アプリの設定を構成する][LX02]

1. <span data-ttu-id="07b15-171">**Create** をクリックしてください。</span><span class="sxs-lookup"><span data-stu-id="07b15-171">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="07b15-172">Azure は、80 または 8080 の標準のポートで実行されている埋め込みの Tomcat サーバーにインターネットの要求を自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="07b15-172">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="07b15-173">ただし、埋め込みの Tomcat サーバーをカスタム ポートで実行するように構成している場合は、埋め込みの Tomcat サーバーのポートを定義する環境変数を Web アプリに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="07b15-173">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="07b15-174">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="07b15-174">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="07b15-175">[Azure ポータル]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="07b15-175">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="07b15-176">**[App Services]** のアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-176">Click the icon for **App Services**.</span></span> <span data-ttu-id="07b15-177">(下の図の項目 #1 を参照。)</span><span class="sxs-lookup"><span data-stu-id="07b15-177">(See item #1 in the image below.)</span></span>
>
> 3. <span data-ttu-id="07b15-178">一覧から Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="07b15-178">Select your web app from the list.</span></span> <span data-ttu-id="07b15-179">(下の図の項目 #2 を参照。)</span><span class="sxs-lookup"><span data-stu-id="07b15-179">(Item #2 in the image below.)</span></span>
>
> 4. <span data-ttu-id="07b15-180">**[アプリケーションの設定]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="07b15-180">Click **Application Settings**.</span></span> <span data-ttu-id="07b15-181">(下の図の項目 #3 を参照。)</span><span class="sxs-lookup"><span data-stu-id="07b15-181">(Item #3 in the image below.)</span></span>
>
> 5. <span data-ttu-id="07b15-182">**[App settings]\(アプリ設定\)** セクションで、**PORT** という名前の新しい環境変数を追加して、この値にカスタム ポート番号を入力します。</span><span class="sxs-lookup"><span data-stu-id="07b15-182">In the **App settings** section, add a new environment variable named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="07b15-183">(下の図の項目 #4 を参照。)</span><span class="sxs-lookup"><span data-stu-id="07b15-183">(Item #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="07b15-184">**[Save]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="07b15-184">Click **Save**.</span></span> <span data-ttu-id="07b15-185">(下の図の項目 #5 を参照。)</span><span class="sxs-lookup"><span data-stu-id="07b15-185">(Item #5 in the image below.)</span></span>
>
> ![Azure Portal でのカスタム ポート番号の保存][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="07b15-187">次の手順</span><span class="sxs-lookup"><span data-stu-id="07b15-187">Next steps</span></span>

<span data-ttu-id="07b15-188">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b15-188">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="07b15-189">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="07b15-189">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="07b15-190">Azure Container Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="07b15-190">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="07b15-191">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b15-191">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="07b15-192">Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b15-192">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="07b15-193">独自の Spring Boot アプリケーションの使用開始に関するヘルプについては、「**Spring Initializr**」(https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b15-193">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="07b15-194">単純な Spring Boot アプリケーションの作成の詳細については、「Spring Initializr」(https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b15-194">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="07b15-195">Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b15-195">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure ポータル]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
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
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker での Spring Boot の使用開始]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-linux/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png

[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
