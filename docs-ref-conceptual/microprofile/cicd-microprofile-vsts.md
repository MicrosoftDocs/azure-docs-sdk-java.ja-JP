---
title: Azure DevOps を使用した MicroProfile アプリケーションの CI/CD
description: Azure DevOps を使用して、MicroProfile アプリケーションを Azure Web App for Containers インスタンスにデプロイするための CI/CD リリース サイクルを設定する方法について説明します。
services: Azure DevOps
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/14/2018
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: 818e37291fa47f99cb161c63a86062bddbf6248c
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799938"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a>Azure DevOps を使用した MicroProfile アプリケーションの CI/CD

このチュートリアルでは、Java EE 開発者が、Azure DevOps (公式には VSTS として知られています) を使用して、[MicroProfile](http://microprofile.io) アプリケーションを Azure Web App for Containers にデプロイするための CI/CD リリース サイクルを簡単に設定する方法について説明します。  この例では、[Payara Micro](https://www.payara.fish/payara_micro) を基本イメージとして使用する MicroProfile アプリケーションを使用します。   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
Docker イメージをビルドし、コンテナー イメージを Azure Container Registry にプッシュして、Azure DevOps コンテナー化プロセスを開始します。  次に、Azure DevOps リリース パイプラインを使用して、コンテナー イメージを Web アプリに展開します。

## <a name="prerequisites"></a>前提条件
- [GitHub](https://github.com/Azure-Samples/microprofile-hello-azure) から Git URL をコピーして保存します。
- [Azure DevOps](https://dev.azure.com) アカウントに登録またはログインします。
- 新しい [Azure DevOps プロジェクト](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav)を作成し、上記の Git URL を使用して**リポジトリをインポート**します。
- [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR) を作成します。
- Azure Web App for Containers を作成します。
  > [!NOTE]
  >
  > Web アプリ インスタンスをプロビジョニングするときに、コンテナーの設定で [クイック スタート] を選択します。


## <a name="create-a-build-definition"></a>ビルド定義を作成する

Azure DevOps のビルド定義により、Java EE アプリケーションのソース アプリケーションでコミットが発生するたびに、ビルド内のすべてのタスクが自動的に実行されます。  この例では、Azure DevOps で Maven を使用して Java MicroProfile プロジェクトをビルドします。

1. Azure DevOps プロジェクト ページの上部にある [Build and Release]\(ビルドとリリース\) タブをクリックします。  次に、**[ビルド]** リンクを選択します。 

<img src="media/VSTS/Buid-and-Release1.png">

2. **[新しいパイプライン]** をクリックし、**[続行]** をクリックしてビルド タスクの定義を開始します。
3. テンプレートの一覧から [Maven] を選択し、**[適用]** をクリックして Java プロジェクトをビルドします。
4. [エージェント プール] フィールドのドロップダウン メニューで、**[Hosted Linux Preview]\(ホストされている Linux プレビュー\)** を選択します。
   > [!NOTE]
   >
   > これにより、使用するビルド サーバーが Azure DevOps に通知されます。  カスタマイズされたプライベート ビルド サーバーを使用できます。

5. 継続的インテグレーション用にビルドを構成するには、**[トリガー]** タブを選択し、**[継続的インテグレーションを有効にする]** チェック ボックスをオンにします。  

<img src="media/VSTS/Build-Triggers2.png"> 

6. <strong>[タスク]</strong> タブを選択して、ビルド パイプラインのメイン ページに戻ります。
7. <strong>[保存してキューに登録]</strong> ドロップダウン メニューで、<strong>[保存]</strong> を選択します。


## <a name="create-a-docker-build-image"></a>Docker ビルド イメージを作成する

このタスクでは、Azure DevOps で Payara Micro の基本イメージを指定した Dockerfile を使用して Docker イメージを作成します。  

1. **[タスク]** タブを選択して、ビルド パイプラインのメイン ページに戻ります。
2. **[+]** アイコンをクリックして、ビルド定義に新しいタスクを追加します。

<img src="media/VSTS/Tasks-add4.png">

3. テンプレートの一覧から &quot;Docker&quot; を選択し、<strong>[追加]</strong> をクリックします。
4. <strong>[表示名]</strong> フィールドにわかりやすい名前を入力します。
5. <strong>[コンテナー レジストリの種類]</strong> のドロップダウン メニューで <strong>[Azure Container Registry]</strong> が選択されていることを確認します。
&gt; [!NOTE]
&gt; &gt;  Docker Hub または別のレジストリを使用する場合は、代わりに &quot;Container Registry&quot; を選択します。  次に、[+ 新規] をクリックして、資格情報と接続情報を入力します。 その後、[コマンド] セクションに進んで続行します。

6. **[Azure サブスクリプション]** ドロップダウン メニューで、Azure サブスクリプション ID を選択します。  次に、**[承認]** をクリックします。
7. **[Azure Container Registry]** ドロップダウン メニューで、Azure で作成したレジストリ名を選択します。
8. **[コマンド]** ドロップダウン メニューで **[build]** が選択されていることを確認します。
9. **Dockerfile** については、テキスト ボックスの横のパス ナビゲーション アイコンをクリックして、GitHub プロジェクトから Dockerfile を選択します。  次に、**[OK]** をクリックします。

<img src="media/VSTS/Dockerfile5.png">

10. **[イメージ名]** で、**[最終のタグを含める]** チェック ボックスをオンにします。 
11. **[保存してキューに登録]** ドロップダウン メニューで、**[保存]** を選択します。

## <a name="push-docker-image-to-acr"></a>Docker イメージを ACR にプッシュする

このタスクでは、Azure DevOps で Docker イメージを Azure Container Registry にプッシュします。  これは、MicroProfile API アプリケーションを、コンテナー化された Java Web アプリとして実行するために使用されます。

1. Azure DevOps で Docker を使用しているので、前述の「**Docker ビルド イメージを作成する**」の手順 1. から 7. を繰り返して、新しい Docker テンプレートを作成します。
2. **[コマンド]** ドロップダウン メニューで **[push]** を選択します。
3. **[保存してキューに登録]** タブをクリックし、**[保存してキューに登録]** を選択します。
4. ポップアップ ウィンドウのエージェント プールに、**[Hosted Linux Preview]\(ホストされている Linux プレビュー\)** が選択されていることを確認します。  次に、**[保存してキューに登録]** をクリックします。
5. ビルド番号をクリックして、Java プロジェクトのビルド パイプラインが正常に完了したことを確認します。

<img src="media/VSTS/Build-Number6.png">


## <a name="create-a-release-definition-for-a-java-app"></a>Java アプリのリリース定義を作成する

Azure DevOps のリリース パイプラインでは、ビルド プロセスが正常に完了するとすぐに、Azure などのターゲット環境へのビルド成果物の配置が自動的にトリガーされます。   開発、テスト、ステージング、運用の各環境用のリリース パイプラインを作成できます。

1. Azure DevOps プロジェクト ページの上部にある [Build and Release]\(ビルドとリリース\) タブをクリックします。  次に、**[リリース]** リンクを選択します。

<img src="media/VSTS/Release-new-pipeline7.png">

2. [新しいパイプライン] をクリックします。
3. テンプレートの一覧で <strong>[Java アプリを Azure App Service に配置]</strong> を選択し、<strong>[適用]</strong> をクリックします。

<img src="media/VSTS/deploy-java-template8.png">

4. <strong>ステージ名</strong> (例: 開発、テスト、ステージング、運用) を設定します。  次に、<strong>[X]</strong> ボタンをクリックしてポップアップ ウィンドウを閉じます。
5. [成果物] セクションの <strong>[+ 追加]</strong> をクリックします。  これにより、ビルド定義からこのリリース定義に成果物がリンクされます。<br/>6.<strong>[Source (build pipeline)]\(ソース (ビルド パイプライン)\)</strong> のドロップダウン メニューでビルド定義を選択します。 次に、<strong>[追加]</strong> をクリックして続行します。

<img src="media/VSTS/add-artifact9.png">

7. パイプラインの <strong>[タスク]</strong> タブをクリックします。  次に、ステージ名を選択します。

<img src="media/VSTS/release-stage10.png">

8. **[Azure サブスクリプション]** ドロップダウン メニューで、Azure サブスクリプション ID を選択します。
9. **[アプリの種類]** ドロップダウン メニューで、**[Linux アプリ]** を選択します。
10. **[App Service の名前]** ドロップダウン メニューから、上記で作成した Web App for Containers インスタンスの名前を選択します。
11. **[Registry or Namespaces]\(レジストリまたは名前空間\)** フィールドに、Azure コンテナー レジストリの名前を入力します   (例: **myregistry.azure.io**)。
12. **[リポジトリ]** フィールドにレジストリ名を入力します。
13. **[Deploy Azure App Service]\(Azure App Service の配置\)** をクリックします。  **[タグ]** テキスト ボックスに、コンテナー イメージのタグを入力します。 
14. **[エージェントで実行]** をクリックします。  エージェント プールのドロップダウン メニューで、**[Hosted Linux Preview]\(ホストされている Linux プレビュー\)** を選択します。 

## <a name="setup-environment-variables"></a>環境変数を設定する

1. **[変数]** タブをクリックします。次に、**[+ 追加]** をクリックして環境変数を定義します。
2. コンテナー レジストリの URL、ユーザー名、パスワードの変数名と値を追加します。   セキュリティを確保するために、ロック アイコンをクリックして、パスワード値を非表示にしておきます。

例: 
- registry.password
- registry.url
- registry.username

<img src="media/VSTS/environment-variables12.png">

3. **[タスク]** タブをクリックして、リリース パイプライン定義のメイン ページに戻ります。
4. **[Deploy Azure App Service]\(Azure App Service の配置\)** をクリックします。 
5. **[Application and Configuration Settings]\(アプリケーションと構成の設定\)** セクションを展開し、**[アプリケーション設定]** フィールドのナビゲーション パスをクリックして、配置時にコンテナー レジストリに接続するための環境変数を追加します。
6. **[+ 追加]** をクリックして、次のアプリ設定を定義し、環境変数を割り当てます。
7. DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)
8. DOCKER_REGISTRY_SERVER_URL = $(registry.url)
9. DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)

<img src="media/VSTS/environment-variables14.png">

7. <strong>[OK]</strong> をクリックして続行します。

## <a name="setup-continious-deployment--deploy-java-application"></a>継続的配置を設定し、Java アプリケーションを配置する

1. 継続的配置を有効にするには、**[パイプライン]** タブをクリックします。
2. [成果物] セクションで稲妻アイコンをクリックします。  次に、**[継続的配置トリガー]** を [有効] に設定します。

<img src="media/VSTS/release-enable-CD.png">

3. <strong>[保存]</strong> をクリックし、<strong>[OK]</strong> をクリックします。 
4. <strong>[+ リリース]</strong> ドロップダウン メニューをクリックし、<strong>[Create a release]\(リリースの作成\)</strong> リンクを選択します。
5. <strong>[Stages for a trigger change from automated to manual]\(トリガーを自動から手動に変更するステージ\)</strong> ドロップダウン メニューで、目的のステージ名のチェック ボックスをオンにします。
6. <strong>[作成]</strong> をクリックして続行します。
7. リリース番号をクリックします。  次に、ステージ名をポイントし、<strong>[Deploy]\(配置\)</strong> をクリックします。
8. ポップアップ ウィンドウの <strong>[Deploy]\(配置\)</strong> をクリックして、Azure へのデプロイ プロセスを開始します。


## <a name="test-the-java-web-application"></a>Java Web アプリケーションをテストする
1. Web ブラウザーで Web アプリの URL を実行します。  
   https://{your-app-service-name}.azurewebsites.net/api/hello


<img src="media/VSTS/web-app16.png">

2. Web ページに "**Hello Azure!**" と表示されます。

<img src="media/VSTS/web-api17.png">
