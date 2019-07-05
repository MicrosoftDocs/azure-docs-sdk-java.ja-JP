---
title: Java ベースの MicroProfile サービスを Azure Web App for Containers にデプロイする
description: Docker および Azure Web App for Containers を使用して MicroProfile サービスをデプロイする方法について説明します
services: container-registry;app-service
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: container-registry;app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 4ecfbf92bc30bc491c991e30111cce8375da7f1a
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533598"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>Java ベースの MicroProfile サービスを Azure Web App for Containers にデプロイする

[Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/) などのサービスにすばやく簡単にデプロイできる、小さな Java アプリケーションを構築するには、MicroProfile が最適です。 このチュートリアルでは、Docker コンテナーにコンテナー化するシンプルな MicroProfile ベースのマイクロサービスを作成し、[Azure コンテナー レジストリ インスタンス](https://azure.microsoft.com/services/container-registry/) にデプロイした後、Azure Web App for Containers を使ってホストします。

> [!NOTE]
> この手順は、Docker コンテナー イメージが自己実行可能である場合 (つまり、イメージにランタイムが含まれている場合) に、任意の MicroProfile.io の実装で機能します。

このサンプルでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile 1.3](https://microprofile.io/) を使用して、小規模な (約 5,000 バイト) Java web アプリ要件 (WAR) ファイルを作成し、それを約 174 メガバイト (MB) の Docker イメージにパッケージ化します。 この Docker イメージには、Web アプリの完全にコンテナー化されたデプロイに必要なすべてのものが含まれています。

アプリケーションのソース コードが変更されるたびに、174 MB の Docker イメージ全体を再デプロイする必要はありません。Docker によって (ずっと小さい) 差分のみがアップロードされるためです。 その結果、CI/CD パイプライン経由での新しい MicroProfile アプリケーション リリースの実行プロセスが非常に効率的かつ迅速になり、煩雑さが軽減され、開発の反復処理の高速化が実現します。

このチュートリアルでは、最初にローカルでコードを作成して実行し、その後、そのコードを Azure で Web アプリとしてデプロイします。 いずれのフェーズでも、Docker を使用して、作業を簡略化および標準化できます。 開始する前に、Docker コンテナーを格納するための Azure コンテナー レジストリ インスタンスを作成します。

## <a name="create-an-azure-container-registry-instance"></a>Azure コンテナー レジストリ インスタンスを作成する

Azure コンテナー レジストリ インスタンスは [Azure portal](http://portal.azure.com) を使用して作成できますが、Azure CLI など他の選択肢もあります。 新しい Azure コンテナー レジストリ インスタンスを作成するには、次の手順を行ってください。

1. [Azure portal](http://portal.azure.com) にサインインし、新しい Azure コンテナー レジストリ リソースを作成します。 レジストリの名前を指定します (この名前は *pom.xml* ファイルの `docker.registry` プロパティとして設定されています)。 必要に応じて既定値を変更し、 **[作成]** を選択します。

1. 約 30 秒で、コンテナー レジストリ インスタンスがライブになったら、コンテナー レジストリ インスタンスを選択し、左側のウィンドウで、 **[アクセス キー]** リンクを選択します。 

    お使いのコンピューターからこのコンテナー レジストリ インスタンスにアクセスして Docker コンテナーをプッシュできるように、*管理者ユーザー*設定が有効になっていることを確認してください。 そうすることで、この後設定する Azure Web App for Containers インスタンスからもアクセスできるようになります。

1. **[アクセス キー]** ウィンドウで、**ユーザー名**と**パスワード**の値をコピーして、グローバル Maven *settings.xml* ファイルに貼り付けます。 Maven 設定の詳細については、[Apache Maven Project](https://maven.apache.org/settings.html) の Web サイトをご覧ください。 

   参考のため、 *${user.home}/.m2/settings.xml* ファイルの例を次に示します。

    ```xml
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
          <server>
            <id>jogilescr.azurecr.io</id>
            <username>jogilescr</username>
            <password>ojoirshois.this-isn't-real.hrihslirhlishrglih</password>
          </server>
        </servers>
    </settings>
    ```

コンテナー レジストリ インスタンスを作成したので、MicroProfile アプリケーションのローカルでのビルドと実行に移ることができます。

## <a name="create-your-microprofile-application"></a>MicroProfile アプリケーションを作成する

このコード例は、GitHub で入手可能なサンプル アプリケーションに基づいています。 コンピューターにコードを複製するには、次のコマンドを入力します。

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

このディレクトリには、Maven ビルド ツールで使用されている形式でプロジェクトを指定する *pom.xml* ファイルが含まれています。 このファイルは各自のニーズに合わせて編集できます。 具体的には、`docker.registry` プロパティと `docker.name` プロパティを、Azure コンテナー レジストリ インスタンスの設定時に作成された `docker.registry` プロパティと `docker.name` プロパティに変更する必要があります。

また、このディレクトリの Dockerfile にも注目します。これを以下に示します。

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

Dockerfile によって、Payara Micro Docker コンテナーに基づく新しい Docker コンテナーが作成されます。 これはビルド プロセスの一部として作成された WAR ファイルでコピーされます。 また、サービスが Docker コンテナー内で実行されるようになったら、サービスにアクセスできるようにポート 8080 も公開されます。

*src* ディレクトリを開くときに、最終的にはここに再現されている `Application` クラスが見つかります。

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

*@ApplicationPath("/api")* 注釈は、マイクロサービスのベース エンドポイントを指定します。 つまり、すべてのエンドポイントに対し、特定の REST エンドポイントにアクセスするために必要な URL の残りの部分より前に */api* を配置します。

*api* パッケージ内には `API` という名前のクラスがあり、次のコードが含まれています。

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld.api;

import javax.enterprise.context.ApplicationScoped;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

import static javax.ws.rs.core.MediaType.TEXT_HTML;

@ApplicationScoped
@Path("/")
public class API {

    @GET
    @Path("/helloworld")
    @Produces(TEXT_HTML)
    public String info() {
        return "Hello, world!";
    }
}
```

*@Path("/helloworld")* 注釈を使用すると、この REST エンドポイントは、`Application` クラスで指定された */api* と組み合わされた場合に、 */api/helloworld* になることがわかります。 HTTP GET 要求を使用してこのエンドポイントを呼び出すと、メソッドによって text/html が生成されることを確認できますが、実際のところ、これは単にハード コーディングされた文字列 "Hello, world!" にすぎません。

ここまで、この記事では、MicroProfile を使用してマイクロサービスを作成するために必要なすべてのコードについて説明してきました。 これで Maven を使用してこれをビルドし、Docker コンテナーにコンテナー化して、次の操作を行ってローカルで実行することができます。

1. `mvn clean package` を実行し、完了するまで待機します。

1. `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest` を実行します。 たとえば、*docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest* の場合、 *\<docker.registry>* は *jogilescr.azurecr.io* で、 *\<docker.name>* は *samples/docker-helloworld* です。

1. Web ブラウザーで [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) および [http://localhost:8080/health](http://localhost:8080/health) にアクセスしてみます。 想定どおりの "Hello, world!" 応答 (および [/health](http://localhost:8080/health) エンドポイントの正常性に関する情報) が表示された場合、MicroProfile アプリケーションはローカル コンピューターに正常に展開されています。

## <a name="push-the-container-to-the-azure-container-registry-instance"></a>Azure コンテナー レジストリ インスタンスにコンテナーをプッシュする

ローカル コンピューターで MicroProfile アプリケーションを正常にビルドして実行したので、このコンテナーをご自分のコンテナー レジストリ インスタンスにプッシュします。 

> [!NOTE]
> この記事では、Azure コンテナー レジストリ インスタンスを使用していますが、関連する場所をポイントするように *pom.xml* ファイルを編集すれば、どのコンテナー レジストリ インスタンスでも機能します。

1. ローカル Docker イメージを消去、コンパイル、作成するには、`mvn clean package` を実行します。
2. コンテナーを Azure コンテナー レジストリ インスタンスにプッシュするには、`mvn dockerfile:push` を実行します。

これで Docker コンテナー イメージが Azure コンテナー イメージ インスタンスにアップロードされました。 しかし、まだ実行されていません。 今度はこれを Azure Web App for Containers インスタンスにデプロイする必要があります。 

## <a name="create-an-azure-web-app-for-containers-instance"></a>Web App for Containers インスタンスを作成する

1. [Azure portal](http://portal.azure.com) の左側のウィンドウで、 **[Web + モバイル]** を選択し、次の操作を行います。

   a. 名前を指定します。 この名前は Web アプリのパブリック URL になるため、覚えやすい名前を選択することをお勧めします。 必要に応じて、カスタム ドメイン名を後で追加することができます。

   b. **[コンテナーの構成]** セクションの **[イメージ ソース]** で、 **[Azure Container Registry]** を選択してから、ドロップダウン リストで適切なイメージを選択します。

   c. **[スタートアップ ファイル]** フィールドに値を指定する必要はありません。

1. インスタンスを作成したら、それを選択し、 **[アプリケーションの設定]** を選択します。 **[キー]** に「**WEBSITES_PORT**」と入力し、 **[値]** に「**8080**」と入力します。 これらの設定では、コンテナー内で公開するポートと、それを外部からポート 80 にマップすることが Azure に指示されます。

1. (省略可能) **[Docker コンテナー]** リンクを選択してから、 **[継続的なデプロイ]** を有効にします。 これにより、Azure コンテナー レジストリ インスタンスのイメージを更新するたびに Azure Web App for Containers インスタンスで自動的に更新されます。

1. これで、Azure でホストされるインスタンスに `http://<appname>.azurewebsites.net/microprofile/api/helloworld` および `http://<appname>.azurewebsites.net/health` でアクセスできます。

## <a name="summary"></a>まとめ

このチュートリアルでは、シンプルな MicroProfile ベースのマイクロサービスを作成し、それを Docker コンテナーにコンテナー化した後、ローカルで実行し、Azure に発行するプロセスについて説明しました。 その他の便利な機能を提供するようにマイクロサービスを拡張することができます。 詳細については、[MicroProfile.io](https://microprofile.io/) をご覧ください。
