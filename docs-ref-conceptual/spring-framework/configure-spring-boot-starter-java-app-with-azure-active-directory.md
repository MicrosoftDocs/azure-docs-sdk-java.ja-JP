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
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a>Azure Active Directory 用の Spring Boot Starter の使用方法

## <a name="overview"></a>概要

この記事では、**[Spring Initializr]** の Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用してアプリを作成する方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Spring Initializr を使用してカスタム アプリケーションを作成する

1. <https://start.spring.io/> に移動します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。

   ![グループとアーティファクトの名前を指定する][security-01]

1. 下へスクロールして **[Core]\(コア\)** セクションを表示し、**[Security]\(セキュリティ\)** チェック ボックスをオンにします。また、**[Web]** セクションの **[Web]** チェック ボックスもオンにします。

   ![セキュリティと Web のスターターを選択する][security-02]

1. 下へスクロールして **[Azure]** セクションを表示し、**[Azure Active Directory]** チェック ボックスをオンにします。

   ![Azure Active Directory スターターを選択する][security-03]

1. ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。

   ![Spring Boot プロジェクトを生成する][security-04]

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a>新しい Azure Active Directory インスタンスの作成と構成

### <a name="create-the-active-directory-instance"></a>Active Directory インスタンスを作成する

1. <https://portal.azure.com> にログインします。

1. **[+新規]**、**[セキュリティ + ID]**、**[Azure Active Directory]** の順にクリックします。

   ![新しい Azure Active Directory インスタンスを作成する][directory-01]

1. **組織名**と**初期ドメイン名**を入力し、**[作成]** をクリックします。

   ![Azure Active Directory の名前を指定する][directory-02]

1. Azure Portal の上部にあるツール バーのドロップダウン メニューから、新しく作成した Azure Active Directory を選択します。

   ![Azure Active Directory を選択する][directory-03]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Spring Boot アプリのアプリケーション登録を追加する

1. ポータルのメニューで **[Azure Active Directory]** を選択し、**[概要]** をクリックして、**[アプリの登録]** をクリックします。

   ![新しいアプリ登録を追加する][directory-04]

1. **[新しいアプリケーションの登録]** をクリックします。アプリケーションの**名前**を指定し、**[サインオン URL]** に「 http://localhost:8080 」と入力して、**[作成]** をクリックします。

   ![新しいアプリ登録を作成する][directory-05]

1. アプリケーション登録が作成されたら、それをクリックします。

   ![アプリ登録を選択する][directory-06]

1. アプリ登録のページで、後で使用できるように**アプリケーション ID** をコピーし、**[キー]** をクリックします。

   ![アプリの登録キーを作成する][directory-07]

1. **説明**を追加し、新しいキーの**期間**を指定して、**[保存]** をクリックします。**[保存]** アイコンをクリックすると、キーの値が自動的に入力されます。後で使用できるように、キーの値をコピーする必要があります  (この値は後で取得することはできません)。

   ![アプリの登録キーのパラメーターを指定する][directory-08]

1. アプリ登録のメイン ページで、**[必要なアクセス許可]** をクリックします。

   ![アプリ登録の必要なアクセス許可][directory-09]

1. **[Windows Azure Active Directory]** をクリックします。

   ![[Windows Azure Active Directory] を選択する][directory-10]

1. **[サインインしたユーザーとしてディレクトリにアクセスします]** と **[サインインとユーザー プロファイルの読み取り]** の各チェック ボックスをオンにし、**[保存]** をクリックします。

   ![アクセス許可を有効にする][directory-11]

1. **[必要なアクセス許可]** ページで **[アクセス許可の付与]** をクリックし、メッセージが表示されたら **[はい]** をクリックします。

   ![アクセス許可を付与する][directory-12]

## <a name="configure-and-compile-your-spring-boot-application"></a>Spring Boot アプリケーションの構成とコンパイル

1. ディレクトリにダウンロードしたプロジェクトのパッケージからファイルを抽出します。

1. プロジェクトの親フォルダーに移動し、テキスト エディターで *pom.xml* ファイルを開きます。

1. Spring OAuth2 セキュリティの依存関係を追加します。次に例を示します。

   ```xml
   <dependency>
      <groupId>org.springframework.security.oauth</groupId>
      <artifactId>spring-security-oauth2</artifactId>
   </dependency>
   ```

1. *pom.xml* ファイルを保存して閉じます。

1. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。

1. 前の手順で取得した値を使用して、ストレージ アカウントのキーを追加します。次に例を示します。

   ```yaml
   # Specifies your Active Directory Application ID:
   azure.activedirectory.clientId=11111111-1111-1111-1111-1111111111111111

   # Specifies your secret key:
   azure.activedirectory.clientSecret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.activeDirectoryGroups=Users
   ```
   各値の説明:
   パラメーター | 説明
   ---|---|---
   `azure.activedirectory.clientId` | 前の手順で取得した**アプリケーション ID** を指定します。
   `azure.activedirectory.clientSecret` | 以前に完了したアプリ登録のキー値を指定します。
   `azure.activedirectory.activeDirectoryGroups` | 認証に使用する Active Directory グループの一覧を指定します。


1. *application.properties* ファイルを保存して閉じます。

1. アプリケーションの Java ソース フォルダーに、"*controller*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/controller*)。

1. *controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

1. 次のコードを入力し、ファイルを保存して閉じます。

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

1. アプリケーションの Java ソース フォルダーに、"*security*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/security*)。

1. *security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

1. 次のコードを入力し、ファイルを保存して閉じます。

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

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルがあるフォルダーに変更します。

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   ```

   ![][build-application]

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```



1. Maven でアプリケーションを ビルドし、起動したら、Web ブラウザーで <http://localhost:8080> を開きます。

## <a name="next-steps"></a>次のステップ

Azure Active Directory の使用方法の詳細については、次の記事をご覧ください。

* [Azure Active Directory のドキュメント]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Java Tools for Visual Studio Team Services] を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

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
