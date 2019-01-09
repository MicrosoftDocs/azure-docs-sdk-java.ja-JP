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
# <a name="how-to-use-spring-data-apache-cassandra-api-with-azure-cosmos-db"></a><span data-ttu-id="c71db-103">Azure Cosmos DB で Spring Data Apache Cassandra API を使用する方法</span><span class="sxs-lookup"><span data-stu-id="c71db-103">How to use Spring Data Apache Cassandra API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="c71db-104">概要</span><span class="sxs-lookup"><span data-stu-id="c71db-104">Overview</span></span>

<span data-ttu-id="c71db-105">この記事では、[Spring Data] を使用して、[Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction) を使って情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="c71db-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c71db-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="c71db-106">Prerequisites</span></span>

<span data-ttu-id="c71db-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="c71db-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="c71db-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="c71db-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="c71db-109">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="c71db-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="c71db-110">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c71db-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="c71db-111">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="c71db-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="c71db-112">[Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="c71db-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="c71db-113">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="c71db-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="c71db-114">Azure Cosmos DB アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="c71db-114">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="c71db-115">Azure Portal を使用して Cosmos DB アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="c71db-115">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="c71db-116">Azure Cosmos DB アカウントの作成に関する詳細については、[Azure Cosmos DB のドキュメント](/azure/cosmos-db/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c71db-116">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="c71db-117">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="c71db-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="c71db-118">**[+ リソースの作成]** をクリックし、**[データベース]**、**[Azure Cosmos DB]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="c71db-118">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure Cosmos DB アカウントを作成する][COSMOSDB01]

1. <span data-ttu-id="c71db-120">次の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-120">Specify the following information:</span></span>

   - <span data-ttu-id="c71db-121">**サブスクリプション**:使用する Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-121">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="c71db-122">**[リソース グループ]**: 新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-122">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="c71db-123">**アカウント名**:Cosmos DB アカウント用に一意の名前を選択します。この名前は、*wingtiptoyscassandra.documents.azure.com* のような完全修飾ドメイン名の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="c71db-123">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoyscassandra.documents.azure.com*.</span></span>
   - <span data-ttu-id="c71db-124">**API**:このチュートリアルでは、`Cassandra` を指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-124">**API**: Specify `Cassandra` for this tutorial.</span></span>
   - <span data-ttu-id="c71db-125">**場所**: データベースに最も近い地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-125">**Location**: Specify the closest geographic region for your database.</span></span>

   ![Cosmos DB アカウントの設定を指定する][COSMOSDB02]
   
1. <span data-ttu-id="c71db-127">上記の情報をすべて入力したら、**[レビュー + 作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c71db-127">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="c71db-128">レビュー ページに問題がなければ、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c71db-128">If everything looks correct on the review page, click **Create**.</span></span>

   ![Cosmos DB アカウントの設定を確認する][COSMOSDB03]

### <a name="add-a-keyspace-to-your-azure-cosmos-db-account"></a><span data-ttu-id="c71db-130">Azure Cosmos DB アカウントにキースペースを追加する</span><span class="sxs-lookup"><span data-stu-id="c71db-130">Add a keyspace to your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="c71db-131">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="c71db-131">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="c71db-132">**[すべてのリソース]** をクリックしてから、先ほど作成した Azure Cosmos DB アカウントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c71db-132">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![Azure Cosmos DB アカウントを選択する][COSMOSDB04]

1. <span data-ttu-id="c71db-134">**[データ エクスプローラー]** をクリックし、**[New Keyspace]\(新しいキースペース\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c71db-134">Click **Data Explorer**, then click **New Keyspace**.</span></span> <span data-ttu-id="c71db-135">**[Keyspace id]\(キースペース ID\)** に一意の識別子を入力し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c71db-135">Enter a unique identifier for your **Keyspace id**, then click **OK**.</span></span>

   ![Cosmos DB キースペースを作成する][COSMOSDB05]

### <a name="retrieve-the-connection-settings-for-your-azure-cosmos-db-account"></a><span data-ttu-id="c71db-137">Azure Cosmos DB アカウントの接続の設定を取得する</span><span class="sxs-lookup"><span data-stu-id="c71db-137">Retrieve the connection settings for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="c71db-138">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="c71db-138">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="c71db-139">**[すべてのリソース]** をクリックしてから、先ほど作成した Azure Cosmos DB アカウントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c71db-139">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![Azure Cosmos DB アカウントを選択する][COSMOSDB04]

1. <span data-ttu-id="c71db-141">**[接続文字列]** をクリックし、**[コンタクト ポイント]**、**[ポート]**、**[ユーザー名]**、および **[プライマリ パスワード]** の各フィールドの値をコピーします。これらの値は、アプリケーションを構成するために後で使用します。</span><span class="sxs-lookup"><span data-stu-id="c71db-141">Click **Connection strings**, and copy the values for the **Contact Point**, **Port**, **Username**, and **Primary Password** fields; you will use those values to configure your application later.</span></span>

   ![Cosmos DB の接続設定を取得する][COSMOSDB05]

## <a name="configure-the-sample-application"></a><span data-ttu-id="c71db-143">サンプル アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="c71db-143">Configure the sample application</span></span>

1. <span data-ttu-id="c71db-144">コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。</span><span class="sxs-lookup"><span data-stu-id="c71db-144">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-cassandra-on-azure.git
   ```

1. <span data-ttu-id="c71db-145">サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="c71db-145">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="c71db-146">テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c71db-146">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.cassandra.contact-points=wingtiptoyscassandra.cassandra.cosmosdb.azure.com
   spring.data.cassandra.port=10350
   spring.data.cassandra.username=wingtiptoyscassandra
   spring.data.cassandra.password=********
   ```
   <span data-ttu-id="c71db-147">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="c71db-147">Where:</span></span>

   | <span data-ttu-id="c71db-148">パラメーター</span><span class="sxs-lookup"><span data-stu-id="c71db-148">Parameter</span></span> | <span data-ttu-id="c71db-149">説明</span><span class="sxs-lookup"><span data-stu-id="c71db-149">Description</span></span> |
   |---|---|
   | `spring.data.cassandra.contact-points` | <span data-ttu-id="c71db-150">この記事の前半の**コンタクト ポイント**を指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-150">Specifies the **Contact Point** from earlier in this article.</span></span> |
   | `spring.data.cassandra.port` | <span data-ttu-id="c71db-151">この記事の前半の**ポート**を指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-151">Specifies the **Port** from earlier in this article.</span></span> |
   | `spring.data.cassandra.username` | <span data-ttu-id="c71db-152">この記事の前半の**ユーザー名**を指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-152">Specifies your **Username** from earlier in this article.</span></span> |
   | `spring.data.cassandra.password` | <span data-ttu-id="c71db-153">この記事の前半の**プライマリ パスワード**を指定します。</span><span class="sxs-lookup"><span data-stu-id="c71db-153">Specifies your **Primary Password** from earlier in this article.</span></span> |

1. <span data-ttu-id="c71db-154">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="c71db-154">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="c71db-155">サンプル アプリケーションをパッケージ化してテストする</span><span class="sxs-lookup"><span data-stu-id="c71db-155">Package and test the sample application</span></span> 

1. <span data-ttu-id="c71db-156">サンプル アプリケーションを Maven でビルドします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c71db-156">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="c71db-157">サンプル アプリケーションを開始します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c71db-157">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-cassandra-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="c71db-158">次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="c71db-158">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="c71db-159">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="c71db-159">Your application should return values like the following:</span></span>

   ```shell
   Added Pet{id=60fa8cb0-0423-11e9-9a70-39311962166b, name='dog', species='canine'}.

   Added Pet{id=72c1c9e0-0423-11e9-9a70-39311962166b, name='cat', species='feline'}.
   ```

1. <span data-ttu-id="c71db-160">次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="c71db-160">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="c71db-161">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="c71db-161">Your application should return values like the following:</span></span>

   ```json
   [{"id":"60fa8cb0-0423-11e9-9a70-39311962166b","name":"dog","species":"canine"},{"id":"72c1c9e0-0423-11e9-9a70-39311962166b","name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="c71db-162">まとめ</span><span class="sxs-lookup"><span data-stu-id="c71db-162">Summary</span></span>

<span data-ttu-id="c71db-163">このチュートリアルでは、Spring Data を使用して、Azure Cosmos DB Cassandra API を使って情報を格納および取得する Java のサンプル アプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="c71db-163">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB Cassandra API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c71db-164">次の手順</span><span class="sxs-lookup"><span data-stu-id="c71db-164">Next steps</span></span>

<span data-ttu-id="c71db-165">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="c71db-165">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c71db-166">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="c71db-166">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="c71db-167">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="c71db-167">Additional Resources</span></span>

<span data-ttu-id="c71db-168">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c71db-168">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[COSMOSDB01]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB05]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05.png
