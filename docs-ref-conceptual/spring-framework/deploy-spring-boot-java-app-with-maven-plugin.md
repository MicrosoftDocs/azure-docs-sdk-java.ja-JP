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
ms.openlocfilehash: 3610312ed17301131967bd2c047c86656de070e7
ms.sourcegitcommit: f313c14e92f38c54a3a583270ee85cc928cd39d7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689425"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a>Azure App Service 用の Maven プラグインを使って、Spring Boot アプリをクラウドにデプロイする

この記事では、Azure App Service Web Apps 用の Maven プラグインを使って、Spring Boot アプリケーションのサンプルをデプロイする方法について説明します。

> [!NOTE]
> 
> [Apache Maven](http://maven.apache.org/) の [Azure App Service Web Apps 用 Maven プラグイン](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)は、Maven プロジェクトに Azure App Service をシームレスに統合し、開発者が Web アプリを Azure App Service にデプロイするプロセスを効率化します。

Maven プラグインを使用する前に、利用可能なプラグインの最新バージョンを Maven Central で確認してください: [![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22) 

## <a name="prerequisites"></a>前提条件

このチュートリアルの手順を実行するには、次の前提条件を満たしておく必要があります。

* Azure サブスクリプション: Azure サブスクリプションをまだ取得していない場合は、[無料の Azure アカウント]にサインアップできます。
* [Azure コマンド ライン インターフェイス (CLI)]。
* 最新の Java Development Kit (JDK) (バージョン 1.7 以降)。
* Apache の [Maven] 構築ツール (バージョン 3)。
* [Git] クライアント。

## <a name="clone-the-sample-spring-boot-web-app"></a>Spring Boot Web アプリのサンプルを複製する

このセクションでは、完成した Spring Boot アプリケーションを複製し、ローカルでテストします。

1. コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- または --
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. [Spring Boot Getting Started] サンプル プロジェクトを作成したディレクトリに複製します。次に例を示します。
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. 完成したプロジェクトにディレクトリを変更します。次に例を示します。
   ```shell
   cd gs-spring-boot/complete
   ```

1. Maven を使用して JAR ファイルを構築します。次に例を示します。
   ```shell
   mvn clean package
   ```

1. Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。
   ```shell
   mvn spring-boot:run
   ```

1. Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。 たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。
   ```shell
   curl http://localhost:8080
   ```

1. **Greetings from Spring Boot! (Spring Boot からのあいさつ)** というメッセージが表示されます。

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a>Azure App Service への WAR ベースのデプロイ用にプロジェクトを調整する

このセクションでは、Spring Boot プロジェクトを、既定ではランタイムとして Tomcat が提供されている Azure App Service に、WAR ファイルとしてデプロイできるようにすばやく調整します。 そのためには、2 つのファイルを変更します。

- Maven の `pom.xml` ファイル
- `Application` Java クラス

Maven の設定から着手しましょう。

1. `pom.xml` を開きます。

1. 先頭の `<artifactId>` 定義の直後に `<packaging>war</packaging>` を追加します。
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. 次の依存関係を追加します。
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

`Application` クラスを開き、期待どおりに IDE によって新しい依存関係が選択されていたら、次の変更に進みます。

1. Application クラスを `SpringBootServletInitializer` のサブクラスにします。
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. Application クラスに次のメソッドを追加します。
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```

これで、ご自身のアプリケーションを Tomcat その他の任意の Servlet ランタイム (例: Jetty) にデプロイする準備が整いました。

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a>Azure App Service Web Apps 用の Maven プラグインを追加する

このセクションでは、Azure App Service Web Apps への、このアプリケーションのデプロイ全体を自動化する Maven プラグインを追加します。

1. もう一度 `pom.xml` を開きます。

1. `<properties>` の内部で、`maven.build.timestamp.format` プロパティを使用してカスタムのタイムスタンプ形式を設定します。 Azure App Service によってご自身のアプリケーションのパブリック URL が作成されるため、この設定を使用してデプロイの名前が生成されることで、他のユーザーのライブ デプロイとの競合を回避できます。
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. `<plugins>` 要素に、次を追加します。
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

これらの設定によって、Maven プロジェクトを Azure App Service Web App にライブ デプロイする準備が整いました。

## <a name="install-and-log-in-to-azure-cli"></a>Azure CLI のインストールとログイン

Maven プラグインで最もシンプルかつ容易に Spring Boot アプリケーションをデプロイするには、[Azure CLI](https://docs.microsoft.com/cli/azure/) を使用します。 これがインストールされていることを確認してください。

1. Azure CLI を使って、Azure アカウントにサインインします。
   ```shell
   az login
   ```
   指示に従って、サインインを完了します。

## <a name="optionally-customize-pomxml-before-deploying"></a>必要に応じて、デプロイの前に pom.xml をカスタマイズする

Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開き、`azure-webapp-maven-plugin` の `<plugin>` 要素を見つけます。 この要素は次の例のようになっています。

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

Maven プラグイン用に変更できる値は複数あります。これらの要素に関する詳しい説明はそれぞれ「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載されています。 この記事でも、次のように重要な値については説明します。

| 要素 | 説明 |
|---|---|
| `<version>` | [Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]のバージョンを指定します。 [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) に表示されているバージョンを確認して、最新バージョンを使用していることを確認してください。 |
| `<resourceGroup>` | ターゲット リソース グループを指定します。この例では `maven-plugin` です。 リソース グループが存在しない場合は、デプロイ中に新しいリソース グループが作成されます。 |
| `<appName>` | Web アプリのターゲット名を指定します。 この例では、ターゲット名は `maven-web-app-${maven.build.timestamp}` です。混乱を避けるため、この例ではサフィックスの `${maven.build.timestamp}` を追加しています  (タイムスタンプは省略可能です。アプリ名には一意の文字列を指定できます)。 |
| `<region>` | ターゲット リージョンを指定します。この例では `westus` です  (完全なリストについては、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」 をご覧ください)。 |
| `<javaVersion>` | Web アプリの Java ランタイム バージョンを指定します  (完全なリストについては、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」(Azure Web Apps 用の Maven プラグイン) をご覧ください)。 |
| `<deploymentType>` | Web アプリのデプロイの種類を指定します。 既定値は `war` です。 |

## <a name="build-and-deploy-your-web-app-to-azure"></a>Web アプリをビルドして Azure にデプロイする

この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。 そのためには、次の手順を実行してください。

1. *pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。
   ```shell
   mvn clean package
   ```

1. Maven を使って次の例のように Azure に Web アプリをデプロイします。
   ```shell
   mvn azure-webapp:deploy
   ```

Maven が Web アプリを Azure にデプロイします。Web アプリが存在しない場合は新たに作成されます。

Web アプリのデプロイが完了すると、[Azure Portal] を使って Web アプリを管理できるようになります。

* Web アプリは **App Services** に一覧表示されます。

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。

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

## <a name="next-steps"></a>次の手順

この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。

* [Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]

* [Azure CLI から Azure へのログイン](/azure/xplat-cli-connect)

* [Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [Azure CLI 2.0 で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven の設定リファレンス](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure Portal]: https://portal.azure.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
