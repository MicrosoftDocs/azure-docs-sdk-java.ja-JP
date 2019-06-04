---
title: Maven と Azure を使用して Spring Boot JAR ファイルのアプリをクラウドにデプロイする
description: Linux 用の Azure Web Apps の Maven プラグインを使って、Spring Boot アプリをクラウドにデプロイする方法について説明します。
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: brborges
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: 5df4ca6ae9f307d937d7dfa0f2c1765f2efde1a1
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625700"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a>Spring Boot JAR ファイルの Web アプリを Linux 用の Azure App Service にデプロイする

この記事では、[Azure App Service Web Apps 用の Maven プラグイン](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)を使用して、Java SE JAR としてパッケージ化された Spring Boot アプリケーションを [Linux の Azure App Services](/azure/app-service/containers/) にデプロイする方法について説明します。 アプリの依存関係、ランタイム、構成を 1 つのデプロイ可能な成果物に統合する必要がある場合は、[Tomcat および WAR ファイル](/azure/app-service/containers/quickstart-java)での Java SE デプロイを選択してください。


Azure サブスクリプションがない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルの手順を完了するには、次をインストールして構成する必要があります。

* [Azure CLI](/cli/azure/)。ローカルで、または [Azure Cloud Shell](https://shell.azure.com) を介して使用。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* Apache の [Maven](https://maven.apache.org/) バージョン 3。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="install-and-sign-in-to-azure-cli"></a>Azure CLI のインストールとサインイン

Maven プラグインで最もシンプルかつ容易に Spring Boot アプリケーションをデプロイするには、[Azure CLI](/cli/azure/) を使用します。

Azure CLI を使って、Azure アカウントにサインインします。
   
   ```shell
   az login
   ```
   
指示に従って、サインインを完了します。

## <a name="clone-the-sample-app"></a>サンプル アプリの複製

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

1. 次のメッセージが表示されます。**Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>Azure App Service 用の Maven プラグインを構成する

このセクションでは、Maven でアプリを Linux 上の Azure App Service にデプロイできるように、Spring Boot プロジェクト `pom.xml` を構成します。

1. コード エディターで `pom.xml` を開きます。

2. pom.xml の `<build>` セクションで、`<plugins>` タグ内に次の `<plugin>` エントリを追加します。

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.5.4</version>
    <configuration>
      <deploymentType>jar</deploymentType>

      <!-- configure app to run on port 80, required by App Service -->
      <appSettings>
        <property> 
          <name>JAVA_OPTS</name> 
          <value>-Dserver.port=80</value> 
        </property> 
      </appSettings>

      <!-- Web App information -->
      <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
      <appName>${WEBAPP_NAME}</appName>
      <region>${REGION}</region>  

      <!-- Java Runtime Stack for Web App on Linux-->
      <linuxRuntime>jre8</linuxRuntime>
    </configuration>
   </plugin>
   ```

3. プラグイン構成で、次のプレースホルダーを更新します。

| プレースホルダー | 説明 |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | Web アプリの作成先となる新しいリソース グループの名前。 アプリのすべてのリソースを 1 つのグループ内に配置することで、それらを一緒に管理できます。 たとえば、リソース グループを削除すれば、そのアプリに関連付けられているすべてのリソースが削除されます。 この値を一意の新しいリソース グループ名 (たとえば、*TestResources*) で更新します。 このリソース グループ名を使用して、後のセクションですべての Azure リソースをクリーンアップします。 |
| `WEBAPP_NAME` | Azure にデプロイされると、このアプリ名は Web アプリのホスト名の一部になります (WEBAPP_NAME.azurewebsites.net)。 この値を、Java アプリをホストする新しい Azure Web アプリの一意の名前 (たとえば、*contoso*) で更新します。 |
| `REGION` | Web アプリがホストされている Azure リージョン (たとえば、`westus2`)。 リージョンの一覧は、`az account list-locations` コマンドを使用して Cloud Shell または CLI から取得できます。 |

構成オプションの完全な一覧については、[GitHub の Maven プラグイン リファレンス](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)をご覧ください。

## <a name="deploy-the-app-to-azure"></a>Azure にアプリケーションをデプロイする

この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。 そのためには、次の手順を実行してください。

1. *pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。
   ```shell
   mvn clean package
   ```

1. Maven を使って次の例のように Azure に Web アプリをデプロイします。
   ```shell
   mvn azure-webapp:deploy
   ```

Maven によって、ご自身の Web アプリが Azure にデプロイされます。Web アプリまたは Web アプリ プランが存在しない場合は、Maven によって新たに作成されます。

Web アプリのデプロイが完了すると、[Azure portal] で Web アプリを管理できるようになります。

* Web アプリは **App Services** に一覧表示されます。

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。

   ![Web アプリの URL の決定][AP02]

前と同じ cURL コマンドを使用して、`localhost` ではなくポータルからご自身の Web アプリ URL を使って、デプロイが適切に行われたことを確認します。 次のメッセージが表示されます。**Greetings from Spring Boot!** 

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>その他のリソース

この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。

* [Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]

* [Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [Azure CLI 2.0 で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven の設定リファレンス](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /java/azure/
[Azure Portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
