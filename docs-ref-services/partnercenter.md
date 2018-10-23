---
title: パートナー センターの Java SDK
description: パートナー センターの Java SDK のリファレンス ドキュメント
keywords: API, CSP, クラウド ソリューション プロバイダー, Java, パートナー センター, SDK
author: iswillia
ms.author: iswillia
manager: lesliep
ms.date: 10/09/2018
ms.topic: reference
ms.prod: partnercenter
ms.technology: partnercenter
ms.devlang: java
ms.openlocfilehash: 2adfbdbd37d6eb91e48ee37091f951b3f7d59eb9
ms.sourcegitcommit: 4da7d2f470331feb7d76a1658f5825f365cdba9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2018
ms.locfileid: "49121645"
---
# <a name="partner-center-libraries-for-java"></a><span data-ttu-id="8d5d0-104">Java 用パートナー センター ライブラリ</span><span class="sxs-lookup"><span data-stu-id="8d5d0-104">Partner Center libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="8d5d0-105">概要</span><span class="sxs-lookup"><span data-stu-id="8d5d0-105">Overview</span></span>

<span data-ttu-id="8d5d0-106">Java 用パートナー センター ライブラリは、Microsoft のパートナー センター サービスを操作するための機能が用意されている SDK です。</span><span class="sxs-lookup"><span data-stu-id="8d5d0-106">The Partner Center libraries for Java is a SDK that provides the ability to interact with Microsoft's Partner Center service.</span></span> <span data-ttu-id="8d5d0-107">これにより、パートナーは、プログラムを使用してパートナー センターの操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="8d5d0-107">This enables partners to perform the Partner Center operations programmatically.</span></span> <span data-ttu-id="8d5d0-108">これは、REST API や .NET SDK といった既存のポートフォリオに最後に追加されました。</span><span class="sxs-lookup"><span data-stu-id="8d5d0-108">This is the latest addition to existing portfolio of REST APIs and the .NET SDK.</span></span>

## <a name="client-library"></a><span data-ttu-id="8d5d0-109">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="8d5d0-109">Client library</span></span>

<span data-ttu-id="8d5d0-110">クライアント ライブラリを使用して、パートナー センターでリソースを管理します。</span><span class="sxs-lookup"><span data-stu-id="8d5d0-110">Manage resources in Partner Center using the client library.</span></span>

<span data-ttu-id="8d5d0-111">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="8d5d0-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```xml
<dependency>
    <groupId>com.microsoft.store</groupId>
    <artifactId>partnercenter</artifactId>
    <version>1.8.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="8d5d0-112">例</span><span class="sxs-lookup"><span data-stu-id="8d5d0-112">Example</span></span>

<span data-ttu-id="8d5d0-113">認証済みリセラーに関連付けられている顧客の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="8d5d0-113">Retrieves a complete list of customer associated with the authenticated reseller.</span></span>

```java
IPartnerCredentials appCredentials = PartnerCredentials.getInstance().generateByApplicationCredentials('YOUR_APP_ID', 'YOUR_APP_SECRET', 'YOUR_TENANT_ID');
IAggregatePartner partnerOperations = PartnerService.getInstance().createPartnerOperations(appCredentials);

// Query the customers
SeekBasedResourceCollection<Customer> customersPage = partnerOperations.getCustomers().query(QueryFactory.getInstance().buildIndexedQuery(100));
this.getContext().getConsoleHelper().stopProgress();

// Create a customer enumerator which will aid us in traversing the customer pages
IResourceCollectionEnumerator<SeekBasedResourceCollection<Customer>> customersEnumerator =
    partnerOperations.getEnumerators().getCustomers().create( customersPage );

int pageNumber = 1;

while ( customersEnumerator.hasValue() )
{
    /*
     * Perform some operation with the current page of customers using 
     * customersEnumerator.getCurrent()  
     */

    // Get the next page of customers
    customersEnumerator.next();
}
```

## <a name="samples"></a><span data-ttu-id="8d5d0-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="8d5d0-114">Samples</span></span>

<span data-ttu-id="8d5d0-115">[サポートされているシナリオ](https://docs.microsoft.com/partner-center/develop/scenarios)
[コンソール アプリケーションのサンプル](https://github.com/Microsoft/Partner-Center-Java-Samples)</span><span class="sxs-lookup"><span data-stu-id="8d5d0-115">[Supported scenarios](https://docs.microsoft.com/partner-center/develop/scenarios)
[Sample console application](https://github.com/Microsoft/Partner-Center-Java-Samples)</span></span>  