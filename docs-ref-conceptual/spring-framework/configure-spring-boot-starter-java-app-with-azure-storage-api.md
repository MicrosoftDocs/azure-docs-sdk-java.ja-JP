---
title: Azure Storage API で Spring Boot Starter を使用する方法
description: Azure Storage API を使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: storage
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 984a3edb89608c806537f991b42e309f31130896
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991446"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-storage-api"></a><span data-ttu-id="11dec-103">Azure Storage API で Spring Boot Starter を使用する方法</span><span class="sxs-lookup"><span data-stu-id="11dec-103">How to use the Spring Boot Starter with the Azure Storage API</span></span>

## <a name="overview"></a><span data-ttu-id="11dec-104">概要</span><span class="sxs-lookup"><span data-stu-id="11dec-104">Overview</span></span>

<span data-ttu-id="11dec-105">この記事では、**Spring Initializr** を使用してカスタム アプリケーションを作成し、そのアプリケーションを使用して、Azure Storage API で Azure Storage にアクセスする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="11dec-105">This article walks you through creating a custom application using the **Spring Initializr**, and then using that application to access Azure storage by using the Azure Storage API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11dec-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="11dec-106">Prerequisites</span></span>

<span data-ttu-id="11dec-107">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="11dec-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="11dec-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、または[無料の Azure アカウント](https://azure.microsoft.com/pricing/free-trial/)にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="11dec-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="11dec-109">[Azure コマンド ライン インターフェイス (CLI)](http://docs.microsoft.com/cli/azure/overview)。</span><span class="sxs-lookup"><span data-stu-id="11dec-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="11dec-110">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="11dec-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="11dec-111">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="11dec-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="11dec-112">Apache [Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="11dec-112">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="11dec-113">Spring Initializr を使用してカスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="11dec-113">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="11dec-114"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="11dec-114">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="11dec-115">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="11dec-115">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![基本的な Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > <span data-ttu-id="11dec-117">Spring Initializr では、**[Group]\(グループ\)** と **[Artifact]\(アーティファクト\)** の名前を使用してパッケージ名を作成します (例: *com.contoso.wingtiptoysdem*)。</span><span class="sxs-lookup"><span data-stu-id="11dec-117">The Spring Initializr will use the **Group** and **Artifact** names to create the package name; for example: *com.contoso.wingtiptoysdemo*.</span></span>
   >

1. <span data-ttu-id="11dec-118">下へスクロールして **[Azure]** セクションを表示し、**[Azure Storage]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="11dec-118">Scroll down to the **Azure** section and check the box for **Azure Storage**.</span></span>

   ![すべての Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. <span data-ttu-id="11dec-120">ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="11dec-120">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![すべての Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. <span data-ttu-id="11dec-122">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="11dec-122">When prompted, download the project to a path on your local computer.</span></span>

   ![カスタム Spring Boot プロジェクトのダウンロード](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="11dec-124">Azure へのサインインと使用するサブスクリプションの選択</span><span class="sxs-lookup"><span data-stu-id="11dec-124">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="11dec-125">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="11dec-125">Open a command prompt.</span></span>

1. <span data-ttu-id="11dec-126">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="11dec-126">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="11dec-127">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="11dec-127">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="11dec-128">サブスクリプションを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-128">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="11dec-129">Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-129">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="11dec-130">Azure で使用するアカウントの GUID を指定します。例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-130">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="11dec-131">Azure Storage アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="11dec-131">Create an Azure Storage account</span></span>

1. <span data-ttu-id="11dec-132">この記事で使用する Azure リソースのリソース グループを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-132">Create a resource group for the Azure resources you will use in this article; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="11dec-133">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="11dec-133">Where:</span></span>

   | <span data-ttu-id="11dec-134">パラメーター</span><span class="sxs-lookup"><span data-stu-id="11dec-134">Parameter</span></span> | <span data-ttu-id="11dec-135">説明</span><span class="sxs-lookup"><span data-stu-id="11dec-135">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="11dec-136">リソース グループの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-136">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="11dec-137">リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-137">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="11dec-138">次のように、Azure CLI に、リソース グループ作成の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="11dec-138">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. <span data-ttu-id="11dec-139">Spring Boot アプリのリソース グループに Azure ストレージ アカウントを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-139">Create an Azure storage account in the in the resource group for your Spring Boot app; for example:</span></span>
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   <span data-ttu-id="11dec-140">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="11dec-140">Where:</span></span>

   | <span data-ttu-id="11dec-141">パラメーター</span><span class="sxs-lookup"><span data-stu-id="11dec-141">Parameter</span></span> | <span data-ttu-id="11dec-142">説明</span><span class="sxs-lookup"><span data-stu-id="11dec-142">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="11dec-143">ストレージ アカウントの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-143">Specifies a unique name for your storage account.</span></span> |
   | `resource-group` | <span data-ttu-id="11dec-144">前の手順で作成したリソース グループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-144">Specifies the name of the resource group group you created in the previous step.</span></span> |
   | `location` | <span data-ttu-id="11dec-145">ストレージ アカウントをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-145">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your storage account will be hosted.</span></span> |
   | `sku` | <span data-ttu-id="11dec-146">`Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS` のいずれかを指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-146">Specifies one of the following: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_ZRS`.</span></span> |

   <span data-ttu-id="11dec-147">Azure から、プロビジョニングの状態を含む長い JSON 文字列が返されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-147">Azure will return a long JSON string which contains the provisioning state; for example: |</span></span>

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "identity": null,
     "kind": "Storage"
       ...
       ... (A long list of values will be displayed here.)
       ...
     "statusOfPrimary": "available",
     "statusOfSecondary": null,
     "tags": {},
     "type": "Microsoft.Storage/storageAccounts"
   }
   ```

3. <span data-ttu-id="11dec-148">ストレージ アカウントの接続文字列を取得します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-148">Retrieve the connection string for your storage account; for example:</span></span>
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   <span data-ttu-id="11dec-149">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="11dec-149">Where:</span></span>

   | <span data-ttu-id="11dec-150">パラメーター</span><span class="sxs-lookup"><span data-stu-id="11dec-150">Parameter</span></span> | <span data-ttu-id="11dec-151">説明</span><span class="sxs-lookup"><span data-stu-id="11dec-151">Description</span></span> |
   | ---|---|
   | `name` | <span data-ttu-id="11dec-152">前の手順で作成したストレージ アカウントの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-152">Specifies a unique name of the storage account you created in previous steps.</span></span> |
   | `resource-group` | <span data-ttu-id="11dec-153">前の手順で作成したリソース グループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="11dec-153">Specifies the name of the resource group you created in previous steps.</span></span> |

   <span data-ttu-id="11dec-154">Azure から、ストレージ アカウントの接続文字列を含む JSON 文字列が返されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-154">Azure will return a JSON string which contains the connection string for your storage account; for example:</span></span>

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="11dec-155">Spring Boot アプリケーションの構成とコンパイル</span><span class="sxs-lookup"><span data-stu-id="11dec-155">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="11dec-156">ダウンロードしたプロジェクトのアーカイブからディレクトリにファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="11dec-156">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="11dec-157">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="11dec-157">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="11dec-158">ストレージ アカウントのキーを追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="11dec-158">Add the key for your storage account; for example:</span></span>
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. <span data-ttu-id="11dec-159">プロジェクトの */src/main/java/com/example/wingtiptoysdemo* フォルダーに移動し、テキスト エディターで *WingtiptoysdemoApplication.java* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="11dec-159">Navigate to the */src/main/java/com/example/wingtiptoysdemo* folder in your project and open the *WingtiptoysdemoApplication.java* file in a text editor.</span></span>

1. <span data-ttu-id="11dec-160">既存の Java コードを、コンテナー内の BLOB を一覧表示する次の例に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="11dec-160">Replace the existing Java code with the following example that lists the blobs in a container:</span></span>

   ```java
   package com.example.wingtiptoysdemo;

   import com.microsoft.azure.storage.*;
   import com.microsoft.azure.storage.blob.*;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   import java.net.URISyntaxException;

   @SpringBootApplication
   public class WingtiptoysdemoApplication implements CommandLineRunner {

      @Autowired
      private CloudStorageAccount cloudStorageAccount;

      final String containerName = "mycontainer";

      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysdemoApplication.class, args);
      }

      public void run(String... var1)
             throws URISyntaxException, StorageException {
          // Create a container (if it does not exist).
          createContainerIfNotExists(containerName);
          // Upload a blob to the container.
          uploadTextBlob(containerName);
      }

      private void createContainerIfNotExists(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Create the container if it does not exist.
            container.createIfNotExists();
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }

      private void uploadTextBlob(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Get a blob reference for a text file.
            CloudBlockBlob blob = container.getBlockBlobReference("test.txt");
            // Upload some text into the blob.
            blob.uploadText("Hello World!");
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }
   }
   ```
   > [!NOTE]
   >
   > <span data-ttu-id="11dec-161">上記の例では、*application.properties* ファイルで定義されているストレージ アカウント設定を Autowire します。</span><span class="sxs-lookup"><span data-stu-id="11dec-161">The above example autowires the storage account settings that you defined in the *application.properties* file.</span></span>
   >

1. <span data-ttu-id="11dec-162">アプリケーションをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="11dec-162">Compile and run the application:</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```

   <span data-ttu-id="11dec-163">アプリケーションによってコンテナーが作成され、テキスト ファイルが BLOB としてコンテナーにアップロードされます。BLOB は、[Azure Portal](https://portal.azure.com) でストレージ アカウントの下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="11dec-163">The application will create a container and upload a text file as a blob to the container, which will be listed under your storage account in the [Azure portal](https://portal.azure.com).</span></span>

   ![Azure Portal での BLOB の表示](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > <span data-ttu-id="11dec-165">アプリケーションをコンパイルするときに、次のエラー メッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="11dec-165">When you compile your application, you might see the following error message:</span></span>
   > 
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] BUILD FAILURE`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] Total time: 2.616 s`<br/>
   > `[INFO] Finished at: 2017-11-11T13:14:15Z`<br/>
   > `[INFO] Final Memory: 26M/213M`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2`<br/>
   > `.18.1:test (default-test) on project wingtiptoysdemo: Execution default-test of`<br/>
   > `goal org.apache.maven.plugins:maven-surefire-plugin:2.18.1:test failed: The for`<br/>
   > `ked VM terminated without properly saying goodbye. VM crash or System.exit called?`<br/>
   > `[ERROR] Command was /bin/sh -c cd /home/robert/SpringBoot/wingtiptoysdemo && /u`<br/>
   > `sr/lib/jvm/java-8-openjdk-amd64/jre/bin/java -jar /home/robert/SpringBoot/wingt`<br/>
   > `iptoysdemo/target/surefire/surefirebooter6371623993063346766.jar /home/robert/S`<br/>
   > `pringBoot/wingtiptoysdemo/target/surefire/surefire5107893623933537917tmp /home/`<br/>
   > `robert/SpringBoot/wingtiptoysdemo/target/surefire/surefire_01414159391084128068tmp`<br/>
   > `[ERROR] -> [Help 1]`<br/>
   > 
   > <span data-ttu-id="11dec-166">この場合、Maven Surefire テストを無効にしてください。そのためには、*pom.xml* ファイルに次のプラグイン エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="11dec-166">If this happens, you might want to disable the Maven Surefire testing; to do so, add the following plugin entry in your *pom.xml* file:</span></span>
   > 
   > ```xml
   > <plugin>
   >   <groupId>org.apache.maven.plugins</groupId>
   >   <artifactId>maven-surefire-plugin</artifactId>
   >   <version>2.20.1</version>
   >   <configuration>
   >     <skipTests>true</skipTests>
   >   </configuration>
   > </plugin>
   > ```
   > 

## <a name="next-steps"></a><span data-ttu-id="11dec-167">次の手順</span><span class="sxs-lookup"><span data-stu-id="11dec-167">Next steps</span></span>

<span data-ttu-id="11dec-168">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="11dec-168">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="11dec-169">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="11dec-169">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="11dec-170">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="11dec-170">Additional Resources</span></span>

<span data-ttu-id="11dec-171">Microsoft Azure で利用できるその他の Spring Boot Starter の詳細については、「[Azure 向けの Spring Boot Starter](spring-boot-starters-for-azure.md)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11dec-171">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="11dec-172">Spring ベースのアプリケーションへの Azure 機能の統合の詳細については、[Azure の Spring Framework](/java/azure/spring-framework/) に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11dec-172">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="11dec-173">Spring Boot アプリケーションから呼び出すことができるその他の Azure Storage API の詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11dec-173">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="11dec-174">Java から Azure Blob Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="11dec-174">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="11dec-175">Java から Azure Queue Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="11dec-175">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="11dec-176">Java から Azure Table Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="11dec-176">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="11dec-177">Java から Azure File Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="11dec-177">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)
