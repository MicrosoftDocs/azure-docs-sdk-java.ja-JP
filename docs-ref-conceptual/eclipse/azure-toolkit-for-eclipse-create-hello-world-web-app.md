---
title: Eclipse を使用して Azure 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for Eclipse を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 5e025c90c2619ec72ffddf5815fd49c3ac59c00f
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893093"
---
# <a name="create-a-hello-world-web-app-for-azure-using-eclipse"></a>Eclipse を使用して Azure 用の Hello World Web アプリを作成する

このチュートリアルでは、[Azure Toolkit for Eclipse] を使用して、Web アプリとして基本的な Hello World アプリケーションを作成し、Azure にデプロイする方法について説明します。

> [!NOTE]
>
> この記事の [Azure Toolkit for IntelliJ] 使用バージョンについては、「[IntelliJ を使用して Azure 用の Hello World Web アプリを作成する][intellij-hello-world]」を参照してください。
>

> [!IMPORTANT]
> 
> Azure Toolkit for Eclipse は 2017 年 8 月に更新され、別のワークフローが導入されました。 この記事では、Azure Toolkit for Eclipse のバージョン 3.0.7 以降を使用して Hello World Web アプリを作成する方法を示します。 バージョン 3.0.6 以前のツールキットを使用している場合は、[レガシ ツールキットを使用した Eclipse での Azure 用 Hello World Web アプリの作成][Legacy Version]に関するページの手順に従う必要があります。
> 

このチュートリアルを完了し、作成したアプリケーションを Web ブラウザーで開くと、次の図のようになります。

![Hello World アプリのプレビュー][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>新しい Web アプリ プロジェクトの作成

1. [Azure Toolkit for Eclipse の Azure サインイン手順][eclipse-sign-in-instructions] の説明に従って、Eclipse を起動し、Azure アカウントにサインインします。

1. **[ファイル]**、**[新規]**、**[Dynamic Web Project]\(動的 Web プロジェクト\)** の順にクリックします  (**[ファイル]** と **[新規]** のクリック後、使用可能なプロジェクトとして **[Dynamic Web Project (動的 Web プロジェクト)]** が表示されない場合は、**[ファイル]**、**[新規]**、**[プロジェクト]** の順にクリックし、**[Web]** を展開して、**[Dynamic Web Project (動的 Web プロジェクト)]**、**[次へ]** の順にクリックします)。

   ![新しい動的 Web プロジェクトの作成][file-new-dynamic-web-project]

2. このチュートリアルでは、プロジェクトに **MyWebApp**という名前を付けます。 画面は次のようになります。
   
   ![新しい動的 Web プロジェクトのプロパティ][dynamic-web-project-properties]

3. **[完了]** をクリックします。

4. Eclipse の Project Explorer ビューで、 **[MyWebApp]** を展開します。 **WebContent** を右クリックし、**[新規]**、**[JSP ファイル]** の順にクリックします。

   ![新しい JSP ファイルの作成][create-new-jsp-file]

5. **[New JSP File (新しい JSP ファイル)]** ダイアログ ボックスで **index.jsp** ファイルに名前を付け、親フォルダーは **MyWebApp/WebContent** のままにして **[次へ]** をクリックします。

   ![[New JSP File\(新しい JSP ファイル\)] ダイアログ ボックス][new-jsp-file-dialog]

6. **[Select JSP Template (JSP テンプレートの選択)]** ダイアログ ボックスで、このチュートリアルのために **[New JSP File (html) (新しい JSP ファイル (html))]** を選択し、**[完了]** をクリックします。

   ![JSP テンプレートの選択][select-jsp-template]

7. index.jsp ファイルが Eclipse で開いたら、"**Hello World!**" を動的に表示するためのテキストを 既存の `<body>` 要素に追加します。 更新された `<body>` コンテンツは、次のようになります。
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. index.jsp を保存します。

## <a name="deploy-your-web-app-to-azure"></a>Azure への Web アプリのデプロイ

1. Eclipse のプロジェクト エクスプローラー ビューでプロジェクトを右クリックし、**[Azure]**、**[Publish as Azure Web App]\(Azure Web アプリとして発行\)** の順に選択します。
   
   ![[Publish as Azure Web App (Azure Web アプリとして発行)]][publish-as-azure-web-app]

1. **[Deploy Web App]\(Web アプリのデプロイ\)** ダイアログ ボックスが表示されたら、次のいずれかのオプションを選択できます。

   * 既存の Web アプリを選択します (存在する場合)。

      ![アプリ サービスの選択][select-app-service]

   * **[Create New Web App]\(新しい Web アプリを作成する\)** をクリックする。

      ![App Service を作成する][create-app-service]

      **[App Service の作成]** ダイアログ ボックスで Web アプリに必要な情報を指定し、**[作成]** をクリックします。

      ![[App Service の作成] ダイアログ ボックス][create-app-service-dialog]

1. Web アプリを選択し、**[デプロイ]** をクリックします。

   ![アプリ サービスのデプロイ][deploy-app-service]

1. ツールキットにより Web アプリが正常にデプロイされると、**[Azure の活動ログ]** タブに **[発行済み]** 状態として表示され、デプロイされた Web アプリの URL へのハイパーリンクが設定されます。

   ![発行済み状態][publish-status]

1. ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。

   ![Web アプリの参照][browse-web-app]

1. Web を Azure に発行した後、アプリを管理するには、それを右クリックし、コンテキスト メニューからオプションのいずれかを選択します。 たとえば、Web アプリを**開始**、**停止**、または**削除**できます。

   ![アプリ サービスの管理][manage-app-service]

## <a name="next-steps"></a>次の手順

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->

[browse-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/browse-web-app.png
[file-new-dynamic-web-project]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/file-new-dynamic-web-project.png
[dynamic-web-project-properties]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/dynamic-web-project-properties.png
[create-new-jsp-file]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-new-jsp-file.png
[new-jsp-file-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/new-jsp-file-dialog.png
[select-jsp-template]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-jsp-template.png
[publish-as-azure-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-as-azure-web-app.png
[deploy-web-app-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-web-app-dialog.png
[select-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-app-service.png
[create-app-service-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service-dialog.png
[publish-status]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-status.png
[create-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service.png
[deploy-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-app-service.png
[manage-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/manage-app-service.png
