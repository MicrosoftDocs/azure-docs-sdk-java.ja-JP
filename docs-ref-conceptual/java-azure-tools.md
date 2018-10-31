---
title: Azure Java 開発者のためのツール | Microsoft Docs
description: Azure を使用する Java 開発者のための IDE 統合、エミュレーター、リソース エクスプローラー、コマンド ライン インターフェイスについて取り上げます。
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: dac0a1c576974a141950919292129890f4e15be4
ms.sourcegitcommit: 19876d17fed0afd9af0cb8e161f5a463696e74cf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2018
ms.locfileid: "49634455"
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="681d4-103">Java 開発者のための Azure ツール</span><span class="sxs-lookup"><span data-stu-id="681d4-103">Azure tools for Java developers</span></span>

## <a name="supported-jdk-runtimes"></a><span data-ttu-id="681d4-104">サポートされている JDK ランタイム</span><span class="sxs-lookup"><span data-stu-id="681d4-104">Supported JDK runtimes</span></span>

<span data-ttu-id="681d4-105">Azure と Azure Stack を使用する Java 開発者は、[OpenJDK の Azul Systems Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)を使用して、運用 Java 7、8、11 アプリケーションを構築し、実行できます。追加のサポート コストは発生しません。</span><span class="sxs-lookup"><span data-stu-id="681d4-105">Java developers on Azure and Azure Stack can build and run production Java 7,8, and 11 applications using [Azul Systems Zulu Enterprise builds of OpenJDK](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="681d4-106">現在、他の JDK で Java アプリを実行している場合は、無料のサポートとメンテナンスを利用できるように、Azure 上の Zulu に移行することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="681d4-106">If you’re currently running Java apps with other JDKs, consider migrating to Zulu on Azure for free support and maintenance.</span></span> 

<span data-ttu-id="681d4-107">Azure でサポートされる Java ランタイムの詳細については、[こちら](java-supported-jdk-runtime.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="681d4-107">[Learn more](java-supported-jdk-runtime.md) about Azure supported Java runtimes.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="681d4-108">Eclipse と IntelliJ のプラグイン</span><span class="sxs-lookup"><span data-stu-id="681d4-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="681d4-109">Azure Toolkit for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) や Azure Toolkit for [IntelliJ](intellij/azure-toolkit-for-intellij.md) を使用して、IDE から Azure リソースを管理したり、アプリをデプロイしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="681d4-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) and [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span></span>   

![IntelliJ ツールキットで Azure エクスプローラーを表示したところ](media/intelliJ-azure-explorer.png)

<span data-ttu-id="681d4-111">[Azure Toolkit for Eclipse の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Azure Toolkit for IntelliJ の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span><span class="sxs-lookup"><span data-stu-id="681d4-111">[Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span></span> 

## <a name="visual-studio-code"></a><span data-ttu-id="681d4-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="681d4-112">Visual Studio Code</span></span>

<span data-ttu-id="681d4-113">[VS Code](https://code.visualstudio.com/) は、MacOS、Windows、Linux で使用できる、軽量でありながら強力なコード エディターです。</span><span class="sxs-lookup"><span data-stu-id="681d4-113">[VS Code](https://code.visualstudio.com/) is a lightweight but powerful code editor available for MacOS, Windows, and Linux.</span></span> <span data-ttu-id="681d4-114">VS Code は、プロジェクト サポート、コード補完、デバッグ、lint 処理、ナビゲーションを提供する一連の拡張機能によって、シンプルな最新の Java 開発ワークフローをサポートします。</span><span class="sxs-lookup"><span data-stu-id="681d4-114">VS Code supports a simple, modern Java development workflow through a set of extensions that provide project support, code completion, debugging, linting, and navigation.</span></span>

<span data-ttu-id="681d4-115">[VS Code と Java のファースト ステップ ガイド](https://code.visualstudio.com/docs/java)
[VS Code 用 Java 拡張パック](https://code.visualstudio.com/docs/java/extensions)</span><span class="sxs-lookup"><span data-stu-id="681d4-115">[Get Started with VS Code and Java](https://code.visualstudio.com/docs/java)
[Java extension pack for VS Code](https://code.visualstudio.com/docs/java/extensions)</span></span>  

## <a name="azure-cli-20"></a><span data-ttu-id="681d4-116">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="681d4-116">Azure CLI 2.0</span></span>

<span data-ttu-id="681d4-117">Azure 2.0 CLI は、Azure リソースをコマンド ラインから管理するためのインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="681d4-117">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="681d4-118">ブラウザーの [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) で使用できるほか、macOS、Linux、Windows に[インストール](https://docs.microsoft.com/cli/azure/install-azure-cli)してコマンド ラインで実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="681d4-118">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="681d4-119">[Azure CLI 2.0 の概要](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="681d4-119">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="681d4-120">Azure ストレージ エクスプローラー</span><span class="sxs-lookup"><span data-stu-id="681d4-120">Azure Storage Explorer</span></span> 

<span data-ttu-id="681d4-121">Azure ストレージ アカウント、コンテナー、BLOB/ファイルをデスクトップから管理します。</span><span class="sxs-lookup"><span data-stu-id="681d4-121">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="681d4-122">Azure ストレージ エクスプローラーは現在プレビュー段階であり、Windows、macOS、Linux で動作します。</span><span class="sxs-lookup"><span data-stu-id="681d4-122">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="681d4-123">Azure ストレージ エクスプローラーの概要</span><span class="sxs-lookup"><span data-stu-id="681d4-123">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
