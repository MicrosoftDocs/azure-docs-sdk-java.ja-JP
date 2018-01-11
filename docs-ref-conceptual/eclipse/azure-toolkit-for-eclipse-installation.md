---
title: "Azure Toolkit for Eclipse をインストールする"
description: "Azure Toolkit for Eclipse プラグインをインストールして、クラウド アプリケーションを作成し、Azure にデプロイする方法を説明します。"
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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 54f636f1291832702bfed2b49888b531d358cb73
ms.sourcegitcommit: 9c354a65b0f8ad49a528f40ddee647b091f7d246
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2018
---
# <a name="install-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse をインストールする

Azure Toolkit for Eclipse は、クラウド アプリケーションを簡単に作成、開発、およびテストし、Eclipse 開発環境から Azure にデプロイできるテンプレートと機能を提供します。

> [!NOTE] 
> 
> Azure Toolkit for Eclipse はオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから MIT License で入手できます。URL は次のとおりです。 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

次の手順は、Azure Toolkit for Eclipse をインストールする方法を示しています。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse をインストールするには

1. Eclipse を起動します。

1. 次の図に示すように、**[Help]\(ヘルプ\)** メニューをクリックし、**[Install New Software]\(新しいソフトウェアのインストール\)** をクリックします。
   
   ![Azure Toolkit for Eclipse のインストール][01]

1. **[Available Software]\(利用可能なソフトウェア\)** ダイアログで、**[Work with]\(対象\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、**Enter** キーを押します。

1. **[Name]** ウィンドウで、**Azure Toolkit for Eclipse** をオンにし、**[Contact all update sites during install to find required software]** をオフにします。 画面は次のようになります。
   
   ![Azure Toolkit for Eclipse のインストール][02]

1. **Azure Toolkit for Eclipse** を展開すると、インストールされるコンポーネントの一覧が表示されます。次に例を示します。

   | 機能 | [説明] | 
   |---|---| 
   | **Application Insights Plugin for Java** | アプリケーションとサーバー インスタンスに対する Azure のテレメトリ ログと分析サービスを使用できるようにします。 | 
   | **Azure Common Plugin** | 他のツールキット コンポーネントで必要な一般的な機能を提供します。 | 
   | **Azure Container Tools for Eclipse** | .WAR を Docker コンテナーとして Docker マシンに構築およびデプロイできるようにします。 | 
   | **Azure Containers for Eclipse** | .WAR または.JAR アーティファクトを Docker コンテナーとして Azure 仮想マシンにデプロイできるようにします。 | 
   | **Azure Explorer for Eclipse** | Azure リソースを管理するためのエクスプローラー スタイルのインターフェイスを提供します。 | 
   | **Microsoft JDBC Driver 6.1 for SQL Server** | SQL Server と Microsoft Azure SQL Database 向けに、Java Platform Enterprise Edition 8 の JDBC API を提供します。 | 
   | **Package for Microsoft Azure Libraries for Java** | Microsoft Azure の各種サービス (ストレージ、Service Bus、サービス ランタイムなど) にアクセスするための API を提供します。 | 

1. **[次へ]** をクリックします。 (ツールキットのインストール時に異常な遅延が発生する場合は、**[Contact all update sites during install to find required software (インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる)]** がオフになっていることを確認してください)。

1. **[Install Details (インストールの詳細)]** ダイアログで、**[Next (次へ)]** をクリックします。
   
   ![Review Installation Details][03]

1. **[Review Licenses (ライセンスの確認)]** ダイアログで、ライセンス契約の条項を確認します。 ライセンス契約の条項に同意する場合は、**[I accept the terms of the license agreements (ライセンス契約の条項に同意する)]** をクリックし、**[Finish (完了)]** をクリックします。 (残りの手順は、ライセンス契約の条項に同意したことを前提としています。 ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)
   
   ![Review Licenses][04]
   
   必要なパッケージが Eclipse によってダウンロードされてインストールされます。
   
   ![Installation Progress][05]

1. インストールを完了するために Eclipse を再起動することを求めるメッセージが表示されたら、 **[Yes]**をクリックします。
   
   ![Restart Prompt][06]

## <a name="next-steps"></a>次の手順

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
