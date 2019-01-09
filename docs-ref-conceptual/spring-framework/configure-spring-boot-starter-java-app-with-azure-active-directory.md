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
ms.date: 12/19/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: b3d97917deffa75bfab5f0d9ded64affd90139e1
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991396"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a>チュートリアル:Azure Active Directory 用の Spring Boot Starter を使用して Java Web アプリをセキュリティで保護する

## <a name="overview"></a>概要

この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した **[Spring Initializr]** で Java アプリを作成する方法について説明します。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> * Spring Initializr を使用して Java アプリケーションを作成する
> * Azure Active Directory を構成する
> * Spring Boot クラスと注釈を使用してアプリケーションをセキュリティで保護する
> * Java アプリケーションをビルドしてテストする

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-an-app-using-spring-initializr"></a>Spring Initializr を使用したアプリの作成

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。

   ![グループとアーティファクトの名前を指定する][create-spring-app-01]

1. 下へスクロールして **[Core]\(コア\)** セクションに移動し、**[Security]\(セキュリティ\)** チェックボックスをオンにし、**[Web]** セクションで **[Web]** チェックボックスをオンにします。次に、下へスクロールして **[Azure]** セクションに移動し、**[Azure Active Directory]** チェックボックスをオンにします。

   ![セキュリティ、Web、および Azure Active Directory スターターを選択する][create-spring-app-02]

1. ページの上部または下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。

   ![Spring Boot プロジェクトを生成する][create-spring-app-03]

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

## <a name="create-azure-active-directory-instance"></a>Azure Active Directory インスタンスの作成

### <a name="create-the-active-directory-instance"></a>Active Directory インスタンスを作成する

1. <https://portal.azure.com> にログインします。

1. **[+リソースの作成]**、**[ID]**、**[Azure Active Directory]** の順にクリックします。

   ![新しい Azure Active Directory インスタンスを作成する][create-directory-01]

1. **組織名**と**初期ドメイン名**を入力します。 ディレクトリの完全な URL をコピーします。このチュートリアルでは後ほど、これを使用してユーザー アカウントを追加します  (例: `wingtiptoysdirectory.onmicrosoft.com`)。操作が完了したら、**[作成]** をクリックします。

   ![Azure Active Directory の名前を指定する][create-directory-02]

1. Azure portal ツール バーの右上にあるご自身のアカウント名を選択し、**[ディレクトリの切り替え]** をクリックします。

   ![Azure アカウント名を選択する][create-directory-03]

1. ドロップダウン メニューから新しく作成した Azure Active Directory を選択します。

   ![Azure Active Directory を選択する][create-directory-04]

1. ポータル メニューから **[Azure Active Directory]** を選択し、**[プロパティ]** をクリックして **[ディレクトリ ID]** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。

   ![Azure Active Directory ID をコピーする][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Spring Boot アプリのアプリケーション登録を追加する

1. ポータルのメニューで **[Azure Active Directory]** を選択し、**[アプリの登録]**、**[新しいアプリケーションの登録]** の順にクリックします。

   ![新しいアプリ登録を追加する][create-app-registration-01]

2. アプリケーションの**名前**を指定し、**[サインオン URL]** に「 http://localhost:8080」と入力し、**[作成]** をクリックします。

   ![新しいアプリ登録を作成する][create-app-registration-02]

4. アプリ登録のページが表示されたら、**アプリケーション ID** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。 **[設定]** をクリックし、**[キー]** をクリックします。

   ![アプリの登録キーを作成する][create-app-registration-03]

5. **説明**を追加し、新しいキーの**期間**を指定して、**[保存]** をクリックします。**[保存]** アイコンをクリックすると、キーの値が自動的に入力されます。このチュートリアルで後ほど *application.properties* ファイルを構成できるように、キーの値をコピーする必要があります。 (この値は後で取得することはできません)。

   ![アプリの登録キーのパラメーターを指定する][create-app-registration-04]

6. アプリ登録のメイン ページで、**[設定]**、**[必要なアクセス許可]** の順にクリックします。

   ![アプリ登録の必要なアクセス許可][create-app-registration-05]

7. **[Windows Azure Active Directory]** をクリックします。

   ![[Windows Azure Active Directory] を選択する][create-app-registration-06]

8. **[サインインしたユーザーとしてディレクトリにアクセスします]** と **[サインインとユーザー プロファイルの読み取り]** の各チェック ボックスをオンにし、**[保存]** をクリックします。

   ![アクセス許可を有効にする][create-app-registration-07]

9. **[必要なアクセス許可]** ページで **[アクセス許可の付与]** をクリックし、メッセージが表示されたら **[はい]** をクリックします。

   ![アクセス許可を付与する][create-app-registration-08]

10. アプリ登録のメイン ページで、**[設定]**、**[応答 URL]** の順にクリックします。

    ![応答 URL を編集する][create-app-registration-09]

11. 新しい応答 URL として「<http://localhost:8080/login/oauth2/code/azure>」と入力し、**[保存]** をクリックします。

    ![新しい応答 URL を追加する][create-app-registration-10]

12. アプリ登録のメイン ページで、**[マニフェスト]** をクリックして `oauth2AllowImplicitFlow` パラメーターの値を `true` に設定し、**[保存]** をクリックします。

    ![アプリケーション マニフェストの構成][create-app-registration-11]

    > [!NOTE]
    > 
    > `oauth2AllowImplicitFlow` パラメーターとその他のアプリケーション設定の詳細については、「[Azure Active Directory アプリケーション マニフェスト][AAD app manifest]」をご覧ください。 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>ディレクトリにユーザー アカウントを追加し、そのアカウントをグループに追加する

1. Active Directory の **[概要]** ページで、**[すべてのユーザー]** をクリックし、次に **[新しいユーザー]** をクリックします。

   ![新しいユーザー アカウントを追加する][create-user-01]

1. **[ユーザー]** パネルが表示されたら、**[名前]** と **[ユーザー名]** に入力します。

   ![ユーザー アカウント情報を入力する][create-user-02]

   > [!NOTE]
   > 
   > ユーザー名を入力するときに、以下のように、このチュートリアルで先に出てきたディレクトリの URL を指定する必要があります。
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. **[グループ]** をクリックして、アプリケーションの承認で使用するグループを選択し、**[選択]** をクリックします  (このチュートリアルの目的では、アカウントを _Users_ グループに追加します)。

   ![ユーザーのグループを選択する][create-user-03]

1. **[パスワードの表示]** をクリックしてパスワードをコピーします。このチュートリアルで後ほどアプリケーションにログインするときに、これを使用します。 パスワードをコピーしたら、**[作成]** をクリックして新しいユーザー アカウントをディレクトリに追加します。

   ![パスワードを表示する][create-user-04]

## <a name="configure-and-compile-your-app"></a>アプリの構成およびコンパイル

1. このチュートリアルで先ほど作成しダウンロードしたプロジェクト アーカイブからディレクトリにファイルを抽出します。

1. プロジェクトの親フォルダーに移動し、テキスト エディターで `pom.xml` Maven プロジェクト ファイルを開きます。

1. Spring OAuth2 セキュリティの依存関係を `pom.xml` に追加します。

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
   > 異なる要求マッピングには異なる承認設定を指定することもできます。以下に例を示します。
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

## <a name="summary"></a>まとめ

このチュートリアルでは、Azure Active Directory スターターを使用した新しい Java Web アプリケーションの作成、新しい Azure AD テナントの構成とそのテナントへの新しいアプリケーションの登録を行いました。また、Spring の注釈とクラスを使用して Web を保護するようにアプリケーションを構成しました。

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /java/azure/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png
[create-spring-app-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-02.png
[create-spring-app-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-03.png

[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png
[create-directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-05.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-06.png
[create-app-registration-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-07.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png
[create-app-registration-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-11.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
