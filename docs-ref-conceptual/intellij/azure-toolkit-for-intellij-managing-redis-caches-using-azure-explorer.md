---
title: Azure Explorer for IntelliJ を使用して Redis Cache を管理する
description: Azure Explorer for IntelliJ を使って Azure Redis Cache を管理する方法について説明します。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 046ae0428d50a7f173f5ad15be53ffd8e66c11c5
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892523"
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="5de9b-103">Azure Explorer for IntelliJ を使用して Redis Cache を管理する</span><span class="sxs-lookup"><span data-stu-id="5de9b-103">Managing Redis Caches using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="5de9b-104">Azure Toolkit for IntelliJ の一部である Azure Explorer は、IntelliJ IDE 内から Azure アカウントの Redis Cache を管理するための使いやすいソリューションを Java 開発者に提供します。</span><span class="sxs-lookup"><span data-stu-id="5de9b-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="5de9b-105">IntelliJ を使用して Redis Cache を作成する</span><span class="sxs-lookup"><span data-stu-id="5de9b-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="5de9b-106">次の手順では、Azure Explorer を使って Redis Cache を作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="5de9b-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="5de9b-107">「[Azure Toolkit for IntelliJ のサインイン手順]」の記事の手順を使用して、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="5de9b-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="5de9b-108">**Azure Explorer** ツール ウィンドウで、**[Azure]** ノードを展開し、**[Redis Cache]** を右クリックして、**[Create Redis Cache]\(Redis Cache の作成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5de9b-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![[Create Redis Cache]\(Redis Cache の作成\) メニュー][CR01]

1. <span data-ttu-id="5de9b-110">**[新規 Redis Cache]** ダイアログ ボックスが表示されたら、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="5de9b-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![新規 Redis Cache 作成ダイアログ ボックス][CR02]

   <span data-ttu-id="5de9b-112">a.</span><span class="sxs-lookup"><span data-stu-id="5de9b-112">a.</span></span> <span data-ttu-id="5de9b-113">**[DNS 名]**: 新しい Redis Cache の DNS サブドメインを指定します。この名前は、".redis.cache.windows.net" の前に付加されます (例: *wingtiptoys.redis.cache.windows.net*)。</span><span class="sxs-lookup"><span data-stu-id="5de9b-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which are prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="5de9b-114">b.</span><span class="sxs-lookup"><span data-stu-id="5de9b-114">b.</span></span> <span data-ttu-id="5de9b-115">**[サブスクリプション]**: 新しい Redis Cache に使う Azure サブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="5de9b-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="5de9b-116">c.</span><span class="sxs-lookup"><span data-stu-id="5de9b-116">c.</span></span> <span data-ttu-id="5de9b-117">**[リソース グループ]**: Redis Cache のリソース グループを指定します。以下のオプションのいずれかを選ぶ必要があります。</span><span class="sxs-lookup"><span data-stu-id="5de9b-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span> 
      * <span data-ttu-id="5de9b-118">**[新規作成]**: 新しいリソース グループを作成することを指定します。</span><span class="sxs-lookup"><span data-stu-id="5de9b-118">**Create New**: Specifies that you want to create a new resource group.</span></span> 
      * <span data-ttu-id="5de9b-119">**[既存のものを使用]**: Azure アカウントに関連付けられているリソース グループの一覧から選ぶことを指定します。</span><span class="sxs-lookup"><span data-stu-id="5de9b-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span> 

   <span data-ttu-id="5de9b-120">d.</span><span class="sxs-lookup"><span data-stu-id="5de9b-120">d.</span></span> <span data-ttu-id="5de9b-121">**[場所]**: Redis Cache を作成する場所を指定します (例: *米国西部*)。</span><span class="sxs-lookup"><span data-stu-id="5de9b-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="5de9b-122">e.</span><span class="sxs-lookup"><span data-stu-id="5de9b-122">e.</span></span> <span data-ttu-id="5de9b-123">**[価格レベル]**: Redis Cache が使う価格レベルを指定します。この設定により、クライアント接続の数が決まります。</span><span class="sxs-lookup"><span data-stu-id="5de9b-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="5de9b-124">詳しくは、「[Redis Cache の価格]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5de9b-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="5de9b-125">f.</span><span class="sxs-lookup"><span data-stu-id="5de9b-125">f.</span></span> <span data-ttu-id="5de9b-126">**[非 SSL ポート]**: Redis Cache が非 SSL 接続を許可するかどうかを指定します。既定では、SSL 接続のみが許可されます。</span><span class="sxs-lookup"><span data-stu-id="5de9b-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="5de9b-127">Redis Cache のすべての設定を指定したら、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5de9b-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="5de9b-128">Redis Cache が作成されて、Azure Explorer に表示されます。</span><span class="sxs-lookup"><span data-stu-id="5de9b-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Azure Explorer の Redis Cache][CR03]

> [!NOTE]
>
> <span data-ttu-id="5de9b-130">Azure Redis Cache の設定の構成について詳しくは、「[Azure Redis Cache の構成方法]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5de9b-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="5de9b-131">IntelliJ で Redis Cache のプロパティを表示する</span><span class="sxs-lookup"><span data-stu-id="5de9b-131">Display the properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="5de9b-132">Azure Explorer で Redis Cache を右クリックし、**[プロパティの表示]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5de9b-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Redis Cache のプロパティを表示する Azure Explorer のコンテキスト メニュー][SP01]

1. <span data-ttu-id="5de9b-134">Azure Explorer で、Redis Cache のプロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5de9b-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![Redis cache properties][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="5de9b-136">IntelliJ を使用して Redis Cache を削除する</span><span class="sxs-lookup"><span data-stu-id="5de9b-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="5de9b-137">Azure Explorer で Redis Cache を右クリックし、**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5de9b-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Redis Cache を削除する Azure Explorer のコンテキスト メニュー][DE01]

1. <span data-ttu-id="5de9b-139">Redis Cache の削除を確認するメッセージが表示されたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5de9b-139">Click **Yes** when prompted to delete your redis cache.</span></span>

   ![Redis Cache 削除のプロンプト][DE02]

## <a name="next-steps"></a><span data-ttu-id="5de9b-141">次の手順</span><span class="sxs-lookup"><span data-stu-id="5de9b-141">Next steps</span></span>

<span data-ttu-id="5de9b-142">Azure Redis Cache、構成設定、および料金について詳しくは、次のリンクをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5de9b-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="5de9b-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="5de9b-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="5de9b-144">[Redis Cache のドキュメント]</span><span class="sxs-lookup"><span data-stu-id="5de9b-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="5de9b-145">[Redis Cache の価格]</span><span class="sxs-lookup"><span data-stu-id="5de9b-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="5de9b-146">[Azure Redis Cache の構成方法]</span><span class="sxs-lookup"><span data-stu-id="5de9b-146">[How to configure Azure Redis Cache]</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Redis Cache の価格]: https://azure.microsoft.com/pricing/details/cache/
[Redis Cache Pricing]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis Cache のドキュメント]: /azure/redis-cache
[Redis Cache Documentation]: /azure/redis-cache
[Azure Redis Cache の構成方法]: /azure/redis-cache/cache-configure
[How to configure Azure Redis Cache]: /azure/redis-cache/cache-configure
[Azure Toolkit for IntelliJ のサインイン手順]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
