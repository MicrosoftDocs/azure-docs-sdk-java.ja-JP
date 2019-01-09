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
# <a name="how-to-use-spring-data-jpa-with-azure-sql-database"></a><span data-ttu-id="37906-103">Azure SQL Database で Spring Data JPA を使用する方法</span><span class="sxs-lookup"><span data-stu-id="37906-103">How to use Spring Data JPA with Azure SQL Database</span></span>

## <a name="overview"></a><span data-ttu-id="37906-104">概要</span><span class="sxs-lookup"><span data-stu-id="37906-104">Overview</span></span>

<span data-ttu-id="37906-105">この記事では、[Spring Data] を使用して、[Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) を使って [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 内の情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="37906-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37906-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="37906-106">Prerequisites</span></span>

<span data-ttu-id="37906-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="37906-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="37906-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="37906-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="37906-109">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="37906-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="37906-110">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="37906-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="37906-111">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="37906-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="37906-112">[Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="37906-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="37906-113">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="37906-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-sql-satabase"></a><span data-ttu-id="37906-114">Azure SQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="37906-114">Create an Azure SQL Satabase</span></span>

### <a name="create-a-sql-database-server-using-the-azure-portal"></a><span data-ttu-id="37906-115">Azure Portal を使用して SQL データベース サーバーを作成する</span><span class="sxs-lookup"><span data-stu-id="37906-115">Create a SQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="37906-116">Azure SQL データベースの作成に関する詳細については、[Azure portal での Azure SQL データベースの作成](/azure/sql-database/sql-database-get-started-portal)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="37906-116">You can read more detailed information about creating Azure SQL databases in [Create an Azure SQL database in the Azure portal](/azure/sql-database/sql-database-get-started-portal).</span></span>

1. <span data-ttu-id="37906-117">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="37906-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="37906-118">**[+ リソースの作成]** をクリックし、**[データベース]**、**[SQL Database]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="37906-118">Click **+Create a resource**, then **Databases**, and then click **SQL Database**.</span></span>

   ![SQL Database の作成][SQL01]

1. <span data-ttu-id="37906-120">次の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-120">Specify the following information:</span></span>

   - <span data-ttu-id="37906-121">**データベース名**: SQL データベース用に一意の名前を選択します。このデータベースが、指定した SQL Server に後で作成されます。</span><span class="sxs-lookup"><span data-stu-id="37906-121">**Database name**: Choose a unique name for your SQL database; this will be created in the SQL server that you will specify later.</span></span>
   - <span data-ttu-id="37906-122">**サブスクリプション**:使用する Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-122">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="37906-123">**[リソース グループ]**: 新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-123">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="37906-124">**ソースの選択**:このチュートリアルでは、`Blank database` を選択して新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="37906-124">**Select source**: For this tutorial, select `Blank database` to create a new database.</span></span>

   ![SQL データベースのプロパティを指定する][SQL02]
   
1. <span data-ttu-id="37906-126">**[サーバー]**、**[新しいサーバーの作成]** の順にクリックして、次の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-126">Click **Server**, then **Create a new server**, and then specify the following information:</span></span>

   - <span data-ttu-id="37906-127">**サーバー名**: SQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoyssql.database.windows.net* のような完全修飾ドメイン名の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="37906-127">**Server name**: Choose a unique name for your SQL server; this will be used to create a fully-qualified domain name like *wingtiptoyssql.database.windows.net*.</span></span>
   - <span data-ttu-id="37906-128">**サーバー管理者ログイン**:データベース管理者名を指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-128">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="37906-129">**[パスワード]** と **[パスワードの確認]**:データベース管理者のパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-129">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="37906-130">**場所**: データベースに最も近い地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-130">**Location**: Specify the closest geographic region for your database.</span></span>

   ![SQL サーバーを指定する][SQL03]

1. <span data-ttu-id="37906-132">上記の情報をすべて入力したら、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37906-132">When you have entered all of the above information, click **Select**.</span></span>

1. <span data-ttu-id="37906-133">このチュートリアルでは、最も安い**価格レベル**を指定して、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37906-133">For this tutorial, specify the least-expensive **Pricing tier**, and then click **Create**.</span></span>

   ![SQL データベースを作成する][SQL04]

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="37906-135">Azure Portal を使用して SQL サーバーのファイアウォール規則を構成する</span><span class="sxs-lookup"><span data-stu-id="37906-135">Configure a firewall rule for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="37906-136">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="37906-136">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="37906-137">**[すべてのリソース]** をクリックし、先ほど作成した SQL サーバーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="37906-137">Click **All Resources**, then click the SQL server you just created.</span></span>

   ![SQL サーバーを選択する][SQL05]

1. <span data-ttu-id="37906-139">**[概要]** セクションで、**[ファイアウォール設定の表示]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37906-139">In the **Overview** section, click **Show firewall settings**</span></span>

   ![ファイアウォール設定の表示][SQL06]

1. <span data-ttu-id="37906-141">**[ファイアウォールと仮想ネットワーク]** セクションで、規則に一意の名前を指定することで新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="37906-141">In the **Firewalls and virtual networks** section, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![ファイアウォール設定を構成する][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="37906-143">Azure Portal を使用して SQL サーバーの接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="37906-143">Retrieve the connection string for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="37906-144">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="37906-144">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="37906-145">**[すべてのリソース]** をクリックし、先ほど作成した SQL データベースをクリックします。</span><span class="sxs-lookup"><span data-stu-id="37906-145">Click **All Resources**, then click the SQL database you just created.</span></span>

   ![SQL データベースを選択する][SQL08]

1. <span data-ttu-id="37906-147">**[接続文字列]**、**[JDBC]** の順にクリックし、JDBC のテキスト フィールド内の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="37906-147">Click **Connection strings**, then click **JDBC**, and copy the value in the JDBC text field.</span></span>

   ![JDBC 接続文字列を取得する][SQL09]

## <a name="configure-the-sample-application"></a><span data-ttu-id="37906-149">サンプル アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="37906-149">Configure the sample application</span></span>

1. <span data-ttu-id="37906-150">コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。</span><span class="sxs-lookup"><span data-stu-id="37906-150">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="37906-151">サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="37906-151">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="37906-152">テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="37906-152">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   <span data-ttu-id="37906-153">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="37906-153">Where:</span></span>

   | <span data-ttu-id="37906-154">パラメーター</span><span class="sxs-lookup"><span data-stu-id="37906-154">Parameter</span></span> | <span data-ttu-id="37906-155">説明</span><span class="sxs-lookup"><span data-stu-id="37906-155">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="37906-156">この記事の前半の SQL JDBC 文字列の編集されたバージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-156">Specifies an edited version of your SQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="37906-157">この記事の前半の SQL 管理者名を指定し、その後に短縮サーバー名を追加します。</span><span class="sxs-lookup"><span data-stu-id="37906-157">Specifies your SQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="37906-158">この記事の前半の SQL 管理者パスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="37906-158">Specifies your SQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="37906-159">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="37906-159">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="37906-160">サンプル アプリケーションをパッケージ化してテストする</span><span class="sxs-lookup"><span data-stu-id="37906-160">Package and test the sample application</span></span> 

1. <span data-ttu-id="37906-161">サンプル アプリケーションを Maven でビルドします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="37906-161">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P sql
   ```

1. <span data-ttu-id="37906-162">サンプル アプリケーションを開始します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="37906-162">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="37906-163">次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="37906-163">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="37906-164">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="37906-164">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="37906-165">次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="37906-165">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="37906-166">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="37906-166">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="37906-167">まとめ</span><span class="sxs-lookup"><span data-stu-id="37906-167">Summary</span></span>

<span data-ttu-id="37906-168">このチュートリアルでは、Spring Data を使用して、JPA を使って Azure SQL データベース内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="37906-168">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure SQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37906-169">次の手順</span><span class="sxs-lookup"><span data-stu-id="37906-169">Next steps</span></span>

<span data-ttu-id="37906-170">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="37906-170">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="37906-171">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="37906-171">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="37906-172">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="37906-172">Additional Resources</span></span>

<span data-ttu-id="37906-173">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="37906-173">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[SQL01]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-09.png
