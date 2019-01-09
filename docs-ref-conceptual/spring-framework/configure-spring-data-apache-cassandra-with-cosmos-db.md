---
title: Azure Cosmos DB で Spring Data Apache Cassandra API を使用する方法
description: Azure Cosmos DB で Spring Data Apache Cassandra API を使用する方法を説明します。
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
ms.openlocfilehash: 1f3f7a55b49d64077df9e7d4d6acc3af4495b424
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992327"
---
# <a name="how-to-use-spring-data-apache-cassandra-api-with-azure-cosmos-db"></a>Azure Cosmos DB で Spring Data Apache Cassandra API を使用する方法

## <a name="overview"></a>概要

この記事では、[Spring Data] を使用して、[Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction) を使って情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* [Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントを作成する

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>Azure Portal を使用して Cosmos DB アカウントを作成する

> [!NOTE]
> 
> Azure Cosmos DB アカウントの作成に関する詳細については、[Azure Cosmos DB のドキュメント](/azure/cosmos-db/)を参照してください。

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[+ リソースの作成]** をクリックし、**[データベース]**、**[Azure Cosmos DB]** の順にクリックします。

   ![Azure Cosmos DB アカウントを作成する][COSMOSDB01]

1. 次の情報を指定します。

   - **サブスクリプション**:使用する Azure サブスクリプションを指定します。
   - **[リソース グループ]**: 新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。
   - **アカウント名**:Cosmos DB アカウント用に一意の名前を選択します。この名前は、*wingtiptoyscassandra.documents.azure.com* のような完全修飾ドメイン名の作成に使用されます。
   - **API**:このチュートリアルでは、`Cassandra` を指定します。
   - **場所**: データベースに最も近い地理的リージョンを指定します。

   ![Cosmos DB アカウントの設定を指定する][COSMOSDB02]
   
1. 上記の情報をすべて入力したら、**[レビュー + 作成]** をクリックします。

1. レビュー ページに問題がなければ、**[作成]** をクリックします。

   ![Cosmos DB アカウントの設定を確認する][COSMOSDB03]

### <a name="add-a-keyspace-to-your-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントにキースペースを追加する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックしてから、先ほど作成した Azure Cosmos DB アカウントをクリックします。

   ![Azure Cosmos DB アカウントを選択する][COSMOSDB04]

1. **[データ エクスプローラー]** をクリックし、**[New Keyspace]\(新しいキースペース\)** をクリックします。 **[Keyspace id]\(キースペース ID\)** に一意の識別子を入力し、**[OK]** をクリックします。

   ![Cosmos DB キースペースを作成する][COSMOSDB05]

### <a name="retrieve-the-connection-settings-for-your-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントの接続の設定を取得する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックしてから、先ほど作成した Azure Cosmos DB アカウントをクリックします。

   ![Azure Cosmos DB アカウントを選択する][COSMOSDB04]

1. **[接続文字列]** をクリックし、**[コンタクト ポイント]**、**[ポート]**、**[ユーザー名]**、および **[プライマリ パスワード]** の各フィールドの値をコピーします。これらの値は、アプリケーションを構成するために後で使用します。

   ![Cosmos DB の接続設定を取得する][COSMOSDB05]

## <a name="configure-the-sample-application"></a>サンプル アプリケーションを構成する

1. コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-cassandra-on-azure.git
   ```

1. サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。

1. テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。

   ```yaml
   spring.data.cassandra.contact-points=wingtiptoyscassandra.cassandra.cosmosdb.azure.com
   spring.data.cassandra.port=10350
   spring.data.cassandra.username=wingtiptoyscassandra
   spring.data.cassandra.password=********
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `spring.data.cassandra.contact-points` | この記事の前半の**コンタクト ポイント**を指定します。 |
   | `spring.data.cassandra.port` | この記事の前半の**ポート**を指定します。 |
   | `spring.data.cassandra.username` | この記事の前半の**ユーザー名**を指定します。 |
   | `spring.data.cassandra.password` | この記事の前半の**プライマリ パスワード**を指定します。 |

1. *application.properties* ファイルを保存して閉じます。

## <a name="package-and-test-the-sample-application"></a>サンプル アプリケーションをパッケージ化してテストする 

1. サンプル アプリケーションを Maven でビルドします。次に例を示します。

   ```shell
   mvn clean package
   ```

1. サンプル アプリケーションを開始します。次に例を示します。

   ```shell
   java -jar target/spring-data-cassandra-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   アプリケーションから次のような値が返されます。

   ```shell
   Added Pet{id=60fa8cb0-0423-11e9-9a70-39311962166b, name='dog', species='canine'}.

   Added Pet{id=72c1c9e0-0423-11e9-9a70-39311962166b, name='cat', species='feline'}.
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   アプリケーションから次のような値が返されます。

   ```json
   [{"id":"60fa8cb0-0423-11e9-9a70-39311962166b","name":"dog","species":"canine"},{"id":"72c1c9e0-0423-11e9-9a70-39311962166b","name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>まとめ

このチュートリアルでは、Spring Data を使用して、Azure Cosmos DB Cassandra API を使って情報を格納および取得する Java のサンプル アプリケーションを作成しました。

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

<!-- URL List -->

[Java 開発者向けの Azure]: /java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[COSMOSDB01]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB05]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05.png
