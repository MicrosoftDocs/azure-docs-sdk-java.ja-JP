---
title: Azure Active Directory B2C 用の Spring Boot Starter の使用方法
description: Azure Active Directory B2C スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: active-directory-b2c
documentationcenter: java
author: panli
manager: kevinzha
editor: panli
ms.assetid: ''
ms.author: panli
ms.date: 02/28/2019
ms.devlang: java
ms.service: active-directory-b2c
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 21aa6c812a519d686356851a57f00688d9a24fa4
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625708"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a><span data-ttu-id="d9e38-103">チュートリアル:Azure Active Directory B2C 用の Spring Boot Starter を使用して Java Web アプリをセキュリティで保護する</span><span class="sxs-lookup"><span data-stu-id="d9e38-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory B2C.</span></span>

## <a name="overview"></a><span data-ttu-id="d9e38-104">概要</span><span class="sxs-lookup"><span data-stu-id="d9e38-104">Overview</span></span>

<span data-ttu-id="d9e38-105">この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した [Spring Initializr](https://start.spring.io/) で Java アプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-105">This article demonstrates creating a Java app with the [Spring Initializr](https://start.spring.io/) that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d9e38-106">このチュートリアルでは、以下の内容を学習します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d9e38-107">Spring Initializr を使用して Java アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="d9e38-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="d9e38-108">Azure Active Directory B2C を構成する</span><span class="sxs-lookup"><span data-stu-id="d9e38-108">Configure Azure Active Directory B2C</span></span>
> * <span data-ttu-id="d9e38-109">Spring Boot クラスと注釈を使用してアプリケーションをセキュリティで保護する</span><span class="sxs-lookup"><span data-stu-id="d9e38-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="d9e38-110">Java アプリケーションをビルドしてテストする</span><span class="sxs-lookup"><span data-stu-id="d9e38-110">Build and test your Java application</span></span>

<span data-ttu-id="d9e38-111">Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。</span><span class="sxs-lookup"><span data-stu-id="d9e38-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9e38-112">前提条件</span><span class="sxs-lookup"><span data-stu-id="d9e38-112">Prerequisites</span></span>

<span data-ttu-id="d9e38-113">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="d9e38-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="d9e38-114">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="d9e38-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="d9e38-115">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d9e38-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="d9e38-116">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="d9e38-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="d9e38-117">Spring Initializr を使用したアプリの作成</span><span class="sxs-lookup"><span data-stu-id="d9e38-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="d9e38-118"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-118">Browse to <https://start.spring.io/>.</span></span>

2. <span data-ttu-id="d9e38-119">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Web]** および **[Security]\(セキュリティ\)** モジュールを選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select the **Web** and **Security** module of the Spring Initializr.</span></span>

   ![グループとアーティファクトの名前を指定する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. <span data-ttu-id="d9e38-121">[`Generate Project`]\(プロジェクトの生成\) をクリックし、メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-121">Click `Generate Project`, download the project to a path on your local computer when prompted.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="d9e38-122">Azure Active Directory インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="d9e38-122">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="d9e38-123">Active Directory インスタンスを作成する</span><span class="sxs-lookup"><span data-stu-id="d9e38-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="d9e38-124"><https://portal.azure.com> にログインします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-124">Log into <https://portal.azure.com>.</span></span>

2. <span data-ttu-id="d9e38-125">**[+リソースの作成]** 、 **[ID]** 、 **[Azure Active Directory B2C]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-125">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory B2C**.</span></span>

   ![新しい Azure Active Directory B2C インスタンスを作成する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. <span data-ttu-id="d9e38-127">**組織名**と**初期ドメイン名**を入力し、**ドメイン名**を `${your-tenant-name}` として記録して **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-127">Enter your **Organization name** and your **Initial domain name**, record the **domain name** as your `${your-tenant-name}` and click **Create**.</span></span>

   ![B2C テナント名の取得](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. <span data-ttu-id="d9e38-129">Azure portal ツール バーの右上にあるご自身のアカウント名を選択し、 **[ディレクトリの切り替え]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-129">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![Azure Active Directory を選択する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. <span data-ttu-id="d9e38-131">ドロップダウン メニューから新しく作成した Azure Active Directory を選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-131">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![Azure Active Directory を選択する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. <span data-ttu-id="d9e38-133">`b2c` を検索し、`Azure AD B2C` サービスをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-133">Search `b2c` and click `Azure AD B2C` service.</span></span>

   ![Azure Active Directory B2C インスタンスの検索](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="d9e38-135">Spring Boot アプリのアプリケーション登録を追加する</span><span class="sxs-lookup"><span data-stu-id="d9e38-135">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="d9e38-136">ポータル メニューから **Azure AD B2C** を選択し、 **[アプリケーション]** 、 **[追加]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-136">Select **Azure AD B2C** from the portal menu, click **Applications**, and then click **Add**.</span></span>

   ![新しいアプリ登録を追加する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. <span data-ttu-id="d9e38-138">アプリケーションの **[名前]** を指定し、 **[応答 URL]** に `http://localhost:8080/home` を追加し、**アプリケーション ID** を `${your-client-id}` として記録して **[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-138">Specify your application **Name**, add `http://localhost:8080/home` for the **Reply URL**, record the **Application ID** as your `${your-client-id}` and then click **Save**.</span></span>

   ![アプリケーションの応答 URL の追加](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. <span data-ttu-id="d9e38-140">ご使用のアプリケーションから **[キー]** を選択し、 **[キーの生成]** をクリックして `${your-client-secret}` を生成してから **[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-140">Select **Keys** from your application, click **Generate key** to generate `${your-client-secret}` and then **Save**.</span></span>

4. <span data-ttu-id="d9e38-141">左側の **[ユーザー フロー]** を選択し、**[新しいユーザー フロー]** を**クリック**します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-141">Select **User flows** on your left, and then **Click** \*\*New user flow \*\*.</span></span>

   ![ユーザー フローの作成](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. <span data-ttu-id="d9e38-143">**[サインアップまたはサインイン]** 、 **[Profile editing]\(プロファイル編集\)** 、 **[パスワードのリセット]** を選択し、それぞれユーザー フローを作成します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-143">Choose **Sign up or in**, **Profile editing** and **Password reset** to create user flows respectively.</span></span> <span data-ttu-id="d9e38-144">ユーザー フローの **[名前]** と **[ユーザー属性と要求]** を指定し、 **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e38-144">Specify your user flow **Name** and **User attributes and claims**, click **Create**.</span></span>

   ![ユーザー フローの構成](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="d9e38-146">アプリの構成およびコンパイル</span><span class="sxs-lookup"><span data-stu-id="d9e38-146">Configure and compile your app</span></span>

1. <span data-ttu-id="d9e38-147">このチュートリアルで先ほど作成しダウンロードしたプロジェクト アーカイブからディレクトリにファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-147">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

2. <span data-ttu-id="d9e38-148">プロジェクトの親フォルダーに移動し、テキスト エディターで `pom.xml` Maven プロジェクト ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-148">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

3. <span data-ttu-id="d9e38-149">Spring OAuth2 セキュリティの依存関係を `pom.xml` に追加します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-149">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-active-directory-b2c-spring-boot-starter</artifactId>
       <version>2.1.6.M2</version>
   </dependency>
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   <dependency>
       <groupId>org.thymeleaf.extras</groupId>
       <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   </dependency>
   ```

4. <span data-ttu-id="d9e38-150">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-150">Save and close the *pom.xml* file.</span></span>

5. <span data-ttu-id="d9e38-151">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.yml* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-151">Navigate to the *src/main/resources* folder in your project and open the *application.yml* file in a text editor.</span></span>

6. <span data-ttu-id="d9e38-152">前に作成した値を使用して、アプリ登録の設定を指定します。たとえば、以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d9e38-152">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

   ```yaml
   azure:
     activedirectory:
       b2c:
         tenant: ${your-tenant-name}
         client-id: ${your-client-id}
         client-secret: ${your-client-secret}
         reply-url: ${your-reply-url-from-aad} # should be absolute url.
         logout-success-url: ${you-logout-success-url}
         user-flows:
           sign-up-or-sign-in: ${your-sign-up-or-in-user-flow}
           profile-edit: ${your-profile-edit-user-flow}     # optional
           password-reset: ${your-password-reset-user-flow} # optional
   ```
   <span data-ttu-id="d9e38-153">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="d9e38-153">Where:</span></span>

   | <span data-ttu-id="d9e38-154">パラメーター</span><span class="sxs-lookup"><span data-stu-id="d9e38-154">Parameter</span></span> | <span data-ttu-id="d9e38-155">説明</span><span class="sxs-lookup"><span data-stu-id="d9e38-155">Description</span></span> |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | <span data-ttu-id="d9e38-156">前の AD B2C の `${your-tenant-name` を指定します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-156">Contains your AD B2C's `${your-tenant-name` from earlier.</span></span> |
   | `azure.activedirectory.b2c.client-id` | <span data-ttu-id="d9e38-157">以前に完了したアプリの `${your-client-id}` を指定します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-157">Contains the `${your-client-id}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.client-secret` | <span data-ttu-id="d9e38-158">以前に完了したアプリの `${your-client-secret}` を指定します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-158">Contains the `${your-client-secret}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.reply-url` | <span data-ttu-id="d9e38-159">以前に完了したアプリの**応答 URL** のいずれかを指定します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-159">Contains one of the **Reply URL** from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.logout-success-url` | <span data-ttu-id="d9e38-160">アプリケーションが正常にログアウトしたときの URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-160">Specify the URL when your application logout successfully.</span></span> |
   | `azure.activedirectory.b2c.user-flows` | <span data-ttu-id="d9e38-161">以前に完了したユーザー フローの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-161">Contains the name of the user flows that you completed earlier.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="d9e38-162">*application.yml* ファイルで使用できる値の完全な一覧については、GitHub の [Azure Active Directory Spring Boot Sample]\(Azure Active Directory Spring Boot のサンプル\)[AAD B2C Spring Boot Sample]\(AAD B2C Spring Boot のサンプル\) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d9e38-162">For a full list of values that are available in your *application.yml* file, see the [Azure Active Directory B2C Spring Boot Sample][AAD B2C Spring Boot Sample] on GitHub.</span></span>
   >

7. <span data-ttu-id="d9e38-163">*application.yml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-163">Save and close the *application.yml* file.</span></span>

8. <span data-ttu-id="d9e38-164">アプリケーションの Java ソース フォルダー内に *controller* という名前のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-164">Create a folder named *controller* in the Java source folder for your application.</span></span>

9. <span data-ttu-id="d9e38-165">*controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-165">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

10. <span data-ttu-id="d9e38-166">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-166">Enter the following code, then save and close the file:</span></span>

    ```java
    package sample.aad.controller;
    
    import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
    import org.springframework.security.oauth2.core.user.OAuth2User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    
    @Controller
    public class WebController {
    
        private void initializeModel(Model model, OAuth2AuthenticationToken token) {
            if (token != null) {
                final OAuth2User user = token.getPrincipal();
    
                model.addAttribute("grant_type", user.getAuthorities());
                model.addAllAttributes(user.getAttributes());
            }
        }
    
        @GetMapping(value = "/")
        public String index(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "home";
        }
    
        @GetMapping(value = "/greeting")
        public String greeting(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "greeting";
        }
    
        @GetMapping(value = "/home")
        public String home(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);
    
            return "home";
        }
    }
    ```

11. <span data-ttu-id="d9e38-167">アプリケーションの Java ソース フォルダー内に *security* という名前のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-167">Create a folder named *security* in the Java source folder for your application.</span></span>

12. <span data-ttu-id="d9e38-168">*security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-168">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

13. <span data-ttu-id="d9e38-169">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-169">Enter the following code, then save and close the file:</span></span>

    ```java
    package sample.aad.security;
    
    import com.microsoft.azure.spring.autoconfigure.b2c.AADB2COidcLoginConfigurer;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    
    @EnableWebSecurity
    public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {
    
        private final AADB2COidcLoginConfigurer configurer;
    
        public WebSecurityConfiguration(AADB2COidcLoginConfigurer configurer) {
            this.configurer = configurer;
        }
    
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .anyRequest()
                    .authenticated()
                    .and()
                    .apply(configurer)
            ;
        }
    }
    ```
14. <span data-ttu-id="d9e38-170">`greeting.html` と `home.html` を [Azure AD B2C Spring Boot サンプル](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates)からコピーし、`${your-profile-edit-user-flow}` と `${your-password-reset-user-flow}` をそれぞれ前に完了したユーザー フロー名で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-170">Copy the `greeting.html` and `home.html` from [Azure AD B2C Spring Boot Sample](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates), and replace the `${your-profile-edit-user-flow}` and `${your-password-reset-user-flow}` with your user flow name respectively that completed earlier.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="d9e38-171">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="d9e38-171">Build and test your app</span></span>

1. <span data-ttu-id="d9e38-172">コマンド プロンプトを開き、ディレクトリをアプリの *pom.xml* ファイルがあるフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

2. <span data-ttu-id="d9e38-173">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="d9e38-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. <span data-ttu-id="d9e38-174">Maven によってアプリケーションがビルドされ、起動したら、Web ブラウザーで <http://localhost:8080/> を開きます。ログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-174">After your application is built and started by Maven, open <http://localhost:8080/> in a web browser; you should be redirected to login page.</span></span>

   ![ログイン ページ](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. <span data-ttu-id="d9e38-176">`${your-sign-up-or-in}` ユーザー フローの名前を持つリンクをクリックすると、Azure AD B2C にリダイレクトされ、認証プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-176">Click linke with name of `${your-sign-up-or-in}` user flow, you should be rediected Azure AD B2C to start the authentication process.</span></span>

   ![Azure AD B2C のログイン](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. <span data-ttu-id="d9e38-178">正常にログインすると、ブラウザーにサンプルの `home page` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d9e38-178">After you have logged in successfully, you should see the sample `home page` from the browser,</span></span>

   ![正常なログイン](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a><span data-ttu-id="d9e38-180">まとめ</span><span class="sxs-lookup"><span data-stu-id="d9e38-180">Summary</span></span>

<span data-ttu-id="d9e38-181">このチュートリアルでは、Azure Active Directory B2C スターターを使用した新しい Java Web アプリケーションの作成、新しい Azure AD B2C テナントの構成とそのテナントへの新しいアプリケーションの登録を行いました。また、Spring の注釈とクラスを使用して Web を保護するようにアプリケーションを構成しました。</span><span class="sxs-lookup"><span data-stu-id="d9e38-181">In this tutorial, you created a new Java web application using the Azure Active Directory B2C starter, configured a new Azure AD B2C tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9e38-182">次の手順</span><span class="sxs-lookup"><span data-stu-id="d9e38-182">Next steps</span></span>

<span data-ttu-id="d9e38-183">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="d9e38-183">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d9e38-184">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="d9e38-184">Spring on Azure</span></span>](/java/azure/spring-framework)
