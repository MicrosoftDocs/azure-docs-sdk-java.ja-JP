---
title: Azure Batch Libraries for Java
description: "Java Batch ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, Batch, 処理, スケジューリング, 長時間実行"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: batch
ms.openlocfilehash: dfc14b08ee39a1422cb54a3b9a80fa579e704499
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-batch-libraries-for-java"></a><span data-ttu-id="d2c50-104">Azure Batch Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="d2c50-104">Azure Batch libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="d2c50-105">概要</span><span class="sxs-lookup"><span data-stu-id="d2c50-105">Overview</span></span>

<span data-ttu-id="d2c50-106">[Azure Batch](/azure/batch/batch-technical-overview) を使用すると、大規模な並列コンピューティングやハイパフォーマンス コンピューティングのアプリケーションをクラウドで効率的に実行できます。</span><span class="sxs-lookup"><span data-stu-id="d2c50-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>   

<span data-ttu-id="d2c50-107">Azure Batch の概要については、「[Azure Portal で Batch アカウントを作成する](/azure/batch/batch-account-create-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d2c50-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="client-library"></a><span data-ttu-id="d2c50-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d2c50-108">Client library</span></span>

<span data-ttu-id="d2c50-109">Azure Batch クライアント ライブラリを使用すると、コンピューティング ノードとプールを構成したり、タスクを定義してそれらをジョブで実行するように構成したり、ジョブ マネージャーを設定してジョブの実行を制御または監視したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="d2c50-109">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="d2c50-110">これらのオブジェクトを使って大規模な並列コンピューティング ソリューションを実行する方法について詳しくは、[こちら](/azure/batch/batch-api-basics)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d2c50-110">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

<span data-ttu-id="d2c50-111">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="d2c50-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="d2c50-112">例</span><span class="sxs-lookup"><span data-stu-id="d2c50-112">Example</span></span>

<span data-ttu-id="d2c50-113">Batch アカウントで Linux コンピューティング ノードのプールを設定する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d2c50-113">Set up a pool of Linux compute nodes in a batch account:</span></span>

```java
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://fabrikambatch.eastus.batch.azure.com", "fabrikambatch", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d2c50-114">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="d2c50-114">Explore the Client APIs</span></span>](/java/api/overview/azure/batch/clientlibrary)


## <a name="management-api"></a><span data-ttu-id="d2c50-115">Management API</span><span class="sxs-lookup"><span data-stu-id="d2c50-115">Management API</span></span>

<span data-ttu-id="d2c50-116">Batch アカウントの作成と削除、Batch アカウント キーの読み取りと再生成、Batch アカウントのストレージ管理は、Azure Batch 管理ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="d2c50-116">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

<span data-ttu-id="d2c50-117">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="d2c50-117">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-batch</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="d2c50-118">例</span><span class="sxs-lookup"><span data-stu-id="d2c50-118">Example</span></span>

<span data-ttu-id="d2c50-119">Azure Batch アカウントを作成し、そのアカウントで使用する新しいアプリケーションと Azure ストレージ アカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="d2c50-119">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```java
BatchAccount batchAccount = azure.batchAccounts().define("newBatchAcct")
    .withRegion(Region.US_EAST)
    .withNewResourceGroup("myResourceGroup")
    .defineNewApplication("batchAppName")
        .defineNewApplicationPackage(applicationPackageName)
        .withAllowUpdates(true)
        .withDisplayName(applicationDisplayName)
        .attach()
    .withNewStorageAccount("batchStorageAcct")
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d2c50-120">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="d2c50-120">Explore the Management APIs</span></span>](/java/api/overview/azure/batch/managementapi)


## <a name="samples"></a><span data-ttu-id="d2c50-121">サンプル</span><span class="sxs-lookup"><span data-stu-id="d2c50-121">Samples</span></span>

<span data-ttu-id="d2c50-122">[Batch アカウントの管理][1]</span><span class="sxs-lookup"><span data-stu-id="d2c50-122">[Manage Batch accounts][1]</span></span>   

<span data-ttu-id="d2c50-123">アプリから利用できる [Azure Batch のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=batch)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="d2c50-123">Explore more [sample Java code for Azure Batch](https://azure.microsoft.com/resources/samples/?platform=java&term=batch) you can use in your apps.</span></span>

[1]: https://github.com/Azure-Samples/batch-java-manage-batch-accounts