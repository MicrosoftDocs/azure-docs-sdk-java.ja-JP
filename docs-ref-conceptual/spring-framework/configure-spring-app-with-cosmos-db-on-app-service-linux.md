---
title: App Service on Linux で Spring と Cosmos DB を使用する方法
description: この記事では、Azure App Service on Linux で Java Web アプリを構築、構成、デプロイ、トラブルシューティング、およびスケーリングするプロセスを、順を追って説明します。
documentationcenter: java
author: bmitchell287
ms.author: brendm; joshuapa
ms.date: 4/24/2019
ms.devlang: java
ms.service: app-service, cosmos-db
ms.topic: article
ms.openlocfilehash: 5d209c0670d6f4265b1237e7b8cff45faa9bb5d8
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568635"
---
# <a name="how-to-use-spring-and-cosmos-db-with-app-service-on-linux"></a>App Service on Linux で Spring と Cosmos DB を使用する方法

## <a name="overview"></a>概要

この記事では、Azure App Service on Linux で Java Web アプリを構築、構成、デプロイ、トラブルシューティング、およびスケーリングするプロセスを、順を追って説明します。

次のコンポーネントの使用方法を説明します。

- [Azure Cosmos DB SQL API を使用した Spring Boot Starter](configure-spring-boot-starter-java-app-with-cosmos-db.md)
- [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction)
- [App Service Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro)

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

- Java Web アプリをクラウドにデプロイするには、Azure サブスクリプションが必要です。 Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、[Azure の無料アカウント]((https://azure.microsoft.com/pricing/free-trial/))にサインアップできます。
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Java 8 JDK](https://docs.microsoft.com/java/azure/jdk/java-jdk-install)
- [Maven 3](http://maven.apache.org/)

## <a name="clone-the-sample-java-web-app-repository"></a>サンプル Java Web アプリ リポジトリを複製する
この演習では、[Spring Boot](https://spring.io/projects/spring-boot)、[Cosmos DB 用の Spring Data](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable)、および [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction) を使用して構築された Java アプリケーションである Spring Todo アプリを使用します。
1. Spring Todo アプリを複製し、 **.prep** フォルダーの内容をコピーしてプロジェクトを初期化します。

    bash の場合:
    ```bash
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    yes | cp -rf .prep/* .
    ```

    Windows の場合:
    ```cmd
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    cd e2e-java-experience-in-app-service-linux-part-2
    xcopy .prep /f /s /e /y
    ```

2. ディレクトリを、複製したリポジトリの以下のフォルダーに変更します。

   ```bash
   cd initial\spring-todo-app
   ``` 
## <a name="create-an-azure-cosmos-db-from-azure-cli"></a>Azure CLI から Azure Cosmos DB を作成する

1. Azure CLI にログインし、サブスクリプション ID を設定します。

    ```bash
    az login
    ```

2. 必要に応じて、サブスクリプション ID を設定します。
    ```bash
    az account set -s <your-subscription-id>
    ```

3. Azure リソース グループを作成し、後で使用するためにそのリソース グループの名前をメモします。

    ```bash
    az group create -n <your-azure-group-name> \
    -l <your-resource-group-region>
    ```

4. Cosmos DB を作成し、タイプとして GlobalDocumentDB を指定します。
Cosmos DB の名前には小文字のみを使用する必要があります。 必ず応答内の `documentEndpoint` フィールドをメモしてください。 これは後で必要になります。

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
     ```

4. Azure Cosmos DB キーを取得し、後で使用するために `primaryMasterKey` 値を記録します。

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="build-and-run-the-app-locally"></a>ローカルでアプリをビルドおよび実行する

1. 選択したコンソール内で、この記事で前に収集した Azure と Cosmos DB の接続情報を使用して、次のコード セクションに示された環境変数を構成します。 **WEBAPP_NAME** に一意の名前を指定し、**REGION** 変数に値を指定する必要があります。

Linux (Bash) の場合:

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Windows (コマンド プロンプト) の場合:
```cmd
set COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
set COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
set COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
set RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
set WEBAPP_NAME=<put-your-Webapp-name-here>
set REGION=<put-your-REGION-here>
```

> [!NOTE]
> スクリプトを使ってこれらの変数をプロビジョニングしたい場合は、.prep ディレクトリに Bash 用のテンプレートがあるため、それをコピーして出発点として使用することができます。

2. ディレクトリを次のように変更します。

    ```bash
    cd initial/spring-todo-app
    ``` 
 
3. 次のコマンドを使用して、Spring Todo アプリをローカルで実行します。

    ```bash
    mvn package spring-boot:run
    ```

4. アプリケーションが起動したら、こちら ([http://localhost:8080/](http://localhost:8080/)) から Spring Todo アプリにアクセスしてデプロイを検証できます。

 ![ローカルで実行されている Spring アプリ][SCDB01]

## <a name="deploy-to-app-service-linux"></a>App Service Linux にデプロイする

1. リポジトリの **initial/spring-todo-app** ディレクトリに以前にコピーした pom.xml ファイルを開きます。 以下の pom.xml ファイルに示されているように、[Azure App Service 用の Maven プラグイン](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)が含まれていることを確認してください。 そのバージョンが **1.6.0** に設定されていない場合は、値を更新してください。

```xml
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.6.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

2. App Service Linux の Java SE にデプロイする

    ```bash
    mvn azure-webapp:deploy
    ```

```bash
// Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.6.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesnt exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlan11111111-1111-1111'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

3. App Service Linux の Java SE で実行されている Web アプリを参照します。

    ```bash
    https://<WEBAPP_NAME>.azurewebsites.net
    ```

![App Service on Linux で実行されている Spring アプリ][SCDB02]

## <a name="troubleshoot-spring-todo-app-on-azure-by-viewing-logs"></a>ログを表示して Azure の Spring Todo アプリをトラブルシューティングする

1. Linux の Azure App Service にデプロイされた Java Web アプリのログを構成します。

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
     --web-server-logging filesystem
    ```
2. ローカル マシンから Java Web アプリのリモート ログ ストリームを開きます。

    ```bash
    az webapp log tail --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME}
     ```

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: http://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers needs non existent service dev
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======


```

3. 終了したら、結果を [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete) のコードと比較できます。


## <a name="scale-out-the-spring-todo-app"></a>Spring Todo アプリをスケールアウトする

1. Azure CLI を使用して Java Web アプリをスケールアウトします。

    ```bash
    az appservice plan update --number-of-workers 2 \
      --name ${WEBAPP_PLAN_NAME} \
      --resource-group ${RESOURCEGROUP_NAME}
    ```

## <a name="next-steps"></a>次の手順

- [App Service Linux の Java 開発ガイド](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-java)
- [Java 開発者向けの Azure](https://docs.microsoft.com/en-us/java/azure/) Spring および Azure の詳細については、Azure の Spring ドキュメント センターにお進みください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、 **[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Java 開発者向けの Azure]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: https://azure.microsoft.com/services/devops/java/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SCDB01]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB01.png
[SCDB02]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB02.png
