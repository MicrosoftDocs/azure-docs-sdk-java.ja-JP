---
title: Azure PostgreSQL で Spring Data JPA を使用する方法
description: Azure PostgreSQL データベースで Spring Data JPA を使用する方法を説明します。
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
ms.openlocfilehash: 1849e03674534882a579f85b2654a628bd867487
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992343"
---
# <a name="how-to-use-spring-data-jpa-with-azure-postgresql"></a><span data-ttu-id="de093-103">Azure PostgreSQL で Spring Data JPA を使用する方法</span><span class="sxs-lookup"><span data-stu-id="de093-103">How to use Spring Data JPA with Azure PostgreSQL</span></span>

## <a name="overview"></a><span data-ttu-id="de093-104">概要</span><span class="sxs-lookup"><span data-stu-id="de093-104">Overview</span></span>

<span data-ttu-id="de093-105">この記事では、[Spring Data] を使用して、[Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) を使って Azure [PostgreSQL]https://www.postgresql.org/ データベース内の情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="de093-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [PostgreSQL]https://www.postgresql.org/ database using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de093-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="de093-106">Prerequisites</span></span>

<span data-ttu-id="de093-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="de093-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="de093-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="de093-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="de093-109">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="de093-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="de093-110">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="de093-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="de093-111">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="de093-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="de093-112">[Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="de093-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span> <span data-ttu-id="de093-113">または機能をテストするための類似の HTTP ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="de093-113">or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="de093-114">[psql](https://www.postgresql.org/docs/current/app-psql.html) コマンドライン ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="de093-114">The [psql](https://www.postgresql.org/docs/current/app-psql.html) command-line utility.</span></span>
* <span data-ttu-id="de093-115">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="de093-115">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-postgresql-database-for-azure"></a><span data-ttu-id="de093-116">Azure 用に PostgreSQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="de093-116">Create a PostgreSQL database for Azure</span></span>

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="de093-117">Azure Portal を使用して PostgreSQL データベース サーバーを作成する</span><span class="sxs-lookup"><span data-stu-id="de093-117">Create a PostgreSQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="de093-118">PostgreSQL データベースの作成に関する詳細については、[Azure Portal を使用した Azure Database for PostgreSQL サーバーの作成](/azure/postgresql/quickstart-create-server-database-portal)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="de093-118">You can read more detailed information about creating PostgreSQL databases in [Create an Azure Database for PostgreSQL server by using the Azure portal](/azure/postgresql/quickstart-create-server-database-portal).</span></span>

1. <span data-ttu-id="de093-119">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="de093-119">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="de093-120">**[+ リソースの作成]** をクリックし、**[データベース]**、**[Azure Database for PostgreSQL]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="de093-120">Click **+Create a resource**, then **Databases**, and then click **Azure Database for PostgreSQL**.</span></span>

   ![PostgreSQL データベースを作成する][POSTGRESQL01]

1. <span data-ttu-id="de093-122">次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="de093-122">Enter the following information:</span></span>

   - <span data-ttu-id="de093-123">**サーバー名**: PostgreSQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoyspostgresql.postgres.database.azure.com* のような完全修飾ドメイン名の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="de093-123">**Server name**: Choose a unique name for your PostgreSQL server; this will be used to create a fully-qualified domain name like *wingtiptoyspostgresql.postgres.database.azure.com*.</span></span>
   - <span data-ttu-id="de093-124">**サブスクリプション**:使用する Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-124">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="de093-125">**[リソース グループ]**: 新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-125">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="de093-126">**ソースの選択**:このチュートリアルでは、`Blank` を選択して新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="de093-126">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="de093-127">**サーバー管理者ログイン**:データベース管理者名を指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-127">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="de093-128">**[パスワード]** と **[パスワードの確認]**:データベース管理者のパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-128">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="de093-129">**場所**: データベースに最も近い地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-129">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="de093-130">**バージョン**:最新のデータベース バージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-130">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="de093-131">**価格レベル**:このチュートリアルでは、最も安い価格レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-131">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![PostgreSQL データベースのプロパティを作成する][POSTGRESQL02]

1. <span data-ttu-id="de093-133">上記の情報をすべて入力したら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de093-133">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="de093-134">Azure Portal を使用して PostgreSQL データベース サーバーのファイアウォール規則を構成する</span><span class="sxs-lookup"><span data-stu-id="de093-134">Configure a firewall rule for your PostgreSQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="de093-135">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="de093-135">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="de093-136">**[すべてのリソース]** をクリックし、先ほど作成した PostgreSQL データベースをクリックします。</span><span class="sxs-lookup"><span data-stu-id="de093-136">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![PostgreSQL データベースを選択する][POSTGRESQL03]

1. <span data-ttu-id="de093-138">**[接続のセキュリティ]** をクリックし、**[ファイアウォール規則]** で、規則の一意の名前を指定して新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de093-138">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![接続のセキュリティを構成する][POSTGRESQL04]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a><span data-ttu-id="de093-140">Azure Portal を使用して PostgreSQL サーバーの接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="de093-140">Retrieve the connection string for your PostgreSQL server using the Azure Portal</span></span>

1. <span data-ttu-id="de093-141">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="de093-141">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="de093-142">**[すべてのリソース]** をクリックし、先ほど作成した PostgreSQL データベースをクリックします。</span><span class="sxs-lookup"><span data-stu-id="de093-142">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![PostgreSQL データベースを選択する][POSTGRESQL03]

1. <span data-ttu-id="de093-144">**[接続文字列]** をクリックし、**[JDBC]** テキスト フィールド内の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="de093-144">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![JDBC 接続文字列を取得する][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a><span data-ttu-id="de093-146">`psql` コマンド ライン ユーティリティを使用して PostgreSQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="de093-146">Create PostgreSQL database using the `psql` command-line utility</span></span>

1. <span data-ttu-id="de093-147">コマンド シェルを開き、次の例のように `psql` コマンドを入力して PostgreSQL サーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="de093-147">Open a command shell and connect to your PostgreSQL server by entering a `psql` command like the following example:</span></span>

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   <span data-ttu-id="de093-148">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="de093-148">Where:</span></span>

   | <span data-ttu-id="de093-149">パラメーター</span><span class="sxs-lookup"><span data-stu-id="de093-149">Parameter</span></span> | <span data-ttu-id="de093-150">説明</span><span class="sxs-lookup"><span data-stu-id="de093-150">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="de093-151">この記事の前半の完全修飾 PostgreSQL サーバー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-151">Specifies your fully qualified PostgreSQL server name from earlier in this article.</span></span> |
   | `host` | <span data-ttu-id="de093-152">PostgreSQL サーバー ポートを指定します。既定では `5432` です。</span><span class="sxs-lookup"><span data-stu-id="de093-152">Specifies the PostgreSQL server port, which is `5432` by default.</span></span> |
   | `username` | <span data-ttu-id="de093-153">この記事の前半の PostgreSQL 管理者と短縮サーバー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-153">Specifies your PostgreSQL administrator and shortened server name from earlier in this article.</span></span> |
   | `dbname` | <span data-ttu-id="de093-154">差し当たり既定の `postgres` データベースを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-154">Specifies that you want to use the default `postgres` database for now.</span></span> |

   <span data-ttu-id="de093-155">PostgreSQL サーバーは、次の例のような表示で応答します。</span><span class="sxs-lookup"><span data-stu-id="de093-155">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```

1. <span data-ttu-id="de093-156">次の例のように `psql` コマンドを入力して、*mypgsqldb* という名前のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="de093-156">Create a database named *mypgsqldb* by entering a `psql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   <span data-ttu-id="de093-157">PostgreSQL サーバーは、次の例のような表示で応答します。</span><span class="sxs-lookup"><span data-stu-id="de093-157">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   CREATE DATABASE
   ```

1. <span data-ttu-id="de093-158">省略可能:データベースが作成されたことは、`psql` で `\l` を入力して確認できます。PostgreSQL サーバーは次の例のように応答します。</span><span class="sxs-lookup"><span data-stu-id="de093-158">OPTIONAL: You can verify that your database was created by entering a `\l` at the `psql`; your PostgreSQL server should respond with something like the following example:</span></span>

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

1. <span data-ttu-id="de093-159">`\q` を入力して `psql` ユーティリティを終了します。</span><span class="sxs-lookup"><span data-stu-id="de093-159">Enter `\q` to exit the `psql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="de093-160">サンプル アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="de093-160">Configure the sample application</span></span>

1. <span data-ttu-id="de093-161">コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。</span><span class="sxs-lookup"><span data-stu-id="de093-161">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jpa-on-azure.git
   ```

1. <span data-ttu-id="de093-162">サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="de093-162">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="de093-163">テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="de093-163">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   <span data-ttu-id="de093-164">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="de093-164">Where:</span></span>

   | <span data-ttu-id="de093-165">パラメーター</span><span class="sxs-lookup"><span data-stu-id="de093-165">Parameter</span></span> | <span data-ttu-id="de093-166">説明</span><span class="sxs-lookup"><span data-stu-id="de093-166">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="de093-167">この記事の前半の PostgreSQL JDBC 文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-167">Specifies your PostgreSQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="de093-168">この記事の前半の PostgreSQL 管理者名を指定し、その後に短縮サーバー名を追加します。</span><span class="sxs-lookup"><span data-stu-id="de093-168">Specifies your PostgreSQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="de093-169">この記事の前半の PostgreSQL 管理者パスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="de093-169">Specifies your PostgreSQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="de093-170">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="de093-170">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="de093-171">サンプル アプリケーションをパッケージ化してテストする</span><span class="sxs-lookup"><span data-stu-id="de093-171">Package and test the sample application</span></span> 

1. <span data-ttu-id="de093-172">サンプル アプリケーションを Maven でビルドします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="de093-172">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P postgresql
   ```

1. <span data-ttu-id="de093-173">サンプル アプリケーションを開始します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="de093-173">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="de093-174">次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="de093-174">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="de093-175">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="de093-175">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="de093-176">次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="de093-176">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="de093-177">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="de093-177">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="de093-178">まとめ</span><span class="sxs-lookup"><span data-stu-id="de093-178">Summary</span></span>

<span data-ttu-id="de093-179">このチュートリアルでは、Spring Data を使用して、JPA を使って Azure PostgreSQL データベース 内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="de093-179">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure PostgreSQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de093-180">次の手順</span><span class="sxs-lookup"><span data-stu-id="de093-180">Next steps</span></span>

<span data-ttu-id="de093-181">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="de093-181">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="de093-182">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="de093-182">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="de093-183">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="de093-183">Additional Resources</span></span>

<span data-ttu-id="de093-184">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="de093-184">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[POSTGRESQL01]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-01.png
[POSTGRESQL02]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-02.png
[POSTGRESQL03]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-03.png
[POSTGRESQL04]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-04.png
[POSTGRESQL05]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-05.png
