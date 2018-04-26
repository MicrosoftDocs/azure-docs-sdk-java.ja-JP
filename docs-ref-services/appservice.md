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
---
# <a name="azure-app-service-libraries-for-java"></a><span data-ttu-id="f5837-104">Azure App Service Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="f5837-104">Azure App Service libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="f5837-105">概要</span><span class="sxs-lookup"><span data-stu-id="f5837-105">Overview</span></span>

<span data-ttu-id="f5837-106">[Azure App Service](/azure/app-service) に対する Web サイト、Web アプリケーション、REST API のデプロイと管理を行います。</span><span class="sxs-lookup"><span data-stu-id="f5837-106">Deploy and manage websites, web applications, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="f5837-107">Azure App Service の概要については、「[Azure で初めての Java Web アプリを作成する](/azure/app-service-web/app-service-web-get-started-java)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f5837-107">To get started with Azure App Service, see [Create your first Java web app in Azure](/azure/app-service-web/app-service-web-get-started-java).</span></span>

## <a name="management-api"></a><span data-ttu-id="f5837-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="f5837-108">Management API</span></span>

<span data-ttu-id="f5837-109">Azure App Service におけるアプリケーションのデプロイ、スケール、構成は、Management API を使って行います。</span><span class="sxs-lookup"><span data-stu-id="f5837-109">Deploy, scale, and configure applications in Azure App Service with the management API.</span></span>

<span data-ttu-id="f5837-110">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="f5837-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [<span data-ttu-id="f5837-111">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="f5837-111">Explore the Management APIs</span></span>](/java/api/overview/azure/appservice/management)

### <a name="example"></a><span data-ttu-id="f5837-112">例</span><span class="sxs-lookup"><span data-stu-id="f5837-112">Example</span></span>

<span data-ttu-id="f5837-113">Linux 上で動作する Azure Web アプリに Docker イメージから Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="f5837-113">Deploy a webapp from a Docker image into an Azure Web App running on Linux.</span></span>

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a><span data-ttu-id="f5837-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="f5837-114">Samples</span></span>

<span data-ttu-id="f5837-115">[FTP または GitHub から Web アプリをデプロイする][1]</span><span class="sxs-lookup"><span data-stu-id="f5837-115">[Deploy a web app from FTP or GitHub][1]</span></span>  
<span data-ttu-id="f5837-116">[デプロイ スロットを使ってアプリのバージョンをスワップする][2]</span><span class="sxs-lookup"><span data-stu-id="f5837-116">[Swap between app versions with deployment slots][2]</span></span>  
<span data-ttu-id="f5837-117">[カスタム ドメインを構成する][3] </span><span class="sxs-lookup"><span data-stu-id="f5837-117">[Configure a custom domain][3] </span></span>  
<span data-ttu-id="f5837-118">[複数のリージョンに Web アプリをスケーリングする][4]</span><span class="sxs-lookup"><span data-stu-id="f5837-118">[Scale a web app across multiple regions][4]</span></span>   

<span data-ttu-id="f5837-119">アプリで利用できる [Azure App Service のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="f5837-119">Explore more [sample Java code for Azure App Service](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice) you can use in your apps.</span></span>

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
