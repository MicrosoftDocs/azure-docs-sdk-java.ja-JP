---
title: Azure Cosmos DB で Spring Data MongoDB API を使用する方法
description: Azure Cosmos DB で Spring Data MongoDB API を使用する方法を説明します。
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
ms.openlocfilehash: 55fb356820e2cc5eb8d1fe4030053d9e580583af
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992402"
---
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a><span data-ttu-id="8c7fb-103">Azure Cosmos DB で Spring Data MongoDB API を使用する方法</span><span class="sxs-lookup"><span data-stu-id="8c7fb-103">How to use Spring Data MongoDB API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="8c7fb-104">概要</span><span class="sxs-lookup"><span data-stu-id="8c7fb-104">Overview</span></span>

<span data-ttu-id="8c7fb-105">この記事では、[Spring Data] を使用して、[Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction) を使って情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c7fb-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="8c7fb-106">Prerequisites</span></span>

<span data-ttu-id="8c7fb-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="8c7fb-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="8c7fb-109">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="8c7fb-110">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="8c7fb-111">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="8c7fb-112">[Git](https://git-scm.com/downloads) クライアント。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-112">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="8c7fb-113">Azure Cosmos DB アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="8c7fb-113">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="8c7fb-114">Azure Portal を使用して Cosmos DB アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="8c7fb-114">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="8c7fb-115">Azure Cosmos DB アカウントの作成に関する詳細については、[Azure Cosmos DB のドキュメント](/azure/cosmos-db/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-115">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="8c7fb-116">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-116">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="8c7fb-117">**[+ リソースの作成]** をクリックし、**[データベース]**、**[Azure Cosmos DB]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-117">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure Cosmos DB アカウントを作成する][COSMOSDB01]

1. <span data-ttu-id="8c7fb-119">次の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-119">Specify the following information:</span></span>

   - <span data-ttu-id="8c7fb-120">**サブスクリプション**:使用する Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-120">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="8c7fb-121">**[リソース グループ]**: 新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-121">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="8c7fb-122">**アカウント名**:Cosmos DB アカウント用に一意の名前を選択します。この名前は、*wingtiptoysmongodb.documents.azure.com* のような完全修飾ドメイン名の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-122">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoysmongodb.documents.azure.com*.</span></span>
   - <span data-ttu-id="8c7fb-123">**API**:このチュートリアルでは、`Azure Cosmos DB for MongoDB API` を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-123">**API**: Specify `Azure Cosmos DB for MongoDB API` for this tutorial.</span></span>
   - <span data-ttu-id="8c7fb-124">**場所**: データベースに最も近い地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-124">**Location**: Specify the closest geographic region for your database.</span></span>

   ![Cosmos DB アカウントの設定を指定する][COSMOSDB02]
   
1. <span data-ttu-id="8c7fb-126">上記の情報をすべて入力したら、**[レビュー + 作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-126">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="8c7fb-127">レビュー ページに問題がなければ、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-127">If everything looks correct on the review page, click **Create**.</span></span>

   ![Cosmos DB アカウントの設定を確認する][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a><span data-ttu-id="8c7fb-129">Azure Cosmos DB アカウントの接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="8c7fb-129">Retrieve the connection string for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="8c7fb-130">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-130">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="8c7fb-131">**[すべてのリソース]** をクリックしてから、先ほど作成した Azure Cosmos DB アカウントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-131">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![Azure Cosmos DB アカウントを選択する][COSMOSDB04]

1. <span data-ttu-id="8c7fb-133">**[接続文字列]** をクリックし、**[プライマリ接続文字列]** フィールドの値をコピーします。この値は、アプリケーションを構成するために後で使用します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-133">Click **Connection strings**, and copy the value for the **Primary Connection String** field; you will use that value to configure your application later.</span></span>

   ![Cosmos DB の接続文字列を取得する][COSMOSDB06]

## <a name="configure-the-sample-application"></a><span data-ttu-id="8c7fb-135">サンプル アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="8c7fb-135">Configure the sample application</span></span>

1. <span data-ttu-id="8c7fb-136">コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-136">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. <span data-ttu-id="8c7fb-137">サンプル プロジェクトの*&lt;プロジェクト ルート&gt; /complete/src/main* ディレクトリに *resources* ディレクトリを作成し、この *resources* ディレクトリに *application.properties* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-137">Create a *resources* directory in the *&lt;project root&gt;/complete/src/main* directory of the sample project, and create an *application.properties* file in the *resources* directory.</span></span>

1. <span data-ttu-id="8c7fb-138">テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加して、サンプルの値を前半の該当する値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-138">Open the *application.properties* file in a text editor, and add the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   <span data-ttu-id="8c7fb-139">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="8c7fb-139">Where:</span></span>

   | <span data-ttu-id="8c7fb-140">パラメーター</span><span class="sxs-lookup"><span data-stu-id="8c7fb-140">Parameter</span></span> | <span data-ttu-id="8c7fb-141">説明</span><span class="sxs-lookup"><span data-stu-id="8c7fb-141">Description</span></span> |
   |---|---|
   | `spring.data.mongodb.database` | <span data-ttu-id="8c7fb-142">この記事の前半の Cosmos DB アカウントの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-142">Specifies the name of your Cosmos DB account from earlier in this article.</span></span> |
   | `spring.data.mongodb.uri` | <span data-ttu-id="8c7fb-143">この記事の前半の**プライマリ接続文字列**を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-143">Specifies the **Primary Connection String** from earlier in this article.</span></span> |

1. <span data-ttu-id="8c7fb-144">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-144">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="8c7fb-145">サンプル アプリケーションをパッケージ化してテストする</span><span class="sxs-lookup"><span data-stu-id="8c7fb-145">Package and test the sample application</span></span> 

1. <span data-ttu-id="8c7fb-146">Maven でサンプル アプリケーションをビルドし、テストをスキップします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-146">Build the sample application with Maven, and configure Maven to skip tests; for example:</span></span>

   ```shell
   mvn clean package -DskipTests
   ```

1. <span data-ttu-id="8c7fb-147">サンプル アプリケーションを開始します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-147">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/gs-accessing-data-mongodb-0.1.0.jar
   ```
    
   <span data-ttu-id="8c7fb-148">アプリケーションから次のような値が返されます。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-148">Your application should return values like the following:</span></span>

   ```json
   Customers found with findAll():
   -------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   
   Customer found with findByFirstName('Alice'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customers found with findByLastName('Smith'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   ```

## <a name="summary"></a><span data-ttu-id="8c7fb-149">まとめ</span><span class="sxs-lookup"><span data-stu-id="8c7fb-149">Summary</span></span>

<span data-ttu-id="8c7fb-150">このチュートリアルでは、Spring Data を使用して、Azure Cosmos DB MongoDB API を使って情報を格納および取得する Java のサンプル アプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-150">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB MongoDB API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c7fb-151">次の手順</span><span class="sxs-lookup"><span data-stu-id="8c7fb-151">Next steps</span></span>

<span data-ttu-id="8c7fb-152">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-152">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8c7fb-153">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="8c7fb-153">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="8c7fb-154">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="8c7fb-154">Additional Resources</span></span>

<span data-ttu-id="8c7fb-155">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8c7fb-155">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[COSMOSDB01]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB06]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-06.png
