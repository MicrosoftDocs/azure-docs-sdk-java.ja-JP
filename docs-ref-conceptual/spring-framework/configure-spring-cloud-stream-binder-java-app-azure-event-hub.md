---
title: Azure Event Hubs を使用する Spring Cloud Stream Binder アプリケーションを作成する方法
description: Spring Boot Initializr を使用して作成された Java ベースの Spring Cloud Stream Binder アプリケーションを、Azure Event Hubs を使用するように構成する方法について説明します。
services: event-hubs
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: na
ms.openlocfilehash: dfc3b6121bddcb637735047e2e7bc7485da9a4fe
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799948"
---
# <a name="how-to-create-a-spring-cloud-stream-binder-application-with-azure-event-hubs"></a><span data-ttu-id="7b470-103">Azure Event Hubs を使用する Spring Cloud Stream Binder アプリケーションを作成する方法</span><span class="sxs-lookup"><span data-stu-id="7b470-103">How to create a Spring Cloud Stream Binder application with Azure Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="7b470-104">概要</span><span class="sxs-lookup"><span data-stu-id="7b470-104">Overview</span></span>

<span data-ttu-id="7b470-105">この記事では、Spring Boot Initializr を使用して作成された Java ベースの Spring Cloud Stream Binder アプリケーションを、Azure Event Hubs を使用するように構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7b470-105">This article demonstrates how to configure a Java-based Spring Cloud Stream Binder application created with the Spring Boot Initializer with Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b470-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="7b470-106">Prerequisites</span></span>

<span data-ttu-id="7b470-107">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="7b470-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="7b470-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="7b470-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="7b470-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="7b470-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="7b470-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="7b470-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="7b470-111">この記事の手順を完了するには、Spring Boot 2.0 以上のバージョンが必要です。</span><span class="sxs-lookup"><span data-stu-id="7b470-111">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a><span data-ttu-id="7b470-112">Azure portal を使用して Azure イベント ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-112">Create an Azure Event Hub using the Azure portal</span></span>

### <a name="create-an-azure-event-hub-namespace"></a><span data-ttu-id="7b470-113">Azure イベント ハブの名前空間を作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-113">Create an Azure Event Hub Namespace</span></span>

1. <span data-ttu-id="7b470-114">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="7b470-114">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="7b470-115">**[+ リソースの作成]** をクリックし、**[モノのインターネット]**、**[Event Hubs]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="7b470-115">Click **+Create a resource**, then **Internet of Things**, and then click **Event Hubs**.</span></span>

   ![Azure イベント ハブの名前空間を作成する][IMG01]

1. <span data-ttu-id="7b470-117">**[名前空間の作成]** ページで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="7b470-117">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="7b470-118">一意の**名前**を入力します。この名前は、イベント ハブの名前空間の URI の一部になります。</span><span class="sxs-lookup"><span data-stu-id="7b470-118">Enter a unique **Name**, which will become part of the URI for your event hub namespace.</span></span> <span data-ttu-id="7b470-119">たとえば、**[名前]** に「**wingtiptoys**」と入力した場合、URI は *wingtiptoys.servicebus.windows.net* になります。</span><span class="sxs-lookup"><span data-stu-id="7b470-119">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.servicebus.windows.net*.</span></span>
   * <span data-ttu-id="7b470-120">イベント ハブの名前空間の**価格レベル**を選択します。</span><span class="sxs-lookup"><span data-stu-id="7b470-120">Choose a **Pricing tier** for your event hub namespace.</span></span>
   * <span data-ttu-id="7b470-121">名前空間に使用する**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="7b470-121">Choose the **Subscription** you want to use for your namespace.</span></span>
   * <span data-ttu-id="7b470-122">名前空間の新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-122">Specify whether to create a new **Resource group** for your namespace, or choose an existing resource group.</span></span>
   * <span data-ttu-id="7b470-123">イベント ハブの名前空間の**場所**を指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-123">Specify the **Location** for your event hub namespace.</span></span>

   ![Azure イベント ハブの名前空間のオプションを指定する][IMG02]

1. <span data-ttu-id="7b470-125">上記のオプションを指定したら、**[作成]** をクリックして名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="7b470-125">When you have specified the options listed above, click **Create** to create your namespace.</span></span>

### <a name="create-an-azure-event-hub-in-your-namespace"></a><span data-ttu-id="7b470-126">名前空間に Azure イベント ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-126">Create an Azure Event Hub in your namespace</span></span>

1. <span data-ttu-id="7b470-127">Azure portal (<https://portal.azure.com/>) を参照します。</span><span class="sxs-lookup"><span data-stu-id="7b470-127">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="7b470-128">**[すべてのリソース]** をクリックし、作成した名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7b470-128">Click **All resources**, and then click the namespace that you created.</span></span>

   ![Azure イベント ハブの名前空間を選択する][IMG03]

1. <span data-ttu-id="7b470-130">**[Event Hubs]** をクリックし、**[+ イベント ハブ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7b470-130">Click **Event Hubs**, and then click **+Event Hub**.</span></span>

   ![新しい Azure イベント ハブを追加する][IMG04]

1. <span data-ttu-id="7b470-132">**[イベント ハブの作成]** ページで、イベント ハブの一意の**名前**を入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7b470-132">On the **Create Event Hub** page, enter a unique **Name** for your Event Hub, and then click **Create**.</span></span>

   ![Azure Event Hub の作成][IMG05]

1. <span data-ttu-id="7b470-134">イベント ハブが作成されると、**[Event Hubs]** ページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="7b470-134">When your Event Hub has been created, it will be listed on the **Event Hubs** page.</span></span>

   ![Azure Event Hub の作成][IMG06]

### <a name="create-an-azure-storage-account-for-your-event-hub-checkpoints"></a><span data-ttu-id="7b470-136">イベント ハブのチェックポイント用の Azure ストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-136">Create an Azure Storage Account for your Event Hub checkpoints</span></span>

1. <span data-ttu-id="7b470-137">Azure portal (<https://portal.azure.com/>) を参照します。</span><span class="sxs-lookup"><span data-stu-id="7b470-137">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="7b470-138">**[+ リソースの作成]** をクリックし、**[Storage]**、**[ストレージ アカウント]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="7b470-138">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![Azure ストレージ アカウントを作成する][IMG07]

1. <span data-ttu-id="7b470-140">**[名前空間の作成]** ページで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="7b470-140">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="7b470-141">一意の**名前**を入力します。この名前は、ストレージ アカウントの URI の一部になります。</span><span class="sxs-lookup"><span data-stu-id="7b470-141">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="7b470-142">たとえば、**[名前]** に「**wingtiptoys**」と入力した場合、URI は *wingtiptoys.core.windows.net* になります。</span><span class="sxs-lookup"><span data-stu-id="7b470-142">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.core.windows.net*.</span></span>
   * <span data-ttu-id="7b470-143">**[アカウントの種類]** で **[Blob Storage]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7b470-143">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="7b470-144">ストレージ アカウントの**場所**を指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-144">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="7b470-145">ストレージ アカウントに使用する**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="7b470-145">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="7b470-146">ストレージ アカウントの新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-146">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>

   ![Azure ストレージ アカウントのオプションを指定する][IMG08]

1. <span data-ttu-id="7b470-148">上記のオプションを指定したら、**[作成]** をクリックしてストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7b470-148">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="7b470-149">Spring Initializr でシンプルな Spring Boot アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-149">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="7b470-150"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="7b470-150">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="7b470-151">次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-151">Specify the following options:</span></span>

   * <span data-ttu-id="7b470-152">**Java** で **Maven** プロジェクトを生成します。</span><span class="sxs-lookup"><span data-stu-id="7b470-152">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="7b470-153">**Spring Boot** のバージョンとして、2.0 以上を指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-153">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="7b470-154">アプリケーションの**グループ (Group)** と**成果物 (Artifact)** の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-154">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="7b470-155">**Web** 依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="7b470-155">Add the **Web** dependency.</span></span>

      ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="7b470-157">Spring Initializr では、**グループ (Group)** と**成果物 (Artifact)** の名前を使用してパッケージ名を作成します (例: *com.wingtiptoys.eventhub*)。</span><span class="sxs-lookup"><span data-stu-id="7b470-157">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.eventhub*.</span></span>
   >

1. <span data-ttu-id="7b470-158">上記のオプションを指定したら、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7b470-158">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="7b470-159">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="7b470-159">When prompted, download the project to a path on your local computer.</span></span>

   ![Spring プロジェクトをダウンロードする][SI02]

1. <span data-ttu-id="7b470-161">ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="7b470-161">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-event-hub-starter"></a><span data-ttu-id="7b470-162">Azure Event Hub スターターを使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="7b470-162">Configure your Spring Boot app to use the Azure Event Hub starter</span></span>

1. <span data-ttu-id="7b470-163">アプリのルート ディレクトリで *pom.xml* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-163">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\pom.xml`

   <span data-ttu-id="7b470-164">または</span><span class="sxs-lookup"><span data-stu-id="7b470-164">-or-</span></span>

   `/users/example/home/eventhub/pom.xml`

1. <span data-ttu-id="7b470-165">テキスト エディターで *pom.xml* ファイルを開き、Spring Cloud Azure Event Hub Stream Binder スターターを `<dependencies>` のリストに追加します。</span><span class="sxs-lookup"><span data-stu-id="7b470-165">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Event Hub Stream Binder starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-eventhub-stream-binder</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![pom.xml ファイルを編集する][SI03]

1. <span data-ttu-id="7b470-167">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7b470-167">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="7b470-168">Azure 資格情報ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-168">Create an Azure Credential File</span></span>

1. <span data-ttu-id="7b470-169">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="7b470-169">Open a command prompt.</span></span>

1. <span data-ttu-id="7b470-170">Spring Boot アプリの *resources* ディレクトリに移動します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-170">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   <span data-ttu-id="7b470-171">または</span><span class="sxs-lookup"><span data-stu-id="7b470-171">-or-</span></span>

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. <span data-ttu-id="7b470-172">Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="7b470-172">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="7b470-173">サブスクリプションを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-173">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="7b470-174">Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-174">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the subscription you want to use with Azure; for example:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. <span data-ttu-id="7b470-175">Azure 資格情報ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7b470-175">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="7b470-176">このコマンドにより、*resources* ディレクトリに、次の例のような内容の *my.azureauth* ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7b470-176">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a><span data-ttu-id="7b470-177">Azure イベント ハブを使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="7b470-177">Configure your Spring Boot app to use your Azure Event Hub</span></span>

1. <span data-ttu-id="7b470-178">アプリの *resources* ディレクトリで *application.properties* を探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-178">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   <span data-ttu-id="7b470-179">または</span><span class="sxs-lookup"><span data-stu-id="7b470-179">-or-</span></span>

   `/users/example/home/eventhub/src/main/resources/application.properties`

2. <span data-ttu-id="7b470-180">テキスト エディターで *application.properties* ファイルを開きます。次の行を追加し、サンプルの値をイベント ハブの適切なプロパティに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7b470-180">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your event hub:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace
   spring.cloud.azure.eventhub.checkpoint-storage-account=wingtiptoysstorage
   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   spring.cloud.stream.eventhub.bindings.input.consumer.checkpoint-mode=MANUAL
   ```
   <span data-ttu-id="7b470-181">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="7b470-181">Where:</span></span>

   |                          <span data-ttu-id="7b470-182">フィールド</span><span class="sxs-lookup"><span data-stu-id="7b470-182">Field</span></span>                           |                                                                                   <span data-ttu-id="7b470-183">説明</span><span class="sxs-lookup"><span data-stu-id="7b470-183">Description</span></span>                                                                                    |
   |----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |        `spring.cloud.azure.credential-file-path`         |                                                    <span data-ttu-id="7b470-184">このチュートリアルで作成した Azure 資格情報ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-184">Specifies Azure credential file that you created earlier in this tutorial.</span></span>                                                    |
   |           `spring.cloud.azure.resource-group`            |                                                      <span data-ttu-id="7b470-185">Azure イベント ハブを含む Azure リソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-185">Specifies the Azure Resource Group that contains your Azure Event Hub.</span></span>                                                      |
   |               `spring.cloud.azure.region`                |                                           <span data-ttu-id="7b470-186">Azure イベント ハブの作成時に指定した地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-186">Specifies the geographical region that you specified when you created your Azure Event Hub.</span></span>                                            |
   |         `spring.cloud.azure.eventhub.namespace`          |                                          <span data-ttu-id="7b470-187">Azure イベント ハブの名前空間の作成時に指定した一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-187">Specifies the unique name that you specified when you created your Azure Event Hub Namespace.</span></span>                                           |
   | `spring.cloud.azure.eventhub.checkpoint-storage-account` |                                                    <span data-ttu-id="7b470-188">このチュートリアルで作成した Azure ストレージ アカウントを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-188">Specifies Azure Storage Account that you created earlier in this tutorial.</span></span>                                                    |
   |     `spring.cloud.stream.bindings.input.destination`     |                            <span data-ttu-id="7b470-189">入力先の Azure イベント ハブを指定します。ここでは、このチュートリアルで作成したハブを指定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-189">Specifies the input destination Azure Event Hub, which for this tutorial is the  hub you created earlier in this tutorial.</span></span>                            |
   |       `spring.cloud.stream.bindings.input.group `        | <span data-ttu-id="7b470-190">Azure イベント ハブのコンシューマー グループを指定します。Azure イベント ハブの作成時に作成された基本コンシューマー グループを使用するには、"$ Default" に設定します。</span><span class="sxs-lookup"><span data-stu-id="7b470-190">Specifies a Consumer Group from Azure Event Hub, which can be set to '$Default' in order to use the basic consumer group that was created when you created your Azure Event Hub.</span></span> |
   |    `spring.cloud.stream.bindings.output.destination`     |                               <span data-ttu-id="7b470-191">出力先の Azure イベント ハブを指定します。ここでは、入力先と同じものになります。</span><span class="sxs-lookup"><span data-stu-id="7b470-191">Specifies the output destination Azure Event Hub, which for this tutorial will be the same as the input destination.</span></span>                               |


3. <span data-ttu-id="7b470-192">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7b470-192">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a><span data-ttu-id="7b470-193">イベント ハブの基本的な機能を実装するサンプル コードを追加する</span><span class="sxs-lookup"><span data-stu-id="7b470-193">Add sample code to implement basic event hub functionality</span></span>

<span data-ttu-id="7b470-194">このセクションでは、イベント ハブにイベントを送信するために必要な Java クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7b470-194">In this section, you create the necessary Java classes for sending events to your event hub.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="7b470-195">アプリケーションのメイン クラスを変更する</span><span class="sxs-lookup"><span data-stu-id="7b470-195">Modify the main application class</span></span>

1. <span data-ttu-id="7b470-196">アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-196">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\java\com\wingtiptoys\eventhub\EventhubApplication.java`

   <span data-ttu-id="7b470-197">または</span><span class="sxs-lookup"><span data-stu-id="7b470-197">-or-</span></span>

   `/users/example/home/eventhub/src/main/java/com/wingtiptoys/eventhub/EventhubApplication.java`

1. <span data-ttu-id="7b470-198">テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7b470-198">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.eventhub;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class EventhubApplication {
      public static void main(String[] args) {
         SpringApplication.run(EventhubApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="7b470-199">メイン アプリケーションの Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7b470-199">Save and close the main application Java file.</span></span>

### <a name="create-a-new-class-for-the-source-connector"></a><span data-ttu-id="7b470-200">ソース コネクタの新しいクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-200">Create a new class for the source connector</span></span>

1. <span data-ttu-id="7b470-201">アプリのパッケージ ディレクトリに *EventhubSource.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7b470-201">Create a new Java file named *EventhubSource.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.eventhub;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   @EnableBinding(Source.class)
   @RestController
   public class EventhubSource {

      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String postMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```
1. <span data-ttu-id="7b470-202">*EventhubSource.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7b470-202">Save and close the *EventhubSource.java* file.</span></span>

### <a name="create-a-new-class-for-the-sink-connector"></a><span data-ttu-id="7b470-203">シンク コネクタの新しいクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-203">Create a new class for the sink connector</span></span>

1. <span data-ttu-id="7b470-204">アプリのパッケージ ディレクトリに *EventhubSink.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7b470-204">Create a new Java file named *EventhubSink.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.eventhub;

   import com.microsoft.azure.spring.integration.core.AzureHeaders;
   import com.microsoft.azure.spring.integration.core.api.Checkpointer;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   import org.springframework.messaging.handler.annotation.Header;

   @EnableBinding(Sink.class)
   public class EventhubSink {

      private static final Logger LOGGER = LoggerFactory.getLogger(EventhubSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
         LOGGER.info("New message received: '{}'", message);
         checkpointer.success().handle((r, ex) -> {
            if (ex == null) {
               LOGGER.info("Message '{}' successfully checkpointed", message);
            }
            return null;
         });
      }
   }
   ```

1. <span data-ttu-id="7b470-205">*EventhubSink.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="7b470-205">Save and close the *EventhubSink.java* file.</span></span>

## <a name="build-and-test-your-application"></a><span data-ttu-id="7b470-206">アプリケーションをビルドしてテストする</span><span class="sxs-lookup"><span data-stu-id="7b470-206">Build and test your application</span></span>

1. <span data-ttu-id="7b470-207">コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-207">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\eventhub`

   <span data-ttu-id="7b470-208">または</span><span class="sxs-lookup"><span data-stu-id="7b470-208">-or-</span></span>

   `cd /users/example/home/eventhub`

1. <span data-ttu-id="7b470-209">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-209">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="7b470-210">アプリケーションが実行されたら、*curl* を使用してアプリケーションをテストできます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b470-210">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   <span data-ttu-id="7b470-211">アプリケーションのログに送信された "hello" が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7b470-211">You should see "hello" posted to your application's logs.</span></span> <span data-ttu-id="7b470-212">例: </span><span class="sxs-lookup"><span data-stu-id="7b470-212">For example:</span></span>

   ```shell
   [Thread-13] INFO com.wingtiptoys.eventhub.EventhubSink - New message received: 'hello'
   [pool-10-thread-7] INFO com.wingtiptoys.eventhub.EventhubSink - Message 'hello' successfully checkpointed
   ```

## <a name="next-steps"></a><span data-ttu-id="7b470-213">次の手順</span><span class="sxs-lookup"><span data-stu-id="7b470-213">Next steps</span></span>

<span data-ttu-id="7b470-214">Azure による Event Hub Stream Binder のサポートの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7b470-214">For more information about Azure support for Event Hub Stream Binder, see the following articles:</span></span>

* [<span data-ttu-id="7b470-215">Azure Event Hubs とは</span><span class="sxs-lookup"><span data-stu-id="7b470-215">What is Azure Event Hubs?</span></span>](/azure/event-hubs/event-hubs-about)

* [<span data-ttu-id="7b470-216">Azure portal を使用して Event Hubs 名前空間とイベント ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="7b470-216">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>](/azure/event-hubs/event-hubs-create)

* [<span data-ttu-id="7b470-217">Azure Event Hubs で Apache Kafka 用 Spring Boot Starter を使用する方法</span><span class="sxs-lookup"><span data-stu-id="7b470-217">How to use the Spring Boot Starter for Apache Kafka with Azure Event Hubs</span></span>](configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub.md)

<span data-ttu-id="7b470-218">Java での Azure の使用の詳細については、「Java 開発者向けの Azure」および [Visual Studio Team Services 用の Java ツール]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7b470-218">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="7b470-219">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="7b470-219">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="7b470-220">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="7b470-220">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="7b470-221">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="7b470-221">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="7b470-222">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="7b470-222">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-03.png
