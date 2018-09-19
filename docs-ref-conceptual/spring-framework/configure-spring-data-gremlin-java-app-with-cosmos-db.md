---
title: Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法
description: Spring Boot Initializer で作成されたアプリケーションを Azure Cosmos DB SQL API で構成する方法について説明します。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.date: 08/20/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 4e0138e3cc474b4c47d3bf492e696ec49ea3ef37
ms.sourcegitcommit: c2019ba6da6c7c28b17b5a85f89e49bb5e570ba4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040270"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="7c804-103">Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法</span><span class="sxs-lookup"><span data-stu-id="7c804-103">How to use the Spring Data Gremlin Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="7c804-104">概要</span><span class="sxs-lookup"><span data-stu-id="7c804-104">Overview</span></span>

<span data-ttu-id="7c804-105">Spring Data Gremlin Starter は、Apache の Gremlin クエリ言語に Spring Data のサポートを提供します。これは、開発者が任意の Gremlin 互換データ ストアで使用できます。</span><span class="sxs-lookup"><span data-stu-id="7c804-105">The Spring Data Gremlin Starter provides Spring Data support for the Gremlin query language from Apache, which developers can use with any Gremlin-compatible data store.</span></span>

<span data-ttu-id="7c804-106">この記事では、Azure portal を使用して Gremlin API で使うための Azure Cosmos DB を作成する方法、**[Spring Initializr]** を使用してカスタム Java アプリケーションを作成する方法、カスタム アプリケーションに Spring Data Gremlin Starter 機能を追加し、Gremlin を使用して Azure Cosmos DB にデータを格納する方法、または Azure Cosmos DB からデータを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-106">This article demonstrates creating an Azure Cosmos DB by using the Azure portal for use with Gremlin API, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Data Gremlin Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using Gremlin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c804-107">前提条件</span><span class="sxs-lookup"><span data-stu-id="7c804-107">Prerequisites</span></span>

<span data-ttu-id="7c804-108">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="7c804-108">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="7c804-109">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="7c804-109">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="7c804-110">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="7c804-110">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="7c804-111">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="7c804-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="7c804-112">この記事の手順を完了するには、Spring Boot 2.0 以上のバージョンが必要です。</span><span class="sxs-lookup"><span data-stu-id="7c804-112">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a><span data-ttu-id="7c804-113">Azure portal を使用して Azure Cosmos DB を作成する</span><span class="sxs-lookup"><span data-stu-id="7c804-113">Create an Azure Cosmos DB using the Azure portal</span></span>

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a><span data-ttu-id="7c804-114">Gremlin API で使用するご自身の Azure Cosmos Database を作成する</span><span class="sxs-lookup"><span data-stu-id="7c804-114">Create your Azure Cosmos Database for use with Gremlin API</span></span>

1. <span data-ttu-id="7c804-115">Azure portal (<https://portal.azure.com/>) を参照し、**[+リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c804-115">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![リソースの作成][AZ01]

1. <span data-ttu-id="7c804-117">**[データベース]**、**[Azure Cosmos DB]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c804-117">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure Cosmos DB の作成][AZ02]

1. <span data-ttu-id="7c804-119">**[Azure Cosmos DB]** ページで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="7c804-119">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="7c804-120">データベースの Gremlin URI の一部として使用する一意の **ID** を入力します。</span><span class="sxs-lookup"><span data-stu-id="7c804-120">Enter a unique **ID**, which you will use as part of the Gremlin URI for your database.</span></span> <span data-ttu-id="7c804-121">たとえば、**ID** として **wingtiptoysdata** を入力した場合、Gremlin URI は *wingtiptoysdata.gremlin.cosmosdb.azure.com* になります。</span><span class="sxs-lookup"><span data-stu-id="7c804-121">For example: if you entered **wingtiptoysdata** for the **ID**, the Gremlin URI would be *wingtiptoysdata.gremlin.cosmosdb.azure.com*.</span></span>
   * <span data-ttu-id="7c804-122">API として **[Gremlin (グラフ)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c804-122">Choose **Gremlin (Graph)** for the API.</span></span>
   * <span data-ttu-id="7c804-123">データベースに使用する**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c804-123">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="7c804-124">データベースに対して新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="7c804-124">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="7c804-125">データベースの**場所**を指定します。</span><span class="sxs-lookup"><span data-stu-id="7c804-125">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="7c804-126">これらのオプションの指定後、**[作成]** をクリックしてデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="7c804-126">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure Cosmos DB オプションの指定][AZ03]

1. <span data-ttu-id="7c804-128">データベースが作成されると、それが Azure **ダッシュボード**に表示され、**[すべてのリソース]** ページと **[Azure Cosmos DB]** ページにも表示されます。</span><span class="sxs-lookup"><span data-stu-id="7c804-128">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="7c804-129">これらのいずれかの場所でデータベースをクリックすると、キャッシュのプロパティ ページを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="7c804-129">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![すべてのリソース][AZ04]

1. <span data-ttu-id="7c804-131">データベースのプロパティ ページが表示されたら、**[アクセス キー]** をクリックし、データベースの URI とアクセス キーをコピーします。これらの値は Spring Boot アプリケーションで使用します。</span><span class="sxs-lookup"><span data-stu-id="7c804-131">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![[アクセス キー]][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a><span data-ttu-id="7c804-133">ご自身の Azure Cosmos Database にグラフを追加する</span><span class="sxs-lookup"><span data-stu-id="7c804-133">Add a graph to your Azure Cosmos Database</span></span>

1. <span data-ttu-id="7c804-134">**[データ エクスプローラー]** をクリックし、**[New Graph]\(新しいグラフ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c804-134">Click **Data Explorer**, and then click **New Graph**.</span></span>

   ![新しいグラフ][AZ06]

1. <span data-ttu-id="7c804-136">**[グラフの追加]** ページが表示されたら、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="7c804-136">When the **Add Graph** is displayed, enter the following information:</span></span>

   * <span data-ttu-id="7c804-137">お使いのデータベースの一意の**データベース ID** を指定します。</span><span class="sxs-lookup"><span data-stu-id="7c804-137">Specify a unique **Database id** for your database.</span></span>
   * <span data-ttu-id="7c804-138">お使いのグラフの一意の**グラフ ID** を指定します。</span><span class="sxs-lookup"><span data-stu-id="7c804-138">Specify a unique **Graph id** for your graph.</span></span>
   * <span data-ttu-id="7c804-139">**ストレージ容量**は、指定することも、既定値をそのまま使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="7c804-139">You can choose to specify your **Storage capacity**, or you can accept the default.</span></span>
   * <span data-ttu-id="7c804-140">**スループット**を指定します。この例では、400 要求ユニット (RU) を選択できます。</span><span class="sxs-lookup"><span data-stu-id="7c804-140">Specify your **Throughput**, and for this example you can choose 400 Request Units (RUs).</span></span>
   
   <span data-ttu-id="7c804-141">これらのオプションの指定後、**[OK]** をクリックして、グラフを作成します。</span><span class="sxs-lookup"><span data-stu-id="7c804-141">When you have specified these options, click **OK** to create your graph.</span></span>

   ![グラフの追加][AZ07]

1. <span data-ttu-id="7c804-143">目的のグラフが作成されたら、**データ エクスプローラー**を使用して、そのグラフを表示できます。</span><span class="sxs-lookup"><span data-stu-id="7c804-143">After your graph has been created, you can use the **Data Explorer** to view it.</span></span>

   ![グラフのプロパティの表示][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="7c804-145">Spring Initializr でシンプルな Spring Boot アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="7c804-145">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="7c804-146"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="7c804-146">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="7c804-147">**Java** で **Maven** プロジェクトを生成することを指定し、ご自身のアプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力します。次に、お使いの **Spring Boot** バージョン (2.0 以上) を指定し、**[プロジェクトの生成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c804-147">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, specify your **Spring Boot** version with a version that is equal to or greater than 2.0, and then click **Generate Project**.</span></span>

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="7c804-149">Spring Initializr では、**[グループ]** と **[アーティファクト]** の名前を使用してパッケージ名を作成します (例: \*com.example.wintiptoysdata)。</span><span class="sxs-lookup"><span data-stu-id="7c804-149">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: \*com.example.wintiptoysdata.</span></span>
   >

1. <span data-ttu-id="7c804-150">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="7c804-150">When prompted, download the project to a path on your local computer.</span></span>

   ![カスタム Spring Boot プロジェクトのダウンロード][SI02]

1. <span data-ttu-id="7c804-152">ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="7c804-152">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![カスタム Spring Boot プロジェクト ファイル][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a><span data-ttu-id="7c804-154">Spring Data Gremlin Starter を使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="7c804-154">Configure your Spring Boot app to use the Spring Data Gremlin Starter</span></span>

1. <span data-ttu-id="7c804-155">アプリのディレクトリで *pom.xml* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-155">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="7c804-156">または</span><span class="sxs-lookup"><span data-stu-id="7c804-156">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![pom.xml ファイルを探す][PM01]

1. <span data-ttu-id="7c804-158">テキスト エディターで *pom.xml* ファイルを開き、`<dependencies>` の一覧に Spring Data Gremlin Starter を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-158">Open the *pom.xml* file in a text editor, and add the Spring Data Gremlin Starter to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![pom.xml ファイルの編集][PM02]

1. <span data-ttu-id="7c804-160">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-160">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="7c804-161">Azure Cosmos DB を使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="7c804-161">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="7c804-162">ご自身のアプリの *resources* ディレクトリを見つけて、*application.yml* という名前の新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7c804-162">Locate the *resources* directory of your app, and create a new file named *application.yml*.</span></span> <span data-ttu-id="7c804-163">例: </span><span class="sxs-lookup"><span data-stu-id="7c804-163">For example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   <span data-ttu-id="7c804-164">または</span><span class="sxs-lookup"><span data-stu-id="7c804-164">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![application.yml ファイルの作成][RE01]

1.  <span data-ttu-id="7c804-166">テキスト エディターで *application.yml* ファイルを開き、そのファイルに次の行を追加して、サンプルの値をご自身のデータベース用の適切なプロパティで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7c804-166">Open the *application.yml* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   <span data-ttu-id="7c804-167">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="7c804-167">Where:</span></span>
   | <span data-ttu-id="7c804-168">フィールド</span><span class="sxs-lookup"><span data-stu-id="7c804-168">Field</span></span> | <span data-ttu-id="7c804-169">説明</span><span class="sxs-lookup"><span data-stu-id="7c804-169">Description</span></span> |
   | ---|---|
   | `endpoint` | <span data-ttu-id="7c804-170">ご自身のデータベースの Gremlin URI を指定します。これは、このチュートリアルで先ほど Azure Cosmos DB を作成したときに指定した一意の **ID** から派生します。</span><span class="sxs-lookup"><span data-stu-id="7c804-170">Specifies the Gremlin URI for your database, which is derived from the unique **ID** that you specified when you created your Azure Cosmos DB earlier in this tutorial.</span></span> |
   | `port` | <span data-ttu-id="7c804-171">TCP/IP ポートを指定します。HTTPS 用の **443** を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7c804-171">Specifies the TCP/IP port, which should be **443** for HTTPS.</span></span> |
   | `username` | <span data-ttu-id="7c804-172">このチュートリアルで先ほどグラフを追加したときに使用した一意の**データベース ID** と**グラフ ID** を指定します。"/dbs/**{Database id}**/colls/**{Graph id}**" という構文を使用して入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7c804-172">Specifies the unique **Database id** and **Graph id** that you used when you added your graph earlier in this tutorial; this must be entered using the following syntax: "/dbs/**{Database id}**/colls/**{Graph id}**".</span></span> |
   | `password` | <span data-ttu-id="7c804-173">このチュートリアルで先ほどコピーしたプライマリまたはセカンダリの**アクセス キー**を指定します。</span><span class="sxs-lookup"><span data-stu-id="7c804-173">Specifies either the primary or secondary **Access key** that you copied earlier in this tutorial.</span></span> |
   | `telemetryAllowed` | <span data-ttu-id="7c804-174">利用統計情報を有効にする場合は **true**、それ以外の場合は **false** を指定します。</span><span class="sxs-lookup"><span data-stu-id="7c804-174">Specify **true** if you want to enable telemetry; otherwise, **false**.</span></span>

1. <span data-ttu-id="7c804-175">*application.yml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-175">Save and close the *application.yml* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="7c804-176">基本的なデータベース機能を実装するサンプル コードを追加する</span><span class="sxs-lookup"><span data-stu-id="7c804-176">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="7c804-177">このセクションでは、ご自身のデータベースにデータを格納するために必要な Java クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7c804-177">In this section, you create the necessary Java classes for storing data in your database.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="7c804-178">アプリケーションのメイン クラスを変更する</span><span class="sxs-lookup"><span data-stu-id="7c804-178">Modify the main application class</span></span>

1. <span data-ttu-id="7c804-179">アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-179">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="7c804-180">または</span><span class="sxs-lookup"><span data-stu-id="7c804-180">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![アプリケーションの Java ファイルを探す][JV01]

1. <span data-ttu-id="7c804-182">テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-182">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoysdata;
   
   // These imports are required for the application.
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.example.wingtiptoysdata.domain.Network;
   import com.example.wingtiptoysdata.domain.Person;
   import com.example.wingtiptoysdata.domain.Relation;
   import com.example.wingtiptoysdata.repository.NetworkRepository;
   import com.example.wingtiptoysdata.repository.PersonRepository;
   import com.example.wingtiptoysdata.repository.RelationRepository;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import javax.annotation.PostConstruct;
   import javax.annotation.PreDestroy;
   
   @SpringBootApplication
   public class WingtiptoysdataApplication {
   
       // Define several person classes to store personal data.
       private final Person person1 = new Person("01", "Nellie Hughes", "23");
       private final Person person2 = new Person("02", "Delmar Alfred", "34");
       private final Person person3 = new Person("03", "Kelley Hunter", "45");
       private final Person person4 = new Person("04", "Roscoe Guerin", "56");
       private final Person person5 = new Person("05", "Gracie Chavez", "67");
   
       // Define relationship classes to define the relationships between some of the persons.
       private final Relation relation1 = new Relation("0102", "siblings", person1, person2);
       private final Relation relation2 = new Relation("0203", "coworkers", person2, person3);
       private final Relation relation3 = new Relation("0301", "parent", person3, person1);
       private final Relation relation4 = new Relation("0302", "parent", person3, person2);
       private final Relation relation5 = new Relation("0501", "grandparent", person5, person1);
       private final Relation relation6 = new Relation("0502", "grandparent", person5, person2);
   
       // Define the network.
       private final Network network = new Network();
   
       // Autowire the repositories and factory.
       @Autowired
       private PersonRepository personRepo;
       @Autowired
       private RelationRepository relationRepo;
       @Autowired
       private NetworkRepository networkRepo;
       @Autowired
       private GremlinFactory factory;
   
       // Run the Spring application and exit.
    public static void main(String[] args) {
           SpringApplication.run(WingtiptoysdataApplication.class, args);
           System.exit(0);
    }
   
       // Perform post-construct operations.    
       @PostConstruct
       public void setup() {
           // Delete any existing data from the database.
           this.networkRepo.deleteAll();
   
           // Add the relationship classes as edges.
           this.network.getEdges().add(this.relation1);
           this.network.getEdges().add(this.relation2);
           this.network.getEdges().add(this.relation3);
           this.network.getEdges().add(this.relation4);
           this.network.getEdges().add(this.relation5);
           this.network.getEdges().add(this.relation6);
   
           // Add the person classes as vertices.
           this.network.getVertexes().add(this.person1);
           this.network.getVertexes().add(this.person2);
           this.network.getVertexes().add(this.person3);
           this.network.getVertexes().add(this.person4);
           this.network.getVertexes().add(this.person5);
   
           // Save the network.
           this.networkRepo.save(this.network);
       }
   }
   ```

1. <span data-ttu-id="7c804-183">メイン アプリケーションの Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-183">Save and close the main application Java file.</span></span>

### <a name="define-a-basic-class-for-storing-configuration-information"></a><span data-ttu-id="7c804-184">構成情報を格納するための基本クラスを定義する</span><span class="sxs-lookup"><span data-stu-id="7c804-184">Define a basic class for storing configuration information</span></span>

1. <span data-ttu-id="7c804-185">ご自身のアプリのパッケージ ディレクトリに *config* という名前のフォルダーを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-185">Create a folder named *config* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   <span data-ttu-id="7c804-186">または</span><span class="sxs-lookup"><span data-stu-id="7c804-186">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. <span data-ttu-id="7c804-187">*config* ディレクトリに *UserRepositoryConfiguration.java* という名前の新しい Java ファイルを作成し、テキスト エディターでそのファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-187">Create a new Java file named *UserRepositoryConfiguration.java* in the *config* directory, then open the file in a text editor, and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.config;

   import com.microsoft.spring.data.gremlin.common.GremlinConfiguration;
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.microsoft.spring.data.gremlin.config.AbstractGremlinConfiguration;
   import com.microsoft.spring.data.gremlin.repository.config.EnableGremlinRepositories;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   
   @Configuration
   @EnableGremlinRepositories(basePackages = "com.example.wingtiptoysdata.repository")
   @EnableConfigurationProperties(GremlinConfiguration.class)
   @PropertySource("classpath:application.yml")
   public class UserRepositoryConfiguration extends AbstractGremlinConfiguration {
   
       @Autowired
       private GremlinConfiguration config;
   
       @Override
       public GremlinConfiguration getGremlinConfiguration() {
           return this.config;
       }
   }
   ```

1. <span data-ttu-id="7c804-188">*UserRepositoryConfiguration.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-188">Save and close the *UserRepositoryConfiguration.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a><span data-ttu-id="7c804-189">グラフ データベースの要素を定義するクラスのセットを定義する</span><span class="sxs-lookup"><span data-stu-id="7c804-189">Define a set of classes that define the elements of your graph database</span></span>

1. <span data-ttu-id="7c804-190">ご自身のアプリのパッケージ ディレクトリに *domain* という名前のフォルダーを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-190">Create a folder named *domain* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   <span data-ttu-id="7c804-191">または</span><span class="sxs-lookup"><span data-stu-id="7c804-191">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. <span data-ttu-id="7c804-192">*domain* ディレクトリに *Person.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-192">Create a new Java file named *Person.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Vertex;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Vertex
   @AllArgsConstructor
   @NoArgsConstructor
   public class Person {
   
       @Id
       private String id;
   
       private String name;
   
       private String age;
   }
   ```

1. <span data-ttu-id="7c804-193">*Person.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-193">Save and close the *Person.java* file.</span></span>

1. <span data-ttu-id="7c804-194">*domain* ディレクトリに *Relation.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-194">Create a new Java file named *Relation.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Edge;
   import com.microsoft.spring.data.gremlin.annotation.EdgeFrom;
   import com.microsoft.spring.data.gremlin.annotation.EdgeTo;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Edge
   @AllArgsConstructor
   @NoArgsConstructor
   public class Relation {
   
       @Id
       private String id;
   
       private String name;
   
       @EdgeFrom
       private Person personFrom;
   
       @EdgeTo
       private Person personTo;
   }
   ```

1. <span data-ttu-id="7c804-195">*Relation.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-195">Save and close the *Relation.java* file.</span></span>

1. <span data-ttu-id="7c804-196">*domain* ディレクトリに *Network.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-196">Create a new Java file named *Network.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.EdgeSet;
   import com.microsoft.spring.data.gremlin.annotation.Graph;
   import com.microsoft.spring.data.gremlin.annotation.VertexSet;
   import lombok.Getter;
   import org.springframework.data.annotation.Id;
   
   import java.util.ArrayList;
   import java.util.List;
   
   @Graph
   public class Network {
   
       @Id
       private String id;
   
       public Network() {
           this.edges = new ArrayList<Object>();
           this.vertexes = new ArrayList<Object>();
       }
   
       @EdgeSet
       @Getter
       private List<Object> edges;
   
       @VertexSet
       @Getter
       private List<Object> vertexes;
   }
   ```

1. <span data-ttu-id="7c804-197">*Network.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-197">Save and close the *Network.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a><span data-ttu-id="7c804-198">グラフ データベース用のリポジトリを定義するクラスのセットを定義する</span><span class="sxs-lookup"><span data-stu-id="7c804-198">Define a set of classes that define the repositories for your graph database</span></span>

1. <span data-ttu-id="7c804-199">ご自身のアプリのパッケージ ディレクトリに *repository* という名前のフォルダーを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-199">Create a folder named *repository* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   <span data-ttu-id="7c804-200">または</span><span class="sxs-lookup"><span data-stu-id="7c804-200">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. <span data-ttu-id="7c804-201">*repository* ディレクトリに *NetworkRepository.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-201">Create a new Java file named *NetworkRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. <span data-ttu-id="7c804-202">*NetworkRepository.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-202">Save and close the *NetworkRepository.java* file.</span></span>

1. <span data-ttu-id="7c804-203">*repository* ディレクトリに *PersonRepository.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-203">Create a new Java file named *PersonRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. <span data-ttu-id="7c804-204">*PersonRepository.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-204">Save and close the *PersonRepository.java* file.</span></span>

1. <span data-ttu-id="7c804-205">*repository* ディレクトリに *RelationRepository.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c804-205">Create a new Java file named *RelationRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. <span data-ttu-id="7c804-206">*RelationRepository.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7c804-206">Save and close the *RelationRepository.java* file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="7c804-207">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="7c804-207">Build and test your app</span></span>

1. <span data-ttu-id="7c804-208">コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-208">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="7c804-209">または</span><span class="sxs-lookup"><span data-stu-id="7c804-209">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="7c804-210">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c804-210">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="7c804-211">ご自身のアプリケーションにランタイム メッセージがいくつか表示されます。エラーがない場合は、Azure portal を使用して、Azure Cosmos DB の内容を表示できます。</span><span class="sxs-lookup"><span data-stu-id="7c804-211">Your application will display several runtime messages, and if there were no errors, you can use the Azure portal to view the contents of your Azure Cosmos DB.</span></span> <span data-ttu-id="7c804-212">これを行うには、お使いのデータベースのプロパティ ページで **[データ エクスプローラー]** をクリックし、**[Execute Gremlin Query]\(Gremlin クエリの実行\)** をクリックして、データを表示する結果の一覧から項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c804-212">To do so, click **Data Explorer** on the properties page for your database, then click **Execute Gremlin Query**, and then select an item from the list of results to view the data.</span></span>

   ![ドキュメント エクスプローラーを使用してデータを表示する][JV03]

## <a name="next-steps"></a><span data-ttu-id="7c804-214">次の手順</span><span class="sxs-lookup"><span data-stu-id="7c804-214">Next steps</span></span>

<span data-ttu-id="7c804-215">Gremlin および Graph API に対する Azure サポートの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7c804-215">For more information about Azure support for Gremlin and Graph API, see the following articles:</span></span>

* [<span data-ttu-id="7c804-216">Azure Cosmos DB の概要: Graph API</span><span class="sxs-lookup"><span data-stu-id="7c804-216">Introduction to Azure Cosmos DB: Graph API</span></span>](https://docs.microsoft.com/azure/cosmos-db/graph-introduction)

* [<span data-ttu-id="7c804-217">Azure Cosmos DB での Gremlin グラフのサポート</span><span class="sxs-lookup"><span data-stu-id="7c804-217">Azure Cosmos DB Gremlin graph support</span></span>](https://docs.microsoft.com/azure/cosmos-db/gremlin-support)

* [<span data-ttu-id="7c804-218">Azure Cosmos DB: グラフ データベースを Java と Azure portal で作成する</span><span class="sxs-lookup"><span data-stu-id="7c804-218">Azure Cosmos DB: Create a graph database using Java and the Azure portal</span></span>](https://docs.microsoft.com/azure/cosmos-db/create-graph-java)

* [<span data-ttu-id="7c804-219">チュートリアル: Gremlin を使って Azure Cosmos DB Graph API を照会する</span><span class="sxs-lookup"><span data-stu-id="7c804-219">Tutorial: Query Azure Cosmos DB Graph API by using Gremlin</span></span>](https://docs.microsoft.com/azure/cosmos-db/tutorial-query-graph)

* <span data-ttu-id="7c804-220">[Spring Data Gremlin Starter]</span><span class="sxs-lookup"><span data-stu-id="7c804-220">[Spring Data Gremlin Starter]</span></span>

<span data-ttu-id="7c804-221">Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7c804-221">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="7c804-222">[Azure Cosmos DB のドキュメント]。</span><span class="sxs-lookup"><span data-stu-id="7c804-222">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="7c804-223">[Azure Cosmos DB: ドキュメント データベースを Java と Azure Portal で作成する][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="7c804-223">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="7c804-224">[Azure Cosmos DB SQL API の Spring Data]</span><span class="sxs-lookup"><span data-stu-id="7c804-224">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="7c804-225">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7c804-225">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="7c804-226">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="7c804-226">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="7c804-227">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="7c804-227">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="7c804-228">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7c804-228">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="7c804-229">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="7c804-229">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="7c804-230">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="7c804-230">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="7c804-231">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="7c804-231">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="7c804-232">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="7c804-232">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>


<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[Azure Cosmos DB SQL API の Spring Data]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ05.png
[AZ06]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ06.png
[AZ07]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ07.png
[AZ08]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ08.png

[SI01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV03.png
