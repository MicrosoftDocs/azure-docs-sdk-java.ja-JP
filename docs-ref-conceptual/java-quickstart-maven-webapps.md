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
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a>Maven で Java アプリを作成して Azure にデプロイする

このクイック スタートを読むと、[Apache Maven](http://maven.apache.org) と Azure CLI を使って、単純な Java Web アプリを作成し、ほんの数分で [Azure App Service](/azure/app-service/app-service-value-prop-what-is) にデプロイできるようになります。

## <a name="before-you-begin"></a>開始する前に

開始する前に、次の設定を行います。

- [Git](https://git-scm.com/)
- [Java 8](https://www.azul.com/downloads/zulu/)
- [Maven 3](http://maven.apache.org/download.cgi)
- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

## <a name="get-the-sample-code"></a>サンプル コードの取得

サンプル アプリ リポジトリをローカル コンピューターに複製します。 このサンプルは単純な JSP アプリケーションであり、それをローカルでテストして Azure App Service にデプロイする作業を支援する追加の Maven 構成が、プロジェクトの `pom.xml` に含まれています。

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a>アプリをローカルで実行する

コマンド プロンプトを開いたら、Maven を使ってアプリをビルドし、ローカルの [Tomcat](https://tomcat.apache.org) Web コンテナーで実行します。 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

Web ブラウザーを開き、 http://localhost:8080 に移動してアプリをプレビューします。

  ![サンプル Java アプリからの Hello World 出力](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a>Azure にログインする

`az login` で Azure CLI にログインします。 このクイック スタートでは、アプリをホストするために必要な Azure リソースを CLI で照会し、作成します。
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a>リソース グループの作成   

[az group create](/cli/azure/group#create) を使用して、リソース グループを作成します。 Azure リソース グループとは、Azure リソースのデプロイと管理に使用する論理コンテナーです。

```azurecli
az group create --location "East US" --name myResourceGroup
```

`---location` に使用できる値については、[az appservice list-locations](/cli/azure/appservice#list-locations) を使って確認してください。

## <a name="create-an-app-service-plan"></a>App Service プランを作成する

[az appservice plan create](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) を使って、[FREE](/cli/azure/appservice/plan#create) **App Service プラン**を作成します。 この App Service プラン内で実行されるすべての Web アプリ間で共有されるリソースが割り当てられます。


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

App Service プランの準備が整うと、Azure CLI によって、次の例のような情報が表示されます。

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


## <a name="create-a-web-app"></a>Web アプリを作成する

Azure CLI の [az webapp create](/cli/azure/appservice/web#create) コマンドを使用して、このプランのリソースを使用する Web アプリを作成します。 Web アプリの定義には、コードのデプロイ先や、稼働後のアプリにアクセスするための URL が記述されます。

次のコマンドで、<appname> プレースホルダーを独自の一意のアプリ名に置き換えてください。 <appname> は、Web アプリの既定のホスト名に使用されます。 <appname> が一意でない場合は、"指定された名前の Web サイトは既に存在します" というわかりやすいエラー メッセージが表示されます。

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

Web アプリが作成されると、Azure CLI によって次の例のような情報が表示されます。

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

## <a name="configure-java-and-tomcat"></a>Java と Tomcat の構成

Web アプリで Java と Tomcat を使うための構成は、Azure CLI の [az webapp config](/cli/azure/appservice/web#config) コマンドで行います。 次の例では、Java 8 と [Apache Tomcat 8.5](http://tomcat.apache.org/) を使ってアプリを実行するように App Service を構成しています。

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a>Maven の構成 

このサンプルでは、Maven の `pom.xml` に、Azure に対して FTP でサンプルを転送するための構成が含まれていますが、独自の Web アプリにデプロイするためには、これをカスタマイズする必要があります。 [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles) で App Service の資格情報を取得してください。

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

`az-settings.xml` ファイル内のプレースホルダーを、出力結果のパスワード、ユーザー名、FTP ホスト名に置き換えます。 ユーザー名に使用する `\` 文字は 1 つだけとしてください。CLI 出力ではエスケープされた値が使用されています。
   
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

## <a name="deploy-the-sample-application"></a>サンプル アプリケーションをデプロイする

サンプルを Azure にデプロイします。このとき、Maven の `-s` パラメーターで、`az-settings.xml` ファイル内の構成を使うように指定します。

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a>Web アプリを確認する

Azure で動作するサンプルを表示します。

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a>アプリの更新

テキスト エディターで `src/main/webapp/index.jsp` を開き、既存の JSP を次の内容に差し替えます。

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

Maven で更新をデプロイします。

```
mvn clean package
mvn install -s az-settings.xml
```

アプリの再デプロイ後、ブラウザーを最新の情報に更新して変更内容を確認します。

## <a name="manage-your-new-azure-app"></a>新しい Azure アプリの管理

Azure Portal に移動し、作成したばかりの Web アプリを表示します。

これを行うには、[https://portal.azure.com](https://portal.azure.com) にサインインします。

左側のメニューで **[App Service]** をクリックした後、Azure Web アプリの名前をクリックします。

 ![ポータルの Web アプリの一覧から目的のアプリを選択します。](media/azure-app-service-portal.png)

アプリに対して `curl` を実行して少しのトラフィックを送信し、監視機能をテストします。

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

数分後、監視機能に要求アクティビティが表示されます。

 ![Azure Portal の監視機能](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a>リソースのクリーンアップ

このガイドで作成したリソースをすべて削除するには、次のコマンドを実行します。

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a>次の手順

こちらで [Azure Java のサンプル](https://azure.microsoft.com/resources/samples/?term=java)をすべてご覧いただけます。
