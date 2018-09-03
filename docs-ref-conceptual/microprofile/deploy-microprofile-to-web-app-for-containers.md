---
title: Java ベースの MicroProfile サービスを Web App for Containers にデプロイする
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
ms.openlocfilehash: 323ae247c3df8c7d7b180d9d60b9014e4e2d7382
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240962"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>Java ベースの MicroProfile サービスを Web App for Containers にデプロイする

[Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/) などのサービスにすばやく簡単にデプロイできる、非常に小さな Java アプリケーションを構築するには、MicroProfile が最適です。 このチュートリアルでは、シンプルな MicroProfile ベースのマイクロサービスを作成します。このマイクロサービスは Docker コンテナーにコンテナー化され、[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) にデプロイされた後、Azure Web App for Containers を使ってホストされます。

> [!NOTE]
>
> この手順は、Docker コンテナー イメージが自己実行可能である場合 (つまり、ランタイムが含まれる場合) に、任意の MicroProfile.io の実装で機能します。

具体的には、このサンプルでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile 1.3](https://microprofile.io/) を使用して、小さな Java WAR ファイル (作成者マシン上では 5,085 バイト) を作成し、Docker イメージ (約 174 メガバイト) にパッケージ化します。 この Docker イメージには、この Web アプリの完全にコンテナー化されたデプロイに必要なすべてのものが含まれています。

Docker のしくみのせいで、多くの場合、アプリケーションのソース コードが変更されるたびに、174 メガバイトの Docker イメージ全体を再デプロイする必要はありません。これは、Docker によって (大幅に小さな) 差分のみがアップロードされるためです。 これにより、CI/CD パイプライン経由での新しい MicroProfile アプリケーション リリースの実行プロセスが非常に効率的かつ迅速になり、煩雑さが軽減され、開発の反復処理の高速化が実現します。

このチュートリアルでは、最初にローカルでコードを作成および実行し、その後、そのコードを Azure で Web アプリとしてデプロイします。 いずれの場合も、Docker を使用して、作業を簡略化および標準化します。 まずは、Docker コンテナーを格納する Azure Container Registry を作成します。

## <a name="creating-an-azure-container-registry"></a>Azure Container Registry の作成

Azure Container Registry の作成には [Azure portal](http://portal.azure.com) を使用しますが、Azure CLI などの代替手段があることに注意してください。 新しい Azure Container Registry を作成するには、次の手順に従ってください。

1. [Azure portal](http://portal.azure.com) にログインし、新しい Azure Container Registry リソースを作成します。 レジストリの名前を指定します (これは `pom.xml` で `docker.registry` プロパティとして設定されている名前です)。 必要に応じて既定値を変更し、[作成] をクリックします。

1. コンテナー レジストリがライブになったら ([作成] をクリックしてから約 30 秒かかります)、コンテナー レジストリをクリックし、左側のメニュー領域にある [アクセス キー] リンクをクリックします。 ここで、"管理者ユーザー" 設定を有効にする必要があります。これにより、(Docker コンテナーがプッシュされた) このコンテナー レジストリに、お使いのマシンからアクセスできるようになります。また、この後に設定する Azure Web App for Containers インスタンスからのアクセスも有効にします。

1. [アクセス キー] 領域では、`username` 値と `password` 値に気を付けてください。 これらをコピーして、グローバル Maven `settings.xml` ファイルに貼り付けます (Maven 設定の詳細については、[Apache Maven プロジェクト](https://maven.apache.org/settings.html)の Web サイトをご覧ください)。 参照用として、この作成者システムには `${user.home}/.m2/settings.xml` ファイルの難読化バージョンがあります。

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

これが完了したら、次は MicroProfile アプリケーションをローカルで構築して実行します。

## <a name="creating-our-microprofile-application"></a>MicroProfile アプリケーションの作成

この例は、GitHub で入手できるサンプル アプリケーションに基づいています。そこで、この例を複製して、コードをステップ実行します。 お使いのマシンにコードを複製するには、次の手順に従ってください。

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

このディレクトリには `pom.xml` ファイルがあります。このファイルは、Maven ビルド ツールで使用される形式でプロジェクトを指定するときに使用されます。 このファイルは、独自のニーズに合わせて編集できます。 具体的には、`docker.registry` プロパティと `docker.name` プロパティは、Azure Container Registry の設定時に作成された `docker.registry` と `docker.name` に変更する必要があります。

また、このディレクトリの Dockerfile にも注目します。これを以下に示します。

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

この Dockerfile では、Payara Micro Docker コンテナーに基づいて新しい Docker コンテナーが作成され、ビルド プロセスの一環として作成された .war ファイルにコピーされるだけです。 また、Docker コンテナーで実行された時点でサービスにアクセスできるように、ポート 8080 が公開されます。

`src` ディレクトリを詳しく見ていくと、最終的には以下のように `Application` クラスが見つかります。

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

`@ApplicationPath("/api")` 注釈により、このマイクロサービス用の基本エンドポイントが指定されます。つまり、すべてのエンドポイントで、特定の REST エンドポイントへのアクセスに必要な残りの URL の前に `/api` が付きます。

`api` パッケージ内には `API` という名前のクラスがあり、次のコードが含まれています。

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

`@Path("/helloworld")` 注釈を使用すると、この REST エンドポイントは、`Application` クラスで指定された `/api` と組み合わされた場合に、`/api/helloworld` になることがわかります。 このエンドポイントが HTTP GET 要求を使用して呼び出されると、メソッドによって text/html が生成されることを確認できますが、実際のところ、これは単にハード コーディングされた文字列 "Hello, world!" にすぎません。

MicroProfile を使用してマイクロサービスを作成するのに必要なコードについては、すべて説明しました。 これで Maven を使用して作成し、Docker コンテナーにコンテナー化して、ローカルで実行することができます。 これを行うには、次の手順を使用します。

1. `mvn clean package` を実行し、正常に完了するまで待ちます。

1. `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest` を実行します。たとえば、`docker.registry` が `jogilescr.azurecr.io`、`docker.name` が `samples/docker-helloworld` の場合は `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest` となります。

1. Web ブラウザーで [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) および [http://localhost:8080/health](http://localhost:8080/health) にアクセスしてみます。 想定どおりの "Hello, world!"  応答 (および [/health](http://localhost:8080/health) エンドポイントの正常性に関する情報) が表示された場合、MicroProfile アプリケーションはローカル コンピューターに正常に展開されています。

## <a name="pushing-to-the-azure-container-registry"></a>Azure Container Registry へのプッシュ

MicroProfile アプリケーションが正常に作成され、ローカル コンピューターで実行されました。次は、このコンテナーをコンテナー レジストリにプッシュします。 このチュートリアルでは Azure Container Registry を使用していますが、(関連する場所をポイントするように `pom.xml` ファイルを編集できれば) 任意のコンテナー レジストリを使用できます。

1. `mvn clean package` を実行して、ローカル Docker イメージをクリーニング、コンパイル、および作成します。
2. `mvn dockerfile:push` を実行して、Azure Container Repository にプッシュします。

この時点で、ご自身の Docker コンテナー イメージは Azure Container Registry にアップロードされていますが、Azure Web App for Containers インスタンスにデプロイする必要があるため、まだ実行されていません。 次は、この操作を行います。

## <a name="creating-an-azure-web-app-for-containers-instance"></a>Web App for Containers インスタンスの作成

1. [Azure portal](http://portal.azure.com) に戻り、(メニューの [Web + モバイル] 見出しの下で) 新しい Web App for Containers インスタンスを作成します。 ポイントがいくつかあります。

   1. ここで指定する名前は Web アプリのパブリック URL になります (ただし、必要に応じて、後でカスタム ドメインを追加できます)。このため、覚えやすい名前を選択することをお勧めします。

   1. [コンテナーの構成] セクションに移動したら、[イメージ ソース] として [Azure Container Registry] を選択し、ドロップダウン リストから適切なイメージを選択できます。

   1. [スタートアップ ファイル] フィールドに値を指定する必要はありません。

1. インスタンスが作成されたら (ここでも非常に迅速です)、そのインスタンスをクリックし、[アプリケーション設定] メニュー項目をクリックします。 ここでは、新しいアプリケーション設定を追加する必要があります。ここで、キーは `WEBSITES_PORT`、値は `8080` です。 これにより、コンテナーで公開するポートが Azure に通知され、外部的にはポート 80 にマップされます。

1. 必要に応じて、[Docker コンテナー] リンクをクリックし、[継続的なデプロイ] を有効にします。これにより、Azure Container Registry イメージを更新するたびに、Azure Web App for Containers インスタンスでも自動的に更新されるようになります。

1. これで、Azure でホストされるインスタンスに `http://<appname>.azurewebsites.net/microprofile/api/helloworld` および `http://<appname>.azurewebsites.net/health` でアクセスできます。

## <a name="summary"></a>まとめ

このチュートリアルでは、シンプルな MicroProfile ベースのマイクロサービスを作成し、Docker コンテナーにコンテナー化した後、ローカルで実行し、Azure に発行するプロセスについて説明しました。 マイクロサービスを拡張して、より便利な機能を提供する方法については、このチュートリアルの範囲外ですが、インターネット上には MicroProfile に関するチュートリアルとアドバイスが豊富に用意されています。[MicroProfile.io](https://microprofile.io/) の詳細が記載されたコンテンツをご覧になることをお勧めします。