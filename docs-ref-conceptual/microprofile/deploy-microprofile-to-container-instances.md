---
title: Docker と Azure を使用して MicroProfile アプリをクラウドにデプロイする
description: Docker および Azure Container Instances を使用して MicroProfile アプリをクラウドにデプロイする方法について説明します。
services: container-instances;container-retistry
documentationcenter: java
author: brborges
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: brborges
ms.date: 07/30/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c6254d11ee1596a23076931c9a2a2370b5f52409
ms.sourcegitcommit: 3d0896f821907278547c283c54b53fbd7f4f30f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43153847"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a>Docker と Azure を使用して MicroProfile アプリケーションをクラウドにデプロイする

この記事では、[MicroProfile.io] アプリケーションを Docker コンテナーにパックし、Azure Container Instances で実行する方法について説明します。

> [!NOTE]
>
> この手順は、Docker コンテナー イメージが自己実行可能である場合 (つまり、ランタイムが含まれる場合) に、任意の MicroProfile.io の実装で機能します。

## <a name="prerequisites"></a>前提条件

このチュートリアルの手順を実行するには、次の前提条件を満たしておく必要があります。

* Azure サブスクリプション: Azure サブスクリプションをまだ取得していない場合は、[無料の Azure アカウント]にサインアップできます。
* [Azure コマンド ライン インターフェイス (CLI)]。
* 最新の [Java Development Kit (JDK)] (バージョン 1.8 以降)。
* Apache の [Maven] ビルド ツール (バージョン 3 以上)。
* [Git] クライアント。

## <a name="microprofile-hello-azure-sample"></a>MicroProfile Hello Azure サンプル

この記事では、[MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) サンプルを使用します。

### <a name="clone-build-and-run-locally"></a>ローカルで複製、ビルドし、および実行する

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

アプリケーションをテストするには、`curl` を呼び出すか、[ブラウザー](http://localhost:8080/api/hello)を使用してアクセスします。

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a>[Deploy to Azure (Azure へのデプロイ)]

ここで [Azure Container Instances] および [Azure Container Registry] サービスを使用して、このアプリケーションをクラウドに移行しましょう。

### <a name="build-a-docker-image"></a>Docker イメージをビルドする

サンプル プロジェクトには、使用できる Docker ファイルが既に用意されています。 ただし、クラウドでのイメージのビルドには Azure Container Registry Build 機能を使用するため、Docker をインストールする必要はありません。

イメージをビルドして Azure で実行できるようにするには、次の手順に従う必要があります。

1. Azure CLI でインストールおよびログインする
1. Azure リソース グループを作成する
1. Azure Container Registry (ACR) を作成する
1. Docker イメージを構築する
1. 前に作成した ACR に Docker イメージを発行する
1. (省略可能) 1 つのコマンドでビルドして ACR に発行する


#### <a name="set-up-azure-cli"></a>Azure CLI をセットアップする

Azure のサブスクリプションを所有していること、[Azure CLI がインストールされている](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)こと、およびお使いのアカウントにご自身が認証されていることを確認します。

```bash
az login
```

#### <a name="create-a-resource-group"></a>リソース グループを作成します

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a>Azure Container Registry インスタンスの作成

このコマンドによって、ランダムな数値を持つ基本的な名前を使用して、(うまくいけば) グローバルに一意のコンテナー レジストリが作成されます。

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>Docker イメージを構築する

Docker 自体を使用して Docker イメージをローカルで簡単にビルドできますが、クラウドでのビルドを検討する理由はいくつかあります。

1. Docker をローカルにインストールする必要がない
1. ビルドが別の場所で行われるため、はるかに高速 (コンテキストのアップロード時間を除く)
1. クラウドのプロセスでより高速なインターネットにアクセスできるため、ダウンロードがさらに速くなる
1. イメージが Container Registry に直接移動する

これらの理由により、[Azure Container Registry Build] 機能を使用してイメージをビルドします。

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a>Docker イメージを Azure Container Registry (ACR) から Azure Container Instances (ACI) にデプロイする

ACR でイメージを使用できるようになったので、次はコンテナー インスタンスを ACI にプッシュしてインスタンス化してみましょう。 ただし、最初に、ACR に認証できることを確認する必要があります。

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>デプロイされた MicroProfile アプリケーションをテストする

これでご自身のアプリケーションが起動されて実行されます。 コマンド ラインからこれをテストするには、次のコマンドを実行してみます。

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

お疲れさまでした。 MicroProfile アプリケーションを Docker コンテナーとして適切にビルドし、Microsoft Azure にデプロイできました。

## <a name="next-steps"></a>次の手順

この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。

* [Azure CLI から Azure へのログイン](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure Container Registry Build]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
