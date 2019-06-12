---
title: Azure 開発のための Java JDK と長期サポート
description: Java アプリケーションを開発して実行するためのダウンロードと Azure のサポートに関する声明。
author: bmitchell287
manager: douge
ms.devlang: java
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: brendm
ms.openlocfilehash: 340f6149ffe39ccbb2d7de534ed63b8784d1f63a
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270882"
---
# <a name="java-long-term-support-for-azure-and-azure-stack"></a><span data-ttu-id="8ad1b-103">Azure および Azure Stack の Java 長期サポート</span><span class="sxs-lookup"><span data-stu-id="8ad1b-103">Java Long-Term Support for Azure and Azure Stack</span></span>

<span data-ttu-id="8ad1b-104">Azure と Azure Stack を使用する Java 開発者は、[Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) を使用して、運用 Java アプリケーションを構築し、実行できます。追加のサポート コストは発生しません。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-104">Java developers on Azure and Azure Stack can build and run production Java applications using [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="8ad1b-105">Azure 上で希望の Java ランタイムを使用できますが、Zulu を使用すると、無料のメンテナンス更新プログラムが提供され、Microsoft でサポート案件を作成できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-105">You can use any Java runtime you want on Azure, but when you use Zulu you get free maintenance updates and can create support issues with Microsoft.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8ad1b-106">Java のダウンロードとインストール</span><span class="sxs-lookup"><span data-stu-id="8ad1b-106">Download and install Java</span></span>](java-jdk-install.md)

## <a name="long-term-support-lts"></a><span data-ttu-id="8ad1b-107">長期サポート (LTS)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-107">Long-term support (LTS)</span></span>

* [<span data-ttu-id="8ad1b-108">Java 11</span><span class="sxs-lookup"><span data-stu-id="8ad1b-108">Java 11</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java11)
* [<span data-ttu-id="8ad1b-109">Java 8</span><span class="sxs-lookup"><span data-stu-id="8ad1b-109">Java 8</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java8)
* [<span data-ttu-id="8ad1b-110">Java 7</span><span class="sxs-lookup"><span data-stu-id="8ad1b-110">Java 7</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java7)

## <a name="technical-preview"></a><span data-ttu-id="8ad1b-111">テクニカル プレビュー</span><span class="sxs-lookup"><span data-stu-id="8ad1b-111">Technical preview</span></span>

* [<span data-ttu-id="8ad1b-112">Java 12</span><span class="sxs-lookup"><span data-stu-id="8ad1b-112">Java 12</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java12)

## <a name="what-is-the-zulu-openjdk-for-azure"></a><span data-ttu-id="8ad1b-113">Azure の Zulu OpenJDK とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="8ad1b-113">What is the Zulu OpenJDK for Azure?</span></span>

<span data-ttu-id="8ad1b-114">OpenJDK の Azul Zulu Enterprise ビルドは、Microsoft および Azul Systems にサポートされる Azure と Azure Stack の OpenJDK の無料でマルチプラット フォーム対応の実稼働可能なディストリビューションです。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-114">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="8ad1b-115">これらのディストリビューションは次のような特長があります。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-115">These distributions are:</span></span>

* <span data-ttu-id="8ad1b-116">Java Development Kit (JDK)、Java Runtime Environment (JRE) およびヘッドレス JRE としてパッケージ化された、100% オープンソースの OpenJDK のビルド。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-116">100% open source builds of OpenJDK packaged as Java Development Kits (JDKs), Java Runtime Environments (JREs), and Headless JREs.</span></span> <span data-ttu-id="8ad1b-117">これらのバイナリは、Java Standard Edition (SE) の完全に互換性のある、準拠している商用ビルドであり、Azure と Azure Stack 上の Java アプリケーションまたはコンポーネントで使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-117">These binaries are fully compatible and compliant commercial builds of Java Standard Edition (SE) that can be used with Java applications or components on Azure and Azure Stack.</span></span>
* <span data-ttu-id="8ad1b-118">バグの修正、パフォーマンス強化、セキュリティ パッチを含む、長期サポートが付属。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-118">Provided with long-term support including bug fixes, performance enhancements, and security patches.</span></span>
* <span data-ttu-id="8ad1b-119">Windows、Linux、および MacOS での Java アプリケーションの開発および実行で使用可能。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-119">Available for developing and running Java applications on Windows, Linux, and MacOS.</span></span>
* <span data-ttu-id="8ad1b-120">Docker Hub のコンテナー イメージおよび Azure Marketplace の仮想マシン (Windows および Linux) として使用可能。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-120">Available as container images on Docker Hub and as virtual machines (Windows and Linux) on Azure Marketplace.</span></span>
* <span data-ttu-id="8ad1b-121">次のような多くの Azure サービスを稼働させるために Microsoft Azure で使用。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-121">Used by Microsoft Azure to power many Azure services, such as:</span></span>
  * <span data-ttu-id="8ad1b-122">App Service Windows</span><span class="sxs-lookup"><span data-stu-id="8ad1b-122">App Service Windows</span></span>
  * <span data-ttu-id="8ad1b-123">App Service Linux</span><span class="sxs-lookup"><span data-stu-id="8ad1b-123">App Service Linux</span></span>
  * <span data-ttu-id="8ad1b-124">Functions</span><span class="sxs-lookup"><span data-stu-id="8ad1b-124">Functions</span></span>
  * <span data-ttu-id="8ad1b-125">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8ad1b-125">Service Fabric</span></span>
  * <span data-ttu-id="8ad1b-126">HDInsight</span><span class="sxs-lookup"><span data-stu-id="8ad1b-126">HDInsight</span></span>
  * <span data-ttu-id="8ad1b-127">Search</span><span class="sxs-lookup"><span data-stu-id="8ad1b-127">Search</span></span>
  * <span data-ttu-id="8ad1b-128">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="8ad1b-128">Azure DevOps</span></span>
  * <span data-ttu-id="8ad1b-129">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="8ad1b-129">Cloud Shell</span></span>  

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="8ad1b-130">サポートされている Java バージョンと更新スケジュール</span><span class="sxs-lookup"><span data-stu-id="8ad1b-130">Supported Java versions and update schedule</span></span>

<span data-ttu-id="8ad1b-131">Azul Systems は、Java SE 7、8、11 以降の Java のすべての長期サポート (LTS) バージョンに対して、[Microsoft Azure 向けの OpenJDK の完全にサポートされた Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)を提供します。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-131">Azul Systems provides fully-supported [Zulu Enterprise builds of OpenJDK for Microsoft Azure](https://www.azul.com/downloads/azure-only/zulu/) for all long-term support (LTS) versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="8ad1b-132">詳細については、[Azul のプレス リリース](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)のページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-132">More information can be found in the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>

|<span data-ttu-id="8ad1b-133">Java SE LTS</span><span class="sxs-lookup"><span data-stu-id="8ad1b-133">Java SE LTS</span></span>  |<span data-ttu-id="8ad1b-134">サポート期限</span><span class="sxs-lookup"><span data-stu-id="8ad1b-134">Support until</span></span>  |
|---------|----------|
|<span data-ttu-id="8ad1b-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span></span> |<span data-ttu-id="8ad1b-136">2023 年 7 月</span><span class="sxs-lookup"><span data-stu-id="8ad1b-136">July 2023</span></span> |
|<span data-ttu-id="8ad1b-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span></span> |<span data-ttu-id="8ad1b-138">2025 年 3 月</span><span class="sxs-lookup"><span data-stu-id="8ad1b-138">March 2025</span></span>|
|<span data-ttu-id="8ad1b-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span></span> |<span data-ttu-id="8ad1b-140">2026 年 9 月</span><span class="sxs-lookup"><span data-stu-id="8ad1b-140">Sept. 2026</span></span>|
|<span data-ttu-id="8ad1b-141">[![Java 12](../media/jdk/java-12.png)]()</span><span class="sxs-lookup"><span data-stu-id="8ad1b-141">[![Java 12](../media/jdk/java-12.png)]()</span></span> |<span data-ttu-id="8ad1b-142">**プレビュー**</span><span class="sxs-lookup"><span data-stu-id="8ad1b-142">**PREVIEW**</span></span>|

<span data-ttu-id="8ad1b-143">これらの JDK リリースには、四半期ごとのセキュリティ更新プログラムとバグ修正が含まれ、必要に応じて、重要なアウトオブバンドの更新プログラムと修正プログラムも含まれます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-143">These JDK releases have quarterly security updates, bug fixes, and critical out-of-band updates and patches as needed.</span></span>  <span data-ttu-id="8ad1b-144">このサポートには、Java 11 などの新しいバージョンの Java で報告されたセキュリティ更新プログラムとバグ修正の Java 7 および 8 へのバック ポーティングが含まれており、古いバージョンの Java の継続的な安定性とセキュリティが確保されます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-144">This support includes back ports of security updates and bug fixes to Java 7 and 8 reported in newer versions of Java such as Java 11, which ensures the continued stability and security of older versions of Java.</span></span>  <span data-ttu-id="8ad1b-145">Azure のお客様は、予定外の Java SE サブスクリプション料金を負担することなく、これらのセキュリティ更新プログラムとプラットフォームのバグ修正を入手できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-145">Azure customers can get these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span>

<span data-ttu-id="8ad1b-146">Azul Systems では、これらのリリースの [Java SE ロードマップ](https://www.azul.com/products/azul_support_roadmap/)を管理しています。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-146">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="benefits-for-developers"></a><span data-ttu-id="8ad1b-147">開発者にとっての利点</span><span class="sxs-lookup"><span data-stu-id="8ad1b-147">Benefits for developers</span></span>

<span data-ttu-id="8ad1b-148">Azul Zulu JDK のリリースには、次の特長があります。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-148">The Azul Zulu JDK releases are:</span></span>

1. <span data-ttu-id="8ad1b-149">Microsoft と Azul Systems の両方でサポート</span><span class="sxs-lookup"><span data-stu-id="8ad1b-149">Backed and supported by both Microsoft and Azul Systems</span></span>

   * <span data-ttu-id="8ad1b-150">Zulu のバイナリは実稼働可能であり、Microsoft と Azul Systems でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-150">Zulu binaries are production-ready and backed by Microsoft and Azul Systems</span></span>
   * <span data-ttu-id="8ad1b-151">Zulu には、Java 7、8、および 11 の無料の長期サポート (LTS) が付属します。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-151">Zulu comes with zero-cost long-term support (LTS) for Java 7, 8, and 11.</span></span> <span data-ttu-id="8ad1b-152">(LTS は Java 17 にも提供されます)。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-152">(LTS will be provided for Java 17, as well).</span></span> <span data-ttu-id="8ad1b-153">Java のバージョンは、必要なときにのみアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-153">You can upgrade Java versions only when you need to.</span></span>
   * <span data-ttu-id="8ad1b-154">Java 7 は 2023 年 7 月までサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-154">Java 7 supported until July 2023.</span></span> <span data-ttu-id="8ad1b-155">Java 8 および 11 は 2024 年以降もサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-155">Java 8 and 11 are supported beyond 2024.</span></span>
   * <span data-ttu-id="8ad1b-156">Microsoft は、多くの Azure サービスを稼働するマシンで Zulu を内部的に実行することに取り組んでいます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-156">Microsoft is committed to running Zulu internally on machines that power many Azure services.</span></span>

2. <span data-ttu-id="8ad1b-157">実稼働可能</span><span class="sxs-lookup"><span data-stu-id="8ad1b-157">Production-ready</span></span>

   * <span data-ttu-id="8ad1b-158">OpenJDK のビルドで 100% オープンソース。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-158">100% open-source for its builds of OpenJDK.</span></span>
   * <span data-ttu-id="8ad1b-159">多くの Java SE ディストリビューションに対する簡単な置き換え。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-159">Drop-in replacements for many Java SE distributions.</span></span>
   * <span data-ttu-id="8ad1b-160">JDK、JRE および JRE ヘッドレス</span><span class="sxs-lookup"><span data-stu-id="8ad1b-160">JDK, JRE, and JRE-headless</span></span>
   * <span data-ttu-id="8ad1b-161">Java 7、8、および 11</span><span class="sxs-lookup"><span data-stu-id="8ad1b-161">Java 7, 8, and 11</span></span>
   * <span data-ttu-id="8ad1b-162">OpenJDK Community Technology Compatibility Kit (TCK) を使用して Java SE 仕様に準拠していることを確認済み。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-162">Verified compliant with the Java SE  specifications using the OpenJDK Community Technology Compatibility Kit (TCK).</span></span>
   * <span data-ttu-id="8ad1b-163">Java 開発者には、Java SE 向けの運用更新プログラムの提供が継続され、これには Java SE 7、8、11 のバグ修正、パフォーマンス強化、およびセキュリティ修正プログラムが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-163">Developers will continue to receive production updates for Java SE, including bug fixes, performance enhancements, and security patches for Java SE 7, 8, and 11.</span></span>

3. <span data-ttu-id="8ad1b-164">マルチプラット フォームのサポート。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-164">Supported for multi-platform.</span></span> <span data-ttu-id="8ad1b-165">Zulu では、次のような複数のプラットフォームとバージョンのバイナリがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-165">Zulu supports binaries for multiple platforms and versions, including:</span></span>

   * <span data-ttu-id="8ad1b-166">Windows クライアント</span><span class="sxs-lookup"><span data-stu-id="8ad1b-166">Windows Client</span></span>
     * <span data-ttu-id="8ad1b-167">10</span><span class="sxs-lookup"><span data-stu-id="8ad1b-167">10</span></span>
     * <span data-ttu-id="8ad1b-168">8.1</span><span class="sxs-lookup"><span data-stu-id="8ad1b-168">8.1</span></span>
     * <span data-ttu-id="8ad1b-169">8、7</span><span class="sxs-lookup"><span data-stu-id="8ad1b-169">8, 7</span></span>
   * <span data-ttu-id="8ad1b-170">Windows Server</span><span class="sxs-lookup"><span data-stu-id="8ad1b-170">Windows Server</span></span>
     * <span data-ttu-id="8ad1b-171">2016R2</span><span class="sxs-lookup"><span data-stu-id="8ad1b-171">2016R2</span></span>
     * <span data-ttu-id="8ad1b-172">2016</span><span class="sxs-lookup"><span data-stu-id="8ad1b-172">2016</span></span>
     * <span data-ttu-id="8ad1b-173">2012 R2</span><span class="sxs-lookup"><span data-stu-id="8ad1b-173">2012 R2</span></span>
     * <span data-ttu-id="8ad1b-174">2012</span><span class="sxs-lookup"><span data-stu-id="8ad1b-174">2012</span></span>
     * <span data-ttu-id="8ad1b-175">2008 R2</span><span class="sxs-lookup"><span data-stu-id="8ad1b-175">2008 R2</span></span>
   * <span data-ttu-id="8ad1b-176">Linux (次を含む)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-176">Linux, including</span></span>
     * <span data-ttu-id="8ad1b-177">RHEL</span><span class="sxs-lookup"><span data-stu-id="8ad1b-177">RHEL</span></span>
     * <span data-ttu-id="8ad1b-178">CentOS</span><span class="sxs-lookup"><span data-stu-id="8ad1b-178">CentOS</span></span>
     * <span data-ttu-id="8ad1b-179">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="8ad1b-179">Ubuntu</span></span>
     * <span data-ttu-id="8ad1b-180">SLES</span><span class="sxs-lookup"><span data-stu-id="8ad1b-180">SLES</span></span>
     * <span data-ttu-id="8ad1b-181">Debian</span><span class="sxs-lookup"><span data-stu-id="8ad1b-181">Debian</span></span>
     * <span data-ttu-id="8ad1b-182">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="8ad1b-182">Oracle Linux</span></span>
   * <span data-ttu-id="8ad1b-183">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="8ad1b-183">Mac OS X</span></span>
   * <span data-ttu-id="8ad1b-184">複数のパッケージ タイプで配信:</span><span class="sxs-lookup"><span data-stu-id="8ad1b-184">delivered in multiple package types:</span></span>
     * <span data-ttu-id="8ad1b-185">MSI、ZIP、TAR、DEB、RPM、および DMG</span><span class="sxs-lookup"><span data-stu-id="8ad1b-185">MSI, ZIP, TAR, DEB, RPM, and DMG</span></span>

    <span data-ttu-id="8ad1b-186">複数のベース OS イメージ上の Zulu JDK、JRE および JRE ヘッドレス用の認定済み Docker コンテナー イメージを Docker で利用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-186">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker.</span></span>

    <span data-ttu-id="8ad1b-187">ハブ:</span><span class="sxs-lookup"><span data-stu-id="8ad1b-187">Hub:</span></span>

    * [<span data-ttu-id="8ad1b-188">JDK</span><span class="sxs-lookup"><span data-stu-id="8ad1b-188">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
    * [<span data-ttu-id="8ad1b-189">JRE</span><span class="sxs-lookup"><span data-stu-id="8ad1b-189">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
    * [<span data-ttu-id="8ad1b-190">JRE ヘッドレス</span><span class="sxs-lookup"><span data-stu-id="8ad1b-190">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

4. <span data-ttu-id="8ad1b-191">無料</span><span class="sxs-lookup"><span data-stu-id="8ad1b-191">No cost</span></span>

   * <span data-ttu-id="8ad1b-192">Microsoft は、Azure 上の Java アプリをビルドおよび拡張するために必要なものをすべて無料で提供します。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-192">Microsoft provides everything you need to build and scale Java apps on Azure at no cost to you.</span></span> <span data-ttu-id="8ad1b-193">Zulu を通じて、Java アプリ用のセキュリティ更新プログラムとプラットフォームのバグ修正が無料で提供されます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-193">Through Zulu you'll receive free security updates and platform bug fixes for Java apps without any fees.</span></span>
   * <span data-ttu-id="8ad1b-194">[Java Flight Recorder および Mission Control](java-jdk-flight-recorder-and-mission-control.md) は、Zulu Java 8、11、および 12 (プレビュー) で利用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-194">[Java Flight Recorder and Mission Control](java-jdk-flight-recorder-and-mission-control.md) are available in Zulu Java 8, 11, and 12 (Preview).</span></span>

5. <span data-ttu-id="8ad1b-195">非 LTS バージョンの技術プレビュー</span><span class="sxs-lookup"><span data-stu-id="8ad1b-195">Tech Preview of Non-LTS versions</span></span>

   * <span data-ttu-id="8ad1b-196">技術プレビューでは、新機能が最終的に Java 17 LTS となる短期的なバージョンで提供される中で、それらを斬新的にテストする機会を得られます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-196">Tech previews provide you with opportunities to progressively test new features as they are delivered in short-term versions that will eventually graduate to Java 17 LTS.</span></span>

6. <span data-ttu-id="8ad1b-197">OpenJDK に対する変更を上流に反映</span><span class="sxs-lookup"><span data-stu-id="8ad1b-197">Changes to OpenJDK are up-streamed</span></span>

   * <span data-ttu-id="8ad1b-198">Azul Systems のコミッターが Zulu の変更を OpenJDK にプッシュするため、上流のリポジトリが包括的に維持されます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-198">Azul Systems committers push Zulu changes to OpenJDK which makes the upstream repo comprehensive and inclusive.</span></span>

<span data-ttu-id="8ad1b-199">Java 開発者は、Oracle JDK や Red Hat JDK などの独自の Java ランタイムを Azure に取り込んで、セキュリティ保護されたインフラストラクチャと機能豊富なサービスを利用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-199">As always, Java developers can bring their own Java run-times, including Oracle JDK and Red Hat JDK, to Azure and use  the secure infrastructure and feature-rich services.</span></span> <span data-ttu-id="8ad1b-200">Java 開発者は、Azure 上の Windows または Linux 仮想マシンで Java ワークロードを実行するために Oracle Java SE の運用エディションも利用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-200">The production edition of Oracle Java SE is also available to Java developers for running Java workloads in Windows or Linux virtual machines on Azure.</span></span>

## <a name="use-for-local-development"></a><span data-ttu-id="8ad1b-201">ローカル開発に使用</span><span class="sxs-lookup"><span data-stu-id="8ad1b-201">Use for local development</span></span> 

<span data-ttu-id="8ad1b-202">開発者は、Azure および Azure Stack 用の Java JDK を[ダウンロード](https://www.azul.com/downloads/azure-only/zulu/)して、ローカル開発環境で使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-202">Developers can [download](https://www.azul.com/downloads/azure-only/zulu/) Java JDKs for Azure and Azure Stack for use in local development environments.</span></span> <span data-ttu-id="8ad1b-203">ダウンロードは、Windows、Linux、macOS で利用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-203">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="8ad1b-204">Linux を使用している開発者は、[yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) および [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) パッケージ マネージャーを使ってパッケージを入手することもできます。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-204">Developers working on Linux can also get packages through the [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="8ad1b-205">追加のガイダンスについては、[Azure 用の Docker イメージ](java-jdk-docker-images.md)に関する記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8ad1b-205">For additional guidance see [Docker images for Azure](java-jdk-docker-images.md).</span></span>