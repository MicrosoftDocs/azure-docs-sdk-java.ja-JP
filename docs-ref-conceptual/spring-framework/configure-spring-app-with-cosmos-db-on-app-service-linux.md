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
# <a name="how-to-use-spring-and-cosmos-db-with-app-service-on-linux"></a><span data-ttu-id="8219c-103">App Service on Linux で Spring と Cosmos DB を使用する方法</span><span class="sxs-lookup"><span data-stu-id="8219c-103">How to use Spring and Cosmos DB with App Service on Linux</span></span>

## <a name="overview"></a><span data-ttu-id="8219c-104">概要</span><span class="sxs-lookup"><span data-stu-id="8219c-104">Overview</span></span>

<span data-ttu-id="8219c-105">この記事では、Azure App Service on Linux で Java Web アプリを構築、構成、デプロイ、トラブルシューティング、およびスケーリングするプロセスを、順を追って説明します。</span><span class="sxs-lookup"><span data-stu-id="8219c-105">This article will walk you through the process of building, configuring, deploying, troubleshooting, and scaling Java Web apps in Azure App Service on Linux.</span></span>

<span data-ttu-id="8219c-106">次のコンポーネントの使用方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="8219c-106">It will demonstrate the usage of the following components:</span></span>

- [<span data-ttu-id="8219c-107">Azure Cosmos DB SQL API を使用した Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="8219c-107">Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>](configure-spring-boot-starter-java-app-with-cosmos-db.md)
- [<span data-ttu-id="8219c-108">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8219c-108">Azure Cosmos DB</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction)
- [<span data-ttu-id="8219c-109">App Service Linux</span><span class="sxs-lookup"><span data-stu-id="8219c-109">App Service Linux</span></span>](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro)

## <a name="prerequisites"></a><span data-ttu-id="8219c-110">前提条件</span><span class="sxs-lookup"><span data-stu-id="8219c-110">Prerequisites</span></span>

<span data-ttu-id="8219c-111">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="8219c-111">The following prerequisites are required in order to follow the steps in this article:</span></span>

- <span data-ttu-id="8219c-112">Java Web アプリをクラウドにデプロイするには、Azure サブスクリプションが必要です。</span><span class="sxs-lookup"><span data-stu-id="8219c-112">In order to deploy a Java Web app to cloud, you need an Azure subscription.</span></span> <span data-ttu-id="8219c-113">Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、[Azure の無料アカウント]((https://azure.microsoft.com/pricing/free-trial/))にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="8219c-113">If you do not already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account]((https://azure.microsoft.com/pricing/free-trial/)).</span></span>
- [<span data-ttu-id="8219c-114">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8219c-114">Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [<span data-ttu-id="8219c-115">Java 8 JDK</span><span class="sxs-lookup"><span data-stu-id="8219c-115">Java 8 JDK</span></span>](https://docs.microsoft.com/java/azure/jdk/java-jdk-install)
- [<span data-ttu-id="8219c-116">Maven 3</span><span class="sxs-lookup"><span data-stu-id="8219c-116">Maven 3</span></span>](http://maven.apache.org/)

## <a name="clone-the-sample-java-web-app-repository"></a><span data-ttu-id="8219c-117">サンプル Java Web アプリ リポジトリを複製する</span><span class="sxs-lookup"><span data-stu-id="8219c-117">Clone the Sample Java Web App Repository</span></span>
<span data-ttu-id="8219c-118">この演習では、[Spring Boot](https://spring.io/projects/spring-boot)、[Cosmos DB 用の Spring Data](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable)、および [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction) を使用して構築された Java アプリケーションである Spring Todo アプリを使用します。</span><span class="sxs-lookup"><span data-stu-id="8219c-118">For this exercise you'll be using the Spring Todo app, which is a Java application built using [Spring Boot](https://spring.io/projects/spring-boot), [Spring Data for Cosmos DB](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable) and [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction).</span></span>
1. <span data-ttu-id="8219c-119">Spring Todo アプリを複製し、 **.prep** フォルダーの内容をコピーしてプロジェクトを初期化します。</span><span class="sxs-lookup"><span data-stu-id="8219c-119">Clone the Spring Todo app and copy the contents of the **.prep** folder to initialize the project:</span></span>

    <span data-ttu-id="8219c-120">bash の場合:</span><span class="sxs-lookup"><span data-stu-id="8219c-120">For bash:</span></span>
    ```bash
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    yes | cp -rf .prep/* .
    ```

    <span data-ttu-id="8219c-121">Windows の場合:</span><span class="sxs-lookup"><span data-stu-id="8219c-121">For Windows:</span></span>
    ```cmd
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    cd e2e-java-experience-in-app-service-linux-part-2
    xcopy .prep /f /s /e /y
    ```

2. <span data-ttu-id="8219c-122">ディレクトリを、複製したリポジトリの以下のフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="8219c-122">Change the directory to the following folder in the cloned repo:</span></span>

   ```bash
   cd initial\spring-todo-app
   ``` 
## <a name="create-an-azure-cosmos-db-from-azure-cli"></a><span data-ttu-id="8219c-123">Azure CLI から Azure Cosmos DB を作成する</span><span class="sxs-lookup"><span data-stu-id="8219c-123">Create an Azure Cosmos DB from Azure CLI</span></span>

1. <span data-ttu-id="8219c-124">Azure CLI にログインし、サブスクリプション ID を設定します。</span><span class="sxs-lookup"><span data-stu-id="8219c-124">Login to your Azure CLI, and set your subscription id.</span></span>

    ```bash
    az login
    ```

2. <span data-ttu-id="8219c-125">必要に応じて、サブスクリプション ID を設定します。</span><span class="sxs-lookup"><span data-stu-id="8219c-125">Set the subscription id if needed.</span></span>
    ```bash
    az account set -s <your-subscription-id>
    ```

3. <span data-ttu-id="8219c-126">Azure リソース グループを作成し、後で使用するためにそのリソース グループの名前をメモします。</span><span class="sxs-lookup"><span data-stu-id="8219c-126">Create an Azure resource group, and write down the resource group name for later use.</span></span>

    ```bash
    az group create -n <your-azure-group-name> \
    -l <your-resource-group-region>
    ```

4. <span data-ttu-id="8219c-127">Cosmos DB を作成し、タイプとして GlobalDocumentDB を指定します。</span><span class="sxs-lookup"><span data-stu-id="8219c-127">Create the Cosmos DB and specify the type as GlobalDocumentDB.</span></span>
<span data-ttu-id="8219c-128">Cosmos DB の名前には小文字のみを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8219c-128">The name of the Cosmos DB must use only lower case letters.</span></span> <span data-ttu-id="8219c-129">必ず応答内の `documentEndpoint` フィールドをメモしてください。</span><span class="sxs-lookup"><span data-stu-id="8219c-129">Make sure to note the `documentEndpoint` field in the response.</span></span> <span data-ttu-id="8219c-130">これは後で必要になります。</span><span class="sxs-lookup"><span data-stu-id="8219c-130">You'll need this later.</span></span>

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
     ```

4. <span data-ttu-id="8219c-131">Azure Cosmos DB キーを取得し、後で使用するために `primaryMasterKey` 値を記録します。</span><span class="sxs-lookup"><span data-stu-id="8219c-131">Get your Azure Cosmos DB keys, record the `primaryMasterKey` value for later use.</span></span>

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="build-and-run-the-app-locally"></a><span data-ttu-id="8219c-132">ローカルでアプリをビルドおよび実行する</span><span class="sxs-lookup"><span data-stu-id="8219c-132">Build and Run the App Locally</span></span>

1. <span data-ttu-id="8219c-133">選択したコンソール内で、この記事で前に収集した Azure と Cosmos DB の接続情報を使用して、次のコード セクションに示された環境変数を構成します。</span><span class="sxs-lookup"><span data-stu-id="8219c-133">Within your console of choice configure the environment variables shown in the following code sections with the Azure and Cosmos DB connection information you gathered previously in this article.</span></span> <span data-ttu-id="8219c-134">**WEBAPP_NAME** に一意の名前を指定し、**REGION** 変数に値を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8219c-134">You'll need to provide a unique name for **WEBAPP_NAME** and value for the **REGION** variables.</span></span>

<span data-ttu-id="8219c-135">Linux (Bash) の場合:</span><span class="sxs-lookup"><span data-stu-id="8219c-135">For Linux (Bash):</span></span>

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

<span data-ttu-id="8219c-136">Windows (コマンド プロンプト) の場合:</span><span class="sxs-lookup"><span data-stu-id="8219c-136">For Windows (Command Prompt):</span></span>
```cmd
set COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
set COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
set COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
set RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
set WEBAPP_NAME=<put-your-Webapp-name-here>
set REGION=<put-your-REGION-here>
```

> [!NOTE]
> <span data-ttu-id="8219c-137">スクリプトを使ってこれらの変数をプロビジョニングしたい場合は、.prep ディレクトリに Bash 用のテンプレートがあるため、それをコピーして出発点として使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8219c-137">If you'd like to provision these variables with a script, there is a template for Bash in the .prep directory that you can copy and use as a starting point.</span></span>

2. <span data-ttu-id="8219c-138">ディレクトリを次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="8219c-138">Change the directory to the following:</span></span>

    ```bash
    cd initial/spring-todo-app
    ``` 
 
3. <span data-ttu-id="8219c-139">次のコマンドを使用して、Spring Todo アプリをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="8219c-139">Run the Spring Todo app locally with the following command:</span></span>

    ```bash
    mvn package spring-boot:run
    ```

4. <span data-ttu-id="8219c-140">アプリケーションが起動したら、こちら ([http://localhost:8080/](http://localhost:8080/)) から Spring Todo アプリにアクセスしてデプロイを検証できます。</span><span class="sxs-lookup"><span data-stu-id="8219c-140">Once the application has started,you can validate the deployment by accessing the Spring Todo app here: [http://localhost:8080/](http://localhost:8080/).</span></span>

 ![ローカルで実行されている Spring アプリ][SCDB01]

## <a name="deploy-to-app-service-linux"></a><span data-ttu-id="8219c-142">App Service Linux にデプロイする</span><span class="sxs-lookup"><span data-stu-id="8219c-142">Deploy to App Service Linux</span></span>

1. <span data-ttu-id="8219c-143">リポジトリの **initial/spring-todo-app** ディレクトリに以前にコピーした pom.xml ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8219c-143">Open the pom.xml file that you previously copied to the **initial/spring-todo-app** directory of the repository.</span></span> <span data-ttu-id="8219c-144">以下の pom.xml ファイルに示されているように、[Azure App Service 用の Maven プラグイン](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)が含まれていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8219c-144">Ensure that the [Maven Plugin for Azure App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) is included as seen in the pom.xml file below.</span></span> <span data-ttu-id="8219c-145">そのバージョンが **1.6.0** に設定されていない場合は、値を更新してください。</span><span class="sxs-lookup"><span data-stu-id="8219c-145">If the version is not set to **1.6.0** then update the value.</span></span>

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

2. <span data-ttu-id="8219c-146">App Service Linux の Java SE にデプロイする</span><span class="sxs-lookup"><span data-stu-id="8219c-146">Deploy to Java SE in App Service Linux</span></span>

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

3. <span data-ttu-id="8219c-147">App Service Linux の Java SE で実行されている Web アプリを参照します。</span><span class="sxs-lookup"><span data-stu-id="8219c-147">Browse to your web app running on Java SE in App Service Linux:</span></span>

    ```bash
    https://<WEBAPP_NAME>.azurewebsites.net
    ```

![App Service on Linux で実行されている Spring アプリ][SCDB02]

## <a name="troubleshoot-spring-todo-app-on-azure-by-viewing-logs"></a><span data-ttu-id="8219c-149">ログを表示して Azure の Spring Todo アプリをトラブルシューティングする</span><span class="sxs-lookup"><span data-stu-id="8219c-149">Troubleshoot Spring Todo App on Azure by Viewing Logs</span></span>

1. <span data-ttu-id="8219c-150">Linux の Azure App Service にデプロイされた Java Web アプリのログを構成します。</span><span class="sxs-lookup"><span data-stu-id="8219c-150">Configure logs for the deployed Java Web app in Azure App Service in Linux:</span></span>

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
     --web-server-logging filesystem
    ```
2. <span data-ttu-id="8219c-151">ローカル マシンから Java Web アプリのリモート ログ ストリームを開きます。</span><span class="sxs-lookup"><span data-stu-id="8219c-151">Open Java Web app remote log stream from a local machine:</span></span>

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

3. <span data-ttu-id="8219c-152">終了したら、結果を [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete) のコードと比較できます。</span><span class="sxs-lookup"><span data-stu-id="8219c-152">When you are finished, you can check your results against the code in [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete).</span></span>


## <a name="scale-out-the-spring-todo-app"></a><span data-ttu-id="8219c-153">Spring Todo アプリをスケールアウトする</span><span class="sxs-lookup"><span data-stu-id="8219c-153">Scale out the Spring Todo App</span></span>

1. <span data-ttu-id="8219c-154">Azure CLI を使用して Java Web アプリをスケールアウトします。</span><span class="sxs-lookup"><span data-stu-id="8219c-154">Scale out Java Web app using Azure CLI:</span></span>

    ```bash
    az appservice plan update --number-of-workers 2 \
      --name ${WEBAPP_PLAN_NAME} \
      --resource-group ${RESOURCEGROUP_NAME}
    ```

## <a name="next-steps"></a><span data-ttu-id="8219c-155">次の手順</span><span class="sxs-lookup"><span data-stu-id="8219c-155">Next steps</span></span>

- [<span data-ttu-id="8219c-156">App Service Linux の Java 開発ガイド</span><span class="sxs-lookup"><span data-stu-id="8219c-156">Java in App Service Linux dev guide</span></span>](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-java)
- <span data-ttu-id="8219c-157">[Java 開発者向けの Azure](https://docs.microsoft.com/en-us/java/azure/) Spring および Azure の詳細については、Azure の Spring ドキュメント センターにお進みください。</span><span class="sxs-lookup"><span data-stu-id="8219c-157">[Azure for Java Developers](https://docs.microsoft.com/en-us/java/azure/) To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8219c-158">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="8219c-158">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="8219c-159">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="8219c-159">Additional Resources</span></span>

<span data-ttu-id="8219c-160">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8219c-160">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="8219c-161">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="8219c-161">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="8219c-162">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="8219c-162">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="8219c-163">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8219c-163">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="8219c-164">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="8219c-164">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="8219c-165">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="8219c-165">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="8219c-166">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="8219c-166">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="8219c-167">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、 **[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="8219c-167">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: https://azure.microsoft.com/services/devops/java/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SCDB01]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB01.png
[SCDB02]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB02.png
