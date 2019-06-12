---
title: IntelliJ を使用して Azure App Service 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for IntelliJ を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
keywords: java, intellij, web アプリ, azure app service, hello world, クイック スタート
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ae0749ce1ddab971f1a83e2e5e58492fd8ccb287
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626119"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a>IntelliJ を使用して Azure App Service 用の Hello World Web アプリを作成する

オープンソースの [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) プラグインを使用すれば、数分で、基本的な Hello World アプリケーション作成して Web アプリとして Azure App Service にデプロイできます。

> [!NOTE]
>
> Eclipse を使用したい場合は、[Eclipse 用の同様のチュートリアル][eclipse-hello-world]をご覧ください。
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> このチュートリアルを完了したら、忘れずにリソースをクリーンアップしてください。 その場合、このガイドの実行によって無料アカウントのクォータが超過することはありません。
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>インストールとサインイン

1. IntelliJ IDEA の [設定/環境設定] ダイアログ (Ctrl+Alt+S) で、 **[プラグイン]** を選択します。 次に、 **[Marketplace]** で **[Azure Toolkit for IntelliJ]** を見つけ、 **[インストール]** をクリックします。 インストール後、 **[再起動]** をクリックしてプラグインをアクティブにします。 

   ![Marketplace の Azure Toolkit for IntelliJ プラグイン][marketplace]

2. Azure アカウントにサインインするには、サイドバーの **Azure Explorer** を開き、上部のバーにある **[Azure サインイン]** アイコンをクリックします (または [IDEA] メニューから **[ツール]/[Azure]/ [Azure サインイン]** をクリックします)。

   ![IntelliJ Azure サインイン コマンド][I01]

3. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、次に **[サインイン]** をクリックします ([他のサインイン オプション](azure-toolkit-for-intellij-sign-in-instructions.md))。

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

4. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

5. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

   ![デバイスのログイン ブラウザー][I04]

6. **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="creating-web-app-project"></a>Web アプリ プロジェクトの作成

1. IntelliJ で、 **[ファイル]** メニュー、 **[新規作成]** 、 **[プロジェクト]** の順にクリックします。

   ![新しいプロジェクトの作成][file-new-project]

2. **[新しいプロジェクト]** ダイアログ ボックスで、 **[Maven]** 、 **maven-archetype-webapp** の順に選択し、 **[次へ]** をクリックします。

   ![Maven アーキタイプ Web アプリの選択][maven-archetype-webapp]

3. Web アプリの **[GroupId]** と **[ArtifactId]** を指定し、 **[次へ]** をクリックします。

   ![GroupId と ArtifactId の指定][groupid-and-artifactid]

4. Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、 **[次へ]** をクリックします。

   ![Maven 設定の指定][maven-options]

5. プロジェクト名と場所を指定し、 **[完了]** をクリックします。

   ![プロジェクト名の指定][project-name]

6. Project Explorer ビューで、**src/main/webapp/index.jsp** ファイルを開き、次のように編集して**変更を保存します**。

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![インデックス ページの編集][edit-index-page]

## <a name="deploying-web-app-to-azure"></a>Azure への Web アプリのデプロイ

1. Project Explorer ビューでプロジェクトを右クリックし、 **[Azure]** を展開して、次に **[Deploy to Azure]\(Azure へのデプロイ\)** をクリックします。

   ![[Deploy to Azure]\(Azure へのデプロイ\) メニュー][deploy-to-azure-menu]

1. [Deploy to Azure]\(Azure へのデプロイ\) ダイアログ ボックスで、既存の Tomcat Web アプリがある場合はそれにアプリケーションを直接デプロイできますが、それ以外の場合は、最初に新しいものを作成する必要があります。
   1. **[No Available webapp, click to create a new one]\(使用可能な Web アプリがありません。クリックして新しいものを作成してください\)** リンクをクリックし、新しい Web アプリを作成します。お使いのサブスクリプションに既存の Web アプリがある場合は、WebApp ドロップダウンから **[新しい Web アプリを作成する]** を選択できます。

      ![[Deploy to Azure]\(Azure へのデプロイ\) ダイアログ ボックス][deploy-to-azure-dialog]

   1. ポップアップ ダイアログ ボックスで、[Web コンテナー] として **TOMCAT 8.5-jre8** を選択し、その他の必要な情報を指定し、 **[OK]** をクリックして Web アプリを作成します。

      ![新しい Web アプリの作成][create-new-web-app-dialog]

   1. WebApp ドロップダウンから Web アプリを選択し、 **[実行]** をクリックします。(既存の Web アプリにデプロイする場合は、ここから開始できます)

      ![既存の Web アプリへのデプロイ][deploy-to-existing-webapp]

1. ツールキットにより Web アプリが正常にデプロイされた場合、状態メッセージと、デプロイされた Web アプリの URL (成功した場合) が表示されます。

   ![デプロイに成功][successfully-deployed]

1. ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。

   ![Web アプリの参照][browse-web-app]

## <a name="managing-deploy-configurations"></a>デプロイ構成の管理

1. Web アプリを発行すると、使用した設定が既定値として保存され、ツール バーの緑色の矢印アイコンをクリックすることでデプロイを実行できます。 設定を変更するには、Web アプリのドロップダウン メニューをクリックし、 **[Edit Configurations]\(構成の編集\)** をクリックします。

   ![[Edit Configurations]\(構成の編集\) メニュー][edit-configuration-menu]

1. **[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、 **[OK]** をクリックします。

   ![[構成の編集] ダイアログ ボックス][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a>リソースのクリーンアップ

1. Azure Explorer での Web アプリ の削除

     ![リソースのクリーンアップ][clean-resources]

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
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->
[marketplace]:./media/azure-toolkit-for-intellij-create-hello-world-web-app/marketplace.png
[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/clean-resource.png
[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png
