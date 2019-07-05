---
title: Azure 開発のための Java JDK と長期サポート
description: この記事では、Java アプリケーションを開発して実行するためのダウンロード リンクと Azure サポートに関する声明を示します。
author: bmitchell287
manager: douge
ms.devlang: java
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: brendm
ms.openlocfilehash: 1bb93f775686b5b0f127c586dda802f4eb5f775d
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533633"
---
# <a name="java-long-term-support-for-azure-and-azure-stack"></a>Azure および Azure Stack の Java 長期サポート

Microsoft Azure と Azure Stack を使用する Java 開発者は、[Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) を使用して、運用 Java アプリケーションをビルドして実行できます。追加のサポート コストは発生しません。 Azure 上で希望の Java ランタイムを使用できますが、Zulu を使用すると、無料のメンテナンス更新プログラムが提供され、Microsoft でサポートに関する問題を解決できます。

> [!div class="nextstepaction"]
> [Java のダウンロードとインストール](java-jdk-install.md)

## <a name="long-term-support"></a>長期サポート

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/#java11)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/#java8)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/#java7)

## <a name="technical-preview"></a>テクニカル プレビュー

* [Java 12](https://www.azul.com/downloads/azure-only/zulu/#java12)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>Azure の Zulu OpenJDK とは何ですか?

OpenJDK の Azul Zulu Enterprise ビルドは、Microsoft および Azul Systems でサポートされる、Azure と Azure Stack 用の OpenJDK の無料でマルチプラット フォーム対応の実稼働可能なディストリビューションです。 これらのディストリビューションは次のような特長があります。

* Java Development Kit (JDK)、Java Runtime Environment (JRE)、ヘッドレス JRE としてパッケージ化された、100% オープンソースの OpenJDK のビルド。 これらのバイナリは、Java Standard Edition (SE) の完全に互換性のある、準拠している商用ビルドであり、Azure と Azure Stack 上の Java アプリケーションまたはコンポーネントで使用できます。
* バグの修正、パフォーマンス強化、セキュリティ パッチを含む、長期サポート (LTS) が付属。
* Windows、Linux、macOS での Java アプリケーションの開発および実行で使用可能。
* Docker Hub のコンテナー イメージおよび Azure Marketplace の仮想マシン (Windows および Linux) として使用可能。
* 次のような多くの Azure サービスを稼働させるために Azure で使用。
  * Azure App Service (Windows)
  * Azure App Service (Linux)
  * Azure Functions
  * Azure Service Fabric
  * Azure HDInsight
  * Azure Search
  * Azure DevOps
  * Azure Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>サポートされている Java バージョンと更新スケジュール

Azul Systems では、Java SE 7、8、11 以降の Java のすべての LTS バージョンに対して、[Azure 向けの OpenJDK の完全にサポートされた Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)が提供されます。 詳細については、[Azul のプレス リリース](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)を参照してください。

|Java SE LTS  |サポート期限  |
|---------|----------|
|[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7) |2023 年 7 月 |
|[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8) |2025 年 3 月|
|[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11) |2026 年 9 月|
|[![Java 12](../media/jdk/java-12.png)]() |**プレビュー**|

これらの JDK リリースには、四半期ごとのセキュリティ更新プログラムとバグ修正が含まれ、必要に応じて、重要なアウトオブバンドの更新プログラムと修正プログラムも含まれます。 このサポートには、Java 11 などの新しいバージョンの Java で報告されたセキュリティ更新プログラムとバグ修正の Java 7 および 8 へのバック ポーティングが含まれています。 このサポートにより古いバージョンの Java の継続的な安定性とセキュリティが確保されます。 Azure のお客様は、予定外の Java SE サブスクリプション料金を負担することなく、これらのセキュリティ更新プログラムとプラットフォームのバグ修正を入手できます。

Azul Systems では、これらのリリースの [Java SE ロードマップ](https://www.azul.com/products/azul_support_roadmap/)を管理しています。

## <a name="benefits-for-developers"></a>開発者にとっての利点

Azul Zulu JDK のリリースには、次の特長があります。

* Microsoft と Azul Systems の両方でサポートされます。

   * Zulu のバイナリは実稼働可能であり、Microsoft と Azul Systems でサポートされます。
   * Zulu には、Java 7、8、および 11 の無料の LTS が付属します (LTS は Java 17 にも提供されます)。 Java のバージョンは、必要なときにのみアップグレードできます。
   * Java 7 は 2023 年 7 月までサポートされます。 Java 8 および 11 は 2024 年以降もサポートされます。
   * Microsoft は、多くの Azure サービスを稼働するマシンで Zulu を内部的に実行することに取り組んでいます。

* 実稼働可能。

   * OpenJDK のビルドで 100% オープンソース。
   * 多くの Java SE ディストリビューションに対する簡単な置き換え。
   * JDK、JRE および JRE ヘッドレス。
   * Java 7、8、および 11。
   * OpenJDK Community Technology Compatibility Kit (TCK) を使用する Java SE 仕様に準拠していることを確認済み。
   * 開発者には、Java SE 向けの運用更新プログラムの提供が継続され、これには Java SE 7、8、11 のバグ修正、パフォーマンス強化、およびセキュリティ修正プログラムが含まれます。

* マルチプラット フォームのサポート。 Zulu では、次のような複数のプラットフォームとバージョンのバイナリがサポートされています。

   * Windows クライアント
     * 10
     * 8.1
     * 8、7
   * Windows Server
     * 2016 R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux (次を含む)
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * macOS X
   * 複数のパッケージの種類で配信:
     * MSI、ZIP、TAR、DEB、RPM、および DMG

    複数のベース OS イメージ上の Zulu JDK、JRE および JRE ヘッドレス用の認定済み Docker コンテナー イメージを Docker で利用できます。

    ハブ:

    * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
    * [JRE](https://hub.docker.com/_/microsoft-java-jre)
    * [JRE ヘッドレス](https://hub.docker.com/_/microsoft-java-jre-headless)

* コストがかかりません。

   * Microsoft は、Azure 上の Java アプリをビルドおよび拡張するために必要なものをすべて無料で提供します。 Zulu を通じて、Java アプリ用のセキュリティ更新プログラムとプラットフォームのバグ修正が無料で提供されます。
   * [Java Flight Recorder および Mission Control](java-jdk-flight-recorder-and-mission-control.md) は、Zulu Java 8、11、および 12 (プレビュー) で利用できます。

* 非 LTS バージョンのテクニカル プレビューとして使用できます。

   * テクニカル プレビューでは、新機能が最終的に Java 17 LTS となる短期的なバージョンで提供される中で、それらを斬新的にテストする機会を得られます。

* OpenJDK に対する変更が上流に送信されます。

   * Azul Systems のコミッターが Zulu の変更を OpenJDK にプッシュするため、上流のリポジトリが包括的に維持されます。

Java 開発者はこれまでどおり、独自の Java ランタイム (Oracle JDK と Red Hat JDK を含む) を Azure に取り込むことができます。 セキュリティで保護されたインフラストラクチャと機能豊富なサービスを使用することもできます。 Oracle Java SE の運用エディションは、Azure 上の Windows または Linux 仮想マシンで Java ワークロードの実行に使用できます。

## <a name="use-java-jdks-for-local-development"></a>ローカル開発に Java JDK を使用する 

[Azure および Azure Stack 用の Java JDK をダウンロード](https://www.azul.com/downloads/azure-only/zulu/)して、ローカル開発環境で使用できます。 ダウンロードは、Windows、Linux、macOS で利用できます。 Linux で作業している場合は、[yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) および [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) パッケージ マネージャーを使ってパッケージを入手することもできます。

追加のガイダンスについては、[Azure 用の Docker イメージ](java-jdk-docker-images.md)に関する記事を参照してください。
