---
title: Java Flight Recorder と Mission Control
description: Java Flight Recorder と Mission Control を使用してアプリ データを収集およびレビューするためのガイダンス。
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 64f64f2e5891fccf9d62510f39bd99d73457d590
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533622"
---
# <a name="use-java-flight-recorder-and-mission-control"></a><span data-ttu-id="eb867-103">Java Flight Recorder と Mission Control の使用</span><span class="sxs-lookup"><span data-stu-id="eb867-103">Use Java Flight Recorder and Mission Control</span></span>

<span data-ttu-id="eb867-104">Zulu Mission Control は、JDK Mission Control の完全にテスト済みのビルドであり、2018 年に Oracle によりオープンソース化され、OpenJDK Umbrella の下でプロジェクトとして管理されています。</span><span class="sxs-lookup"><span data-stu-id="eb867-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open-sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="eb867-105">Mission Control は、Java Flight Recorder (JFR) と組み合わせることで、Java ワークロードに対するオーバーヘッドの低い対話型の監視と管理の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="eb867-105">Coupled with Java Flight Recorder (JFR), Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="eb867-106">Zulu Mission Control は、次の Java Development Kit (JDK) および Java Runtime Environment (JRE) と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="eb867-106">Zulu Mission Control is compatible with the following Java Development Kits (JDKs) and Java Runtime Environments (JREs):</span></span>

* <span data-ttu-id="eb867-107">Zulu 12.1 以降</span><span class="sxs-lookup"><span data-stu-id="eb867-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="eb867-108">Zulu 11.0 以降</span><span class="sxs-lookup"><span data-stu-id="eb867-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="eb867-109">Zulu 8u202 (8.36) 以降</span><span class="sxs-lookup"><span data-stu-id="eb867-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="eb867-110">Oracle OpenJDK 11 および 15 以降</span><span class="sxs-lookup"><span data-stu-id="eb867-110">Oracle OpenJDK 11 and 15 and later</span></span>
* <span data-ttu-id="eb867-111">Oracle Java 11.0 以降</span><span class="sxs-lookup"><span data-stu-id="eb867-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="eb867-112">Oracle Java 8.0 以降</span><span class="sxs-lookup"><span data-stu-id="eb867-112">Oracle Java 8.0 and later</span></span>

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a><span data-ttu-id="eb867-113">Zulu Mission Control をインストールして JVM に接続する</span><span class="sxs-lookup"><span data-stu-id="eb867-113">Install Zulu Mission Control and connect to a JVM</span></span>

<span data-ttu-id="eb867-114">Zulu Mission Control をインストールし、Java 仮想マシン (JVM) に接続し、実行中のアプリケーションのすべての側面をリアルタイムに把握するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="eb867-114">To install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application, do the following:</span></span>

1.  <span data-ttu-id="eb867-115">[Zulu Mission Control と互換性のある JDK と JRE をインストールします](java-jdk-install.md)。</span><span class="sxs-lookup"><span data-stu-id="eb867-115">[Install a Zulu Mission Control-compatible JDK and JRE](java-jdk-install.md).</span></span>

1.  <span data-ttu-id="eb867-116">[Zulu Mission Control をダウンロード](https://www.azul.com/products/zulu-mission-control/)して、ご使用のシステムに適したバージョンを選択し、ローカルに保存してそのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="eb867-116">[Download Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

1.  <span data-ttu-id="eb867-117">ダウンロードしたファイルを展開します。</span><span class="sxs-lookup"><span data-stu-id="eb867-117">Expand the downloaded file.</span></span>

    <span data-ttu-id="eb867-118">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="eb867-118">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="eb867-119">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="eb867-119">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="eb867-120">**macOS:**</span><span class="sxs-lookup"><span data-stu-id="eb867-120">**macOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  <span data-ttu-id="eb867-121">互換性のある JDK のいずれかを使用して Java アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="eb867-121">Start your Java application by using one of the compatible JDKs.</span></span> <span data-ttu-id="eb867-122">例:</span><span class="sxs-lookup"><span data-stu-id="eb867-122">For example:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  <span data-ttu-id="eb867-123">Zulu Mission Control を起動します。</span><span class="sxs-lookup"><span data-stu-id="eb867-123">Start Zulu Mission Control.</span></span>

    <span data-ttu-id="eb867-124">**Linux:**</span><span class="sxs-lookup"><span data-stu-id="eb867-124">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="eb867-125">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="eb867-125">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="eb867-126">**macOS:**</span><span class="sxs-lookup"><span data-stu-id="eb867-126">**macOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  <span data-ttu-id="eb867-127">(省略可能) JVM インストールを Mission Control 用に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="eb867-127">(Optional) Switch the JVM installation for Mission Control.</span></span>

    <span data-ttu-id="eb867-128">Windows デバイスでは、*zmc.exe* はレジストリで構成されている既定の JVM インストールを使用します。</span><span class="sxs-lookup"><span data-stu-id="eb867-128">On Windows devices, *zmc.exe* uses the default JVM installation that's configured in the registry.</span></span> <span data-ttu-id="eb867-129">Zulu Mission Control は、ローカルの JVM インスタンスを自動的に検出することができるためには、完全な JDK から起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb867-129">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="eb867-130">インストールが JRE の場合、JVM は検出されず、次の警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb867-130">If the installation is a JRE, no JVM will be detected, and you will receive the following warning:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="eb867-131">![JDK のインストールが JRE のみの場合の警告](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="eb867-131">![Warning if JDK installation is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="eb867-132">Mission Control によって使用される JVM を変更するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="eb867-132">To change the JVM that's used by Mission Control, do the following:</span></span> 

    <span data-ttu-id="eb867-133">a.</span><span class="sxs-lookup"><span data-stu-id="eb867-133">a.</span></span> <span data-ttu-id="eb867-134">*zmc.exe* ファイルと同じディレクトリにある *zmc.ini* 構成ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="eb867-134">Open the *zmc.ini* configuration file, which is in the same directory as the *zmc.exe* file.</span></span>

    <span data-ttu-id="eb867-135">b.</span><span class="sxs-lookup"><span data-stu-id="eb867-135">b.</span></span> <span data-ttu-id="eb867-136">行 `-vmargs` の前に、2 つの行を追加します。</span><span class="sxs-lookup"><span data-stu-id="eb867-136">Before the line `-vmargs`, add two lines:</span></span>  

       * <span data-ttu-id="eb867-137">最初の行に、`–vm` を入力します。</span><span class="sxs-lookup"><span data-stu-id="eb867-137">On the first line, enter `–vm`.</span></span>  
       * <span data-ttu-id="eb867-138">2 行目に、JDK インストールへのパスを入力します (例: `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。</span><span class="sxs-lookup"><span data-stu-id="eb867-138">On the second line, enter the path to your JDK installation (for example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

1.  <span data-ttu-id="eb867-139">次を行って、アプリケーションを実行している JVM を見つけます。</span><span class="sxs-lookup"><span data-stu-id="eb867-139">Locate the JVM that's running your application by doing the following:</span></span>

    <span data-ttu-id="eb867-140">a.</span><span class="sxs-lookup"><span data-stu-id="eb867-140">a.</span></span> <span data-ttu-id="eb867-141">Zulu Mission Control ウィンドウの左側のウィンドウで、 **[JVM Browser]\(JVM ブラウザー\)** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="eb867-141">In the left pane of the Zulu Mission Control window, select the **JVM Browser** tab.</span></span>

    <span data-ttu-id="eb867-142">b.</span><span class="sxs-lookup"><span data-stu-id="eb867-142">b.</span></span> <span data-ttu-id="eb867-143">左側で、自分のアプリケーションを実行している JVM インスタンスを選択して展開します。</span><span class="sxs-lookup"><span data-stu-id="eb867-143">In the list, select and expand the JVM instance that's running your application.</span></span>

    ![展開されたリスト内の JVM インスタンス](../media/jdk/azul-jfr-2.png)


1.  <span data-ttu-id="eb867-145">必要に応じて Flight Recording を開始します。</span><span class="sxs-lookup"><span data-stu-id="eb867-145">Start a flight recording, if necessary.</span></span>

    <span data-ttu-id="eb867-146">a.</span><span class="sxs-lookup"><span data-stu-id="eb867-146">a.</span></span> <span data-ttu-id="eb867-147">左側のウィンドウの **[Flight Recorder]** の下に、*No Recordings* メッセージが表示されている場合は、 **[Flight Recorder]** を右クリックして **[Start Flight Recording]\(Flight Recording の開始\)** を選択して、記録を開始します。</span><span class="sxs-lookup"><span data-stu-id="eb867-147">In the left pane, under **Flight Recorder**, if a *No Recordings* message is displayed, start a recording by right-clicking **Flight Recorder** and then selecting **Start Flight Recording**.</span></span>

    <span data-ttu-id="eb867-148">b.</span><span class="sxs-lookup"><span data-stu-id="eb867-148">b.</span></span> <span data-ttu-id="eb867-149">**[Time fixed recording]\(時間固定記録\)** または **[Continuous recording]\(継続記録\)** のいずれかと、**プロファイル**構成 (詳細) または**継続**構成 (低オーバーヘッド) のいずれかを選択してから、 **[Finish]\(終了\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="eb867-149">Select either **Time fixed recording** or **Continuous recording**, and either a **Profiling** configuration (fine-grained) or a **Continuous** configuration (lower overhead), and then select **Finish**.</span></span>

    ![Flight Recording の開始](../media/jdk/azul-jfr-3.png)

    <span data-ttu-id="eb867-151">Flight Recording は、[JVM Browser]\(JVM ブラウザー\) の **Flight Recorder** の行の下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb867-151">A flight recording should appear below the **Flight Recorder** line in the JVM browser.</span></span>

1. <span data-ttu-id="eb867-152">Flight Recording をダンプします。</span><span class="sxs-lookup"><span data-stu-id="eb867-152">Dump the flight recording.</span></span> <span data-ttu-id="eb867-153">これを行うには、Flight Recording を表す行を右クリックし、 **[Dump whole recording]\(記録全体をダンプ\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="eb867-153">To do so, right-click the line that represents the flight recording, and then select **Dump whole recording**.</span></span>

    <span data-ttu-id="eb867-154">Zulu Mission Control ウィンドウの右側にある大きなウィンドウに新しいタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb867-154">A new tab appears in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="eb867-155">このウィンドウは、アプリケーションを実行して、JVM から今ダンプされた Flight Recording を表しています。</span><span class="sxs-lookup"><span data-stu-id="eb867-155">This pane represents the flight recording that was just dumped from the JVM that's running your application.</span></span>

1. <span data-ttu-id="eb867-156">Zulu Mission Control を使用して Flight Recording を確認します。</span><span class="sxs-lookup"><span data-stu-id="eb867-156">Examine the flight recording by using Zulu Mission Control.</span></span> <span data-ttu-id="eb867-157">これを行うには、Zulu Mission Control ウィンドウの左側のウィンドウで、 **[Outline]\(アウトライン\)** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="eb867-157">To do so, select the **Outline** tab in the left pane of the Zulu Mission Control window.</span></span> <span data-ttu-id="eb867-158">このタブには、Flight Recording で収集されたデータのさまざまなビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb867-158">This tab displays various views of the data that's collected in the flight recording.</span></span>
 
    ![Flight Recording のレビュー](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a><span data-ttu-id="eb867-160">リソース</span><span class="sxs-lookup"><span data-stu-id="eb867-160">Resources</span></span>

<span data-ttu-id="eb867-161">詳細については、Azul Systems のサイトにアクセスして、「[Azul Webinar:Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)」(Azul ウェビナー: オープンソースの Flight Recorder と Mission Control - OpenJDK 8 パフォーマンスの管理と測定) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="eb867-161">To learn more, go to the Azul Systems site and view [Azul Webinar: Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/).</span></span> <span data-ttu-id="eb867-162">このビデオは、Azul Systems 副 CTO の Simon Ritter 氏がナレーションをしています。</span><span class="sxs-lookup"><span data-stu-id="eb867-162">The video is narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="eb867-163">Flight Recorder の説明は 31 分 30 秒から始まります。</span><span class="sxs-lookup"><span data-stu-id="eb867-163">The Flight Recorder discussion starts at 31:30.</span></span>

