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
ms.openlocfilehash: bf4f7e51f3108d684503465050d69461240f17e3
ms.sourcegitcommit: 9df42bd342ef2d25d56a6045f1ab1baf6f2c250e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2019
ms.locfileid: "54237293"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a>Application Insights を使用するように Spring Boot Initializer アプリを構成する

この記事では、**[Spring Initializr]** を使用して Spring Boot アプリケーションを作成す方法を説明します。このアプリケーションは、Azure Application Insights Spring Boot Starter を使用してクラウド上の Java アプリケーションをエンドツーエンドで監視します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* 現在、Application Insights Spring Boot スターターでは、Web Flux と Netty API は**サポートされていません**。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Spring Initializr を使用してカスタム アプリケーションを作成する

1. [https://start.spring.io/](https://start.spring.io/) を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力して、依存関係セクションで Web 依存関係を選択します。

   ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > Spring Initializr では、**[Group]\(グループ\)** と **[Artifact]\(アーティファクト\)** の名前を使用してパッケージ名を作成します (例: *com.example.demo*)。
   >

1. ボタンをクリックして、**プロジェクトを生成**します。

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

1. ファイルをローカル システム上に展開したら、カスタム Spring Boot アプリケーションの編集を開始できます。

   ![カスタム Spring Boot プロジェクト ファイル][SI02]

## <a name="create-an-application-insights-resource-on-azure"></a>Azure で Application Insights のリソースを作成する

1. <https://portal.azure.com/> で Azure Portal を参照し、**[+新規]** をクリックします。

   ![Azure ポータル][AZ01]

1. **管理ツール** 、 **Application Insights**  の順にクリックします。

   ![Azure ポータル][AZ02]

1. **[Application Insights の新しい リソース]** ページで、以下の情報を指定します。

   * Application Insights のリソースの**名前**を入力してください。
   * **[アプリケーションの種類]** として [Java Web アプリケーション] を選択します。
   * **[サブスクリプション]**、**[リソース グループ]**、**[場所]** を指定します。
   * リソースを Azure portal にピン留めする場合は、[ダッシュボードにピン留めする] を選択します。

   これらのオプションを指定したら、**[作成]** をクリックして Application Insights リソースを作成します。

   ![Azure ポータル][AZ03]

1. リソースが作成されると、Azure の**ダッシュボード**の他、**[すべてのリソース]** ページにも表示されます。 それらの場所のいずれかでリソースをクリックすると、Application Insights リソースの概要ページを開くことができます。 この概要ページから、**インストルメンテーション キー**をコピーしてください。

   ![Azure ポータル][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a>Application Insights を使用するように、ダウンロードした Spring Boot Initializer を構成する

1. アプリのルート ディレクトリで *POM.xml* ファイルを見つけ、dependencies セクションに次の依存関係を追加します。 

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

1. アプリの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。

   ![application.properties ファイルを探す][RE01]

1. テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値を適切な資格情報を含む適切なプロパティで置き換えます。

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   Application Insights の詳細な設定方法については、「[Application Insights Springboot starter readme (Application Insights Springboot Starter の Readme)](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)」を参照してください。

   > [!NOTE]
   > 
   > 異なる Application Insights インストルメンテーション キー (つまり、 異なるリソース) を、運用、開発などのさまざまなプロファイルに対して使用できます。追加情報については、「[Spring Boot のプロファイル固有のプロパティ]」を参照してください。 

1. *application.properties* ファイルを保存して閉じます。

1. パッケージのソース フォルダーの下に *controller* という名前のフォルダーを作成します。次に例を示します。

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   または

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. *TestController.java* という名前の新しいファイルを *controller* フォルダー内に作成します。 テキスト エディターでそのファイルを開き、次の行を追加します。

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

   ここでは、`com.example.demo` を実際のプロジェクトのパッケージ名に置き換える必要があります。

1. *TestController.java* ファイルを保存して閉じます。

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Web ブラウザーを使用して http://localhost:8080/sample/hello を参照することによって Web アプリをテストするか、または curl が使用可能な場合は次の例のような構文を使用します。

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   サンプル コントローラーからの "hello!"  メッセージが表示されます。 Application Insights によって自動的にこの要求が収集され、テレメトリ項目として送信されます。その際、コントローラー ロジックで指定されている、この要求に関連付けられたカスタム イベント、カスタム メトリック、カスタム依存関係、およびカスタム トレースも送信されます。 

   このデータは、数秒後に Azure portal に表示されます。 

   ![Azure Portal][AZ05]

   [アプリケーション マップ] タイルをクリックすると、大まかなコンポーネントと、コンポーネント相互の相互作用を表示できます。 ここで、アプリケーション全体の概要を把握することをお勧めします。 Spring Boot の各マイクロサービスは、Spring のアプリケーション名によって認識されます。 必ずこれを設定してください。

   ![Azure Portal][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a>log4j ログを Application Insights に送信するように Springboot アプリケーションを構成する

1. プロジェクトの POM.xml を開き、次を使用して dependencies セクションに追加/変更します。 

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

2. *POM.xml* ファイルを保存して閉じます。

3. \Src\main\resources フォルダーに、*log4j2.xml* という新しいファイルを作成し、構成します。 例: 

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
4. 上記のように Spring Boot アプリケーションをビルドし、もう一度実行します。 

数秒で、すべての Spring ログが Azure Portal で利用できることを確認できます。 

![Azure Portal][AZ06]

Analytics ポータルでは、詳細なログ メッセージを確認したり、分析を実行したりできます。 

![Azure Portal][AZ07]

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Application Insights では、外部の依存関係を自動収集できます。また、依存関係と着信要求との相関関係も自動収集できます。 現在、Oracle、MsSQL、MySQL、および Redis の自動収集がサポートされています。 自動収集の有効化の詳細については、[Application Insights Java エージェントの使用方法](/azure/application-insights/app-insights-java-agent)に関するページをご覧ください。

Azure Application Insights と、その監視機能の詳細については、**[Application Insights]** のホーム ページをご覧ください。

Application Insights Spring Boot Starter の追加の構成の詳細については、こちらの[リンク](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)を参照してください。

機能要求や潜在的なバグについては、[GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) リポジトリで問題を開いてください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、[https://github.com/spring-guides/](https://github.com/spring-guides/) では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

<!-- URL List -->

[Java 開発者向けの Azure]: /java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot のプロファイル固有のプロパティ]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
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
