---
title: "Azure Active Directory 用の Spring Boot Starter の使用方法"
description: "Azure Active Directory スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。"
services: active-directory
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: robmcm
ms.openlocfilehash: a999e33674ea01e776db10186e8af83ce157ef20
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="5b922-103">Azure Active Directory 用の Spring Boot Starter の使用方法</span><span class="sxs-lookup"><span data-stu-id="5b922-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="5b922-104">概要</span><span class="sxs-lookup"><span data-stu-id="5b922-104">Overview</span></span>

<span data-ttu-id="5b922-105">この記事では、**[Spring Initializr]** の Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用してアプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5b922-105">This article demonstrates creating an app with the **[Spring Initializr]** which the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b922-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="5b922-106">Prerequisites</span></span>

<span data-ttu-id="5b922-107">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="5b922-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="5b922-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="5b922-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="5b922-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="5b922-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="5b922-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="5b922-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="5b922-111">Spring Initializr を使用してカスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="5b922-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="5b922-112"><https://start.spring.io/> に移動します。</span><span class="sxs-lookup"><span data-stu-id="5b922-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="5b922-113">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![グループとアーティファクトの名前を指定する][security-01]

1. <span data-ttu-id="5b922-115">下へスクロールして **[Core]\(コア\)** セクションを表示し、**[Security]\(セキュリティ\)** チェック ボックスをオンにします。また、**[Web]** セクションの **[Web]** チェック ボックスもオンにします。</span><span class="sxs-lookup"><span data-stu-id="5b922-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![セキュリティと Web のスターターを選択する][security-02]

1. <span data-ttu-id="5b922-117">下へスクロールして **[Azure]** セクションを表示し、**[Azure Active Directory]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="5b922-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![Azure Active Directory スターターを選択する][security-03]

1. <span data-ttu-id="5b922-119">ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Boot プロジェクトを生成する][security-04]

1. <span data-ttu-id="5b922-121">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="5b922-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="5b922-122">新しい Azure Active Directory インスタンスの作成と構成</span><span class="sxs-lookup"><span data-stu-id="5b922-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="5b922-123">Active Directory インスタンスを作成する</span><span class="sxs-lookup"><span data-stu-id="5b922-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="5b922-124"><https://portal.azure.com> にログインします。</span><span class="sxs-lookup"><span data-stu-id="5b922-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="5b922-125">**[+新規]**、**[セキュリティ + ID]**、**[Azure Active Directory]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![新しい Azure Active Directory インスタンスを作成する][directory-01]

1. <span data-ttu-id="5b922-127">**組織名**と**初期ドメイン名**を入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-127">Enter your **Organization name** and your **Initial domain name**, and then click **Create**.</span></span>

   ![Azure Active Directory の名前を指定する][directory-02]

1. <span data-ttu-id="5b922-129">Azure Portal の上部にあるツール バーのドロップダウン メニューから、新しく作成した Azure Active Directory を選択します。</span><span class="sxs-lookup"><span data-stu-id="5b922-129">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![Azure Active Directory を選択する][directory-03]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="5b922-131">Spring Boot アプリのアプリケーション登録を追加する</span><span class="sxs-lookup"><span data-stu-id="5b922-131">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="5b922-132">ポータルのメニューで **[Azure Active Directory]** を選択し、**[概要]** をクリックして、**[アプリの登録]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-132">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![新しいアプリ登録を追加する][directory-04]

1. <span data-ttu-id="5b922-134">**[新しいアプリケーションの登録]** をクリックします。アプリケーションの**名前**を指定し、**[サインオン URL]** に「http://localhost:8080」と入力して、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-134">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![新しいアプリ登録を作成する][directory-05]

1. <span data-ttu-id="5b922-136">アプリケーション登録が作成されたら、それをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-136">Click your application registration after it has been created.</span></span>

   ![アプリ登録を選択する][directory-06]

1. <span data-ttu-id="5b922-138">アプリ登録のページで、後で使用できるように**アプリケーション ID** をコピーし、**[キー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-138">When the page for your app registration, copy your **Application ID** for later, then click **Keys**.</span></span>

   ![アプリの登録キーを作成する][directory-07]

1. <span data-ttu-id="5b922-140">**説明**を追加し、新しいキーの**期間**を指定して、**[保存]** をクリックします。**[保存]** アイコンをクリックすると、キーの値が自動的に入力されます。後で使用できるように、キーの値をコピーする必要があります </span><span class="sxs-lookup"><span data-stu-id="5b922-140">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key for later.</span></span> <span data-ttu-id="5b922-141">(この値は後で取得することはできません)。</span><span class="sxs-lookup"><span data-stu-id="5b922-141">(You will not be able to retrieve this value later.)</span></span>

   ![アプリの登録キーのパラメーターを指定する][directory-08]

1. <span data-ttu-id="5b922-143">アプリ登録のメイン ページで、**[必要なアクセス許可]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-143">From the main page for your app registration, click **Required permissions**.</span></span>

   ![アプリ登録の必要なアクセス許可][directory-09]

1. <span data-ttu-id="5b922-145">**[Windows Azure Active Directory]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-145">Click **Windows Azure Active Directory**.</span></span>

   ![[Windows Azure Active Directory] を選択する][directory-10]

1. <span data-ttu-id="5b922-147">**[サインインしたユーザーとしてディレクトリにアクセスします]** と **[サインインとユーザー プロファイルの読み取り]** の各チェック ボックスをオンにし、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-147">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![アクセス許可を有効にする][directory-11]

1. <span data-ttu-id="5b922-149">**[必要なアクセス許可]** ページで **[アクセス許可の付与]** をクリックし、メッセージが表示されたら **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b922-149">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![アクセス許可を付与する][directory-12]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="5b922-151">Spring Boot アプリケーションの構成とコンパイル</span><span class="sxs-lookup"><span data-stu-id="5b922-151">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="5b922-152">ディレクトリにダウンロードしたプロジェクトのパッケージからファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="5b922-152">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="5b922-153">プロジェクトの親フォルダーに移動し、テキスト エディターで *pom.xml* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5b922-153">Navigate to the parent folder in your project and open the *pom.xml* file in a text editor.</span></span>

1. <span data-ttu-id="5b922-154">Spring OAuth2 セキュリティの依存関係を追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5b922-154">Add the dependency for Spring OAuth2 security; for example:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.security.oauth</groupId>
      <artifactId>spring-security-oauth2</artifactId>
   </dependency>
   ```

1. <span data-ttu-id="5b922-155">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="5b922-155">Save and close the  the *pom.xml* file.</span></span>

1. <span data-ttu-id="5b922-156">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5b922-156">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="5b922-157">前の手順で取得した値を使用して、ストレージ アカウントのキーを追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5b922-157">Add the key for your storage account using the values from earlier; for example:</span></span>

   ```yaml
   # Specifies your Active Directory Application ID:
   azure.activedirectory.clientId=11111111-1111-1111-1111-1111111111111111

   # Specifies your secret key:
   azure.activedirectory.clientSecret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.activeDirectoryGroups=Users
   ```
   <span data-ttu-id="5b922-158">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="5b922-158">Where:</span></span>
   <span data-ttu-id="5b922-159">パラメーター</span><span class="sxs-lookup"><span data-stu-id="5b922-159">Parameter</span></span> | <span data-ttu-id="5b922-160">説明</span><span class="sxs-lookup"><span data-stu-id="5b922-160">Description</span></span>
   ---|---|---
   `azure.activedirectory.clientId` | <span data-ttu-id="5b922-161">前の手順で取得した**アプリケーション ID** を指定します。</span><span class="sxs-lookup"><span data-stu-id="5b922-161">Contains your **Application ID** from earlier.</span></span>
   `azure.activedirectory.clientSecret` | <span data-ttu-id="5b922-162">以前に完了したアプリ登録のキー値を指定します。</span><span class="sxs-lookup"><span data-stu-id="5b922-162">Contains the key value from your app registration which you completed earlier.</span></span>
   `azure.activedirectory.activeDirectoryGroups` | <span data-ttu-id="5b922-163">認証に使用する Active Directory グループの一覧を指定します。</span><span class="sxs-lookup"><span data-stu-id="5b922-163">Contains a list of Active Directory groups to use for authentication.</span></span>


1. <span data-ttu-id="5b922-164">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="5b922-164">Save and close the  the *application.properties* file.</span></span>

1. <span data-ttu-id="5b922-165">アプリケーションの Java ソース フォルダーに、"*controller*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/controller*)。</span><span class="sxs-lookup"><span data-stu-id="5b922-165">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="5b922-166">*controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="5b922-166">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="5b922-167">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="5b922-167">Enter the following code, then save and close the file:</span></span>

   ```java
   package com.wingtiptoys.security;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.web.authentication.preauth.PreAuthenticatedAuthenticationToken;
   
   @RestController
   public class HelloController {
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String hello() {
         return "Hello World!";
      }
   }
   ```

1. <span data-ttu-id="5b922-168">アプリケーションの Java ソース フォルダーに、"*security*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/security*)。</span><span class="sxs-lookup"><span data-stu-id="5b922-168">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="5b922-169">*security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="5b922-169">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="5b922-170">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="5b922-170">Enter the following code, then save and close the file:</span></span>

   ```java
   package com.wingtiptoys.security;

   import com.microsoft.azure.spring.autoconfigure.aad.AADAuthenticationFilter;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.autoconfigure.security.oauth2.client.EnableOAuth2Sso;
   import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
   import org.springframework.security.web.csrf.CookieCsrfTokenRepository;
   
   @EnableOAuth2Sso
   @EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true)
   
   public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
      @Autowired
      private AADAuthenticationFilter aadAuthFilter;
      @Override
      protected void configure(HttpSecurity http) throws Exception {
         http.authorizeRequests().anyRequest().permitAll();
         http.addFilterBefore(aadAuthFilter, UsernamePasswordAuthenticationFilter.class);
      }
   }
   ```

## <a name="build-and-test-your-app"></a><span data-ttu-id="5b922-171">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="5b922-171">Build and test your app</span></span>

1. <span data-ttu-id="5b922-172">コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルがあるフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="5b922-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="5b922-173">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5b922-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   ```

   ![][build-application]

1. <span data-ttu-id="5b922-174">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5b922-174">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```



1. <span data-ttu-id="5b922-175">Maven でアプリケーションを ビルドし、起動したら、Web ブラウザーで <http://localhost:8080> を開きます。</span><span class="sxs-lookup"><span data-stu-id="5b922-175">After your application is built and started by Maven, open <http://localhost:8080> in a web browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b922-176">次のステップ</span><span class="sxs-lookup"><span data-stu-id="5b922-176">Next steps</span></span>

<span data-ttu-id="5b922-177">Azure Active Directory の使用方法の詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5b922-177">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="5b922-178">[Azure Active Directory のドキュメント]</span><span class="sxs-lookup"><span data-stu-id="5b922-178">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="5b922-179">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5b922-179">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="5b922-180">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="5b922-180">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="5b922-181">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="5b922-181">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="5b922-182">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Java Tools for Visual Studio Team Services] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5b922-182">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="5b922-183">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="5b922-183">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="5b922-184">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="5b922-184">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="5b922-185">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="5b922-185">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="5b922-186">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="5b922-186">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Active Directory のドキュメント]: /azure/active-directory/
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[security-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-01.png
[security-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-02.png
[security-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-03.png
[security-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-04.png

[directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-01.png
[directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-02.png
[directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-03.png
[directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-04.png
[directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-05.png
[directory-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-06.png
[directory-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-07.png
[directory-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-08.png
[directory-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-09.png
[directory-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-10.png
[directory-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-11.png
[directory-12]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-12.png

[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
