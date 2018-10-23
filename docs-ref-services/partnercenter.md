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
# <a name="partner-center-libraries-for-java"></a>Java 用パートナー センター ライブラリ

## <a name="overview"></a>概要

Java 用パートナー センター ライブラリは、Microsoft のパートナー センター サービスを操作するための機能が用意されている SDK です。 これにより、パートナーは、プログラムを使用してパートナー センターの操作を実行できます。 これは、REST API や .NET SDK といった既存のポートフォリオに最後に追加されました。

## <a name="client-library"></a>クライアント ライブラリ

クライアント ライブラリを使用して、パートナー センターでリソースを管理します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```xml
<dependency>
    <groupId>com.microsoft.store</groupId>
    <artifactId>partnercenter</artifactId>
    <version>1.8.0</version>
</dependency>
```   

### <a name="example"></a>例

認証済みリセラーに関連付けられている顧客の一覧を取得します。

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

## <a name="samples"></a>サンプル

[サポートされているシナリオ](https://docs.microsoft.com/partner-center/develop/scenarios)
[コンソール アプリケーションのサンプル](https://github.com/Microsoft/Partner-Center-Java-Samples)  