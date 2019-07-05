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
# <a name="java-long-term-support-for-azure-and-azure-stack"></a><span data-ttu-id="2c168-103">Azure および Azure Stack の Java 長期サポート</span><span class="sxs-lookup"><span data-stu-id="2c168-103">Java long-term support for Azure and Azure Stack</span></span>

<span data-ttu-id="2c168-104">Microsoft Azure と Azure Stack を使用する Java 開発者は、[Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) を使用して、運用 Java アプリケーションをビルドして実行できます。追加のサポート コストは発生しません。</span><span class="sxs-lookup"><span data-stu-id="2c168-104">As a Java developer on Microsoft Azure and Azure Stack, you can build and run production Java applications by using [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="2c168-105">Azure 上で希望の Java ランタイムを使用できますが、Zulu を使用すると、無料のメンテナンス更新プログラムが提供され、Microsoft でサポートに関する問題を解決できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-105">You can use any Java runtime you want on Azure, but when you use Zulu, you get free maintenance updates and you can resolve support issues with Microsoft.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2c168-106">Java のダウンロードとインストール</span><span class="sxs-lookup"><span data-stu-id="2c168-106">Download and install Java</span></span>](java-jdk-install.md)

## <a name="long-term-support"></a><span data-ttu-id="2c168-107">長期サポート</span><span class="sxs-lookup"><span data-stu-id="2c168-107">Long-term support</span></span>

* [<span data-ttu-id="2c168-108">Java 11</span><span class="sxs-lookup"><span data-stu-id="2c168-108">Java 11</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java11)
* [<span data-ttu-id="2c168-109">Java 8</span><span class="sxs-lookup"><span data-stu-id="2c168-109">Java 8</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java8)
* [<span data-ttu-id="2c168-110">Java 7</span><span class="sxs-lookup"><span data-stu-id="2c168-110">Java 7</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java7)

## <a name="technical-preview"></a><span data-ttu-id="2c168-111">テクニカル プレビュー</span><span class="sxs-lookup"><span data-stu-id="2c168-111">Technical preview</span></span>

* [<span data-ttu-id="2c168-112">Java 12</span><span class="sxs-lookup"><span data-stu-id="2c168-112">Java 12</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java12)

## <a name="what-is-the-zulu-openjdk-for-azure"></a><span data-ttu-id="2c168-113">Azure の Zulu OpenJDK とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="2c168-113">What is the Zulu OpenJDK for Azure?</span></span>

<span data-ttu-id="2c168-114">OpenJDK の Azul Zulu Enterprise ビルドは、Microsoft および Azul Systems でサポートされる、Azure と Azure Stack 用の OpenJDK の無料でマルチプラット フォーム対応の実稼働可能なディストリビューションです。</span><span class="sxs-lookup"><span data-stu-id="2c168-114">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack that's backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="2c168-115">これらのディストリビューションは次のような特長があります。</span><span class="sxs-lookup"><span data-stu-id="2c168-115">These distributions are:</span></span>

* <span data-ttu-id="2c168-116">Java Development Kit (JDK)、Java Runtime Environment (JRE)、ヘッドレス JRE としてパッケージ化された、100% オープンソースの OpenJDK のビルド。</span><span class="sxs-lookup"><span data-stu-id="2c168-116">100 percent open-source builds of OpenJDK that are packaged as Java Development Kits (JDKs), Java Runtime Environments (JREs), and Headless JREs.</span></span> <span data-ttu-id="2c168-117">これらのバイナリは、Java Standard Edition (SE) の完全に互換性のある、準拠している商用ビルドであり、Azure と Azure Stack 上の Java アプリケーションまたはコンポーネントで使用できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-117">These binaries are fully compatible and compliant commercial builds of Java Standard Edition (SE) that can be used with Java applications or components on Azure and Azure Stack.</span></span>
* <span data-ttu-id="2c168-118">バグの修正、パフォーマンス強化、セキュリティ パッチを含む、長期サポート (LTS) が付属。</span><span class="sxs-lookup"><span data-stu-id="2c168-118">Provided with long-term support (LTS), which includes bug fixes, performance enhancements, and security patches.</span></span>
* <span data-ttu-id="2c168-119">Windows、Linux、macOS での Java アプリケーションの開発および実行で使用可能。</span><span class="sxs-lookup"><span data-stu-id="2c168-119">Available for developing and running Java applications on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="2c168-120">Docker Hub のコンテナー イメージおよび Azure Marketplace の仮想マシン (Windows および Linux) として使用可能。</span><span class="sxs-lookup"><span data-stu-id="2c168-120">Available as container images on Docker Hub and as virtual machines (Windows and Linux) in the Azure Marketplace.</span></span>
* <span data-ttu-id="2c168-121">次のような多くの Azure サービスを稼働させるために Azure で使用。</span><span class="sxs-lookup"><span data-stu-id="2c168-121">Used by Azure to power many Azure services, such as:</span></span>
  * <span data-ttu-id="2c168-122">Azure App Service (Windows)</span><span class="sxs-lookup"><span data-stu-id="2c168-122">Azure App Service (Windows)</span></span>
  * <span data-ttu-id="2c168-123">Azure App Service (Linux)</span><span class="sxs-lookup"><span data-stu-id="2c168-123">Azure App Service (Linux)</span></span>
  * <span data-ttu-id="2c168-124">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="2c168-124">Azure Functions</span></span>
  * <span data-ttu-id="2c168-125">Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="2c168-125">Azure Service Fabric</span></span>
  * <span data-ttu-id="2c168-126">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="2c168-126">Azure HDInsight</span></span>
  * <span data-ttu-id="2c168-127">Azure Search</span><span class="sxs-lookup"><span data-stu-id="2c168-127">Azure Search</span></span>
  * <span data-ttu-id="2c168-128">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="2c168-128">Azure DevOps</span></span>
  * <span data-ttu-id="2c168-129">Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="2c168-129">Azure Cloud Shell</span></span>  

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="2c168-130">サポートされている Java バージョンと更新スケジュール</span><span class="sxs-lookup"><span data-stu-id="2c168-130">Supported Java versions and update schedule</span></span>

<span data-ttu-id="2c168-131">Azul Systems では、Java SE 7、8、11 以降の Java のすべての LTS バージョンに対して、[Azure 向けの OpenJDK の完全にサポートされた Zulu Enterprise ビルド](https://www.azul.com/downloads/azure-only/zulu/)が提供されます。</span><span class="sxs-lookup"><span data-stu-id="2c168-131">Azul Systems provides fully supported [Zulu Enterprise builds of OpenJDK for Azure](https://www.azul.com/downloads/azure-only/zulu/) for all LTS versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="2c168-132">詳細については、[Azul のプレス リリース](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2c168-132">For more information, see the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>

|<span data-ttu-id="2c168-133">Java SE LTS</span><span class="sxs-lookup"><span data-stu-id="2c168-133">Java SE LTS</span></span>  |<span data-ttu-id="2c168-134">サポート期限</span><span class="sxs-lookup"><span data-stu-id="2c168-134">Support until</span></span>  |
|---------|----------|
|<span data-ttu-id="2c168-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span><span class="sxs-lookup"><span data-stu-id="2c168-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span></span> |<span data-ttu-id="2c168-136">2023 年 7 月</span><span class="sxs-lookup"><span data-stu-id="2c168-136">July 2023</span></span> |
|<span data-ttu-id="2c168-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span><span class="sxs-lookup"><span data-stu-id="2c168-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span></span> |<span data-ttu-id="2c168-138">2025 年 3 月</span><span class="sxs-lookup"><span data-stu-id="2c168-138">March 2025</span></span>|
|<span data-ttu-id="2c168-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span><span class="sxs-lookup"><span data-stu-id="2c168-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span></span> |<span data-ttu-id="2c168-140">2026 年 9 月</span><span class="sxs-lookup"><span data-stu-id="2c168-140">September 2026</span></span>|
|<span data-ttu-id="2c168-141">[![Java 12](../media/jdk/java-12.png)]()</span><span class="sxs-lookup"><span data-stu-id="2c168-141">[![Java 12](../media/jdk/java-12.png)]()</span></span> |<span data-ttu-id="2c168-142">**プレビュー**</span><span class="sxs-lookup"><span data-stu-id="2c168-142">**Preview**</span></span>|

<span data-ttu-id="2c168-143">これらの JDK リリースには、四半期ごとのセキュリティ更新プログラムとバグ修正が含まれ、必要に応じて、重要なアウトオブバンドの更新プログラムと修正プログラムも含まれます。</span><span class="sxs-lookup"><span data-stu-id="2c168-143">These JDK releases have quarterly security updates, bug fixes, and critical out-of-band updates and patches as needed.</span></span> <span data-ttu-id="2c168-144">このサポートには、Java 11 などの新しいバージョンの Java で報告されたセキュリティ更新プログラムとバグ修正の Java 7 および 8 へのバック ポーティングが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2c168-144">This support includes back ports of security updates and bug fixes to Java 7 and 8 that are reported in newer versions of Java, such as Java 11.</span></span> <span data-ttu-id="2c168-145">このサポートにより古いバージョンの Java の継続的な安定性とセキュリティが確保されます。</span><span class="sxs-lookup"><span data-stu-id="2c168-145">The support ensures the continued stability and security of older versions of Java.</span></span> <span data-ttu-id="2c168-146">Azure のお客様は、予定外の Java SE サブスクリプション料金を負担することなく、これらのセキュリティ更新プログラムとプラットフォームのバグ修正を入手できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-146">Azure customers can get these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span>

<span data-ttu-id="2c168-147">Azul Systems では、これらのリリースの [Java SE ロードマップ](https://www.azul.com/products/azul_support_roadmap/)を管理しています。</span><span class="sxs-lookup"><span data-stu-id="2c168-147">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="benefits-for-developers"></a><span data-ttu-id="2c168-148">開発者にとっての利点</span><span class="sxs-lookup"><span data-stu-id="2c168-148">Benefits for developers</span></span>

<span data-ttu-id="2c168-149">Azul Zulu JDK のリリースには、次の特長があります。</span><span class="sxs-lookup"><span data-stu-id="2c168-149">The Azul Zulu JDK releases are:</span></span>

* <span data-ttu-id="2c168-150">Microsoft と Azul Systems の両方でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="2c168-150">Backed and supported by both Microsoft and Azul Systems.</span></span>

   * <span data-ttu-id="2c168-151">Zulu のバイナリは実稼働可能であり、Microsoft と Azul Systems でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="2c168-151">Zulu binaries are production ready and backed by Microsoft and Azul Systems.</span></span>
   * <span data-ttu-id="2c168-152">Zulu には、Java 7、8、および 11 の無料の LTS が付属します (LTS は Java 17 にも提供されます)。</span><span class="sxs-lookup"><span data-stu-id="2c168-152">Zulu comes with zero-cost LTS for Java 7, 8, and 11 (LTS will be provided for Java 17, as well).</span></span> <span data-ttu-id="2c168-153">Java のバージョンは、必要なときにのみアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="2c168-153">You can upgrade Java versions only when you need to.</span></span>
   * <span data-ttu-id="2c168-154">Java 7 は 2023 年 7 月までサポートされます。</span><span class="sxs-lookup"><span data-stu-id="2c168-154">Java 7 is supported until July 2023.</span></span> <span data-ttu-id="2c168-155">Java 8 および 11 は 2024 年以降もサポートされます。</span><span class="sxs-lookup"><span data-stu-id="2c168-155">Java 8 and 11 are supported beyond 2024.</span></span>
   * <span data-ttu-id="2c168-156">Microsoft は、多くの Azure サービスを稼働するマシンで Zulu を内部的に実行することに取り組んでいます。</span><span class="sxs-lookup"><span data-stu-id="2c168-156">Microsoft is committed to running Zulu internally on machines that power many Azure services.</span></span>

* <span data-ttu-id="2c168-157">実稼働可能。</span><span class="sxs-lookup"><span data-stu-id="2c168-157">Production-ready.</span></span>

   * <span data-ttu-id="2c168-158">OpenJDK のビルドで 100% オープンソース。</span><span class="sxs-lookup"><span data-stu-id="2c168-158">100 percent open-source for its builds of OpenJDK.</span></span>
   * <span data-ttu-id="2c168-159">多くの Java SE ディストリビューションに対する簡単な置き換え。</span><span class="sxs-lookup"><span data-stu-id="2c168-159">Drop-in replacements for many Java SE distributions.</span></span>
   * <span data-ttu-id="2c168-160">JDK、JRE および JRE ヘッドレス。</span><span class="sxs-lookup"><span data-stu-id="2c168-160">JDK, JRE, and JRE-headless.</span></span>
   * <span data-ttu-id="2c168-161">Java 7、8、および 11。</span><span class="sxs-lookup"><span data-stu-id="2c168-161">Java 7, 8, and 11.</span></span>
   * <span data-ttu-id="2c168-162">OpenJDK Community Technology Compatibility Kit (TCK) を使用する Java SE 仕様に準拠していることを確認済み。</span><span class="sxs-lookup"><span data-stu-id="2c168-162">Verified compliant with the Java SE specifications that use the OpenJDK Community Technology Compatibility Kit (TCK).</span></span>
   * <span data-ttu-id="2c168-163">開発者には、Java SE 向けの運用更新プログラムの提供が継続され、これには Java SE 7、8、11 のバグ修正、パフォーマンス強化、およびセキュリティ修正プログラムが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2c168-163">As a developer, you continue to receive production updates for Java SE, including bug fixes, performance enhancements, and security patches for Java SE 7, 8, and 11.</span></span>

* <span data-ttu-id="2c168-164">マルチプラット フォームのサポート。</span><span class="sxs-lookup"><span data-stu-id="2c168-164">Supported for multi-platform.</span></span> <span data-ttu-id="2c168-165">Zulu では、次のような複数のプラットフォームとバージョンのバイナリがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2c168-165">Zulu supports binaries for multiple platforms and versions, including:</span></span>

   * <span data-ttu-id="2c168-166">Windows クライアント</span><span class="sxs-lookup"><span data-stu-id="2c168-166">Windows client</span></span>
     * <span data-ttu-id="2c168-167">10</span><span class="sxs-lookup"><span data-stu-id="2c168-167">10</span></span>
     * <span data-ttu-id="2c168-168">8.1</span><span class="sxs-lookup"><span data-stu-id="2c168-168">8.1</span></span>
     * <span data-ttu-id="2c168-169">8、7</span><span class="sxs-lookup"><span data-stu-id="2c168-169">8, 7</span></span>
   * <span data-ttu-id="2c168-170">Windows Server</span><span class="sxs-lookup"><span data-stu-id="2c168-170">Windows Server</span></span>
     * <span data-ttu-id="2c168-171">2016 R2</span><span class="sxs-lookup"><span data-stu-id="2c168-171">2016 R2</span></span>
     * <span data-ttu-id="2c168-172">2016</span><span class="sxs-lookup"><span data-stu-id="2c168-172">2016</span></span>
     * <span data-ttu-id="2c168-173">2012 R2</span><span class="sxs-lookup"><span data-stu-id="2c168-173">2012 R2</span></span>
     * <span data-ttu-id="2c168-174">2012</span><span class="sxs-lookup"><span data-stu-id="2c168-174">2012</span></span>
     * <span data-ttu-id="2c168-175">2008 R2</span><span class="sxs-lookup"><span data-stu-id="2c168-175">2008 R2</span></span>
   * <span data-ttu-id="2c168-176">Linux (次を含む)</span><span class="sxs-lookup"><span data-stu-id="2c168-176">Linux, including</span></span>
     * <span data-ttu-id="2c168-177">RHEL</span><span class="sxs-lookup"><span data-stu-id="2c168-177">RHEL</span></span>
     * <span data-ttu-id="2c168-178">CentOS</span><span class="sxs-lookup"><span data-stu-id="2c168-178">CentOS</span></span>
     * <span data-ttu-id="2c168-179">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="2c168-179">Ubuntu</span></span>
     * <span data-ttu-id="2c168-180">SLES</span><span class="sxs-lookup"><span data-stu-id="2c168-180">SLES</span></span>
     * <span data-ttu-id="2c168-181">Debian</span><span class="sxs-lookup"><span data-stu-id="2c168-181">Debian</span></span>
     * <span data-ttu-id="2c168-182">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="2c168-182">Oracle Linux</span></span>
   * <span data-ttu-id="2c168-183">macOS X</span><span class="sxs-lookup"><span data-stu-id="2c168-183">macOS X</span></span>
   * <span data-ttu-id="2c168-184">複数のパッケージの種類で配信:</span><span class="sxs-lookup"><span data-stu-id="2c168-184">Delivery in multiple package types:</span></span>
     * <span data-ttu-id="2c168-185">MSI、ZIP、TAR、DEB、RPM、および DMG</span><span class="sxs-lookup"><span data-stu-id="2c168-185">MSI, ZIP, TAR, DEB, RPM, and DMG</span></span>

    <span data-ttu-id="2c168-186">複数のベース OS イメージ上の Zulu JDK、JRE および JRE ヘッドレス用の認定済み Docker コンテナー イメージを Docker で利用できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-186">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker.</span></span>

    <span data-ttu-id="2c168-187">ハブ:</span><span class="sxs-lookup"><span data-stu-id="2c168-187">Hub:</span></span>

    * [<span data-ttu-id="2c168-188">JDK</span><span class="sxs-lookup"><span data-stu-id="2c168-188">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
    * [<span data-ttu-id="2c168-189">JRE</span><span class="sxs-lookup"><span data-stu-id="2c168-189">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
    * [<span data-ttu-id="2c168-190">JRE ヘッドレス</span><span class="sxs-lookup"><span data-stu-id="2c168-190">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

* <span data-ttu-id="2c168-191">コストがかかりません。</span><span class="sxs-lookup"><span data-stu-id="2c168-191">No cost.</span></span>

   * <span data-ttu-id="2c168-192">Microsoft は、Azure 上の Java アプリをビルドおよび拡張するために必要なものをすべて無料で提供します。</span><span class="sxs-lookup"><span data-stu-id="2c168-192">Microsoft provides everything you need to build and scale Java apps on Azure, at no cost to you.</span></span> <span data-ttu-id="2c168-193">Zulu を通じて、Java アプリ用のセキュリティ更新プログラムとプラットフォームのバグ修正が無料で提供されます。</span><span class="sxs-lookup"><span data-stu-id="2c168-193">Through Zulu you'll receive free security updates and platform bug fixes for Java apps without any fees.</span></span>
   * <span data-ttu-id="2c168-194">[Java Flight Recorder および Mission Control](java-jdk-flight-recorder-and-mission-control.md) は、Zulu Java 8、11、および 12 (プレビュー) で利用できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-194">[Java Flight Recorder and Mission Control](java-jdk-flight-recorder-and-mission-control.md) are available in Zulu Java 8, 11, and 12 (Preview).</span></span>

* <span data-ttu-id="2c168-195">非 LTS バージョンのテクニカル プレビューとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-195">Available as tech preview of non-LTS versions.</span></span>

   * <span data-ttu-id="2c168-196">テクニカル プレビューでは、新機能が最終的に Java 17 LTS となる短期的なバージョンで提供される中で、それらを斬新的にテストする機会を得られます。</span><span class="sxs-lookup"><span data-stu-id="2c168-196">With tech previews, you can progressively test new features as they're delivered, in short-term versions that will eventually graduate to Java 17 LTS.</span></span>

* <span data-ttu-id="2c168-197">OpenJDK に対する変更が上流に送信されます。</span><span class="sxs-lookup"><span data-stu-id="2c168-197">Changes to OpenJDK are sent upstream.</span></span>

   * <span data-ttu-id="2c168-198">Azul Systems のコミッターが Zulu の変更を OpenJDK にプッシュするため、上流のリポジトリが包括的に維持されます。</span><span class="sxs-lookup"><span data-stu-id="2c168-198">Azul Systems committers push Zulu changes to OpenJDK, which makes the upstream repo comprehensive and inclusive.</span></span>

<span data-ttu-id="2c168-199">Java 開発者はこれまでどおり、独自の Java ランタイム (Oracle JDK と Red Hat JDK を含む) を Azure に取り込むことができます。</span><span class="sxs-lookup"><span data-stu-id="2c168-199">As always, as a Java developer, you can bring to Azure your own Java runtimes, including the Oracle JDK and the Red Hat JDK.</span></span> <span data-ttu-id="2c168-200">セキュリティで保護されたインフラストラクチャと機能豊富なサービスを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="2c168-200">You can also use the secure infrastructure and feature-rich services.</span></span> <span data-ttu-id="2c168-201">Oracle Java SE の運用エディションは、Azure 上の Windows または Linux 仮想マシンで Java ワークロードの実行に使用できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-201">The production edition of Oracle Java SE is available to you for running Java workloads in Windows or Linux virtual machines on Azure.</span></span>

## <a name="use-java-jdks-for-local-development"></a><span data-ttu-id="2c168-202">ローカル開発に Java JDK を使用する</span><span class="sxs-lookup"><span data-stu-id="2c168-202">Use Java JDKs for local development</span></span> 

<span data-ttu-id="2c168-203">[Azure および Azure Stack 用の Java JDK をダウンロード](https://www.azul.com/downloads/azure-only/zulu/)して、ローカル開発環境で使用できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-203">You can [download Java JDKs for Azure and Azure Stack](https://www.azul.com/downloads/azure-only/zulu/) for use in your local development environments.</span></span> <span data-ttu-id="2c168-204">ダウンロードは、Windows、Linux、macOS で利用できます。</span><span class="sxs-lookup"><span data-stu-id="2c168-204">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="2c168-205">Linux で作業している場合は、[yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) および [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) パッケージ マネージャーを使ってパッケージを入手することもできます。</span><span class="sxs-lookup"><span data-stu-id="2c168-205">If you're working on Linux, you can also get packages through the [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="2c168-206">追加のガイダンスについては、[Azure 用の Docker イメージ](java-jdk-docker-images.md)に関する記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2c168-206">For additional guidance, see [Docker images for Azure](java-jdk-docker-images.md).</span></span>
