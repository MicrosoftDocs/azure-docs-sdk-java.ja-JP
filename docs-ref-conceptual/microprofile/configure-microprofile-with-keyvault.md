---
title: Azure Key Vault を使用した MicroProfile の構成
description: Azure Key Vault を使用してシークレットを MicroProfile Web サービスに挿入する方法について説明します
services: key-vault
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c405711813176823f2ddee6b4002f75c2b25fdb5
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533611"
---
# <a name="configure-microprofile-by-using-azure-key-vault"></a><span data-ttu-id="94696-103">Azure Key Vault を使用した MicroProfile の構成</span><span class="sxs-lookup"><span data-stu-id="94696-103">Configure MicroProfile by using Azure Key Vault</span></span>

<span data-ttu-id="94696-104">この記事では、[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) からシークレットを取得するように [MicroProfile](http://microprofile.io) アプリケーションを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="94696-104">This article demonstrates how to configure a [MicroProfile](http://microprofile.io) application to retrieve secrets from an [Azure key vault](https://azure.microsoft.com/services/key-vault/).</span></span> <span data-ttu-id="94696-105">このプロセスでは、[MicroProfile 構成 API](https://microprofile.io/project/eclipse/microprofile-config) を使用して Azure Key Vault への直接接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="94696-105">In this process, you use the [MicroProfile Config APIs](https://microprofile.io/project/eclipse/microprofile-config) to create a direct connection to an Azure key vault.</span></span> <span data-ttu-id="94696-106">MicroProfile 構成 API を使用すると、開発者は構成データを取得し、これを自分のマイクロサービスに挿入するための標準 API のメリットを活用することができます。</span><span class="sxs-lookup"><span data-stu-id="94696-106">As a developer, by using the MicroProfile Config APIs, you benefit from a standard API for retrieving and injecting configuration data into their microservices.</span></span>

<span data-ttu-id="94696-107">始める前に、Azure Key Vault と MicroProfile 構成 API を組み合わせることで、コードで何を記述できるかを簡単に確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="94696-107">Before you start, take a quick look at what a combination of Azure Key Vault and the MicroProfile Config API enables you to write in your code.</span></span> <span data-ttu-id="94696-108">次は、`@Inject` および `@ConfigProperty` の注釈が付いているクラス内のフィールドのコード スニペットです。</span><span class="sxs-lookup"><span data-stu-id="94696-108">The following code snippet is of a field in a class that's annotated with `@Inject` and `@ConfigProperty`.</span></span> <span data-ttu-id="94696-109">注釈で指定されている *name* はご自分のキー コンテナーで検索するプロパティの名前で、*defaultValue* はキーが検出されない場合に設定されます。</span><span class="sxs-lookup"><span data-stu-id="94696-109">The *name* that's specified in the annotation is the name of the property to look up in your key vault, and the *defaultValue* is what will be set if the key is not discovered.</span></span> <span data-ttu-id="94696-110">その結果、キー コンテナーに格納されている値 (既定値) が実行時に自動的にフィールドに挿入されます。</span><span class="sxs-lookup"><span data-stu-id="94696-110">The result is that the value that's stored in the key vault, or the default value, is injected automatically into the field at runtime.</span></span> <span data-ttu-id="94696-111">このアクションにより、コンストラクターと setter メソッドで値を渡す必要がなくなるので、作業が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="94696-111">This action can simplify your life, because you no longer need to pass values around in constructors and setter methods.</span></span> <span data-ttu-id="94696-112">代わりに、MicroProfile によってこのタスクが処理されます。</span><span class="sxs-lookup"><span data-stu-id="94696-112">Instead, MicroProfile handles this task.</span></span>

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

<span data-ttu-id="94696-113">必要に応じてシークレットを要求するため、MicroProfile 構成に直接アクセスすることもできます。</span><span class="sxs-lookup"><span data-stu-id="94696-113">To request secrets, as necessary, you can also access the MicroProfile config directly.</span></span> <span data-ttu-id="94696-114">例:</span><span class="sxs-lookup"><span data-stu-id="94696-114">For example:</span></span>

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

<span data-ttu-id="94696-115">このサンプル コードでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile](https://microprofile.io/) を使用して、お使いのマシンでローカルに実行できる小さな Java web アプリ要件 (WAR) ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="94696-115">This sample code uses [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile](https://microprofile.io/) to create a tiny Java web application requirement (WAR) file that you can run locally on your machine.</span></span> <span data-ttu-id="94696-116">ここでは、コードを Docker 化したり Azure にプッシュしたりする方法については説明しませんが、この記事の最後にあるセクションには、これについて説明した他の便利なチュートリアルへのリンクが記載されています。</span><span class="sxs-lookup"><span data-stu-id="94696-116">It doesn't demonstrate how to Dockerize or push the code to Azure, but the section at the end of this article has links to other useful tutorials that explain this.</span></span>

<span data-ttu-id="94696-117">このサンプルでは、ご使用のキー コンテナー内に (MicroProfile 構成 API を使用して) 構成ソースを作成する無料のオープン ソース ライブラリが使用されています。</span><span class="sxs-lookup"><span data-stu-id="94696-117">The sample uses a free and open source library that creates a config source (using the MicroProfile Config API) in your key vault.</span></span> <span data-ttu-id="94696-118">このライブラリの詳細とコードを確認するには、[プロジェクト GitHub ページ](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="94696-118">To learn more about this library and review the code, see the [project GitHub page](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span></span> <span data-ttu-id="94696-119">ライブラリを使用する場合、このチュートリアルのコードを使用すると、ライブラリの構成だけに集中して、その後でキーを自分のコードに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="94696-119">If you use the library, the code in this tutorial can simply focus on the configuration of the library and then inject keys into your code.</span></span> <span data-ttu-id="94696-120">Azure 固有のコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="94696-120">You don't need to write any Azure-specific code.</span></span>

<span data-ttu-id="94696-121">ご使用のローカル コンピューターでこのコードを実行するには、次のセクションの手順に従って、キー コンテナー リソースの作成を始めます。</span><span class="sxs-lookup"><span data-stu-id="94696-121">To run this code on your local machine, starting with creating a key vault resource, follow the instructions in the next sections.</span></span>

## <a name="create-a-key-vault-resource"></a><span data-ttu-id="94696-122">キー コンテナー リソースを作成する</span><span class="sxs-lookup"><span data-stu-id="94696-122">Create a key vault resource</span></span>

<span data-ttu-id="94696-123">このセクションでは、Azure CLI を使用してキー コンテナー リソースを作成し、1 つのシークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="94696-123">In this section, you use the Azure CLI to create a key vault resource and populate it with one secret.</span></span>

1. <span data-ttu-id="94696-124">Azure サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="94696-124">Create an Azure service principal.</span></span> <span data-ttu-id="94696-125">このステップにより、ご自分のキー コンテナーにアクセスするのに必要なクライアント ID とキーが提供されます。</span><span class="sxs-lookup"><span data-stu-id="94696-125">This step gives you the client ID and key that you need to access your key vault:</span></span>

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    <span data-ttu-id="94696-126">*microprofile-keyvault-service-principal* を使用して、前のステップの *\<service_principal_name>* を置き換えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="94696-126">Let's use *microprofile-keyvault-service-principal* to replace *\<service_principal_name>* in the preceding step.</span></span> <span data-ttu-id="94696-127">Azure からの応答は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="94696-127">The response from Azure would be similar to the following:</span></span>

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    <span data-ttu-id="94696-128">ここでは注目すべきは、*appId* と *password* の値です。</span><span class="sxs-lookup"><span data-stu-id="94696-128">Of particular note here are the *appId* and *password* values.</span></span> <span data-ttu-id="94696-129">これらは、この記事で後ほど*クライアント ID*と*キー*として使用します。</span><span class="sxs-lookup"><span data-stu-id="94696-129">You'll use them later in this article as *client ID* and *key*.</span></span>

1. <span data-ttu-id="94696-130">(省略可能) サービス プリンシパルを作成したので、リソース グループを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="94696-130">(Optional) Now that you've created a service principal, you can create a resource group.</span></span> <span data-ttu-id="94696-131">使用するリソース グループが既にある場合は、このステップをスキップできます。</span><span class="sxs-lookup"><span data-stu-id="94696-131">If you already have a resource group that you want to use, you can skip this step.</span></span> <span data-ttu-id="94696-132">リソース グループの場所のリストを取得するには、`az account list-locations` を呼び出し、そのリストの *name* 値を使用して、リソース グループの作成先を指定できます。</span><span class="sxs-lookup"><span data-stu-id="94696-132">To get a list of resource group locations, you can call `az account list-locations` and use the *name* value from that list to specify where the resource group should be created.</span></span>

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. <span data-ttu-id="94696-133">キー コンテナー リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="94696-133">Create a key vault resource.</span></span> <span data-ttu-id="94696-134">キー コンテナー名は後でそのキー コンテナーを参照するのに使用するため、覚えやすい名前にするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="94696-134">You'll use your key vault name to refer to the key vault later, so be sure to choose a memorable name.</span></span>

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. <span data-ttu-id="94696-135">以前に作成したサービス プリンシパルに適切なアクセス許可を付与して、キー コンテナー シークレットにアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="94696-135">Grant the appropriate permissions to the service principal that you created earlier, so that it can access the key vault secrets.</span></span> <span data-ttu-id="94696-136">次のコードの appId 値は、サービス プリンシパルを作成したステップ 1 の*appId* 値です。</span><span class="sxs-lookup"><span data-stu-id="94696-136">The appId value in the following code is the *appId* value from step 1, where you created the service principal.</span></span> <span data-ttu-id="94696-137">つまり、ステップ 1 の *appId* は *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79* でしたが、ご自分のターミナルの出力からの値を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94696-137">That is, the *appId* in step 1 was *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*, but you should use the value from your own terminal output.</span></span>

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. <span data-ttu-id="94696-138">これで、キー コンテナーにシークレットをプッシュできます。</span><span class="sxs-lookup"><span data-stu-id="94696-138">Now you can push a secret into your key vault.</span></span> <span data-ttu-id="94696-139">キー名 *demo-key* を使用して、そのキーの値を *demo-value* に設定します。</span><span class="sxs-lookup"><span data-stu-id="94696-139">Use the key name *demo-key*, and set the value of the key to *demo-value*:</span></span>

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

<span data-ttu-id="94696-140">これで完了です。</span><span class="sxs-lookup"><span data-stu-id="94696-140">That's it!</span></span> <span data-ttu-id="94696-141">現在、1 つのシークレットを使用して Azure でキー コンテナーが実行されています。</span><span class="sxs-lookup"><span data-stu-id="94696-141">You now have a key vault running in Azure with a single secret.</span></span> <span data-ttu-id="94696-142">これで、このリポジトリを複製し、アプリでこのリソースを使用するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="94696-142">You can now clone this repo and configure it to use this resource in your app.</span></span>

## <a name="get-up-and-running-locally"></a><span data-ttu-id="94696-143">ローカルで起動して実行する</span><span class="sxs-lookup"><span data-stu-id="94696-143">Get up and running locally</span></span>

<span data-ttu-id="94696-144">この例は、GitHub で入手できるサンプル アプリケーションに基づいていますので、そのアプリケーションを複製し、そのコードを順番に見ていきます。</span><span class="sxs-lookup"><span data-stu-id="94696-144">This example is based on a sample application that's available on GitHub, so you'll clone that application and then step through the code.</span></span> 

1. <span data-ttu-id="94696-145">ご使用のマシンにコードを複製するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="94696-145">Clone the code onto your machine by entering the following commands:</span></span>

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. <span data-ttu-id="94696-146">*src/main/resources/META-INF/microprofile-config.properties* に移動して、*microprofile-config.properties* ファイル内のプロパティを前のステップからの値を使用して変更します。</span><span class="sxs-lookup"><span data-stu-id="94696-146">Go to *src/main/resources/META-INF/microprofile-config.properties*, and then change the properties in the *microprofile-config.properties* file by using the values from the previous steps.</span></span>

1. <span data-ttu-id="94696-147">`mvn clean package payara-micro:start` を使用して、サーバーを実行してみます。</span><span class="sxs-lookup"><span data-stu-id="94696-147">Try running the server by using `mvn clean package payara-micro:start`.</span></span>

1. <span data-ttu-id="94696-148">ご使用の Web ブラウザーで [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) にアクセスしてみます。</span><span class="sxs-lookup"><span data-stu-id="94696-148">Try accessing [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) in your web browser.</span></span> <span data-ttu-id="94696-149">キー コンテナーから読み取られた値を示すシンプルな応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="94696-149">You should see a simple response that demonstrates values that are being read from your key vault.</span></span>

## <a name="summary"></a><span data-ttu-id="94696-150">まとめ</span><span class="sxs-lookup"><span data-stu-id="94696-150">Summary</span></span>

<span data-ttu-id="94696-151">このサンプル アプリケーションでは、MicroProfile 構成 API、Azure Key Vault、無料のオープン ソース [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) ライブラリを結合して、構成データとシークレットを MicroProfile Web サービスに簡単に挿入できるようにします。</span><span class="sxs-lookup"><span data-stu-id="94696-151">This sample application combines the MicroProfile Config API, Azure Key Vault, and the free and open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) library to enable easy injection of configuration data and secrets into your MicroProfile web services.</span></span>
