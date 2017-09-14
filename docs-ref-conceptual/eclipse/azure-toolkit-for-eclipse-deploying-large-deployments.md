---
title: "大規模なデプロイ"
description: "Azure Toolkit for Eclipse を使用して大規模なデプロイを行う方法について説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 0e367e74d038043f1626dbf19aab87db6451bc02
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="e17f9-103">大規模なデプロイ</span><span class="sxs-lookup"><span data-stu-id="e17f9-103">Deploying Large Deployments</span></span>

<span data-ttu-id="e17f9-104">デプロイの規模が大きすぎて既定の approot フォルダーに格納できない場合は、JDK とアプリケーション サーバーのデプロイ ルート フォルダーとしてローカル ストレージ リソースを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e17f9-104">If your deployment is too large to be contained in the default approot folder, you can use a local storage resource as the deployment root folder for your JDK and application server.</span></span>

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="e17f9-105">大規模なデプロイのデプロイ ルート フォルダーとしてローカル ストレージ リソースを使用するには</span><span class="sxs-lookup"><span data-stu-id="e17f9-105">To use a local storage resource as the deployment root folder for large deployments</span></span>

1. <span data-ttu-id="e17f9-106">新しいローカル ストレージ リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="e17f9-106">Create a new local storage resource.</span></span> <span data-ttu-id="e17f9-107">リソースの名前は問いません。</span><span class="sxs-lookup"><span data-stu-id="e17f9-107">The name of the resource does not matter.</span></span> <span data-ttu-id="e17f9-108">ストレージ リソースはロール レベルで定義されます。</span><span class="sxs-lookup"><span data-stu-id="e17f9-108">Storage resources are defined at the role level.</span></span> <span data-ttu-id="e17f9-109">新しいローカル ストレージ リソースを作成するためのローカル ストレージ構成ダイアログにアクセスする最も簡単な方法は、次の手順に従うことです。**[Project Explorer (プロジェクト エクスプローラー)]** ビューでロールを右クリックします (ロールが表示されていない場合は、Azure プロジェクト ノードを展開します)。**[Azure]** をクリックし、**[Local Storage (ローカル ストレージ)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e17f9-109">The quickest way to access the local storage configuration dialog, from which you could create a new local storage resource, is by using the following steps: Right-click the role in the **Project Explorer** view (expand your Azure project node if you don't see the role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="e17f9-110">**[Local Storage (ローカル ストレージ)]** ダイアログで、**[Add (追加)]** をクリックして新しいローカル ストレージ リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="e17f9-110">Within the **Local Storage** dialog, click **Add** to create a new local storage resource.</span></span>

1. <span data-ttu-id="e17f9-111">2048 MB 以上の目的のサイズを設定します (このサイズより小さくすると、approot のように同一ファイル サイズ問題が発生する可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="e17f9-111">Set the desired size to at least 2048 MB (anything less may cause the same file size problems as you would encounter in the approot).</span></span>

1. <span data-ttu-id="e17f9-112">**[Clean the contents when the role instance is recycled (ロール インスタンスをリサイクルするときにコンテンツをクリーニングする)]** がオンになっていることを確認します。これで、ロール インスタンスがリサイクルされるときに、デプロイのスタートアップ ロジックがリソース内の既存のファイルと競合することを防止できます。</span><span class="sxs-lookup"><span data-stu-id="e17f9-112">Ensure that **Clean the contents when the role instance is recycled** is checked; this will help prevent the deployment's startup logic from running into conflicts with pre-existing files in the resource when the role instance is recycled.</span></span>

1. <span data-ttu-id="e17f9-113">**[Environment variable storing the resource's directory path after deployment (デプロイ後にリソースのディレクトリ パスを格納する環境変数)]** の値が文字列 **DEPLOYROOT** に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e17f9-113">Ensure that the **Environment variable storing the resource's directory path after deployment** value is set to the string **DEPLOYROOT**.</span></span> <span data-ttu-id="e17f9-114">ローカル ストレージ リソース ダイアログは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e17f9-114">Your local storage resource dialog will look similar to the following.</span></span>

   ![][ic667943]

<span data-ttu-id="e17f9-115">ローカル リソースの*名前*として **DEPLOYROOT** を使用し、自動的に生成される環境変数 (この場合は **DEPLOYROOT_PATH** に設定されます) を変更しないという操作でも、アプリケーションは問題なく動作します。</span><span class="sxs-lookup"><span data-stu-id="e17f9-115">Alternatively, if you use **DEPLOYROOT** as the *name* of your local resource and you do not change the automatically-generated environment variable name (which will be set to **DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="e17f9-116">ローカル ストレージ リソースの作成に関する追加情報については、「[ローカル ストレージのプロパティ][Local storage properties]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e17f9-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="next-steps"></a><span data-ttu-id="e17f9-117">次のステップ</span><span class="sxs-lookup"><span data-stu-id="e17f9-117">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
