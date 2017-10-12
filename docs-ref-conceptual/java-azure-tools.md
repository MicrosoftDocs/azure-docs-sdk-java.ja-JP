---
title: "Azure Java 開発者のためのツール | Microsoft Docs"
description: "Azure を使用する Java 開発者のための IDE 統合、エミュレーター、リソース エクスプローラー、コマンド ライン インターフェイスについて取り上げます。"
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: ff3ea805daefb3c0a413b109e431d2235a5dc5b8
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2017
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="933b5-103">Java 開発者のための Azure ツール</span><span class="sxs-lookup"><span data-stu-id="933b5-103">Azure tools for Java developers</span></span>

## <a name="client-and-management-libraries"></a><span data-ttu-id="933b5-104">クライアントと管理ライブラリ</span><span class="sxs-lookup"><span data-stu-id="933b5-104">Client and management libraries</span></span>

<span data-ttu-id="933b5-105">アプリケーションから Java 用 Azure ライブラリを使ってサービスに接続して Azure リソースを管理します。</span><span class="sxs-lookup"><span data-stu-id="933b5-105">Connect to services and manage Azure resources from your applications with the Azure libraries for Java.</span></span> <span data-ttu-id="933b5-106">プロジェクトの *pom.xml* に次の依存関係を追加して、Maven プロジェクトに管理ライブラリをインポートしてください。</span><span class="sxs-lookup"><span data-stu-id="933b5-106">Import the management libraries into your Maven projects by adding this dependency to your project *pom.xml*.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

<span data-ttu-id="933b5-107">Java 用 Azure ライブラリの[概要](java-sdk-azure-get-started.md)と[ライブラリの全一覧](java-sdk-azure-install.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="933b5-107">View the [complete list of libraries](java-sdk-azure-install.md) and [get started](java-sdk-azure-get-started.md) with the Azure libraries for Java.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="933b5-108">Eclipse と IntelliJ のプラグイン</span><span class="sxs-lookup"><span data-stu-id="933b5-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="933b5-109">Azure Toolkit for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) や Azure Toolkit for [IntelliJ](intellij/azure-toolkit-for-intellij.md) を使用して、IDE から Azure リソースを管理したり、アプリをデプロイしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="933b5-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) and [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span></span>   

![IntelliJ ツールキットで Azure エクスプローラーを表示したところ](media/intelliJ-azure-explorer.png)

<span data-ttu-id="933b5-111">[Azure Toolkit for Eclipse の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Azure Toolkit for IntelliJ の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span><span class="sxs-lookup"><span data-stu-id="933b5-111">[Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span></span> 

## <a name="azure-cli-20"></a><span data-ttu-id="933b5-112">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="933b5-112">Azure CLI 2.0</span></span>

<span data-ttu-id="933b5-113">Azure 2.0 CLI は、Azure リソースをコマンド ラインから管理するためのインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="933b5-113">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="933b5-114">ブラウザーの [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) で使用できるほか、macOS、Linux、Windows に[インストール](https://docs.microsoft.com/cli/azure/install-azure-cli)してコマンド ラインで実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="933b5-114">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="933b5-115">[Azure CLI 2.0 の概要](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="933b5-115">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="933b5-116">Azure ストレージ エクスプローラー</span><span class="sxs-lookup"><span data-stu-id="933b5-116">Azure Storage Explorer</span></span> 

<span data-ttu-id="933b5-117">Azure ストレージ アカウント、コンテナー、BLOB/ファイルをデスクトップから管理します。</span><span class="sxs-lookup"><span data-stu-id="933b5-117">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="933b5-118">Azure ストレージ エクスプローラーは現在プレビュー段階であり、Windows、macOS、Linux で動作します。</span><span class="sxs-lookup"><span data-stu-id="933b5-118">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="933b5-119">Azure ストレージ エクスプローラーの概要</span><span class="sxs-lookup"><span data-stu-id="933b5-119">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)