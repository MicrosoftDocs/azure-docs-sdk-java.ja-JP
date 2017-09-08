---
title: Azure Application Insights Libraries for Java
description: "Azure Appplication Insights 用の Java 管理 API のリファレンス ドキュメント"
keywords: "Azure, Java, SDK, API, AppInsights, テレメトリ, 診断, トレース, ログ, パフォーマンス"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/21/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appinsights
ms.openlocfilehash: 9f943dc87d9e9b3e015407eea4dfd2900040da37
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="azure-application-insights-libraries-for-java"></a>Azure Application Insights Libraries for Java

## <a name="overview"></a>概要

[Application Insights](/azure/application-insights/app-insights-overview) で Web アプリおよびサービスの問題を検出、トリアージ、および診断します。

Application Insights の使用を開始するには、「[Java Web プロジェクトで Application Insights を使う](/azure/application-insights/app-insights-java-get-started)」を参照してください。

## <a name="client-library"></a>クライアント ライブラリ

Application Insights クライアント ライブラリで、テレメトリを追加し、アプリ内のイベント、例外、およびユーザー メトリックを追跡します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a>例

新しいメトリック エントリを作成し、その値を記録します。

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/appinsights/clientlibrary)

## <a name="samples"></a>サンプル

アプリから利用できる [Application Insights のサンプル Java コード](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java)を探しましょう。