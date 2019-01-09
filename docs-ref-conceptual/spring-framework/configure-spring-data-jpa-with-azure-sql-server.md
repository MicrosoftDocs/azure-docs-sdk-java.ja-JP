---
title: Azure SQL Database で Spring Data JPA を使用する方法
description: Azure SQL Database で Spring Data JPA を使用する方法を説明します。
services: sql-database
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 7119283bec250a4ab0854ba2c29b0906624448e9
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992335"
---
# <a name="how-to-use-spring-data-jpa-with-azure-sql-database"></a>Azure SQL Database で Spring Data JPA を使用する方法

## <a name="overview"></a>概要

この記事では、[Spring Data] を使用して、[Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) を使って [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 内の情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* [Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="create-an-azure-sql-satabase"></a>Azure SQL データベースを作成する

### <a name="create-a-sql-database-server-using-the-azure-portal"></a>Azure Portal を使用して SQL データベース サーバーを作成する

> [!NOTE]
> 
> Azure SQL データベースの作成に関する詳細については、[Azure portal での Azure SQL データベースの作成](/azure/sql-database/sql-database-get-started-portal)に関するページを参照してください。

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[+ リソースの作成]** をクリックし、**[データベース]**、**[SQL Database]** の順にクリックします。

   ![SQL Database の作成][SQL01]

1. 次の情報を指定します。

   - **データベース名**: SQL データベース用に一意の名前を選択します。このデータベースが、指定した SQL Server に後で作成されます。
   - **サブスクリプション**:使用する Azure サブスクリプションを指定します。
   - **[リソース グループ]**: 新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。
   - **ソースの選択**:このチュートリアルでは、`Blank database` を選択して新しいデータベースを作成します。

   ![SQL データベースのプロパティを指定する][SQL02]
   
1. **[サーバー]**、**[新しいサーバーの作成]** の順にクリックして、次の情報を指定します。

   - **サーバー名**: SQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoyssql.database.windows.net* のような完全修飾ドメイン名の作成に使用されます。
   - **サーバー管理者ログイン**:データベース管理者名を指定します。
   - **[パスワード]** と **[パスワードの確認]**:データベース管理者のパスワードを指定します。
   - **場所**: データベースに最も近い地理的リージョンを指定します。

   ![SQL サーバーを指定する][SQL03]

1. 上記の情報をすべて入力したら、**[選択]** をクリックします。

1. このチュートリアルでは、最も安い**価格レベル**を指定して、**[作成]** をクリックします。

   ![SQL データベースを作成する][SQL04]

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a>Azure Portal を使用して SQL サーバーのファイアウォール規則を構成する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックし、先ほど作成した SQL サーバーをクリックします。

   ![SQL サーバーを選択する][SQL05]

1. **[概要]** セクションで、**[ファイアウォール設定の表示]** をクリックします。

   ![ファイアウォール設定の表示][SQL06]

1. **[ファイアウォールと仮想ネットワーク]** セクションで、規則に一意の名前を指定することで新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、**[保存]** をクリックします。

   ![ファイアウォール設定を構成する][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a>Azure Portal を使用して SQL サーバーの接続文字列を取得する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックし、先ほど作成した SQL データベースをクリックします。

   ![SQL データベースを選択する][SQL08]

1. **[接続文字列]**、**[JDBC]** の順にクリックし、JDBC のテキスト フィールド内の値をコピーします。

   ![JDBC 接続文字列を取得する][SQL09]

## <a name="configure-the-sample-application"></a>サンプル アプリケーションを構成する

1. コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。

1. テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `spring.datasource.url` | この記事の前半の SQL JDBC 文字列の編集されたバージョンを指定します。 |
   | `spring.datasource.username` | この記事の前半の SQL 管理者名を指定し、その後に短縮サーバー名を追加します。 |
   | `spring.datasource.password` | この記事の前半の SQL 管理者パスワードを指定します。 |

1. *application.properties* ファイルを保存して閉じます。

## <a name="package-and-test-the-sample-application"></a>サンプル アプリケーションをパッケージ化してテストする 

1. サンプル アプリケーションを Maven でビルドします。次に例を示します。

   ```shell
   mvn clean package -P sql
   ```

1. サンプル アプリケーションを開始します。次に例を示します。

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   アプリケーションから次のような値が返されます。

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   アプリケーションから次のような値が返されます。

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>まとめ

このチュートリアルでは、Spring Data を使用して、JPA を使って Azure SQL データベース内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。

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

[SQL01]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-09.png
