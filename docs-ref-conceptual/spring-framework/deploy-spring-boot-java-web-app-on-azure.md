---
title: Azure App Service で Spring Boot アプリケーションをクラウドにデプロイする
description: このチュートリアルでは、Azure App Service を使用して Spring Boot Getting Started Web アプリをクラウドにデプロイする手順について説明します。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: adf779e2ba6ca73ea3a2406613f9622cc9ecbf99
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954525"
---
# <a name="deploy-a-spring-boot-application-to-the-cloud-with-azure-app-service"></a><span data-ttu-id="deef0-103">Azure App Service で Spring Boot アプリケーションをクラウドにデプロイする</span><span class="sxs-lookup"><span data-stu-id="deef0-103">Deploy a Spring Boot application to the cloud with Azure App Service</span></span>

<span data-ttu-id="deef0-104">このチュートリアルでは、サンプルの [Spring Boot] Getting Started Web アプリを作成し、それを [Azure App Service] にデプロイする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="deef0-104">This tutorial will walk you though creating a sample [Spring Boot] Getting Started web app and deploying it to [Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="deef0-105">前提条件</span><span class="sxs-lookup"><span data-stu-id="deef0-105">Prerequisites</span></span>

<span data-ttu-id="deef0-106">このチュートリアルの手順を完了するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="deef0-106">In order to complete the steps in this tutorial, you need to have the following:</span></span>

* <span data-ttu-id="deef0-107">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="deef0-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="deef0-108">最新の [Java Developer Kit (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="deef0-108">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="deef0-109">Apache の [Maven] 構築ツール (バージョン 3)。</span><span class="sxs-lookup"><span data-stu-id="deef0-109">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="deef0-110">[Git] クライアント。</span><span class="sxs-lookup"><span data-stu-id="deef0-110">A [Git] client.</span></span>

## <a name="create-the-spring-boot-getting-started-web-app"></a><span data-ttu-id="deef0-111">Spring Boot Getting Started Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="deef0-111">Create the Spring Boot Getting Started web app</span></span>

<span data-ttu-id="deef0-112">次の手順では、単純な Spring Boot Web アプリケーションを作成し、それをローカルにテストするために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="deef0-112">The following steps will walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="deef0-113">コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成して、そのディレクトリに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="deef0-113">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="deef0-114">-- または --</span><span class="sxs-lookup"><span data-stu-id="deef0-114">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="deef0-115">[Spring Boot Getting Started] サンプル プロジェクトを今作成したディレクトリに複製します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="deef0-115">Clone the [Spring Boot Getting Started] sample project into the directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="deef0-116">完成したプロジェクトにディレクトリを変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="deef0-116">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="deef0-117">Maven を使用して JAR ファイルを構築します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="deef0-117">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="deef0-118">Web アプリが作成されたら、JAR ファイルにディレクトリを変更し、Web アプリを起動します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="deef0-118">Once the web app has been created, change directory to the JAR file and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="deef0-119">Web ブラウザーを使用して http://localhost:8080 を参照することによって Web アプリをテストするか、または curl が使用可能な場合は次の例のような構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="deef0-119">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="deef0-120">**Greetings from Spring Boot! (Spring Boot からのあいさつ)** というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="deef0-120">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![サンプル アプリを見る][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="deef0-122">Java で使用する Azure Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="deef0-122">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="deef0-123">次の手順では、Azure Web アプリを作成し、Java に必要な設定を構成して、FTP 資格情報を構成する手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="deef0-123">The following steps will walk you through the steps to create an Azure Web App, configure the required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="deef0-124">[Azure Portal] を参照して、ログインします。</span><span class="sxs-lookup"><span data-stu-id="deef0-124">Browse to the [Azure portal] and log in.</span></span>

1. <span data-ttu-id="deef0-125">Azure Portal で自分のアカウントにログインしたら、**[App Services]** のメニュー アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-125">Once you have logged into your account on the Azure portal, click the menu icon for **App Services**:</span></span>
   
   ![Azure ポータル][AZ01]

1. <span data-ttu-id="deef0-127">**[App Services]** ページが表示されたら、**[+ 追加]** をクリックして新しい App Service を作成します。</span><span class="sxs-lookup"><span data-stu-id="deef0-127">When the **App Services** page is displayed, click **+ Add** to create a new App Service.</span></span>

   ![App Service を作成する][AZ02]

1. <span data-ttu-id="deef0-129">Web アプリ テンプレートの一覧が表示されたら、基本的な Microsoft Web アプリのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-129">When the list of web app templates is displayed, click the link for the basic Microsoft Web App.</span></span>

   ![Web アプリ テンプレート][AZ03]

1. <span data-ttu-id="deef0-131">Web アプリ テンプレートの情報ページが表示されたら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-131">When the information page for the Web App template is displayed, click **Create**.</span></span>

   ![Create Web App][AZ04]

1. <span data-ttu-id="deef0-133">Web アプリの一意の名前を指定し、追加の設定をすべて指定してから、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-133">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![Web アプリの設定を作成する][AZ05]

1. <span data-ttu-id="deef0-135">Web アプリが作成されたら、**[App Services]** のメニュー アイコンをクリックしてから、新しく作成された Web アプリをクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-135">Once your web app has been created, click the menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![Web アプリを一覧表示する][AZ06]

1. <span data-ttu-id="deef0-137">Web アプリが表示されたら、次の手順を使用して Java バージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="deef0-137">When your web app is displayed, specify the Java version by using the following steps:</span></span>

   <span data-ttu-id="deef0-138">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="deef0-138">a.</span></span> <span data-ttu-id="deef0-139">**[アプリケーションの設定]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-139">Click the **Application Settings** menu item.</span></span>

   <span data-ttu-id="deef0-140">b.</span><span class="sxs-lookup"><span data-stu-id="deef0-140">b.</span></span> <span data-ttu-id="deef0-141">Java バージョンとして **[Java 8]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="deef0-141">Choose **Java 8** for the Java version.</span></span>

   <span data-ttu-id="deef0-142">c.</span><span class="sxs-lookup"><span data-stu-id="deef0-142">c.</span></span> <span data-ttu-id="deef0-143">マイナー Java バージョンとして **[Newest] \(最新)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="deef0-143">Choose **Newest** for the minor Java version.</span></span>

   <span data-ttu-id="deef0-144">d.</span><span class="sxs-lookup"><span data-stu-id="deef0-144">d.</span></span> <span data-ttu-id="deef0-145">Web コンテナーとして **[Newest Tomcat 8.5] \(最新の Tomcat 8.5)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="deef0-145">Choose **Newest Tomcat 8.5** for the web container.</span></span> <span data-ttu-id="deef0-146">(このコンテナーは、実際には使用されません。Azure は、Spring Boot アプリケーションからコンテナーを使用します。)</span><span class="sxs-lookup"><span data-stu-id="deef0-146">(This container will not actually be used; Azure will use the container from your Spring Boot application.)</span></span>

   <span data-ttu-id="deef0-147">e.</span><span class="sxs-lookup"><span data-stu-id="deef0-147">e.</span></span> <span data-ttu-id="deef0-148">**[Save]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-148">Click **Save**.</span></span>

   ![アプリケーションの設定][AZ07]

1. <span data-ttu-id="deef0-150">次の手順を使用して FTP デプロイ資格情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="deef0-150">Specify your FTP deployment credentials by using the following steps:</span></span>

   <span data-ttu-id="deef0-151">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="deef0-151">a.</span></span> <span data-ttu-id="deef0-152">**[デプロイ資格情報]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-152">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="deef0-153">b.</span><span class="sxs-lookup"><span data-stu-id="deef0-153">b.</span></span> <span data-ttu-id="deef0-154">ユーザー名とパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="deef0-154">Specify your username and password.</span></span>

   <span data-ttu-id="deef0-155">c.</span><span class="sxs-lookup"><span data-stu-id="deef0-155">c.</span></span> <span data-ttu-id="deef0-156">**[Save]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-156">Click **Save**.</span></span>

   ![デプロイ資格情報を指定する][AZ08]

1. <span data-ttu-id="deef0-158">次の手順を使用して FTP 接続情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="deef0-158">Retrieve your FTP connection information by using the following steps:</span></span>

   <span data-ttu-id="deef0-159">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="deef0-159">a.</span></span> <span data-ttu-id="deef0-160">**[デプロイ資格情報]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="deef0-160">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="deef0-161">b.</span><span class="sxs-lookup"><span data-stu-id="deef0-161">b.</span></span> <span data-ttu-id="deef0-162">完全な FTP ユーザー名と URL をコピーし、それらをこのチュートリアルの次のセクションのために保存します。</span><span class="sxs-lookup"><span data-stu-id="deef0-162">Copy your full FTP username and URL and save them for the next section of this tutorial.</span></span>

   ![FTP URL と資格情報][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a><span data-ttu-id="deef0-164">Spring Boot Web アプリを Azure にデプロイする</span><span class="sxs-lookup"><span data-stu-id="deef0-164">Deploy your Spring Boot web app to Azure</span></span>

<span data-ttu-id="deef0-165">次の手順では、Spring Boot Web アプリを Azure にデプロイする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="deef0-165">The following steps will walk you through the steps to deploy your Spring Boot web app to Azure.</span></span>

1. <span data-ttu-id="deef0-166">Windows のメモ帳などのテキスト エディタを開き、次のテキストを新しいドキュメントに貼り付けてから、そのファイルを *web.config* として保存します。</span><span class="sxs-lookup"><span data-stu-id="deef0-166">Open a text editor such as Windows Notepad and paste the following text into a new document, then save the file as *web.config*:</span></span>
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. <span data-ttu-id="deef0-167">*web.config* ファイルをシステムに保存した後、このチュートリアルの前のセクションの URL、ユーザー名、およびパスワードを使用して FTP 経由で Web アプリに接続します。</span><span class="sxs-lookup"><span data-stu-id="deef0-167">After you have saved the *web.config* file to your system, connect to your web app via FTP using the URL, username, and password from the preceding section of this tutorial.</span></span> <span data-ttu-id="deef0-168">例: </span><span class="sxs-lookup"><span data-stu-id="deef0-168">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="deef0-169">リモート ディレクトリを Web アプリのルート フォルダー (*/site/wwwroot*) に変更してから、Spring Boot アプリケーションの JAR ファイルと前の *web.config* をコピーします。</span><span class="sxs-lookup"><span data-stu-id="deef0-169">Change the remote directory to the root folder of your web app, (which is at */site/wwwroot*), then copy the JAR file from your Spring Boot application and the *web.config* from earlier.</span></span> <span data-ttu-id="deef0-170">例: </span><span class="sxs-lookup"><span data-stu-id="deef0-170">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="deef0-171">JAR および *web.config* ファイルを Web アプリにデプロイした後、Azure Portal を使用して Web アプリを再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="deef0-171">After you have deployed your JAR and *web.config* files to your web app, you need to restart your web app using the Azure portal:</span></span>

   ![Web アプリを再起動する][AZ10]

1. <span data-ttu-id="deef0-173">Web ブラウザーを使用して Web アプリの URL を参照することによって Web アプリをテストするか、または curl が使用可能な場合は次の例のような構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="deef0-173">Test the web app by browsing to your web app's URL using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="deef0-174">**Greetings from Spring Boot! (Spring Boot からのあいさつ)** というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="deef0-174">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![サンプル アプリを見る][SB02]

## <a name="next-steps"></a><span data-ttu-id="deef0-176">次の手順</span><span class="sxs-lookup"><span data-stu-id="deef0-176">Next steps</span></span>

<span data-ttu-id="deef0-177">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="deef0-177">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="deef0-178">Azure Container Service で Spring Boot アプリケーションを Linux にデプロイする</span><span class="sxs-lookup"><span data-stu-id="deef0-178">Deploy a Spring Boot Application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

* [<span data-ttu-id="deef0-179">Azure Container Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする</span><span class="sxs-lookup"><span data-stu-id="deef0-179">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="deef0-180">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="deef0-180">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="deef0-181">FTP を使用した Azure への Web アプリのデプロイの詳細については、「[FTP/S を使用した Azure App Service へのアプリのデプロイ]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="deef0-181">For additional information about depoying web apps to Azure using FTP, see [Deploy your app to Azure App Service using FTP/S].</span></span>

<span data-ttu-id="deef0-182">Spring Boot サンプル プロジェクトの詳細については、「[Spring Boot Getting Started]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="deef0-182">For further details about the Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="deef0-183">独自の Spring Boot アプリケーションの使用開始に関するヘルプについては、「**Spring Initializr**」(https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="deef0-183">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="deef0-184">Web アプリの追加設定の構成の詳細については、「[Configure web apps in Azure App Service (Azure App Service で Web アプリを構成する)]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="deef0-184">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure Portal]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Configure web apps in Azure App Service (Azure App Service で Web アプリを構成する)]: /azure/app-service/web-sites-configure
[Configure web apps in Azure App Service]: /azure/app-service/web-sites-configure
[FTP/S を使用した Azure App Service へのアプリのデプロイ]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[Deploy your app to Azure App Service using FTP/S]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-web-app-on-azure/SB01.png
[SB02]: ./media/deploy-spring-boot-java-web-app-on-azure/SB02.png

[AZ01]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ01.png
[AZ02]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ02.png
[AZ03]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ03.png
[AZ04]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ04.png
[AZ05]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ05.png
[AZ06]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ06.png
[AZ07]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ07.png
[AZ08]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ08.png
[AZ09]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ09.png
[AZ10]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ10.png
