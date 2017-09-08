---
title: "Azure Management Libraries for Java リリース ノート | Microsoft Docs"
description: "Azure Management Libraries for Java の新機能や重大な変更が記載されています。"
keywords: "Azure,  Java, API, リファレンス,  ノート,  更新, 非推奨"
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: fc246d499b2f5f20a7efbaed16c4b4193d8d8e6c
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="release-notes"></a><span data-ttu-id="ea8e6-104">リリース ノート</span><span class="sxs-lookup"><span data-stu-id="ea8e6-104">Release Notes</span></span> 

## <a name="june-30-2017---110"></a><span data-ttu-id="ea8e6-105">2017 年 6 月 30 日 - 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ea8e6-105">June 30, 2017 - 1.1.0</span></span> 

<span data-ttu-id="ea8e6-106">V1.0 で一般提供 (安定) 段階に達したパブリック ユース向け API に関して、V1.1 には V1.0 との下位互換性があります。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-106">V1.1 is backwards compatible with V1.0 in the APIs intended for public use that reached the general availability (stable) stage in V1.0.</span></span>

<span data-ttu-id="ea8e6-107">V.0 で @Beta の注釈を使ってマークされた API には、いくつかの重大な変更が導入されました。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-107">Some breaking changes were introduced in APIs marked with the @Beta annotation in V.0</span></span>

<span data-ttu-id="ea8e6-108">コードを 1.1.0 に移行する場合は、[こちらの注意事項](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)を参照して、1.0.0 から 1.1.0 にコードを対応させてください。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-108">If you are migrating your code to 1.1.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) for preparing your code for 1.1.0 from 1.0.0.</span></span>

### <a name="generally-availabile-in-v11"></a><span data-ttu-id="ea8e6-109">V1.1 で一般提供</span><span class="sxs-lookup"><span data-stu-id="ea8e6-109">Generally availabile in V1.1</span></span>

<span data-ttu-id="ea8e6-110">V1.0 でベータ版のままとなっている一部の API が、V1.1 で GA になりました。具体的な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-110">Some of the APIs that were still in Beta in V1.0 are now GA in V1.1, in particular:</span></span>

- <span data-ttu-id="ea8e6-111">非同期メソッド</span><span class="sxs-lookup"><span data-stu-id="ea8e6-111">async methods</span></span>
- <span data-ttu-id="ea8e6-112">これまで Beta 版であった CDN のすべてのメソッド</span><span class="sxs-lookup"><span data-stu-id="ea8e6-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="ea8e6-113">これまで Beta 版であった Application Gateway のすべてのメソッドおよびインターフェイス</span><span class="sxs-lookup"><span data-stu-id="ea8e6-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

 <span data-ttu-id="ea8e6-114">ライブラリの一部については引き続きプレビューとなります。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="ea8e6-115">現在のライブラリの状態については、次の表を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="ea8e6-116">サービスまたは機能</span><span class="sxs-lookup"><span data-stu-id="ea8e6-116">Service or feature</span></span> | <span data-ttu-id="ea8e6-117">GA として利用可能</span><span class="sxs-lookup"><span data-stu-id="ea8e6-117">Available as GA</span></span> | <span data-ttu-id="ea8e6-118">プレビューとして利用可能</span><span class="sxs-lookup"><span data-stu-id="ea8e6-118">Available as Preview</span></span>  | <span data-ttu-id="ea8e6-119">近日対応予定</span><span class="sxs-lookup"><span data-stu-id="ea8e6-119">Coming soon</span></span> |
---------|---------|---------|---------|
<span data-ttu-id="ea8e6-120">コンピューティング</span><span class="sxs-lookup"><span data-stu-id="ea8e6-120">Compute</span></span>  | <span data-ttu-id="ea8e6-121">仮想マシンと VM の拡張機能、仮想マシン スケール セット、管理ディスク</span><span class="sxs-lookup"><span data-stu-id="ea8e6-121">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="ea8e6-122">Azure Container Service、Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="ea8e6-122">Azure container service, Azure container registry</span></span> |    |
<span data-ttu-id="ea8e6-123">Storage</span><span class="sxs-lookup"><span data-stu-id="ea8e6-123">Storage</span></span>   |  <span data-ttu-id="ea8e6-124">ストレージ アカウント</span><span class="sxs-lookup"><span data-stu-id="ea8e6-124">Storage accounts</span></span>       |         |   <span data-ttu-id="ea8e6-125">暗号化</span><span class="sxs-lookup"><span data-stu-id="ea8e6-125">Encryption</span></span>      |
<span data-ttu-id="ea8e6-126">SQL Database</span><span class="sxs-lookup"><span data-stu-id="ea8e6-126">SQL Database</span></span>  | <span data-ttu-id="ea8e6-127">データベース、ファイアウォール、エラスティック プール</span><span class="sxs-lookup"><span data-stu-id="ea8e6-127">Databases, firewalls, elastic pools</span></span>        |         |   <span data-ttu-id="ea8e6-128">その他の機能</span><span class="sxs-lookup"><span data-stu-id="ea8e6-128">More features</span></span>      |
<span data-ttu-id="ea8e6-129">ネットワーク</span><span class="sxs-lookup"><span data-stu-id="ea8e6-129">Networking</span></span>    |  <span data-ttu-id="ea8e6-130">仮想ネットワーク、ネットワーク インターフェイス、IP アドレス、ルーティング テーブル、ネットワーク セキュリティ グループ、DNS、Traffic Manager、Application Gateway</span><span class="sxs-lookup"><span data-stu-id="ea8e6-130">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="ea8e6-131">ロード バランサー</span><span class="sxs-lookup"><span data-stu-id="ea8e6-131">Load balancers</span></span>     |   <span data-ttu-id="ea8e6-132">VPN、Network Watcher</span><span class="sxs-lookup"><span data-stu-id="ea8e6-132">VPN, Network watchers</span></span>   |
<span data-ttu-id="ea8e6-133">その他のサービス</span><span class="sxs-lookup"><span data-stu-id="ea8e6-133">More services</span></span>    |  <span data-ttu-id="ea8e6-134">リソース マネージャー、Key Vault、Redis、CDN、Batch</span><span class="sxs-lookup"><span data-stu-id="ea8e6-134">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="ea8e6-135">Web Apps、Function App、Service Bus、Graph RBAC、DocumentDB</span><span class="sxs-lookup"><span data-stu-id="ea8e6-135">Web apps, Function apps, Service Bus, Graph RBAC, DocumentDB</span></span>   | <span data-ttu-id="ea8e6-136">Monitor、Scheduler、Functions 管理、Search、Graph RBAC の各種機能</span><span class="sxs-lookup"><span data-stu-id="ea8e6-136">Monitor ,Scheduler, Functions management, Search, more Graph RBAC features</span></span>        |
<span data-ttu-id="ea8e6-137">基礎</span><span class="sxs-lookup"><span data-stu-id="ea8e6-137">Fundamentals</span></span>     |   <span data-ttu-id="ea8e6-138">認証 - コア、非同期メソッド</span><span class="sxs-lookup"><span data-stu-id="ea8e6-138">Authentication - core , Async methods</span></span>       |      |         |

### <a name="import-with-maven"></a><span data-ttu-id="ea8e6-139">Maven でのインポート</span><span class="sxs-lookup"><span data-stu-id="ea8e6-139">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="ea8e6-140">質問とフィードバック</span><span class="sxs-lookup"><span data-stu-id="ea8e6-140">Get help and give feedback</span></span>

<span data-ttu-id="ea8e6-141">自分のコードでライブラリを使用する方法についてご不明な点がありましたら、[Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) コミュニティのサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-141">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="ea8e6-142">バグを見つけた場合や、これらのライブラリの改善に向けた提案がある場合は、[GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 経由でお寄せください。</span><span class="sxs-lookup"><span data-stu-id="ea8e6-142">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>

### <a name="migrate-from-previous-releases"></a><span data-ttu-id="ea8e6-143">以前のリリースからの移行</span><span class="sxs-lookup"><span data-stu-id="ea8e6-143">Migrate from previous releases</span></span>

[<span data-ttu-id="ea8e6-144">1.0.0-beta5 からの移行](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [1.1.0 からの移行</span><span class="sxs-lookup"><span data-stu-id="ea8e6-144">Migrate from 1.0.0-beta5](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [Migrate from 1.1.0</span></span>](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)


