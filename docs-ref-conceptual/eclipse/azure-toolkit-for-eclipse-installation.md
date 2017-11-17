---
title: "Azure Toolkit for Eclipse のインストール"
description: "Azure Toolkit for Eclipse をインストールする方法について説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 59a8bfb6ab4db8ea8c6c9025ca3ced8a13192628
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="9a9e4-103">Azure Toolkit for Eclipse のインストール</span><span class="sxs-lookup"><span data-stu-id="9a9e4-103">Installing the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="9a9e4-104">Azure Toolkit for Eclipse は、Eclipse 開発環境を使って Azure アプリケーションを簡単に作成、開発、テスト、またデプロイできるテンプレートと機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the Eclipse development environment.</span></span> <span data-ttu-id="9a9e4-105">Azure Toolkit for Eclipse はオープン ソース プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-105">The Azure Toolkit for Eclipse is an Open Source project.</span></span> <span data-ttu-id="9a9e4-106">ソース コードは MIT ライセンスで <https://github.com/microsoft/azure-tools-for-java> から入手できます。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-106">The source code is available under the MIT License from <https://github.com/microsoft/azure-tools-for-java>.</span></span>

<span data-ttu-id="9a9e4-107">次の手順は、Azure Toolkit for Eclipse をインストールする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="9a9e4-108">Azure Toolkit for Eclipse をインストールするには</span><span class="sxs-lookup"><span data-stu-id="9a9e4-108">To install the Azure Toolkit for Eclipse</span></span>

1. <span data-ttu-id="9a9e4-109">Eclipse を起動します。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-109">Start Eclipse.</span></span>

1. <span data-ttu-id="9a9e4-110">次の図に示すように、**[Help]\(ヘルプ\)** メニューをクリックし、**[Install New Software]\(新しいソフトウェアのインストール\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
   ![Azure Toolkit for Eclipse のインストール][01]

1. <span data-ttu-id="9a9e4-112">**[Available Software]\(利用可能なソフトウェア\)** ダイアログで、**[Work with]\(対象\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="9a9e4-113">**[Name]** ウィンドウで、**Azure Toolkit for Eclipse** をオンにし、**[Contact all update sites during install to find required software]** をオフにします。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="9a9e4-114">画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-114">Your screen should appear similar to the following:</span></span>
   
   ![Azure Toolkit for Eclipse のインストール][02]

1. <span data-ttu-id="9a9e4-116">**Azure Toolkit for Eclipse** を展開すると、次のような項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-116">If you expand the **Azure Toolkit for Eclipse**, you will see a list of items like following example:</span></span>
   
   * <span data-ttu-id="9a9e4-117">**Application Insights Plugin for Java**: このコンポーネントによって、アプリケーションとサーバー インスタンスに対する Azure のテレメトリ ログと分析サービスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-117">**Application Insights Plugin for Java**: This component allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span>
   * <span data-ttu-id="9a9e4-118">**Azure Access Control Services Filter**: Azure ACS を使用するアプリケーション ユーザーの認証を支援するコンポーネントです。シングル サインオンの利用形態が可能となり、ID のロジックをアプリケーションから切り離すことができます。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-118">**Azure Access Control Services Filter**: This component provides support for authenticating application users with Azure ACS, enabling single sign-on scenarios and externalizing identity logic from the application.</span></span>
   * <span data-ttu-id="9a9e4-119">**Azure Common Plugin**: 他のツールキット コンポーネント全般で必要となる機能を備えたコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-119">**Azure Common Plugin**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="9a9e4-120">**Azure Explorer for Eclipse**: 他のツールキット コンポーネント全般で必要となる機能を備えたコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-120">**Azure Explorer for Eclipse**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="9a9e4-121">**Azure Plugin for Eclipse with Java**: Microsoft Azure クラウド向け Java アプリケーションを Eclipse やコマンド ラインによってビルド、テスト、デプロイできるプロジェクトの開発を支援するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-121">**Azure Plugin for Eclipse with Java**: This component provides support for developing projects that help build, test and deploy Java applications for the Microsoft Azure cloud in Eclipse and via command line.</span></span>
   * <span data-ttu-id="9a9e4-122">**Azure Web Apps Plugin with Java**: Microsoft Azure Web アプリ コンテナーへの Java Web アプリケーションのデプロイを支援するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-122">**Azure Web Apps Plugin with Java**: This component provides support for deploying Java web applications to Microsoft Azure Web App containers.</span></span>
   * <span data-ttu-id="9a9e4-123">**Microsoft JDBC Driver 4.2 for SQL Server**: SQL Server と Microsoft Azure SQL Database 向けに、Java Platform Enterprise Edition 8 の JDBC API を提供するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-123">**Microsoft JDBC Driver 4.2 for SQL Server**: This component provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span>
   * <span data-ttu-id="9a9e4-124">**Package for Apache Qpid Client Libraries for JMS**: Apache Qpid プロジェクトの JMS クライアント ライブラリを備えたコンポーネントです。Microsoft Azure でアプリケーションから AMQP メッセージングを利用するには、このコンポーネントを使用します。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-124">**Package for Apache Qpid Client Libraries for JMS**: This component provides the JMS client component from the Apache Qpid project to enable your application to use AMQP messaging in Microsoft Azure.</span></span>
   * <span data-ttu-id="9a9e4-125">**Package for Microsoft Azure Libraries for Java**: Microsoft Azure の各種サービス (ストレージ、Service Bus、サービス ランタイムなど) にアクセスするための API を備えたコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-125">**Package for Microsoft Azure Libraries for Java**: This component provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span>

1. <span data-ttu-id="9a9e4-126">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-126">Click **Next**.</span></span> <span data-ttu-id="9a9e4-127">(ツールキットのインストール時に異常な遅延が発生する場合は、**[Contact all update sites during install to find required software (インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる)]** がオフになっていることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-127">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="9a9e4-128">**[Install Details (インストールの詳細)]** ダイアログで、**[Next (次へ)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-128">In the **Install Details** dialog, click **Next**.</span></span>
   
   ![Review Installation Details][03]

1. <span data-ttu-id="9a9e4-130">**[Review Licenses (ライセンスの確認)]** ダイアログで、ライセンス契約の条項を確認します。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-130">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="9a9e4-131">ライセンス契約の条項に同意する場合は、**[I accept the terms of the license agreements (ライセンス契約の条項に同意する)]** をクリックし、**[Finish (完了)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-131">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="9a9e4-132">(残りの手順は、ライセンス契約の条項に同意したことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-132">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="9a9e4-133">ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)</span><span class="sxs-lookup"><span data-stu-id="9a9e4-133">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
   ![Review Licenses][04]
   
   <span data-ttu-id="9a9e4-135">必要なパッケージが Eclipse によってダウンロードされてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-135">Eclipse will download and install the requisite packages.</span></span>
   
   ![Installation Progress][05]

1. <span data-ttu-id="9a9e4-137">インストールを完了するために Eclipse を再起動することを求めるメッセージが表示されたら、 **[Yes]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9a9e4-137">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
   ![Restart Prompt][06]

## <a name="next-steps"></a><span data-ttu-id="9a9e4-139">次のステップ</span><span class="sxs-lookup"><span data-stu-id="9a9e4-139">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->

[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
