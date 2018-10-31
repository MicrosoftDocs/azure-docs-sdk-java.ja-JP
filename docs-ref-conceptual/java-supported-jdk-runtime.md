---
title: Azure 開発のための Java JDK と長期サポート
description: Java アプリケーションを開発して実行するためのダウンロードと Azure のサポートに関する声明。
author: rloutlaw
manager: angerobe
ms.devlang: java
ms.topic: article
ms.date: 10/15/2017
ms.author: routlaw
ms.openlocfilehash: 2865219f350990e8b07f7d2cd99f536168a6b8d4
ms.sourcegitcommit: 7df2d442ad7cbdb235e5dd35302a9b73379c23d5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026998"
---
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a><span data-ttu-id="ba3b3-103">Azure 向けの開発時に Java JDK のダウンロードとサポートを入手する</span><span class="sxs-lookup"><span data-stu-id="ba3b3-103">Get Java JDK downloads and support when developing for Azure</span></span>

<span data-ttu-id="ba3b3-104">Azure と Azure Stack を使用する Java 開発者は、[OpenJDK の Azul Systems Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)を使用して、運用 Java アプリケーションを構築し、実行できます。追加のサポート コストは発生しません。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-104">Java developers on Azure and Azure Stack can build and run production Java applications using [Azul Systems Zulu Enterprise builds of OpenJDK](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="ba3b3-105">Azure 上で希望の Java ランタイムを使用できますが、Zulu を使用すると、無料のメンテナンス更新プログラムが提供され、[正規の Azure サポート プラン](https://azure.microsoft.com/support/plans/)を利用して、Microsoft によるサポート案件を作成できます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-105">You can use any Java runtime you want on Azure, but when you use Zulu you get free maintenance updates and can create support issues with Microsoft with a  [qualified Azure support plan](https://azure.microsoft.com/support/plans/).</span></span>

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="ba3b3-106">サポートされている Java バージョンと更新スケジュール</span><span class="sxs-lookup"><span data-stu-id="ba3b3-106">Supported Java versions and update schedule</span></span>

<span data-ttu-id="ba3b3-107">Azul Systems は、Java SE 7、8、11 以降の Java のすべての長期サポート (LTS) バージョンに対して、[Microsoft Azure 向けの OpenJDK の完全にサポートされた Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)を提供します。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-107">Azul Systems will provide fully-supported [Zulu Enterprise builds of OpenJDK for Microsoft Azure](https://www.azul.com/downloads/azure-only/zulu/) for all long-term support (LTS) versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="ba3b3-108">詳細については、[Azul のプレス リリース](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)のページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-108">More information can be found in the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>


<span data-ttu-id="ba3b3-109">これらの JDK リリースには、四半期ごとのセキュリティ更新プログラムとバグ修正が含まれ、必要に応じて、重要なアウトオブバンドの更新プログラムと修正プログラムも含まれます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-109">These JDK releases will have quarterly security updates and bug fixes as well as critical out-of-band updates and patches as needed.</span></span>  <span data-ttu-id="ba3b3-110">このサポートには、Java 11 などの新しいバージョンの Java で報告された Java 7 および 8 のセキュリティ更新プログラムとバグ修正のバック ポーティングが含まれており、古いバージョンの Java の継続的な安定性とセキュリティが確保されます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-110">This support includes back porting of security updates and bug fixes to Java 7 and 8 reported in newer versions of Java such as Java 11, and ensures the continued stability and security of older versions of Java.</span></span>  <span data-ttu-id="ba3b3-111">Azure のお客様は、予定外の Java SE サブスクリプション料金を負担することなく、これらのセキュリティ更新プログラムとプラットフォームのバグ修正を入手できます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-111">Azure customers are entitled to these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span> <span data-ttu-id="ba3b3-112">次の図は、Java SE の各バージョンのサポート期間を示しています。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-112">The dates of support for each version of Java SE are highlighted in the image below.</span></span>

![Azure での JDK サポート タイムライン](media/azure-jdk-support.png)

<span data-ttu-id="ba3b3-114">Azul Systems では、これらのリリースの [Java SE ロードマップ](https://www.azul.com/products/azul_support_roadmap/)を管理しています。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-114">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="use-for-local-development"></a><span data-ttu-id="ba3b3-115">ローカル開発に使用</span><span class="sxs-lookup"><span data-stu-id="ba3b3-115">Use for Local development</span></span> 

<span data-ttu-id="ba3b3-116">開発者は、[Azul Systems の Web サイト](https://www.azul.com/downloads/azure-only/zulu/)から、Azure および Azure Stack 用の Java JDK をダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-116">Developers can download Java JDKs for Azure and Azure Stack from [Azul Systems' website](https://www.azul.com/downloads/azure-only/zulu/).</span></span> <span data-ttu-id="ba3b3-117">ダウンロードは、Windows、Linux、macOS で利用できます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-117">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="ba3b3-118">Linux を使用している開発者は、[yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) および [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) パッケージ マネージャーを使ってパッケージを入手することもできます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-118">Developers working on Linux can also get packages through the  [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="ba3b3-119">[正規の Azure サポート プラン](https://azure.microsoft.com/support/plans/)を利用して Azure または Azure Stack 用の開発を行うときには、Azure がサポートする Azul Zulu JDK の製品サポートを利用できます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-119">Product support for the Azure-supported Azul Zulu JDK is available through when developing for Azure or Azure Stack with a [qualified Azure support plan](https://azure.microsoft.com/support/plans/).</span></span>

## <a name="use-in-docker-containers"></a><span data-ttu-id="ba3b3-120">Docker コンテナーでの使用</span><span class="sxs-lookup"><span data-stu-id="ba3b3-120">Use in Docker containers</span></span>

<span data-ttu-id="ba3b3-121">任意のディストリビューションに含まれる OpenJDK の Zulu Enterprise ビルドを使用して、無制限に Docker イメージをビルドできます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-121">You can build unlimited Docker images using Zulu Enterprise builds of OpenJDK on any distros of your choice.</span></span> <span data-ttu-id="ba3b3-122">Azul Zulu Enterprise for Azure JDKs に基づく Zulu Docker イメージは、[Microsoft のパブリック Docker リポジトリ](https://hub.docker.com/r/microsoft/java-jdk/)で入手できます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-122">Zulu Docker images based off the Azul Zulu Enterprise for Azure JDKs are available on [Microsoft's public Docker repository](https://hub.docker.com/r/microsoft/java-jdk/).</span></span> <span data-ttu-id="ba3b3-123">これらのイメージのビルドに使用された Dockerfile は、[Microsoft の Java GitHub リポジトリ](https://github.com/Microsoft/java/tree/master/docker)で入手できます。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-123">The  Dockerfiles that used to build these images are available on [Microsoft's Java GitHub repo](https://github.com/Microsoft/java/tree/master/docker).</span></span>

<span data-ttu-id="ba3b3-124">これらのイメージを使用してアプリをコンテナー化するには、Dockerfile に `FROM` ステートメントを設定し、アプリケーションの依存関係でコンテナーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-124">To containerize your apps using these images, you will need to set a `FROM` statement in your Dockerfile and then configure the container with your application's dependencies.</span></span> <span data-ttu-id="ba3b3-125">たとえば、ポート 8080 にバインドする、JAR ファイルにパッケージ化された Java SE アプリケーションを実行するには、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-125">For example, to run a JAR file packaged Java SE application that binds to port 8080:</span></span>

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a><span data-ttu-id="ba3b3-126">Azure サービス ランタイム サポート</span><span class="sxs-lookup"><span data-stu-id="ba3b3-126">Azure service runtime support</span></span>

<span data-ttu-id="ba3b3-127">[App Service](/azure/app-service/containers/)、[Functions](/azure/azure-functions/functions-create-first-java-maven)、[Service Fabric](/azure/service-fabric/)、[HDInsight](/azure/hdinsight/) などの Azure プラットフォーム サービスでは、セキュリティ修正プログラムとバグ修正が含まれた Java のマイナー リリースの自動適用が組み込まれた、OpenJDK の Zulu Enterprise ビルドを使用しています。</span><span class="sxs-lookup"><span data-stu-id="ba3b3-127">Azure platform services such as [App Service](/azure/app-service/containers/), [Functions](/azure/azure-functions/functions-create-first-java-maven), [Service Fabric](/azure/service-fabric/) and [HDInsight](/azure/hdinsight/)  use Zulu Enterprise builds of OpenJDK with built-in auto-patching of minor releases of Java with security patches and bug fixes.</span></span>