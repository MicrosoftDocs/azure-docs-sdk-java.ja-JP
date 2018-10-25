---
title: Azure Active Directory 用の Spring Boot Starter の使用方法
description: Azure Active Directory スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: active-directory
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 07/02/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 665768ffe7bec977d553ffa62e1dbd6b968eb9de
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799908"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a>Azure Active Directory 用の Spring Boot Starter の使用方法

## <a name="overview"></a>概要

この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した **[Spring Initializr]** を用いてアプリを作成する方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Spring Initializr を使用してカスタム アプリケーションを作成する

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。

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

1. **組織名**と**初期ドメイン名**を入力します。 ディレクトリの完全な URL をコピーします。このチュートリアルでは後ほど、これを使用してユーザー アカウントを追加します  (例: `wingtiptoysdirectory.onmicrosoft.com`)。操作が完了したら、**[作成]** をクリックします。

   ![Azure Active Directory の名前を指定する][directory-02]

1. Azure Portal の上部にあるツール バーのドロップダウン メニューから、新しく作成した Azure Active Directory を選択します。

   ![Azure Active Directory を選択する][directory-03]

1. ポータル メニューから **[Azure Active Directory]** を選択し、**[プロパティ]** をクリックして **[ディレクトリ ID]** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。

   ![Azure Active Directory ID をコピーする][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Spring Boot アプリのアプリケーション登録を追加する

1. ポータルのメニューで **[Azure Active Directory]** を選択し、**[概要]** をクリックして、**[アプリの登録]** をクリックします。

   ![新しいアプリ登録を追加する][directory-04]

2. **[新しいアプリケーションの登録]** をクリックします。アプリケーションの **名前** を指定し、**[サインオン URL]** に「 http://localhost:8080」と入力して、**[作成]** をクリックします。

   ![新しいアプリ登録を作成する][directory-05]

3. アプリケーション登録が作成されたら、それをクリックします。

   ![アプリ登録を選択する][directory-06]

4. アプリ登録のページが表示されたら、**アプリケーション ID** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。 **[設定]** をクリックし、**[キー]** をクリックします。

   ![アプリの登録キーを作成する][directory-07]

5. **説明**を追加し、新しいキーの**期間**を指定して、**[保存]** をクリックします。**[保存]** アイコンをクリックすると、キーの値が自動的に入力されます。このチュートリアルで後ほど *application.properties* ファイルを構成できるように、キーの値をコピーする必要があります。 (この値は後で取得することはできません)。

   ![アプリの登録キーのパラメーターを指定する][directory-08]

6. アプリ登録のメイン ページで、**[設定]**、**[必要なアクセス許可]** の順にクリックします。

   ![アプリ登録の必要なアクセス許可][directory-09]

7. **[Windows Azure Active Directory]** をクリックします。

   ![[Windows Azure Active Directory] を選択する][directory-10]

8. **[サインインしたユーザーとしてディレクトリにアクセスします]** と **[サインインとユーザー プロファイルの読み取り]** の各チェック ボックスをオンにし、**[保存]** をクリックします。

   ![アクセス許可を有効にする][directory-11]

9. **[必要なアクセス許可]** ページで **[アクセス許可の付与]** をクリックし、メッセージが表示されたら **[はい]** をクリックします。

   ![アクセス許可を付与する][directory-12]

10. アプリ登録のメイン ページで、**[設定]**、**[応答 URL]** の順にクリックします。

    ![応答 URL を編集する][directory-14]

11. 新しい応答 URL として「<http://localhost:8080/login/oauth2/code/azure>」と入力し、**[保存]** をクリックします。

    ![新しい応答 URL を追加する][directory-15]

12. アプリ登録のメイン ページで、**[マニフェスト]** をクリックして `oauth2AllowImplicitFlow` パラメーターの値を `true` に設定し、**[保存]** をクリックします。

    ![アプリケーション マニフェストの構成][directory-16]

    > [!NOTE]
    > 
    > `oauth2AllowImplicitFlow` パラメーターとその他のアプリケーション設定の詳細については、「[Azure Active Directory アプリケーション マニフェスト][AAD app manifest]」をご覧ください。 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>ディレクトリにユーザー アカウントを追加し、そのアカウントをグループに追加する

1. Active Directory の **[概要]** ページで、**[ユーザー]** をクリックします。

   ![ユーザー パネルを開く][directory-17]

1. **[ユーザー]** パネルが表示されたら、**[新しいユーザー]** をクリックします。

   ![新しいユーザー アカウントを追加する][directory-18]

1. **[ユーザー]** パネルが表示されたら、**[名前]** と **[ユーザー名]** に入力します。

   ![ユーザー アカウント情報を入力する][directory-19]

   > [!NOTE]
   > 
   > ユーザー名を入力するときに、以下のように、このチュートリアルで先に出てきたディレクトリの URL を指定する必要があります。
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. **[グループ]** をクリックして、アプリケーションの承認で使用するグループを選択し、**[選択]** をクリックします  (このチュートリアルの目的では、アカウントを _Users_ グループに追加します)。

   ![ユーザーのグループを選択する][directory-20]

1. **[パスワードの表示]** をクリックしてパスワードをコピーします。このチュートリアルで後ほどアプリケーションにログインするときに、これを使用します。

   ![パスワードを表示する][directory-21]

1. **[作成]** をクリックして、新しいユーザー アカウントをディレクトリに追加します。

   ![新しいユーザー アカウントを作成する][directory-22]

## <a name="configure-and-compile-your-spring-boot-application"></a>Spring Boot アプリケーションの構成とコンパイル

1. このチュートリアルで先ほど作成しダウンロードしたプロジェクト アーカイブからディレクトリにファイルを抽出します。

1. プロジェクトの親フォルダーに移動し、テキスト エディターで *pom.xml* ファイルを開きます。

1. Spring OAuth2 セキュリティの依存関係を追加します。次に例を示します。

   ```xml
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-client</artifactId>
   </dependency>
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-jose</artifactId>
   </dependency>
   ```

1. *pom.xml* ファイルを保存して閉じます。

1. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。

1. 前に作成した値を使用して、アプリ登録の設定を指定します。たとえば、以下のとおりです。

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.active-directory-groups=Users
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `azure.activedirectory.tenant-id` | 前の Active Directory の **ディレクトリ ID** を指定します。 |
   | `spring.security.oauth2.client.registration.azure.client-id` | 以前に完了したアプリ登録の**アプリケーション ID** を指定します。 |
   | `spring.security.oauth2.client.registration.azure.client-secret` | 以前に完了したアプリ登録キーの**値** を指定します。 |
   | `azure.activedirectory.active-directory-groups` | 承認に使用する Active Directory グループの一覧を指定します。 |

   > [!NOTE]
   > 
   > *application.properties* ファイルで使用できる値の完全な一覧については、GitHub の「[Azure Active Directory Spring Boot Sample (Azure Active Directory Spring Boot のサンプル)][AAD Spring Boot Sample]」を参照してください。
   >

1. *application.properties* ファイルを保存して閉じます。

1. アプリケーションの Java ソース フォルダーに、"*controller*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/controller*)。

1. *controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

1. 次のコードを入力し、ファイルを保存して閉じます。

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.oauth2.client.OAuth2AuthorizedClient;
   import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
   import org.springframework.ui.Model;

   @RestController
   public class HelloController {
      @Autowired
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String helloWorld() {
         return "Hello World!";
      }
   }
   ```
   > [!NOTE]
   > 
   > `@PreAuthorize("hasRole('')")` メソッドに指定するグループ名には、*application.properties* ファイルの `azure.activedirectory.active-directory-groups` フィールドに指定したグループのいずれかが含まれている必要があります。
   >

   > [!NOTE]
   > 
   > 異なる要求マッピングには異なる承認設定を指定できます。次に例を示します。
   >
   > ``` java
   > public class HelloController {
   >    @Autowired
   >    @PreAuthorize("hasRole('Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('Group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('Group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```
   >    

1. アプリケーションの Java ソース フォルダーに、"*security*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/security*)。

1. *security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

1. 次のコードを入力し、ファイルを保存して閉じます。

    ```java
    package com.wingtiptoys.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.oauth2.client.oidc.userinfo.OidcUserRequest;
    import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
    import org.springframework.security.oauth2.core.oidc.user.OidcUser;

    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Autowired
        private OAuth2UserService<OidcUserRequest, OidcUser> oidcUserService;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .oidcUserService(oidcUserService);
        }
    }
    ```

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、ディレクトリをアプリの *pom.xml* ファイルがあるフォルダーに変更します。

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![アプリをビルドする][build-application]

1. Maven によってアプリケーションがビルドされ、起動したら、Web ブラウザーで <http://localhost:8080> を開きます。ユーザー名とパスワードを入力するように求めるメッセージが表示されます。

   ![アプリケーションへのログイン][application-login]

   > [!NOTE]
   > 
   > 新しいユーザー アカウントに初めてログインする場合、パスワードの変更を求められることがあります。
   > 
   > ![パスワードの変更][update-password]
   > 

1. 正常にログインしたら、コントローラーにサンプルの "Hello World" テキストが表示されます。

   ![正常なログイン][hello-world]

   > [!NOTE]
   > 
   > 承認されていないユーザー アカウントには、**HTTP 403 Unauthorized** メッセージが表示されます。
   >

## <a name="next-steps"></a>次の手順

Azure Active Directory の使用方法の詳細については、次の記事をご覧ください。

* [Azure Active Directory のドキュメント]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

より詳細なサンプルについては、GitHub の「[Azure Active Directory Spring Boot Sample (Azure Active Directory Spring Boot のサンプル)][AAD Spring Boot Sample]」を参照してください。

<!-- URL List -->

[Azure Active Directory のドキュメント]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Java 開発者向けの Azure]: /java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

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
[directory-13]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-13.png
[directory-14]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-14.png
[directory-15]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-15.png
[directory-16]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-16.png
[directory-17]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-17.png
[directory-18]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-18.png
[directory-19]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-19.png
[directory-20]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-20.png
[directory-21]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-21.png
[directory-22]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-22.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
