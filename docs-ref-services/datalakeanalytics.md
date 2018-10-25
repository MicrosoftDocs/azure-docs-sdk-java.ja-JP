---
title: Azure Data Lake Analytics Libraries for Java
description: Java Data Lake Analytics ライブラリのリファレンス ドキュメント
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
ms.openlocfilehash: 0e97d2c53ca6b993a2240b37576e765009f81c51
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799858"
---
# <a name="azure-data-lake-analytics-libraries-for-java"></a><span data-ttu-id="c895c-104">Azure Data Lake Analytics Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="c895c-104">Azure Data Lake Analytics libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="c895c-105">概要</span><span class="sxs-lookup"><span data-stu-id="c895c-105">Overview</span></span>

<span data-ttu-id="c895c-106">[Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) を使用して、大規模なデータ セットに対応するビッグ データ分析ジョブを実行します。</span><span class="sxs-lookup"><span data-stu-id="c895c-106">Run big data analysis jobs that scale to massive data sets with [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview).</span></span>

<span data-ttu-id="c895c-107">Azure Data Lake Analytics を導入するには、「[Java SDK で Azure Data Lake Analytics の使用を開始する](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c895c-107">To get started with Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics using Java SDK](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk).</span></span>

## <a name="management-api"></a><span data-ttu-id="c895c-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="c895c-108">Management API</span></span>

<span data-ttu-id="c895c-109">Management API を使用して、Data Lake Analytics のアカウント、ジョブ、ポリシー、カタログを管理します。</span><span class="sxs-lookup"><span data-stu-id="c895c-109">Use the management API to manage Data Lake Analytics accounts, jobs, policies, and catalogs.</span></span>

<span data-ttu-id="c895c-110">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="c895c-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-analytics</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="c895c-111">例</span><span class="sxs-lookup"><span data-stu-id="c895c-111">Example</span></span>

<span data-ttu-id="c895c-112">新しい U-SQL ジョブを Data Lake Analytics に送信します。</span><span class="sxs-lookup"><span data-stu-id="c895c-112">Submit a new U-SQL job to Data Lake Analytics.</span></span>

```java
// authenticate with service principal credentials
ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
DataLakeAnalyticsJobManagementClient adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);

// set up job parameters
UUID jobId = java.util.UUID.randomUUID();
USqlJobProperties properties = new USqlJobProperties();
properties.setScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();");
JobInformation parameters = new JobInformation();
parameters.setName("testJob");
parameters.setJobId(jobId);
parameters.setType(JobType.USQL);
parameters.setProperties(properties);

// create the job
JobInformation jobInfo = adlaJobClient.getJobOperations().create(accountName, jobId, parameters).getBody();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c895c-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="c895c-113">Explore the Management APIs</span></span>](/java/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a><span data-ttu-id="c895c-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="c895c-114">Samples</span></span>

<span data-ttu-id="c895c-115">[Java SDK を使用した Azure Data Lake Analytics][1]</span><span class="sxs-lookup"><span data-stu-id="c895c-115">[Azure Data Lake Analytics using Java SDK][1]</span></span> 

[1]: https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk

<span data-ttu-id="c895c-116">Azure Data Lake Analytics のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c895c-116">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics) of Azure Data Lake Analytics samples.</span></span>
