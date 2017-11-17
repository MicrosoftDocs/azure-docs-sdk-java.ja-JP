---
title: "Azure Web Apps 用の Maven プラグインを使用して、Spring Boot アプリを Azure にデプロイする方法"
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
ms.date: 10/11/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 32781b3ee923f4d64d4e1a11df73f077281eee40
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a><span data-ttu-id="8c9f3-104">Azure Web Apps 用の Maven プラグインを使用して、Spring Boot アプリを Azure にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="8c9f3-104">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure</span></span>

<span data-ttu-id="8c9f3-105">[Apache Maven](http://maven.apache.org/) 用の [Azure Web Apps 用 Maven プラグイン](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) は、Maven プロジェクトに Azure App Service をシームレスに統合し、開発者が Web アプリを Azure App Service にデプロイする作業を効率化します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-105">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="8c9f3-106">この記事では、Azure Web Apps 用の Maven プラグインを使って、Spring Boot アプリケーションのサンプルを Azure App Service にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-106">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application to Azure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="8c9f3-107">Azure Web Apps の Maven プラグインは現在プレビューとして提供されています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-107">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="8c9f3-108">今後、機能が追加される予定ですが、現在は FTP 発行のみがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-108">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="8c9f3-109">前提条件</span><span class="sxs-lookup"><span data-stu-id="8c9f3-109">Prerequisites</span></span>

<span data-ttu-id="8c9f3-110">このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-110">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="8c9f3-111">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="8c9f3-112">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-112">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="8c9f3-113">最新の Java Development Kit (JDK) (バージョン 1.7 以降)。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="8c9f3-114">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="8c9f3-115">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-115">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="8c9f3-116">Spring Boot Web アプリのサンプルを複製する</span><span class="sxs-lookup"><span data-stu-id="8c9f3-116">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="8c9f3-117">このセクションでは、完成した Spring Boot アプリケーションを複製してローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-117">In this section, you clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="8c9f3-118">コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリを変更します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-118">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="8c9f3-119">-- または --</span><span class="sxs-lookup"><span data-stu-id="8c9f3-119">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="8c9f3-120">[Spring Boot Getting Started] サンプル プロジェクトを作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-120">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="8c9f3-121">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-121">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="8c9f3-122">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-122">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="8c9f3-123">Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-123">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="8c9f3-124">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-124">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="8c9f3-125">たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-125">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="8c9f3-126">**Greetings from Spring Boot! (Spring Boot からのあいさつ)** というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-126">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="8c9f3-127">Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="8c9f3-127">Create an Azure service principal</span></span>

<span data-ttu-id="8c9f3-128">このセクションでは、Azure に Web アプリをデプロイするときに、Maven プラグインが使う Azure サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-128">In this section, you create an Azure service principal that the Maven plugin uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="8c9f3-129">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-129">Open a command prompt.</span></span>

1. <span data-ttu-id="8c9f3-130">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-130">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="8c9f3-131">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-131">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="8c9f3-132">Azure サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-132">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="8c9f3-133">ここでは `uuuuuuuu` がサービス プリンシパルのユーザー名で、`pppppppp` がパスワードです。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-133">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="8c9f3-134">Azure が次の例のような JSON で応答します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-134">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="8c9f3-135">Maven プラグインを構成して Azure に Web アプリをデプロイするときに、この JSON の応答にある値を使います。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-135">You will use the values from this JSON response when you configure the Maven plugin to deploy your web app to Azure.</span></span> <span data-ttu-id="8c9f3-136">`aaaaaaaa`、`uuuuuuuu`、`pppppppp`、`tttttttt` はプレースホルダーの値であり、次のセクションで Maven の `settings.xml` ファイルを構成するときにこれらの値を値の各要素にマップしやすくするために、ここでこのように使います。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-136">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="8c9f3-137">Azure サービス プリンシパルを使用するように Maven を構成する</span><span class="sxs-lookup"><span data-stu-id="8c9f3-137">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="8c9f3-138">このセクションでは、Azure サービス プリンシパルの値を使って、Web アプリを Azure にデプロイするときに Maven が使う認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-138">In this section, you use the values from your Azure service principal to configure the authentication that Maven uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="8c9f3-139">Maven の `settings.xml` ファイルをテキスト エディターで開くと、次の例のようにパスが記載されていることがあります。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-139">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="8c9f3-140">このチュートリアルの前のセクションで説明した Azure サービス プリンシパルの設定を、次の例のように *settings.xml* ファイルの `<servers>` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-140">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="8c9f3-141">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="8c9f3-141">Where:</span></span>
   <span data-ttu-id="8c9f3-142">要素</span><span class="sxs-lookup"><span data-stu-id="8c9f3-142">Element</span></span> | <span data-ttu-id="8c9f3-143">Description</span><span class="sxs-lookup"><span data-stu-id="8c9f3-143">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="8c9f3-144">Web アプリを Azure にデプロイするとき、セキュリティ設定を検索するために Maven が使う一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-144">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="8c9f3-145">サービス プリンシパルの `appId` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-145">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="8c9f3-146">サービス プリンシパルの `tenant` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-146">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="8c9f3-147">サービス プリンシパルの `password` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-147">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="8c9f3-148">ターゲットの Azure クラウド環境を定義します。この例では `AZURE` です </span><span class="sxs-lookup"><span data-stu-id="8c9f3-148">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="8c9f3-149">(環境の全リストは、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載しています)</span><span class="sxs-lookup"><span data-stu-id="8c9f3-149">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="8c9f3-150">*settings.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-150">Save and close the *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a><span data-ttu-id="8c9f3-151">省略可能: Web アプリを Azure にデプロイする前に pom.xml をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-151">OPTIONAL: Customize your pom.xml before deploying your web app to Azure</span></span>

<span data-ttu-id="8c9f3-152">Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開き、`azure-webapp-maven-plugin` の `<plugin>` 要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-152">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="8c9f3-153">この要素は次の例のようになっています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-153">This element should resemble the following example:</span></span>

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="8c9f3-154">Maven プラグイン用に変更できる値は複数あります。これらの要素に関する詳しい説明はそれぞれ「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載されています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-154">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="8c9f3-155">この記事でも、次のように重要な値については説明します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-155">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="8c9f3-156">要素</span><span class="sxs-lookup"><span data-stu-id="8c9f3-156">Element</span></span> | <span data-ttu-id="8c9f3-157">Description</span><span class="sxs-lookup"><span data-stu-id="8c9f3-157">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="8c9f3-158">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]のバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-158">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="8c9f3-159">最新バージョンを使用していることを確認するために、[Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) で一覧表示されているバージョンを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-159">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="8c9f3-160">Azure の認証情報を指定します。この例では `azure-auth` を含む `<serverId>` 要素が認証情報です。Maven はこの値を、この記事の前のセクションで定義した Maven の*settings.xml* ファイル内にある Azure サービス プリンシパルを見つけるために使います。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-160">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="8c9f3-161">ターゲット リソース グループを指定します。この例では `maven-plugin` です。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-161">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="8c9f3-162">リソース グループが存在しない場合は、デプロイ中に新しいリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-162">The resource group is created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="8c9f3-163">Web アプリのターゲット名を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-163">Specifies the target name for your web app.</span></span> <span data-ttu-id="8c9f3-164">この例では、ターゲット名は `maven-web-app-${maven.build.timestamp}` です。混乱を避けるため、この例ではサフィックスの `${maven.build.timestamp}` を追加しています </span><span class="sxs-lookup"><span data-stu-id="8c9f3-164">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="8c9f3-165">(タイムスタンプは省略可能です。アプリ名には一意の文字列を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-165">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="8c9f3-166">ターゲット リージョンを指定します。この例では `westus` です </span><span class="sxs-lookup"><span data-stu-id="8c9f3-166">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="8c9f3-167">(完全なリストについては、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」(Azure Web Apps 用の Maven プラグイン) をご覧ください)。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-167">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<javaVersion>` | <span data-ttu-id="8c9f3-168">Web アプリの Java ランタイム バージョンを指定します </span><span class="sxs-lookup"><span data-stu-id="8c9f3-168">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="8c9f3-169">(完全なリストについては、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」(Azure Web Apps 用の Maven プラグイン) をご覧ください)。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-169">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<deploymentType>` | <span data-ttu-id="8c9f3-170">Web アプリのデプロイの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-170">Specifies deployment type for your web app.</span></span> <span data-ttu-id="8c9f3-171">現時点では `ftp` のみがサポートされていますが、他のデプロイの種類のサポートも開発中です。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-171">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span>
`<resources>` | <span data-ttu-id="8c9f3-172">Web アプリを Azure にデプロイするときに Maven が使うリソースとターゲットの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-172">Specifies resources and target destinations which Maven uses when deploying your web app to Azure.</span></span> <span data-ttu-id="8c9f3-173">この例では、2 つの `<resource>` 要素で、Maven が Web アプリの JAR ファイルと Spring Boot プロジェクトからの *web.config* ファイルをデプロイすることを指定しています。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-173">In this example, two `<resource>` elements specify that Maven will deploy the JAR file for your web app and the *web.config* file from the Spring Boot project.</span></span>

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="8c9f3-174">Web アプリをビルドして Azure にデプロイする</span><span class="sxs-lookup"><span data-stu-id="8c9f3-174">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="8c9f3-175">この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-175">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="8c9f3-176">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="8c9f3-177">*pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-177">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="8c9f3-178">Maven を使って次の例のように Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-178">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="8c9f3-179">Maven が Web アプリを Azure にデプロイします。Web アプリが存在しない場合は新たに作成されます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-179">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="8c9f3-180">Web アプリのデプロイが完了すると、[Azure Portal] を使って Web アプリを管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-180">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="8c9f3-181">Web アプリは **App Services** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-181">Your web app will be listed in **App Services**:</span></span>

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* <span data-ttu-id="8c9f3-183">Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-183">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8c9f3-185">次のステップ</span><span class="sxs-lookup"><span data-stu-id="8c9f3-185">Next steps</span></span>

<span data-ttu-id="8c9f3-186">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8c9f3-186">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="8c9f3-187">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]</span><span class="sxs-lookup"><span data-stu-id="8c9f3-187">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="8c9f3-188">Azure CLI から Azure へのログイン</span><span class="sxs-lookup"><span data-stu-id="8c9f3-188">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="8c9f3-189">Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="8c9f3-189">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="8c9f3-190">Azure CLI 2.0 で Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="8c9f3-190">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="8c9f3-191">Maven の設定リファレンス</span><span class="sxs-lookup"><span data-stu-id="8c9f3-191">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure Portal]: https://portal.azure.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
