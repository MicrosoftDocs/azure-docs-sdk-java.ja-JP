---
title: "Azure Toolkit for IntelliJ を使用して Linux コンテナーで Hello World Web アプリを実行する"
description: "Linux コンテナーに基本的な Hello World Web アプリを作成し、IntelliJ の Azure Toolkit を使用して Azure に発行する方法を説明します。"
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: b6b8db6c7713157a35b6a113d7ec69b4419386d4
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="run-a-hello-world-web-app-in-a-linux-container-by-using-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ を使用して Linux コンテナーで Hello World Web アプリを実行する

[Docker] コンテナーは、Web アプリケーションをデプロイするために広く使用されている方法です。 Docker コンテナーを使用すると、開発者は、すべてのプロジェクト ファイルと依存関係を、サーバーにデプロイするために 1 つのパッケージに統合できます。 Azure Toolkit for IntelliJ には、Microsoft Azure にデプロイする機能が追加されているため、Java 開発者はプロセスを簡略化できます。

この記事では、基本的な Hello World Web アプリを作成し、Azure Toolkit for IntelliJ を使用して Azure 用の Linux コンテナーに Web アプリを発行するために必要な手順について説明します。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* [Docker] クライアント。

> [!NOTE]
>
> このチュートリアルの手順を実行するには、TLS を使用せず、ポート 2375 でデーモンを公開する [Docker] を構成する必要があります。 この設定は、Docker のインストール時、または、[Docker の設定] メニューから構成できます。
>
> ![[Docker の設定] メニュー][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>新しい Web アプリ プロジェクトの作成

1. 「Azure Toolkit for IntelliJ のサインイン手順」の記事に記載されている手順を使用して、IntelliJ を起動し Azure アカウントにサインインします。

1. **[ファイル]** メニューの **[New]\(新規\)** をクリックし、**[プロジェクト]** をクリックします。
   
   ![新しいプロジェクトの作成][file-new-project]

1. **[新しいプロジェクト]** ダイアログ ボックスで、**[Maven]**、**[maven-archetype-webapp]** の順に選択し、**[次へ]** をクリックします。
   
   ![Maven アーキタイプ Web アプリの選択][maven-archetype-webapp]
   
1. Web アプリの **[GroupId]** と **[ArtifactId]** を指定し、**[次へ]** をクリックします。
   
   ![GroupId と ArtifactId の指定][groupid-and-artifactid]

1. Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、**[次へ]** をクリックします。
   
   ![Maven 設定の指定][maven-options]

1. プロジェクト名と場所を指定し、**[完了]** をクリックします。
   
   ![プロジェクト名の指定][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Azure Container Registry をプライベート Docker レジストリとして使用するために作成する

Azure Portal を使用して Azure Container Registry を作成する手順を説明します。

> [!NOTE]
>
> Azure ポータルではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成][Create Docker Registry using Azure CLI]」の手順に従ってください。
>

1. [Azure ポータル]を参照して、サインインします。

   Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。

1. **[+ 新規]** のメニュー アイコン、**[コンテナー]**、**[Azure Container Registry]** の順にクリックします。
   
   ![Azure Container Registry を新しく作成する][AR01]

1. Azure Container Registry テンプレートの情報ページが表示されたら、**[作成]** をクリックします。 

   ![Azure Container Registry を新しく作成する][AR02]

1. **[コンテナー レジストリの作成]** ページが表示されたら、**[レジストリ名]** と **[リソース グループ]** を入力し、**[管理ユーザー]** に対して **[有効化]** を選択した後、**[作成]** をクリックします。

   ![Azure Container Registry 設定を構成する][AR03]

1. コンテナー レジストリが作成されたら、Azure Portal でコンテナー レジストリに移動して、**[アクセス キー]** をクリックします。 次の手順で使用するため、ユーザー名とパスワードをメモします。

   ![Azure Container Registry のアクセス キー][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a>Docker コンテナーに Web アプリを配置する

1. プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択し、**[Add Docker Support]\(Docker サポートの追加\) **をクリックします。

   既定の構成の Docker ファイルが自動的に作成されます。

   ![Docker サポートの追加][add-docker-support]

1. Docker のサポートを追加したら、プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択し、**[Run on Web App (Linux)]\(Web App を (Linux 上で) 実行\)** をクリックします。

   ![[Run on Web App]\(Web アプリで実行\)][run-on-web-app-linux]

1. **[Run on Web App]\(Web アプリで実行\)** ダイアログ ボックスが表示されたら、必要な情報を入力します。

   * **[名前]**: Azure Toolkit に表示されるフレンドリ名を指定します。 

   * **[サーバーの URL]**: この記事の前のセクションで説明されている、コンテナー レジストリの URL を指定します。通常、"*registry*.azurecr.io" の構文が使用されます。 

   * **[ユーザー名]** と **[パスワード]**: この記事の前のセクションで説明されている、コンテナー レジストリのアクセス キーを指定します。 

   * **[Image and tag]\(イメージとタグ\)**: コンテナー イメージ名を指定します。通常、"*registry*.azurecr.io/*appname*: latest" の構文が使用されます。ここで、 
      * *registry* は、この記事の前のセクションで説明されているコンテナー レジストリを指します。 
      * *appname* は、Web アプリの名前です。 

   * **[Use Existing Web App]\(既存の Web アプリを使用\)** または**[Create New Web App]\(新しい Web アプリの作成\)**: 既存の Web アプリへコンテナーを展開するか、または新しい Web アプリを作成するかを指定します。 

   * **[リソース グループ]** で、新しいリソース グループを作成するか、既存のリソース グループを使用するかを指定します。 

   * **[App Service プラン]** では、新しいリソース グループを作成するか、既存のリソース グループを使用するかを指定します。 

1. 上記の設定の構成が完了したら、**[実行]** をクリックします。

   ![Create Web App][create-web-app]

1. Web アプリを発行すると、使用した設定が既定の設定として保存されます。ツール バーの緑色の矢印アイコンをクリックすると、Azure でアプリケーションを実行できます。 設定を変更するには、Web アプリのドロップダウン メニューをクリックし、**[構成の編集]** をクリックします。

   ![[構成の編集] メニュー][edit-configuration-menu]

1. **[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、**[OK]** をクリックします。

   ![[構成の編集] ダイアログ ボックス][edit-configuration-dialog]

## <a name="next-steps"></a>次のステップ

Docker の他のリソースについては、公式の [Docker の Web サイト]の [Docker] を参照してください。

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure ポータル]: https://portal.azure.com/
[Azure ポータルを使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[AR01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR01.png
[AR02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR02.png
[AR03]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR03.png
[AR04]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR04.png

[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[create-web-app]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-web-app.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
