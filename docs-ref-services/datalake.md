---
title: Azure Data Lake Store Libraries for Java
description: Java Data Lake Store ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, ビッグ データ, Data Lake
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: bcd1fd17759f7d171006d7b2126019d00d06d1db
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823725"
---
# <a name="azure-data-lake-store-libraries-for-java"></a><span data-ttu-id="93161-104">Azure Data Lake Store Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="93161-104">Azure Data Lake Store libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="93161-105">概要</span><span class="sxs-lookup"><span data-stu-id="93161-105">Overview</span></span>

<span data-ttu-id="93161-106">[Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview) を使用して、分析用にあらゆるサイズ、種類、取り込み速度のデータを 1 つの場所にキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="93161-106">Capture data of any size, type, and ingestion speed in a single place for analytics with [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview).</span></span>

<span data-ttu-id="93161-107">Data Lake Store を導入するには、「[Java で Azure Data Lake Store の使用を開始する](/azure/data-lake-store/data-lake-store-get-started-java-sdk)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="93161-107">To get started with Data Lake Store, see [Get started with Azure Data Lake Store using Java](/azure/data-lake-store/data-lake-store-get-started-java-sdk).</span></span>


## <a name="client-library"></a><span data-ttu-id="93161-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="93161-108">Client library</span></span>

<span data-ttu-id="93161-109">クライアント ライブラリを使用して、Data Lake Store でファイルの読み取りと書き込み、アクセス許可とメタデータの設定、ファイルとディレクトリの管理を行います。</span><span class="sxs-lookup"><span data-stu-id="93161-109">Read and write files, set permissions and metadata, and manage files and directories in Data Lake Store with the client library.</span></span>

<span data-ttu-id="93161-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="93161-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="93161-111">例</span><span class="sxs-lookup"><span data-stu-id="93161-111">Example</span></span>

<span data-ttu-id="93161-112">完全修飾ドメイン名と OAuth2 アクセス トークンから Data Lake クライアントを作成した後、Data Lake にファイルを作成し、そのファイルに書き込みます。</span><span class="sxs-lookup"><span data-stu-id="93161-112">Create a Data Lake client from a fully qualified domain name and OAuth2 access token, then create a file in Data Lake and write to it.</span></span>

```java
// AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

// create directory
client.createDirectory("/a/b/w");
        
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="93161-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="93161-113">Explore the Client APIs</span></span>](/java/api/overview/azure/datalakestore/client)


## <a name="management-api"></a><span data-ttu-id="93161-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="93161-114">Management API</span></span>

<span data-ttu-id="93161-115">Management API を使用して、Data Lake Store アカウント、ファイアウォール規則、信頼できる ID プロバイダーを管理します。</span><span class="sxs-lookup"><span data-stu-id="93161-115">Use the management API to manage Data Lake Store accounts, firewall rules, and trusted identity providers.</span></span>

<span data-ttu-id="93161-116">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="93161-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="93161-117">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="93161-117">Explore the Management APIs</span></span>](/java/api/overview/azure/datalakestore/management)

## <a name="samples"></a><span data-ttu-id="93161-118">サンプル</span><span class="sxs-lookup"><span data-stu-id="93161-118">Samples</span></span>

<span data-ttu-id="93161-119">[Azure Data Lake の概要][1]</span><span class="sxs-lookup"><span data-stu-id="93161-119">[Azure Data Lake Get Started][1]</span></span> 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

<span data-ttu-id="93161-120">アプリで利用できる [Azure Data Lake Store のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=lake)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="93161-120">Explore more [sample Java code for Azure Data Lake Store](https://azure.microsoft.com/resources/samples/?platform=java&term=lake) you can use in your apps.</span></span>
