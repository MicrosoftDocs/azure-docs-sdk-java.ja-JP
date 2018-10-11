---
title: Azure Management Libraries for Java Web アプリ サンプル
description: Azure Management Libraries for Java を使用して、App Service でホストされる Azure Web アプリの作成と更新を行うサンプル コードを入手しましょう。
keywords: Azure, Java, SDK, API, Maven, Gradle, Web アプリ, App Service
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.openlocfilehash: 2f1e43f3835ffcdb138bf7e29a1656b7ee381281
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893033"
---
# <a name="azure-management-libraries-for-java-samples-for-web-apps"></a><span data-ttu-id="31bbf-104">Azure Management Libraries for Java の Web アプリ サンプル</span><span class="sxs-lookup"><span data-stu-id="31bbf-104">Azure management libraries for Java samples for web apps</span></span>

| <span data-ttu-id="31bbf-105">**アプリの作成**</span><span class="sxs-lookup"><span data-stu-id="31bbf-105">**Create an app**</span></span> ||
|---|---|
| <span data-ttu-id="31bbf-106">[Web アプリを作成して FTP または GitHub からデプロイする][1]</span><span class="sxs-lookup"><span data-stu-id="31bbf-106">[Create a web app and deploy from FTP or GitHub][1]</span></span> | <span data-ttu-id="31bbf-107">ローカル Git、FTP、GitHub の継続的インテグレーションから Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="31bbf-107">Deploy web apps from local Git, FTP, and continuous integration from GitHub.</span></span> |
| <span data-ttu-id="31bbf-108">[Web アプリを作成してデプロイ スロットを管理する][2]</span><span class="sxs-lookup"><span data-stu-id="31bbf-108">[Create a web app and manage deployment slots][2]</span></span> | <span data-ttu-id="31bbf-109">Web アプリを作成してステージング スロットにデプロイした後、スロット間でデプロイをスワップします。</span><span class="sxs-lookup"><span data-stu-id="31bbf-109">Create a web app and deploy to staging slots, and then swap deployments between slots.</span></span> |
| <span data-ttu-id="31bbf-110">**アプリケーションの構成**</span><span class="sxs-lookup"><span data-stu-id="31bbf-110">**Configure app**</span></span> ||
| <span data-ttu-id="31bbf-111">[Web アプリを作成してカスタム ドメインを構成する][3]</span><span class="sxs-lookup"><span data-stu-id="31bbf-111">[Create a web app and configure a custom domain][3]</span></span> | <span data-ttu-id="31bbf-112">カスタム ドメインと自己署名 SSL 証明書を使った Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="31bbf-112">Create a web app with a custom domain and self-signed SSL certificate.</span></span> |
| <span data-ttu-id="31bbf-113">**アプリのスケール**</span><span class="sxs-lookup"><span data-stu-id="31bbf-113">**Scale apps**</span></span> ||
| <span data-ttu-id="31bbf-114">[複数のリージョンにわたる高可用性によって Web アプリをスケーリングする][4]</span><span class="sxs-lookup"><span data-stu-id="31bbf-114">[Scale a web app with high availability across multiple regions][4]</span></span> | <span data-ttu-id="31bbf-115">3 つの異なる地理的リージョンに Web アプリをスケーリングし、Azure Traffic Manager を使用して、1 つのエンドポイントを介して利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="31bbf-115">Scale a web app in three different geographical regions and make them available through a single endpoint using Azure Traffic Manager.</span></span> | 
| <span data-ttu-id="31bbf-116">**アプリのリソースへの接続**</span><span class="sxs-lookup"><span data-stu-id="31bbf-116">**Connect app to resources**</span></span> ||
| <span data-ttu-id="31bbf-117">[Web アプリをストレージ アカウントに接続する][5]</span><span class="sxs-lookup"><span data-stu-id="31bbf-117">[Connect a web app to a storage account][5]</span></span> | <span data-ttu-id="31bbf-118">Azure ストレージ アカウントを作成し、そのストレージ アカウントの接続文字列をアプリケーション設定に追加します。</span><span class="sxs-lookup"><span data-stu-id="31bbf-118">Create an Azure storage account and add the storage account connection string to the app settings.</span></span> |
| <span data-ttu-id="31bbf-119">[Web アプリを SQL データベースに接続する][6]</span><span class="sxs-lookup"><span data-stu-id="31bbf-119">[Connect a web app to a SQL database][6]</span></span> | <span data-ttu-id="31bbf-120">Web アプリと SQL データベースを作成し、SQL データベースの接続文字列をアプリケーション設定に追加します。</span><span class="sxs-lookup"><span data-stu-id="31bbf-120">Create a web app and SQL database, and then add the SQL database connection string to the app settings.</span></span> |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://azure.microsoft.com/resources/samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://azure.microsoft.com/resources/samples/app-service-java-manage-data-connections-for-web-apps/