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
ms.openlocfilehash: 910d1e43c9942d6402aeccb8757ba819b7453dab
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893143"
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a><span data-ttu-id="0940b-103">Java アプリケーションから Azure App Service のデプロイ ソースを構成する</span><span class="sxs-lookup"><span data-stu-id="0940b-103">Configure Azure App Service deployment sources from your Java applications</span></span>

<span data-ttu-id="0940b-104">[このサンプル](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)では、1 つの [Azure App Service](https://docs.microsoft.com/azure/app-service/) プランの 4 つのアプリケーションに、それぞれ異なるデプロイ ソースを使ってコードをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="0940b-104">[This sample](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) deploys code to four applications in a single [Azure App Service](https://docs.microsoft.com/azure/app-service/) plan, each using a different deployment source.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="0940b-105">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="0940b-105">Run the sample</span></span>

<span data-ttu-id="0940b-106">[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="0940b-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="0940b-107">次に、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="0940b-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

<span data-ttu-id="0940b-108">[完全なサンプル コードについては GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0940b-108">View the [complete sample code on GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="0940b-109">Azure での認証</span><span class="sxs-lookup"><span data-stu-id="0940b-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a><span data-ttu-id="0940b-110">Apache Tomcat を実行する App Service アプリの作成</span><span class="sxs-lookup"><span data-stu-id="0940b-110">Create a App Service app running Apache Tomcat</span></span>

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

<span data-ttu-id="0940b-111">`withJavaVersion()` と `withWebContainer()` で、Tomcat 8 を使って HTTP 要求を処理するように App Service を構成しています。</span><span class="sxs-lookup"><span data-stu-id="0940b-111">`withJavaVersion()` and `withWebContainer()` configure App Service to serve HTTP requests using Tomcat 8.</span></span>

## <a name="deploy-a-java-application-using-ftp"></a><span data-ttu-id="0940b-112">FTP による Java アプリケーションのデプロイ</span><span class="sxs-lookup"><span data-stu-id="0940b-112">Deploy a Java application using FTP</span></span>
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

<span data-ttu-id="0940b-113">このコードは、`/site/wwwroot/webapps` ディレクトリに WAR ファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="0940b-113">This code uploads a WAR file to the `/site/wwwroot/webapps` directory.</span></span> <span data-ttu-id="0940b-114">App Service には、このディレクトリに格納された WAR ファイルが既定で Tomcat によってデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="0940b-114">Tomcat deploys WAR files placed in this directory by default in App Service.</span></span>

## <a name="deploy-a-java-application-from-a-local-git-repo"></a><span data-ttu-id="0940b-115">ローカル Git リポジトリからの Java アプリケーションのデプロイ</span><span class="sxs-lookup"><span data-stu-id="0940b-115">Deploy a Java application from a local Git repo</span></span>

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

<span data-ttu-id="0940b-116">このコードは、[JGit](https://eclipse.org/jgit/) ライブラリを使って、新しい Git リポジトリを `src/main/resources/azure-samples-appservice-helloworld` フォルダーに作成します。</span><span class="sxs-lookup"><span data-stu-id="0940b-116">This code uses the [JGit](https://eclipse.org/jgit/) libraries to create a new Git repo in the `src/main/resources/azure-samples-appservice-helloworld` folder.</span></span> <span data-ttu-id="0940b-117">その後、このフォルダーにあるすべてのファイルを初期コミットに追加し、Web アプリの `PublishingProfile` から得た Git のデプロイ情報を使って、そのコミットを Azure にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="0940b-117">The sample then adds all files in the folder to an initial commit and pushes the commit to Azure using Git deployment information from the webapp's `PublishingProfile`.</span></span> 

>[!NOTE]
> <span data-ttu-id="0940b-118">リポジトリ内のファイルのレイアウトは、Azure App Service の `/site/wwwroot/` ディレクトリに対して実際にファイルをデプロイするときとまったく同じレイアウトにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0940b-118">The layout of the files in the repo must match exactly how you want the files deployed under the `/site/wwwroot/` directory in Azure App Service.</span></span>

## <a name="deploy-an-application-from-a-public-git-repo"></a><span data-ttu-id="0940b-119">パブリック Git リポジトリからのアプリケーションのデプロイ</span><span class="sxs-lookup"><span data-stu-id="0940b-119">Deploy an application from a public Git repo</span></span>

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

 <span data-ttu-id="0940b-120">.NET プロジェクトのビルドとデプロイは、App Service ランタイムが、リポジトリの `master` ブランチにある最新のコードを使って自動的に行います。</span><span class="sxs-lookup"><span data-stu-id="0940b-120">The App Service runtime automatically builds and deploys the .NET project using the latest code on the `master` branch of the repo.</span></span>

## <a name="continuous-deployment-from-a-github-repo"></a><span data-ttu-id="0940b-121">GitHub リポジトリからの継続的デプロイ</span><span class="sxs-lookup"><span data-stu-id="0940b-121">Continuous deployment from a GitHub repo</span></span>

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

<span data-ttu-id="0940b-122">`username` と `reponame` には、GitHub で使われている値を指定します。</span><span class="sxs-lookup"><span data-stu-id="0940b-122">The `username` and `reponame` values are the ones used in GitHub.</span></span> <span data-ttu-id="0940b-123">リポジトリの読み取りアクセス許可で [GitHub 個人用アクセス トークンを作成](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)し、そのトークンを `withGitHubAccessToken` に渡してください。</span><span class="sxs-lookup"><span data-stu-id="0940b-123">[Create a GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) with repo read permissions and pass it to `withGitHubAccessToken`.</span></span> 


## <a name="sample-explanation"></a><span data-ttu-id="0940b-124">サンプルの説明</span><span class="sxs-lookup"><span data-stu-id="0940b-124">Sample explanation</span></span>

<span data-ttu-id="0940b-125">このサンプルでは、新しく作成した [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service プランで稼働する Java 8 と Tomcat 8 を使用して、1 つ目のアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="0940b-125">The sample creates the first application using Java 8 and Tomcat 8 running in a newly created [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service plan.</span></span> <span data-ttu-id="0940b-126">その後、`PublishingProfile` オブジェクトに保持されている情報を使って WAR ファイルを FTP で送信すると、Tomcat によってその WAR ファイルがデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="0940b-126">The code then FTPs a WAR file using the information in the `PublishingProfile` object and Tomcat deploys it.</span></span>

<span data-ttu-id="0940b-127">2 つ目のアプリケーションは、1 つ目のアプリケーションと同じプランを使用するもので、こちらも Java 8/Tomcat 8 アプリケーションとして構成されます。</span><span class="sxs-lookup"><span data-stu-id="0940b-127">The second application uses in the same plan as the first and is also configured as a Java 8/Tomcat 8 application.</span></span> <span data-ttu-id="0940b-128">アンパックされた Java Web アプリケーションが格納されているフォルダーには、JGit ライブラリにより、App Service に対応するディレクトリ構造で新しい Git リポジトリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0940b-128">The JGit libraries create a new Git repository in a folder that contains an unpacked Java web application in a directory structure that maps to App Service.</span></span> <span data-ttu-id="0940b-129">新たにコミットすると、このフォルダー内のファイルが、新しい Git リポジトリに追加され、そのコミットは、Web アプリの `PublishingProfile` に指定されたリモート URL とユーザー名/パスワードで Azure にプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="0940b-129">A new commit adds the files in the folder to the new Git repo, and Git pushes the commit to Azure with a remote URL and username/password provided by the webapp's `PublishingProfile`.</span></span>

<span data-ttu-id="0940b-130">3 つ目のアプリケーションは、Java と Tomcat の構成ではなく、</span><span class="sxs-lookup"><span data-stu-id="0940b-130">The third application is not configured for Java and Tomcat.</span></span> <span data-ttu-id="0940b-131">パブリック GitHub リポジトリにある .NET サンプルを直接ソースからデプロイするものです。</span><span class="sxs-lookup"><span data-stu-id="0940b-131">Instead, a .NET sample in a public GitHub repo is deployed directly from source.</span></span>

<span data-ttu-id="0940b-132">4 つ目のアプリケーションでは、GitHub リポジトリのマスター ブランチに対して変更をプッシュするかプル要求をマージすると、その都度、マスター ブランチにあるコードがデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="0940b-132">The fourth application deploys the code in your master branch every time you push changes or merge a pull request into the GitHub repo's master branch.</span></span>

| <span data-ttu-id="0940b-133">サンプルで使われているクラス</span><span class="sxs-lookup"><span data-stu-id="0940b-133">Class used in sample</span></span> | <span data-ttu-id="0940b-134">メモ</span><span class="sxs-lookup"><span data-stu-id="0940b-134">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="0940b-135">WebApp</span><span class="sxs-lookup"><span data-stu-id="0940b-135">WebApp</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | <span data-ttu-id="0940b-136">`azure.webApps().define()....create()` という fluent チェーンから作成されます。</span><span class="sxs-lookup"><span data-stu-id="0940b-136">Created from the `azure.webApps().define()....create()` fluent chain.</span></span> <span data-ttu-id="0940b-137">App Service Web アプリのほか、そのアプリに必要なすべてのリソースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0940b-137">Creates a App Service web app and any resources needed for the app.</span></span> <span data-ttu-id="0940b-138">そのメソッドは、オブジェクトに構成の詳細を照会するものが大半ですが、動詞を含んだメソッド (`restart()` など) では、Web アプリの状態が変更されます。</span><span class="sxs-lookup"><span data-stu-id="0940b-138">Most methods query the object for configuration details, but verb methods like `restart()` change the state of the webapp.</span></span>
| [<span data-ttu-id="0940b-139">WebContainer</span><span class="sxs-lookup"><span data-stu-id="0940b-139">WebContainer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | <span data-ttu-id="0940b-140">static public フィールドがあるクラスで、Java Web コンテナーを実行する WebApp を定義する際に `withWebContainer()` のパラメーターとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="0940b-140">Class with static public fields used as paramters to `withWebContainer()` when defining a WebApp running a Java webcontainer.</span></span> <span data-ttu-id="0940b-141">Jetty と Tomcat のどちらもバージョンを選択できます。</span><span class="sxs-lookup"><span data-stu-id="0940b-141">Has choices for both Jetty and Tomcat versions.</span></span>
| [<span data-ttu-id="0940b-142">PublishingProfile</span><span class="sxs-lookup"><span data-stu-id="0940b-142">PublishingProfile</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | <span data-ttu-id="0940b-143">WebApp オブジェクトの `getPublishingProfile()` メソッドを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="0940b-143">Obtained through a WebApp object using the `getPublishingProfile()` method.</span></span> <span data-ttu-id="0940b-144">FTP と Git のデプロイ情報を保持します。たとえば、デプロイのユーザー名とパスワード (Azure アカウントやサービス プリンシパルの資格情報とは区別されます) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0940b-144">Contains FTP and Git deployment information, including deployment username and password (which is separate from Azure account or service principal credentials).</span></span>
| [<span data-ttu-id="0940b-145">AppServicePlan</span><span class="sxs-lookup"><span data-stu-id="0940b-145">AppServicePlan</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | <span data-ttu-id="0940b-146">`azure.appServices().appServicePlans().getByResourceGroup()` から返されます。</span><span class="sxs-lookup"><span data-stu-id="0940b-146">Returned by `azure.appServices().appServicePlans().getByResourceGroup()`.</span></span> <span data-ttu-id="0940b-147">プランで実行されている Web アプリの数、レベル、キャパシティをチェックするためのメソッドが利用できます。</span><span class="sxs-lookup"><span data-stu-id="0940b-147">Methods are availble to check the capacity, tier, and number of web apps running in the plan.</span></span>
| [<span data-ttu-id="0940b-148">AppServicePricingTier</span><span class="sxs-lookup"><span data-stu-id="0940b-148">AppServicePricingTier</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | <span data-ttu-id="0940b-149">App Service のレベルを表す static public フィールドがあるクラス。</span><span class="sxs-lookup"><span data-stu-id="0940b-149">Class with static public fields representing App Service tiers.</span></span> <span data-ttu-id="0940b-150">プランのレベルをアプリの作成時に `withPricingTier()` を使ってインラインで定義するとき、または `azure.appServices().appServicePlans().define()` によってプランを定義する際にそのレベルを直接定義するときに使用します。</span><span class="sxs-lookup"><span data-stu-id="0940b-150">Used to define a plan tier in-line during app creation with `withPricingTier()` or directly when defining a plan via `azure.appServices().appServicePlans().define()`</span></span>
| [<span data-ttu-id="0940b-151">JavaVersion</span><span class="sxs-lookup"><span data-stu-id="0940b-151">JavaVersion</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | <span data-ttu-id="0940b-152">App Service によってサポートされる Java のバージョンを表す static public フィールドがあるクラス。</span><span class="sxs-lookup"><span data-stu-id="0940b-152">Class with static public fields representing Java versions supported by App Service.</span></span> <span data-ttu-id="0940b-153">新しい Web アプリを作成するときに `define()...create()` チェーンの中で `withJavaVersion()` と組み合わせて使用します。</span><span class="sxs-lookup"><span data-stu-id="0940b-153">Used with `withJavaVersion()` during the `define()...create()` chain when creating a new webapp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0940b-154">次の手順</span><span class="sxs-lookup"><span data-stu-id="0940b-154">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]