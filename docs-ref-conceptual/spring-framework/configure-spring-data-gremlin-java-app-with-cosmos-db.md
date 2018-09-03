---
title: Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法
description: Spring Boot Initializer で作成されたアプリケーションを Azure Cosmos DB SQL API で構成する方法について説明します。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 08/20/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 0976f4b0c13ce5c577458f2974f5dce123bf7e59
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43241045"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法

## <a name="overview"></a>概要

Spring Data Gremlin Starter は、Apache の Gremlin クエリ言語に Spring Data のサポートを提供します。これは、開発者が任意の Gremlin 互換データ ストアで使用できます。

この記事では、Azure portal を使用して Gremlin API で使うための Azure Cosmos DB を作成する方法、**[Spring Initializr]** を使用してカスタム Java アプリケーションを作成する方法、カスタム アプリケーションに Spring Data Gremlin Starter 機能を追加し、Gremlin を使用して Azure Cosmos DB にデータを格納する方法、または Azure Cosmos DB からデータを取得する方法を示します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

> [!IMPORTANT]
>
> この記事の手順を完了するには、Spring Boot 2.0 以上のバージョンが必要です。
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a>Azure portal を使用して Azure Cosmos DB を作成する

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a>Gremlin API で使用するご自身の Azure Cosmos Database を作成する

1. Azure portal (<https://portal.azure.com/>) を参照し、**[+リソースの作成]** をクリックします。

   ![リソースの作成][AZ01]

1. **[データベース]**、**[Azure Cosmos DB]** の順にクリックします。

   ![Azure Cosmos DB の作成][AZ02]

1. **[Azure Cosmos DB]** ページで、次の情報を入力します。

   * データベースの Gremlin URI の一部として使用する一意の **ID** を入力します。 たとえば、**ID** として **wingtiptoysdata** を入力した場合、Gremlin URI は *wingtiptoysdata.gremlin.cosmosdb.azure.com* になります。
   * API として **[Gremlin (グラフ)]** を選択します。
   * データベースに使用する**サブスクリプション**を選択します。
   * データベースに対して新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。
   * データベースの**場所**を指定します。
   
   これらのオプションの指定後、**[作成]** をクリックしてデータベースを作成します。

   ![Azure Cosmos DB オプションの指定][AZ03]

1. データベースが作成されると、それが Azure **ダッシュボード**に表示され、**[すべてのリソース]** ページと **[Azure Cosmos DB]** ページにも表示されます。 これらのいずれかの場所でデータベースをクリックすると、キャッシュのプロパティ ページを開くことができます。

   ![すべてのリソース][AZ04]

1. データベースのプロパティ ページが表示されたら、**[アクセス キー]** をクリックし、データベースの URI とアクセス キーをコピーします。これらの値は Spring Boot アプリケーションで使用します。

   ![[アクセス キー]][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>ご自身の Azure Cosmos Database にグラフを追加する

1. **[データ エクスプローラー]** をクリックし、**[New Graph]\(新しいグラフ\)** をクリックします。

   ![新しいグラフ][AZ06]

1. **[グラフの追加]** ページが表示されたら、次の情報を入力します。

   * お使いのデータベースの一意の**データベース ID** を指定します。
   * お使いのグラフの一意の**グラフ ID** を指定します。
   * **ストレージ容量**は、指定することも、既定値をそのまま使用することもできます。
   * **スループット**を指定します。この例では、400 要求ユニット (RU) を選択します。
   
   これらのオプションの指定後、**[OK]** をクリックして、グラフを作成します。

   ![グラフの追加][AZ07]

1. 目的のグラフが作成されたら、**データ エクスプローラー**を使用して、そのグラフを表示できます。

   ![グラフのプロパティの表示][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Spring Initializr でシンプルな Spring Boot アプリケーションを作成する

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、ご自身のアプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力します。次に、お使いの **Spring Boot** バージョン (2.0 以上) を指定し、**[プロジェクトの生成]** をクリックします。

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > Spring Initializr では、**[グループ]** と **[アーティファクト]** の名前を使用してパッケージ名を作成します (例: *com.example.wintiptoysdata)。
   >

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

   ![カスタム Spring Boot プロジェクトのダウンロード][SI02]

1. ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。

   ![カスタム Spring Boot プロジェクト ファイル][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a>Spring Data Gremlin Starter を使用するように Spring Boot アプリを構成する

1. アプリのディレクトリで *pom.xml* ファイルを探します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   または

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![pom.xml ファイルを探す][PM01]

1. テキスト エディターで *pom.xml* ファイルを開き、`<dependencies>` の一覧に Spring Data Gremlin Starter を追加します。

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![pom.xml ファイルの編集][PM02]

1. *pom.xml* ファイルを保存して閉じます。

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>Azure Cosmos DB を使用するように Spring Boot アプリを構成する

1. ご自身のアプリの *resources* ディレクトリを見つけて、*application.yml* という名前の新しいファイルを作成します。 例: 

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   または

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![application.yml ファイルの作成][RE01]

1.  テキスト エディターで *application.yml* ファイルを開き、そのファイルに次の行を追加して、サンプルの値をご自身のデータベース用の適切なプロパティで置き換えます。

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   各値の説明:
   | フィールド | 説明 |
   | ---|---|
   | `endpoint` | ご自身のデータベースの Gremlin URI を指定します。これは、このチュートリアルで先ほど Azure Cosmos DB を作成したときに指定した一意の **ID** から派生します。 |
   | `port` | TCP/IP ポートを指定します。HTTPS 用の **443** を使用する必要があります。 |
   | `username` | このチュートリアルで先ほどグラフを追加したときに使用した一意の**データベース ID** と**グラフ ID** を指定します。"/dbs/**{Database id}**/colls/**{Graph id}**" という構文を使用して入力する必要があります。 |
   | `password` | このチュートリアルで先ほどコピーしたプライマリまたはセカンダリの**アクセス キー**を指定します。 |
   | `telemetryAllowed` | 利用統計情報を有効にする場合は **true**、それ以外の場合は **false** を指定します。

1. *application.yml* ファイルを保存して閉じます。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>基本的なデータベース機能を実装するサンプル コードを追加する

このセクションでは、ご自身のデータベースにデータを格納するために必要な Java クラスを作成します。

### <a name="modify-the-main-application-class"></a>アプリケーションのメイン クラスを変更する

1. アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   または

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![アプリケーションの Java ファイルを探す][JV01]

1. テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。

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

1. メイン アプリケーションの Java ファイルを保存して閉じます。

### <a name="define-a-basic-class-for-storing-configuration-information"></a>構成情報を格納するための基本クラスを定義する

1. ご自身のアプリのパッケージ ディレクトリに *config* という名前のフォルダーを作成します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   または

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. *config* ディレクトリに *UserRepositoryConfiguration.java* という名前の新しい Java ファイルを作成し、テキスト エディターでそのファイルを開いて、次の行を追加します。

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

1. *UserRepositoryConfiguration.java* ファイルを保存して閉じます。

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a>グラフ データベースの要素を定義するクラスのセットを定義する

1. ご自身のアプリのパッケージ ディレクトリに *domain* という名前のフォルダーを作成します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   または

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. *domain* ディレクトリに *Person.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。

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

1. *Person.java* ファイルを保存して閉じます。

1. *domain* ディレクトリに *Relation.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。

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

1. *Relation.java* ファイルを保存して閉じます。

1. *domain* ディレクトリに *Network.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。

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

1. *Network.java* ファイルを保存して閉じます。

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a>グラフ データベース用のリポジトリを定義するクラスのセットを定義する

1. ご自身のアプリのパッケージ ディレクトリに *repository* という名前のフォルダーを作成します。次に例を示します。

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   または

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. *repository* ディレクトリに *NetworkRepository.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. *NetworkRepository.java* ファイルを保存して閉じます。

1. *repository* ディレクトリに *PersonRepository.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. *PersonRepository.java* ファイルを保存して閉じます。

1. *repository* ディレクトリに *RelationRepository.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. *RelationRepository.java* ファイルを保存して閉じます。

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。

   `cd C:\SpringBoot\wingtiptoysdata`

   または

   `cd /users/example/home/wingtiptoysdata`

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. ご自身のアプリケーションにランタイム メッセージがいくつか表示されます。エラーがない場合は、Azure portal を使用して、Azure Cosmos DB の内容を表示できます。 これを行うには、お使いのデータベースのプロパティ ページで **[データ エクスプローラー]** をクリックし、**[Execute Gremlin Query]\(Gremlin クエリの実行\)** をクリックして、データを表示する結果の一覧から項目を選択します。

   ![ドキュメント エクスプローラーを使用してデータを表示する][JV03]

## <a name="next-steps"></a>次の手順

Gremlin および Graph API に対する Azure サポートの詳細については、次の記事をご覧ください。

* [Azure Cosmos DB の概要: Graph API](https://docs.microsoft.com/azure/cosmos-db/graph-introduction)

* [Azure Cosmos DB での Gremlin グラフのサポート](https://docs.microsoft.com/azure/cosmos-db/gremlin-support)

* [Azure Cosmos DB: グラフ データベースを Java と Azure portal で作成する](https://docs.microsoft.com/azure/cosmos-db/create-graph-java)

* [チュートリアル: Gremlin を使って Azure Cosmos DB Graph API を照会する](https://docs.microsoft.com/azure/cosmos-db/tutorial-query-graph)

* [Spring Data Gremlin Starter]

Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。

* [Azure Cosmos DB のドキュメント]。

* [Azure Cosmos DB: ドキュメント データベースを Java と Azure Portal で作成する][Build a SQL API app with Java]

* [Azure Cosmos DB SQL API の Spring Data]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。


<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[Azure Cosmos DB SQL API の Spring Data]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
