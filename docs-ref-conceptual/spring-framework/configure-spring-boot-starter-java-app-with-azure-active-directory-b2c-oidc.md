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
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a>チュートリアル:Azure Active Directory B2C 用の Spring Boot Starter を使用して Java Web アプリをセキュリティで保護する

## <a name="overview"></a>概要

この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した [Spring Initializr](https://start.spring.io/) で Java アプリを作成する方法について説明します。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> * Spring Initializr を使用して Java アプリケーションを作成する
> * Azure Active Directory B2C を構成する
> * Spring Boot クラスと注釈を使用してアプリケーションをセキュリティで保護する
> * Java アプリケーションをビルドしてテストする

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-an-app-using-spring-initializr"></a>Spring Initializr を使用したアプリの作成

1. <https://start.spring.io/> を参照します。

2. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Web]** および **[Security]\(セキュリティ\)** モジュールを選択します。

   ![グループとアーティファクトの名前を指定する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. [`Generate Project`]\(プロジェクトの生成\) をクリックし、メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

## <a name="create-azure-active-directory-instance"></a>Azure Active Directory インスタンスの作成

### <a name="create-the-active-directory-instance"></a>Active Directory インスタンスを作成する

1. <https://portal.azure.com> にログインします。

2. **[+リソースの作成]** 、 **[ID]** 、 **[Azure Active Directory B2C]** の順にクリックします。

   ![新しい Azure Active Directory B2C インスタンスを作成する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. **組織名**と**初期ドメイン名**を入力し、**ドメイン名**を `${your-tenant-name}` として記録して **[作成]** をクリックします。

   ![B2C テナント名の取得](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. Azure portal ツール バーの右上にあるご自身のアカウント名を選択し、 **[ディレクトリの切り替え]** をクリックします。

   ![Azure Active Directory を選択する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. ドロップダウン メニューから新しく作成した Azure Active Directory を選択します。

   ![Azure Active Directory を選択する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. `b2c` を検索し、`Azure AD B2C` サービスをクリックします。

   ![Azure Active Directory B2C インスタンスの検索](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Spring Boot アプリのアプリケーション登録を追加する

1. ポータル メニューから **Azure AD B2C** を選択し、 **[アプリケーション]** 、 **[追加]** の順にクリックします。

   ![新しいアプリ登録を追加する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. アプリケーションの **[名前]** を指定し、 **[応答 URL]** に `http://localhost:8080/home` を追加し、**アプリケーション ID** を `${your-client-id}` として記録して **[保存]** をクリックします。

   ![アプリケーションの応答 URL の追加](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. ご使用のアプリケーションから **[キー]** を選択し、 **[キーの生成]** をクリックして `${your-client-secret}` を生成してから **[保存]** をクリックします。

4. 左側の **[ユーザー フロー]** を選択し、**[新しいユーザー フロー]** を**クリック**します。

   ![ユーザー フローの作成](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. **[サインアップまたはサインイン]** 、 **[Profile editing]\(プロファイル編集\)** 、 **[パスワードのリセット]** を選択し、それぞれユーザー フローを作成します。 ユーザー フローの **[名前]** と **[ユーザー属性と要求]** を指定し、 **[作成]** をクリックします。

   ![ユーザー フローの構成](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a>アプリの構成およびコンパイル

1. このチュートリアルで先ほど作成しダウンロードしたプロジェクト アーカイブからディレクトリにファイルを抽出します。

2. プロジェクトの親フォルダーに移動し、テキスト エディターで `pom.xml` Maven プロジェクト ファイルを開きます。

3. Spring OAuth2 セキュリティの依存関係を `pom.xml` に追加します。

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

4. *pom.xml* ファイルを保存して閉じます。

5. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.yml* ファイルを開きます。

6. 前に作成した値を使用して、アプリ登録の設定を指定します。たとえば、以下のとおりです。

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
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | 前の AD B2C の `${your-tenant-name` を指定します。 |
   | `azure.activedirectory.b2c.client-id` | 以前に完了したアプリの `${your-client-id}` を指定します。 |
   | `azure.activedirectory.b2c.client-secret` | 以前に完了したアプリの `${your-client-secret}` を指定します。 |
   | `azure.activedirectory.b2c.reply-url` | 以前に完了したアプリの**応答 URL** のいずれかを指定します。 |
   | `azure.activedirectory.b2c.logout-success-url` | アプリケーションが正常にログアウトしたときの URL を指定します。 |
   | `azure.activedirectory.b2c.user-flows` | 以前に完了したユーザー フローの名前を指定します。

   > [!NOTE]
   > 
   > *application.yml* ファイルで使用できる値の完全な一覧については、GitHub の [Azure Active Directory Spring Boot Sample]\(Azure Active Directory Spring Boot のサンプル\)[AAD B2C Spring Boot Sample]\(AAD B2C Spring Boot のサンプル\) を参照してください。
   >

7. *application.yml* ファイルを保存して閉じます。

8. アプリケーションの Java ソース フォルダー内に *controller* という名前のフォルダーを作成します。

9. *controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

10. 次のコードを入力し、ファイルを保存して閉じます。

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

11. アプリケーションの Java ソース フォルダー内に *security* という名前のフォルダーを作成します。

12. *security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

13. 次のコードを入力し、ファイルを保存して閉じます。

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
14. `greeting.html` と `home.html` を [Azure AD B2C Spring Boot サンプル](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates)からコピーし、`${your-profile-edit-user-flow}` と `${your-password-reset-user-flow}` をそれぞれ前に完了したユーザー フロー名で置き換えます。

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、ディレクトリをアプリの *pom.xml* ファイルがあるフォルダーに変更します。

2. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. Maven によってアプリケーションがビルドされ、起動したら、Web ブラウザーで <http://localhost:8080/> を開きます。ログイン ページにリダイレクトされます。

   ![ログイン ページ](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. `${your-sign-up-or-in}` ユーザー フローの名前を持つリンクをクリックすると、Azure AD B2C にリダイレクトされ、認証プロセスが開始されます。

   ![Azure AD B2C のログイン](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. 正常にログインすると、ブラウザーにサンプルの `home page` が表示されます。

   ![正常なログイン](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、Azure Active Directory B2C スターターを使用した新しい Java Web アプリケーションの作成、新しい Azure AD B2C テナントの構成とそのテナントへの新しいアプリケーションの登録を行いました。また、Spring の注釈とクラスを使用して Web を保護するようにアプリケーションを構成しました。

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)
