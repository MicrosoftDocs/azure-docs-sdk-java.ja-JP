---
title: Azure Web Apps 用の Maven プラグインを使用して Azure Container Registry の Spring Boot アプリを Azure App Service にデプロイする方法
description: このチュートリアルでは、Maven プラグインを使用して Azure Container Registry の Spring Boot アプリケーションを Azure App Service にデプロイする方法について順を追って説明します。
services: container-registry
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: abe73f46e3b5a3b85a9f0272c12539d230c1a879
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991376"
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a><span data-ttu-id="97aa3-103">Azure Web Apps 用の Maven プラグインを使用して Azure Container Registry の Spring Boot アプリを Azure App Service にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="97aa3-103">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app in Azure Container Registry to Azure App Service</span></span>

<span data-ttu-id="97aa3-104">この記事では、[Spring Boot] サンプル アプリケーションを Azure Container Registry にデプロイし、Azure Web Apps 用の Maven プラグインを使用して、アプリケーションを Azure App Service にデプロイする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-104">This article demonstrates how to deploy a sample [Spring Boot] application to Azure Container Registry, and then use the Maven Plugin for Azure Web Apps to deploy your application to Azure App Service.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="97aa3-105">[Apache Maven](http://maven.apache.org/) 用の Azure Web Apps 用 Maven プラグイン は、Maven プロジェクトに Azure App Service をシームレスに統合し、開発者が Web アプリを Azure App Service にデプロイするプロセスを効率化します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-105">The Maven Plugin for Azure Web Apps for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>
> 
> <span data-ttu-id="97aa3-106">Azure Web Apps の Maven プラグインは現在プレビューとして提供されています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="97aa3-107">今後、機能が追加される予定ですが、現在は FTP 発行のみがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="97aa3-108">前提条件</span><span class="sxs-lookup"><span data-stu-id="97aa3-108">Prerequisites</span></span>

<span data-ttu-id="97aa3-109">このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="97aa3-110">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="97aa3-111">[Azure コマンド ライン インターフェイス (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="97aa3-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="97aa3-112">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="97aa3-112">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="97aa3-113">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="97aa3-113">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="97aa3-114">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="97aa3-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="97aa3-115">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="97aa3-115">A [Git] client.</span></span>
* <span data-ttu-id="97aa3-116">[Docker] クライアント。</span><span class="sxs-lookup"><span data-stu-id="97aa3-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="97aa3-117">このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="97aa3-118">Docker Web アプリの Spring Boot サンプルの複製</span><span class="sxs-lookup"><span data-stu-id="97aa3-118">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="97aa3-119">このセクションでは、コンテナー化された Spring Boot アプリケーションを複製してローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="97aa3-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="97aa3-120">コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="97aa3-121">-- または --</span><span class="sxs-lookup"><span data-stu-id="97aa3-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="97aa3-122">[Docker での Spring Boot の使用開始]のサンプル プロジェクトを今作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone -b https://github.com/spring-guides/gs-spring-boot-docker
   ```

1. <span data-ttu-id="97aa3-123">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="97aa3-124">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="97aa3-125">Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="97aa3-126">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="97aa3-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="97aa3-127">たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="97aa3-128">次のメッセージが表示されます。**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="97aa3-128">You should see the following message displayed: **Hello Docker World**</span></span>

   ![サンプル アプリをローカルに参照する][SB01]

> [!NOTE]
>
> <span data-ttu-id="97aa3-130">ローカルで Docker を使用していると、ポート 2375 で localhost に接続できないことを示すエラーが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-130">When you are using Docker locally, you may see an error which states that you cannot connect to localhost on port 2375.</span></span> <span data-ttu-id="97aa3-131">これが発生した場合は、TLS を使用せずにローカルでの Docker の使用を有効にすることが必要になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-131">If this happens, you may need to enable using Docker locally without TLS.</span></span> <span data-ttu-id="97aa3-132">これを行うには、Docker の設定を開き、**[Expose daemon on TCP://localhost:2375 without TLS]\(TLS を使用せずに TCP://localhost:2375 でデーモンを公開する\)** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="97aa3-132">To do so, open your Docker settings and check the option to **Expose daemon on TCP://localhost:2375 without TLS**.</span></span>
>
> ![ローカルの TCP ポート 2375 で Docker デーモンを公開する][TL01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="97aa3-134">Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="97aa3-134">Create an Azure service principal</span></span>

<span data-ttu-id="97aa3-135">このセクションでは、Azure にコンテナーをデプロイするときに、Maven プラグインが使用する Azure サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-135">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="97aa3-136">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-136">Open a command prompt.</span></span>

2. <span data-ttu-id="97aa3-137">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="97aa3-137">Sign into your Azure account by using the Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="97aa3-138">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-138">Follow the instructions to complete the sign-in process.</span></span>

3. <span data-ttu-id="97aa3-139">Azure サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-139">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="97aa3-140">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="97aa3-140">Where:</span></span>

   | <span data-ttu-id="97aa3-141">パラメーター</span><span class="sxs-lookup"><span data-stu-id="97aa3-141">Parameter</span></span>  |                    <span data-ttu-id="97aa3-142">説明</span><span class="sxs-lookup"><span data-stu-id="97aa3-142">Description</span></span>                     |
   |------------|----------------------------------------------------|
   | `uuuuuuuu` | <span data-ttu-id="97aa3-143">サービス プリンシパルのユーザー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-143">Specifies the user name for the service principal.</span></span> |
   | `pppppppp` | <span data-ttu-id="97aa3-144">サービス プリンシパルのパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-144">Specifies the password for the service principal.</span></span>  |


4. <span data-ttu-id="97aa3-145">Azure が次の例に類似する JSON で応答します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-145">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="97aa3-146">Maven プラグインを構成して Azure にコンテナーをデプロイするときに、この JSON の応答にある値を使用します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-146">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="97aa3-147">`aaaaaaaa``uuuuuuuu``pppppppp``tttttttt` はプレースホルダーの値であり、次のセクションで Maven の `settings.xml` ファイルを構成するときに、これらの値と値の各要素を簡単にマップできるよう使用されています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-147">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="97aa3-148">Azure CLI を使用して Azure Container Registry を作成する</span><span class="sxs-lookup"><span data-stu-id="97aa3-148">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="97aa3-149">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-149">Open a command prompt.</span></span>

1. <span data-ttu-id="97aa3-150">Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="97aa3-150">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="97aa3-151">この記事で使用する Azure リソースのリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-151">Create a resource group for the Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="97aa3-152">この例の `wingtiptoysresources` をご利用のリソース グループの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-152">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="97aa3-153">リソース グループ内に、ご利用の Spring Boot アプリ用のプライベートな Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-153">Create a private Azure container registry in the resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="97aa3-154">この例の `wingtiptoysregistry` をコンテナー レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-154">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="97aa3-155">コンテナー レジストリのパスワードを取得します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-155">Retrieve the password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="97aa3-156">Azure が次の例のようにパスワードを返します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-156">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a><span data-ttu-id="97aa3-157">Maven の設定に Azure コンテナー レジストリと Azure サービス プリンシパルを追加する</span><span class="sxs-lookup"><span data-stu-id="97aa3-157">Add your Azure container registry and Azure service principal to your Maven settings</span></span>

1. <span data-ttu-id="97aa3-158">Maven の `settings.xml` ファイルをテキスト エディターで開きます。このファイルは次の例のようなパスに存在していることがあります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-158">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

2. <span data-ttu-id="97aa3-159">この記事の前のセクションで説明した Azure Container Registry のアクセス設定を、次の例のように *settings.xml* ファイルの `<servers>` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-159">Add your Azure Container Registry access settings from the previous section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="97aa3-160">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="97aa3-160">Where:</span></span>

   |   <span data-ttu-id="97aa3-161">要素</span><span class="sxs-lookup"><span data-stu-id="97aa3-161">Element</span></span>    |                                 <span data-ttu-id="97aa3-162">説明</span><span class="sxs-lookup"><span data-stu-id="97aa3-162">Description</span></span>                                  |
   |--------------|------------------------------------------------------------------------------|
   |    `<id>`    |         <span data-ttu-id="97aa3-163">プライベートな Azure コンテナー レジストリの名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-163">Contains the name of your private Azure container registry.</span></span>          |
   | `<username>` |         <span data-ttu-id="97aa3-164">プライベートな Azure コンテナー レジストリの名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-164">Contains the name of your private Azure container registry.</span></span>          |
   | `<password>` | <span data-ttu-id="97aa3-165">この記事の前のセクションで取得したパスワードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-165">Contains the password you retrieved in the previous section of this article.</span></span> |


3. <span data-ttu-id="97aa3-166">この記事の前のセクションで説明した Azure サービス プリンシパル設定を、次の例のように *settings.xml* ファイルの `<servers>` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-166">Add your Azure service principal settings from an earlier section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="97aa3-167">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="97aa3-167">Where:</span></span>

   |     <span data-ttu-id="97aa3-168">要素</span><span class="sxs-lookup"><span data-stu-id="97aa3-168">Element</span></span>     |                                                                                   <span data-ttu-id="97aa3-169">説明</span><span class="sxs-lookup"><span data-stu-id="97aa3-169">Description</span></span>                                                                                   |
   |-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `<id>`      |                                <span data-ttu-id="97aa3-170">Web アプリを Azure にデプロイするとき、セキュリティ設定を検索するために Maven が使う一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-170">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>                                |
   |   `<client>`    |                                                             <span data-ttu-id="97aa3-171">サービス プリンシパルの `appId` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-171">Contains the `appId` value from your service principal.</span></span>                                                             |
   |   `<tenant>`    |                                                            <span data-ttu-id="97aa3-172">サービス プリンシパルの `tenant` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-172">Contains the `tenant` value from your service principal.</span></span>                                                             |
   |     `<key>`     |                                                           <span data-ttu-id="97aa3-173">サービス プリンシパルの `password` 値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-173">Contains the `password` value from your service principal.</span></span>                                                            |
   | `<environment>` | <span data-ttu-id="97aa3-174">ターゲットの Azure クラウド環境を定義します。この例では `AZURE` です </span><span class="sxs-lookup"><span data-stu-id="97aa3-174">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="97aa3-175">(環境の全リストは、「[Azure Web Apps 用の Maven プラグイン]」のドキュメントに記載しています)</span><span class="sxs-lookup"><span data-stu-id="97aa3-175">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span> |


4. <span data-ttu-id="97aa3-176">*settings.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-176">Save and close the *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a><span data-ttu-id="97aa3-177">Docker コンテナー イメージを構築し、Azure コンテナー レジストリにプッシュする</span><span class="sxs-lookup"><span data-stu-id="97aa3-177">Build your Docker container image and push it to your Azure container registry</span></span>

1. <span data-ttu-id="97aa3-178">Spring Boot アプリケーションの完了プロジェクトディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-178">Navigate to the completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

2. <span data-ttu-id="97aa3-179">*pom.xml* ファイル内の `<properties>` コレクションを、このチュートリアルの前のセクションにあった Azure Container Registry のログイン サーバー値で更新します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-179">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="97aa3-180">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="97aa3-180">Where:</span></span>

   |           <span data-ttu-id="97aa3-181">要素</span><span class="sxs-lookup"><span data-stu-id="97aa3-181">Element</span></span>           |                                                                       <span data-ttu-id="97aa3-182">説明</span><span class="sxs-lookup"><span data-stu-id="97aa3-182">Description</span></span>                                                                       |
   |-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `<azure.containerRegistry>` |                                              <span data-ttu-id="97aa3-183">プライベートな Azure コンテナー レジストリの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-183">Specifies the name of your private Azure container registry.</span></span>                                               |
   |   `<docker.image.prefix>`   | <span data-ttu-id="97aa3-184">プライベートな Azure コンテナー レジストリの URL (プライベートなコンテナー レジストリの名前に ".azurecr.io" が追加されたもの) を指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-184">Specifies the URL of your private Azure container registry, which is derived by appending ".azurecr.io" to the name of your private container registry.</span></span> |


3. <span data-ttu-id="97aa3-185">*pom.xml* ファイル内の Docker プラグインの `<plugin>` に、このチュートリアルの前の手順で説明したログイン サーバー アドレスとレジストリ名の正しいプロパティが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-185">Verify that `<plugin>` for the Docker plugin in your *pom.xml* file contains the correct properties for the login server address and registry name from the previous step in this tutorial.</span></span> <span data-ttu-id="97aa3-186">例: </span><span class="sxs-lookup"><span data-stu-id="97aa3-186">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   <span data-ttu-id="97aa3-187">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="97aa3-187">Where:</span></span>

   |     <span data-ttu-id="97aa3-188">要素</span><span class="sxs-lookup"><span data-stu-id="97aa3-188">Element</span></span>     |                                       <span data-ttu-id="97aa3-189">説明</span><span class="sxs-lookup"><span data-stu-id="97aa3-189">Description</span></span>                                       |
   |-----------------|-----------------------------------------------------------------------------------------|
   |  `<serverId>`   |  <span data-ttu-id="97aa3-190">プライベートな Azure コンテナー レジストリの名前を含むプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-190">Specifies the property which contains name of your private Azure container registry.</span></span>   |
   | `<registryUrl>` | <span data-ttu-id="97aa3-191">プライベートな Azure コンテナー レジストリの URL を含むプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-191">Specifies the property which contains the URL of your private Azure container registry.</span></span> |


4. <span data-ttu-id="97aa3-192">Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行してアプリケーションをリビルドし、コンテナーを Azure コンテナー レジストリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="97aa3-192">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

5. <span data-ttu-id="97aa3-193">省略可能:[Azure portal] を参照して、コンテナー レジストリに **gs-spring-boot-docker** という名前の Docker コンテナー イメージがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-193">OPTIONAL: Browse to the [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![Azure Portal でコンテナーを確認][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a><span data-ttu-id="97aa3-195">pom.xml をカスタマイズしてコンテナーを構築し Azure にデプロイする</span><span class="sxs-lookup"><span data-stu-id="97aa3-195">Customize your pom.xml, then build and deploy your container to Azure</span></span>

<span data-ttu-id="97aa3-196">Spring Boot アプリケーションの `pom.xml` ファイルをテキスト エディターで開き、`azure-webapp-maven-plugin` の `<plugin>` 要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-196">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="97aa3-197">この要素は次の例のようになっています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-197">This element should resemble the following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
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

<span data-ttu-id="97aa3-198">Maven プラグイン用に変更できる値は複数あります。これらの要素に関する詳しい説明はそれぞれ「[Azure Web Apps 用の Maven プラグイン]」のドキュメントに記載されています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-198">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="97aa3-199">この記事でも、次のように重要な値については説明します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-199">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="97aa3-200">要素</span><span class="sxs-lookup"><span data-stu-id="97aa3-200">Element</span></span> | <span data-ttu-id="97aa3-201">説明</span><span class="sxs-lookup"><span data-stu-id="97aa3-201">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="97aa3-202">[Azure Web Apps 用の Maven プラグイン]のバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-202">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="97aa3-203">最新バージョンを使用していることを確認するために、[Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) で一覧表示されているバージョンを確認してください。</span><span class="sxs-lookup"><span data-stu-id="97aa3-203">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<authentication>` | <span data-ttu-id="97aa3-204">Azure の認証情報を指定します。この例では `azure-auth` を含む `<serverId>` 要素が認証情報です。Maven はこの値を、この記事の前のセクションで定義した Maven の*settings.xml* ファイル内にある Azure サービス プリンシパルを見つけるために使います。</span><span class="sxs-lookup"><span data-stu-id="97aa3-204">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="97aa3-205">ターゲット リソース グループを指定します。この例では `wingtiptoysresources` です。</span><span class="sxs-lookup"><span data-stu-id="97aa3-205">Specifies the target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="97aa3-206">リソース グループが存在しない場合は、デプロイ中に新しいリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-206">The resource group will be created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="97aa3-207">Web アプリのターゲット名を指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-207">Specifies the target name for your web app.</span></span> <span data-ttu-id="97aa3-208">この例では、ターゲット名は `maven-linux-app-${maven.build.timestamp}` です。混乱を避けるため、この例ではサフィックスの `${maven.build.timestamp}` を追加しています </span><span class="sxs-lookup"><span data-stu-id="97aa3-208">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="97aa3-209">(タイムスタンプは省略可能です。アプリ名には一意の文字列を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="97aa3-209">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="97aa3-210">ターゲット リージョンを指定します。この例では `westus` です </span><span class="sxs-lookup"><span data-stu-id="97aa3-210">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="97aa3-211">(全リストは、「[Azure Web Apps 用の Maven プラグイン]」のドキュメントに記載しています。)</span><span class="sxs-lookup"><span data-stu-id="97aa3-211">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<containerSettings>` | <span data-ttu-id="97aa3-212">コンテナーの名前を含むプロパティと URL を含むプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-212">Specifies the properties which contain the name and URL of your container.</span></span> |
| `<appSettings>` | <span data-ttu-id="97aa3-213">Azure に Web アプリをデプロイするときに使用するために、Maven 用の一意の設定を指定します。</span><span class="sxs-lookup"><span data-stu-id="97aa3-213">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="97aa3-214">この例では、`<property>` 要素には、アプリのポートを指定する子要素の名前と値のペアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="97aa3-214">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span> |

> [!NOTE]
>
> <span data-ttu-id="97aa3-215">既定と異なるポートに変更する場合のみ、この例のポート番号を変更する設定が必要になります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-215">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

1. <span data-ttu-id="97aa3-216">*pom.xml* ファイルに変更を加える場合は、以前使用していたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使用して JAR ファイルをリビルドします。</span><span class="sxs-lookup"><span data-stu-id="97aa3-216">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="97aa3-217">Maven を使って次の例のように Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="97aa3-217">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="97aa3-218">Maven が Web アプリを Azure にデプロイします。Web アプリが存在しない場合は新たに作成されます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-218">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="97aa3-219">デプロイ開始時に、*pom.xml* ファイルの `<region>` 要素で指定したリージョンに十分な数の使用可能なサーバーがない場合は、次の例のようなエラーが表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-219">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
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
> <span data-ttu-id="97aa3-220">この場合、別のリージョンを指定し、Maven コマンドを再実行してアプリケーションをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-220">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="97aa3-221">Web アプリのデプロイが完了すると、[Azure Portal] を使用して Web アプリを管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="97aa3-221">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="97aa3-222">Web アプリは **App Services** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-222">Your web app will be listed in **App Services**:</span></span>

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* <span data-ttu-id="97aa3-224">Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="97aa3-224">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Web アプリの URL の決定][AP02]

## <a name="next-steps"></a><span data-ttu-id="97aa3-226">次の手順</span><span class="sxs-lookup"><span data-stu-id="97aa3-226">Next steps</span></span>

<span data-ttu-id="97aa3-227">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="97aa3-227">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="97aa3-228">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="97aa3-228">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="97aa3-229">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="97aa3-229">Additional Resources</span></span>

<span data-ttu-id="97aa3-230">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="97aa3-230">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="97aa3-231">[Azure Web Apps 用の Maven プラグイン]</span><span class="sxs-lookup"><span data-stu-id="97aa3-231">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="97aa3-232">Azure CLI から Azure へのログイン</span><span class="sxs-lookup"><span data-stu-id="97aa3-232">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="97aa3-233">Azure CLI 2.0 で Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="97aa3-233">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="97aa3-234">Maven の設定リファレンス</span><span class="sxs-lookup"><span data-stu-id="97aa3-234">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="97aa3-235">[Maven 用の Docker プラグイン]</span><span class="sxs-lookup"><span data-stu-id="97aa3-235">[Docker plugin for Maven]</span></span>

<span data-ttu-id="97aa3-236">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="97aa3-236">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure Portal]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Azure Web Apps 用の Maven プラグイン]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service/containers/tutorial-custom-docker-image
[Docker]: https://www.docker.com/
[Maven 用の Docker プラグイン]: https://github.com/spotify/docker-maven-plugin
[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Azure DevOps と Java の操作]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker での Spring Boot の使用開始]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP02.png
[TL01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/TL01.png
