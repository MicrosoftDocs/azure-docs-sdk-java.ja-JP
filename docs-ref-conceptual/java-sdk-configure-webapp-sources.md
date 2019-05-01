---
title: Java を使って Web アプリのデプロイを構成する | Microsoft Docs
description: Azure SDK for Java を使って Git または FTP による Azure App Service デプロイを構成するサンプル Java コード。
author: rloutlaw
manager: douge
ms.assetid: 833e9c78-1e50-4c23-a611-f73a2f0c2983
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: c416cb871ac8264cd6da2045d1ffdd50607fb092
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592627"
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a>Java アプリケーションから Azure App Service のデプロイ ソースを構成する

[このサンプル](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)では、1 つの [Azure App Service](https://docs.microsoft.com/azure/app-service/) プランの 4 つのアプリケーションに、それぞれ異なるデプロイ ソースを使ってコードをデプロイします。

## <a name="run-the-sample"></a>サンプルを実行する

[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。 次に、以下を実行します。

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

[完全なサンプル コードについては GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java) を参照してください。

## <a name="authenticate-with-azure"></a>Azure での認証

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a>Apache Tomcat を実行する App Service アプリの作成

```java
// create a new Standard app service plan and create a single Java 8/Tomcat 8 app in it
WebApp app1 = azure.webApps().define(app1Name)
             .withNewResourceGroup(rgName)
             .withNewAppServicePlan(planName)
             .withRegion(Region.US_WEST)
             .withPricingTier(AppServicePricingTier.STANDARD_S1)
             .withJavaVersion(JavaVersion.JAVA_8_NEWEST)
             .withWebContainer(WebContainer.TOMCAT_8_0_NEWEST)
             .create();
```

`withJavaVersion()` と `withWebContainer()` で、Tomcat 8 を使って HTTP 要求を処理するように App Service を構成しています。

## <a name="deploy-a-java-application-using-ftp"></a>FTP による Java アプリケーションのデプロイ
```java
// pass the PublishingProfile that contains FTP information to a helper method 
uploadFileToFtp(app1.getPublishingProfile(), "helloworld.war", 
      ManageWebAppSourceControl.class.getResourceAsStream("/helloworld.war"));

// Use the FTP classes in the Apache Commons library to connect to Azure using 
// the information from the PublishingProfile
private static void uploadFileToFtp(PublishingProfile profile, String fileName, InputStream file) throws Exception {
        FTPClient ftpClient = new FTPClient();
        String[] ftpUrlSegments = profile.ftpUrl().split("/", 2);
        String server = ftpUrlSegments[0];
        // Tomcat will deploy WAR files uploaded to this directory.
        String path = "./site/wwwroot/webapps"; 

        // FTP the build WAR to Azure
        ftpClient.connect(server);
        ftpClient.login(profile.ftpUsername(), profile.ftpPassword());
        ftpClient.setFileType(FTP.BINARY_FILE_TYPE);
        ftpClient.changeWorkingDirectory(path);
        ftpClient.storeFile(fileName, file);
        ftpClient.disconnect();
}
```

このコードは、`/site/wwwroot/webapps` ディレクトリに WAR ファイルをアップロードします。 App Service には、このディレクトリに格納された WAR ファイルが既定で Tomcat によってデプロイされます。

## <a name="deploy-a-java-application-from-a-local-git-repo"></a>ローカル Git リポジトリからの Java アプリケーションのデプロイ

```java
// get the publishing profile from the App Service webapp
PublishingProfile profile = app2.getPublishingProfile();

// create a new Git repo in the sample directory under src/main/resources 
Git git = Git
    .init()
    .setDirectory(new File(ManageWebAppSourceControl.class.getResource("/azure-samples-appservice-helloworld/").getPath()))
    .call();
git.add().addFilepattern(".").call();
// add the files in the sample app to an initial commit
git.commit().setMessage("Initial commit").call(); 

// push the commit using the Azure Git remote URL and credentials in the publishing profile
PushCommand command = git.push();
command.setRemote(profile.gitUrl()); 
command.setCredentialsProvider(new UsernamePasswordCredentialsProvider(profile.gitUsername(), profile.gitPassword()));
command.setRefSpecs(new RefSpec("master:master")); 
command.setForce(true);
command.call();
```      

このコードは、[JGit](https://eclipse.org/jgit/) ライブラリを使って、新しい Git リポジトリを `src/main/resources/azure-samples-appservice-helloworld` フォルダーに作成します。 その後、このフォルダーにあるすべてのファイルを初期コミットに追加し、Web アプリの `PublishingProfile` から得た Git のデプロイ情報を使って、そのコミットを Azure にプッシュします。 

>[!NOTE]
> リポジトリ内のファイルのレイアウトは、Azure App Service の `/site/wwwroot/` ディレクトリに対して実際にファイルをデプロイするときとまったく同じレイアウトにする必要があります。

## <a name="deploy-an-application-from-a-public-git-repo"></a>パブリック Git リポジトリからのアプリケーションのデプロイ

```java
// deploy a .NET sample app from a public GitHub repo into a new webapp
WebApp app3 = azure.webApps().define(app3Name)
                .withNewResourceGroup(rgName)
                .withExistingAppServicePlan(plan)
                .defineSourceControl()
                .withPublicGitRepository(
                   "https://github.com/Azure-Samples/app-service-web-dotnet-get-started")
                .withBranch("master")
                .attach()
                .create();
 ```

 .NET プロジェクトのビルドとデプロイは、App Service ランタイムが、リポジトリの `master` ブランチにある最新のコードを使って自動的に行います。

## <a name="continuous-deployment-from-a-github-repo"></a>GitHub リポジトリからの継続的デプロイ

```java
// deploy the application whenever you push a new commit or merge a pull request into your master branch
WebApp app4 = azure.webApps()
                    .define(app4Name)
                    .withExistingResourceGroup(rgName)
                    .withExistingAppServicePlan(plan)
                    // Uncomment the following lines to turn on continuous deployment scenario
                    //.defineSourceControl()
                    //    .withContinuouslyIntegratedGitHubRepository("username", "reponame")
                    //    .withBranch("master")
                    //    .withGitHubAccessToken("YOUR GITHUB PERSONAL TOKEN")
                    //    .attach()
                    .create();
```  

`username` と `reponame` には、GitHub で使われている値を指定します。 リポジトリの読み取りアクセス許可で [GitHub 個人用アクセス トークンを作成](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)し、そのトークンを `withGitHubAccessToken` に渡してください。 


## <a name="sample-explanation"></a>サンプルの説明

このサンプルでは、新しく作成した [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service プランで稼働する Java 8 と Tomcat 8 を使用して、1 つ目のアプリケーションを作成します。 その後、`PublishingProfile` オブジェクトに保持されている情報を使って WAR ファイルを FTP で送信すると、Tomcat によってその WAR ファイルがデプロイされます。

2 つ目のアプリケーションは、1 つ目のアプリケーションと同じプランを使用するもので、こちらも Java 8/Tomcat 8 アプリケーションとして構成されます。 アンパックされた Java Web アプリケーションが格納されているフォルダーには、JGit ライブラリにより、App Service に対応するディレクトリ構造で新しい Git リポジトリが作成されます。 新たにコミットすると、このフォルダー内のファイルが、新しい Git リポジトリに追加され、そのコミットは、Web アプリの `PublishingProfile` に指定されたリモート URL とユーザー名/パスワードで Azure にプッシュされます。

3 つ目のアプリケーションは、Java と Tomcat の構成ではなく、 パブリック GitHub リポジトリにある .NET サンプルを直接ソースからデプロイするものです。

4 つ目のアプリケーションでは、GitHub リポジトリのマスター ブランチに対して変更をプッシュするか pull request をマージすると、その都度、マスター ブランチにあるコードがデプロイされます。

| サンプルで使われているクラス | メモ
|-------|-------|
| [WebApp](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | `azure.webApps().define()....create()` という fluent チェーンから作成されます。 App Service Web アプリのほか、そのアプリに必要なすべてのリソースが作成されます。 そのメソッドは、オブジェクトに構成の詳細を照会するものが大半ですが、動詞を含んだメソッド (`restart()` など) では、Web アプリの状態が変更されます。
| [WebContainer](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | static public フィールドがあるクラスで、Java Web コンテナーを実行する WebApp を定義する際に `withWebContainer()` のパラメーターとして使用されます。 Jetty と Tomcat のどちらもバージョンを選択できます。
| [PublishingProfile](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | WebApp オブジェクトの `getPublishingProfile()` メソッドを使用して取得できます。 FTP と Git のデプロイ情報を保持します。たとえば、デプロイのユーザー名とパスワード (Azure アカウントやサービス プリンシパルの資格情報とは区別されます) が含まれます。
| [AppServicePlan](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | `azure.appServices().appServicePlans().getByResourceGroup()` から返されます。 プランで実行されている Web アプリの数、レベル、キャパシティをチェックするためのメソッドが利用できます。
| [AppServicePricingTier](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | App Service のレベルを表す static public フィールドがあるクラス。 プランのレベルをアプリの作成時に `withPricingTier()` を使ってインラインで定義するとき、または `azure.appServices().appServicePlans().define()` によってプランを定義する際にそのレベルを直接定義するときに使用します。
| [JavaVersion](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | App Service によってサポートされる Java のバージョンを表す static public フィールドがあるクラス。 新しい Web アプリを作成するときに `define()...create()` チェーンの中で `withJavaVersion()` と組み合わせて使用します。

## <a name="next-steps"></a>次の手順

[!INCLUDE [next-steps](includes/java-next-steps.md)]