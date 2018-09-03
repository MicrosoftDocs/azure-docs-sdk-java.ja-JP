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
ms.openlocfilehash: fa93788f9b600d963f35ea599a58d309d3a3ee52
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240825"
---
# <a name="configure-microprofile-with-azure-key-vault"></a>Azure Key Vault を使用した MicroProfile の構成

このチュートリアルでは、[MicroProfile 構成 API](https://microprofile.io/project/eclipse/microprofile-config) を使用して [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) からシークレットを取得して Azure Key Vault に直接接続を作成するように、[MicroProfile](http://microprofile.io) アプリケーションを構成する方法について説明します。 MicroProfile 構成 API を使用すると、構成データを取得し、これをご自分のマイクロサービスに挿入する際に、開発者が標準 API のメリットを活かすことができます。

詳しく説明する前に、Azure Key Vault と MicroProfile 構成 API を組み合わせることで、コードで何を記述できるかを簡単に確認しましょう。 `@Inject` および `@ConfigProperty` の注釈が付いている、クラス内のフィールドのコード スニペットを次に示します。 注釈で指定されている `name` は Azure Key Vault で検索するプロパティの名前で、キーが検出されない場合は、`defaultValue` が設定されます。 最終的には、Azure Key Vault に格納されている値、または既定値が実行時にフィールドに自動的に挿入され、開発者の作業が簡略化されます。開発者がコンストラクターおよびセッター メソッドで値を渡す必要がなく、MicroProfile にこの処理を任すことができるからです。

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

MicroProfile 構成に直接アクセスし、必要に応じてシークレットを要求することもできます。次に例を示します。

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

このサンプルでは、[Payara Micro](https://www.payara.fish/payara_micro) と [MicroProfile](https://microprofile.io/) を使用して、お使いのマシンでローカルに実行できる小さな Java war ファイルを作成します。 ここでは、コードを Docker 化したり Azure にプッシュしたりする方法については説明しませんが、このチュートリアルの最後にあるリンク セクションには、これについて説明した他の便利なチュートリアルへのリンクが記載されています。

このサンプルでは、無料のオープン ソース ライブラリが使用されています。このライブラリによって、Azure Key Vault 用の構成ソースが (MicroProfile 構成 API を使用して) 作成されます。 [プロジェクト GitHub ページ](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)では、このライブラリの詳細とコードを確認できます。 このライブラリを使用することで、このチュートリアルのコードの焦点を、ライブラリの構成だけに絞ることができます。その後、ご自身のコードにキーを挿入します。Azure 固有のコードを記述する必要はありません。

ここでは、お使いのローカル コンピューターで、このコードを実行するのに必要な手順を示します。最初は、Azure Key Vault リソースを作成します。

## <a name="creating-an-azure-key-vault-resource"></a>Azure Key Vault リソースの作成

Azure CLI を使用して Azure Key Vault リソースを作成し、1 つのシークレットを設定します。

1. まず、Azure サービス プリンシパルを作成しましょう。 これにより、Key Vault へのアクセスに必要なクライアント ID とキーが提供されます。

```bash
az login
az account set --subscription <subscription_id>

az ad sp create-for-rbac --name <service_principal_name>
```

たとえば、前の手順でサービス プリンシパル名として `microprofile-keyvault-service-principal` を使用したとします。 これを行うと、Azure から次のようなフォームで応答があります (一部伏せ字になっています)。

```json
{
  "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
  "displayName": "microprofile-keyvault-service-principal",
  "name": "http://microprofile-keyvault-service-principal",
  "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
  "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
}
```

ここで注目するのは `appID` と `password` の値です。これらはそれぞれ、`client ID` および `key` として後で使用します。

サービス プリンシパルを作成したので、次は必要に応じてリソース グループを作成します (作成済みの場合、この手順はスキップできます)。 リソース グループの場所を取得するために、`az account list-locations` を呼び出し、そのリストの `name` 値を使用して、リソース グループの作成先を指定できることに注意してください。

```bash
# For this tutorial, the author chose to use `westus`
# and `jg-test` for the resource group name.
az group create -l <resource_group_location> -n <resource_group_name>
```

ここで、Azure Key Vault リソースを作成します。 Key Vault 名は、後でキー コンテナーを参照するときに使用する名前です。したがって、覚えやすいものを選択してください。

```bash
az keyvault create --name <your_keyvault_name>            \
                   --resource-group <your_resource_group> \
                   --location <location>                  \
                   --enabled-for-deployment true          \
                   --enabled-for-disk-encryption true     \
                   --enabled-for-template-deployment true \
                   --sku standard
```

また、Key Vault シークレットにアクセスできるように、前に作成したサービス プリンシパルに適切なアクセス許可を付与する必要もあります。 appID 値は、サービス プリンシパルを作成した上記の場所の `appId` 値です (つまり、`5292398e-XXXX-40ce-XXXX-d49fXXXX9e79`。ただし、ターミナル出力の値を使用)。

```bash
az keyvault set-policy --name <your_keyvault_name>   \
                       --secret-permission get list  \
                       --spn <your_sp_appId_created_in_step1>
```

これでシークレットを Key Vault にプッシュできます。 キー名 `demo-key` を使用して、そのキーの値を `demo-value` に設定します。

```bash
az keyvault secret set --name demo-key      \
                       --value demo-value   \
                       --vault-name <your_keyvault_name>  
```

これで完了です。 現在、1 つのシークレットを使用して Azure で Key Vault が実行されています。 これで、このリポジトリを複製し、アプリでこのリソースを使用するように構成できます。

## <a name="getting-up-and-running-locally"></a>ローカルでの起動と実行

この例は、GitHub で入手できるサンプル アプリケーションに基づいています。そこで、この例を複製して、コードをステップ実行します。 お使いのマシンにコードを複製するには、次の手順に従ってください。

1. `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

1. `cd microprofile-configsource-keyvault`

1. `src/main/resources/META-INF/microprofile-config.properties` に移動し、上記の詳細を含む microprofile config.properties ファイルでプロパティを変更します。

1. `mvn clean package payara-micro:start` を使用して、サーバーを実行してみます

1. ご自身の Web ブラウザーで [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) にアクセスしてみます。Azure Key Vault から読み取られた値を示すシンプルな応答が表示されます。

## <a name="summary"></a>まとめ

このサンプル アプリケーションでは、MicroProfile 構成 API、Azure Key Vault、および無料のオープン ソース [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) ライブラリを一緒に処理して、構成データとシークレットを MicroProfile Web サービスに簡単に挿入できるようにします。
