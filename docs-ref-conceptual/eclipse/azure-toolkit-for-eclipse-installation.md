---
title: Azure Toolkit for Eclipse のインストール
description: Azure Toolkit for Eclipse プラグインをインストールして、クラウド アプリケーションを作成し、Azure にデプロイする方法を説明します。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 8e6630f7e019d950249e7e84024ac800a0f2f136
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270845"
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="ed4b4-103">Azure Toolkit for Eclipse のインストール</span><span class="sxs-lookup"><span data-stu-id="ed4b4-103">Installing the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="ed4b4-104">Azure Toolkit for Eclipse をインストールするには次の 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-104">There are two ways to install Azure Toolkit for Eclipse:</span></span>

  - [<span data-ttu-id="ed4b4-105">Eclipse Marketplace</span><span class="sxs-lookup"><span data-stu-id="ed4b4-105">Eclipse marketplace</span></span>](#eclipse-marketplace)
  - [<span data-ttu-id="ed4b4-106">新しいソフトウェアのインストール</span><span class="sxs-lookup"><span data-stu-id="ed4b4-106">Install new software</span></span>](#install-new-software)

> [!NOTE] 
> 
> <span data-ttu-id="ed4b4-107">Azure Toolkit for Eclipse はオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから MIT License で入手できます。URL は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-107">The Azure Toolkit for Eclipse is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

[!INCLUDE [azure-toolkit-for-eclipse-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="eclipse-marketplace"></a><span data-ttu-id="ed4b4-108">Eclipse Marketplace</span><span class="sxs-lookup"><span data-stu-id="ed4b4-108">Eclipse marketplace</span></span>

1. <span data-ttu-id="ed4b4-109">次のボタンを実行中の Eclipse ワークスペースにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-109">Drag the following button to your running Eclipse workspace.</span></span>

    <span data-ttu-id="ed4b4-110">[![実行中の Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "実行中の Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要")</span><span class="sxs-lookup"><span data-stu-id="ed4b4-110">[![Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client")</span></span>

2. <span data-ttu-id="ed4b4-111">あるいは、 **[ヘルプ]/[Eclipse Marketplace]** で **Azure Toolkit for Eclipse プラグイン**を検索してインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-111">Otherwise, it is also possible to search and install the **Azure Toolkit for Eclipse plugin** at **Help/Eclipse Marketplace**.</span></span>

    ![マーケットプレース](./media/azure-toolkit-for-eclipse-installation/marketplace.png)

## <a name="install-new-software"></a><span data-ttu-id="ed4b4-113">新しいソフトウェアのインストール</span><span class="sxs-lookup"><span data-stu-id="ed4b4-113">Install new software</span></span>

1. <span data-ttu-id="ed4b4-114">Eclipse を起動します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-114">Start Eclipse.</span></span>

1. <span data-ttu-id="ed4b4-115">次の図に示すように、 **[Help]\(ヘルプ\)** メニューをクリックし、 **[Install New Software]\(新しいソフトウェアのインストール\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-115">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>

   ![Azure Toolkit for Eclipse のインストール][01]

1. <span data-ttu-id="ed4b4-117">**[Available Software]\(利用可能なソフトウェア\)** ダイアログで、 **[Work with]\(対象\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-117">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="ed4b4-118">**[Name]\(名前\)** ウィンドウで、 **[Azure Toolkit for Java]** をオンにし、 **[Contact all update sites during install to find required software]\(インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる\)** をオフにします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-118">In the **Name** pane, check **Azure Toolkit for Java**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="ed4b4-119">画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-119">Your screen should appear similar to the following:</span></span>

   ![Azure Toolkit for Eclipse のインストール][02]

1. <span data-ttu-id="ed4b4-121">**Azure Toolkit for Eclipse** を展開すると、インストールされるコンポーネントの一覧が表示されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-121">If you expand **Azure Toolkit for Eclipse**, you will see a list of components that will be installed; for example:</span></span>

   | <span data-ttu-id="ed4b4-122">機能</span><span class="sxs-lookup"><span data-stu-id="ed4b4-122">Feature</span></span> | <span data-ttu-id="ed4b4-123">説明</span><span class="sxs-lookup"><span data-stu-id="ed4b4-123">Description</span></span> | 
   |---|---| 
   | <span data-ttu-id="ed4b4-124">**Application Insights Plugin for Java**</span><span class="sxs-lookup"><span data-stu-id="ed4b4-124">**Application Insights Plugin for Java**</span></span> | <span data-ttu-id="ed4b4-125">アプリケーションとサーバー インスタンスに対する Azure のテレメトリ ログと分析サービスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-125">Allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span> | 
   | <span data-ttu-id="ed4b4-126">**Azure Common Plugin**</span><span class="sxs-lookup"><span data-stu-id="ed4b4-126">**Azure Common Plugin**</span></span> | <span data-ttu-id="ed4b4-127">他のツールキット コンポーネントで必要な一般的な機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-127">provides the common functionality needed by other toolkit components.</span></span> | 
   | <span data-ttu-id="ed4b4-128">**Azure Container Tools for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="ed4b4-128">**Azure Container Tools for Eclipse**</span></span> | <span data-ttu-id="ed4b4-129">.WAR を Docker コンテナーとして Docker マシンに構築およびデプロイできるようにします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-129">Enables you to build and deploy a .WAR as a Docker container to a docker machine.</span></span> | 
   | <span data-ttu-id="ed4b4-130">**Azure Containers for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="ed4b4-130">**Azure Containers for Eclipse**</span></span> | <span data-ttu-id="ed4b4-131">.WAR または.JAR アーティファクトを Docker コンテナーとして Azure 仮想マシンにデプロイできるようにします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-131">Enables you to deploy a .WAR or .JAR artifact as a Docker container to an Azure virtual machine.</span></span> | 
   | <span data-ttu-id="ed4b4-132">**Azure Explorer for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="ed4b4-132">**Azure Explorer for Eclipse**</span></span> | <span data-ttu-id="ed4b4-133">Azure リソースを管理するためのエクスプローラー スタイルのインターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-133">Provides an explorer-style interface for managing your Azure resources.</span></span> | 
   | <span data-ttu-id="ed4b4-134">**Microsoft JDBC Driver 6.1 for SQL Server**</span><span class="sxs-lookup"><span data-stu-id="ed4b4-134">**Microsoft JDBC Driver 6.1 for SQL Server**</span></span> | <span data-ttu-id="ed4b4-135">SQL Server と Microsoft Azure SQL Database 向けに、Java Platform Enterprise Edition 8 の JDBC API を提供します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-135">Provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span> | 
   | <span data-ttu-id="ed4b4-136">**Package for Microsoft Azure Libraries for Java**</span><span class="sxs-lookup"><span data-stu-id="ed4b4-136">**Package for Microsoft Azure Libraries for Java**</span></span> | <span data-ttu-id="ed4b4-137">Microsoft Azure の各種サービス (ストレージ、Service Bus、サービス ランタイムなど) にアクセスするための API を提供します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-137">Provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span> | 

1. <span data-ttu-id="ed4b4-138">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-138">Click **Next**.</span></span> <span data-ttu-id="ed4b4-139">(ツールキットのインストール時に異常な遅延が発生する場合は、 **[Contact all update sites during install to find required software (インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる)]** がオフになっていることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-139">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="ed4b4-140">**[Install Details (インストールの詳細)]** ダイアログで、 **[Next (次へ)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-140">In the **Install Details** dialog, click **Next**.</span></span>

   ![Review Installation Details][03]

1. <span data-ttu-id="ed4b4-142">**[Review Licenses (ライセンスの確認)]** ダイアログで、ライセンス契約の条項を確認します。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-142">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="ed4b4-143">ライセンス契約の条項に同意する場合は、 **[I accept the terms of the license agreements (ライセンス契約の条項に同意する)]** をクリックし、 **[Finish (完了)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-143">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="ed4b4-144">(残りの手順は、ライセンス契約の条項に同意したことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-144">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="ed4b4-145">ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)</span><span class="sxs-lookup"><span data-stu-id="ed4b4-145">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>

   ![Review Licenses][04]

   <span data-ttu-id="ed4b4-147">必要なパッケージが Eclipse によってダウンロードされてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-147">Eclipse will download and install the requisite packages.</span></span>

   ![Installation Progress][05]

1. <span data-ttu-id="ed4b4-149">インストールを完了するために Eclipse を再起動することを求めるメッセージが表示されたら、 **[Yes]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ed4b4-149">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>

   ![Restart Prompt][06]

## <a name="next-steps"></a><span data-ttu-id="ed4b4-151">次の手順</span><span class="sxs-lookup"><span data-stu-id="ed4b4-151">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->
[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
