---
title: Azure Batch Libraries for Java
description: Java Batch ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, Batch, 処理, スケジューリング, 長時間実行
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: batch
ms.openlocfilehash: 67381d68d23f98579a472aefbebaa929af622b8d
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823595"
---
# <a name="azure-batch-libraries-for-java"></a>Azure Batch Libraries for Java

## <a name="overview"></a>概要

[Azure Batch](/azure/batch/batch-technical-overview) を使用すると、大規模な並列コンピューティングやハイパフォーマンス コンピューティングのアプリケーションをクラウドで効率的に実行できます。   

Azure Batch の概要については、「[Azure Portal で Batch アカウントを作成する](/azure/batch/batch-account-create-portal)」を参照してください。

## <a name="client-library"></a>クライアント ライブラリ

Azure Batch クライアント ライブラリを使用すると、コンピューティング ノードとプールを構成したり、タスクを定義してそれらをジョブで実行するように構成したり、ジョブ マネージャーを設定してジョブの実行を制御または監視したりすることができます。 これらのオブジェクトを使って大規模な並列コンピューティング ソリューションを実行する方法について詳しくは、[こちら](/azure/batch/batch-api-basics)を参照してください。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```   

### <a name="example"></a>例

Batch アカウントで Linux コンピューティング ノードのプールを設定する例を次に示します。

```java
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://fabrikambatch.eastus.batch.azure.com", "fabrikambatch", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/batch/client)


## <a name="management-api"></a>管理 API

Batch アカウントの作成と削除、Batch アカウント キーの読み取りと再生成、Batch アカウントのストレージ管理は、Azure Batch 管理ライブラリを使って行います。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-batch</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a>例

Azure Batch アカウントを作成し、そのアカウントで使用する新しいアプリケーションと Azure ストレージ アカウントを構成します。

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
> [Management API を探す](/java/api/overview/azure/batch/management)


## <a name="samples"></a>サンプル

[Batch アカウントの管理][1]   

アプリから利用できる [Azure Batch のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=batch)を探しましょう。

[1]: https://github.com/Azure-Samples/batch-java-manage-batch-accounts