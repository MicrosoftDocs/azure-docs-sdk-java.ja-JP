---
title: Azure DNS Libraries for Java
description: Azure DNS Java 管理ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, ドメイン, DNS, ネーム, サービス, ドメイン ネーム サービス
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: dns
ms.openlocfilehash: 364c51f985b7bf3a8c445cd7e03a5e91a8e589ba
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593437"
---
# <a name="azure-traffic-manager-libraries-for-java"></a>Azure Traffic Manager Libraries for Java

## <a name="overview"></a>概要

ドメイン名を解決し、[Azure DNS](/azure/dns/dns-overview) を使用する他の Azure サービスと同じ資格情報、API、ツール、課金情報を使用して DNS レコードを管理します。

Azure DNS を導入するには、「[Azure CLI 2.0 で Azure DNS の使用を開始する](/azure/dns/dns-getstarted-cli)」を参照してください。

## <a name="management-api"></a>管理 API

DNS ゾーンを作成し、Management API を使用してゾーンにレコードを追加します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>例

ルート DNS ゾーンを作成し、既存のリソース グループ内の `www` CNAME レコードを追加します。

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/dns/management)

## <a name="samples"></a>サンプル

[Azure DNS でドメインをホストして管理する](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

アプリで利用できる [Azure DNS のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=dns)を探しましょう。

<!---Loc Comment: Please, refer to conversation section to check the issue. Thanks.--->
