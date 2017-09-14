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
# <a name="azure-resource-manager-libraries-for-java"></a>Azure Resource Manager Libraries for Java

## <a name="overview"></a>概要

[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) を使用して、グループ内のリソースをデプロイ、監視、管理します。

## <a name="management-api"></a>Management API

Management API を使用して、リソース グループを作成し、テンプレートからリソースをデプロイします。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.2.1</version>
</dependency>
```

## <a name="example"></a>例

米国東部の Azure リージョンに新しいリソース グループを作成します。

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/resources/managementapi)

## <a name="samples"></a>サンプル

[Java を使用して Azure リソース グループを管理する][1] 
[ARM テンプレートを使用してリソースをデプロイする][2]

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

Azure Resource Manager のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=java&term=resource)を参照してください。
