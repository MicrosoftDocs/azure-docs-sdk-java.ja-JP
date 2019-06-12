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
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a>Java Flight Recorder (JFR) および Mission Control の使用

Zulu Mission Control は、JDK Mission Control の完全にテスト済みのビルドであり、2018 年に Oracle によりオープンソース化され、OpenJDK Umbrella の下でプロジェクトとして管理されています。 Mission Control は、Flight Recorder と組み合わせることで、Java ワークロードに対するオーバーヘッドの低い対話型の監視と管理の機能を提供します。

Zulu Mission Control は、次の JDK/JRE と互換性があります。

* Zulu 12.1 以降
* Zulu 11.0 以降
* Zulu 8u202 (8.36) 以降
* Oracle OpenJDK 11+15 以降
* Oracle Java 11.0 以降
* Oracle Java 8.0 以降

Zulu Mission Control をインストールし、Java 仮想マシン (JVM) に接続し、実行中のアプリケーションのすべての側面をリアルタイムに把握するのには、次の手順に従います。

1.  [Zulu Mission Control と互換性のある JDK/JRE をインストールします](java-jdk-install.md)。

2.  [Azul ダウンロード サイト](https://www.azul.com/products/zulu-mission-control/)から Zulu Mission Control をダウンロードします。ご使用のシステム用の適切なバージョンを選択し、ローカルに保存してそのディレクトリに移動します。

3.  ダウンロードしたファイルを展開します。

    **Linux:**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows:**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **MacOS:**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  互換性のある JDK のいずれかを使用して Java アプリケーションを起動します。 例:

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  Zulu Mission Control を開始します

    **Linux:**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows:**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **MacOS:**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  Mission Control の Java インストールを切り替えます (オプション)

    Windows では、*zmc.exe* はレジストリで構成されている既定の JVM インストールを使用します。 Zulu Mission Control は、ローカルの JVM インスタンスを自動的に検出することができるためには、完全な JDK から起動する必要があります。 これが JRE の場合、以下の警告が表示されます。

    > [!div class="mx-imgBorder"]
    ![JDK のインストールが JRE のみの場合の警告](../media/jdk/azul-jfr-1.png)

    Mission Control によって使用される JVM を変更するには、次の手順を実行します。 
    1.  *zmc.exe* と同じディレクトリにある *zmc.ini*構成ファイルを開きます。
    2.  行 `-vmargs` の前に、2 つの行を追加します。
        * 最初の行に、`–vm` と記述します。
        * 2 行目に、JDK インストールへのパスを記述します。 (例えば、`C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。

7.  アプリケーションを実行している JVM を見つけます
    1.  Zulu Mission Control ウィンドウの左上のウィンドウで、 **[JVM Browser]\(JVM ブラウザー\)** というラベルのタブをクリックします。
    2.  左上で、自分のアプリケーションを実行している JVM インスタンスのリスト アイテムを選択して展開します。

    > [!div class="mx-imgBorder"]
    ![JVM インスタンスの左上のリスト アイテムを展開します](../media/jdk/azul-jfr-2.png)


8.  Flight Recording を開始します (必要な場合)
    1.  Flight Recorder に「No Recordings」と表示されている場合、[JVM Browser]\(JVM ブラウザー\) タブの Flight Recorder の行を右クリックし、 **[Start Flight Recording...]\(Flight Recording の開始...\)** を選択して開始します。
    2.  固定期間の記録または継続的な記録のいずれかと、プロファイル構成 (詳細) または継続構成 (低オーバーヘッド) のいずれかを選択してから、 **[Finish]\(終了\)** をクリックします。

    > [!div class="mx-imgBorder"]
    ![Flight Recording の開始](../media/jdk/azul-jfr-3.png)

9.  Flight Recording のダンプ
    1.  Flight Recording は、[JVM Browser]\(JVM ブラウザー\) の Flight Recorder の行の下に表示されます。 Flight Recording を表す行を右クリックし、 **[Dump whole recording]\(記録全体をダンプ\)** を選択します。
    2.  Zulu Mission Control ウィンドウの右側にある大きなウィンドウに新しいタブが表示されます。 このウィンドウは、アプリケーションを実行して、JVM から今ダンプされた Flight Recording を表しています。

10. Zulu Mission Control を使用して Flight Recording を確認します
    1.  まだアクティブ化されていない場合、Zulu Mission Control ウィンドウの左側のウィンドウで **[Outline]\(アウトライン\)** というラベルのタブを選択します。 このタブには、Flight Recording で収集されたデータのさまざまなビューが含まれています。
 
    > [!div class="mx-imgBorder"]
    ![Fliight Recording のレビュー](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>リソース

Azul Systems の Deputy CTO である Simon Ritter 氏のナレーションによる[デモ動画](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)も用意されています。 この動画では、Flight Recorder と Zulu Mission Control 両方の構成とセットアップについて説明します。 Flight Recorder の説明は 31 分 30 秒から始まります。

