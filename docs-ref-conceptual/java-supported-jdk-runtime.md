---
title: Azure 開発のための Java JDK と長期サポート
description: Java アプリケーションを開発して実行するためのダウンロードと Azure のサポートに関する声明。
author: rloutlaw
manager: angerobe
ms.devlang: java
ms.topic: article
ms.date: 10/26/2017
ms.author: routlaw
ms.openlocfilehash: 7f75b26bffc02a161e8d58827970bd80a3a6c48a
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235208"
---
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a>Azure 向けの開発時に Java JDK のダウンロードとサポートを入手する

Azure と Azure Stack を使用する Java 開発者は、[Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) を使用して、運用 Java アプリケーションを構築し、実行できます。追加のサポート コストは発生しません。 Azure 上で希望の Java ランタイムを使用できますが、Zulu を使用すると、無料のメンテナンス更新プログラムが提供され、[正規の Azure サポート プラン](https://azure.microsoft.com/support/plans/)を利用して、Microsoft によるサポート案件を作成できます。

開発者は Oracle JDK、Red Hat JDK などの独自の Java ランタイムを使用して、自身のアプリを Azure で実行し、Azure のサービスと機能に接続できます。 Oracle Java SE の運用エディションは、Azure の Windows または Linux 仮想マシンでワークロードを実行している Java 開発者が引き続き利用できます。

## <a name="supported-java-versions-and-update-schedule"></a>サポートされている Java バージョンと更新スケジュール

Azul Systems は、Java SE 7、8、11 以降の Java のすべての長期サポート (LTS) バージョンに対して、[Microsoft Azure 向けの OpenJDK の完全にサポートされた Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)を提供します。 詳細については、[Azul のプレス リリース](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)のページをご覧ください。


これらの JDK リリースには、四半期ごとのセキュリティ更新プログラムとバグ修正が含まれ、必要に応じて、重要なアウトオブバンドの更新プログラムと修正プログラムも含まれます。  このサポートには、Java 11 などの新しいバージョンの Java で報告された Java 7 および 8 のセキュリティ更新プログラムとバグ修正のバック ポーティングが含まれており、古いバージョンの Java の継続的な安定性とセキュリティが確保されます。  Azure のお客様は、予定外の Java SE サブスクリプション料金を負担することなく、これらのセキュリティ更新プログラムとプラットフォームのバグ修正を入手できます。 次の図は、Java SE の各バージョンのサポート期間を示しています。

![Azure での JDK サポート タイムライン](media/azure-jdk-support.png)

Azul Systems では、これらのリリースの [Java SE ロードマップ](https://www.azul.com/products/azul_support_roadmap/)を管理しています。

## <a name="use-for-local-development"></a>ローカル開発に使用 

開発者は、Azure および Azure Stack 用の Java JDK を[ダウンロード](https://www.azul.com/downloads/azure-only/zulu/)して、ローカル開発環境で使用できます。 ダウンロードは、Windows、Linux、macOS で利用できます。 Linux を使用している開発者は、[yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) および [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) パッケージ マネージャーを使ってパッケージを入手することもできます。

Azure または Azure Stack 用の開発を行う際は、[正規の Azure サポート プラン](https://azure.microsoft.com/support/plans/)で JDK 用のローカル開発のサポートを利用できます。

## <a name="use-in-docker-containers"></a>Docker コンテナーでの使用

任意のディストリビューションに含まれる OpenJDK の Zulu Enterprise ビルドを使用して、無制限に Docker イメージをビルドできます。 Azul Zulu Enterprise for Azure JDKs に基づく Zulu Docker イメージは、[Microsoft のパブリック Docker リポジトリ](https://hub.docker.com/r/microsoft/java-jdk/)で入手できます。 これらのイメージのビルドに使用された Dockerfile は、[Microsoft の Java GitHub リポジトリ](https://github.com/Microsoft/java/tree/master/docker)で入手できます。

これらのイメージを使用してアプリをコンテナー化するには、Dockerfile に `FROM` ステートメントを設定し、アプリケーションの依存関係でコンテナーを構成する必要があります。 たとえば、ポート 8080 にバインドする、JAR ファイルにパッケージ化された Java SE アプリケーションを実行するには、次のコードを使用します。

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a>Azure サービス ランタイム サポート

[App Service](/azure/app-service/containers/)、[Functions](/azure/azure-functions/functions-create-first-java-maven)、[Service Fabric](/azure/service-fabric/)、[HDInsight](/azure/hdinsight/) などの Azure プラットフォーム サービスでは、セキュリティ修正プログラムとバグ修正が含まれた Java のマイナー リリースの自動適用が組み込まれた、OpenJDK の Zulu Enterprise ビルドを使用しています。