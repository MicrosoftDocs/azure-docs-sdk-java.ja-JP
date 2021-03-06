---
title: Eclipse を使用して Azure App Service 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for Eclipse を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
keywords: java, eclipse, web アプリ, azure app service, hello world, クイック スタート
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
ms.openlocfilehash: 7e88298afaf0b4601d85d6063b7096c79e677421
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625841"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a>Eclipse を使用して Azure App Service 用の Hello World Web アプリを作成する

オープンソースの [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) プラグインを使用すれば、数分で、基本的な Hello World アプリケーション作成して Web アプリとして Azure App Service にデプロイできます。

> [!NOTE]
>
> IntelliJ IDEA を使用したい場合は、[IntelliJ 用の同様のチュートリアル][intellij-hello-world]をご覧ください。
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> このチュートリアルを完了したら、忘れずにリソースをクリーンアップしてください。 その場合、このガイドの実行によって無料アカウントのクォータが超過することはありません。
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>インストールとサインイン

1. 次のボタンを実行中の Eclipse ワークスペースにドラッグして、Azure Toolkit for Eclipse プラグインをインストールします ([他のインストール オプション](azure-toolkit-for-eclipse-installation.md))。

    [![実行中の Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "実行中の Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要")

1. Azure アカウントにサインインするには、 **[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。
   ![Azure サインイン用の Eclipse メニュー][I01]

1. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、次に **[サインイン]** をクリックします ([他のサインイン オプション](azure-toolkit-for-eclipse-sign-in-instructions.md))。

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

1. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

1. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

   ![デバイスのログイン ブラウザー][I04]

1. 最後に、 **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="creating-web-app-project"></a>Web アプリ プロジェクトの作成

1. **[ファイル]** 、 **[新規]** 、 **[Dynamic Web Project]\(動的 Web プロジェクト\)** の順にクリックします ( **[ファイル]** と **[新規]** のクリック後、使用可能なプロジェクトとして **[Dynamic Web Project (動的 Web プロジェクト)]** が表示されない場合は、 **[ファイル]** 、 **[新規]** 、 **[プロジェクト]** の順にクリックし、 **[Web]** を展開して、 **[Dynamic Web Project (動的 Web プロジェクト)]** 、 **[次へ]** の順にクリックします)。

   ![新しい動的 Web プロジェクトの作成][file-new-dynamic-web-project]

2. このチュートリアルでは、プロジェクトに **MyWebApp**という名前を付けます。 画面は次のようになります。
   
   ![新しい動的 Web プロジェクトのプロパティ][dynamic-web-project-properties]

3. **[完了]** をクリックします。

4. Eclipse の Project Explorer ビューで、 **[MyWebApp]** を展開します。 **WebContent** を右クリックし、 **[新規]** 、 **[JSP ファイル]** の順にクリックします。

   ![新しい JSP ファイルの作成][create-new-jsp-file]

5. **[New JSP File (新しい JSP ファイル)]** ダイアログ ボックスで **index.jsp** ファイルに名前を付け、親フォルダーは **MyWebApp/WebContent** のままにして **[次へ]** をクリックします。

   ![[New JSP File\(新しい JSP ファイル\)] ダイアログ ボックス][new-jsp-file-dialog]

6. **[Select JSP Template (JSP テンプレートの選択)]** ダイアログ ボックスで、このチュートリアルのために **[New JSP File (html) (新しい JSP ファイル (html))]** を選択し、 **[完了]** をクリックします。

   ![JSP テンプレートの選択][select-jsp-template]

7. index.jsp ファイルが Eclipse で開いたら、"**Hello World!** " を動的に表示するためのテキストを 既存の `<body>` 要素に追加します。 更新された `<body>` コンテンツは、次のようになります。
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. index.jsp を保存します。

## <a name="deploying-web-app-to-azure"></a>Azure への Web アプリのデプロイ

1. Eclipse のプロジェクト エクスプローラー ビューでプロジェクトを右クリックし、 **[Azure]** 、 **[Publish as Azure Web App]\(Azure Web アプリとして発行\)** の順に選択します。
   
   ![[Publish as Azure Web App (Azure Web アプリとして発行)]][publish-as-azure-web-app]

1. **[Deploy Web App]\(Web アプリのデプロイ\)** ダイアログ ボックスが表示されたら、次のいずれかのオプションを選択できます。

   * 既存の Web アプリを選択します (存在する場合)。

      ![アプリ サービスの選択][select-app-service]

   * **[Create New Web App]\(新しい Web アプリを作成する\)** をクリックする。

      ![App Service を作成する][create-app-service]

      **[App Service の作成]** ダイアログ ボックスで Web アプリに必要な情報を指定し、 **[作成]** をクリックします。

      ここで、ランタイム環境、アプリ設定、サービス プラン、およびリソース グループを構成できます。

      ![[App Service の作成] ダイアログ ボックス][create-app-service-dialog]

1. Web アプリを選択し、 **[デプロイ]** をクリックします。

   ![アプリ サービスのデプロイ][deploy-app-service]

1. ツールキットにより Web アプリが正常にデプロイされると、 **[Azure の活動ログ]** タブに **[発行済み]** 状態として表示され、デプロイされた Web アプリの URL へのハイパーリンクが設定されます。

   ![発行済み状態][publish-status]

1. ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。

   ![Web アプリの参照][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="cleaning-up-resources"></a>リソースのクリーンアップ

1. Web アプリを Azure に発行した後、それを管理するには、Azure Explorer で右クリックし、コンテキスト メニューからオプションのいずれかを選択します。 たとえば、ここで Web アプリを **[削除]** して、このチュートリアルのリソースをクリーンアップできます。

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
[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

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
