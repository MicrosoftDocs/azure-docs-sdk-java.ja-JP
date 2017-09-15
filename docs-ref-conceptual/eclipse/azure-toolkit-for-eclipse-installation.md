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
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse のインストール

Azure Toolkit for Eclipse は、Eclipse 開発環境を使って Azure アプリケーションを簡単に作成、開発、テスト、またデプロイできるテンプレートと機能を提供します。 Azure Toolkit for Eclipse はオープン ソース プロジェクトです。 ソース コードは MIT ライセンスで <https://github.com/microsoft/azure-tools-for-java> から入手できます。

次の手順は、Azure Toolkit for Eclipse をインストールする方法を示しています。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse をインストールするには

1. Eclipse を起動します。

1. 次の図に示すように、**[Help]\(ヘルプ\)** メニューをクリックし、**[Install New Software]\(新しいソフトウェアのインストール\)** をクリックします。
   
   ![Azure Toolkit for Eclipse のインストール][01]

1. **[Available Software]\(利用可能なソフトウェア\)** ダイアログで、**[Work with]\(対象\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、**Enter** キーを押します。

1. **[Name]** ウィンドウで、**Azure Toolkit for Eclipse** をオンにし、**[Contact all update sites during install to find required software]** をオフにします。 画面は次のようになります。
   
   ![Azure Toolkit for Eclipse のインストール][02]

1. **Azure Toolkit for Eclipse** を展開すると、次のような項目が表示されます。
   
   * **Application Insights Plugin for Java**: このコンポーネントによって、アプリケーションとサーバー インスタンスに対する Azure のテレメトリ ログと分析サービスを使用できます。
   * **Azure Access Control Services Filter**: Azure ACS を使用するアプリケーション ユーザーの認証を支援するコンポーネントです。シングル サインオンの利用形態が可能となり、ID のロジックをアプリケーションから切り離すことができます。
   * **Azure Common Plugin**: 他のツールキット コンポーネント全般で必要となる機能を備えたコンポーネントです。
   * **Azure Explorer for Eclipse**: 他のツールキット コンポーネント全般で必要となる機能を備えたコンポーネントです。
   * **Azure Plugin for Eclipse with Java**: Microsoft Azure クラウド向け Java アプリケーションを Eclipse やコマンド ラインによってビルド、テスト、デプロイできるプロジェクトの開発を支援するコンポーネントです。
   * **Azure Web Apps Plugin with Java**: Microsoft Azure Web アプリ コンテナーへの Java Web アプリケーションのデプロイを支援するコンポーネントです。
   * **Microsoft JDBC Driver 4.2 for SQL Server**: SQL Server と Microsoft Azure SQL Database 向けに、Java Platform Enterprise Edition 8 の JDBC API を提供するコンポーネントです。
   * **Package for Apache Qpid Client Libraries for JMS**: Apache Qpid プロジェクトの JMS クライアント ライブラリを備えたコンポーネントです。Microsoft Azure でアプリケーションから AMQP メッセージングを利用するには、このコンポーネントを使用します。
   * **Package for Microsoft Azure Libraries for Java**: Microsoft Azure の各種サービス (ストレージ、Service Bus、サービス ランタイムなど) にアクセスするための API を備えたコンポーネントです。

1. **[次へ]** をクリックします。 (ツールキットのインストール時に異常な遅延が発生する場合は、**[Contact all update sites during install to find required software (インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる)]** がオフになっていることを確認してください)。

1. **[Install Details (インストールの詳細)]** ダイアログで、**[Next (次へ)]** をクリックします。
   
   ![Review Installation Details][03]

1. **[Review Licenses (ライセンスの確認)]** ダイアログで、ライセンス契約の条項を確認します。 ライセンス契約の条項に同意する場合は、**[I accept the terms of the license agreements (ライセンス契約の条項に同意する)]** をクリックし、**[Finish (完了)]** をクリックします。 (残りの手順は、ライセンス契約の条項に同意したことを前提としています。 ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)
   
   ![Review Licenses][04]
   
   必要なパッケージが Eclipse によってダウンロードされてインストールされます。
   
   ![Installation Progress][05]

1. インストールを完了するために Eclipse を再起動することを求めるメッセージが表示されたら、 **[Yes]**をクリックします。
   
   ![Restart Prompt][06]

## <a name="next-steps"></a>次のステップ

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
