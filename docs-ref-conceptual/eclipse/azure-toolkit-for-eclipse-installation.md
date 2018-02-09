---
title: "Azure Toolkit for Eclipse をインストールする"
description: "Azure Toolkit for Eclipse プラグインをインストールして、クラウド アプリケーションを作成し、Azure にデプロイする方法を説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 81a784a09c07e0ace4d12989c745c80f55cd70cd
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
---
# <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="2a9bb-103">Azure Toolkit for Eclipse をインストールする</span><span class="sxs-lookup"><span data-stu-id="2a9bb-103">Install the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="2a9bb-104">Azure Toolkit for Eclipse は、Eclipse 開発環境からクラウド アプリケーションを簡単に作成、開発、およびテストし、Azure にデプロイできるようにするテンプレートと機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy cloud applications to Azure from the Eclipse development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="2a9bb-105">Azure Toolkit for Eclipse はオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから MIT License で入手できます。URL は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-105">The Azure Toolkit for Eclipse is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <span data-ttu-id="2a9bb-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="2a9bb-106"><https://github.com/microsoft/azure-tools-for-java></span></span> 
> 

<span data-ttu-id="2a9bb-107">次の手順は、Azure Toolkit for Eclipse をインストールする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="2a9bb-108">Azure Toolkit for Eclipse をインストールするには</span><span class="sxs-lookup"><span data-stu-id="2a9bb-108">To install the Azure Toolkit for Eclipse</span></span>

1. <span data-ttu-id="2a9bb-109">Eclipse を起動します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-109">Start Eclipse.</span></span>

1. <span data-ttu-id="2a9bb-110">次の図に示すように、**[Help]\(ヘルプ\)** メニューをクリックし、**[Install New Software]\(新しいソフトウェアのインストール\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
   ![Azure Toolkit for Eclipse のインストール][01]

1. <span data-ttu-id="2a9bb-112">**[Available Software]\(利用可能なソフトウェア\)** ダイアログで、**[Work with]\(対象\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="2a9bb-113">**[Name]** ウィンドウで、**Azure Toolkit for Eclipse** をオンにし、**[Contact all update sites during install to find required software]** をオフにします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="2a9bb-114">画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-114">Your screen should appear similar to the following:</span></span>
   
   ![Azure Toolkit for Eclipse のインストール][02]

1. <span data-ttu-id="2a9bb-116">**Azure Toolkit for Eclipse** を展開すると、インストールされるコンポーネントの一覧が表示されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-116">If you expand **Azure Toolkit for Eclipse**, you will see a list of components that will be installed; for example:</span></span>

   | <span data-ttu-id="2a9bb-117">Feature</span><span class="sxs-lookup"><span data-stu-id="2a9bb-117">Feature</span></span> | <span data-ttu-id="2a9bb-118">[説明]</span><span class="sxs-lookup"><span data-stu-id="2a9bb-118">Description</span></span> | 
   |---|---| 
   | <span data-ttu-id="2a9bb-119">**Application Insights Plugin for Java**</span><span class="sxs-lookup"><span data-stu-id="2a9bb-119">**Application Insights Plugin for Java**</span></span> | <span data-ttu-id="2a9bb-120">アプリケーションとサーバー インスタンスに対する Azure のテレメトリ ログと分析サービスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-120">Allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span> | 
   | <span data-ttu-id="2a9bb-121">**Azure Common Plugin**</span><span class="sxs-lookup"><span data-stu-id="2a9bb-121">**Azure Common Plugin**</span></span> | <span data-ttu-id="2a9bb-122">他のツールキット コンポーネントで必要な一般的な機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-122">provides the common functionality needed by other toolkit components.</span></span> | 
   | <span data-ttu-id="2a9bb-123">**Azure Container Tools for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="2a9bb-123">**Azure Container Tools for Eclipse**</span></span> | <span data-ttu-id="2a9bb-124">.WAR を Docker コンテナーとして Docker マシンに構築およびデプロイできるようにします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-124">Enables you to build and deploy a .WAR as a Docker container to a docker machine.</span></span> | 
   | <span data-ttu-id="2a9bb-125">**Azure Containers for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="2a9bb-125">**Azure Containers for Eclipse**</span></span> | <span data-ttu-id="2a9bb-126">.WAR または.JAR アーティファクトを Docker コンテナーとして Azure 仮想マシンにデプロイできるようにします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-126">Enables you to deploy a .WAR or .JAR artifact as a Docker container to an Azure virtual machine.</span></span> | 
   | <span data-ttu-id="2a9bb-127">**Azure Explorer for Eclipse**</span><span class="sxs-lookup"><span data-stu-id="2a9bb-127">**Azure Explorer for Eclipse**</span></span> | <span data-ttu-id="2a9bb-128">Azure リソースを管理するためのエクスプローラー スタイルのインターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-128">Provides an explorer-style interface for managing your Azure resources.</span></span> | 
   | <span data-ttu-id="2a9bb-129">**Microsoft JDBC Driver 6.1 for SQL Server**</span><span class="sxs-lookup"><span data-stu-id="2a9bb-129">**Microsoft JDBC Driver 6.1 for SQL Server**</span></span> | <span data-ttu-id="2a9bb-130">SQL Server と Microsoft Azure SQL Database 向けに、Java Platform Enterprise Edition 8 の JDBC API を提供します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-130">Provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span> | 
   | <span data-ttu-id="2a9bb-131">**Package for Microsoft Azure Libraries for Java**</span><span class="sxs-lookup"><span data-stu-id="2a9bb-131">**Package for Microsoft Azure Libraries for Java**</span></span> | <span data-ttu-id="2a9bb-132">Microsoft Azure の各種サービス (ストレージ、Service Bus、サービス ランタイムなど) にアクセスするための API を提供します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-132">Provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span> | 

1. <span data-ttu-id="2a9bb-133">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-133">Click **Next**.</span></span> <span data-ttu-id="2a9bb-134">(ツールキットのインストール時に異常な遅延が発生する場合は、**[Contact all update sites during install to find required software (インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる)]** がオフになっていることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-134">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="2a9bb-135">**[Install Details (インストールの詳細)]** ダイアログで、**[Next (次へ)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-135">In the **Install Details** dialog, click **Next**.</span></span>
   
   ![Review Installation Details][03]

1. <span data-ttu-id="2a9bb-137">**[Review Licenses (ライセンスの確認)]** ダイアログで、ライセンス契約の条項を確認します。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-137">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="2a9bb-138">ライセンス契約の条項に同意する場合は、**[I accept the terms of the license agreements (ライセンス契約の条項に同意する)]** をクリックし、**[Finish (完了)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-138">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="2a9bb-139">(残りの手順は、ライセンス契約の条項に同意したことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-139">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="2a9bb-140">ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)</span><span class="sxs-lookup"><span data-stu-id="2a9bb-140">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
   ![Review Licenses][04]
   
   <span data-ttu-id="2a9bb-142">必要なパッケージが Eclipse によってダウンロードされてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-142">Eclipse will download and install the requisite packages.</span></span>
   
   ![Installation Progress][05]

1. <span data-ttu-id="2a9bb-144">インストールを完了するために Eclipse を再起動することを求めるメッセージが表示されたら、 **[Yes]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a9bb-144">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
   ![Restart Prompt][06]

## <a name="next-steps"></a><span data-ttu-id="2a9bb-146">次の手順</span><span class="sxs-lookup"><span data-stu-id="2a9bb-146">Next steps</span></span>

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
