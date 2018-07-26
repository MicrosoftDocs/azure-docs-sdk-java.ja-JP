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
ms.openlocfilehash: c14c89f961951d114362adee4fec6239e78cffb3
ms.sourcegitcommit: 33c70f921f1524c8bdb69ad5a1a3c1b4b1de97ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026774"
---
# <a name="azure-data-lake-analytics-libraries-for-java"></a>Azure Data Lake Analytics Libraries for Java

## <a name="overview"></a>概要

[Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) を使用して、大規模なデータ セットに対応するビッグ データ分析ジョブを実行します。

Azure Data Lake Analytics を導入するには、「[Java SDK で Azure Data Lake Analytics の使用を開始する](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk)」を参照してください。

## <a name="management-api"></a>管理 API

Management API を使用して、Data Lake Analytics のアカウント、ジョブ、ポリシー、カタログを管理します。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-analytics</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

## <a name="example"></a>例

新しい U-SQL ジョブを Data Lake Analytics に送信します。

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
> [Management API を探す](/java/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a>サンプル

[Java SDK を使用した Azure Data Lake Analytics][1] 

[1]: https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk

Azure Data Lake Analytics のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics)を参照してください。
