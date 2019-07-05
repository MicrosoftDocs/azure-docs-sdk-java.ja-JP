---
title: Azure Key Vault を使用した MicroProfile の構成
description: Azure Key Vault を使用してシークレットを MicroProfile Web サービスに挿入する方法について説明します
services: key-vault
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c405711813176823f2ddee6b4002f75c2b25fdb5
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533611"
---
# <a name="configure-microprofile-by-using-azure-key-vault"></a>Azure Key Vault を使用した MicroProfile の構成

この記事では、[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) からシークレットを取得するように [MicroProfile](http://microprofile.io) アプリケーションを構成する方法を示します。 このプロセスでは、[MicroProfile 構成 API](https://microprofile.io/project/eclipse/microprofile-config) を使用して Azure Key Vault への直接接続を作成します。 MicroProfile 構成 API を使用すると、開発者は構成データを取得し、これを自分のマイクロサービスに挿入するための標準 API のメリットを活用することができます。

始める前に、Azure Key Vault と MicroProfile 構成 API を組み合わせることで、コードで何を記述できるかを簡単に確認しましょう。 次は、`@Inject` および `@ConfigProperty` の注釈が付いているクラス内のフィールドのコード スニペットです。 注釈で指定されている *name* はご自分のキー コンテナーで検索するプロパティの名前で、*defaultValue* はキーが検出されない場合に設定されます。 その結果、キー コンテナーに格納されている値 (既定値) が実行時に自動的にフィールドに挿入されます。 このアクションにより、コンストラクターと setter メソッドで値を渡す必要がなくなるので、作業が簡略化されます。 代わりに、MicroProfile によってこのタスクが処理されます。

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

必要に応じてシークレットを要求するため、MicroProfile 構成に直接アクセスすることもできます。 例:

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

このサンプル コードでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile](https://microprofile.io/) を使用して、お使いのマシンでローカルに実行できる小さな Java web アプリ要件 (WAR) ファイルを作成します。 ここでは、コードを Docker 化したり Azure にプッシュしたりする方法については説明しませんが、この記事の最後にあるセクションには、これについて説明した他の便利なチュートリアルへのリンクが記載されています。

このサンプルでは、ご使用のキー コンテナー内に (MicroProfile 構成 API を使用して) 構成ソースを作成する無料のオープン ソース ライブラリが使用されています。 このライブラリの詳細とコードを確認するには、[プロジェクト GitHub ページ](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)を参照してください。 ライブラリを使用する場合、このチュートリアルのコードを使用すると、ライブラリの構成だけに集中して、その後でキーを自分のコードに挿入することができます。 Azure 固有のコードを記述する必要はありません。

ご使用のローカル コンピューターでこのコードを実行するには、次のセクションの手順に従って、キー コンテナー リソースの作成を始めます。

## <a name="create-a-key-vault-resource"></a>キー コンテナー リソースを作成する

このセクションでは、Azure CLI を使用してキー コンテナー リソースを作成し、1 つのシークレットを設定します。

1. Azure サービス プリンシパルを作成します。 このステップにより、ご自分のキー コンテナーにアクセスするのに必要なクライアント ID とキーが提供されます。

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    *microprofile-keyvault-service-principal* を使用して、前のステップの *\<service_principal_name>* を置き換えてみましょう。 Azure からの応答は次のようになります。

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    ここでは注目すべきは、*appId* と *password* の値です。 これらは、この記事で後ほど*クライアント ID*と*キー*として使用します。

1. (省略可能) サービス プリンシパルを作成したので、リソース グループを作成することができます。 使用するリソース グループが既にある場合は、このステップをスキップできます。 リソース グループの場所のリストを取得するには、`az account list-locations` を呼び出し、そのリストの *name* 値を使用して、リソース グループの作成先を指定できます。

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. キー コンテナー リソースを作成します。 キー コンテナー名は後でそのキー コンテナーを参照するのに使用するため、覚えやすい名前にするようにしてください。

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. 以前に作成したサービス プリンシパルに適切なアクセス許可を付与して、キー コンテナー シークレットにアクセスできるようにします。 次のコードの appId 値は、サービス プリンシパルを作成したステップ 1 の*appId* 値です。 つまり、ステップ 1 の *appId* は *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79* でしたが、ご自分のターミナルの出力からの値を使用する必要があります。

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. これで、キー コンテナーにシークレットをプッシュできます。 キー名 *demo-key* を使用して、そのキーの値を *demo-value* に設定します。

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

これで完了です。 現在、1 つのシークレットを使用して Azure でキー コンテナーが実行されています。 これで、このリポジトリを複製し、アプリでこのリソースを使用するように構成できます。

## <a name="get-up-and-running-locally"></a>ローカルで起動して実行する

この例は、GitHub で入手できるサンプル アプリケーションに基づいていますので、そのアプリケーションを複製し、そのコードを順番に見ていきます。 

1. ご使用のマシンにコードを複製するには、次のコマンドを入力します。

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. *src/main/resources/META-INF/microprofile-config.properties* に移動して、*microprofile-config.properties* ファイル内のプロパティを前のステップからの値を使用して変更します。

1. `mvn clean package payara-micro:start` を使用して、サーバーを実行してみます。

1. ご使用の Web ブラウザーで [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) にアクセスしてみます。 キー コンテナーから読み取られた値を示すシンプルな応答が表示されます。

## <a name="summary"></a>まとめ

このサンプル アプリケーションでは、MicroProfile 構成 API、Azure Key Vault、無料のオープン ソース [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) ライブラリを結合して、構成データとシークレットを MicroProfile Web サービスに簡単に挿入できるようにします。
