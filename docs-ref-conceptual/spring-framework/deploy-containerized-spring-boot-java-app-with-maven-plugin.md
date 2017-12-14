---
title: "Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法"
description: "Azure Web Apps 用の Maven プラグインを使用して、Spring Boot アプリを Azure にデプロイする方法について説明します。"
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 1ab19a4805884773239c4d99090b9e117b3859cd
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-containerized-spring-boot-app-to-azure"></a>Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法

この記事では、[Azure Web Apps 用の Maven プラグイン](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin)を使用して、Docker コンテナーの Spring Boot サンプル アプリケーションを Azure App Services にデプロイする方法について説明します。

> [!NOTE]
> 
> [Apache Maven](http://maven.apache.org/) 用の Azure Web Apps 用 Maven プラグイン は、Maven プロジェクトに Azure App Service をシームレスに統合し、開発者が Web アプリを Azure App Service にデプロイするプロセスを効率化します。
> 
> Azure Web Apps の Maven プラグインは現在プレビューとして提供されています。 今後、機能が追加される予定ですが、現在は FTP 発行のみがサポートされています。
> 

## <a name="prerequisites"></a>前提条件

このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* [Azure コマンド ライン インターフェイス (CLI)]。
* 最新の Java Development Kit (JDK) (バージョン 1.7 以降)。
* Apache の [Maven] 構築ツール (バージョン 3)。
* [Git] クライアント。
* [Docker] クライアント。

> [!NOTE]
>
> このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a>Docker Web アプリの Spring Boot サンプルの複製

このセクションでは、コンテナー化された Spring Boot アプリケーションを複製してローカルでテストします。

1. コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- または --
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. [Docker での Spring Boot の使用開始]のサンプル プロジェクトを今作成したディレクトリに複製します。次に例を示します。
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. 完成したプロジェクトにディレクトリを変更します。次に例を示します。
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Maven を使用して JAR ファイルを構築します。次に例を示します。
   ```shell
   mvn clean package
   ```

1. Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。
   ```shell
   mvn spring-boot:run
   ```

1. Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。 たとえば、curl を使用できる場合は次のようなコマンドを実行できます。
   ```shell
   curl http://localhost:8080
   ```

1. 次のメッセージが表示されるはずです。**Hello Docker World**

## <a name="create-an-azure-service-principal"></a>Azure サービス プリンシパルの作成

このセクションでは、Azure にコンテナーをデプロイするときに、Maven プラグインが使用する Azure サービス プリンシパルを作成します。

1. コマンド プロンプトを開きます。

1. Azure CLI を使って、Azure アカウントにサインインします。
   ```shell
   az login
   ```
   指示に従って、サインインを完了します。

1. Azure サービス プリンシパルを作成します。
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   ここでは `uuuuuuuu` がサービス プリンシパルのユーザー名で、`pppppppp` がパスワードです。

1. Azure が次の例に類似する JSON で応答します。
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
   > Maven プラグインを構成して Azure にコンテナーをデプロイするときに、この JSON の応答にある値を使用します。 `aaaaaaaa``uuuuuuuu``pppppppp``tttttttt` はプレースホルダーの値であり、次のセクションで Maven の `settings.xml` ファイルを構成するときにこれらの値を値の各要素にマップしやすくするために、ここでこのように使用しています。
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Maven を構成して Azure サービス プリンシパルを使用する

このセクションでは、Azure サービス プリンシパルの値を使用して、コンテナーを Azure にデプロイするときに Maven が使用する認証を構成します。

1. Maven の `settings.xml` ファイルをテキスト エディターで開くと、次の例のようにパスが記載されていることがあります。
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. このチュートリアルの前のセクションで説明した Azure サービス プリンシパルの設定を、次の例のように *settings.xml* ファイルの `<servers>` コレクションに追加します。

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
   各値の説明:
   要素 | Description
   ---|---|---
   `<id>` | Web アプリを Azure にデプロイするとき、セキュリティ設定を検索するために Maven が使う一意の名前を指定します。
   `<client>` | サービス プリンシパルの `appId` 値が含まれています。
   `<tenant>` | サービス プリンシパルの `tenant` 値が含まれています。
   `<key>` | サービス プリンシパルの `password` 値が含まれています。
   `<environment>` | ターゲットの Azure クラウド環境を定義します。この例では `AZURE` です  (環境の全リストは、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載しています)

1. *settings.xml* ファイルを保存して閉じます。

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a>省略可能: ローカルの Docker ファイルを Docker Hub にデプロイします

Docker アカウントがあれば、Docker コンテナー イメージをローカルで構築して Docker Hub にプッシュできます。 そのためには、次の手順に従います。

1. Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開きます。

1. `<containerSettings>` 要素の `<imageName>` 子要素を見つけます。

1. `${docker.image.prefix}` の値を Docker アカウント名に更新します。
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. 次のいずれかのデプロイ方法を選択してください。

   * Maven でコンテナー イメージをローカルで構築し、Docker を使用してコンテナーを Docker Hub にプッシュします。
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * [Maven 用の Docker プラグイン]がインストールされていれば、`-DpushImage` パラメーターを使用してコンテナー イメージを自動で Docker Hub に構築できます。
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a>省略可能: コンテナーを Azure にデプロイする前に pom.xml をカスタマイズします

Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開き、`azure-webapp-maven-plugin` の `<plugin>` 要素を見つけます。 この要素は次の例のようになっています。

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

Maven プラグイン用に変更できる値は複数あります。これらの要素に関する詳しい説明はそれぞれ「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載されています。 この記事でも、次のように重要な値については説明します。

要素 | Description
---|---|---
`<version>` | [Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]のバージョンを指定します。 最新バージョンを使用していることを確認するために、[Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) で一覧表示されているバージョンを確認してください。
`<authentication>` | Azure の認証情報を指定します。この例では `azure-auth` を含む `<serverId>` 要素が認証情報です。Maven はこの値を、この記事の前のセクションで定義した Maven の*settings.xml* ファイル内にある Azure サービス プリンシパルを見つけるために使います。
`<resourceGroup>` | ターゲット リソース グループを指定します。この例では `maven-plugin` です。 リソース グループが存在しない場合は、デプロイ中に新しいリソース グループが作成されます。
`<appName>` | Web アプリのターゲット名を指定します。 この例では、ターゲット名は `maven-linux-app-${maven.build.timestamp}` です。混乱を避けるため、この例ではサフィックスの `${maven.build.timestamp}` を追加しています  (タイムスタンプは省略可能です。アプリ名には一意の文字列を指定できます)。
`<region>` | ターゲット リージョンを指定します。この例では `westus` です  (全リストは、「[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]」のドキュメントに記載しています。)
`<appSettings>` | Azure に Web アプリをデプロイするときに使用するために、Maven 用の一意の設定を指定します。 この例では、`<property>` 要素には、アプリのポートを指定する子要素の名前と値のペアが含まれています。

> [!NOTE]
>
> 既定と異なるポートに変更する場合のみ、この例のポート番号を変更する設定が必要になります。
>

## <a name="build-and-deploy-your-container-to-azure"></a>Azure にコンテナーを構築してデプロイする

この記事の前のセクションで説明した設定をすべて構成したら、次は Azure にコンテナーをデプロイします。 そのためには、次の手順を実行してください。

1. *pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。
   ```shell
   mvn clean package
   ```

1. Maven を使って次の例のように Azure に Web アプリをデプロイします。
   ```shell
   mvn azure-webapp:deploy
   ```

Maven が Web アプリを Azure にデプロイします。Web アプリが存在しない場合は新たに作成されます。

> [!NOTE]
>
> デプロイ開始時に、*pom.xml* ファイルの `<region>` 要素で指定したリージョンに十分な数の使用可能なサーバーがない場合は、次の例のようなエラーが表示されることがあります。
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
> この場合、別のリージョンを指定し、Maven コマンドを再実行してアプリケーションをデプロイできます。
>
>

Web アプリのデプロイが完了すると、[Azure Portal] を使用して Web アプリを管理できるようになります。

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

## <a name="next-steps"></a>次のステップ

この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。

* [Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]

* [Azure CLI から Azure へのログイン](/azure/xplat-cli-connect)

* [Azure Web Apps 用の Maven プラグインを使用して、Spring Boot アプリを Azure App Service にデプロイする方法](deploy-spring-boot-java-app-with-maven-plugin.md)

* [Azure CLI 2.0 で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven の設定リファレンス](https://maven.apache.org/settings.html)

* [Maven 用の Docker プラグイン]

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
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
