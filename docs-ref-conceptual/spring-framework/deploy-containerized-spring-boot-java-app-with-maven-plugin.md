---
title: "Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法"
description: "Azure Web Apps 用の Maven プラグインを使って、Spring Boot アプリを Azure にデプロイする方法について説明します。"
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Maven
ms.assetid: 
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 5bc0eb96586cac2be54065a2c1d8edefe2a21f57
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/18/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-containerized-spring-boot-app-to-azure"></a><span data-ttu-id="330db-104">Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="330db-104">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>

<span data-ttu-id="330db-105">[Apache Maven](http://maven.apache.org/) 用の [Azure Web Apps 用 Maven プラグイン](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) は、Maven プロジェクトに Azure App Service をシームレスに統合し、開発者が Web アプリを Azure App Service にデプロイする作業を効率化します。</span><span class="sxs-lookup"><span data-stu-id="330db-105">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service .</span></span>

<span data-ttu-id="330db-106">この記事では、Azure Web Apps 用の Maven プラグインを使用して、Docker コンテナーの Spring Boot アプリケーションのサンプルを Azure App Services にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="330db-106">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application in a Docker container to Azure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="330db-107">Azure Web Apps の Maven プラグインは現在プレビューとして提供されています。</span><span class="sxs-lookup"><span data-stu-id="330db-107">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="330db-108">今後、機能が追加される予定ですが、現在は FTP 発行のみがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="330db-108">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="330db-109">前提条件</span><span class="sxs-lookup"><span data-stu-id="330db-109">Prerequisites</span></span>

<span data-ttu-id="330db-110">このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="330db-110">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="330db-111">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="330db-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="330db-112">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="330db-112">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="330db-113">最新の Java Development Kit (JDK) (バージョン 1.7 以降)。</span><span class="sxs-lookup"><span data-stu-id="330db-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="330db-114">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="330db-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="330db-115">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="330db-115">A [Git] client.</span></span>
* <span data-ttu-id="330db-116">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="330db-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="330db-117">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="330db-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="330db-118">Docker Web アプリの Spring Boot サンプルの複製</span><span class="sxs-lookup"><span data-stu-id="330db-118">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="330db-119">このセクションでは、コンテナー化された Spring Boot アプリケーションを複製してローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="330db-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="330db-120">コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="330db-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="330db-121">-- または --</span><span class="sxs-lookup"><span data-stu-id="330db-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="330db-122">[Docker での Spring Boot の使用開始]のサンプル プロジェクトを今作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="330db-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="330db-123">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="330db-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="330db-124">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="330db-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="330db-125">Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="330db-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="330db-126">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="330db-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="330db-127">たとえば、curl を使用できる場合は次のようなコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="330db-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="330db-128">次のメッセージが表示されるはずです。**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="330db-128">You should see the following message displayed: **Hello Docker World**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="330db-129">Azure サービス プリンシパルの作成</span><span class="sxs-lookup"><span data-stu-id="330db-129">Create an Azure service principal</span></span>

<span data-ttu-id="330db-130">このセクションでは、Azure にコンテナーをデプロイするときに、Maven プラグインが使用する Azure サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="330db-130">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="330db-131">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="330db-131">Open a command prompt.</span></span>

1. <span data-ttu-id="330db-132">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="330db-132">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="330db-133">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="330db-133">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="330db-134">Azure サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="330db-134">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="330db-135">ここでは `uuuuuuuu` がサービス プリンシパルのユーザー名で、`pppppppp` がパスワードです。</span><span class="sxs-lookup"><span data-stu-id="330db-135">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="330db-136">Azure が次の例に類似する JSON で応答します。</span><span class="sxs-lookup"><span data-stu-id="330db-136">Azure responds with JSON that resembles the following example:</span></span>
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="330db-137">Maven プラグインを構成して Azure にコンテナーをデプロイするときに、この JSON の応答にある値を使用します。</span><span class="sxs-lookup"><span data-stu-id="330db-137">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="330db-138">`aaaaaaaa``uuuuuuuu``pppppppp``tttttttt` はプレースホルダーの値であり、次のセクションで Maven の `settings.xml` ファイルを構成するときにこれらの値を値の各要素にマップしやすくするために、ここでこのように使用しています。</span><span class="sxs-lookup"><span data-stu-id="330db-138">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="330db-139">Maven を構成して Azure サービス プリンシパルを使用する</span><span class="sxs-lookup"><span data-stu-id="330db-139">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="330db-140">このセクションでは、Azure サービス プリンシパルの値を使用して、コンテナーを Azure にデプロイするときに Maven が使用する認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="330db-140">In this section, you use the values from your Azure service principal to configure the authentication that Maven will use when deploying your container to Azure.</span></span>

1. <span data-ttu-id="330db-141">Maven の `settings.xml` ファイルをテキスト エディターで開くと、次の例のようにパスが記載されていることがあります。</span><span class="sxs-lookup"><span data-stu-id="330db-141">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="330db-142">このチュートリアルの前のセクションで説明した Azure サービス プリンシパルの設定を、次の例のように *settings.xml* ファイルの `<servers>` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="330db-142">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   <span data-ttu-id="330db-143">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="330db-143">Where:</span></span>
   <span data-ttu-id="330db-144">要素</span><span class="sxs-lookup"><span data-stu-id="330db-144">Element</span></span> | <span data-ttu-id="330db-145">Description</span><span class="sxs-lookup"><span data-stu-id="330db-145">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="330db-146">Web アプリを Azure にデプロイするとき、セキュリティ設定を検索するために Maven が使う一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="330db-146">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="330db-147">サービス プリンシパルの `appId` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="330db-147">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="330db-148">サービス プリンシパルの `tenant` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="330db-148">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="330db-149">サービス プリンシパルの `password` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="330db-149">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="330db-150">ターゲットの Azure クラウド環境を定義します。この例では `AZURE` です </span><span class="sxs-lookup"><span data-stu-id="330db-150">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="330db-151">(環境の全リストは、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載しています)</span><span class="sxs-lookup"><span data-stu-id="330db-151">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="330db-152">*settings.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="330db-152">Save and close the *settings.xml* file.</span></span>

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a><span data-ttu-id="330db-153">省略可能: ローカルの Docker ファイルを Docker Hub にデプロイします</span><span class="sxs-lookup"><span data-stu-id="330db-153">OPTIONAL: Deploy your local Docker file to Docker Hub</span></span>

<span data-ttu-id="330db-154">Docker アカウントがあれば、Docker コンテナー イメージをローカルで構築して Docker Hub にプッシュできます。</span><span class="sxs-lookup"><span data-stu-id="330db-154">If you have a Docker account, you can build your Docker container image locally and push it to Docker Hub.</span></span> <span data-ttu-id="330db-155">そのためには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="330db-155">To do so, use the following steps.</span></span>

1. <span data-ttu-id="330db-156">Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="330db-156">Open the `pom.xml` file for your Spring Boot application in a text editor.</span></span>

1. <span data-ttu-id="330db-157">`<containerSettings>` 要素の `<imageName>` 子要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="330db-157">Locate the `<imageName>` child element of the `<containerSettings>` element.</span></span>

1. <span data-ttu-id="330db-158">`${docker.image.prefix}` の値を Docker アカウント名に更新します。</span><span class="sxs-lookup"><span data-stu-id="330db-158">Update the `${docker.image.prefix}` value with your Docker account name:</span></span>
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. <span data-ttu-id="330db-159">次のいずれかのデプロイ方法を選択してください。</span><span class="sxs-lookup"><span data-stu-id="330db-159">Choose one of the following deployment methods:</span></span>

   * <span data-ttu-id="330db-160">Maven でコンテナー イメージをローカルで構築し、Docker を使用してコンテナーを Docker Hub にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="330db-160">Build your container image locally with Maven, and then use Docker to push your container to Docker Hub:</span></span>
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * <span data-ttu-id="330db-161">[Maven 用の Docker プラグイン]がインストールされていれば、`-DpushImage` パラメーターを使用してコンテナー イメージを自動で Docker Hub に構築できます。</span><span class="sxs-lookup"><span data-stu-id="330db-161">If you have the [Docker plugin for Maven] installed, you can automatically build and your container image to Docker Hub by using the `-DpushImage` parameter:</span></span>
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a><span data-ttu-id="330db-162">省略可能: コンテナーを Azure にデプロイする前に pom.xml をカスタマイズします</span><span class="sxs-lookup"><span data-stu-id="330db-162">OPTIONAL: Customize your pom.xml before deploying your container to Azure</span></span>

<span data-ttu-id="330db-163">Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開き、`azure-webapp-maven-plugin` の `<plugin>` 要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="330db-163">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="330db-164">この要素は次の例のようになっています。</span><span class="sxs-lookup"><span data-stu-id="330db-164">This element should resemble the following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="330db-165">Maven プラグイン用に変更できる値は複数あります。これらの要素に関する詳しい説明はそれぞれ「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載されています。</span><span class="sxs-lookup"><span data-stu-id="330db-165">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="330db-166">この記事でも、次のように重要な値については説明します。</span><span class="sxs-lookup"><span data-stu-id="330db-166">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="330db-167">要素</span><span class="sxs-lookup"><span data-stu-id="330db-167">Element</span></span> | <span data-ttu-id="330db-168">Description</span><span class="sxs-lookup"><span data-stu-id="330db-168">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="330db-169">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]のバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="330db-169">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="330db-170">最新バージョンを使用していることを確認するために、[Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) で一覧表示されているバージョンを確認してください。</span><span class="sxs-lookup"><span data-stu-id="330db-170">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="330db-171">Azure の認証情報を指定します。この例では `azure-auth` を含む `<serverId>` 要素が認証情報です。Maven はこの値を、この記事の前のセクションで定義した Maven の*settings.xml* ファイル内にある Azure サービス プリンシパルを見つけるために使います。</span><span class="sxs-lookup"><span data-stu-id="330db-171">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="330db-172">ターゲット リソース グループを指定します。この例では `maven-plugin` です。</span><span class="sxs-lookup"><span data-stu-id="330db-172">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="330db-173">リソース グループが存在しない場合は、デプロイ中に新しいリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="330db-173">The resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="330db-174">Web アプリのターゲット名を指定します。</span><span class="sxs-lookup"><span data-stu-id="330db-174">Specifies the target name for your web app.</span></span> <span data-ttu-id="330db-175">この例では、ターゲット名は `maven-linux-app-${maven.build.timestamp}` です。混乱を避けるため、この例ではサフィックスの `${maven.build.timestamp}` を追加しています </span><span class="sxs-lookup"><span data-stu-id="330db-175">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="330db-176">(タイムスタンプは省略可能です。アプリ名には一意の文字列を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="330db-176">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="330db-177">ターゲット リージョンを指定します。この例では `westus` です </span><span class="sxs-lookup"><span data-stu-id="330db-177">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="330db-178">(全リストは、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載しています。)</span><span class="sxs-lookup"><span data-stu-id="330db-178">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<appSettings>` | <span data-ttu-id="330db-179">Azure に Web アプリをデプロイするときに使用するために、Maven 用の一意の設定を指定します。</span><span class="sxs-lookup"><span data-stu-id="330db-179">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="330db-180">この例では、`<property>` 要素には、アプリのポートを指定する子要素の名前と値のペアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="330db-180">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="330db-181">既定と異なるポートに変更する場合のみ、この例のポート番号を変更する設定が必要になります。</span><span class="sxs-lookup"><span data-stu-id="330db-181">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

## <a name="build-and-deploy-your-container-to-azure"></a><span data-ttu-id="330db-182">Azure にコンテナーを構築してデプロイする</span><span class="sxs-lookup"><span data-stu-id="330db-182">Build and deploy your container to Azure</span></span>

<span data-ttu-id="330db-183">この記事の前のセクションで説明した設定をすべて構成したら、次は Azure にコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="330db-183">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your container to Azure.</span></span> <span data-ttu-id="330db-184">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="330db-184">To do so, use the following steps:</span></span>

1. <span data-ttu-id="330db-185">*pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。</span><span class="sxs-lookup"><span data-stu-id="330db-185">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="330db-186">Maven を使って次の例のように Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="330db-186">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="330db-187">Maven が Web アプリを Azure にデプロイします。Web アプリが存在しない場合は新たに作成されます。</span><span class="sxs-lookup"><span data-stu-id="330db-187">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="330db-188">デプロイ開始時に、*pom.xml* ファイルの `<region>` 要素で指定したリージョンに十分な数の使用可能なサーバーがない場合は、次の例のようなエラーが表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="330db-188">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="330db-189">この場合、別のリージョンを指定し、Maven コマンドを再実行してアプリケーションをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="330db-189">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="330db-190">Web アプリのデプロイが完了すると、[Azure Portal] を使用して Web アプリを管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="330db-190">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="330db-191">Web アプリは **App Services** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="330db-191">Your web app will be listed in **App Services**:</span></span>

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* <span data-ttu-id="330db-193">Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="330db-193">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Web アプリの URL の決定][AP02]

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

## <a name="next-steps"></a><span data-ttu-id="330db-195">次のステップ</span><span class="sxs-lookup"><span data-stu-id="330db-195">Next steps</span></span>

<span data-ttu-id="330db-196">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="330db-196">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="330db-197">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]</span><span class="sxs-lookup"><span data-stu-id="330db-197">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="330db-198">Azure CLI から Azure へのログイン</span><span class="sxs-lookup"><span data-stu-id="330db-198">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="330db-199">Azure Web Apps 用の Maven プラグインを使用して、Spring Boot アプリを Azure App Service にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="330db-199">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure App Service </span></span>](deploy-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="330db-200">Azure CLI 2.0 で Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="330db-200">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="330db-201">Maven の設定リファレンス</span><span class="sxs-lookup"><span data-stu-id="330db-201">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="330db-202">[Maven 用の Docker プラグイン]</span><span class="sxs-lookup"><span data-stu-id="330db-202">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure Portal]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Maven 用の Docker プラグイン]: https://github.com/spotify/docker-maven-plugin
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker での Spring Boot の使用開始]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/deploy-containerized-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-containerized-spring-boot-java-app-with-maven-plugin/AP02.png
