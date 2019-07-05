---
title: Docker と Azure を使用して MicroProfile アプリをクラウドにデプロイする
description: Docker および Azure Container Instances を使用して MicroProfile アプリをクラウドにデプロイする方法について説明します。
services: container-instances;container-registry
documentationcenter: java
author: brunoborges
manager: routlaw
editor: brunoborges
ms.assetid: ''
ms.author: brborges
ms.date: 11/21/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 6ba12bb183969103676fa988199603df6cf36bba
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533604"
---
# <a name="deploy-a-microprofile-app-to-the-cloud-by-using-docker-and-azure"></a>Docker と Azure を使用して MicroProfile アプリをクラウドにデプロイする

この記事では、[MicroProfile.io] アプリケーションを Docker コンテナーにパックし、Azure Container Instances で実行する方法について説明します。

> [!NOTE]
> この手順は、Docker コンテナー イメージが自己実行可能である (つまり、イメージにランタイムが含まれている) 限り、任意の MicroProfile.io の実装で機能します。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、次の前提条件を用意しておく必要があります。

* Azure サブスクリプション。 Azure サブスクリプションをまだ取得していない場合は、[無料の Azure アカウント]にサインアップすることができます。
* [Azure CLI] がインストールされていること。
* サポートされている Java Development Kit (JDK)。 Azure で開発するときに使用可能な JDK の詳細については、「[Azure および Azure Stack の Java 長期サポート](https://aka.ms/azure-jdks)」を参照してください。
* [Apache Maven] ビルド ツール (バージョン 3 以降)。
* [Git] クライアント。

## <a name="microprofile-hello-azure-sample"></a>MicroProfile Hello Azure サンプル

この記事では、[MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) サンプルを使用します。 それを次のコマンドを使用して複製、ビルドしてローカルに実行します。

```bash
$ git clone https://github.com/Azure-Samples/microprofile-hello-azure.git
$ mvn clean package
...
[INFO] BUILD SUCCESS
...
$ mvn payara-micro:start
...
[2018-07-30T13:34:51.553-0700] [] [INFO] [] [PayaraMicro] [tid: _ThreadID=1 _ThreadName=main] [timeMillis: 1532982891553] [levelValue: 800] Payara Micro  5.182 #badassmicrofish (build 303) ready in 10,304 (ms)
...
```

`curl` を呼び出すか、[ブラウザー](http://localhost:8080/api/hello)を使用してアプリケーションをテストできます。

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-the-app-to-azure"></a>Azure にアプリケーションをデプロイする

ここで [Azure Container Instances] および [Azure Container Registry] サービスを使用して、このアプリケーションを Azure に移行しましょう。

### <a name="build-a-docker-image"></a>Docker イメージをビルドする

サンプル プロジェクトには、使用できる Dockerfile が用意されています。 ただし、クラウドでのイメージのビルドには Azure Container Registry Build 機能を使用するため、Docker をインストールする必要はありません。

イメージをビルドしてそれを Azure で実行するための準備をするには、次の操作を行います。

1. Azure CLI をインストールしてサインインします。
1. Azure リソース グループを作成します。
1. Azure コンテナー レジストリ インスタンスを作成します。
1. Docker イメージをビルドします。
1. Docker イメージを以前に作成したコンテナー レジストリ インスタンスに発行します。
1. (省略可能) 1 つのコマンドでイメージをビルドして、コンテナー レジストリ インスタンスに発行します。


#### <a name="set-up-the-azure-cli"></a>Azure CLI のセットアップ

Azure のサブスクリプションを所有していること、[Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) がインストールされていること、お使いのアカウントでご自身が認証されていることを確認します。

```bash
az login
```

#### <a name="create-a-resource-group"></a>リソース グループの作成

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-a-container-registry-instance"></a>コンテナー レジストリ インスタンスの作成

このコマンドによって、基本的な名前とランダムな数値を使用して、グローバルに一意のコンテナー レジストリが作成されます。

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>Docker イメージを構築する

Docker イメージは、Docker そのものを使用してローカルで簡単にビルドできますが、クラウドでのビルドを検討する理由はいくつかあります。

* Docker をローカルにインストールする必要がない。
* ビルドが別の場所で行われるため、はるかに高速 (コンテキストのアップロード時間を除く)。
* クラウド内のプロセスはより高速にインターネットにアクセスできるため、ダウンロードがさらに速くなる。
* イメージがコンテナー レジストリ インスタンスに直接移動する。

これらの理由により、[Azure Container Registry Build] 機能を使用してイメージをビルドします。

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-the-docker-image-from-the-azure-container-registry-instance-to-container-instances"></a>Docker イメージを Azure コンテナー レジストリ インスタンスから Container Instances にデプロイする

これで、イメージがお使いのコンテナー レジストリ インスタンスで使用できるようになったので、Container Instances でコンテナー インスタンスをプッシュしてインスタンス化します。 しかし、まずコンテナー レジストリ インスタンスに認証できることを確認します。

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>デプロイされた MicroProfile アプリケーションのテスト

これでご自身のアプリケーションが起動されて実行されます。 コマンド ライン インターフェイスからこれをテストするには、次のコマンドを使用します。

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

お疲れさまでした。 MicroProfile アプリケーションを Docker コンテナーとして適切にビルドし、Azure にデプロイできました。

## <a name="next-steps"></a>次の手順

この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。

* [Azure CLI を使用して Azure にサインインする](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure CLI]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Apache Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry
