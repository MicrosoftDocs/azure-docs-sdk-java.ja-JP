---
title: Azure PostgreSQL で Spring Data JDBC を使用する方法
description: Azure PostgreSQL データベースで Spring Data JDBC を使用する方法を説明します。
services: postgresql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 29f3c957dd0ccd754eedef12e3fc01c3484dddf3
ms.sourcegitcommit: 1c1412ad5d8960975c3fc7fd3d1948152ef651ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57335395"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-postgresql"></a><span data-ttu-id="f9155-103">Azure PostgreSQL で Spring Data JDBC を使用する方法</span><span class="sxs-lookup"><span data-stu-id="f9155-103">How to use Spring Data JDBC with Azure PostgreSQL</span></span>

## <a name="overview"></a><span data-ttu-id="f9155-104">概要</span><span class="sxs-lookup"><span data-stu-id="f9155-104">Overview</span></span>

<span data-ttu-id="f9155-105">この記事では、[Spring Data] を使用して、[Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使って Azure [PostgreSQL](https://www.postgresql.org/) データベース内の情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f9155-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [PostgreSQL](https://www.postgresql.org/) database using [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9155-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="f9155-106">Prerequisites</span></span>

<span data-ttu-id="f9155-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9155-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="f9155-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="f9155-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="f9155-109">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="f9155-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="f9155-110">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f9155-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="f9155-111">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="f9155-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="f9155-112">[Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="f9155-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="f9155-113">[psql](https://www.postgresql.org/docs/current/app-psql.html) コマンドライン ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="f9155-113">The [psql](https://www.postgresql.org/docs/current/app-psql.html) command-line utility.</span></span>
* <span data-ttu-id="f9155-114">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="f9155-114">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-postgresql-database-for-azure"></a><span data-ttu-id="f9155-115">Azure 用に PostgreSQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="f9155-115">Create a PostgreSQL database for Azure</span></span>

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="f9155-116">Azure Portal を使用して PostgreSQL データベース サーバーを作成する</span><span class="sxs-lookup"><span data-stu-id="f9155-116">Create a PostgreSQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="f9155-117">PostgreSQL データベースの作成に関する詳細については、[Azure Portal を使用した Azure Database for PostgreSQL サーバーの作成](/azure/postgresql/quickstart-create-server-database-portal)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f9155-117">You can read more detailed information about creating PostgreSQL databases in [Create an Azure Database for PostgreSQL server by using the Azure portal](/azure/postgresql/quickstart-create-server-database-portal).</span></span>

1. <span data-ttu-id="f9155-118">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="f9155-118">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="f9155-119">**[+ リソースの作成]** をクリックし、**[データベース]**、**[Azure Database for PostgreSQL]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9155-119">Click **+Create a resource**, then **Databases**, and then click **Azure Database for PostgreSQL**.</span></span>

   ![PostgreSQL データベースを作成する][POSTGRESQL01]

1. <span data-ttu-id="f9155-121">次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="f9155-121">Enter the following information:</span></span>

   - <span data-ttu-id="f9155-122">**サーバー名**: PostgreSQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoyspostgresql.postgres.database.azure.com* のような完全修飾ドメイン名の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="f9155-122">**Server name**: Choose a unique name for your PostgreSQL server; this will be used to create a fully-qualified domain name like *wingtiptoyspostgresql.postgres.database.azure.com*.</span></span>
   - <span data-ttu-id="f9155-123">**サブスクリプション**:使用する Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-123">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="f9155-124">**[リソース グループ]**:新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-124">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="f9155-125">**ソースの選択**:このチュートリアルでは、`Blank` を選択して新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9155-125">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="f9155-126">**サーバー管理者ログイン**:データベース管理者名を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-126">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="f9155-127">**[パスワード]** と **[パスワードの確認]**:データベース管理者のパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-127">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="f9155-128">**[場所]**:データベースに最も近い地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-128">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="f9155-129">**バージョン**:最新のデータベース バージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-129">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="f9155-130">**価格レベル**:このチュートリアルでは、最も安い価格レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-130">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![PostgreSQL データベースのプロパティを作成する][POSTGRESQL02]

1. <span data-ttu-id="f9155-132">上記の情報をすべて入力したら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9155-132">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="f9155-133">Azure Portal を使用して PostgreSQL データベース サーバーのファイアウォール規則を構成する</span><span class="sxs-lookup"><span data-stu-id="f9155-133">Configure a firewall rule for your PostgreSQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="f9155-134">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="f9155-134">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="f9155-135">**[すべてのリソース]** をクリックし、先ほど作成した PostgreSQL データベースをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9155-135">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![PostgreSQL データベースを選択する][POSTGRESQL03]

1. <span data-ttu-id="f9155-137">**[接続のセキュリティ]** をクリックし、**[ファイアウォール規則]** で、規則の一意の名前を指定して新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9155-137">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![接続のセキュリティを構成する][POSTGRESQL04]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a><span data-ttu-id="f9155-139">Azure Portal を使用して PostgreSQL サーバーの接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="f9155-139">Retrieve the connection string for your PostgreSQL server using the Azure Portal</span></span>

1. <span data-ttu-id="f9155-140">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="f9155-140">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="f9155-141">**[すべてのリソース]** をクリックし、先ほど作成した PostgreSQL データベースをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9155-141">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![PostgreSQL データベースを選択する][POSTGRESQL03]

1. <span data-ttu-id="f9155-143">**[接続文字列]** をクリックし、**[JDBC]** テキスト フィールド内の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="f9155-143">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![JDBC 接続文字列を取得する][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a><span data-ttu-id="f9155-145">`psql` コマンド ライン ユーティリティを使用して PostgreSQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="f9155-145">Create PostgreSQL database using the `psql` command-line utility</span></span>

1. <span data-ttu-id="f9155-146">コマンド シェルを開き、次の例のように `psql` コマンドを入力して PostgreSQL サーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="f9155-146">Open a command shell and connect to your PostgreSQL server by entering a `psql` command like the following example:</span></span>

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   <span data-ttu-id="f9155-147">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="f9155-147">Where:</span></span>

   | <span data-ttu-id="f9155-148">パラメーター</span><span class="sxs-lookup"><span data-stu-id="f9155-148">Parameter</span></span> | <span data-ttu-id="f9155-149">説明</span><span class="sxs-lookup"><span data-stu-id="f9155-149">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="f9155-150">この記事の前半の完全修飾 PostgreSQL サーバー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-150">Specifies your fully qualified PostgreSQL server name from earlier in this article.</span></span> |
   | `host` | <span data-ttu-id="f9155-151">PostgreSQL サーバー ポートを指定します。既定では `5432` です。</span><span class="sxs-lookup"><span data-stu-id="f9155-151">Specifies the PostgreSQL server port, which is `5432` by default.</span></span> |
   | `username` | <span data-ttu-id="f9155-152">この記事の前半の PostgreSQL 管理者と短縮サーバー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-152">Specifies your PostgreSQL administrator and shortened server name from earlier in this article.</span></span> |
   | `dbname` | <span data-ttu-id="f9155-153">差し当たり既定の `postgres` データベースを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-153">Specifies that you want to use the default `postgres` database for now.</span></span> |

   <span data-ttu-id="f9155-154">PostgreSQL サーバーは、次の例のような表示で応答します。</span><span class="sxs-lookup"><span data-stu-id="f9155-154">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```

1. <span data-ttu-id="f9155-155">次の例のように `psql` コマンドを入力して、*mypgsqldb* という名前のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9155-155">Create a database named *mypgsqldb* by entering a `psql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   <span data-ttu-id="f9155-156">PostgreSQL サーバーは、次の例のような表示で応答します。</span><span class="sxs-lookup"><span data-stu-id="f9155-156">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   CREATE DATABASE
   ```

1. <span data-ttu-id="f9155-157">省略可能:データベースが作成されたことは、`psql` で `\l` を入力して確認できます。PostgreSQL サーバーは次の例のように応答します。</span><span class="sxs-lookup"><span data-stu-id="f9155-157">OPTIONAL: You can verify that your database was created by entering a `\l` at the `psql`; your PostgreSQL server should respond with something like the following example:</span></span>

   ```shell
                   List of databases
          Name        |      Owner      | Encoding
   -------------------+-----------------+----------
    azure_maintenance | azure_superuser | UTF8
    azure_sys         | azure_superuser | UTF8
    mypgsqldb         | wingtiptoysuser | UTF8
    postgres          | azure_superuser | UTF8
    template0         | azure_superuser | UTF8
    template1         | azure_superuser | UTF8
   (6 rows)
   ```

1. <span data-ttu-id="f9155-158">`\q` を入力して `psql` ユーティリティを終了します。</span><span class="sxs-lookup"><span data-stu-id="f9155-158">Enter `\q` to exit the `psql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="f9155-159">サンプル アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="f9155-159">Configure the sample application</span></span>

1. <span data-ttu-id="f9155-160">コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。</span><span class="sxs-lookup"><span data-stu-id="f9155-160">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="f9155-161">サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9155-161">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="f9155-162">テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f9155-162">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   <span data-ttu-id="f9155-163">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="f9155-163">Where:</span></span>

   | <span data-ttu-id="f9155-164">パラメーター</span><span class="sxs-lookup"><span data-stu-id="f9155-164">Parameter</span></span> | <span data-ttu-id="f9155-165">説明</span><span class="sxs-lookup"><span data-stu-id="f9155-165">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="f9155-166">この記事の前半の PostgreSQL JDBC 文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-166">Specifies your PostgreSQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="f9155-167">この記事の前半の PostgreSQL 管理者名を指定し、その後に短縮サーバー名を追加します。</span><span class="sxs-lookup"><span data-stu-id="f9155-167">Specifies your PostgreSQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="f9155-168">この記事の前半の PostgreSQL 管理者パスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9155-168">Specifies your PostgreSQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="f9155-169">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="f9155-169">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="f9155-170">サンプル アプリケーションをパッケージ化してテストする</span><span class="sxs-lookup"><span data-stu-id="f9155-170">Package and test the sample application</span></span> 

1. <span data-ttu-id="f9155-171">サンプル アプリケーションを Maven でビルドします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f9155-171">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P postgresql
   ```

1. <span data-ttu-id="f9155-172">サンプル アプリケーションを開始します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f9155-172">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="f9155-173">次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9155-173">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="f9155-174">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="f9155-174">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="f9155-175">次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="f9155-175">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="f9155-176">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="f9155-176">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="f9155-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="f9155-177">Summary</span></span>

<span data-ttu-id="f9155-178">このチュートリアルでは、Spring Data を使用して、JDBC を使って Azure PostgreSQL データベース 内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="f9155-178">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure PostgreSQL database using JDBC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9155-179">次の手順</span><span class="sxs-lookup"><span data-stu-id="f9155-179">Next steps</span></span>

<span data-ttu-id="f9155-180">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="f9155-180">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f9155-181">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="f9155-181">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="f9155-182">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="f9155-182">Additional Resources</span></span>

<span data-ttu-id="f9155-183">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f9155-183">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[POSTGRESQL01]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png
[POSTGRESQL02]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png
[POSTGRESQL03]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-03.png
[POSTGRESQL04]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-04.png
[POSTGRESQL05]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-05.png
