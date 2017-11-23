---
title: "Azure Cosmos DB DocumentDB API で Spring Boot Starter を使用する方法"
description: "Spring Boot Initializer で作成されたアプリケーションを Azure Cosmos DB DocumentDB API で構成する方法について説明します。"
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Spring Starter, Cosmos DB, DocumentDB
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: a80ac6be1064e40cd0b693ac4e6c0b1a9723cfc4
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/18/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="3127a-104">Azure Cosmos DB DocumentDB API で Spring Boot Starter を使用する方法</span><span class="sxs-lookup"><span data-stu-id="3127a-104">How to use the Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="3127a-105">概要</span><span class="sxs-lookup"><span data-stu-id="3127a-105">Overview</span></span>

<span data-ttu-id="3127a-106">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="3127a-106">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="3127a-107">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="3127a-107">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="3127a-108">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="3127a-108">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="3127a-109">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="3127a-109">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="3127a-110">Azure Cosmos DB は、開発者が DocumentDB、MongoDB、Graph、Table API などのさまざまな標準 API を使用してデータを操作できるようにするグローバル分散型データベース サービスです。</span><span class="sxs-lookup"><span data-stu-id="3127a-110">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="3127a-111">Microsoft の Spring Boot Starter を使用すると、開発者は、DocumentDB API を使用して Azure Cosmos DB と簡単に統合できる Spring Boot アプリケーションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="3127a-111">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="3127a-112">この記事では、Azure Portal を使用して Azure Cosmos DB を作成してから、**Spring Initializr** を使用してカスタム Java アプリケーションを作成し、さらにカスタム アプリケーションに Spring Boot Starter 機能を追加して、DocumentDB API を使用して Azure Cosmos DB にデータを格納またはデータを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3127a-112">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **Spring Initializr** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3127a-113">前提条件</span><span class="sxs-lookup"><span data-stu-id="3127a-113">Prerequisites</span></span>

<span data-ttu-id="3127a-114">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="3127a-114">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="3127a-115">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="3127a-115">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="3127a-116">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="3127a-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="3127a-117">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="3127a-117">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="3127a-118">Azure Portal を使用して Azure Cosmos DB を作成する</span><span class="sxs-lookup"><span data-stu-id="3127a-118">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="3127a-119"><https://portal.azure.com/> で Azure Portal を参照し、**[+新規]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3127a-119">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure ポータル][AZ01]

1. <span data-ttu-id="3127a-121">**[データベース]**、**[Azure Cosmos DB]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="3127a-121">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure ポータル][AZ02]

1. <span data-ttu-id="3127a-123">**[Azure Cosmos DB]** ページで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="3127a-123">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="3127a-124">データベースの URI として使用する一意の **ID** を入力します。</span><span class="sxs-lookup"><span data-stu-id="3127a-124">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="3127a-125">例: *wingtiptoysdata.documents.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="3127a-125">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="3127a-126">API として **[SQL (Document DB)]\(SQL (Document DB)\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3127a-126">Choose **SQL (Document DB)** for the API.</span></span>
   * <span data-ttu-id="3127a-127">データベースに使用する**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="3127a-127">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="3127a-128">データベースに対して新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="3127a-128">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="3127a-129">データベースの**場所**を指定します。</span><span class="sxs-lookup"><span data-stu-id="3127a-129">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="3127a-130">これらのオプションの指定後、**[作成]** をクリックしてデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="3127a-130">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure ポータル][AZ03]

1. <span data-ttu-id="3127a-132">データベースが作成されると、それが Azure **ダッシュボード**に表示され、**[すべてのリソース]** ページと **[Azure Cosmos DB]** ページにも表示されます。</span><span class="sxs-lookup"><span data-stu-id="3127a-132">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="3127a-133">これらのいずれかの場所でデータベースをクリックすると、キャッシュのプロパティ ページを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="3127a-133">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure ポータル][AZ04]

1. <span data-ttu-id="3127a-135">データベースのプロパティ ページが表示されたら、**[アクセス キー]** をクリックし、データベースの URI とアクセス キーをコピーします。これらの値は Spring Boot アプリケーションで使用します。</span><span class="sxs-lookup"><span data-stu-id="3127a-135">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure ポータル][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="3127a-137">Spring Initializr でシンプルな Spring Boot アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="3127a-137">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="3127a-138"><https://start.spring.io/> に移動します。</span><span class="sxs-lookup"><span data-stu-id="3127a-138">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="3127a-139">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力して、**[プロジェクトの生成]** のボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3127a-139">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the button to **Generate Project**.</span></span>

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="3127a-141">Spring Initializr では、**[グループ]** と **[アーティファクト]** の名前を使用してパッケージ名を作成します (例: *com.example.wintiptoys*)。</span><span class="sxs-lookup"><span data-stu-id="3127a-141">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="3127a-142">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="3127a-142">When prompted, download the project to a path on your local computer.</span></span>

   ![カスタム Spring Boot プロジェクトのダウンロード][SI02]

1. <span data-ttu-id="3127a-144">ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="3127a-144">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![カスタム Spring Boot プロジェクト ファイル][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="3127a-146">Azure Spring Boot Starter を使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="3127a-146">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="3127a-147">アプリのディレクトリで *pom.xml* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3127a-147">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="3127a-148">または</span><span class="sxs-lookup"><span data-stu-id="3127a-148">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![pom.xml ファイルを探す][PM01]

1. <span data-ttu-id="3127a-150">テキスト エディターで *pom.xml* ファイルを開き、`<dependencies>` の一覧に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="3127a-150">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![pom.xml ファイルの編集][PM02]

1. <span data-ttu-id="3127a-152">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3127a-152">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="3127a-153">Azure Cosmos DB を使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="3127a-153">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="3127a-154">アプリの *resources* ディレクトリ内で *application.properties* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3127a-154">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="3127a-155">または</span><span class="sxs-lookup"><span data-stu-id="3127a-155">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![application.properties ファイルを探す][RE01]

1. <span data-ttu-id="3127a-157">テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値をデータベースの適切なプロパティに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3127a-157">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![application.properties ファイルの編集][RE02]

1. <span data-ttu-id="3127a-159">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3127a-159">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="3127a-160">基本的なデータベース機能を実装するサンプル コードを追加する</span><span class="sxs-lookup"><span data-stu-id="3127a-160">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="3127a-161">このセクションでは、ユーザー データを格納するための 2 つの Java クラスを作成します。その後、アプリケーションのメイン クラスを変更してユーザー クラスのインスタンスを作成し、それをデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="3127a-161">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="3127a-162">ユーザー データを格納するための基本クラスを定義する</span><span class="sxs-lookup"><span data-stu-id="3127a-162">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="3127a-163">*User.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。</span><span class="sxs-lookup"><span data-stu-id="3127a-163">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="3127a-164">テキスト エディターで *User.java* ファイルを開き、次の行をファイルに追加して、データベースの値を格納および取得する汎用ユーザー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="3127a-164">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
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
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="3127a-165">*User.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3127a-165">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="3127a-166">データ リポジトリ インターフェイスを定義する</span><span class="sxs-lookup"><span data-stu-id="3127a-166">Define a data repository interface</span></span>

1. <span data-ttu-id="3127a-167">*UserRepository.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。</span><span class="sxs-lookup"><span data-stu-id="3127a-167">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="3127a-168">テキスト エディターで *UserRepository.java* ファイルを開き、既定の DocumentDB リポジトリ インターフェイスを拡張するユーザー リポジトリ インターフェイスを定義する次の行をファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="3127a-168">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="3127a-169">*UserRepository.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3127a-169">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="3127a-170">アプリケーションのメイン クラスを変更する</span><span class="sxs-lookup"><span data-stu-id="3127a-170">Modify the main application class</span></span>

1. <span data-ttu-id="3127a-171">アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3127a-171">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="3127a-172">または</span><span class="sxs-lookup"><span data-stu-id="3127a-172">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![アプリケーションの Java ファイルを探す][JV01]

1. <span data-ttu-id="3127a-174">テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="3127a-174">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. <span data-ttu-id="3127a-175">メイン アプリケーションの Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="3127a-175">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="3127a-176">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="3127a-176">Build and test your app</span></span>

1. <span data-ttu-id="3127a-177">コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3127a-177">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="3127a-178">または</span><span class="sxs-lookup"><span data-stu-id="3127a-178">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="3127a-179">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3127a-179">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="3127a-180">アプリケーションによっていくつかのランタイム メッセージが表示されます。値が正常に格納され、データベースから取得されたことを示すメッセージ `User: testFirstName testLastName` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3127a-180">Your application will display several runtime messages, and you should see the message `User: testFirstName testLastName` displayed to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ![アプリケーションからの正常な出力][JV02]

1. <span data-ttu-id="3127a-182">省略可能: Azure ポータルで **[ドキュメント エクスプローラー]** をクリックし、表示されたリストから項目を選んで内容を表示することで、データベースのプロパティ ページから Azure Cosmos DB の内容を表示できます。</span><span class="sxs-lookup"><span data-stu-id="3127a-182">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Document Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![ドキュメント エクスプローラーを使用してデータを表示する][JV03]

## <a name="next-steps"></a><span data-ttu-id="3127a-184">次のステップ</span><span class="sxs-lookup"><span data-stu-id="3127a-184">Next steps</span></span>

<span data-ttu-id="3127a-185">Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3127a-185">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="3127a-186">[Azure Cosmos DB のドキュメント]。</span><span class="sxs-lookup"><span data-stu-id="3127a-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="3127a-187">[Azure Cosmos DB: Java と Azure Portal による DocumentDB API アプリの構築][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="3127a-187">[Azure Cosmos DB: Build a DocumentDB API app with Java and the Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="3127a-188">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3127a-188">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="3127a-189">Azure の Spring Boot DocumenDB Starter</span><span class="sxs-lookup"><span data-stu-id="3127a-189">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="3127a-190">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="3127a-190">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="3127a-191">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="3127a-191">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="3127a-192">Java での Azure の使用の詳細については、 [Azure Java デベロッパー センター] と[Java Tools for Visual Studio Team Services] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3127a-192">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Azure Java デベロッパー センター]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
