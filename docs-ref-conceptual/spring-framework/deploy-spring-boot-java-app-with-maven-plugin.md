---
title: Maven と Azure を使用して Spring Boot JAR ファイルのアプリをクラウドにデプロイする
description: Linux 用の Azure Web Apps の Maven プラグインを使って、Spring Boot アプリをクラウドにデプロイする方法について説明します。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.author: robmcm;kevinzha;brborges
ms.date: 10/04/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: 36afcc764c1cb984779518ddec004ecbfa1b7c57
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48876396"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="3404c-103">Spring Boot JAR ファイルの Web アプリを Linux 用の Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="3404c-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="3404c-104">この記事では、[Azure App Service Web Apps 用の Maven プラグイン](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)を使用して、Java SE JAR としてパッケージ化された Spring Boot アプリケーションを [Linux の Azure App Services](https://docs.microsoft.com/en-us/azure/app-service/containers/) にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3404c-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/).</span></span> <span data-ttu-id="3404c-105">ご自身のアプリの依存関係、ランタイム、および構成を 1 つのデプロイ可能な成果物に統合する必要がある場合は、[Tomcat および WAR ファイル](/azure/app-service/containers/quickstart-java)での Java SE デプロイを選択してください。</span><span class="sxs-lookup"><span data-stu-id="3404c-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's depedencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="3404c-106">Azure サブスクリプションがない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。</span><span class="sxs-lookup"><span data-stu-id="3404c-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3404c-107">前提条件</span><span class="sxs-lookup"><span data-stu-id="3404c-107">Prerequisites</span></span>

<span data-ttu-id="3404c-108">このチュートリアルの手順を完了するには、次をインストールして構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3404c-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="3404c-109">[Azure CLI](/cli/azure/)。ローカルで、または [Azure Cloud Shell](https://shell.azure.com) を介して使用。</span><span class="sxs-lookup"><span data-stu-id="3404c-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="3404c-110">[Java Development Kit (JDK)](https://www.azul.com/downloads/azure-only/zulu/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="3404c-110">[Java Development Kit (JDK)](https://www.azul.com/downloads/azure-only/zulu/), version 1.7 or later.</span></span>
* <span data-ttu-id="3404c-111">Apache の [Maven](https://maven.apache.org/) バージョン 3。</span><span class="sxs-lookup"><span data-stu-id="3404c-111">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="3404c-112">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="3404c-112">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="3404c-113">サンプル アプリの複製</span><span class="sxs-lookup"><span data-stu-id="3404c-113">Clone the sample app</span></span>

<span data-ttu-id="3404c-114">このセクションでは、完成した Spring Boot アプリケーションを複製し、ローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="3404c-114">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="3404c-115">コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="3404c-115">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="3404c-116">-- または --</span><span class="sxs-lookup"><span data-stu-id="3404c-116">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="3404c-117">[Spring Boot Getting Started] サンプル プロジェクトを作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3404c-117">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="3404c-118">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3404c-118">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="3404c-119">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3404c-119">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="3404c-120">Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="3404c-120">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="3404c-121">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="3404c-121">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="3404c-122">たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="3404c-122">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="3404c-123">**Greetings from Spring Boot! (Spring Boot からのあいさつ)** というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3404c-123">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="3404c-124">Azure App Service 用の Maven プラグインを構成する</span><span class="sxs-lookup"><span data-stu-id="3404c-124">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="3404c-125">このセクションでは、Maven でアプリを Linux の Azure App Service にデプロイできるように、Spring Boot プロジェクト `pom.xml` を構成します。</span><span class="sxs-lookup"><span data-stu-id="3404c-125">In this section, you will configure the the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="3404c-126">コード エディターで `pom.xml` を開きます。</span><span class="sxs-lookup"><span data-stu-id="3404c-126">Open `pom.xml` in a code editor.</span></span>

1. <span data-ttu-id="3404c-127">pom.xml の `<build>` セクションで、`<plugins>` タグ内に次の `<plugin>` エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="3404c-127">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
  <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.4.0</version>
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

1. <span data-ttu-id="3404c-128">プラグイン構成で、次のプレースホルダーを更新します。</span><span class="sxs-lookup"><span data-stu-id="3404c-128">Update the following placeholders in the plugin configuration:</span></span>

| <span data-ttu-id="3404c-129">プレースホルダー</span><span class="sxs-lookup"><span data-stu-id="3404c-129">Placeholder</span></span> | <span data-ttu-id="3404c-130">説明</span><span class="sxs-lookup"><span data-stu-id="3404c-130">Description</span></span> |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | <span data-ttu-id="3404c-131">Web アプリの作成先となる新しいリソース グループの名前。</span><span class="sxs-lookup"><span data-stu-id="3404c-131">Name for the new resource group in which to create your web app.</span></span> <span data-ttu-id="3404c-132">アプリのすべてのリソースを 1 つのグループ内に配置することで、それらを一緒に管理できます。</span><span class="sxs-lookup"><span data-stu-id="3404c-132">By putting all the resources for an app in a group, you can manage them together.</span></span> <span data-ttu-id="3404c-133">たとえば、リソース グループを削除すれば、そのアプリに関連付けられているすべてのリソースが削除されます。</span><span class="sxs-lookup"><span data-stu-id="3404c-133">For example, deleting the resource group would delete all resources associated with the app.</span></span> <span data-ttu-id="3404c-134">この値を一意の新しいリソース グループ名 (たとえば、*TestResources*) で更新します。</span><span class="sxs-lookup"><span data-stu-id="3404c-134">Update this value with a unique new resource group name, for example, *TestResources*.</span></span> <span data-ttu-id="3404c-135">このリソース グループ名を使用して、後のセクションですべての Azure リソースをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="3404c-135">You will use this resource group name to clean up all Azure resources in a later section.</span></span> |
| `WEBAPP_NAME` | <span data-ttu-id="3404c-136">Azure にデプロイされると、このアプリ名は Web アプリのホスト名の一部になります (WEBAPP_NAME.azurewebsites.net)。</span><span class="sxs-lookup"><span data-stu-id="3404c-136">The app name will be part the host name for the web app when deployed to Azure (WEBAPP_NAME.azurewebsites.net).</span></span> <span data-ttu-id="3404c-137">この値を、Java アプリをホストする新しい Azure Web アプリの一意の名前 (たとえば、*contoso*) で更新します。</span><span class="sxs-lookup"><span data-stu-id="3404c-137">Update this value with a unique name for the new Azure web app, which will host your Java app, for example *contoso*.</span></span> |
| `REGION` | <span data-ttu-id="3404c-138">Web アプリがホストされている Azure リージョン (たとえば、`westus2`)。</span><span class="sxs-lookup"><span data-stu-id="3404c-138">An Azure region where the web app is hosted, for example `westus2`.</span></span> <span data-ttu-id="3404c-139">リージョンの一覧は、`az account list-locations` コマンドを使用して Cloud Shell または CLI から取得できます。</span><span class="sxs-lookup"><span data-stu-id="3404c-139">You can get a list of regions from the Cloud Shell or CLI using the `az account list-locations` command.</span></span> |

<span data-ttu-id="3404c-140">構成オプションの完全な一覧については、[GitHub の Maven プラグイン リファレンス](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3404c-140">A full list of configuration options can be found in the [Maven plugin reference on GitHub](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin).</span></span>

## <a name="install-and-log-in-to-azure-cli"></a><span data-ttu-id="3404c-141">Azure CLI のインストールとログイン</span><span class="sxs-lookup"><span data-stu-id="3404c-141">Install and log in to Azure CLI</span></span>

<span data-ttu-id="3404c-142">Maven プラグインで最もシンプルかつ容易に Spring Boot アプリケーションをデプロイするには、[Azure CLI](https://docs.microsoft.com/cli/azure/) を使用します。</span><span class="sxs-lookup"><span data-stu-id="3404c-142">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span>

1. <span data-ttu-id="3404c-143">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="3404c-143">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
   <span data-ttu-id="3404c-144">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="3404c-144">Follow the instructions to complete the sign-in process.</span></span>

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="3404c-145">Azure にアプリケーションをデプロイする</span><span class="sxs-lookup"><span data-stu-id="3404c-145">Deploy the app to Azure</span></span>

<span data-ttu-id="3404c-146">この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3404c-146">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="3404c-147">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="3404c-147">To do so, use the following steps:</span></span>

1. <span data-ttu-id="3404c-148">*pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。</span><span class="sxs-lookup"><span data-stu-id="3404c-148">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="3404c-149">Maven を使って次の例のように Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3404c-149">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="3404c-150">Maven によって、ご自身の Web アプリが Azure にデプロイされます。Web アプリまたは Web アプリ プランが存在しない場合は、Maven によって新たに作成されます。</span><span class="sxs-lookup"><span data-stu-id="3404c-150">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="3404c-151">Web アプリのデプロイが完了すると、[Azure portal] で Web アプリを管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="3404c-151">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="3404c-152">Web アプリは **App Services** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="3404c-152">Your web app will be listed in **App Services**:</span></span>

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* <span data-ttu-id="3404c-154">Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="3404c-154">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Web アプリの URL の決定][AP02]

<span data-ttu-id="3404c-156">前と同じ cURL コマンドを使用して、`localhost` ではなくポータルからご自身の Web アプリ URL を使って、デプロイが適切に行われたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="3404c-156">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="3404c-157">**Greetings from Spring Boot! (Spring Boot からのあいさつ)** というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3404c-157">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3404c-158">次の手順</span><span class="sxs-lookup"><span data-stu-id="3404c-158">Next steps</span></span>

<span data-ttu-id="3404c-159">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3404c-159">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="3404c-160">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]</span><span class="sxs-lookup"><span data-stu-id="3404c-160">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="3404c-161">Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="3404c-161">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="3404c-162">Azure CLI 2.0 で Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="3404c-162">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="3404c-163">Maven の設定リファレンス</span><span class="sxs-lookup"><span data-stu-id="3404c-163">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure Portal]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
