---
title: Azure Key Vault 用の Spring Boot Starter の使用方法
description: Azure Key Vault スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 78b7a9a2e26168b19dc8a1d12e47456752b57ffc
ms.sourcegitcommit: e017de4677c5bedd6ef88c8c1b6da279dc973efe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2018
ms.locfileid: "45639775"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>Azure Key Vault 用の Spring Boot Starter の使用方法

## <a name="overview"></a>概要

この記事では、Azure Key Vault 用 Spring Boot Starter を使用した **[Spring Initializr]** を用いて、キー コンテナーにシークレットとして格納されている接続文字列を取得するアプリを作成する方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-an-app-using-the-spring-initialzr"></a>Spring Initialzr を使用したアプリの作成

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。

   ![グループとアーティファクトの名前を指定する][secrets-01]

1. 下へスクロールして **[Azure]** セクションを表示し、**[Azure Key Vault]** チェック ボックスをオンにします。

   ![Azure Key Vault スターターを選択する][secrets-02]

1. ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。

   ![Spring Boot プロジェクトを生成する][secrets-03]

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a>Azure へのサインインと使用するサブスクリプションの選択

1. コマンド プロンプトを開きます。

1. Azure CLI を使って、Azure アカウントにサインインします。

   ```azurecli
   az login
   ```
   指示に従って、サインインを完了します。

1. サブスクリプションを一覧表示します。

   ```azurecli
   az account list
   ```
   Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. Azure で使用するアカウントの GUID を指定します。例を次に示します。

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a>Azure CLI を使用した新しい Azure Key Vault の作成と構成

1. キー コンテナーに使用する Azure リソースのリソース グループを作成します。次に例を示します。
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | リソース グループの一意の名前を指定します。 |
   | `location` | リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |

   次のように、Azure CLI に、リソース グループ作成の結果が表示されます。  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. アプリケーション登録から Azure サービス プリンシパルを作成します。次に例を示します。
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | Azure サービス プリンシパルの名前を指定します。 |

   Azure CLI から JSON ステータス メッセージが返されます。このメッセージに含まれている *appId* と *password* は、後でクライアント ID およびクライアント パスワードとして使用します。次に例を示します。

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. リソース グループに新しいキー コンテナーを作成します。次に例を示します。
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | キー コンテナーの一意の名前を指定します。 |
   | `location` | リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |
   | `enabled-for-deployment` | [キー コンテナーのデプロイ オプション](https://docs.microsoft.com/cli/azure/keyvault)を指定します。 |
   | `enabled-for-disk-encryption` | [キー コンテナーの暗号化オプション](https://docs.microsoft.com/cli/azure/keyvault)を指定します。 |
   | `enabled-for-template-deployment` | [キー コンテナーの暗号化オプション](https://docs.microsoft.com/cli/azure/keyvault)を指定します。 |
   | `sku` | [キー コンテナーの SKU オプション](https://docs.microsoft.com/cli/azure/keyvault)を指定します。 |
   | `query` | 応答から取得する値を指定します。これは、このチュートリアルを完了するために必要なキー コンテナーの URI です。 |

   Azure CLI にキー コンテナーの URI が表示されます。この URI は後で使用します。次に例を示します。  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

4. 前の手順で作成した Azure サービス プリンシパルのアクセス ポリシーを設定します。次に例を示します。
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | 前の手順で作成したキー コンテナーの名前を指定します。 |
   | `secret-permission` | キー コンテナーの[セキュリティ ポリシー](https://docs.microsoft.com/cli/azure/keyvault)を指定します。 |
   | `spn` | アプリケーションの登録時に取得した GUID を指定します。 |

   Azure CLI にセキュリティ ポリシー作成の結果が表示されます。次に例を示します。  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "wingtiptoyskeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "wingtiptoysresources",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

5. 新しいキー コンテナーにシークレットを格納します。次に例を示します。
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `vault-name` | 前の手順で作成したキー コンテナーの名前を指定します。 |
   | `name` | シークレットの名前を指定します。 |
   | `value` | シークレットの値を指定します。 |

   Azure CLI にシークレット作成の結果が表示されます。次に例を示します。  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://wingtiptoyskeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://wingtiptoys.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a>Spring Boot アプリケーションの構成とコンパイル

1. 以前にディレクトリにダウンロードした Spring Boot プロジェクトのパッケージ ファイルからファイルを抽出します。

2. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。

3. このチュートリアルで既に完了した手順で取得した値を使用して、キー コンテナーの値を追加します。次に例を示します。
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   各値の説明:

   |          パラメーター          |                                 説明                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           キー コンテナーの作成時に取得した URI を指定します。           |
   | `azure.keyvault.client-id`  |  サービス プリンシパルの作成時に取得した *appId* GUID を指定します。   |
   | `azure.keyvault.client-key` | サービス プリンシパルの作成時に取得した *password* GUID を指定します。 |


4. プロジェクトのメイン ソース コード ファイルに移動します (例: */src/main/java/com/wingtiptoys/secrets*)。

5. テキスト エディターでアプリケーションのメイン Java ファイル (例: *SecretsApplication.java*) を開き、ファイルに次の行を追加します。

   ```java
   package com.wingtiptoys.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   このコード例では、キー コンテナーから接続文字列を取得し、コマンド ラインに表示します。

6. Java ファイルを保存して閉じます。

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. Spring Boot アプリの *pom.xml* ファイルがあるディレクトリに移動します。

1. Spring Boot アプリケーションを Maven でビルドします。次に例を示します。

   ```bash
   mvn clean package
   ```

   Maven にビルドの結果が表示されます。

   ![Spring Boot アプリケーションのビルドの状態][build-application-01]

1. Maven で Spring Boot アプリケーションを実行します。アプリケーションによって、キー コンテナーから取得した接続文字列が表示されます。 For example:

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot の実行時メッセージ][build-application-02]

## <a name="next-steps"></a>次のステップ

Azure Key Vault の使用方法の詳細については、次の記事をご覧ください。

* [Key Vault のドキュメント]

* [Azure Key Vault の概要]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。

<!-- URL List -->

[Key Vault のドキュメント]: /azure/key-vault/
[Azure Key Vault の概要]: /azure/key-vault/key-vault-get-started
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
