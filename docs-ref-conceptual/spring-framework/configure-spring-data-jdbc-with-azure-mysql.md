---
title: Azure MySQL で Spring Data JDBC を使用する方法
description: Azure MySQL データベースで Spring Data JDBC を使用する方法を説明します。
services: mysql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 5ec89194c72cef81c79d21382e41b33ebe91d3d0
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992330"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-mysql"></a><span data-ttu-id="be96c-103">Azure MySQL で Spring Data JDBC を使用する方法</span><span class="sxs-lookup"><span data-stu-id="be96c-103">How to use Spring Data JDBC with Azure MySQL</span></span>

## <a name="overview"></a><span data-ttu-id="be96c-104">概要</span><span class="sxs-lookup"><span data-stu-id="be96c-104">Overview</span></span>

<span data-ttu-id="be96c-105">この記事では、[Spring Data] を使用して、[Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使って Azure [MySQL](https://www.mysql.com/) 内の情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="be96c-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [MySQL](https://www.mysql.com/) database using [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be96c-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="be96c-106">Prerequisites</span></span>

<span data-ttu-id="be96c-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="be96c-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="be96c-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="be96c-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="be96c-109">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="be96c-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="be96c-110">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="be96c-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="be96c-111">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="be96c-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="be96c-112">[Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="be96c-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="be96c-113">[mysql](https://dev.mysql.com/downloads/) コマンドライン ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="be96c-113">The [mysql](https://dev.mysql.com/downloads/) command-line utility.</span></span>
* <span data-ttu-id="be96c-114">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="be96c-114">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-mysql-database-for-azure"></a><span data-ttu-id="be96c-115">Azure 用に MySQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="be96c-115">Create a MySQL database for Azure</span></span>

### <a name="create-a-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="be96c-116">Azure Portal を使用して MySQL データベース サーバーを作成する</span><span class="sxs-lookup"><span data-stu-id="be96c-116">Create a MySQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="be96c-117">MySQL データベースの作成に関する詳細については、「[Azure portal を使用した Azure Database for MySQL サーバーの作成](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="be96c-117">You can read more detailed information about creating MySQL databases in [Create an Azure Database for MySQL server by using the Azure portal](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).</span></span>

1. <span data-ttu-id="be96c-118">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="be96c-118">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="be96c-119">**[+ リソースの作成]** をクリックし、**[データベース]**、**[Azure Database for MySQL]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="be96c-119">Click **+Create a resource**, then **Databases**, and then click **Azure Database for MySQL**.</span></span>

   ![MySQL データベースを作成する][MYSQL01]

1. <span data-ttu-id="be96c-121">次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="be96c-121">Enter the following information:</span></span>

   - <span data-ttu-id="be96c-122">**サーバー名**: MySQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoysmysql.mysql.database.azure.com* のような完全修飾ドメイン名の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="be96c-122">**Server name**: Choose a unique name for your MySQL server; this will be used to create a fully-qualified domain name like *wingtiptoysmysql.mysql.database.azure.com*.</span></span>
   - <span data-ttu-id="be96c-123">**サブスクリプション**:使用する Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-123">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="be96c-124">**[リソース グループ]**: 新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-124">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="be96c-125">**ソースの選択**:このチュートリアルでは、`Blank` を選択して新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="be96c-125">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="be96c-126">**サーバー管理者ログイン**:データベース管理者名を指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-126">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="be96c-127">**[パスワード]** と **[パスワードの確認]**:データベース管理者のパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-127">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="be96c-128">**場所**: データベースに最も近い地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-128">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="be96c-129">**バージョン**:最新のデータベース バージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-129">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="be96c-130">**価格レベル**:このチュートリアルでは、最も安い価格レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-130">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![MySQL データベースのプロパティを作成する][MYSQL02]

1. <span data-ttu-id="be96c-132">上記の情報をすべて入力したら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="be96c-132">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="be96c-133">Azure Portal を使用して MySQL データベース サーバーのファイアウォール規則を構成する</span><span class="sxs-lookup"><span data-stu-id="be96c-133">Configure a firewall rule for your MySQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="be96c-134">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="be96c-134">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="be96c-135">**[すべてのリソース]** をクリックし、先ほど作成した MySQL データベースをクリックします。</span><span class="sxs-lookup"><span data-stu-id="be96c-135">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![MySQL データベースを選択する][MYSQL03]

1. <span data-ttu-id="be96c-137">**[接続のセキュリティ]** をクリックし、**[ファイアウォール規則]** で、規則の一意の名前を指定して新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="be96c-137">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![接続のセキュリティを構成する][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-mysql-server-using-the-azure-portal"></a><span data-ttu-id="be96c-139">Azure Portal を使用して MySQL サーバーの接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="be96c-139">Retrieve the connection string for your MySQL server using the Azure Portal</span></span>

1. <span data-ttu-id="be96c-140">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="be96c-140">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="be96c-141">**[すべてのリソース]** をクリックし、先ほど作成した MySQL データベースをクリックします。</span><span class="sxs-lookup"><span data-stu-id="be96c-141">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![MySQL データベースを選択する][MYSQL03]

1. <span data-ttu-id="be96c-143">**[接続文字列]** をクリックし、**[JDBC]** テキスト フィールド内の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="be96c-143">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![JDBC 接続文字列を取得する][MYSQL05]

### <a name="create-mysql-database-using-the-mysql-command-line-utility"></a><span data-ttu-id="be96c-145">`mysql` コマンド ライン ユーティリティを使用して MySQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="be96c-145">Create MySQL database using the `mysql` command-line utility</span></span>

1. <span data-ttu-id="be96c-146">コマンド シェルを開き、次の例のように `mysql` コマンドを入力して MySQL サーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="be96c-146">Open a command shell and connect to your MySQL server by entering a `mysql` command like the following example:</span></span>

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   <span data-ttu-id="be96c-147">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="be96c-147">Where:</span></span>

   | <span data-ttu-id="be96c-148">パラメーター</span><span class="sxs-lookup"><span data-stu-id="be96c-148">Parameter</span></span> | <span data-ttu-id="be96c-149">説明</span><span class="sxs-lookup"><span data-stu-id="be96c-149">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="be96c-150">この記事の前半の完全修飾 MySQL サーバー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-150">Specifies your fully qualified MySQL server name from earlier in this article.</span></span> |
   | `user` | <span data-ttu-id="be96c-151">この記事の前半の MySQL 管理者と短縮サーバー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-151">Specifies your MySQL administrator and shortened server name from earlier in this article.</span></span> |
   | `p` | <span data-ttu-id="be96c-152">パスワードが要求されるまで待機するように指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-152">Specifies to wait until prompted for a password.</span></span> |

   <span data-ttu-id="be96c-153">MySQL サーバーは、次の例のような表示で応答します。</span><span class="sxs-lookup"><span data-stu-id="be96c-153">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```

1. <span data-ttu-id="be96c-154">次の例のように `mysql` コマンドを入力して、*mysqldb* という名前のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="be96c-154">Create a database named *mysqldb* by entering a `mysql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   <span data-ttu-id="be96c-155">MySQL サーバーは、次の例のような表示で応答します。</span><span class="sxs-lookup"><span data-stu-id="be96c-155">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. <span data-ttu-id="be96c-156">省略可能:次の例のように、`mysql` コマンドを入力することで、データベースが作成されたことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="be96c-156">OPTIONAL: You can verify that your database was created by entering a `mysql` command like the following example:</span></span>

   ```SQL
   SHOW DATABASES;
   ```

   <span data-ttu-id="be96c-157">MySQL サーバーは、次の例のような表示で応答します。</span><span class="sxs-lookup"><span data-stu-id="be96c-157">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. <span data-ttu-id="be96c-158">`\q` を入力して `mysql` ユーティリティを終了します。</span><span class="sxs-lookup"><span data-stu-id="be96c-158">Enter `\q` to exit the `mysql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="be96c-159">サンプル アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="be96c-159">Configure the sample application</span></span>

1. <span data-ttu-id="be96c-160">コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。</span><span class="sxs-lookup"><span data-stu-id="be96c-160">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="be96c-161">サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="be96c-161">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="be96c-162">テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be96c-162">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false&serverTimezone=UTC
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   <span data-ttu-id="be96c-163">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="be96c-163">Where:</span></span>

   | <span data-ttu-id="be96c-164">パラメーター</span><span class="sxs-lookup"><span data-stu-id="be96c-164">Parameter</span></span> | <span data-ttu-id="be96c-165">説明</span><span class="sxs-lookup"><span data-stu-id="be96c-165">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="be96c-166">タイム ゾーンを追加して、この記事の前半の MySQL JDBC 文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-166">Specifies your MySQL JDBC string from earlier in this article, with the time zone added.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="be96c-167">この記事の前半の MySQL 管理者名を指定し、その後に短縮サーバー名を追加します。</span><span class="sxs-lookup"><span data-stu-id="be96c-167">Specifies your MySQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="be96c-168">この記事の前半の MySQL 管理者パスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="be96c-168">Specifies your MySQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="be96c-169">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="be96c-169">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="be96c-170">サンプル アプリケーションをパッケージ化してテストする</span><span class="sxs-lookup"><span data-stu-id="be96c-170">Package and test the sample application</span></span> 

1. <span data-ttu-id="be96c-171">サンプル アプリケーションを Maven でビルドします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="be96c-171">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P mysql
   ```

1. <span data-ttu-id="be96c-172">サンプル アプリケーションを開始します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="be96c-172">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="be96c-173">次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="be96c-173">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="be96c-174">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="be96c-174">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="be96c-175">次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="be96c-175">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="be96c-176">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="be96c-176">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="be96c-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="be96c-177">Summary</span></span>

<span data-ttu-id="be96c-178">このチュートリアルでは、Spring Data を使用して、JDBC を使って Azure MySQL データベース内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="be96c-178">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure MySQL database using JDBC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be96c-179">次の手順</span><span class="sxs-lookup"><span data-stu-id="be96c-179">Next steps</span></span>

<span data-ttu-id="be96c-180">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="be96c-180">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="be96c-181">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="be96c-181">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="be96c-182">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="be96c-182">Additional Resources</span></span>

<span data-ttu-id="be96c-183">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="be96c-183">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[MYSQL01]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png
[MYSQL03]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-03.png
[MYSQL04]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-05.png
