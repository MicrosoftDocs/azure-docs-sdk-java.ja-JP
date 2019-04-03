---
title: IntelliJ を使用して Azure 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for IntelliJ を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
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
ms.openlocfilehash: 7055751d1b1c37e019ef4ed59f1710ce6905e9f8
ms.sourcegitcommit: a108a82414bd35be896e3c4e7047f5eb7b1518cb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489640"
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a>IntelliJ を使用して Azure 用の Hello World Web アプリを作成する

このチュートリアルでは、[Azure Toolkit for IntelliJ] を使用して、Web アプリとして基本的な Hello World アプリケーションを作成し、Azure にデプロイする方法について説明します。

> [!NOTE]
>
> この記事の [Azure Toolkit for Eclipse] 使用バージョンについては、「[Eclipse を使用して Azure 用の Hello World Web アプリを作成する][eclipse-hello-world]」を参照してください。
>

> [!IMPORTANT]
> 
> Azure Toolkit for IntelliJ は 2017 年 8 月に更新され、別のワークフローが導入されました。 この記事では、Azure Toolkit for IntelliJ バージョン 3.0.7 以降を使用して Hello World Web アプリを作成する方法を示します。 バージョン 3.0.6 以前のツールキットを使用している場合は、[レガシ ツールキットを使用した IntelliJ での Azure 用 Hello World Web アプリの作成][Legacy Version]に関するページの手順に従う必要があります。
> 

このチュートリアルを完了し、作成したアプリケーションを Web ブラウザーで開くと、次の図のようになります。

![Hello World アプリのプレビュー][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>新しい Web アプリ プロジェクトの作成

1. [Azure Toolkit for IntelliJ の Azure サインイン手順][intelliJ-sign-in-instructions]に関する記事の説明に従って、IntelliJ を起動し、Azure アカウントにサインインします。

1. **[ファイル]** メニューの **[New]\(新規\)** をクリックし、**[プロジェクト]** をクリックします。
   
   ![新しいプロジェクトの作成][file-new-project]

1. **[新しいプロジェクト]** ダイアログ ボックスで、 **[Maven]**、 **maven-archetype-webapp** の順に選択し、**[次へ]** をクリックします。
   
   ![Maven アーキタイプ Web アプリの選択][maven-archetype-webapp]
   
1. Web アプリの **[GroupId]** と **[ArtifactId]** を指定し、**[次へ]** をクリックします。
   
   ![GroupId と ArtifactId の指定][groupid-and-artifactid]

1. Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、**[次へ]** をクリックします。
   
   ![Maven 設定の指定][maven-options]

1. プロジェクト名と場所を指定し、**[完了]** をクリックします。
   
   ![プロジェクト名の指定][project-name]

1. IntelliJ のプロジェクト エクスプローラー ビューで、**[src]**、**[main]**、**[webapp]** の順に展開し、**index.jsp** をダブルクリックします。
   
   ![インデックス ページを開く][open-index-page]

1. index.jsp ファイルが IntelliJ で開いたら、"**Hello World!**" を動的に表示するためのテキストを 既存の `<body>` 要素に追加します。 更新された `<body>` コンテンツは、次のようになります。
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![インデックス ページの編集][edit-index-page]

1. index.jsp を保存します。

## <a name="deploy-your-web-app-to-azure"></a>Azure への Web アプリのデプロイ

1. IntelliJ のプロジェクト エクスプローラー ビューでプロジェクトを右クリックし、**[Azure]**、**[Run on Web App]\(Web アプリで実行\)** の順に選択します。
   
   ![[Run on Web App]\(Web アプリで実行\) メニュー][run-on-web-app-menu]

1. [Run on Web App]\(Web アプリで実行\) ダイアログ ボックスで、次のいずれかのオプションを選択できます。

   * 既存の Web アプリ (存在する場合) を選択して **[Run]\(実行\)** をクリックする。

      ![[Run on Web App]\(Web アプリで実行\) ダイアログ ボックス][run-on-web-app-dialog]

   * WebApp ドロップダウンの **[新しい Web アプリを作成する]** をクリックします。 新しい Web アプリを作成する場合は、ご自身の Web アプリに必要な情報を指定し、Web アプリの作成後に **[実行]** をクリックします。

      ![新しい Web アプリの作成][create-new-web-app-dialog]

1. Web アプリが正常にデプロイされると、その Web アプリの URL を含むステータス メッセージがツールキットに表示されます。

   ![デプロイに成功][successfully-deployed]

1. ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。

   ![Web アプリの参照][browse-web-app]

1. Web アプリを発行した後、使用した設定が既定の設定として保存されます。ツール バーの緑色矢印のアイコンをクリックすることで、Azure でアプリケーションを実行できます。 設定を変更するには、Web アプリのドロップダウン メニューをクリックし、**[Edit Configurations]\(構成の編集\)** をクリックします。

   ![[Edit Configurations]\(構成の編集\) メニュー][edit-configuration-menu]

1. **[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、**[OK]** をクリックします。

   ![[構成の編集] ダイアログ ボックス][edit-configuration-dialog]

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

[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[run-on-web-app-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[run-on-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
