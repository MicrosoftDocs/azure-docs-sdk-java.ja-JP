---
title: Azure Redis Cache を使用するように Spring Boot Initializer アプリを構成する
description: Spring Initializer で作成された Spring Boot アプリケーションを、Azure Redis Cache によってクラウドで Redis を使用するように構成します。
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: cache
ms.tgt_pltfrm: cache-redis
ms.topic: article
ms.workload: na
ms.openlocfilehash: 4b720cf4639a12c6dd8cc5040107c1b52de6f642
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991406"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-redis-in-the-cloud-with-azure-redis-cache"></a><span data-ttu-id="77d7f-103">Azure Redis Cache によってクラウドで Redis を使用するように Spring Boot Initializer アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="77d7f-103">Configure a Spring Boot Initializer app to use Redis in the cloud with Azure Redis Cache</span></span>

<span data-ttu-id="77d7f-104">この記事では、Azure Portal を使用したクラウドでの Redis Cache の作成、**[Spring Initializr]** を使用したカスタム アプリケーションの作成、Redis Cache を使用してデータを保存および取得する Java Web アプリケーションの作成について説明します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-104">This article walks you through creating a Redis cache in the cloud using the Azure portal, then using the **[Spring Initializr]** to create a custom application, and then creating a Java web application that stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77d7f-105">前提条件</span><span class="sxs-lookup"><span data-stu-id="77d7f-105">Prerequisites</span></span>

<span data-ttu-id="77d7f-106">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="77d7f-106">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="77d7f-107">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="77d7f-108">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="77d7f-108">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="77d7f-109">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="77d7f-109">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="77d7f-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="77d7f-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="77d7f-111">Spring Initializr を使用してカスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="77d7f-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="77d7f-112"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="77d7f-113">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="77d7f-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="77d7f-115">Spring Initializr では、**[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** の名前を使用してパッケージ名を作成します (例: *com.contoso.myazuredemo*)。</span><span class="sxs-lookup"><span data-stu-id="77d7f-115">The Spring Initializr will use the **Group** and **Aritifact** names to create the package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="77d7f-116">下にスクロールして **[Web]** セクションに移動し、**[Web]** チェック ボックスをオンにし、下にスクロールして **[NoSQL]** セクションに移動し、**[Redis]** チェック ボックスをオンにします。その後、ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="77d7f-116">Scroll down to the **Web** section and check the box for **Web**, then scroll down to the **NoSQL** section and check the box for **Redis**, then scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![すべての Spring Initializr オプション][SI02]

1. <span data-ttu-id="77d7f-118">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="77d7f-118">When prompted, download the project to a path on your local computer.</span></span>

   ![カスタム Spring Boot プロジェクトのダウンロード][SI03]

1. <span data-ttu-id="77d7f-120">ファイルをローカル システム上に展開したら、カスタム Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-120">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![カスタム Spring Boot プロジェクト ファイル][SI04]

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="77d7f-122">Azure で Redis Cache を作成する</span><span class="sxs-lookup"><span data-stu-id="77d7f-122">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="77d7f-123"><https://portal.azure.com/> で Azure Portal を参照し、**[+新規]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="77d7f-123">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure ポータル][AZ01]

1. <span data-ttu-id="77d7f-125">**[データベース]** をクリックし、**[Redis Cache]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="77d7f-125">Click **Database**, and then click **Redis Cache**.</span></span>

   ![Azure ポータル][AZ02]

1. <span data-ttu-id="77d7f-127">**[新規 Redis Cache]** ページで、以下の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-127">On the **New Redis Cache** page, specify the following information:</span></span>

   * <span data-ttu-id="77d7f-128">キャッシュの **[DNS 名]** を入力します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-128">Enter the **DNS name** for your cache.</span></span>
   * <span data-ttu-id="77d7f-129">**[サブスクリプション]**、**[リソース グループ]**、**[場所]**、および **[価格レベル]** を指定します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-129">Specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span>
   * <span data-ttu-id="77d7f-130">このチュートリアルでは、**[ポート 6379 のブロックを解除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-130">For this tutorial, choose **Unblock port 6379**.</span></span>

   > [!NOTE]
   >
   > <span data-ttu-id="77d7f-131">Redis キャッシュで SSL を使用できますが、Jedis のような異なる Redis クライアントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="77d7f-131">You can use SSL with Redis caches, but you would need to use a different Redis client like Jedis.</span></span> <span data-ttu-id="77d7f-132">詳細については、「[Java で Azure Redis Cache を使用する方法][Redis Cache with Java]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="77d7f-132">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>
   >

   <span data-ttu-id="77d7f-133">これらのオプションの指定後、**[作成]** をクリックしてキャッシュを作成します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-133">When you have specified these options, click **Create** to create your cache.</span></span>

   ![Azure ポータル][AZ03]

1. <span data-ttu-id="77d7f-135">キャッシュが作成されると、Azure の**ダッシュボード**のほか、**[すべてのリソース]** ブレードと **[Redis Cach]** ページにも作成したキャッシュが表示されます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-135">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="77d7f-136">これらのいずれかの場所でキャッシュをクリックすると、そのキャッシュのプロパティ ページを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-136">You can click on your cache on any of those locations to open the properties page for your cache.</span></span>

   ![Azure ポータル][AZ04]

1. <span data-ttu-id="77d7f-138">キャッシュのプロパティの一覧が含まれているページが表示されたら、**[アクセス キー]** をクリックし、キャッシュのアクセス キーをコピーします。</span><span class="sxs-lookup"><span data-stu-id="77d7f-138">When the page that contains the list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![Azure ポータル][AZ05]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a><span data-ttu-id="77d7f-140">Redis Cache を使用するようにカスタム Spring Boot を構成する</span><span class="sxs-lookup"><span data-stu-id="77d7f-140">Configure your custom Spring Boot to use your Redis Cache</span></span>

1. <span data-ttu-id="77d7f-141">アプリの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-141">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![application.properties ファイルを探す][RE01]

1. <span data-ttu-id="77d7f-143">テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値を実際のキャッシュの適切なプロパティに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-143">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties from your cache:</span></span>

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![application.properties ファイルの編集][RE02]

   > [!NOTE] 
   > 
   > <span data-ttu-id="77d7f-145">SSL を有効にする Jedis のような異なる Redis クライアントを使用している場合は、SSL 使用する旨を *application.properties* ファイルで指定し、ポート 6380 を使用します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-145">If you were using a different Redis client like Jedis that enables SSL, you would specify that you want to use SSL in your *application.properties* file and use port 6380.</span></span> <span data-ttu-id="77d7f-146">例: </span><span class="sxs-lookup"><span data-stu-id="77d7f-146">For example:</span></span>
   > 
   > ```yaml
   > # Specify the DNS URI of your Redis cache.
   > spring.redis.host=myspringbootcache.redis.cache.windows.net
   > # Specify the access key for your Redis cache.
   > spring.redis.password=57686f6120447564652c2049495320526f636b73=
   > # Specify that you want to use SSL.
   > spring.redis.ssl=true
   > # Specify the SSL port for your Redis cache.
   > spring.redis.port=6380
   > ```
   > 
   > <span data-ttu-id="77d7f-147">詳細については、「[Java で Azure Redis Cache を使用する方法][Redis Cache with Java]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="77d7f-147">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span> 
   > 

1. <span data-ttu-id="77d7f-148">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-148">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="77d7f-149">パッケージのソース フォルダーの下に *controller* という名前のフォルダーを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-149">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="77d7f-150">または</span><span class="sxs-lookup"><span data-stu-id="77d7f-150">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="77d7f-151">*HelloController.java* という名前の新しいファイルを *controller* フォルダー内に作成します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-151">Create a new file named *HelloController.java* in the *controller* folder.</span></span> <span data-ttu-id="77d7f-152">テキスト エディターでそのファイルを開き、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-152">Open the file in a text editor and add the following code to it:</span></span>

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   <span data-ttu-id="77d7f-153">ここでは、`com.contoso.myazuredemo` を実際のプロジェクトのパッケージ名に置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="77d7f-153">Where you will need to replace `com.contoso.myazuredemo` with the package name for your project.</span></span>

1. <span data-ttu-id="77d7f-154">*HelloController.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-154">Save and close the *HelloController.java* file.</span></span>

1. <span data-ttu-id="77d7f-155">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-155">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="77d7f-156">Web ブラウザーを使用して http://localhost:8080 を参照することによって Web アプリをテストするか、または curl が使用可能な場合は次の例のような構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-156">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="77d7f-157">"Hello World!" </span><span class="sxs-lookup"><span data-stu-id="77d7f-157">You should see the "Hello World!"</span></span> <span data-ttu-id="77d7f-158">というメッセージがサンプル コントローラーから表示されることがわかります。これは、Redis cache から動的に取得されます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-158">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77d7f-159">次の手順</span><span class="sxs-lookup"><span data-stu-id="77d7f-159">Next steps</span></span>

<span data-ttu-id="77d7f-160">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="77d7f-160">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="77d7f-161">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="77d7f-161">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="77d7f-162">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="77d7f-162">Additional Resources</span></span>

<span data-ttu-id="77d7f-163">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="77d7f-163">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="77d7f-164">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="77d7f-164">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="77d7f-165">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="77d7f-165">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="77d7f-166">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="77d7f-166">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="77d7f-167">Azure 上の Java での Redis Cache の使用開始の詳細については、「[Java で Azure Redis Cache を使用する方法][Redis Cache with Java]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="77d7f-167">For more information about getting started using Redis Cache with Java on Azure, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>

<span data-ttu-id="77d7f-168">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="77d7f-168">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="77d7f-169">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="77d7f-169">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="77d7f-170">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="77d7f-170">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="77d7f-171">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="77d7f-171">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Redis Cache with Java]: /azure/redis-cache/cache-java-get-started

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ01.png
[AZ02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ02.png
[AZ03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ03.png
[AZ04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ04.png
[AZ05]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ05.png

[SI01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI01.png
[SI02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI02.png
[SI03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI03.png
[SI04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI04.png

[RE01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE01.png
[RE02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE02.png
