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
ms.openlocfilehash: 924ccf9bdaad4bc635f133adbcfcc8f797d06644
ms.sourcegitcommit: acc83bb537d77568b2a5427479d6354d6ae30885
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2017
---
# <a name="release-notes"></a><span data-ttu-id="bef83-104">リリース ノート</span><span class="sxs-lookup"><span data-stu-id="bef83-104">Release Notes</span></span> 

## <a name="october-5-2017---130"></a><span data-ttu-id="bef83-105">2017 年 10 月 5 日 - 1.3.0</span><span class="sxs-lookup"><span data-stu-id="bef83-105">October 5, 2017 - 1.3.0</span></span> 

<span data-ttu-id="bef83-106">バージョン 1.3.0 は、以前のリリースで一般公開 (安定) 段階に達したサービスおよび機能の使用について、以前のバージョンと下位互換性があります。</span><span class="sxs-lookup"><span data-stu-id="bef83-106">Version 1.3.0 is backwards compatible with previous versions for services and features use that reached the general availability (stable) stage in previous releases.</span></span>

<span data-ttu-id="bef83-107">これらのサービスに関する以前のバージョンからの重大な変更には、@Beta という注釈が付いています。</span><span class="sxs-lookup"><span data-stu-id="bef83-107">Any breaking changes from Preview versions for those services are marked with the @Beta annotation.</span></span>

<span data-ttu-id="bef83-108">コードを 1.3.0 に移行する場合は、[こちらの注意事項](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md)を参照して、既存のコードを 1.3 バージョンに対応させてください。</span><span class="sxs-lookup"><span data-stu-id="bef83-108">If you are migrating your code to 1.3.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md) to prepare your existing code for the 1.3 version.</span></span>

### <a name="generally-availabile-in-v13"></a><span data-ttu-id="bef83-109">V1.3 での一般提供</span><span class="sxs-lookup"><span data-stu-id="bef83-109">Generally availabile in V1.3</span></span>

<span data-ttu-id="bef83-110">以前のリリースではまだベータ版だった一部の API が GA になりました。具体的な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="bef83-110">Some of the APIs that were still in Beta in previous releases are now GA, in particular:</span></span>

- <span data-ttu-id="bef83-111">非同期メソッド</span><span class="sxs-lookup"><span data-stu-id="bef83-111">async methods</span></span>
- <span data-ttu-id="bef83-112">これまで Beta 版であった CDN のすべてのメソッド</span><span class="sxs-lookup"><span data-stu-id="bef83-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="bef83-113">これまで Beta 版であった Application Gateway のすべてのメソッドおよびインターフェイス</span><span class="sxs-lookup"><span data-stu-id="bef83-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

 <span data-ttu-id="bef83-114">ライブラリの一部については引き続きプレビューとなります。</span><span class="sxs-lookup"><span data-stu-id="bef83-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="bef83-115">現在のライブラリの状態については、次の表を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bef83-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="bef83-116">サービスまたは機能</span><span class="sxs-lookup"><span data-stu-id="bef83-116">Service or feature</span></span> | <span data-ttu-id="bef83-117">GA として利用可能</span><span class="sxs-lookup"><span data-stu-id="bef83-117">Available as GA</span></span> | <span data-ttu-id="bef83-118">プレビューとして利用可能</span><span class="sxs-lookup"><span data-stu-id="bef83-118">Available as Preview</span></span> 
---------|---------|---------|-
<span data-ttu-id="bef83-119">コンピューティング</span><span class="sxs-lookup"><span data-stu-id="bef83-119">Compute</span></span>  | <span data-ttu-id="bef83-120">仮想マシンと VM の拡張機能、仮想マシン スケール セット、管理ディスク</span><span class="sxs-lookup"><span data-stu-id="bef83-120">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="bef83-121">Azure Container Service、Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="bef83-121">Azure container service, Azure container registry</span></span> 
<span data-ttu-id="bef83-122">ストレージ</span><span class="sxs-lookup"><span data-stu-id="bef83-122">Storage</span></span>   |  <span data-ttu-id="bef83-123">ストレージ アカウント</span><span class="sxs-lookup"><span data-stu-id="bef83-123">Storage accounts</span></span>       |    <span data-ttu-id="bef83-124">暗号化</span><span class="sxs-lookup"><span data-stu-id="bef83-124">Encryption</span></span>     
<span data-ttu-id="bef83-125">SQL Database</span><span class="sxs-lookup"><span data-stu-id="bef83-125">SQL Database</span></span>  | <span data-ttu-id="bef83-126">データベース、ファイアウォール、エラスティック プール</span><span class="sxs-lookup"><span data-stu-id="bef83-126">Databases, firewalls, elastic pools</span></span>              
<span data-ttu-id="bef83-127">ネットワーク</span><span class="sxs-lookup"><span data-stu-id="bef83-127">Networking</span></span>    |  <span data-ttu-id="bef83-128">仮想ネットワーク、ネットワーク インターフェイス、IP アドレス、ルーティング テーブル、ネットワーク セキュリティ グループ、DNS、Traffic Manager、Application Gateway</span><span class="sxs-lookup"><span data-stu-id="bef83-128">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="bef83-129">ロード バランサー、ネットワーク ピアリング、仮想ネットワーク ゲートウェイ、ネットワーク ウォッチャー</span><span class="sxs-lookup"><span data-stu-id="bef83-129">Load balancers, Network peering, Virtual Network Gateway, Network watchers</span></span> 
<span data-ttu-id="bef83-130">その他のサービス</span><span class="sxs-lookup"><span data-stu-id="bef83-130">More services</span></span>    |  <span data-ttu-id="bef83-131">リソース マネージャー、Key Vault、Redis、CDN、Batch</span><span class="sxs-lookup"><span data-stu-id="bef83-131">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="bef83-132">Web アプリ、関数アプリ、Service Bus、Graph RBAC、Cosmos DB、検索</span><span class="sxs-lookup"><span data-stu-id="bef83-132">Web apps, Function apps, Service Bus, Graph RBAC, Cosmos DB, Search</span></span>  
<span data-ttu-id="bef83-133">基礎</span><span class="sxs-lookup"><span data-stu-id="bef83-133">Fundamentals</span></span>     |   <span data-ttu-id="bef83-134">認証 - コア、非同期メソッド、管理対象サービス ID</span><span class="sxs-lookup"><span data-stu-id="bef83-134">Authentication - core , Async methods , Managed Service Identity</span></span>      |      |

> <span data-ttu-id="bef83-135">ライブラリのクラス、インターフェイス、メソッド レベルでは、プレビュー機能に `@Beta` という注釈が付いています。</span><span class="sxs-lookup"><span data-stu-id="bef83-135">Preview features are marked with a `@Beta` annotation at the class or interface or method level in libraries.</span></span> <span data-ttu-id="bef83-136">これらの機能は、変更されることがあります。</span><span class="sxs-lookup"><span data-stu-id="bef83-136">These features are subject to change.</span></span> <span data-ttu-id="bef83-137">将来的に、何らかの変更が行われたり、削除されたりする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bef83-137">They can be modified in any way, or even removed, in the future.</span></span>

### <a name="import-with-maven"></a><span data-ttu-id="bef83-138">Maven でのインポート</span><span class="sxs-lookup"><span data-stu-id="bef83-138">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="bef83-139">質問とフィードバック</span><span class="sxs-lookup"><span data-stu-id="bef83-139">Get help and give feedback</span></span>

<span data-ttu-id="bef83-140">自分のコードでライブラリを使用する方法についてご不明な点がありましたら、[Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) コミュニティのサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bef83-140">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="bef83-141">バグを見つけた場合や、これらのライブラリの改善に向けた提案がある場合は、[GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 経由でお寄せください。</span><span class="sxs-lookup"><span data-stu-id="bef83-141">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>


