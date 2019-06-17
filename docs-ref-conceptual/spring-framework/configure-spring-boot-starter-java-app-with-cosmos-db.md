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
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: f00afbdd09ce617f863ed758f4bdddcb40701e27
ms.sourcegitcommit: 5bbf64121a99019207ed8cca29280fc5183c7314
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/12/2019
ms.locfileid: "66840842"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API で Spring Boot Starter を使用する方法

## <a name="overview"></a>概要

Azure Cosmos DB は、開発者が SQL、MongoDB、Graph、Table API などのさまざまな標準 API を使用してデータを操作できるようにするグローバル分散型データベース サービスです。 Microsoft の Spring Boot Starter を使用すると、開発者は、SQL API を使用して Azure Cosmos DB と簡単に統合できる Spring Boot アプリケーションを使用できます。

この記事では、Azure portal を使用して Azure Cosmos DB を作成する方法、 **[Spring Initializr]** を使用してカスタム Spring Boot アプリケーションを作成する方法、カスタム アプリケーションに [Azure の Spring Boot Cosmos DB Starter] を追加し、Spring Data と Cosmos DB SQL API を使用して Azure Cosmos DB にデータを格納またはデータを取得する方法を示します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Azure Portal を使用して Azure Cosmos DB を作成する

1. Azure portal (<https://portal.azure.com/>) を参照し、 **[+リソースの作成]** をクリックします。

   ![Azure ポータル][AZ01]

1. **[データベース]** 、 **[Azure Cosmos DB]** の順にクリックします。

   ![Azure ポータル][AZ02]

1. **[Azure Cosmos DB]** ページで、次の情報を入力します。

   * データベースに使用する**サブスクリプション**を選択します。
   * データベースに対して新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。
   * データベースの URI として使用する一意の**アカウント名**を入力します。 例: *wingtiptoysdata*
   * API の**コア (SQL)** を選択します。
   * データベースの**場所**を指定します。

   これらのオプションの指定後、 **[Review + create]\(確認および作成\)** をクリックしてデータベースを作成します。

   ![Azure ポータル][AZ03]

1. データベースが作成されると、それが Azure **ダッシュボード**に表示され、 **[すべてのリソース]** ページと **[Azure Cosmos DB]** ページにも表示されます。 これらのいずれかの場所でデータベースをクリックすると、キャッシュのプロパティ ページを開くことができます。

   ![Azure ポータル][AZ04]

1. データベースのプロパティ ページが表示されたら、 **[キー]** をクリックし、データベースの URI とアクセス キーをコピーします。これらの値は Spring Boot アプリケーションで使用します。

   ![Azure ポータル][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Spring Initializr でシンプルな Spring Boot アプリケーションを作成する

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven プロジェクト**を生成することを指定し、**Spring Boot** のバージョンを指定して、アプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力します。依存関係に **Azure サポート**を追加してから、 **[プロジェクトの生成]** のボタンをクリックします。

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > Spring Initializr では、 **[グループ]** と **[アーティファクト]** の名前を使用してパッケージ名を作成します (例: *com.example.wintiptoysdata*)。
   >

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードして、ファイルを抽出します。

   ![カスタム Spring Boot プロジェクトの抽出][SI02]

1. ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。

   ![カスタム Spring Boot プロジェクト ファイル][SI03]

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>Azure Spring Boot Starter を使用するように Spring Boot アプリケーションを構成する

1. アプリのディレクトリで *pom.xml* ファイルを探します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   または

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![pom.xml ファイルを探す][PM01]

1. テキスト エディターで *pom.xml* ファイルを開き、`<dependencies>` の一覧に次の行を追加します。

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
   </dependency>
   ```

   ![pom.xml ファイルの編集][PM02]

1. Spring Boot のバージョンが、Spring Initializr でアプリケーションを作成したときに選択したバージョンであることを確認します。次に例を示します。

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. 以下のような、最新バージョンの [Azure Spring Boot スターター](https://github.com/microsoft/azure-spring-boot)を使用していることを確認します。

   ```xml
   <azure.version>2.1.6</azure.version>
   ```

1. *pom.xml* ファイルを保存して閉じます。

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>Azure Cosmos DB を使用するように Spring Boot アプリケーションを構成する

1. アプリの *resources* ディレクトリ内で *application.properties* ファイルを探します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   または

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![application.properties ファイルを探す][RE01]

1. テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値をデータベースの適切なプロパティに置き換えます。

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.cosmosdb.database=wingtiptoysdata
   ```

   ![application.properties ファイルの編集][RE02]

1. *application.properties* ファイルを保存して閉じます。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>基本的なデータベース機能を実装するサンプル コードを追加する

このセクションでは、ユーザー データを格納するための 2 つの Java クラスを作成します。その後、アプリケーションのメイン クラスを変更して "*ユーザー*" クラスのインスタンスを作成し、それをデータベースに保存します。

### <a name="define-a-base-class-for-storing-user-data"></a>ユーザー データを格納するための基本クラスを定義する

1. *User.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。

1. テキスト エディターで *User.java* ファイルを開き、次の行をファイルに追加して、データベースの値を格納および取得する汎用ユーザー クラスを定義します。

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

1. *User.java* ファイルを保存して閉じます。

### <a name="define-a-data-repository-interface"></a>データ リポジトリ インターフェイスを定義する

1. *UserRepository.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。

1. テキスト エディターで *UserRepository.java* ファイルを開き、既定の DocumentDB リポジトリ インターフェイスを拡張するユーザー リポジトリ インターフェイスを定義する次の行をファイルに追加します。

   ```java
   package com.example.wingtiptoysdata;

   import com.microsoft.azure.spring.data.cosmosdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { }
   ```

1. *UserRepository.java* ファイルを保存して閉じます。

### <a name="modify-the-main-application-class"></a>アプリケーションのメイン クラスを変更する

1. アプリケーションのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   または

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![アプリケーションの Java ファイルを探す][JV01]

1. テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。

   ```java
    package com.example.wingtiptoysdata;

    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    import java.util.Optional;
    import java.util.UUID;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private final UserRepository repository;

        public WingtiptoysdataApplication(UserRepository repository) {
            this.repository = repository;
        }

        public static void main(String[] args) {
            // Execute the command line runner.
            SpringApplication.run(WingtiptoysdataApplication.class, args);
            System.exit(0);
        }

        public void run(String... args) throws Exception {
            // Create a unique identifier.
            String uuid = UUID.randomUUID().toString();

            // Create a new User class.
            final User testUser = new User(uuid, "John", "Doe");

            // For this example, remove all of the existing records.
            repository.deleteAll();

            // Save the User class to the Azure database.
            repository.save(testUser);

            // Retrieve the database record for the User class you just saved by ID.
            Optional<User> result = repository.findById(testUser.getId());

            // Display the results of the database record retrieval.
            System.out.println("\nSaved user is: " + result + "\n")
        }
    }
   ```

1. メイン アプリケーションの Java ファイルを保存して閉じます。

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。

   `cd C:\SpringBoot\wingtiptoysdata`

   または

   `cd /users/example/home/wingtiptoysdata`

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvnw clean spring-boot:run
   ```

1. アプリケーションによっていくつかのランタイム メッセージが表示されます。値が正常に格納され、データベースから取得されたことを示す、次の例のようなメッセージが表示されます。

   ```shell
   Saved user is: Optional[User: 24093cb5-55fe-4d2c-b459-cb8bafdd39fe John Doe]
   ```

   ![アプリケーションからの正常な出力][JV02]

1. 省略可能:Azure portal で **[データ エクスプローラー]** をクリックし、表示されたリストから項目を選択して内容を表示することで、データベースのプロパティ ページから Azure Cosmos DB の内容を表示できます。

   ![ドキュメント エクスプローラーを使用してデータを表示する][JV03]

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。

* [Azure Cosmos DB のドキュメント]。

* [Azure Cosmos DB:ドキュメント データベースを Java と Azure portal で作成する][Build a SQL API app with Java]

* [Azure Cosmos DB SQL API の Spring Data]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Azure の Spring Boot Cosmos DB Starter]

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、 **[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Java 開発者向けの Azure]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Azure Cosmos DB SQL API の Spring Data]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Azure の Spring Boot Cosmos DB Starter]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: https://azure.microsoft.com/services/devops/java/
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
