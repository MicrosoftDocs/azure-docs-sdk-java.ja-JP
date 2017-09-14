---
title: Azure DNS Libraries for Java
description: "Azure DNS Java 管理ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, ドメイン, DNS, ネーム, サービス, ドメイン ネーム サービス"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: dns
ms.openlocfilehash: 123f61004c473bc060f0360f0fcb027d1591523e
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2017
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="f83d7-104">Azure Traffic Manager Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="f83d7-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="f83d7-105">概要</span><span class="sxs-lookup"><span data-stu-id="f83d7-105">Overview</span></span>

<span data-ttu-id="f83d7-106">ドメイン名を解決し、[Azure DNS](/azure/dns/dns-overview) を使用する他の Azure サービスと同じ資格情報、API、ツール、課金情報を使用して DNS レコードを管理します。</span><span class="sxs-lookup"><span data-stu-id="f83d7-106">Provide domain name resolution and manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services with [Azure DNS](/azure/dns/dns-overview).</span></span>

<span data-ttu-id="f83d7-107">Azure DNS を導入するには、「[Azure CLI 2.0 で Azure DNS の使用を開始する](/azure/dns/dns-getstarted-cli)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f83d7-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure CLI 2.0](/azure/dns/dns-getstarted-cli).</span></span>

## <a name="management-api"></a><span data-ttu-id="f83d7-108">Management API</span><span class="sxs-lookup"><span data-stu-id="f83d7-108">Management API</span></span>

<span data-ttu-id="f83d7-109">DNS ゾーンを作成し、Management API を使用してゾーンにレコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f83d7-109">Create DNS zones and add records to zones with the management API.</span></span>

<span data-ttu-id="f83d7-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="f83d7-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.2.1</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="f83d7-111">例</span><span class="sxs-lookup"><span data-stu-id="f83d7-111">Example</span></span>

<span data-ttu-id="f83d7-112">ルート DNS ゾーンを作成し、既存のリソース グループ内の `www` CNAME レコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f83d7-112">Create a root DNS zone and add a `www` CNAME record in an existing resource group.</span></span>

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="f83d7-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="f83d7-113">Explore the Management APIs</span></span>](/java/api/overview/azure/dns/managementapi)

## <a name="samples"></a><span data-ttu-id="f83d7-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="f83d7-114">Samples</span></span>

[<span data-ttu-id="f83d7-115">Azure DNS でドメインをホストして管理する</span><span class="sxs-lookup"><span data-stu-id="f83d7-115">Host and manage your domains with Azure DNS</span></span>](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

<span data-ttu-id="f83d7-116">アプリで利用できる [Azure DNS のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=dns)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="f83d7-116">Explore more [sample Java code for Azure DNS](https://azure.microsoft.com/resources/samples/?platform=java&term=dns) you can use in your apps.</span></span>