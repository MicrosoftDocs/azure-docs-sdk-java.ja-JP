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
# <a name="azure-application-insights-libraries-for-java"></a><span data-ttu-id="7e640-104">Azure Application Insights Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="7e640-104">Azure Application Insights libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="7e640-105">概要</span><span class="sxs-lookup"><span data-stu-id="7e640-105">Overview</span></span>

<span data-ttu-id="7e640-106">[Application Insights](/azure/application-insights/app-insights-overview) で Web アプリおよびサービスの問題を検出、トリアージ、および診断します。</span><span class="sxs-lookup"><span data-stu-id="7e640-106">Detect, triage, and diagnose issues in your web apps and services with [Application Insights](/azure/application-insights/app-insights-overview).</span></span>

<span data-ttu-id="7e640-107">Application Insights の使用を開始するには、「[Java Web プロジェクトで Application Insights を使う](/azure/application-insights/app-insights-java-get-started)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7e640-107">To get started with Application Insights, see [Get started with Application Insights in a Java web project](/azure/application-insights/app-insights-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="7e640-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="7e640-108">Client library</span></span>

<span data-ttu-id="7e640-109">Application Insights クライアント ライブラリで、テレメトリを追加し、アプリ内のイベント、例外、およびユーザー メトリックを追跡します。</span><span class="sxs-lookup"><span data-stu-id="7e640-109">Add telemetry to track events, exceptions, and user metrics in your apps with the Application Insights client library.</span></span>

<span data-ttu-id="7e640-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="7e640-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="7e640-111">例</span><span class="sxs-lookup"><span data-stu-id="7e640-111">Example</span></span>

<span data-ttu-id="7e640-112">新しいメトリック エントリを作成し、その値を記録します。</span><span class="sxs-lookup"><span data-stu-id="7e640-112">Create a new metric entry and record a value for it.</span></span>

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="7e640-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="7e640-113">Explore the Client APIs</span></span>](/java/api/overview/azure/appinsights/clientlibrary)

## <a name="samples"></a><span data-ttu-id="7e640-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="7e640-114">Samples</span></span>

<span data-ttu-id="7e640-115">アプリから利用できる [Application Insights のサンプル Java コード](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="7e640-115">Explore more [sample Java code for Application Insights](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java) you can use in your apps.</span></span>