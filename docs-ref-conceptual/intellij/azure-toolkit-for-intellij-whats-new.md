---
title: "Azure Toolkit for IntelliJ の新機能"
description: "Azure Toolkit for IntelliJ の最新の機能について説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 10/19/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 3e3d22cc5720346e31a096adfd935b2712aac4e9
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ の新機能

## <a name="azure-toolkit-for-intellij-releases"></a>Azure Toolkit for IntelliJ リリース
この記事では、Azure Toolkit for IntelliJ の各種リリースと最新情報について記載しています。

> [!NOTE]
> 最新情報については、次の Web ページを参照してください。
> 
> <https://github.com/Microsoft/azure-tools-for-java/releases>

> [!NOTE]
> Eclipse IDE 用の Azure Toolkit もあります。 詳細については、「 [Azure Toolkit for Eclipse]」をご覧ください。
> 
> 

### <a name="april-14-2017"></a>2017 年 4 月 14 日
Azure Toolkit for IntelliJ - April 2017 リリースでは、次の点が強化されています。

* **Azure サインイン エクスペリエンスの向上**: Azure Toolkit for IntelliJ では、Azure アカウントへの 2 つのログイン方法 ("*対話型*" と "*自動*") がサポートされるようになりました。 詳しくは、「[Azure Toolkit for IntelliJ の Azure サインイン手順]」をご覧ください。
* **Docker コンテナーを使用した発行**: Azure Toolkit for IntelliJ を使用して、Web アプリケーションを Docker コンテナーとして発行できるようになりました。 詳しくは、「[Azure Toolkit for IntelliJ を使用して、Web アプリを Docker コンテナーとして発行する方法]」をご覧ください。
* **ストレージ アカウント管理**: Azure Toolkit for IntelliJ は、Azure Explorer ビューからのストレージ アカウントの管理をサポートするようになりました。 詳しくは、「[Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する]」をご覧ください。
* **仮想マシン管理**: Azure Toolkit for IntelliJ は、Azure Explorer ツール ウィンドウからの仮想マシンの管理をサポートするようになりました。 詳しくは、「[Azure Explorer for IntelliJ を使用して仮想マシンを管理する]」をご覧ください。
* **リモート デバッグ サポートの削除**。 Azure App Service での Java Web アプリのリモート デバッグは、Azure Toolkit for IntelliJ から削除されました。これは、顧客がツールキットを使用するときに発生していたいくつかの問題を解決するために必要でした。

### <a name="august-26-2016"></a>2016 年 8 月 26 日
Azure Toolkit for IntelliJ - August 2016 リリースでは、次の点が強化されています。

* **カスタム JDK ディストリビューション**。 Azure Toolkit for IntelliJ は、Azure WebApp コンテナーに対する任意の JDK バージョンの指定およびデプロイをサポートするようになりました。
  * Azure によって提供されている JDK に加えて、Azul Systems から提供されている Azure で利用できる Zulu OpenJDK の多様なバージョンから選択できます。
  * ZIP ファイルとしてストレージ アカウントにアップロードする場合は、独自の JDK ディストリビューションも指定できます。
* **Azure 用エクスプローラー ビューの機能強化**:
  * Azure の新しい Resource Manager モデルを使用した仮想マシンの管理をサポートします。IDE を離れずにリソース マネージャー ベースの仮想マシンを一覧表示、作成、削除できます。
  * Azure Resource Manager を使用したストレージ アカウントの BLOB 管理をサポートします。これは、「クラシック」ストレージ アカウントを管理するための既存の機能を補完するものです。
* **Microsoft JDBC Driver 6.0 for SQL Server**。 この更新は、Microsoft SQL Server (v6.0) の最新の JDBC ドライバーを組み込みます。このドライバーは、Java プロジェクトに簡単に追加できるライブラリとして含まれて、以前のバージョンから置き換わるようになりました。

### <a name="june-29-2016"></a>2016 年 6 月 29 日
Azure Toolkit for IntelliJ - June 2016 リリースでは、次の点が強化されています。

* **Java 8 の要件**。 Azure Toolkit for IntelliJ には、現在、Java 8 が必要です。ただし、この要件はツールキットにのみ適用されます。 - アプリケーションは、Azure でサポートされている Java のすべてのバージョンを引き続き使用できます。
* **最新の Java JDK のサポート**。 Java JDK の最新バージョンは、現在、Azure Toolkit for IntelliJ でサポートされています。
* **Azure SDK v2.9.1 のサポート**。 最新バージョンの Azure SDK が、Azure Toolkit for IntelliJ を使用するための最低限の前提条件になりました。
* **統合サンプル**。 Azure Toolkit for IntelliJ には、現在、開発者の作業開始に役立ついくつかのサンプル アプリケーションが特徴付けられています。
* **HDInsight ツールの統合**。 Azure の HDInsight ツールは、現在、Azure Toolkit for IntelliJ にバンドルされています。 詳細については、 [IntelliJ 用の HDInsight Tools プラグイン]に関するページを参照してください。
* **Java Web アプリのリモート デバッグ**。 Azure Toolkit for IntelliJ には、現在、Azure App Service での Java Web アプリのリモート デバッグがサポートされています。

### <a name="april-12-2016"></a>2016 年 4 月 12 日
Azure Toolkit for IntelliJ - April 2016 リリースでは、次の点が強化されています。

* **Azure SDK v2.9.0 のサポート**。 最新バージョンの Azure SDK が、Azure Toolkit for IntelliJ を使用するための最低限の前提条件になりました。
* **Azure Web アプリのサポートに関連する、その他の操作性、応答性、およびパフォーマンスの強化**。 Toolkit が Azure と通信する際のパフォーマンスがさまざまな点で最適化されたため、より応答性の高い UI となっています。
* **IntelliJ 内から Azure の既存の Web アプリケーション コンテナーを削除する機能**。 Azure Toolkit for IntelliJ を使用して、IntelliJ を離れることなく既存の Azure Web コンテナーを削除できるようになりました。

## <a name="next-steps"></a>次のステップ

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md

[Azure Toolkit for IntelliJ の Azure サインイン手順]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Azure Toolkit for IntelliJ を使用して、Web アプリを Docker コンテナーとして発行する方法]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[Azure Explorer for IntelliJ を使用して仮想マシンを管理する]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Azure Java Developer Center]: https://docs.microsoft.com/java/azure

[IntelliJ 用の HDInsight Tools プラグイン]: /azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin
