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
# <a name="azure-tools-for-java-developers"></a>Java 開発者のための Azure ツール

## <a name="supported-jdk-runtimes"></a>サポートされている JDK ランタイム

Azure と Azure Stack を使用する Java 開発者は、[OpenJDK の Azul Systems Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)を使用して、運用 Java 7、8、11 アプリケーションを構築し、実行できます。追加のサポート コストは発生しません。 現在、他の JDK で Java アプリを実行している場合は、無料のサポートとメンテナンスを利用できるように、Azure 上の Zulu に移行することを検討してください。 

Azure でサポートされる Java ランタイムの詳細については、[こちら](java-supported-jdk-runtime.md)をご覧ください。

## <a name="eclipse-and-intellij-plugins"></a>Eclipse と IntelliJ のプラグイン

Azure Toolkit for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) や Azure Toolkit for [IntelliJ](intellij/azure-toolkit-for-intellij.md) を使用して、IDE から Azure リソースを管理したり、アプリをデプロイしたりすることができます。   

![IntelliJ ツールキットで Azure エクスプローラーを表示したところ](media/intelliJ-azure-explorer.png)

[Azure Toolkit for Eclipse の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Azure Toolkit for IntelliJ の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app) 

## <a name="visual-studio-code"></a>Visual Studio Code

[VS Code](https://code.visualstudio.com/) は、MacOS、Windows、Linux で使用できる、軽量でありながら強力なコード エディターです。 VS Code は、プロジェクト サポート、コード補完、デバッグ、lint 処理、ナビゲーションを提供する一連の拡張機能によって、シンプルな最新の Java 開発ワークフローをサポートします。

[VS Code と Java のファースト ステップ ガイド](https://code.visualstudio.com/docs/java)
[VS Code 用 Java 拡張パック](https://code.visualstudio.com/docs/java/extensions)  

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure 2.0 CLI は、Azure リソースをコマンド ラインから管理するためのインターフェイスです。 ブラウザーの [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) で使用できるほか、macOS、Linux、Windows に[インストール](https://docs.microsoft.com/cli/azure/install-azure-cli)してコマンド ラインで実行することもできます。

[Azure CLI 2.0 の概要](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)

## <a name="azure-storage-explorer"></a>Azure ストレージ エクスプローラー 

Azure ストレージ アカウント、コンテナー、BLOB/ファイルをデスクトップから管理します。 Azure ストレージ エクスプローラーは現在プレビュー段階であり、Windows、macOS、Linux で動作します。

[Azure ストレージ エクスプローラーの概要](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
