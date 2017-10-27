---
title: "Redis Cache を使用するように Spring Boot Initializer アプリを構成する方法"
description: "Spring Initializer で作成された Spring Boot アプリケーションを、Azure Redis Cache を使用するように構成する方法について説明します。"
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Spring Starter, Redis Cache
ms.assetid: 
ms.service: cache
ms.workload: na
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 10/11/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: ce8202b48c6759a80560616492eab018434e9307
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2017
---
# <a name="how-to-configure-a-spring-boot-initializer-app-to-use-redis-cache"></a>Redis Cache を使用するように Spring Boot Initializer アプリを構成する方法

## <a name="overview"></a>概要

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

この記事では、Azure Portal を使用した Redis キャッシュの作成と、カスタム アプリケーションを作成するための **Spring Initializr** の使用、さらに Redis キャッシュを使用してデータを保管および取得する Java Web アプリケーションの作成について説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。

* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。

* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-a-redis-cache-on-azure"></a>Azure で Redis Cache を作成する

1. <https://portal.azure.com/> で Azure Portal を開き、**[+ 新規]** の項目をクリックします。

   ![Azure Portal][AZ01]

1. **[データベース]** をクリックし、**[Redis Cache]** をクリックします。

   ![Azure ポータル][AZ02]

1. **[新規 Redis Cache]** ページで、以下の情報を指定します。

   * キャッシュの **[DNS 名]** を入力します。
   * **[サブスクリプション]**、**[リソース グループ]**、**[場所]**、および **[価格レベル]** を指定します。
   * このチュートリアルでは、**[ポート 6379 のブロックを解除]** を選択します。

   > [!NOTE]
   >
   > Redis キャッシュで SSL を使用できますが、Jedis のような異なる Redis クライアントを使用する必要があります。 詳細については、「[Java で Azure Redis Cache を使用する方法][Redis Cache with Java]」をご覧ください。
   >

   これらのオプションの指定後、**[作成]** をクリックしてキャッシュを作成します。

   ![Azure ポータル][AZ03]

1. キャッシュが作成されると、Azure の**ダッシュボード**のほか、**[すべてのリソース]** ブレードと **[Redis Cach]** ページにも作成したキャッシュが表示されます。 これらのいずれかの場所でキャッシュをクリックすると、そのキャッシュのプロパティ ページを開くことができます。

   ![Azure ポータル][AZ04]

1. キャッシュのプロパティの一覧が含まれているページが表示されたら、**[アクセス キー]** をクリックし、キャッシュのアクセス キーをコピーします。

   ![Azure ポータル][AZ05]

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Spring Initializr を使用してカスタム アプリケーションを作成する

1. <https://start.spring.io/> に移動します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > Spring Initializr では、**[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** の名前を使用してパッケージ名を作成します (例: *com.contoso.myazuredemo*)。
   >

1. 下にスクロールして **[Web]** セクションに移動し、**[Web]** チェック ボックスをオンにし、下にスクロールして **[NoSQL]** セクションに移動し、**[Redis]** チェック ボックスをオンにします。その後、ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** ボタンをクリックします。

   ![すべての Spring Initializr オプション][SI02]

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

   ![カスタム Spring Boot プロジェクトのダウンロード][SI03]

1. ファイルをローカル システム上に展開したら、カスタム Spring Boot アプリケーションの編集を開始できます。

   ![カスタム Spring Boot プロジェクト ファイル][SI04]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a>Redis Cache を使用するようにカスタム Spring Boot を構成する

1. アプリの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。

   ![application.properties ファイルを探す][RE01]

1. テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値を実際のキャッシュの適切なプロパティに置き換えます。

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
   > SSL を有効にする Jedis のような異なる Redis クライアントを使用している場合は、*application.properties* ファイルでポート 6380 を指定します。 詳細については、「[Java で Azure Redis Cache を使用する方法][Redis Cache with Java]」をご覧ください。
   >

1. *application.properties* ファイルを保存して閉じます。

1. パッケージのソース フォルダーの下に *controller* という名前のフォルダーを作成します。次に例を示します。

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   または

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. *HelloController.java* という名前の新しいファイルを *controller* フォルダー内に作成します。 テキスト エディターでそのファイルを開き、次の行を追加します。

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
   
   ここでは、`com.contoso.myazuredemo` を実際のプロジェクトのパッケージ名に置き換える必要があります。

1. *HelloController.java* ファイルを保存して閉じます。

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Web ブラウザーを使用して http://localhost:8080 を参照することによって Web アプリをテストするか、または curl が使用可能な場合は次の例のような構文を使用します。

   ```shell
   curl http://localhost:8080
   ```

   "Hello World!"  というメッセージがサンプル コントローラーから表示されることがわかります。これは、Redis cache から動的に取得されます。

## <a name="next-steps"></a>次のステップ

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、 [Azure Java デベロッパー センター] と[Java Tools for Visual Studio Team Services] を参照してください。

Azure 上の Java での Redis Cache の使用開始の詳細については、「[Java で Azure Redis Cache を使用する方法][Redis Cache with Java]」を参照してください。

<!-- URL List -->

[Azure Java デベロッパー センター]: https://azure.microsoft.com/develop/java/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
