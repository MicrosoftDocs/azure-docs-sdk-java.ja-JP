---
title: Maven を使って Java Web アプリを Azure に 5 分でデプロイする | Microsoft Docs
description: Maven で Java アプリを作成、ビルドして Azure にデプロイする
services: app-service\web
documentationcenter: ''
author: rloutlaw
manager: douge
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: routlaw
ms.openlocfilehash: 70b508118c50b75693e2d746dc1e2919c827cb29
ms.sourcegitcommit: 0f38ef9ad64cffdb7b2e9e966224dfd0af251b0f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/23/2018
ms.locfileid: "42703545"
---
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a><span data-ttu-id="27787-103">Maven で Java アプリを作成して Azure にデプロイする</span><span class="sxs-lookup"><span data-stu-id="27787-103">Create and deploy a Java app to Azure with Maven</span></span>

<span data-ttu-id="27787-104">このクイック スタートを読むと、[Apache Maven](http://maven.apache.org) と Azure CLI を使って、単純な Java Web アプリを作成し、ほんの数分で [Azure App Service](/azure/app-service/app-service-value-prop-what-is) にデプロイできるようになります。</span><span class="sxs-lookup"><span data-stu-id="27787-104">This quickstart helps you create and deploy a simple Java web app to [Azure App Service](/azure/app-service/app-service-value-prop-what-is) in just a few minutes using [Apache Maven](http://maven.apache.org) and the Azure CLI.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="27787-105">開始する前に</span><span class="sxs-lookup"><span data-stu-id="27787-105">Before you begin</span></span>

<span data-ttu-id="27787-106">開始する前に、次の設定を行います。</span><span class="sxs-lookup"><span data-stu-id="27787-106">Before you start, set up the following:</span></span>

- [<span data-ttu-id="27787-107">Git</span><span class="sxs-lookup"><span data-stu-id="27787-107">Git</span></span>](https://git-scm.com/)
- [<span data-ttu-id="27787-108">Java 8</span><span class="sxs-lookup"><span data-stu-id="27787-108">Java 8</span></span>](https://www.azul.com/downloads/zulu/)
- [<span data-ttu-id="27787-109">Maven 3</span><span class="sxs-lookup"><span data-stu-id="27787-109">Maven 3</span></span>](http://maven.apache.org/download.cgi)
- [<span data-ttu-id="27787-110">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="27787-110">Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-az-cli2)

<span data-ttu-id="27787-111">Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。</span><span class="sxs-lookup"><span data-stu-id="27787-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="get-the-sample-code"></a><span data-ttu-id="27787-112">サンプル コードの取得</span><span class="sxs-lookup"><span data-stu-id="27787-112">Get the sample code</span></span>

<span data-ttu-id="27787-113">サンプル アプリ リポジトリをローカル コンピューターに複製します。</span><span class="sxs-lookup"><span data-stu-id="27787-113">Clone the sample app repository to your local machine.</span></span> <span data-ttu-id="27787-114">このサンプルは単純な JSP アプリケーションであり、それをローカルでテストして Azure App Service にデプロイする作業を支援する追加の Maven 構成が、プロジェクトの `pom.xml` に含まれています。</span><span class="sxs-lookup"><span data-stu-id="27787-114">The sample is a simple JSP application with additional Maven configuration in the project `pom.xml` to test the app locally and help deploy the sample to Azure App Service.</span></span>

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a><span data-ttu-id="27787-115">アプリをローカルで実行する</span><span class="sxs-lookup"><span data-stu-id="27787-115">Run the app locally</span></span>

<span data-ttu-id="27787-116">コマンド プロンプトを開いたら、Maven を使ってアプリをビルドし、ローカルの [Tomcat](https://tomcat.apache.org) Web コンテナーで実行します。</span><span class="sxs-lookup"><span data-stu-id="27787-116">Open a command prompt and use Maven to build the app and run it in a local [Tomcat](https://tomcat.apache.org) web container.</span></span> 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

<span data-ttu-id="27787-117">Web ブラウザーを開き、 http://localhost:8080 に移動してアプリをプレビューします。</span><span class="sxs-lookup"><span data-stu-id="27787-117">Open a web browser and navigate to http://localhost:8080 to preview the app:</span></span>

  ![サンプル Java アプリからの Hello World 出力](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a><span data-ttu-id="27787-119">Azure にログインする</span><span class="sxs-lookup"><span data-stu-id="27787-119">Log in to Azure</span></span>

<span data-ttu-id="27787-120">`az login` で Azure CLI にログインします。</span><span class="sxs-lookup"><span data-stu-id="27787-120">Log in to the Azure CLI with `az login`.</span></span> <span data-ttu-id="27787-121">このクイック スタートでは、アプリをホストするために必要な Azure リソースを CLI で照会し、作成します。</span><span class="sxs-lookup"><span data-stu-id="27787-121">This quickstart uses the CLI to query and create the Azure resources needed to host the app.</span></span>
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="27787-122">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="27787-122">Create a resource group</span></span>   

<span data-ttu-id="27787-123">[az group create](/cli/azure/group#create) を使用して、リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="27787-123">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="27787-124">Azure リソース グループとは、Azure リソースのデプロイと管理に使用する論理コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="27787-124">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

```azurecli
az group create --location "East US" --name myResourceGroup
```

<span data-ttu-id="27787-125">`---location` に使用できる値については、[az appservice list-locations](/cli/azure/appservice#list-locations) を使って確認してください。</span><span class="sxs-lookup"><span data-stu-id="27787-125">To see other possible values you can use for `---location`, use [az appservice list-locations](/cli/azure/appservice#list-locations)</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="27787-126">App Service プランを作成する</span><span class="sxs-lookup"><span data-stu-id="27787-126">Create an App Service plan</span></span>

<span data-ttu-id="27787-127">[az appservice plan create](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) を使って、[FREE](/cli/azure/appservice/plan#create) **App Service プラン**を作成します。</span><span class="sxs-lookup"><span data-stu-id="27787-127">Create a **FREE** [App Service plan](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) using [az appservice plan create](/cli/azure/appservice/plan#create).</span></span> <span data-ttu-id="27787-128">この App Service プラン内で実行されるすべての Web アプリ間で共有されるリソースが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="27787-128">App Service plans allocate resources shared across all web apps running in the plan.</span></span>


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="27787-129">App Service プランの準備が整うと、Azure CLI によって、次の例のような情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="27787-129">When the App Service Plan is ready, the Azure CLI shows information similar to the following example:</span></span>

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "location": "East US",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```


## <a name="create-a-web-app"></a><span data-ttu-id="27787-130">Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="27787-130">Create a web app</span></span>

<span data-ttu-id="27787-131">Azure CLI の [az webapp create](/cli/azure/appservice/web#create) コマンドを使用して、このプランのリソースを使用する Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="27787-131">Create a web app that uses the plan's resources using the Azure CLI [az webapp create](/cli/azure/appservice/web#create) command.</span></span> <span data-ttu-id="27787-132">Web アプリの定義には、コードのデプロイ先や、稼働後のアプリにアクセスするための URL が記述されます。</span><span class="sxs-lookup"><span data-stu-id="27787-132">The web app definition provides a space to deploy the code to and a URL to access the app once it's running.</span></span>

<span data-ttu-id="27787-133">次のコマンドで、<appname> プレースホルダーを独自の一意のアプリ名に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="27787-133">In the command below substitute your own unique app name where you see the <appname> placeholder.</span></span> <span data-ttu-id="27787-134"><appname> は、Web アプリの既定のホスト名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="27787-134">The <appname> is used in the default hostname for the web app.</span></span> <span data-ttu-id="27787-135"><appname> が一意でない場合は、"指定された名前の Web サイトは既に存在します" というわかりやすいエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="27787-135">If <appname> is not unique, you get the friendly error message "Website with given name already exists."</span></span>

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

<span data-ttu-id="27787-136">Web アプリが作成されると、Azure CLI によって次の例のような情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="27787-136">When the Web App has been created, the Azure CLI shows information similar to the following example.1</span></span>

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<appname>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<appname>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "East US",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

## <a name="configure-java-and-tomcat"></a><span data-ttu-id="27787-137">Java と Tomcat の構成</span><span class="sxs-lookup"><span data-stu-id="27787-137">Configure Java and Tomcat</span></span>

<span data-ttu-id="27787-138">Web アプリで Java と Tomcat を使うための構成は、Azure CLI の [az webapp config](/cli/azure/appservice/web#config) コマンドで行います。</span><span class="sxs-lookup"><span data-stu-id="27787-138">Configure the web app to use Java and Tomcat using the Azure CLI's [az webapp config](/cli/azure/appservice/web#config) command.</span></span> <span data-ttu-id="27787-139">次の例では、Java 8 と [Apache Tomcat 8.5](http://tomcat.apache.org/) を使ってアプリを実行するように App Service を構成しています。</span><span class="sxs-lookup"><span data-stu-id="27787-139">The example below configures App Service to run Java 8 and [Apache Tomcat 8.5](http://tomcat.apache.org/) to run the app.</span></span>

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a><span data-ttu-id="27787-140">Maven の構成</span><span class="sxs-lookup"><span data-stu-id="27787-140">Configure Maven</span></span> 

<span data-ttu-id="27787-141">このサンプルでは、Maven の `pom.xml` に、Azure に対して FTP でサンプルを転送するための構成が含まれていますが、独自の Web アプリにデプロイするためには、これをカスタマイズする必要があります。</span><span class="sxs-lookup"><span data-stu-id="27787-141">The sample's Maven `pom.xml` includes configuration to FTP the sample into Azure, but you'll need to customize it to deploy to your own web app.</span></span> <span data-ttu-id="27787-142">[az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles) で App Service の資格情報を取得してください。</span><span class="sxs-lookup"><span data-stu-id="27787-142">Retreive your App Service credentials with [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles):</span></span>

```azurecli
az webapp deployment list-publishing-profiles  \
--name appname \ 
--resource-group myResourceGroup  \
--query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" 
```

```json
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "appname\\$appname"
  }
]
```

<span data-ttu-id="27787-143">`az-settings.xml` ファイル内のプレースホルダーを、出力結果のパスワード、ユーザー名、FTP ホスト名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="27787-143">Replace the placeholders in the `az-settings.xml` file with password, username, and FTP hostname from the output.</span></span> <span data-ttu-id="27787-144">ユーザー名に使用する `\` 文字は 1 つだけとしてください。CLI 出力ではエスケープされた値が使用されています。</span><span class="sxs-lookup"><span data-stu-id="27787-144">Make sure to only use one `\` character in the username instead of the escaped value from the CLI output.</span></span>
   
```XML
...
    <server>
      <id>azure-hello-world</id>
      <username>appname\$appname</username>
      <password>aBcDeFgHiJkLmNoPqRsTuVwXyZ</password>
    </server>
...
   <configuration>
     <fromFile>${basedir}/target/AzureAppDemo-0.0.1-SNAPSHOT.war</fromFile>
     <url>ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot</url>
     <toFile>webapps/ROOT.war</toFile>
     <serverId>azure-hello-world</serverId>
   </configuration>
...
```

## <a name="deploy-the-sample-application"></a><span data-ttu-id="27787-145">サンプル アプリケーションをデプロイする</span><span class="sxs-lookup"><span data-stu-id="27787-145">Deploy the sample application</span></span>

<span data-ttu-id="27787-146">サンプルを Azure にデプロイします。このとき、Maven の `-s` パラメーターで、`az-settings.xml` ファイル内の構成を使うように指定します。</span><span class="sxs-lookup"><span data-stu-id="27787-146">Deploy with sample to Azure, using Maven's `-s` parameter to use the configuration in the `az-settings.xml` file.</span></span>

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a><span data-ttu-id="27787-147">Web アプリを確認する</span><span class="sxs-lookup"><span data-stu-id="27787-147">Browse to web app</span></span>

<span data-ttu-id="27787-148">Azure で動作するサンプルを表示します。</span><span class="sxs-lookup"><span data-stu-id="27787-148">View the sample running in Azure:</span></span>

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a><span data-ttu-id="27787-149">アプリの更新</span><span class="sxs-lookup"><span data-stu-id="27787-149">Update the app</span></span>

<span data-ttu-id="27787-150">テキスト エディターで `src/main/webapp/index.jsp` を開き、既存の JSP を次の内容に差し替えます。</span><span class="sxs-lookup"><span data-stu-id="27787-150">Using a text editor, open `src/main/webapp/index.jsp`, and replace the existing JSP with the one below.</span></span>

```html
<%--
  Copyright (c) Microsoft Corporation. All rights reserved.
  Licensed under the MIT License. See License.txt in the project root for
  license information.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java"  import="java.util.Date" %>
<html>
<head>
  <title>Azure Samples Hello World</title>
</head>
<body>
  <H1>Hello Azure!</H1>
   Current time is: <%= new Date() %>
</body>
</html>
```

<span data-ttu-id="27787-151">Maven で更新をデプロイします。</span><span class="sxs-lookup"><span data-stu-id="27787-151">Deploy the update with Maven:</span></span>

```
mvn clean package
mvn install -s az-settings.xml
```

<span data-ttu-id="27787-152">アプリの再デプロイ後、ブラウザーを最新の情報に更新して変更内容を確認します。</span><span class="sxs-lookup"><span data-stu-id="27787-152">Refresh your browser after the app redeploys to view your changes.</span></span>

## <a name="manage-your-new-azure-app"></a><span data-ttu-id="27787-153">新しい Azure アプリの管理</span><span class="sxs-lookup"><span data-stu-id="27787-153">Manage your new Azure app</span></span>

<span data-ttu-id="27787-154">Azure Portal に移動し、作成したばかりの Web アプリを表示します。</span><span class="sxs-lookup"><span data-stu-id="27787-154">Go to the Azure portal to take a look at the web app you just created.</span></span>

<span data-ttu-id="27787-155">これを行うには、[https://portal.azure.com](https://portal.azure.com) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="27787-155">To do this, sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="27787-156">左側のメニューで **[App Service]** をクリックした後、Azure Web アプリの名前をクリックします。</span><span class="sxs-lookup"><span data-stu-id="27787-156">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

 ![ポータルの Web アプリの一覧から目的のアプリを選択します。](media/azure-app-service-portal.png)

<span data-ttu-id="27787-158">アプリに対して `curl` を実行して少しのトラフィックを送信し、監視機能をテストします。</span><span class="sxs-lookup"><span data-stu-id="27787-158">Test the monitoring by running `curl` against the app to send some traffic.</span></span>

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

<span data-ttu-id="27787-159">数分後、監視機能に要求アクティビティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="27787-159">You'll see the request activity in the monitoring after a couple of minutes.</span></span>

 ![Azure Portal の監視機能](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a><span data-ttu-id="27787-161">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="27787-161">Clean up resources</span></span>

<span data-ttu-id="27787-162">このガイドで作成したリソースをすべて削除するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="27787-162">To remove all the resources created in this guide, run the following command:</span></span>

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a><span data-ttu-id="27787-163">次の手順</span><span class="sxs-lookup"><span data-stu-id="27787-163">Next steps</span></span>

<span data-ttu-id="27787-164">こちらで [Azure Java のサンプル](https://azure.microsoft.com/resources/samples/?term=java)をすべてご覧いただけます。</span><span class="sxs-lookup"><span data-stu-id="27787-164">Browse the full list of [Azure Java samples](https://azure.microsoft.com/resources/samples/?term=java).</span></span>
