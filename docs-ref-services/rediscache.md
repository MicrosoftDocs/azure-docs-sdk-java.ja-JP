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
ms.openlocfilehash: 25c91e02d1ce52afab68286c89c859ab61da56fe
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="redis-cache-libraries-for-java"></a><span data-ttu-id="2f65f-104">Redis Cache Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="2f65f-104">Redis Cache libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="2f65f-105">概要</span><span class="sxs-lookup"><span data-stu-id="2f65f-105">Overview</span></span>

<span data-ttu-id="2f65f-106">Azure Redis Cache は、一般的なオープン ソースの [Redis](https://redis.io/) キャッシュに基づいた安全な分散型キー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="2f65f-106">Azure Redis Cache is a secure, distributed key-value store based on the popular open source [Redis](https://redis.io/) cache.</span></span> 

<span data-ttu-id="2f65f-107">Azure Redis Cache を導入するには「[Java で Azure Redis Cache を使用する方法](/azure/redis-cache/cache-java-get-started)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2f65f-107">To get started with Azure Redis Cache, see [How to use Azure Redis Cache with Java](/azure/redis-cache/cache-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="2f65f-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="2f65f-108">Client library</span></span>

<span data-ttu-id="2f65f-109">Azure Redis Cache に接続し、オープンソースの [Jedis](https://github.com/xetorthio/jedis) クライアントを使用して、キャッシュの値の保存と取得を行います。</span><span class="sxs-lookup"><span data-stu-id="2f65f-109">Connect to Azure Redis Cache and store and retrieve values from the cache using the open-source [Jedis](https://github.com/xetorthio/jedis) client.</span></span>  

<span data-ttu-id="2f65f-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="2f65f-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a><span data-ttu-id="2f65f-111">例</span><span class="sxs-lookup"><span data-stu-id="2f65f-111">Example</span></span>

<span data-ttu-id="2f65f-112">Azure Redis に接続し、キャッシュに文字列を挿入します。</span><span class="sxs-lookup"><span data-stu-id="2f65f-112">Connect to Azure Redis and insert a string into the cache.</span></span>

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a><span data-ttu-id="2f65f-113">Management API</span><span class="sxs-lookup"><span data-stu-id="2f65f-113">Management API</span></span>

<span data-ttu-id="2f65f-114">Management API を使用して、Azure Redis リソースの作成とスケーリング、アクセス キーの管理を行います。</span><span class="sxs-lookup"><span data-stu-id="2f65f-114">Create and scale Azure Redis resources and manage access keys to with the management API.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.1.2</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="2f65f-115">例</span><span class="sxs-lookup"><span data-stu-id="2f65f-115">Example</span></span>

<span data-ttu-id="2f65f-116">[2 ノードの Standard レベル](https://azure.microsoft.com/services/cache/)に Azure Redis Cache を新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="2f65f-116">Create a new Azure Redis Cache in the [two-node standard tier](https://azure.microsoft.com/services/cache/).</span></span> 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="2f65f-117">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="2f65f-117">Explore the Management APIs</span></span>](/java/api/overview/azure/rediscache/managementapi)

## <a name="samples"></a><span data-ttu-id="2f65f-118">サンプル</span><span class="sxs-lookup"><span data-stu-id="2f65f-118">Samples</span></span>

[<span data-ttu-id="2f65f-119">Azure Redis Cache を管理する</span><span class="sxs-lookup"><span data-stu-id="2f65f-119">Manage Azure Redis Cache</span></span>](https://github.com/Azure-Samples/redis-java-manage-cache)   

<span data-ttu-id="2f65f-120">アプリで利用できる [Azure Redis Cache のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=redis)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="2f65f-120">Explore more [sample Java code for Azure Redis Cache](https://azure.microsoft.com/resources/samples/?platform=java&term=redis) you can use in your apps.</span></span>