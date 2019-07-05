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
# <a name="use-java-flight-recorder-and-mission-control"></a>Java Flight Recorder と Mission Control の使用

Zulu Mission Control は、JDK Mission Control の完全にテスト済みのビルドであり、2018 年に Oracle によりオープンソース化され、OpenJDK Umbrella の下でプロジェクトとして管理されています。 Mission Control は、Java Flight Recorder (JFR) と組み合わせることで、Java ワークロードに対するオーバーヘッドの低い対話型の監視と管理の機能を提供します。

Zulu Mission Control は、次の Java Development Kit (JDK) および Java Runtime Environment (JRE) と互換性があります。

* Zulu 12.1 以降
* Zulu 11.0 以降
* Zulu 8u202 (8.36) 以降
* Oracle OpenJDK 11 および 15 以降
* Oracle Java 11.0 以降
* Oracle Java 8.0 以降

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a>Zulu Mission Control をインストールして JVM に接続する

Zulu Mission Control をインストールし、Java 仮想マシン (JVM) に接続し、実行中のアプリケーションのすべての側面をリアルタイムに把握するには、次の操作を行います。

1.  [Zulu Mission Control と互換性のある JDK と JRE をインストールします](java-jdk-install.md)。

1.  [Zulu Mission Control をダウンロード](https://www.azul.com/products/zulu-mission-control/)して、ご使用のシステムに適したバージョンを選択し、ローカルに保存してそのディレクトリに移動します。

1.  ダウンロードしたファイルを展開します。

    **Linux:**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows:**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **macOS:**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  互換性のある JDK のいずれかを使用して Java アプリケーションを起動します。 例:

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  Zulu Mission Control を起動します。

    **Linux:**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows:**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **macOS:**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  (省略可能) JVM インストールを Mission Control 用に切り替えます。

    Windows デバイスでは、*zmc.exe* はレジストリで構成されている既定の JVM インストールを使用します。 Zulu Mission Control は、ローカルの JVM インスタンスを自動的に検出することができるためには、完全な JDK から起動する必要があります。 インストールが JRE の場合、JVM は検出されず、次の警告が表示されます。

    > [!div class="mx-imgBorder"]
    ![JDK のインストールが JRE のみの場合の警告](../media/jdk/azul-jfr-1.png)

    Mission Control によって使用される JVM を変更するには、次の操作を行います。 

    a. *zmc.exe* ファイルと同じディレクトリにある *zmc.ini* 構成ファイルを開きます。

    b. 行 `-vmargs` の前に、2 つの行を追加します。  

       * 最初の行に、`–vm` を入力します。  
       * 2 行目に、JDK インストールへのパスを入力します (例: `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`)。

1.  次を行って、アプリケーションを実行している JVM を見つけます。

    a. Zulu Mission Control ウィンドウの左側のウィンドウで、 **[JVM Browser]\(JVM ブラウザー\)** タブを選択します。

    b. 左側で、自分のアプリケーションを実行している JVM インスタンスを選択して展開します。

    ![展開されたリスト内の JVM インスタンス](../media/jdk/azul-jfr-2.png)


1.  必要に応じて Flight Recording を開始します。

    a. 左側のウィンドウの **[Flight Recorder]** の下に、*No Recordings* メッセージが表示されている場合は、 **[Flight Recorder]** を右クリックして **[Start Flight Recording]\(Flight Recording の開始\)** を選択して、記録を開始します。

    b. **[Time fixed recording]\(時間固定記録\)** または **[Continuous recording]\(継続記録\)** のいずれかと、**プロファイル**構成 (詳細) または**継続**構成 (低オーバーヘッド) のいずれかを選択してから、 **[Finish]\(終了\)** を選択します。

    ![Flight Recording の開始](../media/jdk/azul-jfr-3.png)

    Flight Recording は、[JVM Browser]\(JVM ブラウザー\) の **Flight Recorder** の行の下に表示されます。

1. Flight Recording をダンプします。 これを行うには、Flight Recording を表す行を右クリックし、 **[Dump whole recording]\(記録全体をダンプ\)** を選択します。

    Zulu Mission Control ウィンドウの右側にある大きなウィンドウに新しいタブが表示されます。 このウィンドウは、アプリケーションを実行して、JVM から今ダンプされた Flight Recording を表しています。

1. Zulu Mission Control を使用して Flight Recording を確認します。 これを行うには、Zulu Mission Control ウィンドウの左側のウィンドウで、 **[Outline]\(アウトライン\)** タブを選択します。 このタブには、Flight Recording で収集されたデータのさまざまなビューが表示されます。
 
    ![Flight Recording のレビュー](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>リソース

詳細については、Azul Systems のサイトにアクセスして、「[Azul Webinar:Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)」(Azul ウェビナー: オープンソースの Flight Recorder と Mission Control - OpenJDK 8 パフォーマンスの管理と測定) をご覧ください。 このビデオは、Azul Systems 副 CTO の Simon Ritter 氏がナレーションをしています。 Flight Recorder の説明は 31 分 30 秒から始まります。

