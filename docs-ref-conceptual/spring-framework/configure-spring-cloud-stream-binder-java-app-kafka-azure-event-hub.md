---
title: Azure Event Hubs で Apache Kafka 用 Spring Boot Starter を使用する方法
description: Spring Boot Initializer を使用して作成されたアプリケーションを、Apache Kafka と Azure Event Hubs を使用するように構成する方法について説明します。
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
ms.openlocfilehash: 00062f5442e072af30036388f2f1f066221d7316
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506584"
---
# <a name="how-to-use-the-spring-boot-starter-for-apache-kafka-with-azure-event-hubs"></a><span data-ttu-id="46540-103">Azure Event Hubs で Apache Kafka 用 Spring Boot Starter を使用する方法</span><span class="sxs-lookup"><span data-stu-id="46540-103">How to use the Spring Boot Starter for Apache Kafka with Azure Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="46540-104">概要</span><span class="sxs-lookup"><span data-stu-id="46540-104">Overview</span></span>

<span data-ttu-id="46540-105">この記事では、Spring Boot Initializr を使用して作成された Java ベースの Spring Cloud Stream Binder を、[Apache Kafka] と Azure Event Hubs を使用するように構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="46540-105">This article demonstrates how to configure a Java-based Spring Cloud Stream Binder created with the Spring Boot Initializer to use [Apache Kafka] with Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46540-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="46540-106">Prerequisites</span></span>

<span data-ttu-id="46540-107">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="46540-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="46540-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="46540-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="46540-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="46540-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="46540-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="46540-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="46540-111">この記事の手順を完了するには、Spring Boot 2.0 以上のバージョンが必要です。</span><span class="sxs-lookup"><span data-stu-id="46540-111">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a><span data-ttu-id="46540-112">Azure portal を使用して Azure イベント ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="46540-112">Create an Azure Event Hub using the Azure portal</span></span>

### <a name="create-an-azure-event-hub-namespace"></a><span data-ttu-id="46540-113">Azure イベント ハブの名前空間を作成する</span><span class="sxs-lookup"><span data-stu-id="46540-113">Create an Azure Event Hub Namespace</span></span>

1. <span data-ttu-id="46540-114">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="46540-114">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="46540-115">**[+ リソースの作成]** をクリックし、**[モノのインターネット]**、**[Event Hubs]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="46540-115">Click **+Create a resource**, then **Internet of Things**, and then click **Event Hubs**.</span></span>

   ![Azure イベント ハブの名前空間を作成する][IMG01]

1. <span data-ttu-id="46540-117">**[名前空間の作成]** ページで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="46540-117">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="46540-118">一意の**名前**を入力します。この名前は、イベント ハブの名前空間の URI の一部になります。</span><span class="sxs-lookup"><span data-stu-id="46540-118">Enter a unique **Name**, which will become part of the URI for your event hub namespace.</span></span> <span data-ttu-id="46540-119">たとえば、**[名前]** に「**wingtiptoys**」と入力した場合、URI は *wingtiptoys.servicebus.windows.net* になります。</span><span class="sxs-lookup"><span data-stu-id="46540-119">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.servicebus.windows.net*.</span></span>
   * <span data-ttu-id="46540-120">イベント ハブの名前空間の**価格レベル**を選択します。</span><span class="sxs-lookup"><span data-stu-id="46540-120">Choose a **Pricing tier** for your event hub namespace.</span></span>
   * <span data-ttu-id="46540-121">名前空間に対して **[Kafka を有効にする]** を指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-121">Specify **Enable Kafka** for your namespace.</span></span>
   * <span data-ttu-id="46540-122">名前空間に使用する**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="46540-122">Choose the **Subscription** you want to use for your namespace.</span></span>
   * <span data-ttu-id="46540-123">名前空間の新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-123">Specify whether to create a new **Resource group** for your namespace, or choose an existing resource group.</span></span>
   * <span data-ttu-id="46540-124">イベント ハブの名前空間の**場所**を指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-124">Specify the **Location** for your event hub namespace.</span></span>
   
   ![Azure イベント ハブの名前空間のオプションを指定する][IMG02]

1. <span data-ttu-id="46540-126">上記のオプションを指定したら、**[作成]** をクリックして名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="46540-126">When you have specified the options listed above, click **Create** to create your namespace.</span></span>

### <a name="create-an-azure-event-hub-in-your-namespace"></a><span data-ttu-id="46540-127">名前空間に Azure イベント ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="46540-127">Create an Azure Event Hub in your namespace</span></span>

1. <span data-ttu-id="46540-128">Azure portal (<https://portal.azure.com/>) を参照します。</span><span class="sxs-lookup"><span data-stu-id="46540-128">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="46540-129">**[すべてのリソース]** をクリックし、作成した名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="46540-129">Click **All resources**, and then click the namespace that you created.</span></span>

   ![Azure イベント ハブの名前空間を選択する][IMG03]

1. <span data-ttu-id="46540-131">**[Event Hubs]** をクリックし、**[+ イベント ハブ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="46540-131">Click **Event Hubs**, and then click **+Event Hub**.</span></span>

   ![新しい Azure イベント ハブを追加する][IMG04]

1. <span data-ttu-id="46540-133">**[イベント ハブの作成]** ページで、イベント ハブの一意の**名前**を入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="46540-133">On the **Create Event Hub** page, enter a unique **Name** for your Event Hub, and then click **Create**.</span></span>

   ![Azure Event Hub の作成][IMG05]

1. <span data-ttu-id="46540-135">イベント ハブが作成されると、**[Event Hubs]** ページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="46540-135">When your Event Hub has been created, it will be listed on the **Event Hubs** page.</span></span>

   ![Azure Event Hub の作成][IMG06]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="46540-137">Spring Initializr でシンプルな Spring Boot アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="46540-137">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="46540-138"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="46540-138">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="46540-139">次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-139">Specify the following options:</span></span>

   * <span data-ttu-id="46540-140">**Java** で **Maven** プロジェクトを生成します。</span><span class="sxs-lookup"><span data-stu-id="46540-140">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="46540-141">**Spring Boot** のバージョンとして、2.0 以上を指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-141">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="46540-142">アプリケーションの**グループ (Group)** と**成果物 (Artifact)** の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-142">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="46540-143">**Web** 依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="46540-143">Add the **Web** dependency.</span></span>

      ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="46540-145">Spring Initializr では、**グループ (Group)** と**成果物 (Artifact)** の名前を使用してパッケージ名を作成します (例: *com.wingtiptoys.kafka*)。</span><span class="sxs-lookup"><span data-stu-id="46540-145">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.kafka*.</span></span>
   >

1. <span data-ttu-id="46540-146">上記のオプションを指定したら、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="46540-146">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="46540-147">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="46540-147">When prompted, download the project to a path on your local computer.</span></span>

   ![Spring プロジェクトをダウンロードする][SI02]

1. <span data-ttu-id="46540-149">ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="46540-149">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-spring-cloud-kafka-stream-and-azure-event-hub-starters"></a><span data-ttu-id="46540-150">Spring Cloud の Kafka Stream スターターと Azure Event Hub スターターを使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="46540-150">Configure your Spring Boot app to use the Spring Cloud Kafka Stream and Azure Event Hub starters</span></span>

1. <span data-ttu-id="46540-151">アプリのルート ディレクトリで *pom.xml* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-151">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\kafka\pom.xml`

   <span data-ttu-id="46540-152">または</span><span class="sxs-lookup"><span data-stu-id="46540-152">-or-</span></span>

   `/users/example/home/kafka/pom.xml`

1. <span data-ttu-id="46540-153">テキスト エディターで *pom.xml* ファイルを開き、Spring Cloud の Kafka Stream スターターと Azure Event Hub スターターを `<dependencies>` のリストに追加します。</span><span class="sxs-lookup"><span data-stu-id="46540-153">Open the *pom.xml* file in a text editor, and add the Spring Cloud Kafka Stream and Azure Event Hub starters to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-stream-kafka</artifactId>
      <version>2.0.1.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-starter-eventhub</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![pom.xml ファイルを編集する][SI03]

1. <span data-ttu-id="46540-155">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="46540-155">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="46540-156">Azure 資格情報ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="46540-156">Create an Azure Credential File</span></span>

1. <span data-ttu-id="46540-157">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="46540-157">Open a command prompt.</span></span>

1. <span data-ttu-id="46540-158">Spring Boot アプリの *resources* ディレクトリに移動します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-158">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   <span data-ttu-id="46540-159">または</span><span class="sxs-lookup"><span data-stu-id="46540-159">-or-</span></span>

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. <span data-ttu-id="46540-160">Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="46540-160">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="46540-161">サブスクリプションを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="46540-161">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="46540-162">Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-162">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="46540-163">Azure 資格情報ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="46540-163">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="46540-164">このコマンドにより、*resources* ディレクトリに、次の例のような内容の *my.azureauth* ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="46540-164">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a><span data-ttu-id="46540-165">Azure イベント ハブを使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="46540-165">Configure your Spring Boot app to use your Azure Event Hub</span></span>

1. <span data-ttu-id="46540-166">アプリの *resources* ディレクトリで *application.properties* を探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-166">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   <span data-ttu-id="46540-167">または</span><span class="sxs-lookup"><span data-stu-id="46540-167">-or-</span></span>

   `/users/example/home/eventhub/src/main/resources/application.properties`

1.  <span data-ttu-id="46540-168">テキスト エディターで *application.properties* ファイルを開きます。次の行を追加し、サンプルの値をイベント ハブの適切なプロパティに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="46540-168">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your event hub:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace

   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   <span data-ttu-id="46540-169">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="46540-169">Where:</span></span>
   | <span data-ttu-id="46540-170">フィールド</span><span class="sxs-lookup"><span data-stu-id="46540-170">Field</span></span> | <span data-ttu-id="46540-171">説明</span><span class="sxs-lookup"><span data-stu-id="46540-171">Description</span></span> |
   | ---|---|
   | `spring.cloud.azure.credential-file-path` | <span data-ttu-id="46540-172">このチュートリアルで作成した Azure 資格情報ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-172">Specifies Azure credential file that you created earlier in this tutorial.</span></span> |
   | `spring.cloud.azure.resource-group` | <span data-ttu-id="46540-173">Azure イベント ハブを含む Azure リソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-173">Specifies the Azure Resource Group that contains your Azure Event Hub.</span></span> |
   | `spring.cloud.azure.region` | <span data-ttu-id="46540-174">Azure イベント ハブの作成時に指定した地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-174">Specifies the geographical region that you specified when you created your Azure Event Hub.</span></span> |
   | `spring.cloud.azure.eventhub.namespace` | <span data-ttu-id="46540-175">Azure イベント ハブの名前空間の作成時に指定した一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-175">Specifies the unique name that you specified when you created your Azure Event Hub Namespace.</span></span> |
   | `spring.cloud.stream.bindings.input.destination` | <span data-ttu-id="46540-176">入力先の Azure イベント ハブを指定します。ここでは、このチュートリアルで作成したハブを指定します。</span><span class="sxs-lookup"><span data-stu-id="46540-176">Specifies the input destination Azure Event Hub, which for this tutorial is the  hub you created earlier in this tutorial.</span></span> |
   | `spring.cloud.stream.bindings.input.group `| <span data-ttu-id="46540-177">Azure イベント ハブのコンシューマー グループを指定します。Azure イベント ハブの作成時に作成された基本コンシューマー グループを使用するには、 "$ Default" に設定します。</span><span class="sxs-lookup"><span data-stu-id="46540-177">Specifies a Consumer Group from Azure Event Hub, which can be set to '$Default' in order to use the basic consumer group that was created when you created your Azure Event Hub.</span></span> |
   | `spring.cloud.stream.bindings.output.destination` | <span data-ttu-id="46540-178">出力先の Azure イベント ハブを指定します。ここでは、入力先と同じものになります。</span><span class="sxs-lookup"><span data-stu-id="46540-178">Specifies the output destination Azure Event Hub, which for this tutorial will be the same as the input destination.</span></span> |

1. <span data-ttu-id="46540-179">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="46540-179">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a><span data-ttu-id="46540-180">イベント ハブの基本的な機能を実装するサンプル コードを追加する</span><span class="sxs-lookup"><span data-stu-id="46540-180">Add sample code to implement basic event hub functionality</span></span>

<span data-ttu-id="46540-181">このセクションでは、イベント ハブにイベントを送信するために必要な Java クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="46540-181">In this section, you create the necessary Java classes for sending events to your event hub.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="46540-182">アプリケーションのメイン クラスを変更する</span><span class="sxs-lookup"><span data-stu-id="46540-182">Modify the main application class</span></span>

1. <span data-ttu-id="46540-183">アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-183">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\kafka\src\main\java\com\wingtiptoys\kafka\KafkaApplication.java`

   <span data-ttu-id="46540-184">または</span><span class="sxs-lookup"><span data-stu-id="46540-184">-or-</span></span>

   `/users/example/home/kafka/src/main/java/com/wingtiptoys/kafka/KafkaApplication.java`

1. <span data-ttu-id="46540-185">テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="46540-185">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.kafka;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class KafkaApplication {
      public static void main(String[] args) {
         SpringApplication.run(KafkaApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="46540-186">メイン アプリケーションの Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="46540-186">Save and close the main application Java file.</span></span>


### <a name="create-a-new-class-for-the-source-connector"></a><span data-ttu-id="46540-187">ソース コネクタの新しいクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="46540-187">Create a new class for the source connector</span></span>

1. <span data-ttu-id="46540-188">アプリのパッケージ ディレクトリに *KafkaSource.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="46540-188">Create a new Java file named *KafkaSource.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.kafka;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;
   
   @EnableBinding(Source.class)
   @RestController
   public class KafkaSource {
      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String sendMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```

1. <span data-ttu-id="46540-189">*KafkaSource.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="46540-189">Save and close the *KafkaSource.java* file.</span></span>

### <a name="create-a-new-class-for-the-sink-connector"></a><span data-ttu-id="46540-190">シンク コネクタの新しいクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="46540-190">Create a new class for the sink connector</span></span>

1. <span data-ttu-id="46540-191">アプリのパッケージ ディレクトリに *KafkaSink.java* という名前の新しい Java ファイルを作成し、テキスト エディターでファイルを開いて、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="46540-191">Create a new Java file named *KafkaSink.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.kafka;
   
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   
   @EnableBinding(Sink.class)
   public class KafkaSink {
      private static final Logger LOGGER = LoggerFactory.getLogger(KafkaSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message) {
         LOGGER.info("New message received: " + message);
      }
   }
   ```

1. <span data-ttu-id="46540-192">*KafkaSink.java* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="46540-192">Save and close the *KafkaSink.java* file.</span></span>

## <a name="build-and-test-your-application"></a><span data-ttu-id="46540-193">アプリケーションをビルドしてテストする</span><span class="sxs-lookup"><span data-stu-id="46540-193">Build and test your application</span></span>

1. <span data-ttu-id="46540-194">コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-194">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\kafka`

   <span data-ttu-id="46540-195">または</span><span class="sxs-lookup"><span data-stu-id="46540-195">-or-</span></span>

   `cd /users/example/home/kafka`

1. <span data-ttu-id="46540-196">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-196">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="46540-197">アプリケーションが実行されたら、*curl* を使用してアプリケーションをテストできます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="46540-197">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   <span data-ttu-id="46540-198">アプリケーションのログに送信された "hello" が表示されます。</span><span class="sxs-lookup"><span data-stu-id="46540-198">You should see "hello" posted to your application's logs.</span></span> <span data-ttu-id="46540-199">例: </span><span class="sxs-lookup"><span data-stu-id="46540-199">For example:</span></span>

   ```shell
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka version : 1.0.2
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka commitId : 2a121f7b1d402825
   [wingtiptoyshub.container-0-C-1] INFO com.wingtiptoys.kafka.KafkaSink - New message received: hello
   ```


> [!NOTE]
> 
> <span data-ttu-id="46540-200">テストのために、*KafkaSource.java* を変更して、次の例のような簡単な HTML フォームを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="46540-200">For testing purposes, you could modify your *KafkaSource.java* so that it contains a simple HTML form like the following example:</span></span>
> 
> ```java
> package com.wingtiptoys.kafka;
>    
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.cloud.stream.annotation.EnableBinding;
> import org.springframework.cloud.stream.messaging.Source;
> import org.springframework.messaging.support.GenericMessage;
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.PostMapping;
> import org.springframework.web.bind.annotation.RequestBody;
> import org.springframework.web.bind.annotation.RequestParam;
> import org.springframework.web.bind.annotation.RestController;
> 
> @EnableBinding(Source.class)
> @RestController
> public class KafkaSource {
>   @Autowired
>   private Source source;
> 
>   @GetMapping("/")
>   public String sendForm() {
>     return "<html><body>" +
>       "<form action=\"/messages\" method=\"post\">" +
>       "<input type=\"text\" name=\"text\">" +
>       "<input type=\"submit\">" +
>       "</form></body><html>";
>     }
> 
>   @PostMapping("/messages")
>   public String sendMessage(@RequestBody String message) {
>     this.source.output().send(new GenericMessage<>(message));
>     return message;
>   }
> }
> ```
> 
> <span data-ttu-id="46540-201">これにより、Web ブラウザーを使用してアプリケーションをテストできます。</span><span class="sxs-lookup"><span data-stu-id="46540-201">This will allow you to use a web browser to test your application:</span></span>
> 
> ![Web ブラウザーを使用したアプリケーションのテスト][TB01]
> 
> <span data-ttu-id="46540-203">フォームを送信すると、アプリケーションの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="46540-203">When you submit the form, your application will display the results:</span></span>
> 
> ![Web ブラウザーでのアプリケーションの応答][TB02]
> 

## <a name="next-steps"></a><span data-ttu-id="46540-205">次の手順</span><span class="sxs-lookup"><span data-stu-id="46540-205">Next steps</span></span>

<span data-ttu-id="46540-206">Azure による Event Hub Stream Binder と Apache Kafka のサポートの詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="46540-206">For more information about Azure support for Event Hub Stream Binder and Apache Kafka, see the following articles:</span></span>

* [<span data-ttu-id="46540-207">Azure Event Hubs とは</span><span class="sxs-lookup"><span data-stu-id="46540-207">What is Azure Event Hubs?</span></span>](/azure/event-hubs/event-hubs-about)

* [<span data-ttu-id="46540-208">Apache Kafka 用の Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="46540-208">Azure Event Hubs for Apache Kafka</span></span>](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

* [<span data-ttu-id="46540-209">Azure portal を使用して Event Hubs 名前空間とイベント ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="46540-209">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>](/azure/event-hubs/event-hubs-create)

* [<span data-ttu-id="46540-210">Apache Kafka 対応イベント ハブの作成</span><span class="sxs-lookup"><span data-stu-id="46540-210">Create Apache Kafka enabled event hubs</span></span>](/azure/event-hubs/event-hubs-create-kafka-enabled)

<span data-ttu-id="46540-211">Java での Azure の使用の詳細については、「Java 開発者向けの Azure」および [Visual Studio Team Services 用の Java ツール]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46540-211">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="46540-212">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="46540-212">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="46540-213">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="46540-213">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="46540-214">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="46540-214">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="46540-215">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="46540-215">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Apache Kafka]: http://kafka.apache.org
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

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-03.png

[TB01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-01.png
[TB02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-02.png
