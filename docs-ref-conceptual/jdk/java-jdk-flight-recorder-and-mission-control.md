---
title: Java Flight Recorder と Mission Control
description: Java Flight Recorder と Mission Control を使用してアプリ データを収集およびレビューするためのガイダンス。
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: b27e0f741f1322b7e8e1df363dbb2f40a3d34d53
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568565"
---
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a><span data-ttu-id="96387-103">Java Flight Recorder (JFR) および Mission Control の使用</span><span class="sxs-lookup"><span data-stu-id="96387-103">Using Java Flight Recorder (JFR) and Mission Control</span></span>

<span data-ttu-id="96387-104">Zulu Mission Control は、JDK Mission Control の完全にテスト済みのビルドであり、2018 年に Oracle によりオープンソース化され、OpenJDK Umbrella の下でプロジェクトとして管理されています。</span><span class="sxs-lookup"><span data-stu-id="96387-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="96387-105">Mission Control は、Flight Recorder と組み合わせることで、Java ワークロードに対するオーバーヘッドの低い対話型の監視と管理の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="96387-105">Coupled with Flight Recorder, Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="96387-106">Zulu Mission Control は、次の JDK/JRE と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="96387-106">Zulu Mission Control is compatible with the following JDKs/JREs:</span></span>

* <span data-ttu-id="96387-107">Zulu 12.1 以降</span><span class="sxs-lookup"><span data-stu-id="96387-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="96387-108">Zulu 11.0 以降</span><span class="sxs-lookup"><span data-stu-id="96387-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="96387-109">Zulu 8u202 (8.36) 以降</span><span class="sxs-lookup"><span data-stu-id="96387-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="96387-110">Oracle OpenJDK 11+15 以降</span><span class="sxs-lookup"><span data-stu-id="96387-110">Oracle OpenJDK 11+15 and later</span></span>
* <span data-ttu-id="96387-111">Oracle Java 11.0 以降</span><span class="sxs-lookup"><span data-stu-id="96387-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="96387-112">Oracle Java 8.0 以降</span><span class="sxs-lookup"><span data-stu-id="96387-112">Oracle Java 8.0 and later</span></span>

<span data-ttu-id="96387-113">Zulu Mission Control をインストールし、Java 仮想マシン (JVM) に接続し、実行中のアプリケーションのすべての側面をリアルタイムに把握するのには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="96387-113">Follow the steps below to install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application.</span></span>

1.  <span data-ttu-id="96387-114">[Zulu Mission Control と互換性のある JDK/JRE をインストールします](java-jdk-install.md)。</span><span class="sxs-lookup"><span data-stu-id="96387-114">[Install a Zulu Mission Control compatible JDK/JRE](java-jdk-install.md).</span></span>

2.  <span data-ttu-id="96387-115">[Azul ダウンロード サイト](https://www.azul.com/products/zulu-mission-control/)から Zulu Mission Control をダウンロードします。ご使用のシステム用の適切なバージョンを選択し、ローカルに保存してそのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="96387-115">Download Zulu Mission Control from [the Azul download site](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

3.  <span data-ttu-id="96387-116">ダウンロードしたファイルを展開します。</span><span class="sxs-lookup"><span data-stu-id="96387-116">Expand the downloaded file.</span></span>

    <span data-ttu-id="96387-117">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="96387-117">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="96387-118">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="96387-118">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="96387-119">**MacOS:**</span><span class="sxs-lookup"><span data-stu-id="96387-119">**MacOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  <span data-ttu-id="96387-120">互換性のある JDK のいずれかを使用して Java アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="96387-120">Start your Java application using one of the compatible JDKs.</span></span> <span data-ttu-id="96387-121">例:</span><span class="sxs-lookup"><span data-stu-id="96387-121">E.g.:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  <span data-ttu-id="96387-122">Zulu Mission Control を開始します</span><span class="sxs-lookup"><span data-stu-id="96387-122">Start Zulu Mission Control</span></span>

    <span data-ttu-id="96387-123">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="96387-123">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="96387-124">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="96387-124">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="96387-125">**MacOS:**</span><span class="sxs-lookup"><span data-stu-id="96387-125">**MacOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  <span data-ttu-id="96387-126">Mission Control の Java インストールを切り替えます (オプション)</span><span class="sxs-lookup"><span data-stu-id="96387-126">Switch the JVM installation for Mission Control (Optional)</span></span>

    <span data-ttu-id="96387-127">Windows では、*zmc.exe* はレジストリで構成されている既定の JVM インストールを使用します。</span><span class="sxs-lookup"><span data-stu-id="96387-127">On Windows, *zmc.exe* will use the default JVM installation configured in the registry.</span></span> <span data-ttu-id="96387-128">Zulu Mission Control は、ローカルの JVM インスタンスを自動的に検出することができるためには、完全な JDK から起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="96387-128">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="96387-129">これが JRE の場合、以下の警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="96387-129">If this is a JRE, you will see the warning below:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="96387-130">![JDK のインストールが JRE のみの場合の警告](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="96387-130">![Warning if JDK install is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="96387-131">Mission Control によって使用される JVM を変更するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="96387-131">To change the JVM used by Mission Control, follow these steps:</span></span> 
    1.  <span data-ttu-id="96387-132">*zmc.exe* と同じディレクトリにある *zmc.ini*構成ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="96387-132">Open *zmc.ini* configuration file, located in the same directory as the *zmc.exe*</span></span>
    2.  <span data-ttu-id="96387-133">行 `-vmargs` の前に、2 つの行を追加します。</span><span class="sxs-lookup"><span data-stu-id="96387-133">Before the line `-vmargs`, add two lines:</span></span>
        * <span data-ttu-id="96387-134">最初の行に、`–vm` と記述します。</span><span class="sxs-lookup"><span data-stu-id="96387-134">On the first line, write `–vm`</span></span>
        * <span data-ttu-id="96387-135">2 行目に、JDK インストールへのパスを記述します。</span><span class="sxs-lookup"><span data-stu-id="96387-135">On the second line, write the path to your JDK installation.</span></span> <span data-ttu-id="96387-136">(例えば、`C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。</span><span class="sxs-lookup"><span data-stu-id="96387-136">(For example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

7.  <span data-ttu-id="96387-137">アプリケーションを実行している JVM を見つけます</span><span class="sxs-lookup"><span data-stu-id="96387-137">Locate the JVM running your application</span></span>
    1.  <span data-ttu-id="96387-138">Zulu Mission Control ウィンドウの左上のウィンドウで、 **[JVM Browser]\(JVM ブラウザー\)** というラベルのタブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="96387-138">In the upper left pane of the Zulu Mission Control window click on the tab labelled **JVM Browser**.</span></span>
    2.  <span data-ttu-id="96387-139">左上で、自分のアプリケーションを実行している JVM インスタンスのリスト アイテムを選択して展開します。</span><span class="sxs-lookup"><span data-stu-id="96387-139">Select and expand the list item in the upper left for your the JVM instance running your application.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="96387-140">![JVM インスタンスの左上のリスト アイテムを展開します](../media/jdk/azul-jfr-2.png)</span><span class="sxs-lookup"><span data-stu-id="96387-140">![Expand the list item in the upper-left for your JVM instance](../media/jdk/azul-jfr-2.png)</span></span>


8.  <span data-ttu-id="96387-141">Flight Recording を開始します (必要な場合)</span><span class="sxs-lookup"><span data-stu-id="96387-141">Start a Flight Recording, if necessary</span></span>
    1.  <span data-ttu-id="96387-142">Flight Recorder に「No Recordings」と表示されている場合、[JVM Browser]\(JVM ブラウザー\) タブの Flight Recorder の行を右クリックし、 **[Start Flight Recording...]\(Flight Recording の開始...\)** を選択して開始します。</span><span class="sxs-lookup"><span data-stu-id="96387-142">If the Flight Recorder displays "No Recordings", start one by right-clicking on the Flight Recorder line in the JVM Browser tab and selecting **Start Flight Recording...**</span></span>
    2.  <span data-ttu-id="96387-143">固定期間の記録または継続的な記録のいずれかと、プロファイル構成 (詳細) または継続構成 (低オーバーヘッド) のいずれかを選択してから、 **[Finish]\(終了\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="96387-143">Select either a fixed duration recording or a continuous recording, and either a Profiling configuration (fine-grained) or a Continuous configuration (lower overhead), then click **Finish**.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="96387-144">![Flight Recording の開始](../media/jdk/azul-jfr-3.png)</span><span class="sxs-lookup"><span data-stu-id="96387-144">![Start a Flight Recording](../media/jdk/azul-jfr-3.png)</span></span>

9.  <span data-ttu-id="96387-145">Flight Recording のダンプ</span><span class="sxs-lookup"><span data-stu-id="96387-145">Dump the Flight Recording</span></span>
    1.  <span data-ttu-id="96387-146">Flight Recording は、[JVM Browser]\(JVM ブラウザー\) の Flight Recorder の行の下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="96387-146">A Flight Recording should appear below the Flight Recorder line in the JVM Browser.</span></span> <span data-ttu-id="96387-147">Flight Recording を表す行を右クリックし、 **[Dump whole recording]\(記録全体をダンプ\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="96387-147">Right-click on the line representing the Flight Recording and select **Dump whole recording**.</span></span>
    2.  <span data-ttu-id="96387-148">Zulu Mission Control ウィンドウの右側にある大きなウィンドウに新しいタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="96387-148">A new tab will appear in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="96387-149">このウィンドウは、アプリケーションを実行して、JVM から今ダンプされた Flight Recording を表しています。</span><span class="sxs-lookup"><span data-stu-id="96387-149">This pane represents the Flight Recording just dumped from the JVM running your application.</span></span>

10. <span data-ttu-id="96387-150">Zulu Mission Control を使用して Flight Recording を確認します</span><span class="sxs-lookup"><span data-stu-id="96387-150">Examine the Flight Recording using Zulu Mission Control</span></span>
    1.  <span data-ttu-id="96387-151">まだアクティブ化されていない場合、Zulu Mission Control ウィンドウの左側のウィンドウで **[Outline]\(アウトライン\)** というラベルのタブを選択します。</span><span class="sxs-lookup"><span data-stu-id="96387-151">If not already activated, select the tab labelled **Outline** in the left pane of the Zulu Mission Control Window.</span></span> <span data-ttu-id="96387-152">このタブには、Flight Recording で収集されたデータのさまざまなビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="96387-152">This tab contains different views of the data collected in the Flight Recording.</span></span>
 
    > [!div class="mx-imgBorder"]
    <span data-ttu-id="96387-153">![Fliight Recording のレビュー](../media/jdk/azul-jfr-4.png)</span><span class="sxs-lookup"><span data-stu-id="96387-153">![Review the Fliight Recording](../media/jdk/azul-jfr-4.png)</span></span>

## <a name="resources"></a><span data-ttu-id="96387-154">リソース</span><span class="sxs-lookup"><span data-stu-id="96387-154">Resources</span></span>

<span data-ttu-id="96387-155">Azul Systems の Deputy CTO である Simon Ritter 氏のナレーションによる[デモ動画](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)も用意されています。</span><span class="sxs-lookup"><span data-stu-id="96387-155">We have also prepared a [demonstration video](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="96387-156">この動画では、Flight Recorder と Zulu Mission Control 両方の構成とセットアップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="96387-156">The video walks you through the configuration and setup of both Flight Recorder and Zulu Mission Control.</span></span> <span data-ttu-id="96387-157">Flight Recorder の説明は 31 分 30 秒から始まります。</span><span class="sxs-lookup"><span data-stu-id="96387-157">The Flight Recorder discussion starts at 31:30.</span></span>

