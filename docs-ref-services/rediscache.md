---
title: Redis Cache Libraries for Java
description: "Redis Cache 用 Java クライアント ライブラリと管理ライブラリのリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, キャッシュ, redis, Web キャッシュ, キー値, インメモリ"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: redis-cache
ms.openlocfilehash: 6d436c49124fd0a406486e0c7bac4d1605de5d32
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2017
---
# <a name="redis-cache-libraries-for-java"></a>Redis Cache Libraries for Java

## <a name="overview"></a>概要

Azure Redis Cache は、一般的なオープン ソースの [Redis](https://redis.io/) キャッシュに基づいた安全な分散型キー値ストアです。 

Azure Redis Cache を導入するには「[Java で Azure Redis Cache を使用する方法](/azure/redis-cache/cache-java-get-started)」を参照してください。

## <a name="client-library"></a>クライアント ライブラリ

Azure Redis Cache に接続し、オープンソースの [Jedis](https://github.com/xetorthio/jedis) クライアントを使用して、キャッシュの値の保存と取得を行います。  

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a>例

Azure Redis に接続し、キャッシュに文字列を挿入します。

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a>Management API

Management API を使用して、Azure Redis リソースの作成とスケーリング、アクセス キーの管理を行います。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a>例

[2 ノードの Standard レベル](https://azure.microsoft.com/services/cache/)に Azure Redis Cache を新しく作成します。 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/rediscache/managementapi)

## <a name="samples"></a>サンプル

[Azure Redis Cache を管理する](https://github.com/Azure-Samples/redis-java-manage-cache)   

アプリで利用できる [Azure Redis Cache のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=redis)を探しましょう。
