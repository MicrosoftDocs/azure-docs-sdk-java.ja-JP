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
ms.openlocfilehash: ce0b003cc7c48c8690f4236547ddec36e6ea9d53
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2017
---
# <a name="azure-tools-for-java-developers"></a>Java 開発者のための Azure ツール

## <a name="client-and-management-libraries"></a>クライアントと管理ライブラリ

アプリケーションから Java 用 Azure ライブラリを使ってサービスに接続して Azure リソースを管理します。 プロジェクトの *pom.xml* に次の依存関係を追加して、Maven プロジェクトに管理ライブラリをインポートしてください。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
```

Java 用 Azure ライブラリの[概要](java-sdk-azure-get-started.md)と[ライブラリの全一覧](java-sdk-azure-install.md)を参照してください。

## <a name="eclipse-and-intellij-plugins"></a>Eclipse と IntelliJ のプラグイン

Azure Toolkit for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) や Azure Toolkit for [IntelliJ](intellij/azure-toolkit-for-intellij.md) を使用して、IDE から Azure リソースを管理したり、アプリをデプロイしたりすることができます。   

![IntelliJ ツールキットで Azure エクスプローラーを表示したところ](media/intelliJ-azure-explorer.png)

[Azure Toolkit for Eclipse の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Azure Toolkit for IntelliJ の概要](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app) 

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure 2.0 CLI は、Azure リソースをコマンド ラインから管理するためのインターフェイスです。 ブラウザーの [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) で使用できるほか、macOS、Linux、Windows に[インストール](https://docs.microsoft.com/cli/azure/install-azure-cli)してコマンド ラインで実行することもできます。

[Azure CLI 2.0 の概要](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)

## <a name="azure-storage-explorer"></a>Azure ストレージ エクスプローラー 

Azure ストレージ アカウント、コンテナー、BLOB/ファイルをデスクトップから管理します。 Azure ストレージ エクスプローラーは現在プレビュー段階であり、Windows、macOS、Linux で動作します。

[Azure ストレージ エクスプローラーの概要](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)