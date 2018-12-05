---
title: Azure Storage 用の Spring Boot Starter の使用方法
description: Azure Storage スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: storage
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/21/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: f94b2981f1e641a6e4b2d9d3028608a56a6590e7
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338876"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a><span data-ttu-id="902aa-103">Azure Storage 用の Spring Boot Starter の使用方法</span><span class="sxs-lookup"><span data-stu-id="902aa-103">How to use the Spring Boot Starter for Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="902aa-104">概要</span><span class="sxs-lookup"><span data-stu-id="902aa-104">Overview</span></span>

<span data-ttu-id="902aa-105">この記事では、**Spring Initializr** を使用してカスタム アプリケーションを作成し、そのアプリケーションに Azure Storage スターターを追加した後、アプリケーションを使用して Azure ストレージ アカウントに BLOB をアップロードする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="902aa-105">This article walks you through creating a custom application using the **Spring Initializr**, then adding the Azure storage starter to your application, and then using your application to upload a blob to your Azure storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="902aa-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="902aa-106">Prerequisites</span></span>

<span data-ttu-id="902aa-107">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="902aa-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="902aa-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、または[無料の Azure アカウント](https://azure.microsoft.com/pricing/free-trial/)にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="902aa-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="902aa-109">[Azure コマンド ライン インターフェイス (CLI)](http://docs.microsoft.com/cli/azure/overview)。</span><span class="sxs-lookup"><span data-stu-id="902aa-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="902aa-110">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="902aa-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="902aa-111">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="902aa-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="902aa-112">Apache [Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="902aa-112">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="902aa-113">この記事の手順を完了するには、Spring Boot 2.0 以上のバージョンが必要です。</span><span class="sxs-lookup"><span data-stu-id="902aa-113">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a><span data-ttu-id="902aa-114">アプリケーションの Azure ストレージ アカウントと BLOB コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="902aa-114">Create an Azure Storage Account and blob container for your application</span></span>

1. <span data-ttu-id="902aa-115">Azure portal (<https://portal.azure.com/>) を参照し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="902aa-115">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="902aa-116">**[+ リソースの作成]** をクリックし、**[Storage]**、**[ストレージ アカウント]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="902aa-116">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![Azure ストレージ アカウントを作成する][IMG01]

1. <span data-ttu-id="902aa-118">**[名前空間の作成]** ページで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="902aa-118">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="902aa-119">一意の**名前**を入力します。この名前は、ストレージ アカウントの URI の一部になります。</span><span class="sxs-lookup"><span data-stu-id="902aa-119">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="902aa-120">たとえば、**[名前]** に「**wingtiptoysstorage**」と入力した場合、URI は *wingtiptoysstorage.core.windows.net* になります。</span><span class="sxs-lookup"><span data-stu-id="902aa-120">For example: if you entered **wingtiptoysstorage** for the **Name**, the URI would be *wingtiptoysstorage.core.windows.net*.</span></span>
   * <span data-ttu-id="902aa-121">**[アカウントの種類]** で **[Blob Storage]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="902aa-121">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="902aa-122">ストレージ アカウントの**場所**を指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-122">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="902aa-123">ストレージ アカウントに使用する**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="902aa-123">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="902aa-124">ストレージ アカウントの新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-124">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>

   ![Azure ストレージ アカウントのオプションを指定する][IMG02]

1. <span data-ttu-id="902aa-126">上記のオプションを指定したら、**[作成]** をクリックしてストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="902aa-126">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

1. <span data-ttu-id="902aa-127">Azure portal でストレージ アカウントが作成されたら、**[BLOB]** をクリックし、**[+ コンテナー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="902aa-127">When the Azure portal has created your storage account, click **Blobs**, then click **+Container**.</span></span>

   ![BLOB コンテナーを作成する][IMG03]

1. <span data-ttu-id="902aa-129">BLOB コンテナーの**名前**を入力し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="902aa-129">Enter a **Name** for your blob container, and then click **OK**.</span></span>

   ![BLOB コンテナーのオプションを指定する][IMG04]

1. <span data-ttu-id="902aa-131">BLOB コンテナーが作成されると、Azure portal に表示されます。</span><span class="sxs-lookup"><span data-stu-id="902aa-131">The Azure portal will list your blob container after is has been created.</span></span>

   ![BLOB コンテナーの一覧の表示][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="902aa-133">Spring Initializr でシンプルな Spring Boot アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="902aa-133">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="902aa-134"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="902aa-134">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="902aa-135">次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-135">Specify the following options:</span></span>

   * <span data-ttu-id="902aa-136">**Java** で **Maven** プロジェクトを生成します。</span><span class="sxs-lookup"><span data-stu-id="902aa-136">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="902aa-137">**Spring Boot** のバージョンとして、2.0 以上を指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-137">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="902aa-138">アプリケーションの**グループ (Group)** と**成果物 (Artifact)** の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-138">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="902aa-139">**Web** 依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="902aa-139">Add the **Web** dependency.</span></span>

      ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="902aa-141">Spring Initializr では、**グループ (Group)** と**成果物 (Artifact)** の名前を使用してパッケージ名を作成します (例: *com.wingtiptoys.storage*)。</span><span class="sxs-lookup"><span data-stu-id="902aa-141">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.storage*.</span></span>
   >

1. <span data-ttu-id="902aa-142">上記のオプションを指定したら、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="902aa-142">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="902aa-143">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="902aa-143">When prompted, download the project to a path on your local computer.</span></span>

   ![Spring プロジェクトをダウンロードする][SI02]

1. <span data-ttu-id="902aa-145">ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="902aa-145">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a><span data-ttu-id="902aa-146">Azure Storage スターターを使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="902aa-146">Configure your Spring Boot app to use the Azure Storage starter</span></span>

1. <span data-ttu-id="902aa-147">アプリのルート ディレクトリで *pom.xml* ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-147">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\pom.xml`

   <span data-ttu-id="902aa-148">または</span><span class="sxs-lookup"><span data-stu-id="902aa-148">-or-</span></span>

   `/users/example/home/storage/pom.xml`

1. <span data-ttu-id="902aa-149">テキスト エディターで *pom.xml* ファイルを開き、Spring Cloud Azure Storage スターターを `<dependencies>` のリストに追加します。</span><span class="sxs-lookup"><span data-stu-id="902aa-149">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Storage starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![pom.xml ファイルを編集する][SI03]

1. <span data-ttu-id="902aa-151">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="902aa-151">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="902aa-152">Azure 資格情報ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="902aa-152">Create an Azure Credential File</span></span>

1. <span data-ttu-id="902aa-153">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="902aa-153">Open a command prompt.</span></span>

1. <span data-ttu-id="902aa-154">Spring Boot アプリの *resources* ディレクトリに移動します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-154">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   <span data-ttu-id="902aa-155">または</span><span class="sxs-lookup"><span data-stu-id="902aa-155">-or-</span></span>

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. <span data-ttu-id="902aa-156">Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="902aa-156">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="902aa-157">サブスクリプションを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-157">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="902aa-158">Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-158">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="902aa-159">Azure 資格情報ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="902aa-159">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="902aa-160">このコマンドにより、*resources* ディレクトリに、次の例のような内容の *my.azureauth* ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="902aa-160">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a><span data-ttu-id="902aa-161">Azure ストレージ アカウントを使用するように Spring Boot アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="902aa-161">Configure your Spring Boot app to use your Azure Storage account</span></span>

1. <span data-ttu-id="902aa-162">アプリの *resources* ディレクトリで *application.properties* を探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-162">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   <span data-ttu-id="902aa-163">または</span><span class="sxs-lookup"><span data-stu-id="902aa-163">-or-</span></span>

   `/users/example/home/storage/src/main/resources/application.properties`

2. <span data-ttu-id="902aa-164">テキスト エディターで *application.properties* ファイルを開きます。次の行を追加し、サンプルの値をストレージ アカウントの適切なプロパティに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="902aa-164">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your storage account:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   <span data-ttu-id="902aa-165">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="902aa-165">Where:</span></span>

   |                   <span data-ttu-id="902aa-166">フィールド</span><span class="sxs-lookup"><span data-stu-id="902aa-166">Field</span></span>                   |                                            <span data-ttu-id="902aa-167">説明</span><span class="sxs-lookup"><span data-stu-id="902aa-167">Description</span></span>                                            |
   |-------------------------------------------|---------------------------------------------------------------------------------------------------|
   | `spring.cloud.azure.credential-file-path` |            <span data-ttu-id="902aa-168">このチュートリアルで作成した Azure 資格情報ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-168">Specifies Azure credential file that you created earlier in this tutorial.</span></span>             |
   |    `spring.cloud.azure.resource-group`    |           <span data-ttu-id="902aa-169">Azure ストレージ アカウントを含む Azure リソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-169">Specifies the Azure Resource Group that contains your Azure Storage account.</span></span>            |
   |        `spring.cloud.azure.region`        | <span data-ttu-id="902aa-170">Azure ストレージ アカウントの作成時に指定した地理的リージョンを指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-170">Specifies the geographical region that you specified when you created your Azure Storage account.</span></span> |
   |   `spring.cloud.azure.storage.account`    |            <span data-ttu-id="902aa-171">このチュートリアルで作成した Azure ストレージ アカウントを指定します。</span><span class="sxs-lookup"><span data-stu-id="902aa-171">Specifies Azure Storage account that you created earlier in this tutorial.</span></span>             |


3. <span data-ttu-id="902aa-172">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="902aa-172">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a><span data-ttu-id="902aa-173">Azure Storage の基本的な機能を実装するサンプル コードを追加する</span><span class="sxs-lookup"><span data-stu-id="902aa-173">Add sample code to implement basic Azure storage functionality</span></span>

<span data-ttu-id="902aa-174">このセクションでは、BLOB を Azure ストレージ アカウントに保存するために必要な Java クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="902aa-174">In this section, you create the necessary Java classes for storing a blob in your Azure storage account.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="902aa-175">アプリケーションのメイン クラスを変更する</span><span class="sxs-lookup"><span data-stu-id="902aa-175">Modify the main application class</span></span>

1. <span data-ttu-id="902aa-176">アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-176">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   <span data-ttu-id="902aa-177">または</span><span class="sxs-lookup"><span data-stu-id="902aa-177">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. <span data-ttu-id="902aa-178">テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="902aa-178">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="902aa-179">メイン アプリケーションの Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="902aa-179">Save and close the main application Java file.</span></span>

### <a name="add-a-web-controller-class"></a><span data-ttu-id="902aa-180">Web コントローラー クラスを追加する</span><span class="sxs-lookup"><span data-stu-id="902aa-180">Add a web controller class</span></span>

1. <span data-ttu-id="902aa-181">アプリのパッケージ ディレクトリに、*WebController.java* という名前の新しい Java ファイルを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-181">Create a new Java file named *WebController.java* in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   <span data-ttu-id="902aa-182">または</span><span class="sxs-lookup"><span data-stu-id="902aa-182">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. <span data-ttu-id="902aa-183">テキスト エディターで Web コントローラー Java ファイルを開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="902aa-183">Open the web controller Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;

   @RestController
   public class WebController {

      @Value("blob://test/myfile.txt")
      private Resource blobFile;

      @GetMapping(value = "/")
      public String readBlobFile() throws IOException {
         return StreamUtils.copyToString(
            this.blobFile.getInputStream(),
            Charset.defaultCharset()) + "\n";
      }

      @PostMapping(value = "/")
      public String writeBlobFile(@RequestBody String data) throws IOException {
         try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
            os.write(data.getBytes());
         }
         return "File was updated.\n";
      }
   }
   ```

   <span data-ttu-id="902aa-184">`@Value("blob://[container]/[blob]")` 構文では、データを格納するコンテナーと BLOB の名前をそれぞれ定義します。</span><span class="sxs-lookup"><span data-stu-id="902aa-184">Where the `@Value("blob://[container]/[blob]")` syntax respectively defines the names of the container and blob where you want to store the data.</span></span>

1. <span data-ttu-id="902aa-185">Web コントローラー Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="902aa-185">Save and close the web controller Java file.</span></span>

1. <span data-ttu-id="902aa-186">コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-186">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\storage`

   <span data-ttu-id="902aa-187">または</span><span class="sxs-lookup"><span data-stu-id="902aa-187">-or-</span></span>

   `cd /users/example/home/storage`

1. <span data-ttu-id="902aa-188">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-188">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="902aa-189">アプリケーションが実行されたら、*curl* を使用してアプリケーションをテストできます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="902aa-189">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   <span data-ttu-id="902aa-190">a.</span><span class="sxs-lookup"><span data-stu-id="902aa-190">a.</span></span> <span data-ttu-id="902aa-191">POST 要求を送信して、ファイルの内容を更新します。</span><span class="sxs-lookup"><span data-stu-id="902aa-191">Send a POST request to update a file's contents:</span></span>

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      <span data-ttu-id="902aa-192">ファイルが更新されたことを示す応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="902aa-192">You should see a response that the file was updated.</span></span>

   <span data-ttu-id="902aa-193">b.</span><span class="sxs-lookup"><span data-stu-id="902aa-193">b.</span></span> <span data-ttu-id="902aa-194">GET 要求を送信して、ファイルの内容を確認します。</span><span class="sxs-lookup"><span data-stu-id="902aa-194">Send a GET request to verify the file's contents:</span></span>

      ```shell
      curl -X GET http://localhost:8080/
      ```

     <span data-ttu-id="902aa-195">送信した "Hello World" というテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="902aa-195">You should see the "Hello World" text that you posted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="902aa-196">次の手順</span><span class="sxs-lookup"><span data-stu-id="902aa-196">Next steps</span></span>

<span data-ttu-id="902aa-197">Microsoft Azure で利用できるその他の Spring Boot Starter の詳細については、「[Azure 向けの Spring Boot Starter](spring-boot-starters-for-azure.md)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="902aa-197">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="902aa-198">Spring ベースのアプリケーションへの Azure 機能の統合の詳細については、[Azure の Spring Framework](/java/azure/spring-framework/) に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="902aa-198">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="902aa-199">Spring Boot アプリケーションから呼び出すことができるその他の Azure Storage API の詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="902aa-199">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="902aa-200">Java から Azure Blob Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="902aa-200">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="902aa-201">Java から Azure Queue Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="902aa-201">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="902aa-202">Java から Azure Table Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="902aa-202">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="902aa-203">Java から Azure File Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="902aa-203">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
