---
title: Azure Storage 用の Spring Boot Starter の使用方法
description: Azure Storage スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: yungez;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: e10ecfb7f6d705aa3ccffc49d354d1019f7f1a0b
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a><span data-ttu-id="bfc86-103">Azure Storage 用の Spring Boot Starter の使用方法</span><span class="sxs-lookup"><span data-stu-id="bfc86-103">How to use the Spring Boot Starter for Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="bfc86-104">概要</span><span class="sxs-lookup"><span data-stu-id="bfc86-104">Overview</span></span>

<span data-ttu-id="bfc86-105">この記事では、**Spring Initializr** を使用してカスタム アプリケーションを作成し、そのアプリケーションを使用して Azure Storage にアクセスする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-105">This article walks you through creating a custom application using the **Spring Initializr**, and then using that application to access Azure storage.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bfc86-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="bfc86-106">Prerequisites</span></span>

<span data-ttu-id="bfc86-107">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="bfc86-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="bfc86-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、または[無料の Azure アカウント](https://azure.microsoft.com/pricing/free-trial/)にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="bfc86-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="bfc86-109">[Azure コマンド ライン インターフェイス (CLI)](http://docs.microsoft.com/cli/azure/overview)。</span><span class="sxs-lookup"><span data-stu-id="bfc86-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="bfc86-110">最新の [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) (バージョン 1.7 以降)。</span><span class="sxs-lookup"><span data-stu-id="bfc86-110">An up-to-date [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="bfc86-111">Apache [Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="bfc86-111">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="bfc86-112">Spring Initializr を使用してカスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="bfc86-112">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="bfc86-113"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-113">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="bfc86-114">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bfc86-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![基本的な Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > <span data-ttu-id="bfc86-116">Spring Initializr では、**[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** の名前を使用してパッケージ名を作成します (例: *com.contoso.wingtiptoysdem*)。</span><span class="sxs-lookup"><span data-stu-id="bfc86-116">The Spring Initializr will use the **Group** and **Aritifact** names to create the package name; for example: *com.contoso.wingtiptoysdemo*.</span></span>
   >

1. <span data-ttu-id="bfc86-117">下へスクロールして **[Azure]** セクションを表示し、**[Azure Storage]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="bfc86-117">Scroll down to the **Azure** section and check the box for **Azure Storage**.</span></span>

   ![すべての Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. <span data-ttu-id="bfc86-119">ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bfc86-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![すべての Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. <span data-ttu-id="bfc86-121">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="bfc86-121">When prompted, download the project to a path on your local computer.</span></span>

   ![カスタム Spring Boot プロジェクトのダウンロード](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="bfc86-123">Azure へのサインインと使用するサブスクリプションの選択</span><span class="sxs-lookup"><span data-stu-id="bfc86-123">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="bfc86-124">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="bfc86-124">Open a command prompt.</span></span>

1. <span data-ttu-id="bfc86-125">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="bfc86-125">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="bfc86-126">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-126">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="bfc86-127">サブスクリプションを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-127">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="bfc86-128">Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-128">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="bfc86-129">Azure で使用するアカウントの GUID を指定します。例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-129">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="bfc86-130">Azure Storage アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="bfc86-130">Create an Azure Storage account</span></span>

1. <span data-ttu-id="bfc86-131">この記事で使用する Azure リソースのリソース グループを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-131">Create a resource group for the Azure resources you will use in this article; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="bfc86-132">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="bfc86-132">Where:</span></span>
   | <span data-ttu-id="bfc86-133">パラメーター</span><span class="sxs-lookup"><span data-stu-id="bfc86-133">Parameter</span></span> | <span data-ttu-id="bfc86-134">説明</span><span class="sxs-lookup"><span data-stu-id="bfc86-134">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="bfc86-135">リソース グループの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-135">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="bfc86-136">リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-136">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="bfc86-137">次のように、Azure CLI に、リソース グループ作成の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="bfc86-137">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

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

1. <span data-ttu-id="bfc86-138">Spring Boot アプリのリソース グループに Azure ストレージ アカウントを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-138">Create an Azure storage account in the in the resource group for your Spring Boot app; for example:</span></span>
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   <span data-ttu-id="bfc86-139">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="bfc86-139">Where:</span></span>
   | <span data-ttu-id="bfc86-140">パラメーター</span><span class="sxs-lookup"><span data-stu-id="bfc86-140">Parameter</span></span> | <span data-ttu-id="bfc86-141">[説明]</span><span class="sxs-lookup"><span data-stu-id="bfc86-141">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="bfc86-142">ストレージ アカウントの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-142">Specifies a unique name for your storage account.</span></span> |
   | `resource-group` | <span data-ttu-id="bfc86-143">前の手順で作成したリソース グループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-143">Specifies the name of the resource group group you created in the previous step.</span></span> |
   | `location` | <span data-ttu-id="bfc86-144">ストレージ アカウントをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-144">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your storage account will be hosted.</span></span> |
   | `sku` | <span data-ttu-id="bfc86-145">`Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS` のいずれかを指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-145">Specifies one of the following: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_ZRS`.</span></span> |

   <span data-ttu-id="bfc86-146">Azure から、プロビジョニングの状態を含む長い JSON 文字列が返されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-146">Azure will return a long JSON string which contains the provisioning state; for example: |</span></span>
   
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

1. <span data-ttu-id="bfc86-147">ストレージ アカウントの接続文字列を取得します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-147">Retrieve the connection string for your storage account; for example:</span></span>
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   <span data-ttu-id="bfc86-148">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="bfc86-148">Where:</span></span>
   | <span data-ttu-id="bfc86-149">パラメーター</span><span class="sxs-lookup"><span data-stu-id="bfc86-149">Parameter</span></span> | <span data-ttu-id="bfc86-150">[説明]</span><span class="sxs-lookup"><span data-stu-id="bfc86-150">Description</span></span> |
   | ---|---|
   | `name` | <span data-ttu-id="bfc86-151">前の手順で作成したストレージ アカウントの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-151">Specifies a unique name of the storage account you created in previous steps.</span></span> |
   | `resource-group` | <span data-ttu-id="bfc86-152">前の手順で作成したリソース グループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-152">Specifies the name of the resource group you created in previous steps.</span></span> |

   <span data-ttu-id="bfc86-153">Azure から、ストレージ アカウントの接続文字列を含む JSON 文字列が返されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-153">Azure will return a JSON string which contains the connection string for your storage account; for example:</span></span>

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="bfc86-154">Spring Boot アプリケーションの構成とコンパイル</span><span class="sxs-lookup"><span data-stu-id="bfc86-154">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="bfc86-155">ダウンロードしたプロジェクトのアーカイブからディレクトリにファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-155">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="bfc86-156">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="bfc86-156">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="bfc86-157">ストレージ アカウントのキーを追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-157">Add the key for your storage account; for example:</span></span>
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. <span data-ttu-id="bfc86-158">プロジェクトの */src/main/java/com/example/wingtiptoysdemo* フォルダーに移動し、テキスト エディターで *WingtiptoysdemoApplication.java* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="bfc86-158">Navigate to the */src/main/java/com/example/wingtiptoysdemo* folder in your project and open the *WingtiptoysdemoApplication.java* file in a text editor.</span></span>

1. <span data-ttu-id="bfc86-159">既存の Java コードを、コンテナー内の BLOB を一覧表示する次の例に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bfc86-159">Replace the existing Java code with the following example that lists the blobs in a container:</span></span>

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
   > <span data-ttu-id="bfc86-160">上記の例では、*application.properties* ファイルで定義されているストレージ アカウント設定を Autowire します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-160">The above example autowires the storage account settings that you defined in the *application.properties* file.</span></span>
   >

1. <span data-ttu-id="bfc86-161">アプリケーションをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-161">Compile and run the application:</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```
   
   <span data-ttu-id="bfc86-162">アプリケーションによってコンテナーが作成され、テキスト ファイルが BLOB としてコンテナーにアップロードされます。BLOB は、[Azure Portal](https://portal.azure.com) でストレージ アカウントの下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="bfc86-162">The application will create a container and upload a text file as a blob to the container, which will be listed under your storage account in the [Azure portal](https://portal.azure.com).</span></span>

   ![Azure Portal での BLOB の表示](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > <span data-ttu-id="bfc86-164">アプリケーションをコンパイルするときに、次のエラー メッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="bfc86-164">When you compile your application, you might see the following error message:</span></span>
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
   > <span data-ttu-id="bfc86-165">この場合、Maven Surefire テストを無効にしてください。そのためには、*pom.xml* ファイルに次のプラグイン エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="bfc86-165">If this happens, you might want to disable the Maven Surefire testing; to do so, add the following plugin entry in your *pom.xml* file:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="bfc86-166">次の手順</span><span class="sxs-lookup"><span data-stu-id="bfc86-166">Next steps</span></span>

<span data-ttu-id="bfc86-167">Microsoft Azure で利用できるその他の Spring Boot Starter の詳細については、「[Azure 向けの Spring Boot Starter](spring-boot-starters-for-azure.md)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bfc86-167">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="bfc86-168">Spring ベースのアプリケーションへの Azure 機能の統合の詳細については、[Azure の Spring Framework](/java/azure/spring-framework/) に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bfc86-168">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="bfc86-169">Spring Boot アプリケーションから呼び出すことができるその他の Azure Storage API の詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bfc86-169">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="bfc86-170">Java から Azure Blob Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="bfc86-170">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="bfc86-171">Java から Azure Queue Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="bfc86-171">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="bfc86-172">Java から Azure Table Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="bfc86-172">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="bfc86-173">Java から Azure File Storage を使用する方法</span><span class="sxs-lookup"><span data-stu-id="bfc86-173">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)
