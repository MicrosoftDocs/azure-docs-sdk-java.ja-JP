---
title: Azure Cosmos DB SQL API で Spring Boot Starter を使用する方法
description: Spring Boot Initializer で作成されたアプリケーションを Azure Cosmos DB SQL API で構成する方法について説明します。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/21/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 6675b3f76f19ec0bfdb28351681258b8c4792104
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339106"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="3ad09-103">Azure Cosmos DB SQL API で Spring Boot Starter を使用する方法</span><span class="sxs-lookup"><span data-stu-id="3ad09-103">How to use the Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="3ad09-104">概要</span><span class="sxs-lookup"><span data-stu-id="3ad09-104">Overview</span></span>

<span data-ttu-id="3ad09-105">Azure Cosmos DB は、開発者が SQL、MongoDB、Graph、Table API などのさまざまな標準 API を使用してデータを操作できるようにするグローバル分散型データベース サービスです。</span><span class="sxs-lookup"><span data-stu-id="3ad09-105">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as SQL, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="3ad09-106">Microsoft の Spring Boot Starter を使用すると、開発者は、SQL API を使用して Azure Cosmos DB と簡単に統合できる Spring Boot アプリケーションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-106">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using the SQL API.</span></span>

<span data-ttu-id="3ad09-107">この記事では、Azure Portal を使用して Azure Cosmos DB を作成する方法、**[Spring Initializr]** を使用してカスタム Java アプリケーションを作成する方法、カスタム アプリケーションに Spring Boot Starter 機能を追加し、SQL API を使用して Azure Cosmos DB にデータを格納またはデータを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-107">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the SQL API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ad09-108">前提条件</span><span class="sxs-lookup"><span data-stu-id="3ad09-108">Prerequisites</span></span>

<span data-ttu-id="3ad09-109">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="3ad09-109">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="3ad09-110">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="3ad09-111">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="3ad09-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="3ad09-112">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3ad09-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="3ad09-113">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="3ad09-113">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="3ad09-114">Azure Portal を使用して Azure Cosmos DB を作成する</span><span class="sxs-lookup"><span data-stu-id="3ad09-114">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="3ad09-115">Azure portal (<https://portal.azure.com/>) を参照し、**[+リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3ad09-115">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![Azure ポータル][AZ01]

1. <span data-ttu-id="3ad09-117">**[データベース]**、**[Azure Cosmos DB]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="3ad09-117">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure ポータル][AZ02]

1. <span data-ttu-id="3ad09-119">**[Azure Cosmos DB]** ページで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-119">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="3ad09-120">データベースの URI として使用する一意の **ID** を入力します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-120">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="3ad09-121">例: *wingtiptoysdata.documents.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="3ad09-121">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="3ad09-122">API の **[SQL]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-122">Choose **SQL** for the API.</span></span>
   * <span data-ttu-id="3ad09-123">データベースに使用する**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-123">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="3ad09-124">データベースに対して新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-124">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="3ad09-125">データベースの**場所**を指定します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-125">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="3ad09-126">これらのオプションの指定後、**[作成]** をクリックしてデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-126">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure ポータル][AZ03]

1. <span data-ttu-id="3ad09-128">データベースが作成されると、それが Azure **ダッシュボード**に表示され、**[すべてのリソース]** ページと **[Azure Cosmos DB]** ページにも表示されます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-128">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="3ad09-129">これらのいずれかの場所でデータベースをクリックすると、キャッシュのプロパティ ページを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-129">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure ポータル][AZ04]

1. <span data-ttu-id="3ad09-131">データベースのプロパティ ページが表示されたら、**[アクセス キー]** をクリックし、データベースの URI とアクセス キーをコピーします。これらの値は Spring Boot アプリケーションで使用します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-131">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure ポータル][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="3ad09-133">Spring Initializr でシンプルな Spring Boot アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="3ad09-133">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="3ad09-134"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-134">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="3ad09-135">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力します。**Spring Boot** のバージョンを指定し、**[プロジェクトの生成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3ad09-135">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, specify your **Spring Boot** version, and then click the button to **Generate Project**.</span></span>

   > [!IMPORTANT]
   >
   > <span data-ttu-id="3ad09-136">Spring Boot バージョン 2.0.n では API にいくつかの破壊的変更が加えられており、この記事の手順はこのバージョンを使用して実行します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-136">There were several breaking changes to the APIs in Spring Boot version 2.0.n, which will be used to complete the steps in this article.</span></span> <span data-ttu-id="3ad09-137">このチュートリアルの手順の実行には、現在も Spring Boot 1.5.n バージョンのいずれかを使用することもできます。違いについては、必要に応じて強調します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-137">You can still use one of the Spring Boot 1.5.n versions to complete the steps in this tutorial, and the differences will be highlighted when necessary.</span></span>
   >

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="3ad09-139">Spring Initializr では、**[グループ]** と **[アーティファクト]** の名前を使用してパッケージ名を作成します (例: *com.example.wintiptoysdata*)。</span><span class="sxs-lookup"><span data-stu-id="3ad09-139">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoysdata*.</span></span>
   >

1. <span data-ttu-id="3ad09-140">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="3ad09-140">When prompted, download the project to a path on your local computer.</span></span>

   ![カスタム Spring Boot プロジェクトのダウンロード][SI02]

1. <span data-ttu-id="3ad09-142">ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-142">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![カスタム Spring Boot プロジェクト ファイル][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="3ad09-144">Azure Spring Boot Starter を使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="3ad09-144">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="3ad09-145">アプリのディレクトリで *pom.xml* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-145">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="3ad09-146">または</span><span class="sxs-lookup"><span data-stu-id="3ad09-146">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![pom.xml ファイルを探す][PM01]

1. <span data-ttu-id="3ad09-148">テキスト エディターで *pom.xml* ファイルを開き、`<dependencies>` の一覧に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-148">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>2.0.4</version>
   </dependency>
   ```

   ![pom.xml ファイルの編集][PM02]

   > [!IMPORTANT]
   >
   > <span data-ttu-id="3ad09-150">このチュートリアルの実行に Spring Boot 1.5.n バージョンのいずれかを使用している場合は、以前のバージョンの Azure Cosmos DB のスターターを指定する必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-150">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to specify the older version of the Azure Cosmos DB starter; for example:</span></span>
   >
   > ```xml
   > <dependency>
   >   <groupId>com.microsoft.azure</groupId>
   >   <artifactId>azure-documentdb-spring-boot-starter</artifactId>
   >   <version>0.1.4</version>
   > </dependency>
   > ```

1. <span data-ttu-id="3ad09-151">Spring Boot のバージョンが、Spring Initializr でアプリケーションを作成したときに選択したバージョンであることを確認します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-151">Verify that the Spring Boot version is the version that you chose when you created your application with the Spring Initializr; for example:</span></span>

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.0.1.RELEASE</version>
      <relativePath/>
   </parent>
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="3ad09-152">このチュートリアルの実行に Spring Boot 1.5.n バージョンのいずれかを使用している場合は、正しいバージョン (例: `<version>1.5.14.RELEASE</version>`) であることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3ad09-152">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to verify the correct version; for example: `<version>1.5.14.RELEASE</version>`.</span></span>
   >

1. <span data-ttu-id="3ad09-153">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-153">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="3ad09-154">Azure Cosmos DB を使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="3ad09-154">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="3ad09-155">アプリの *resources* ディレクトリ内で *application.properties* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-155">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   <span data-ttu-id="3ad09-156">または</span><span class="sxs-lookup"><span data-stu-id="3ad09-156">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![application.properties ファイルを探す][RE01]

1. <span data-ttu-id="3ad09-158">テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値をデータベースの適切なプロパティに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-158">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![application.properties ファイルの編集][RE02]

1. <span data-ttu-id="3ad09-160">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-160">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="3ad09-161">基本的なデータベース機能を実装するサンプル コードを追加する</span><span class="sxs-lookup"><span data-stu-id="3ad09-161">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="3ad09-162">このセクションでは、ユーザー データを格納するための 2 つの Java クラスを作成します。その後、アプリケーションのメイン クラスを変更してユーザー クラスのインスタンスを作成し、それをデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-162">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="3ad09-163">ユーザー データを格納するための基本クラスを定義する</span><span class="sxs-lookup"><span data-stu-id="3ad09-163">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="3ad09-164">*User.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-164">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="3ad09-165">テキスト エディターで *User.java* ファイルを開き、次の行をファイルに追加して、データベースの値を格納および取得する汎用ユーザー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-165">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoysdata;

   // Define a generic User class.
   public class User {
      private String id;
      private String firstName;
      private String lastName;
   
      public User() {
      }
   
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }
   
      public void setId(String id) {
         this.id = id;
      }
   
      public String getFirstName() {
         return firstName;
      }
   
      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }
   
      public String getLastName() {
         return lastName;
      }
   
      public void setLastName(String lastName) {
         this.lastName = lastName;
      }
   
      @Override
      public String toString() {
         return String.format("User: %s %s %s", id, firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="3ad09-166">*User.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-166">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="3ad09-167">データ リポジトリ インターフェイスを定義する</span><span class="sxs-lookup"><span data-stu-id="3ad09-167">Define a data repository interface</span></span>

1. <span data-ttu-id="3ad09-168">*UserRepository.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-168">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="3ad09-169">テキスト エディターで *UserRepository.java* ファイルを開き、既定の DocumentDB リポジトリ インターフェイスを拡張するユーザー リポジトリ インターフェイスを定義する次の行をファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-169">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoysdata;
   
   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { } 
   ```

1. <span data-ttu-id="3ad09-170">*UserRepository.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-170">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="3ad09-171">アプリケーションのメイン クラスを変更する</span><span class="sxs-lookup"><span data-stu-id="3ad09-171">Modify the main application class</span></span>

1. <span data-ttu-id="3ad09-172">アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-172">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="3ad09-173">または</span><span class="sxs-lookup"><span data-stu-id="3ad09-173">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![アプリケーションの Java ファイルを探す][JV01]

1. <span data-ttu-id="3ad09-175">テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-175">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoysdata;

   // These imports are required for the application.
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   // These imports are only used to create an ID for this example.
   import java.util.Date;
   import java.text.SimpleDateFormat;

   @SpringBootApplication
   public class wingtiptoysdataApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;

      public static void main(String[] args) {
         // Execute the command line runner.
         SpringApplication.run(wingtiptoysdataApplication.class, args);
         System.exit(0);
      }

      public void run(String... args) throws Exception {
         // Create a simple date/time ID.
         SimpleDateFormat userId = new SimpleDateFormat("yyyyMMddHHmmssSSS");
         Date currentDate = new Date();

         // Create a new User class.
         final User testUser = new User(userId.format(currentDate), "Gena", "Soto");

         // For this example, remove all of the existing records.
         repository.deleteAll();

         // Save the User class to the Azure database.
         repository.save(testUser);
      
         // Retrieve the database record for the User class you just saved by ID.
         // final User result = repository.findOne(testUser.getId());
         final User result = repository.findById(testUser.getId()).get();

         // Display the results of the database record retrieval.
         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

   > [!IMPORTANT]
   >
   > <span data-ttu-id="3ad09-176">このチュートリアルの実行に Spring Boot 1.5.n バージョンのいずれかを使用している場合は、`final User result = repository.findById(testUser.getId()).get();` 構文を `final User result = repository.findOne(testUser.getId());` で置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="3ad09-176">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to replace the `final User result = repository.findById(testUser.getId()).get();` syntax with `final User result = repository.findOne(testUser.getId());`.</span></span>
   >

1. <span data-ttu-id="3ad09-177">メイン アプリケーションの Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-177">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="3ad09-178">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="3ad09-178">Build and test your app</span></span>

1. <span data-ttu-id="3ad09-179">コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-179">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="3ad09-180">または</span><span class="sxs-lookup"><span data-stu-id="3ad09-180">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="3ad09-181">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-181">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="3ad09-182">アプリケーションによっていくつかのランタイム メッセージが表示されます。値が正常に格納され、データベースから取得されたことを示す、次の例のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-182">Your application will display several runtime messages, and it will display a message like the following examples to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ```
   User: 20170724025215132 Gena Soto
   ```

   ![アプリケーションからの正常な出力][JV02]

1. <span data-ttu-id="3ad09-184">省略可能: Azure portal で **[データ エクスプローラー]** をクリックし、表示されたリストから項目を選択して内容を表示することで、データベースのプロパティ ページから Azure Cosmos DB の内容を表示できます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-184">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Data Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![ドキュメント エクスプローラーを使用してデータを表示する][JV03]

## <a name="next-steps"></a><span data-ttu-id="3ad09-186">次の手順</span><span class="sxs-lookup"><span data-stu-id="3ad09-186">Next steps</span></span>

<span data-ttu-id="3ad09-187">Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3ad09-187">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="3ad09-188">[Azure Cosmos DB のドキュメント]。</span><span class="sxs-lookup"><span data-stu-id="3ad09-188">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="3ad09-189">[Azure Cosmos DB: ドキュメント データベースを Java と Azure Portal で作成する][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="3ad09-189">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="3ad09-190">[Azure Cosmos DB SQL API の Spring Data]</span><span class="sxs-lookup"><span data-stu-id="3ad09-190">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="3ad09-191">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3ad09-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* <span data-ttu-id="3ad09-192">[Azure の Spring Boot DocumentDB Starter]</span><span class="sxs-lookup"><span data-stu-id="3ad09-192">[Spring Boot Document DB Starter for Azure]</span></span>

* [<span data-ttu-id="3ad09-193">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="3ad09-193">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="3ad09-194">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="3ad09-194">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="3ad09-195">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3ad09-195">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="3ad09-196">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="3ad09-196">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="3ad09-197">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="3ad09-197">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="3ad09-198">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="3ad09-198">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="3ad09-199">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="3ad09-199">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[Azure Cosmos DB SQL API の Spring Data]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Azure の Spring Boot DocumentDB Starter]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services 用の Java ツール]: https://azure.microsoft.com/services/devops/java/
[Java Tools for Visual Studio Team Services]: https://azure.microsoft.com/services/devops/java/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV03.png
