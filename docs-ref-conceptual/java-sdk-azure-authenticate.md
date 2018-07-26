---
title: Azure Management Libraries for Java を使って認証する
description: Azure Management Libraries for Java への認証にサービス プリンシパルを使う方法について説明します。
keywords: Azure, Java, SDK, API, Maven, Gradle, 認証, active directory, サービス プリンシパル
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 10f457e3-578b-4655-8cd1-51339226ee7d
ms.openlocfilehash: 1d556955fcc5b73f1ba099a0b846b571ba64ccff
ms.sourcegitcommit: 107c3c5ed8c6991c751f95bcaf3757220940df9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050729"
---
# <a name="authenticate-with-the-azure-libraries-for-java"></a>Java 用 Azure ライブラリを使った認証 

## <a name="connect-to-services-with-connection-strings"></a>接続文字列を使ってサービスに接続する

ほとんどの Azure サービス ライブラリでは、接続文字列またはセキュリティ キーが認証に使用されます。 たとえば SQL Database では、JDBC 接続文字列にユーザー名とパスワードの情報が格納されます。

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

Azure Storage でのアプリケーションの承認には、ストレージ キーが使用されます。

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

他の Azure サービス ([Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService)、[Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started)、[Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues) など) に対する認証には、サービスの接続文字列が使用されます。 この接続文字列は、Azure Portal または CLI を使って取得できます。  また、コード内から Azure Management Libraries for Java を使用し、リソースを照会することによって接続文字列を作成することもできます。 

たとえば次のコードでは、管理ライブラリを使ってストレージ アカウントの接続文字列を作成しています。

```java
// create a new storage account
StorageAccount storage = azure.storageAccounts().getByResourceGroup("myResourceGroup","myStorageAccount");

// create a storage container to hold the file
List<StorageAccountKey> keys = storage.getKeys();
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storage.name()
        + ";AccountKey=" + keys.get(0).value()
        + ";EndpointSuffix=core.windows.net";
```

その他のライブラリでは、[サービス プリンシパル](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)を使ってアプリケーションを実行し、許可された資格情報で動作することをアプリケーションに承認する必要があります。 この構成は、以下に示した管理ライブラリのオブジェクト ベースの認証ステップと似ています。

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a>Azure Management Libraries for Java を使って認証する

Java 管理ライブラリを使ってリソースの作成と管理を行うときに、Azure に対してアプリケーションを認証する方法としては 2 つの選択肢があります。

### <a name="authenticate-with-an-applicationtokencredentials-object"></a>ApplicationTokenCredentials オブジェクトを使った認証

コード内で `ApplicationTokenCredentials` のインスタンスを作成し、最上位の `Azure` オブジェクトにサービス プリンシパルの資格情報を渡します。

```java
import com.microsoft.azure.credentials.ApplicationTokenCredentials;
import com.microsoft.azure.AzureEnvironment;

// ...

ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(client, 
        tenant,
        key, 
        AzureEnvironment.AZURE);
        
Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credentials)
        .withDefaultSubscription();
```

`client`、`tenant`、`key` は、[ファイル ベースの認証](#mgmt-file)で使うサービス プリンシパルと同じ値です。 `AzureEnvironment.AZURE` の値は、Azure パブリック クラウドに対する資格情報を作成するものです。 別のクラウド (`AzureEnvironment.AZURE_GERMANY` など) にアクセスする必要がある場合は、異なる値に変更してください。  

 サービス プリンシパルの値は、環境変数から読み取るか、シークレット管理ストア ([Key Vault](/azure/key-vault/key-vault-whatis) など) から読み取ります。 バージョン コントロール履歴で意図せず資格情報が漏えいするのを防ぐため、これらの値をコード内でクリア テキスト文字列として設定することは避けてください。   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a>ファイル ベースの認証 (プレビュー)

最も簡単な認証方法は、[Azure サービス プリンシパル](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)の資格情報を含んだプロパティ ファイルを次の形式で作成することです。

```text
# sample management library properties file
subscription=########-####-####-####-############
client=########-####-####-####-############
key=XXXXXXXXXXXXXXXX
tenant=########-####-####-####-############
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

- subscription: Azure CLI 2.0 の `az account show` から得られる *id* 値を使用します。
- client: アプリケーションを実行するために作成したサービス プリンシパルの出力から得られる *appId* 値を使用します。 アプリのサービス プリンシパルがない場合は、[Azure CLI 2.0 で作成](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)してください。
- key: サービス プリンシパル作成 CLI の出力から得られる *password* 値を使用します。 
- tenant: サービス プリンシパル作成 CLI の出力から得られる *tenant* 値を使用します。

このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。 ご利用のシェルから、このファイルの完全なパスを保持する環境変数を設定します。

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

ライブラリを使用するための起点となる `Azure` オブジェクトを作成します。 プロパティ ファイルの場所は、環境変数から読み取ります。

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



