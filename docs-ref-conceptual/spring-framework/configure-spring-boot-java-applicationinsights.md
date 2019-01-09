---
title: Azure Application Insights SpringBoot Starter を使用するように Spring Boot Initializer アプリを構成する
description: Application Insights SpringBoot Starter を使用するように、Spring Initializer で作成された Spring Boot アプリケーションを構成します。
services: Application-Insights
documentationcenter: java
author: dhaval24
manager: alexklim
editor: ''
ms.assetid: ''
ms.author: dhdoshi
ms.date: 12/19/2018
ms.devlang: java
ms.service: Azure Monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.workload: na
ms.openlocfilehash: f69cdcc5b479e83b230f23a8a76f96284a1b785b
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991436"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a><span data-ttu-id="182d7-103">Application Insights を使用するように Spring Boot Initializer アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="182d7-103">Configure a Spring Boot Initializer app to use Application Insights</span></span>

<span data-ttu-id="182d7-104">この記事では、**[Spring Initializr]** を使用して Spring Boot アプリケーションを作成す方法を説明します。このアプリケーションは、Azure Application Insights Spring Boot Starter を使用してクラウド上の Java アプリケーションをエンドツーエンドで監視します。</span><span class="sxs-lookup"><span data-stu-id="182d7-104">This article walks you through creating a Spring Boot application using **[Spring Initializr]**, that uses Azure Application Insights Spring Boot Starter for end-to-end monitoring of Java applications on cloud.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="182d7-105">前提条件</span><span class="sxs-lookup"><span data-stu-id="182d7-105">Prerequisites</span></span>

<span data-ttu-id="182d7-106">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="182d7-106">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="182d7-107">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="182d7-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="182d7-108">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="182d7-108">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="182d7-109">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-109">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="182d7-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="182d7-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="182d7-111">Spring Initializr を使用してカスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="182d7-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="182d7-112">[https://start.spring.io/](https://start.spring.io/) を参照します。</span><span class="sxs-lookup"><span data-stu-id="182d7-112">Browse to [https://start.spring.io/](https://start.spring.io/).</span></span>

1. <span data-ttu-id="182d7-113">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力して、依存関係セクションで Web 依存関係を選択します。</span><span class="sxs-lookup"><span data-stu-id="182d7-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select web dependency in the dependenies section.</span></span>

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="182d7-115">Spring Initializr では、**[Group]\(グループ\)** と **[Artifact]\(アーティファクト\)** の名前を使用してパッケージ名を作成します (例: *com.example.demo*)。</span><span class="sxs-lookup"><span data-stu-id="182d7-115">The Spring Initializr will use the **Group** and **Artifact** names to create the package name; for example: *com.example.demo*.</span></span>
   >

1. <span data-ttu-id="182d7-116">ボタンをクリックして、**プロジェクトを生成**します。</span><span class="sxs-lookup"><span data-stu-id="182d7-116">Click the button to **Generate Project**.</span></span>

1. <span data-ttu-id="182d7-117">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="182d7-117">When prompted, download the project to a path on your local computer.</span></span>

1. <span data-ttu-id="182d7-118">ファイルをローカル システム上に展開したら、カスタム Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="182d7-118">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![カスタム Spring Boot プロジェクト ファイル][SI02]

## <a name="create-an-application-insights-resource-on-azure"></a><span data-ttu-id="182d7-120">Azure で Application Insights のリソースを作成する</span><span class="sxs-lookup"><span data-stu-id="182d7-120">Create an Application Insights Resource on Azure</span></span>

1. <span data-ttu-id="182d7-121"><https://portal.azure.com/> で Azure Portal を参照し、**[+新規]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="182d7-121">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure ポータル][AZ01]

1. <span data-ttu-id="182d7-123">**管理ツール** 、 **Application Insights**  の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="182d7-123">Click **Management Tools**, and then click **Application Insights**.</span></span>

   ![Azure ポータル][AZ02]

1. <span data-ttu-id="182d7-125">**[Application Insights の新しい リソース]** ページで、以下の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="182d7-125">On the **New Application Insights Resource** page, specify the following information:</span></span>

   * <span data-ttu-id="182d7-126">Application Insights のリソースの**名前**を入力してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-126">Enter the **Name** for your Application Insights resource.</span></span>
   * <span data-ttu-id="182d7-127">**[アプリケーションの種類]** として [Java Web アプリケーション] を選択します。</span><span class="sxs-lookup"><span data-stu-id="182d7-127">Choose the **Application Type** to Java Web Application.</span></span>
   * <span data-ttu-id="182d7-128">**[サブスクリプション]**、**[リソース グループ]**、**[場所]** を指定します。</span><span class="sxs-lookup"><span data-stu-id="182d7-128">Specify your **Subscription**, **Resource group** and **Location**.</span></span>
   * <span data-ttu-id="182d7-129">リソースを Azure portal にピン留めする場合は、[ダッシュボードにピン留めする] を選択します。</span><span class="sxs-lookup"><span data-stu-id="182d7-129">Select Pin to dashboard option, if you would like to pin the resource on your Azure portal.</span></span>

   <span data-ttu-id="182d7-130">これらのオプションを指定したら、**[作成]** をクリックして Application Insights リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="182d7-130">When you have specified these options, click **Create** to create your Application Insights resource.</span></span>

   ![Azure ポータル][AZ03]

1. <span data-ttu-id="182d7-132">リソースが作成されると、Azure の**ダッシュボード**の他、**[すべてのリソース]** ページにも表示されます。</span><span class="sxs-lookup"><span data-stu-id="182d7-132">Once your resource has been created, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources** pages.</span></span> <span data-ttu-id="182d7-133">それらの場所のいずれかでリソースをクリックすると、Application Insights リソースの概要ページを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="182d7-133">You can click on your resource on any of those locations to open the overview page of the Application Insights resource.</span></span> <span data-ttu-id="182d7-134">この概要ページから、**インストルメンテーション キー**をコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="182d7-134">From this overview page please copy the **instrumentation key**.</span></span>

   ![Azure ポータル][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a><span data-ttu-id="182d7-136">Application Insights を使用するように、ダウンロードした Spring Boot Initializer を構成する</span><span class="sxs-lookup"><span data-stu-id="182d7-136">Configure your downloaded Spring Boot Application to use Application Insights</span></span>

1. <span data-ttu-id="182d7-137">アプリのルート ディレクトリで *POM.xml* ファイルを見つけ、dependencies セクションに次の依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="182d7-137">Locate the *POM.xml* file in the root directory of your app, and add the following dependency in its dependencies section.</span></span> 

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

1. <span data-ttu-id="182d7-138">アプリの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="182d7-138">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![application.properties ファイルを探す][RE01]

1. <span data-ttu-id="182d7-140">テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値を適切な資格情報を含む適切なプロパティで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="182d7-140">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties with appropriate credentials:</span></span>

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   <span data-ttu-id="182d7-141">Application Insights の詳細な設定方法については、「[Application Insights Springboot starter readme (Application Insights Springboot Starter の Readme)](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-141">For more ways to fine tune Application Insights please refer to [Application Insights Springboot starter readme](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="182d7-142">異なる Application Insights インストルメンテーション キー (つまり、</span><span class="sxs-lookup"><span data-stu-id="182d7-142">You can use different Application Insights instrumentation keys (i.e</span></span> <span data-ttu-id="182d7-143">異なるリソース) を、運用、開発などのさまざまなプロファイルに対して使用できます。追加情報については、「[Spring Boot のプロファイル固有のプロパティ]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-143">different resources) for different profiles like PROD, DEV etc. Please refer to [Spring Boot Profile Specific Properties] for additional information.</span></span> 

1. <span data-ttu-id="182d7-144">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="182d7-144">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="182d7-145">パッケージのソース フォルダーの下に *controller* という名前のフォルダーを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="182d7-145">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   <span data-ttu-id="182d7-146">または</span><span class="sxs-lookup"><span data-stu-id="182d7-146">-or-</span></span>

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. <span data-ttu-id="182d7-147">*TestController.java* という名前の新しいファイルを *controller* フォルダー内に作成します。</span><span class="sxs-lookup"><span data-stu-id="182d7-147">Create a new file named *TestController.java* in the *controller* folder.</span></span> <span data-ttu-id="182d7-148">テキスト エディターでそのファイルを開き、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="182d7-148">Open the file in a text editor and add the following code to it:</span></span>

   ```java
    package com.example.demo;

    import com.microsoft.applicationinsights.TelemetryClient;
    import java.io.IOException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    import com.microsoft.applicationinsights.telemetry.Duration;

    @RestController
    @RequestMapping("/sample")
    public class TestController {

        @Autowired
        TelemetryClient telemetryClient;

        @GetMapping("/hello")
        public String hello() {

            //track a custom event  
            telemetryClient.trackEvent("Sending a custom event...");

            //trace a custom trace
            telemetryClient.trackTrace("Sending a custom trace....");

            //track a custom metric
            telemetryClient.trackMetric("custom metric", 1.0);

            //track a custom dependency
            telemetryClient.trackDependency("SQL", "Insert", new Duration(0, 0, 1, 1, 1), true);

            return "hello";
        }
    }
   ```

   <span data-ttu-id="182d7-149">ここでは、`com.example.demo` を実際のプロジェクトのパッケージ名に置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="182d7-149">Where you will need to replace `com.example.demo` with the package name for your project.</span></span>

1. <span data-ttu-id="182d7-150">*TestController.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="182d7-150">Save and close the *TestController.java* file.</span></span>

1. <span data-ttu-id="182d7-151">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="182d7-151">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="182d7-152">Web ブラウザーを使用して http://localhost:8080/sample/hello を参照することによって Web アプリをテストするか、または curl が使用可能な場合は次の例のような構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="182d7-152">Test the web app by browsing to http://localhost:8080/sample/hello using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   <span data-ttu-id="182d7-153">サンプル コントローラーからの "hello!" </span><span class="sxs-lookup"><span data-stu-id="182d7-153">You should see the "hello!"</span></span> <span data-ttu-id="182d7-154">メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="182d7-154">message from your sample controller displayed.</span></span> <span data-ttu-id="182d7-155">Application Insights によって自動的にこの要求が収集され、テレメトリ項目として送信されます。その際、コントローラー ロジックで指定されている、この要求に関連付けられたカスタム イベント、カスタム メトリック、カスタム依存関係、およびカスタム トレースも送信されます。</span><span class="sxs-lookup"><span data-stu-id="182d7-155">Application Insights will automatically collect this request and send it as a telemetry item with it's associated custom event, custom metric, custom dependency and custom trace as specified in the controller logic.</span></span> 

   <span data-ttu-id="182d7-156">このデータは、数秒後に Azure portal に表示されます。</span><span class="sxs-lookup"><span data-stu-id="182d7-156">After a few seconds you should see the data on Azure portal.</span></span> 

   ![Azure Portal][AZ05]

   <span data-ttu-id="182d7-158">[アプリケーション マップ] タイルをクリックすると、大まかなコンポーネントと、コンポーネント相互の相互作用を表示できます。</span><span class="sxs-lookup"><span data-stu-id="182d7-158">You can click on Application Map tile to view high level components and their interaction with each other.</span></span> <span data-ttu-id="182d7-159">ここで、アプリケーション全体の概要を把握することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="182d7-159">This is a recommended place to get a high level overview of entire application.</span></span> <span data-ttu-id="182d7-160">Spring Boot の各マイクロサービスは、Spring のアプリケーション名によって認識されます。</span><span class="sxs-lookup"><span data-stu-id="182d7-160">Each Spring Boot Microservice is recognized by the spring application name.</span></span> <span data-ttu-id="182d7-161">必ずこれを設定してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-161">Please remember to set it.</span></span>

   ![Azure Portal][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a><span data-ttu-id="182d7-163">log4j ログを Application Insights に送信するように Springboot アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="182d7-163">Configure Springboot Application to send log4j logs to Application Insights</span></span>

1. <span data-ttu-id="182d7-164">プロジェクトの POM.xml を開き、次を使用して dependencies セクションに追加/変更します。</span><span class="sxs-lookup"><span data-stu-id="182d7-164">Modify the POM.xml file of the project and add/modify the dependencies section with following.</span></span> 

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. <span data-ttu-id="182d7-165">*POM.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="182d7-165">Save and close the *POM.xml* file.</span></span>

3. <span data-ttu-id="182d7-166">\Src\main\resources フォルダーに、*log4j2.xml* という新しいファイルを作成し、構成します。</span><span class="sxs-lookup"><span data-stu-id="182d7-166">In \src\main\resources folder, create a new file *log4j2.xml* and configure it.</span></span> <span data-ttu-id="182d7-167">例: </span><span class="sxs-lookup"><span data-stu-id="182d7-167">For example:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration packages="com.microsoft.applicationinsights.log4j.v2">
  <Properties>
    <Property name="LOG_PATTERN">
      %d{yyyy-MM-dd HH:mm:ss.SSS} %5p ${hostName} --- [%15.15t] %-40.40c{1.} : %m%n%ex
    </Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
    <ApplicationInsightsAppender name="aiAppender">
    </ApplicationInsightsAppender>
  </Appenders>
  <Loggers>
    <Root level="trace">
      <AppenderRef ref="Console"  />
      <AppenderRef ref="aiAppender"  />
    </Root>
  </Loggers>
</Configuration>
```
4. <span data-ttu-id="182d7-168">上記のように Spring Boot アプリケーションをビルドし、もう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="182d7-168">Build and run the Spring Boot application again as shown above.</span></span> 

<span data-ttu-id="182d7-169">数秒で、すべての Spring ログが Azure Portal で利用できることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="182d7-169">Within few seconds, you should see all the spring logs being available on Azure Portal.</span></span> 

![Azure Portal][AZ06]

<span data-ttu-id="182d7-171">Analytics ポータルでは、詳細なログ メッセージを確認したり、分析を実行したりできます。</span><span class="sxs-lookup"><span data-stu-id="182d7-171">You can even look at the detailed log messages and do analysis on Analytics Portal.</span></span> 

![Azure Portal][AZ07]

## <a name="next-steps"></a><span data-ttu-id="182d7-173">次の手順</span><span class="sxs-lookup"><span data-stu-id="182d7-173">Next steps</span></span>

<span data-ttu-id="182d7-174">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="182d7-174">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="182d7-175">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="182d7-175">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="182d7-176">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="182d7-176">Additional Resources</span></span>

<span data-ttu-id="182d7-177">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-177">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="182d7-178">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="182d7-178">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="182d7-179">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="182d7-179">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="182d7-180">Application Insights では、外部の依存関係を自動収集できます。また、依存関係と着信要求との相関関係も自動収集できます。</span><span class="sxs-lookup"><span data-stu-id="182d7-180">Application Insights supports automatic collection of external dependencies and its correlation with incoming requests.</span></span> <span data-ttu-id="182d7-181">現在、Oracle、MsSQL、MySQL、および Redis の自動収集がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="182d7-181">Currently we support autocollection of Oracle, MsSQL, MySQL and Redis.</span></span> <span data-ttu-id="182d7-182">自動収集の有効化の詳細については、[Application Insights Java エージェントの使用方法](/azure/application-insights/app-insights-java-agent)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="182d7-182">For more details on enabling autocollection please follow the article [how to use Application Insights Java agent](/azure/application-insights/app-insights-java-agent).</span></span>

<span data-ttu-id="182d7-183">Azure Application Insights と、その監視機能の詳細については、**[Application Insights]** のホーム ページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="182d7-183">For more information about Azure Application Insights, and it's monitoring capabilities, see the **[Application Insights]** home page.</span></span>

<span data-ttu-id="182d7-184">Application Insights Spring Boot Starter の追加の構成の詳細については、こちらの[リンク](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-184">For more information about additional configuration details of Application Insights Spring Boot Starter, please refer to this [link](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

<span data-ttu-id="182d7-185">機能要求や潜在的なバグについては、[GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) リポジトリで問題を開いてください。</span><span class="sxs-lookup"><span data-stu-id="182d7-185">For feature requests and potential bugs, please open issues on our [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) repository.</span></span>

<span data-ttu-id="182d7-186">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="182d7-186">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="182d7-187">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="182d7-187">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="182d7-188">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="182d7-188">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="182d7-189">Spring Boot を使い始めた開発者を支援するために、[https://github.com/spring-guides/](https://github.com/spring-guides/) では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="182d7-189">To help developers get started with Spring Boot, several sample Spring Boot packages are available at [https://github.com/spring-guides/](https://github.com/spring-guides/).</span></span> <span data-ttu-id="182d7-190">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="182d7-190">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot のプロファイル固有のプロパティ]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Boot Profile Specific Properties]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Application Insights]: /azure/application-insights/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_2.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_3.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Get_IKey.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/OverviewBladeResults.png
[AZ06]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Search_and_traces.png
[AZ07]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/traces_details.png
[AZ08]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/AppMap.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/spring_start.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/After_extract.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationproperties_loc.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationinsightsproperties.png
