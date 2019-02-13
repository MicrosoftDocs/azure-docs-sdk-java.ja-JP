---
title: Azure Toolkit for Eclipse を使用して Linux コンテナーで実行されている Hello World Web アプリをクラウドにデプロイする
description: Linux コンテナーで基本的な Hello World Web アプリを実行し、Azure Toolkit for Eclipse を使用してクラウドにデプロイします。
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/20/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 799f21a282956f9a88aa35743157fc1292197569
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55649248"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse を使用して Hello World Web アプリをクラウドの Linux コンテナーにデプロイする

[Docker] コンテナーは、Web アプリケーションをデプロイするために広く使用されている方法です。 Docker コンテナーを使用すると、開発者は、すべてのプロジェクト ファイルと依存関係を、サーバーにデプロイするために 1 つのパッケージに統合できます。 Azure Toolkit for Eclipse には、コンテナーを Microsoft Azure にデプロイするための機能が追加されているため、Java 開発者はこのプロセスを簡素化できます。

この記事では、基本的な Hello World Web アプリを作成し、Azure Toolkit for Eclipse を使用して Linux コンテナーの Web アプリを Azure に 発行するために必要な手順について説明します。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]
* [Docker] クライアント。

> [!NOTE]
>
> このチュートリアルの手順を実行するには、TLS を使用せず、ポート 2375 でデーモンを公開する [Docker] を構成する必要があります。 この設定は、Docker のインストール時、または、[Docker の設定] メニューから構成できます。
>
> ![[Docker の設定] メニュー][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>新しい Web アプリ プロジェクトの作成

1. Eclipse を起動し、[Azure Toolkit for Eclipse のサインイン手順](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)に関する記事の手順に従って、Azure アカウントにサインインします。

1. **[File]\(ファイル\)** メニューをクリックし、**[New]\(新規作成\)**、**[Dynamic Web Project]\(動的 Web プロジェクト\)** の順にクリックします。
   
   ![新しいプロジェクトの作成][file-new-project]

1. **[New Dynamic Web Project]\(新しい動的 Web プロジェクト\)** ダイアログ ボックスで、プロジェクト名と場所を指定し、**[Finish]\(完了\)** をクリックします。
   
   ![プロジェクト名の指定][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Azure Container Registry をプライベート Docker レジストリとして使用するために作成する

Azure Portal を使用して Azure Container Registry を作成する手順を説明します。

> [!NOTE]
>
> Azure ポータルではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成][Create Docker Registry using Azure CLI]」の手順に従ってください。
>

1. [Azure ポータル]を参照して、サインインします。

   Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。

1. **[+ リソースの作成]** のメニュー アイコンをクリックし、**[コンテナー]**、**[コンテナー レジストリ]** の順にクリックします。
   
   ![Azure Container Registry を新しく作成する][create-container-registry-01]

1. **[コンテナー レジストリの作成]** ページが表示されたら、**[レジストリ名]** と **[リソース グループ]** を入力し、**[管理ユーザー]** に対して **[有効化]** を選択した後、**[作成]** をクリックします。

   ![Azure Container Registry 設定を構成する][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a>Docker コンテナーに Web アプリを配置する

1. プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択し、**[Add Docker Support]\(Docker サポートの追加\)** をクリックします。

   既定の構成の Docker ファイルが自動的に作成されます。

   ![Docker サポートの追加][add-docker-support]

1. Docker のサポートを追加したら、プロジェクト エクスプローラーでプロジェクトを右クリックし、**[Azure]** を選択して、**[Publish to Web App for Containers]\(Web App for Containers に発行\)** をクリックします。

   ![[Publish to Web App for Containers]\(Web App for Containers に発行\)][run-on-web-app-for-containers]

1. **[Run on Web App for Containers]\(Web App for Containers で実行\)** ダイアログ ボックスが表示されたら、必要な情報を入力します。

   * **[Docker ファイル]**:Docker のサポートをプロジェクトに追加したときに作成された Docker ファイルのパスを指定します。 

   * **[コンテナー レジストリ]**:この記事の前のセクションで作成したコンテナー レジストリをドロップダウン メニューから選択します。 **[サーバーの URL]**、**[ユーザー名]**、**[パスワード]** の各フィールドには、値が自動的に入力されます。

   * **[Image and tag]\(イメージとタグ\)**:コンテナー イメージ名を指定します。通常は、"*registry*.azurecr.io/*appname*: latest" の構文が使用されます。 
      * *registry* は、この記事の前のセクションで説明されているコンテナー レジストリを指します。 
      * *appname* は、Web アプリの名前です。 

   * **[Web App for Container]**:**[既存のものを使用]** または **[新規作成]** を選択して、コンテナーを既存の Web アプリに展開するか、新しい Web アプリを作成するかを指定します。  指定した**アプリ名**を使用して Web アプリの URL が作成されます (例: *wingtiptoys.azurewebsites.net*)。

   * **リソース グループ**:既存のリソース グループを使用するか、新しいリソース グループを作成するかを指定します。 

   * **[App Service プラン]**:既存の App Service プランを使用するか、新しい App Service プランを作成するかを指定します。 

   ![[Run on Web App for Containers]\(Web App for Containers で実行\)][run-on-web-app-linux]

1. 上記の設定の構成が完了したら、**[OK]** をクリックして Web アプリを Azure に発行します。

1. Web アプリが発行されたら、以前に指定した Web アプリの URL (例: *wingtiptoys.azurewebsites.net*) を参照できます。

   ![Web アプリの参照][browsing-to-web-app]

## <a name="next-steps"></a>次の手順

Docker の他のリソースについては、公式の [Docker の Web サイト]の [Docker] を参照してください。

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure ポータル]: https://portal.azure.com/
[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[add-docker-support]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-eclipse-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/file-new-project.png
[project-name]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-linux.png
