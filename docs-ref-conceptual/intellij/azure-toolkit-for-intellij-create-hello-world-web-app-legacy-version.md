---
title: IntelliJ 用のレガシ ツールキットを使用して Azure 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for IntelliJ バージョン 3.0.6 以前を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: efc1dffa248987772827bbe7bc0caa9f10a0b4ef
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893395"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-intellij"></a>IntelliJ 用のレガシ ツールキットを使用して Azure 用の Hello World Web アプリを作成する

このチュートリアルでは、[Azure Toolkit for IntelliJ] バージョン 3.0.6 以前を使用して、Web アプリとして基本的な Hello World アプリケーションを作成し、Azure にデプロイする方法について説明します。

> [!NOTE]
>
> この記事の [Azure Toolkit for Eclipse] 使用バージョンについては、「[Eclipse を使用して Azure 用の Hello World Web アプリを作成する][eclipse-hello-world]」を参照してください。
>

> [!IMPORTANT]
> 
> Azure Toolkit for IntelliJ は 2017 年 8 月に更新され、別のワークフローが導入されました。 この記事では、Azure Toolkit for IntelliJ のバージョン 3.0.6 以前を使用して Hello World Web アプリを作成する方法を示します。 バージョン 3.0.7 以降のツールキットを使用している場合は、[IntelliJ での Azure 用の Hello World Web アプリの作成][Updated Version]に関するページの手順に従う必要があります。
>

このチュートリアルを完了し、作成したアプリケーションを Web ブラウザーで開くと、次の図のようになります。

![Hello World アプリのプレビュー][01]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>新しい Web アプリ プロジェクトの作成

1. [Azure Toolkit for IntelliJ の Azure サインイン手順][intelliJ-sign-in-instructions]に関する記事の説明に従って、IntelliJ を起動し、Azure アカウントにサインインします。

1. **[ファイル]** メニューの **[New]\(新規\)** をクリックし、**[プロジェクト]** をクリックします。
   
   ![[File (ファイル)]、[New (新規)]、[Project (プロジェクト)]][02]

2. **[New Project]\(新しいプロジェクトの作成\)** ダイアログ ボックスで、**[Java]**、**[Web Application]\(Web アプリケーション\)**、**[Next]\(次へ\)** の順にクリックし、Project SDK を追加します。
   
   ![[新しいプロジェクト] ダイアログ][03a]
   
3. [Select Home Directory for JDK (JDK のホーム ディレクトリの選択)] ダイアログ ボックスで JDK がインストールされているフォルダーを選択し、**[OK]** をクリックします。 [New Project (新しいプロジェクト)] ダイアログ ボックスで **[Next (次へ)]** をクリックして続行します。
   
   ![JDK ホーム ディレクトリを指定する][03b]

4. このチュートリアルでは、プロジェクトに **Java-Web-App-On-Azure** という名前を付け、**[Finish (完了)]** をクリックします。
   
   ![[新しいプロジェクト] ダイアログ][04]

5. IntelliJ の [Project Explorer (プロジェクト エクスプローラー)] ビューで、**[Java-Web-App-On-Azure]**、**[Web]** の順に展開し、**index.jsp** をダブルクリックします。
   
   ![インデックスを開くページ][05c]

6. index.jsp ファイルが IntelliJ で開いたら、"**Hello World!**" を動的に表示するためのテキストを 既存の `<body>` 要素に追加します。 更新された `<body>` コンテンツは、次のようになります。
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. index.jsp を保存します。

## <a name="deploy-your-web-app-to-azure"></a>Azure への Web アプリのデプロイ

Java Web アプリを Azure にデプロイする方法はいくつかあります。 このチュートリアルでは、アプリケーションを Azure Web アプリ コンテナーにデプロイするという最も簡単な方法について説明します。特殊なプロジェクトの種類や追加のツールは必要ありません。 JDK と Web コンテナー ソフトウェアは Azure から提供されるので、自分でアップロードする必要はありません。必要なものは Java Web アプリのみです。 結果として、アプリケーションの発行プロセスにかかる時間は分単位ではなく、秒単位になります。

アプリケーションを発行する前に、まずモジュール設定を構成する必要があります。 そのためには、次の手順を実行してください。

1. IntelliJ の Project Explorer で、 **Java-Web-App-On-Azure** プロジェクトを右クリックします。 コンテキスト メニューが表示されたら、**[Open Module Settings (モジュール設定を開く)]** をクリックします。

   ![モジュール設定を開く][05a]

2. [プロジェクト構造] ダイアログが表示されます。

   a. **[プロジェクト設定]** の一覧で、**[Artifacts (アーティファクト)]** をクリックします。

   b. **[名前]** ボックスのアーティファクトの名前を、空白または特殊文字が含まれないように変更します。この名前が Uniform Resource Identifier (URI) で使用されるため、変更が必要になります。

   c. **[タイプ]** を **[Web Application: Archive (Web アプリケーション: アーカイブ)]** に変更します。

   d. **[OK]** をクリックして、[プロジェクト構造] ダイアログ ボックスを閉じます。

   ![モジュール設定を開く][05b]

モジュール設定を構成したら、次の手順を使用してアプリケーションを Azure に発行できます。

1. IntelliJ の Project Explorer で、 **Java-Web-App-On-Azure** プロジェクトを右クリックします。 コンテキスト メニューが表示されたら、**[Azure]**、**[Publish as Azure Web App (Azure Web アプリとして発行)]** の順に選択します。
   
   ![Azure に発行するためのコンテキスト メニュー][06]

2. まだ IntelliJ から Azure にサインインしていない場合、Azure アカウントにサインインするように求められます。 (複数の Azure アカウントがある場合、サインイン プロセス中に同じようなプロンプトが何度も表示されることがあります。 このような状況の場合、次のサインイン手順を続行します。)
   
   ![Azure のログイン ダイアログ][07]

3. Azure アカウントに正常にサインインすると、**[サブスクリプションの管理]** ダイアログ ボックスに、資格情報に関連付けられたサブスクリプションの一覧が表示されます。 (複数のサブスクリプションが表示された場合、その一部のみを使うには、使わないサブスクリプションのチェックボックスを必要に応じてオフにします)。サブスクリプションを選択したら、 **[閉じる]** をクリックします。
   
   ![サブスクリプションの管理][08]

4. **[Deploy to Azure Web App Container (Azure Web アプリ コンテナーにデプロイ)]** ダイアログ ボックスを開くと、以前に作成した Web アプリ コンテナーがすべて表示されます。コンテナーを作成していない場合、一覧は空欄です。
   
   ![アプリ コンテナー][09]

5. 以前に Azure Web アプリ コンテナーを作成していない場合、またはアプリケーションを新しいコンテナーに発行する場合は、次の手順を実行します。 作成済みの場合は、既存の Web アプリ コンテナーを選択し、以下の手順 6. に進みます。
   
   a. **+** 記号をクリックします。
      
      ![アプリ コンテナーの追加][10]

   b. **[New Web App Container (新しい Web アプリ コンテナー)]** ダイアログ ボックスが表示されます。このコンテナーは、後続のいくつかの手順で使用します。
      
      ![新しいアプリ コンテナー][11a]
   
   c. Web アプリ コンテナーの **[DNS Label (DNS ラベル)]** を入力します。これで、Azure の Web アプリケーションについて、ホスト URL のリーフ DNS ラベルが構成されます。 この名前は使用可能であり、Azure Web アプリの名前付け要件に準拠している必要があります。

   d. **[Web Container (Web コンテナー)]** ドロップダウン メニューで、アプリケーションに適したソフトウェアを選択します。
      
      現在、Tomcat 8、Tomcat 7 または Jetty 9 から選択することができます。 選択したソフトウェアの最新の配信が Azure で提供されます。また、これは Oracle によって作成され、Azure で提供された JDK 8 の最新の配信で実行されます。

   e. **[サブスクリプション]** ドロップダウン メニューで、このデプロイに使用するサブスクリプションを選択します。

   f. **[Resource Group (リソース グループ)]** ドロップダウン メニューで、Web アプリに関連付けるリソース グループを選択します。 (Azure リソース グループを使用すると、関連リソースをグループ化できるため、たとえば、リソースをまとめて削除できます。)
      
      (所有している場合は) 既存のリソース グループを選択して、下記のステップ g にスキップするか、以下のステップに従って、新しいリソース グループを作成します。
      
      * **[リソース グループ]** ドロップダウン メニューから、**&lt;&lt;新規リソース グループの作成&gt;&gt;** を選択します。
      * **[New Resource Group (新しいリソース グループ)]** ダイアログ ボックスが表示されます。
        
         ![New Resource Group][12]

      * **[Name]\(名前\)** テキスト ボックスに、新しいリソース グループの名前を入力します。
      * **[Region]\(リージョン\)** ドロップダウン メニューで、リソース グループに適した Azure データ センターの場所を選択します。
      * Click **OK**.

   g. **[App Service Plan (App Service プラン)]** ドロップダウン メニューには、選択したリソース グループに関連付けられた App Service プランが表示されます。 (App Service プランでは、Web アプリの場所、価格レベル、およびコンピューティング インスタンス サイズなどの情報を指定します。 単一の App Service プランを複数の Web Apps に使用できます。そのため、App Service プランは、特定の Web アプリのデプロイとは別に保持されます。)
      
      (所有している場合は) 既存の App Service プランを選択して、下記のステップ h にスキップするか、以下のステップに従って、新しい App Service プランを作成します。
      
      * **[App Service プラン]** ドロップダウン メニューから、**&lt;&lt;新しい App Service プランの作成&gt;&gt;** を選択します。
      * **[New App Service Plan (新しい App Service プラン)]** ダイアログ ボックスが表示されます。
        
         ![新しい App Service プラン][13]

      * **[Name]\(名前\)** ボックスに、新しい App Service プランの名前を入力します。
      * **[Location]\(場所\)** ドロップダウン メニューで、プランに適した Azure データ センターの場所を選択します。
      * **[Pricing Tier]\(価格レベル\)** ドロップダウン メニューで、プランに適した価格を選択します。 テスト目的の場合は、 **[Free]** を選択できます。
      * **[Instance Size]\(インスタンス サイズ\)** ドロップダウン メニューで、プランに適したインスタンス サイズを選択します。 テスト目的の場合は、 **[Small]** を選択できます。
      * Click **OK**.

   h. (オプション) 既定では、Java 8 の最新のディストリビューションは、Azure によって Web アプリ コンテナーに JVM として自動的にデプロイされます。 ただし、JVM の別のバージョンおよびディストリビューションを選択することができます。 そのためには、次の手順を実行してください。
      
   * **[New Web App Container (新しい Web アプリ コンテナー)]** ダイアログ ボックスの **[JDK]** タブをクリックします。
   * 次のオプションのいずれかを選択できます。
        
      * Azure によって提供される既定の JDK をデプロイする
      * Azure で利用可能な追加の JDK のドロップダウン リストからサード パーティの JDK をデプロイする
      * カスタム JDK をデプロイする。カスタム JDK は、ZIP ファイルとしてパッケージ化され、一般に入手可能であるか、Azure ストレージ アカウントに格納されている必要があります。
        
     ![[New Web App Container (新しい Web アプリ コンテナー)] の [JDK] タブ][11b]

   i. これらの手順をすべて完了すると、[New Web App Container] \(新しい Web アプリ コンテナー) ダイアログ ボックスは次の図のようになります。
      
      ![新しいアプリ コンテナー][14]
   
   j. **[OK]** をクリックすると、新しい Web アプリ コンテナーの作成が完了します。
       
      数秒待つと Web アプリ コンテナーの一覧が更新されます。一覧で新しく作成した Web アプリ コンテナーが選択されています。

6. これで、Azure への Web アプリの初期デプロイを完了する準備ができました。**[OK]** をクリックして、Java アプリケーションを選択した Web アプリ コンテナーにデプロイします。 既定では、アプリケーションはアプリケーション サーバーのサブディレクトリとしてデプロイされます。 ルート アプリケーションとしてデプロイする場合、**[Deploy to root (ルートにデプロイ)]** チェック ボックスをオンにして **[OK]** をクリックします。
   
   ![Azure へのデプロイ][15]

7. **[Azure Activity Log (Azure アクティビティ ログ)]** ビューが開き、Web アプリのデプロイの状態が表示されます。
   
   ![進捗状況インジケーター][16]
   
   Web アプリを Azure にデプロイするプロセスは、わずか数秒で完了します。 アプリケーションの準備ができると、 **[Status]\(状態\)** 列に **[Published]\(発行済み\)** というリンクが表示されます。 リンクをクリックすると、デプロイされた Web アプリのホーム ページに移動します。また、次のセクションの手順を使用して Web アプリを参照することもできます。

## <a name="browsing-to-your-web-app-on-azure"></a>Azure の Web アプリの参照

**Azure Explorer** ビューを使用すると、Azure の Web アプリを参照できます。

**[Azure Explorer (Azure エクスプローラー)]** ビューがまだ開いていない場合、IntelliJ の **[View (ビュー)]** メニュー、**[Tool Windows (ツール ウィンドウ)]**、**[Service Explorer (サービス エクスプローラー)]** の順にクリックして開きます。 まだログインしていない場合は、ログインするように求められます。

**[Azure Explorer]** ビューが表示されたら、次の手順に従って Web アプリを参照します。 

1. **[Azure]** ノードを展開します。
2. **[Web Apps (Web アプリ)]** ノードを展開します。 
3. 目的の Web App を右クリックします。
4. コンテキスト メニューが表示されたら、 **[Open in Browser]**(ブラウザーで開く) をクリックします。
   
   ![Web アプリの参照][17]

## <a name="updating-your-web-app"></a>Web アプリの更新

既存の実行中の Azure Web アプリを更新するプロセスは短時間で簡単です。更新には 2 つのオプションがあります。

* 既存の Java Web アプリのデプロイを更新できます。
* 同じ Web アプリ コンテナーに追加の Java アプリケーションを発行できます。

いずれの場合でもプロセスは同じで、かかる時間は数秒です。

1. IntelliJ の Project Explorer で、更新する Java アプリケーションを右クリックするか、既存の Web アプリ コンテナーに追加します。
2. コンテキスト メニューが表示されたら、**[Azure]**、**[Publish as Azure Web App (Azure Web アプリとして発行)]** の順に選択します。
3. 既にログインしているので、既存の Web アプリ コンテナーの一覧が表示されます。 Java アプリケーションを発行または再発行するコンテナーを選択し、 **[OK]** をクリックします。

数秒後、**[Azure Activity Log (Azure アクティビティ ログ)]** ビューに更新されたデプロイが **[Published (発行済み)]** と表示され、Web ブラウザーで更新されたアプリケーションを確認できます。

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>既存の Web アプリの起動、停止、再起動

既存の Azure Web アプリ コンテナー (コンテナー内にデプロイされているすべての Java アプリケーションを含む) を起動または停止するには、 **[Azure Explorer]** (Azure Explorer) ビューを使用できます。

**[Azure Explorer (Azure エクスプローラー)]** ビューがまだ開いていない場合、IntelliJ の **[View (ビュー)]** メニュー、**[Tool Windows (ツール ウィンドウ)]**、**[Service Explorer (サービス エクスプローラー)]** の順にクリックして開きます。 まだログインしていない場合は、ログインするように求められます。

**[Azure Explorer]** ビューが表示されたら、次の手順に従って Web アプリを起動また停止します。 

1. **[Azure]** ノードを展開します。
2. **[Web Apps (Web アプリ)]** ノードを展開します。 
3. 目的の Web App を右クリックします。
4. コンテキスト メニューが表示されたら、**[Start (起動)]****[Stop (停止)]**、または **[Restart (再起動)]** をクリックします。 メニュー項目はコンテキストに依存します。つまり、Web アプリが実行しているときは停止操作のみ、Web アプリが現在実行されていないときは起動操作のみを行うことができます。
   
   ![Web アプリの停止][18]

## <a name="next-steps"></a>次の手順

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Updated Version]: azure-toolkit-for-intellij-create-hello-world-web-app.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/01-Web-Page.png
[02]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/02-File-New-Project.png
[03a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-Dialog.png
[03b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-SDK-Dialog.png
[04]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/04-New-Project-Dialog.png
[05a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Module-Settings.png
[05b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Project-Structure-Dialog.png
[05c]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Index-Page.png
[06]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/06-Azure-Publish-Context-Menu.png
[07]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/07-Azure-Log-In-Dialog.png
[08]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/08-Manage-Subscriptions.png
[09]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/09-App-Containers.png
[10]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/10-Add-App-Container.png
[11a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container.png
[11b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container-JDK-Tab.png
[12]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/12-New-Resource-Group.png
[13]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/13-New-App-Service-Plan.png
[14]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/14-New-App-Container.png
[15]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/15-Deploy-To-Azure.png
[16]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/16-Progress-Indicator.png
[17]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/17-Browse-Web-App.png
[18]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/18-Stop-Web-App.png
