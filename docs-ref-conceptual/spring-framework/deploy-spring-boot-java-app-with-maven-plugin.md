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
ms.openlocfilehash: b133290d1f14429cbf36d6ed5a67d27e1a637593
ms.sourcegitcommit: 599405a9ce892d75073ef0776befa2fa22407b4c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2019
ms.locfileid: "67237600"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="3ccaf-103">Spring Boot JAR ファイルの Web アプリを Linux 用の Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="3ccaf-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="3ccaf-104">この記事では、[Azure App Service Web Apps 用の Maven プラグイン](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)を使用して、Java SE JAR としてパッケージ化された Spring Boot アプリケーションを [Linux の Azure App Services](/azure/app-service/containers/) にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](/azure/app-service/containers/).</span></span> <span data-ttu-id="3ccaf-105">アプリの依存関係、ランタイム、構成を 1 つのデプロイ可能な成果物に統合する必要がある場合は、[Tomcat および WAR ファイル](/azure/app-service/containers/quickstart-java)での Java SE デプロイを選択してください。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="3ccaf-106">Azure サブスクリプションがない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ccaf-107">前提条件</span><span class="sxs-lookup"><span data-stu-id="3ccaf-107">Prerequisites</span></span>

<span data-ttu-id="3ccaf-108">このチュートリアルの手順を完了するには、次をインストールして構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="3ccaf-109">[Azure CLI](/cli/azure/)。ローカルで、または [Azure Cloud Shell](https://shell.azure.com) を介して使用。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="3ccaf-110">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="3ccaf-111">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="3ccaf-112">Apache の [Maven](https://maven.apache.org/) バージョン 3。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-112">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="3ccaf-113">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="install-and-sign-in-to-azure-cli"></a><span data-ttu-id="3ccaf-114">Azure CLI のインストールとサインイン</span><span class="sxs-lookup"><span data-stu-id="3ccaf-114">Install and sign in to Azure CLI</span></span>

<span data-ttu-id="3ccaf-115">Maven プラグインで最もシンプルかつ容易に Spring Boot アプリケーションをデプロイするには、[Azure CLI](/cli/azure/) を使用します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-115">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](/cli/azure/).</span></span>

<span data-ttu-id="3ccaf-116">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-116">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
<span data-ttu-id="3ccaf-117">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-117">Follow the instructions to complete the sign-in process.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="3ccaf-118">サンプル アプリの複製</span><span class="sxs-lookup"><span data-stu-id="3ccaf-118">Clone the sample app</span></span>

<span data-ttu-id="3ccaf-119">このセクションでは、完成した Spring Boot アプリケーションを複製し、ローカルでテストします。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-119">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="3ccaf-120">コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="3ccaf-121">-- または --</span><span class="sxs-lookup"><span data-stu-id="3ccaf-121">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="3ccaf-122">[Spring Boot Getting Started] サンプル プロジェクトを作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-122">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="3ccaf-123">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="3ccaf-124">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="3ccaf-125">Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="3ccaf-126">Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="3ccaf-127">たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="3ccaf-128">次のメッセージが表示されます。**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="3ccaf-128">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="3ccaf-129">Azure App Service 用の Maven プラグインを構成する</span><span class="sxs-lookup"><span data-stu-id="3ccaf-129">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="3ccaf-130">このセクションでは、Maven でアプリを Linux 上の Azure App Service にデプロイできるように、Spring Boot プロジェクト `pom.xml` を構成します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-130">In this section, you will configure the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="3ccaf-131">コード エディターで `pom.xml` を開きます。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-131">Open `pom.xml` in a code editor.</span></span>

2. <span data-ttu-id="3ccaf-132">pom.xml の `<build>` セクションで、`<plugins>` タグ内に次の `<plugin>` エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-132">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.6.0</version>
   </plugin>
   ```

3. <span data-ttu-id="3ccaf-133">次にデプロイを構成し、コマンド プロンプトで maven コマンド `mvn azure-webapp:config` を実行して、**数字**を使用してプロンプトで以下のオプションを選択できます。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-133">Then you can configure the deployment, run the maven command `mvn azure-webapp:config` in the Command Prompt and use the **number** to choose these options in the prompt:</span></span>
    * <span data-ttu-id="3ccaf-134">**OS**: linux</span><span class="sxs-lookup"><span data-stu-id="3ccaf-134">**OS**: linux</span></span>  
    * <span data-ttu-id="3ccaf-135">**javaVersion**: jre8</span><span class="sxs-lookup"><span data-stu-id="3ccaf-135">**javaVersion**: jre8</span></span>
    * <span data-ttu-id="3ccaf-136">**runtimeStack**: jre8</span><span class="sxs-lookup"><span data-stu-id="3ccaf-136">**runtimeStack**: jre8</span></span>

<span data-ttu-id="3ccaf-137">**Confirm (Y/N)** のプロンプトが表示されたら、 **'y'** を押すと構成が終了します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-137">When you get the **Confirm (Y/N)** prompt, press **'y'** and the configuration is done.</span></span>

```cmd
~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< org.springframework:gs-spring-boot >-----------------
[INFO] Building gs-spring-boot 0.1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.6.0:config (default-cli) @ gs-spring-boot ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. jre8 [*]
2. java11
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. jre8
3. TOMCAT 8.5 [*]
4. WILDFLY 14
Enter index to use: 2
Please confirm webapp properties
AppName : gs-spring-boot-1559091271202
ResourceGroup : gs-spring-boot-1559091271202-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : JAVA 8-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

4. <span data-ttu-id="3ccaf-138">`<appSettings>` セクションを `<azure-webapp-maven-plugin>`の `<configuration>` セクションに追加して、*80* のポートでリッスンするようにします。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-138">Add the `<appSettings>` section to the `<configuration>` section of `<azure-webapp-maven-plugin>` to listen on the *80* port.</span></span>

    ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.6.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>

          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          ...
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="3ccaf-139">Azure にアプリケーションをデプロイする</span><span class="sxs-lookup"><span data-stu-id="3ccaf-139">Deploy the app to Azure</span></span>

<span data-ttu-id="3ccaf-140">この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-140">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="3ccaf-141">そのためには、次の手順を実行してください。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-141">To do so, use the following steps:</span></span>

1. <span data-ttu-id="3ccaf-142">*pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-142">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="3ccaf-143">Maven を使って次の例のように Azure に Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-143">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="3ccaf-144">Maven によって、ご自身の Web アプリが Azure にデプロイされます。Web アプリまたは Web アプリ プランが存在しない場合は、Maven によって新たに作成されます。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-144">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="3ccaf-145">Web アプリのデプロイが完了すると、[Azure portal] で Web アプリを管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-145">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="3ccaf-146">Web アプリは **App Services** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-146">Your web app will be listed in **App Services**:</span></span>

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* <span data-ttu-id="3ccaf-148">Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-148">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Web アプリの URL の決定][AP02]

<span data-ttu-id="3ccaf-150">前と同じ cURL コマンドを使用して、`localhost` ではなくポータルからご自身の Web アプリ URL を使って、デプロイが適切に行われたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-150">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="3ccaf-151">次のメッセージが表示されます。**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="3ccaf-151">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3ccaf-152">次の手順</span><span class="sxs-lookup"><span data-stu-id="3ccaf-152">Next steps</span></span>

<span data-ttu-id="3ccaf-153">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-153">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3ccaf-154">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="3ccaf-154">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="3ccaf-155">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="3ccaf-155">Additional Resources</span></span>

<span data-ttu-id="3ccaf-156">この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3ccaf-156">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="3ccaf-157">[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]</span><span class="sxs-lookup"><span data-stu-id="3ccaf-157">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="3ccaf-158">Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="3ccaf-158">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="3ccaf-159">Azure CLI 2.0 で Azure サービス プリンシパルを作成する</span><span class="sxs-lookup"><span data-stu-id="3ccaf-159">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="3ccaf-160">Maven の設定リファレンス</span><span class="sxs-lookup"><span data-stu-id="3ccaf-160">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /java/azure/
[Azure Portal]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
