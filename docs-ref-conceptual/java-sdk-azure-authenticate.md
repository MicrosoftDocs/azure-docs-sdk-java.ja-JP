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
ms.openlocfilehash: 3808c6d56b04f28c84a89a25219e4ec523f87964
ms.sourcegitcommit: 61030d025614b084e897809e603b2ec79900ec8d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="authenticate-with-the-azure-libraries-for-java"></a><span data-ttu-id="eccc1-104">Java 用 Azure ライブラリを使った認証</span><span class="sxs-lookup"><span data-stu-id="eccc1-104">Authenticate with the Azure libraries for Java</span></span> 

## <a name="connect-to-services-with-connection-strings"></a><span data-ttu-id="eccc1-105">接続文字列を使ってサービスに接続する</span><span class="sxs-lookup"><span data-stu-id="eccc1-105">Connect to services with connection strings</span></span>

<span data-ttu-id="eccc1-106">ほとんどの Azure サービス ライブラリでは、接続文字列またはセキュリティ キーが認証に使用されます。</span><span class="sxs-lookup"><span data-stu-id="eccc1-106">Most Azure service libraries use a connection string or secure key for authentication.</span></span> <span data-ttu-id="eccc1-107">たとえば SQL Database では、JDBC 接続文字列にユーザー名とパスワードの情報が格納されます。</span><span class="sxs-lookup"><span data-stu-id="eccc1-107">For example, SQL Database includes username and password information in the JDBC connection string:</span></span>

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

<span data-ttu-id="eccc1-108">Azure Storage でのアプリケーションの承認には、ストレージ キーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="eccc1-108">Azure Storage uses a storage key to authorize the application:</span></span>

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

<span data-ttu-id="eccc1-109">他の Azure サービス ([Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService)、[Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started)、[Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues) など) に対する認証には、サービスの接続文字列が使用されます。</span><span class="sxs-lookup"><span data-stu-id="eccc1-109">Service connection strings are used to authenticate to other Azure services like [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService), [Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started), and [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues).</span></span> <span data-ttu-id="eccc1-110">この接続文字列は、Azure Portal または CLI を使って取得できます。</span><span class="sxs-lookup"><span data-stu-id="eccc1-110">You can get the connection strings using the Azure portal or the CLI.</span></span>  <span data-ttu-id="eccc1-111">また、コード内から Azure Management Libraries for Java を使用し、リソースを照会することによって接続文字列を作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="eccc1-111">You can also use the Azure management libraries for Java to query resources to build connection strings in your code.</span></span> 

<span data-ttu-id="eccc1-112">たとえば次のコードでは、管理ライブラリを使ってストレージ アカウントの接続文字列を作成しています。</span><span class="sxs-lookup"><span data-stu-id="eccc1-112">For example, this code uses the management libraries to create a storage account connection string:</span></span>

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

<span data-ttu-id="eccc1-113">その他のライブラリでは、[サービス プリンシパル](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)を使ってアプリケーションを実行し、許可された資格情報で動作することをアプリケーションに承認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eccc1-113">Other libraries require your application to run with a [service prinicpal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) authorizing the application to run with granted credentials.</span></span> <span data-ttu-id="eccc1-114">この構成は、以下に示した管理ライブラリのオブジェクト ベースの認証ステップと似ています。</span><span class="sxs-lookup"><span data-stu-id="eccc1-114">This configuration is similar to the object-based authentication steps for the management library listed below.</span></span>

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a><span data-ttu-id="eccc1-115">Azure Management Libraries for Java を使って認証する</span><span class="sxs-lookup"><span data-stu-id="eccc1-115">Authenticate with the Azure management libraries for Java</span></span>

<span data-ttu-id="eccc1-116">Java 管理ライブラリを使ってリソースの作成と管理を行うときに、Azure に対してアプリケーションを認証する方法としては 2 つの選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="eccc1-116">Two options are available to authenticate your application with Azure when using the Java management libraries to create and manage resources.</span></span>

### <a name="authenticate-with-an-applicationtokencredentials-object"></a><span data-ttu-id="eccc1-117">ApplicationTokenCredentials オブジェクトを使った認証</span><span class="sxs-lookup"><span data-stu-id="eccc1-117">Authenticate with an ApplicationTokenCredentials object</span></span>

<span data-ttu-id="eccc1-118">コード内で `ApplicationTokenCredentials` のインスタンスを作成し、最上位の `Azure` オブジェクトにサービス プリンシパルの資格情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="eccc1-118">Create an instance of `ApplicationTokenCredentials` to supply the service principal credentials to the top-level `Azure` object from inside your code.</span></span>

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

<span data-ttu-id="eccc1-119">`client`、`tenant`、`key` は、[ファイル ベースの認証](#mgmt-file)で使うサービス プリンシパルと同じ値です。</span><span class="sxs-lookup"><span data-stu-id="eccc1-119">The `client`, `tenant` and `key` are the same service principal values used with [file-based authentication](#mgmt-file).</span></span> <span data-ttu-id="eccc1-120">`AzureEnvironment.AZURE` の値は、Azure パブリック クラウドに対する資格情報を作成するものです。</span><span class="sxs-lookup"><span data-stu-id="eccc1-120">The `AzureEnvironment.AZURE` value creates credentials against the Azure public cloud.</span></span> <span data-ttu-id="eccc1-121">別のクラウド (`AzureEnvironment.AZURE_GERMANY` など) にアクセスする必要がある場合は、異なる値に変更してください。</span><span class="sxs-lookup"><span data-stu-id="eccc1-121">Change this to a different value if you need to access another cloud (for example, `AzureEnvironment.AZURE_GERMANY`).</span></span>  

 <span data-ttu-id="eccc1-122">サービス プリンシパルの値は、環境変数から読み取るか、シークレット管理ストア ([Key Vault](/azure/key-vault/key-vault-whatis.md) など) から読み取ります。</span><span class="sxs-lookup"><span data-stu-id="eccc1-122">Read the service principal values from environment variables or a secret management store like [Key Vault](/azure/key-vault/key-vault-whatis.md).</span></span> <span data-ttu-id="eccc1-123">バージョン コントロール履歴で意図せず資格情報が漏えいするのを防ぐため、これらの値をコード内でクリア テキスト文字列として設定することは避けてください。</span><span class="sxs-lookup"><span data-stu-id="eccc1-123">Avoid setting these values as cleartext strings in your code to prevent accidentally exposing credentials in your version control history.</span></span>   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a><span data-ttu-id="eccc1-124">ファイル ベースの認証 (プレビュー)</span><span class="sxs-lookup"><span data-stu-id="eccc1-124">File based authentication (Preview)</span></span>

<span data-ttu-id="eccc1-125">最も簡単な認証方法は、[Azure サービス プリンシパル](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)の資格情報を含んだプロパティ ファイルを次の形式で作成することです。</span><span class="sxs-lookup"><span data-stu-id="eccc1-125">The simplest way to authenticate is to create a properties file that contains credentials for an [Azure service principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) using the following format:</span></span>

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

- <span data-ttu-id="eccc1-126">subscription: Azure CLI 2.0 の `az account show` から得られる *id* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="eccc1-126">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="eccc1-127">client: アプリケーションを実行するために作成したサービス プリンシパルの出力から得られる *appId* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="eccc1-127">client: use the *appId* value from the output taken from a service principal created to run the application.</span></span> <span data-ttu-id="eccc1-128">アプリのサービス プリンシパルがない場合は、[Azure CLI 2.0 で作成](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)してください。</span><span class="sxs-lookup"><span data-stu-id="eccc1-128">If you don't have a service principal for your app, [create one with the Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>
- <span data-ttu-id="eccc1-129">key: サービス プリンシパル作成 CLI の出力から得られる *password* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="eccc1-129">key: use the *password* value from the service principal create CLI output</span></span> 
- <span data-ttu-id="eccc1-130">tenant: サービス プリンシパル作成 CLI の出力から得られる *tenant* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="eccc1-130">tenant: use the *tenant* value from the service principal create CLI output</span></span>

<span data-ttu-id="eccc1-131">このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。</span><span class="sxs-lookup"><span data-stu-id="eccc1-131">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="eccc1-132">ご利用のシェルから、このファイルの完全なパスを保持する環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="eccc1-132">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="eccc1-133">ライブラリを使用するための起点となる `Azure` オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="eccc1-133">Create the entry point `Azure` object to start working with the libraries.</span></span> <span data-ttu-id="eccc1-134">プロパティ ファイルの場所は、環境変数から読み取ります。</span><span class="sxs-lookup"><span data-stu-id="eccc1-134">Read the location of the properties file through the environment variable.</span></span>

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



