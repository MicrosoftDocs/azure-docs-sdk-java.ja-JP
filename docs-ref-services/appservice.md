---
title: Azure App Service Libraries for Java
description: Azure Management API を使用して Azure App Service への Web アプリのデプロイを自動化します。
keywords: Azure, Java, SDK, API, Web アプリ, モバイル, App Service
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/09/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appservice
ms.openlocfilehash: adbb527666553ecc3039ce35c035d017f502c801
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823795"
---
# <a name="azure-app-service-libraries-for-java"></a>Azure App Service Libraries for Java

## <a name="overview"></a>概要

[Azure App Service](/azure/app-service) に対する Web サイト、Web アプリケーション、REST API のデプロイと管理を行います。

Azure App Service の概要については、「[Azure で初めての Java Web アプリを作成する](/azure/app-service-web/app-service-web-get-started-java)」を参照してください。

## <a name="management-api"></a>管理 API

Azure App Service におけるアプリケーションのデプロイ、スケール、構成は、Management API を使って行います。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/appservice/management)

### <a name="example"></a>例

Linux 上で動作する Azure Web アプリに Docker イメージから Web アプリをデプロイします。

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a>サンプル

[FTP または GitHub から Web アプリをデプロイする][1]  
[デプロイ スロットを使ってアプリのバージョンをスワップする][2]  
[カスタム ドメインを構成する][3]   
[複数のリージョンに Web アプリをスケーリングする][4]   

アプリで利用できる [Azure App Service のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice)を探しましょう。

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
