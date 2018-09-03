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
ms.openlocfilehash: fa93788f9b600d963f35ea599a58d309d3a3ee52
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240825"
---
# <a name="configure-microprofile-with-azure-key-vault"></a><span data-ttu-id="b6775-103">Azure Key Vault を使用した MicroProfile の構成</span><span class="sxs-lookup"><span data-stu-id="b6775-103">Configure MicroProfile with Azure Key Vault</span></span>

<span data-ttu-id="b6775-104">このチュートリアルでは、[MicroProfile 構成 API](https://microprofile.io/project/eclipse/microprofile-config) を使用して [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) からシークレットを取得して Azure Key Vault に直接接続を作成するように、[MicroProfile](http://microprofile.io) アプリケーションを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b6775-104">This tutorial will demonstrate how to configure a [MicroProfile](http://microprofile.io) application to retrieve secrets from [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) using the [MicroProfile Config APIs](https://microprofile.io/project/eclipse/microprofile-config) to create a direct connection to Azure Key Vault.</span></span> <span data-ttu-id="b6775-105">MicroProfile 構成 API を使用すると、構成データを取得し、これをご自分のマイクロサービスに挿入する際に、開発者が標準 API のメリットを活かすことができます。</span><span class="sxs-lookup"><span data-stu-id="b6775-105">By using the MicroProfile Config APIs, developers benefit from a standard API for retrieving and injecting configuration data into their microservices.</span></span>

<span data-ttu-id="b6775-106">詳しく説明する前に、Azure Key Vault と MicroProfile 構成 API を組み合わせることで、コードで何を記述できるかを簡単に確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="b6775-106">Before we dive in, lets quickly take a look at what a combination of Azure Key Vault and the MicroProfile Config API enables us to write in our code.</span></span> <span data-ttu-id="b6775-107">`@Inject` および `@ConfigProperty` の注釈が付いている、クラス内のフィールドのコード スニペットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="b6775-107">Here's a code snippet of a field in a class that has been annotated with `@Inject` and `@ConfigProperty`.</span></span> <span data-ttu-id="b6775-108">注釈で指定されている `name` は Azure Key Vault で検索するプロパティの名前で、キーが検出されない場合は、`defaultValue` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="b6775-108">The `name` specified in the annotation is the name of the property to look up in Azure Key Vault, and the `defaultValue` is what will be set if the key is not discovered.</span></span> <span data-ttu-id="b6775-109">最終的には、Azure Key Vault に格納されている値、または既定値が実行時にフィールドに自動的に挿入され、開発者の作業が簡略化されます。開発者がコンストラクターおよびセッター メソッドで値を渡す必要がなく、MicroProfile にこの処理を任すことができるからです。</span><span class="sxs-lookup"><span data-stu-id="b6775-109">The end result is that the value stored in Azure Key Vault, or the default value, will be injected automatically into the field at runtime, simplifying the life of developers as they no longer need to pass values around in constuctors and setter methods, instead leaving it to MicroProfile to handle.</span></span>

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

<span data-ttu-id="b6775-110">MicroProfile 構成に直接アクセスし、必要に応じてシークレットを要求することもできます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b6775-110">It is also possible to access the MicroProfile config directly, to request secrets as necessary, e.g.:</span></span>

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

<span data-ttu-id="b6775-111">このサンプルでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile](https://microprofile.io/) を使用して、お使いのマシンでローカルに実行できる小さな Java war ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b6775-111">This sample makes use of [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile](https://microprofile.io/) to create a tiny Java war file that can be run locally on your machine.</span></span> <span data-ttu-id="b6775-112">ここでは、コードを Docker 化したり Azure にプッシュしたりする方法については説明しませんが、このチュートリアルの最後にあるリンク セクションには、これについて説明した他の便利なチュートリアルへのリンクが記載されています。</span><span class="sxs-lookup"><span data-stu-id="b6775-112">It does not demonstrate how to dockerise or push the code to Azure, but the links section at the end of this tutorial has links to other useful tutorials that explain this.</span></span>

<span data-ttu-id="b6775-113">このサンプルでは、無料のオープン ソース ライブラリが使用されています。このライブラリによって、Azure Key Vault 用の構成ソースが (MicroProfile 構成 API を使用して) 作成されます。</span><span class="sxs-lookup"><span data-stu-id="b6775-113">This sample makes use of a free and open source library that creats a config source (using the MicroProfile Config API) for Azure Key Vault.</span></span> <span data-ttu-id="b6775-114">[プロジェクト GitHub ページ](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)では、このライブラリの詳細とコードを確認できます。</span><span class="sxs-lookup"><span data-stu-id="b6775-114">You can learn more about this library, and review the code, on the [project GitHub page](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span></span> <span data-ttu-id="b6775-115">このライブラリを使用することで、このチュートリアルのコードの焦点を、ライブラリの構成だけに絞ることができます。その後、ご自身のコードにキーを挿入します。Azure 固有のコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b6775-115">By using this library, the code in this tutorial can simply focus on configuration of the library, followed by injecting keys into your code, and we don't need to write any Azure-specific code.</span></span>

<span data-ttu-id="b6775-116">ここでは、お使いのローカル コンピューターで、このコードを実行するのに必要な手順を示します。最初は、Azure Key Vault リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="b6775-116">Here are the steps required to run this code on your local machine, starting with creating an Azure Key Vault resource.</span></span>

## <a name="creating-an-azure-key-vault-resource"></a><span data-ttu-id="b6775-117">Azure Key Vault リソースの作成</span><span class="sxs-lookup"><span data-stu-id="b6775-117">Creating an Azure Key Vault resource</span></span>

<span data-ttu-id="b6775-118">Azure CLI を使用して Azure Key Vault リソースを作成し、1 つのシークレットを設定します。</span><span class="sxs-lookup"><span data-stu-id="b6775-118">We will use the Azure CLI to create the Azure Key Vault resource and populate it with one secret.</span></span>

1. <span data-ttu-id="b6775-119">まず、Azure サービス プリンシパルを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="b6775-119">Firstly, lets create an Azure service principal.</span></span> <span data-ttu-id="b6775-120">これにより、Key Vault へのアクセスに必要なクライアント ID とキーが提供されます。</span><span class="sxs-lookup"><span data-stu-id="b6775-120">This will provide us with the client ID and key we need to access Key Vault:</span></span>

```bash
az login
az account set --subscription <subscription_id>

az ad sp create-for-rbac --name <service_principal_name>
```

<span data-ttu-id="b6775-121">たとえば、前の手順でサービス プリンシパル名として `microprofile-keyvault-service-principal` を使用したとします。</span><span class="sxs-lookup"><span data-stu-id="b6775-121">Lets say we use `microprofile-keyvault-service-principal` for the service principal name in the previous step.</span></span> <span data-ttu-id="b6775-122">これを行うと、Azure から次のようなフォームで応答があります (一部伏せ字になっています)。</span><span class="sxs-lookup"><span data-stu-id="b6775-122">The response from Azure for doing this will be in the following, slightly censored, form:</span></span>

```json
{
  "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
  "displayName": "microprofile-keyvault-service-principal",
  "name": "http://microprofile-keyvault-service-principal",
  "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
  "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
}
```

<span data-ttu-id="b6775-123">ここで注目するのは `appID` と `password` の値です。これらはそれぞれ、`client ID` および `key` として後で使用します。</span><span class="sxs-lookup"><span data-stu-id="b6775-123">Of particular note here is the `appID` and `password` values - these are what we will use later on as `client ID` and `key`, respectively.</span></span>

<span data-ttu-id="b6775-124">サービス プリンシパルを作成したので、次は必要に応じてリソース グループを作成します (作成済みの場合、この手順はスキップできます)。</span><span class="sxs-lookup"><span data-stu-id="b6775-124">Now that we have created a service principal, we can optionally create a resource group (if you already have one you want to use, you can skip this step).</span></span> <span data-ttu-id="b6775-125">リソース グループの場所を取得するために、`az account list-locations` を呼び出し、そのリストの `name` 値を使用して、リソース グループの作成先を指定できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b6775-125">Note that to get a list of resource group locations, you can call `az account list-locations` and use the `name` value from that list to specify where the resource group should be created.</span></span>

```bash
# For this tutorial, the author chose to use `westus`
# and `jg-test` for the resource group name.
az group create -l <resource_group_location> -n <resource_group_name>
```

<span data-ttu-id="b6775-126">ここで、Azure Key Vault リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="b6775-126">We now create an Azure Key Vault resource.</span></span> <span data-ttu-id="b6775-127">Key Vault 名は、後でキー コンテナーを参照するときに使用する名前です。したがって、覚えやすいものを選択してください。</span><span class="sxs-lookup"><span data-stu-id="b6775-127">Note that the Key Vault name is what you will use to reference the key vault later, so choose something memorable.</span></span>

```bash
az keyvault create --name <your_keyvault_name>            \
                   --resource-group <your_resource_group> \
                   --location <location>                  \
                   --enabled-for-deployment true          \
                   --enabled-for-disk-encryption true     \
                   --enabled-for-template-deployment true \
                   --sku standard
```

<span data-ttu-id="b6775-128">また、Key Vault シークレットにアクセスできるように、前に作成したサービス プリンシパルに適切なアクセス許可を付与する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="b6775-128">We also need to grant the appropriate permissions to the service principal we created earlier, so that it may access the Key Vault secrets.</span></span> <span data-ttu-id="b6775-129">appID 値は、サービス プリンシパルを作成した上記の場所の `appId` 値です (つまり、`5292398e-XXXX-40ce-XXXX-d49fXXXX9e79`。ただし、ターミナル出力の値を使用)。</span><span class="sxs-lookup"><span data-stu-id="b6775-129">Note that the appID value is the `appId` value from above where we created the service principal (that is, `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79` - but use the value from your terminal output).</span></span>

```bash
az keyvault set-policy --name <your_keyvault_name>   \
                       --secret-permission get list  \
                       --spn <your_sp_appId_created_in_step1>
```

<span data-ttu-id="b6775-130">これでシークレットを Key Vault にプッシュできます。</span><span class="sxs-lookup"><span data-stu-id="b6775-130">We are now at the point where we can push a secret into Key Vault.</span></span> <span data-ttu-id="b6775-131">キー名 `demo-key` を使用して、そのキーの値を `demo-value` に設定します。</span><span class="sxs-lookup"><span data-stu-id="b6775-131">Lets use the key name `demo-key`, and set the value of the key to be `demo-value`:</span></span>

```bash
az keyvault secret set --name demo-key      \
                       --value demo-value   \
                       --vault-name <your_keyvault_name>  
```

<span data-ttu-id="b6775-132">これで完了です。</span><span class="sxs-lookup"><span data-stu-id="b6775-132">That's it!</span></span> <span data-ttu-id="b6775-133">現在、1 つのシークレットを使用して Azure で Key Vault が実行されています。</span><span class="sxs-lookup"><span data-stu-id="b6775-133">We now have Key Vault running in Azure with a single secret.</span></span> <span data-ttu-id="b6775-134">これで、このリポジトリを複製し、アプリでこのリソースを使用するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="b6775-134">We can now clone this repo and configure it to use this resource in our app.</span></span>

## <a name="getting-up-and-running-locally"></a><span data-ttu-id="b6775-135">ローカルでの起動と実行</span><span class="sxs-lookup"><span data-stu-id="b6775-135">Getting up and running locally</span></span>

<span data-ttu-id="b6775-136">この例は、GitHub で入手できるサンプル アプリケーションに基づいています。そこで、この例を複製して、コードをステップ実行します。</span><span class="sxs-lookup"><span data-stu-id="b6775-136">This example is based on a sample application available on GitHub, so we will clone that and then step through the code.</span></span> <span data-ttu-id="b6775-137">お使いのマシンにコードを複製するには、次の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="b6775-137">Follow the steps below to get the code cloned onto your machine:</span></span>

1. `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

1. `cd microprofile-configsource-keyvault`

1. <span data-ttu-id="b6775-138">`src/main/resources/META-INF/microprofile-config.properties` に移動し、上記の詳細を含む microprofile config.properties ファイルでプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="b6775-138">Navigate to `src/main/resources/META-INF/microprofile-config.properties` and change the properties in microprofile-config.properties file with details from above.</span></span>

1. <span data-ttu-id="b6775-139">`mvn clean package payara-micro:start` を使用して、サーバーを実行してみます</span><span class="sxs-lookup"><span data-stu-id="b6775-139">Try running the server using `mvn clean package payara-micro:start`</span></span>

1. <span data-ttu-id="b6775-140">ご自身の Web ブラウザーで [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) にアクセスしてみます。Azure Key Vault から読み取られた値を示すシンプルな応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b6775-140">Try accessing [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) in your web browser - you should see a simple response that demonstrates values being read from Azure Key Vault.</span></span>

## <a name="summary"></a><span data-ttu-id="b6775-141">まとめ</span><span class="sxs-lookup"><span data-stu-id="b6775-141">Summary</span></span>

<span data-ttu-id="b6775-142">このサンプル アプリケーションでは、MicroProfile 構成 API、Azure Key Vault、および無料のオープン ソース [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) ライブラリを一緒に処理して、構成データとシークレットを MicroProfile Web サービスに簡単に挿入できるようにします。</span><span class="sxs-lookup"><span data-stu-id="b6775-142">This sample application bakes together the MicroProfile Config API, Azure Key Vault, and the free and open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) library to enable easy injection of configuration data and secrets into our MicroProfile web services.</span></span>
