---
title: Maven と Azure を使用して Spring Boot アプリをクラウドにデプロイする
description: Azure Web Apps の Maven プラグインを使って、Spring Boot アプリをクラウドにデプロイする方法について説明します。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: robmcm;kevinzha;brborges
ms.date: 06/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ca788354d26964bd9f1e21a0d3a8005ff65ce4bc
ms.sourcegitcommit: 280d13b43cef94177d95e03879a5919da234a23c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2018
ms.locfileid: "43324348"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a><span data-ttu-id="4636f-103">Azure App Service 用の Maven プラグインを使って、Spring Boot アプリをクラウドにデプロイする</span><span class="sxs-lookup"><span data-stu-id="4636f-103">Deploy a Spring Boot app to the cloud using the Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="4636f-104">この記事では、Azure App Service Web Apps 用の Maven プラグインを使って、Spring Boot アプリケーションのサンプルをデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4636f-104">This article demonstrates using the Maven Plugin for Azure App Service Web Apps to deploy a sample Spring Boot application.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="4636f-105">[Apache Maven](http://maven.apache.org/) の [Azure App Service Web Apps 用 Maven プラグイン](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)は、Maven プロジェクトに Azure App Service をシームレスに統合し、開発者が Web アプリを Azure App Service にデプロイするプロセスを効率化します。</span><span class="sxs-lookup"><span data-stu-id="4636f-105">The [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="4636f-106">Maven プラグインを使用する前に、利用可能なプラグインの最新バージョンを Maven Central で確認してください: [![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span><span class="sxs-lookup"><span data-stu-id="4636f-106">Before using the Maven plugin, check on Maven Central for the latest available version of the plugin: [![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4636f-107">前提条件</span><span class="sxs-lookup"><span data-stu-id="4636f-107">Prerequisites</span></span>

<span data-ttu-id="4636f-108">このチュートリアルの手順を実行するには、次の前提条件を満たしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="4636f-108">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="4636f-109">Azure サブスクリプション: Azure サブスクリプションをまだ取得していない場合は、[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="4636f-109">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="4636f-110">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="4636f-110">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="4636f-111">最新の Java Development Kit (JDK) (バージョン 1.7 以降)。</span><span class="sxs-lookup"><span data-stu-id="4636f-111">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="4636f-112">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="4636f-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="4636f-113">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="4636f-113">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="4636f-114">Spring Boot Web アプリのサンプルを複製する</span><span class="sxs-lookup"><span data-stu-id="4636f-114">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="4636f-115">このセクションでは、完成した Spring Boot アプリケーションを複製し、ローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="4636f-115">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="4636f-116">コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="4636f-116">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="4636f-117">-- または --</span><span class="sxs-lookup"><span data-stu-id="4636f-117">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="4636f-118">[Spring Boot Getting Started] サンプル プロジェクトを作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4636f-118">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="4636f-119">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4636f-119">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="4636f-120">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4636f-120">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="4636f-121">Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="4636f-121">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="4636f-122">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="4636f-122">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="4636f-123">たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="4636f-123">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="4636f-124">**Greetings from Spring Boot! (Spring Boot からのあいさつ)** というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4636f-124">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a><span data-ttu-id="4636f-125">Azure App Service への WAR ベースのデプロイ用にプロジェクトを調整する</span><span class="sxs-lookup"><span data-stu-id="4636f-125">Adjust project for WAR-based deployment on Azure App Service</span></span>

<span data-ttu-id="4636f-126">このセクションでは、Spring Boot プロジェクトを、既定ではランタイムとして Tomcat が提供されている Azure App Service に、WAR ファイルとしてデプロイできるようにすばやく調整します。</span><span class="sxs-lookup"><span data-stu-id="4636f-126">In this section we will quickly adjust the Spring Boot project to be deployed as a WAR file on Azure App Service, which provides Tomcat as the runtime by default.</span></span> <span data-ttu-id="4636f-127">そのためには、2 つのファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="4636f-127">For this to work, there are two files to be modified:</span></span>

- <span data-ttu-id="4636f-128">Maven の `pom.xml` ファイル</span><span class="sxs-lookup"><span data-stu-id="4636f-128">The Maven `pom.xml` file</span></span>
- <span data-ttu-id="4636f-129">`Application` Java クラス</span><span class="sxs-lookup"><span data-stu-id="4636f-129">The `Application` Java class</span></span>

<span data-ttu-id="4636f-130">Maven の設定から着手しましょう。</span><span class="sxs-lookup"><span data-stu-id="4636f-130">Let's start with the Maven settings:</span></span>

1. <span data-ttu-id="4636f-131">`pom.xml` を開きます。</span><span class="sxs-lookup"><span data-stu-id="4636f-131">Open `pom.xml`</span></span>

1. <span data-ttu-id="4636f-132">先頭の `<artifactId>` 定義の直後に `<packaging>war</packaging>` を追加します。</span><span class="sxs-lookup"><span data-stu-id="4636f-132">Add `<packaging>war</packaging>` right after the `<artifactId>` definition at the top:</span></span>
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. <span data-ttu-id="4636f-133">次の依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="4636f-133">Add the following dependency:</span></span>
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

<span data-ttu-id="4636f-134">`Application` クラスを開き、期待どおりに IDE によって新しい依存関係が選択されていたら、次の変更に進みます。</span><span class="sxs-lookup"><span data-stu-id="4636f-134">Now open the class `Application`, hopefully after your IDE has already picked up the new dependencies, and proceed with the following modifications:</span></span>

1. <span data-ttu-id="4636f-135">Application クラスを `SpringBootServletInitializer` のサブクラスにします。</span><span class="sxs-lookup"><span data-stu-id="4636f-135">Make class Application a subclass of `SpringBootServletInitializer`:</span></span>
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. <span data-ttu-id="4636f-136">Application クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="4636f-136">Add the following method to the Application class:</span></span>
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```
1. <span data-ttu-id="4636f-137">ご自身のインポートを整理して、`SpringApplicationBuilder` と `SpringBootServletInitializer` が正しくインポートされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4636f-137">Organize your imports to ensure `SpringApplicationBuilder` and `SpringBootServletInitializer` are properly imported.</span></span>

<span data-ttu-id="4636f-138">これで、ご自身のアプリケーションを Tomcat その他の任意の Servlet ランタイム (例: Jetty) にデプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="4636f-138">Your application is now ready to be deployed to Tomcat and any other Servlet runtime (e.g. Jetty).</span></span>

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a><span data-ttu-id="4636f-139">Azure App Service Web Apps 用の Maven プラグインを追加する</span><span class="sxs-lookup"><span data-stu-id="4636f-139">Add the Maven Plugin for Azure App Service Web Apps</span></span>

<span data-ttu-id="4636f-140">このセクションでは、Azure App Service Web Apps への、このアプリケーションのデプロイ全体を自動化する Maven プラグインを追加します。</span><span class="sxs-lookup"><span data-stu-id="4636f-140">In this section, we will add a Maven plugin that will automate the entire deployment of this application into Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="4636f-141">もう一度 `pom.xml` を開きます。</span><span class="sxs-lookup"><span data-stu-id="4636f-141">Open `pom.xml` once again.</span></span>

1. <span data-ttu-id="4636f-142">`<properties>` の内部で、`maven.build.timestamp.format` プロパティを使用してカスタムのタイムスタンプ形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="4636f-142">Inside `<properties>`, set a custom timestamp format with the property `maven.build.timestamp.format`.</span></span> <span data-ttu-id="4636f-143">Azure App Service によってご自身のアプリケーションのパブリック URL が作成されるため、この設定を使用してデプロイの名前が生成されることで、他のユーザーのライブ デプロイとの競合を回避できます。</span><span class="sxs-lookup"><span data-stu-id="4636f-143">Because Azure App Service creates a public URL for your application, this setting will be used to generate the name of your deployment, and avoid conflict with other users' live deployments.</span></span>
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. <span data-ttu-id="4636f-144">`<plugins>` 要素に、次を追加します。</span><span class="sxs-lookup"><span data-stu-id="4636f-144">In the `<plugins>` element, add the following:</span></span>
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

<span data-ttu-id="4636f-145">これらの設定によって、Maven プロジェクトを Azure App Service Web App にライブ デプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="4636f-145">With these settings, your Maven project is now ready for live deployment to Azure App Service Web App.</span></span>

## <a name="install-and-log-in-to-azure-cli"></a><span data-ttu-id="4636f-146">Azure CLI のインストールとログイン</span><span class="sxs-lookup"><span data-stu-id="4636f-146">Install and log in to Azure CLI</span></span>

<span data-ttu-id="4636f-147">Maven プラグインで最もシンプルかつ容易に Spring Boot アプリケーションをデプロイするには、[Azure CLI](https://docs.microsoft.com/cli/azure/) を使用します。</span><span class="sxs-lookup"><span data-stu-id="4636f-147">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span> <span data-ttu-id="4636f-148">これがインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4636f-148">Make sure you have it installed.</span></span>

1. <span data-ttu-id="4636f-149">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="4636f-149">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
   <span data-ttu-id="4636f-150">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="4636f-150">Follow the instructions to complete the sign-in process.</span></span>

## <a name="optionally-customize-pomxml-before-deploying"></a><span data-ttu-id="4636f-151">必要に応じて、デプロイの前に pom.xml をカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="4636f-151">Optionally, customize pom.xml before deploying</span></span>

<span data-ttu-id="4636f-152">Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開き、`azure-webapp-maven-plugin` の `<plugin>` 要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="4636f-152">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="4636f-153">この要素は次の例のようになっています。</span><span class="sxs-lookup"><span data-stu-id="4636f-153">This element should resemble the following example:</span></span>

   ```xml
  <plugins>
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
      <configuration>
         <resourceGroup>maven-projects</resourceGroup>
         <appName>${project.artifactId}-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>war</deploymentType>
      </configuration>
    </plugin>
  </plugins>
   ```

<span data-ttu-id="4636f-154">Maven プラグイン用に変更できる値は複数あります。これらの要素に関する詳しい説明はそれぞれ「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載されています。</span><span class="sxs-lookup"><span data-stu-id="4636f-154">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="4636f-155">この記事でも、次のように重要な値については説明します。</span><span class="sxs-lookup"><span data-stu-id="4636f-155">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="4636f-156">要素</span><span class="sxs-lookup"><span data-stu-id="4636f-156">Element</span></span> | <span data-ttu-id="4636f-157">説明</span><span class="sxs-lookup"><span data-stu-id="4636f-157">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="4636f-158">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]のバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="4636f-158">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="4636f-159">[Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) に表示されているバージョンを確認して、最新バージョンを使用していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4636f-159">Verify the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="4636f-160">ターゲット リソース グループを指定します。この例では `maven-plugin` です。</span><span class="sxs-lookup"><span data-stu-id="4636f-160">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="4636f-161">リソース グループが存在しない場合は、デプロイ中に新しいリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="4636f-161">The resource group is created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="4636f-162">Web アプリのターゲット名を指定します。</span><span class="sxs-lookup"><span data-stu-id="4636f-162">Specifies the target name for your web app.</span></span> <span data-ttu-id="4636f-163">この例では、ターゲット名は `maven-web-app-${maven.build.timestamp}` です。混乱を避けるため、この例ではサフィックスの `${maven.build.timestamp}` を追加しています </span><span class="sxs-lookup"><span data-stu-id="4636f-163">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="4636f-164">(タイムスタンプは省略可能です。アプリ名には一意の文字列を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="4636f-164">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="4636f-165">ターゲット リージョンを指定します。この例では `westus` です </span><span class="sxs-lookup"><span data-stu-id="4636f-165">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="4636f-166">(完全なリストについては、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」 をご覧ください)。</span><span class="sxs-lookup"><span data-stu-id="4636f-166">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<javaVersion>` | <span data-ttu-id="4636f-167">Web アプリの Java ランタイム バージョンを指定します </span><span class="sxs-lookup"><span data-stu-id="4636f-167">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="4636f-168">(完全なリストについては、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」(Azure Web Apps 用の Maven プラグイン) をご覧ください)。</span><span class="sxs-lookup"><span data-stu-id="4636f-168">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<deploymentType>` | <span data-ttu-id="4636f-169">Web アプリのデプロイの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="4636f-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="4636f-170">既定値は `war` です。</span><span class="sxs-lookup"><span data-stu-id="4636f-170">Default is `war`.</span></span> |

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="4636f-171">Web アプリをビルドして Azure にデプロイする</span><span class="sxs-lookup"><span data-stu-id="4636f-171">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="4636f-172">この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4636f-172">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="4636f-173">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="4636f-173">To do so, use the following steps:</span></span>

1. <span data-ttu-id="4636f-174">*pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。</span><span class="sxs-lookup"><span data-stu-id="4636f-174">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="4636f-175">Maven を使って次の例のように Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4636f-175">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="4636f-176">Maven が Web アプリを Azure にデプロイします。Web アプリが存在しない場合は新たに作成されます。</span><span class="sxs-lookup"><span data-stu-id="4636f-176">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="4636f-177">Web アプリのデプロイが完了すると、[Azure Portal] を使って Web アプリを管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="4636f-177">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="4636f-178">Web アプリは **App Services** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="4636f-178">Your web app will be listed in **App Services**:</span></span>

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* <span data-ttu-id="4636f-180">Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="4636f-180">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4636f-182">次の手順</span><span class="sxs-lookup"><span data-stu-id="4636f-182">Next steps</span></span>

<span data-ttu-id="4636f-183">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4636f-183">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="4636f-184">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]</span><span class="sxs-lookup"><span data-stu-id="4636f-184">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="4636f-185">Azure CLI から Azure へのログイン</span><span class="sxs-lookup"><span data-stu-id="4636f-185">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="4636f-186">Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="4636f-186">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="4636f-187">Azure CLI 2.0 で Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="4636f-187">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="4636f-188">Maven の設定リファレンス</span><span class="sxs-lookup"><span data-stu-id="4636f-188">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure Portal]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
