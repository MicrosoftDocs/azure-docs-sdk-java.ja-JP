---
title: Azure Resource Manager Libraries for Java
description: "Java Resource Manager ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, リソース グループ, arm, リソース マネージャー"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 1e3ec9080da70477cd7d7bd966769c8d396abe9e
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2017
---
# <a name="azure-resource-manager-libraries-for-java"></a><span data-ttu-id="b3a9c-104">Azure Resource Manager Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="b3a9c-104">Azure Resource Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="b3a9c-105">概要</span><span class="sxs-lookup"><span data-stu-id="b3a9c-105">Overview</span></span>

<span data-ttu-id="b3a9c-106">[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) を使用して、グループ内のリソースをデプロイ、監視、管理します。</span><span class="sxs-lookup"><span data-stu-id="b3a9c-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="b3a9c-107">Management API</span><span class="sxs-lookup"><span data-stu-id="b3a9c-107">Management API</span></span>

<span data-ttu-id="b3a9c-108">Management API を使用して、リソース グループを作成し、テンプレートからリソースをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="b3a9c-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

<span data-ttu-id="b3a9c-109">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="b3a9c-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.2.1</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="b3a9c-110">例</span><span class="sxs-lookup"><span data-stu-id="b3a9c-110">Example</span></span>

<span data-ttu-id="b3a9c-111">米国東部の Azure リージョンに新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="b3a9c-111">Create a new resource group in the Azure Eastern US region.</span></span>

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3a9c-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="b3a9c-112">Explore the Management APIs</span></span>](/java/api/overview/azure/resources/managementapi)

## <a name="samples"></a><span data-ttu-id="b3a9c-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="b3a9c-113">Samples</span></span>

<span data-ttu-id="b3a9c-114">[Java を使用して Azure リソース グループを管理する][1] 
[ARM テンプレートを使用してリソースをデプロイする][2]</span><span class="sxs-lookup"><span data-stu-id="b3a9c-114">[Manage Azure Resource Groups with Java][1] 
[Deploy resources using an ARM template][2]</span></span>

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

<span data-ttu-id="b3a9c-115">Azure Resource Manager のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=java&term=resource)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b3a9c-115">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=java&term=resource) of Azure Resource Manager samples.</span></span>
