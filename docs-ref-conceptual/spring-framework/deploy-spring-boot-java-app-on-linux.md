---
title: コンテナー用 Azure App Service で Spring Boot Web アプリをデプロイする
description: このチュートリアルでは、Microsoft Azure の Linux Web アプリとして Spring Boot アプリケーションをデプロイする方法について説明します。
services: azure app service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: azure app service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 407b852e24ef88d2fb075bd064f1acf2b107ddc1
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270862"
---
# <a name="deploy-a-spring-boot-application-on-azure-app-service-for-container"></a><span data-ttu-id="0f3af-103">コンテナー用 Azure App Service で Spring Boot アプリケーションをデプロイする</span><span class="sxs-lookup"><span data-stu-id="0f3af-103">Deploy a Spring Boot application on Azure App Service for Container</span></span>

<span data-ttu-id="0f3af-104">このチュートリアルでは、[Docker] を使用してお使いの [Spring Boot] アプリケーションをコンテナー化し、ご自身の Docker イメージを [Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) の Linux ホストにデプロイする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-104">This tutorial walks you through using [Docker] to containerize your [Spring Boot] application and deploy your own docker image to a Linux host in the [Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f3af-105">前提条件</span><span class="sxs-lookup"><span data-stu-id="0f3af-105">Prerequisites</span></span>

<span data-ttu-id="0f3af-106">このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f3af-106">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="0f3af-107">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="0f3af-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="0f3af-108">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="0f3af-108">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="0f3af-109">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="0f3af-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="0f3af-110">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="0f3af-111">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="0f3af-111">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="0f3af-112">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="0f3af-112">A [Git] client.</span></span>
* <span data-ttu-id="0f3af-113">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="0f3af-113">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0f3af-114">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f3af-114">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="0f3af-115">最初に使用する Docker での Spring Boot Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="0f3af-115">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="0f3af-116">次の手順では、単純な Spring Boot Web アプリケーションを作成し、それをローカルにテストするために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-116">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="0f3af-117">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成して、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-117">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="0f3af-118">-- または --</span><span class="sxs-lookup"><span data-stu-id="0f3af-118">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="0f3af-119">[Docker での Spring Boot の使用開始]のサンプル プロジェクトを今作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-119">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="0f3af-120">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-120">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="0f3af-121">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-121">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="0f3af-122">Web アプリが作成されたら、JAR ファイルがある `target` ディレクトリにディレクトリを変更し、Web アプリを起動します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-122">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="0f3af-123">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="0f3af-123">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="0f3af-124">たとえば curl が使用でき、Tomcat サーバーをポート 80 で実行されるように構成した場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0f3af-124">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="0f3af-125">次のメッセージが表示されます。**Hello Docker World!**</span><span class="sxs-lookup"><span data-stu-id="0f3af-125">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="0f3af-127">Azure Container Registry をプライベート Docker レジストリとして使用するために作成する</span><span class="sxs-lookup"><span data-stu-id="0f3af-127">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="0f3af-128">Azure Portal を使用して Azure Container Registry を作成する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-128">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0f3af-129">Azure Portal ではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成](/azure/container-registry/container-registry-get-started-azure-cli)」の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="0f3af-129">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
>

1. <span data-ttu-id="0f3af-130">[Azure Portal]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-130">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="0f3af-131">Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-131">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="0f3af-132">**[+ 新規]** のメニュー アイコン、 **[コンテナー]** 、 **[Azure Container Registry]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-132">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Azure Container Registry を新しく作成する][AR01]

1. <span data-ttu-id="0f3af-134">Azure Container Registry テンプレートの情報ページが表示されたら、 **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-134">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Azure Container Registry を新しく作成する][AR02]

1. <span data-ttu-id="0f3af-136">**[コンテナー レジストリの作成]** ページが表示されたら、 **[レジストリ名]** と **[リソース グループ]** を入力し、 **[管理ユーザー]** に対して **[有効化]** を選択した後、 **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-136">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Azure Container Registry 設定を構成する][AR03]

1. <span data-ttu-id="0f3af-138">コンテナー レジストリが作成されたら、Azure Portal でコンテナー レジストリに移動して、 **[アクセス キー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-138">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="0f3af-139">次の手順で使用するため、ユーザー名とパスワードをメモします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-139">Take note of the username and password for the next steps.</span></span>

   ![Azure Container Registry のアクセス キー][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="0f3af-141">Azure Container Registry のアクセス キーを使用するために Maven を構成する</span><span class="sxs-lookup"><span data-stu-id="0f3af-141">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="0f3af-142">Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や " */users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="0f3af-142">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="0f3af-143">*pom.xml* ファイル内の `<properties>` コレクションを、最新バージョンの [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) と、このチュートリアルの前のセクションにある Azure Container Registry のログイン サーバー値とアクセス設定で更新します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-143">Update the `<properties>` collection in the *pom.xml* file with the latest version of [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) and login server value and access settings for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="0f3af-144">例:</span><span class="sxs-lookup"><span data-stu-id="0f3af-144">For example:</span></span>

   ```xml
   <properties>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. <span data-ttu-id="0f3af-145">*pom.xml* ファイル内の `<plugins>` コレクションに [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) を追加し、基本イメージを `<from>/<image>` で指定し、最終イメージ名を `<to>/<image>` で指定し、前のセクションにあるユーザー名とパスワードを `<to>/<auth>` で指定します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-145">Add [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) to the `<plugins>` collection in the *pom.xml* file, specify the base image at `<from>/<image>` and  final image name `<to>/<image>`, specify the username and password from previous section at `<to>/<auth>`.</span></span> <span data-ttu-id="0f3af-146">例:</span><span class="sxs-lookup"><span data-stu-id="0f3af-146">For example:</span></span>

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
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="0f3af-147">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行してアプリケーションをリビルドし、コンテナーを Azure Container Registry にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-147">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> <span data-ttu-id="0f3af-148">Jib を使ってイメージを Azure Container Registry にプッシュする場合、イメージは *Dockerfile* を受け付けません。詳細については、[こちら](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html)のドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-148">When you are using Jib to push your image to the Azure Container Registry, the image will not honor *Dockerfile*, see [this](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html) document for details.</span></span>
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="0f3af-149">コンテナー イメージを使用して Azure App Service で Linux に Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="0f3af-149">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="0f3af-150">[Azure Portal]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-150">Browse to the [Azure portal] and sign in.</span></span>

2. <span data-ttu-id="0f3af-151">**[+ リソースの作成]** のメニュー アイコンをクリックし、 **[Web]** 、 **[Web App for Containers]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-151">Click the menu icon for **+ Create a resource**, then click **Web**, and then click **Web App for Containers**.</span></span>
   
   ![Azure ポータルで Web アプリを新しく作成する][LX01]

3. <span data-ttu-id="0f3af-153">**[Web App on Linux]** ページが表示されたら、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-153">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="0f3af-154">a.</span><span class="sxs-lookup"><span data-stu-id="0f3af-154">a.</span></span> <span data-ttu-id="0f3af-155">**[App name]\(アプリ名\)** に一意の名前 ("*wingtiptoyslinux*" など) を入力します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-155">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*"</span></span>

   <span data-ttu-id="0f3af-156">b.</span><span class="sxs-lookup"><span data-stu-id="0f3af-156">b.</span></span> <span data-ttu-id="0f3af-157">**[サブスクリプション]** ボックスの一覧で、サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-157">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="0f3af-158">c.</span><span class="sxs-lookup"><span data-stu-id="0f3af-158">c.</span></span> <span data-ttu-id="0f3af-159">**[リソース グループ]** ボックスの一覧で、既存のリソース グループを選択するか、新しいリソース グループの名前を指定して作成します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-159">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="0f3af-160">d.</span><span class="sxs-lookup"><span data-stu-id="0f3af-160">d.</span></span> <span data-ttu-id="0f3af-161">**[OS]** として *[Linux]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-161">Choose *Linux* as the **OS**.</span></span>

   <span data-ttu-id="0f3af-162">e.</span><span class="sxs-lookup"><span data-stu-id="0f3af-162">e.</span></span> <span data-ttu-id="0f3af-163">**[App Service プラン/場所]** をクリックして既存の App Service プランを選択するか、 **[新規作成]** をクリックして新しい App Service プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-163">Click **App Service plan/Location** and choose an existing app service plan, or click **Create new** to create a new app service plan.</span></span>

   <span data-ttu-id="0f3af-164">f.</span><span class="sxs-lookup"><span data-stu-id="0f3af-164">f.</span></span> <span data-ttu-id="0f3af-165">**[コンテナーの構成]** をクリックして、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-165">Click **Configure container** and enter the following information:</span></span>

   * <span data-ttu-id="0f3af-166">**[単一コンテナー]** と **[Azure Container Registry]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-166">Choose **Single Container** and  **Azure Container Registry**.</span></span>

   * <span data-ttu-id="0f3af-167">**レジストリ**:先に作成したコンテナー名 ("*wingtiptoysregistry*" など) を選択します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-167">**Registry**: Choose your container name created earlier; for example: "*wingtiptoysregistry*"</span></span>

   * <span data-ttu-id="0f3af-168">**イメージ**:イメージ名 ("*gs-spring-boot-docker*" など) を選択します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-168">**Image**: Choose the image name; for example: "*gs-spring-boot-docker*"</span></span>
   
   * <span data-ttu-id="0f3af-169">**タグ**: イメージのタグ ("*latest*" など) を選択します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-169">**Tag**: Choose the tag for the image; for example: "*latest*"</span></span>
   
   * <span data-ttu-id="0f3af-170">**スタートアップ ファイル**:イメージには既にスタートアップ コマンドがあるため、これは空白のままにしておきます</span><span class="sxs-lookup"><span data-stu-id="0f3af-170">**Startup File**: Keep it blank since the image already has the start up command</span></span>
   
   <span data-ttu-id="0f3af-171">e.</span><span class="sxs-lookup"><span data-stu-id="0f3af-171">e.</span></span> <span data-ttu-id="0f3af-172">上記の情報をすべて入力したら、 **[適用]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-172">Once you have entered all of the above information, click **Apply**.</span></span>

   ![Web アプリの設定を構成する][LX02]

4. <span data-ttu-id="0f3af-174">**Create** をクリックしてください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0f3af-175">Azure は、80 または 8080 の標準のポートで実行されている埋め込みの Tomcat サーバーにインターネットの要求を自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-175">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="0f3af-176">ただし、埋め込みの Tomcat サーバーをカスタム ポートで実行するように構成している場合は、埋め込みの Tomcat サーバーのポートを定義する環境変数を Web アプリに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f3af-176">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="0f3af-177">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-177">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="0f3af-178">[Azure Portal]を参照して、サインインします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-178">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="0f3af-179">**App Services** のアイコンをクリックし、一覧から Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-179">Click the icon for **App Services**, and select your web app from the list.</span></span>
>
> 4. <span data-ttu-id="0f3af-180">**[構成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-180">Click **Configuration**.</span></span> <span data-ttu-id="0f3af-181">(下の図の項目 #1 を参照。)</span><span class="sxs-lookup"><span data-stu-id="0f3af-181">(Item #1 in the image below.)</span></span>
>
> 5. <span data-ttu-id="0f3af-182">**[アプリケーション設定]** セクションで、**PORT** という名前の新しい設定を追加して、この値にカスタム ポート番号を入力します。</span><span class="sxs-lookup"><span data-stu-id="0f3af-182">In the **Application settings** section, add a new setting named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="0f3af-183">(下の図の項目 #2、#3、#4 を参照。)</span><span class="sxs-lookup"><span data-stu-id="0f3af-183">(Item #2, #3, #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="0f3af-184">**[Save]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0f3af-184">Click **Save**.</span></span> <span data-ttu-id="0f3af-185">(下の図の項目 #5 を参照。)</span><span class="sxs-lookup"><span data-stu-id="0f3af-185">(Item #5 in the image below.)</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="0f3af-187">次の手順</span><span class="sxs-lookup"><span data-stu-id="0f3af-187">Next steps</span></span>

<span data-ttu-id="0f3af-188">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-188">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0f3af-189">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="0f3af-189">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="0f3af-190">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="0f3af-190">Additional Resources</span></span>

<span data-ttu-id="0f3af-191">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="0f3af-192">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="0f3af-192">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="0f3af-193">Azure Container Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="0f3af-193">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="0f3af-194">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-194">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="0f3af-195">Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-195">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="0f3af-196">独自の Spring Boot アプリケーションの使用開始に関するヘルプについては、「**Spring Initializr**」(https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-196">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="0f3af-197">単純な Spring Boot アプリケーションの作成開始の詳細については、「Spring Initializr」(https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-197">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="0f3af-198">Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f3af-198">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure Portal]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Azure DevOps と Java の操作]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
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

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-linux/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png

[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
