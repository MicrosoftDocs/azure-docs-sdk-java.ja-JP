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
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-storage-api"></a>Azure Storage API で Spring Boot Starter を使用する方法

## <a name="overview"></a>概要

この記事では、**Spring Initializr** を使用してカスタム アプリケーションを作成し、そのアプリケーションを使用して、Azure Storage API で Azure Storage にアクセスする手順について説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、または[無料の Azure アカウント](https://azure.microsoft.com/pricing/free-trial/)にサインアップできます。
* [Azure コマンド ライン インターフェイス (CLI)](http://docs.microsoft.com/cli/azure/overview)。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* Apache [Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Spring Initializr を使用してカスタム アプリケーションを作成する

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。

   ![基本的な Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > Spring Initializr では、**[Group]\(グループ\)** と **[Artifact]\(アーティファクト\)** の名前を使用してパッケージ名を作成します (例: *com.contoso.wingtiptoysdem*)。
   >

1. 下へスクロールして **[Azure]** セクションを表示し、**[Azure Storage]** チェック ボックスをオンにします。

   ![すべての Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。

   ![すべての Spring Initializr オプション](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

   ![カスタム Spring Boot プロジェクトのダウンロード](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a>Azure へのサインインと使用するサブスクリプションの選択

1. コマンド プロンプトを開きます。

1. Azure CLI を使って、Azure アカウントにサインインします。

   ```azurecli
   az login
   ```
   指示に従って、サインインを完了します。

1. サブスクリプションを一覧表示します。

   ```azurecli
   az account list
   ```
   Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。

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

1. Azure で使用するアカウントの GUID を指定します。例を次に示します。

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a>Azure Storage アカウントの作成

1. この記事で使用する Azure リソースのリソース グループを作成します。次に例を示します。
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | リソース グループの一意の名前を指定します。 |
   | `location` | リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |

   次のように、Azure CLI に、リソース グループ作成の結果が表示されます。  

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

2. Spring Boot アプリのリソース グループに Azure ストレージ アカウントを作成します。次に例を示します。
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | ストレージ アカウントの一意の名前を指定します。 |
   | `resource-group` | 前の手順で作成したリソース グループの名前を指定します。 |
   | `location` | ストレージ アカウントをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |
   | `sku` | `Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS` のいずれかを指定します。 |

   Azure から、プロビジョニングの状態を含む長い JSON 文字列が返されます。次に例を示します。

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

3. ストレージ アカウントの接続文字列を取得します。次に例を示します。
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   各値の説明:

   | パラメーター | 説明 |
   | ---|---|
   | `name` | 前の手順で作成したストレージ アカウントの一意の名前を指定します。 |
   | `resource-group` | 前の手順で作成したリソース グループの名前を指定します。 |

   Azure から、ストレージ アカウントの接続文字列を含む JSON 文字列が返されます。次に例を示します。

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a>Spring Boot アプリケーションの構成とコンパイル

1. ダウンロードしたプロジェクトのアーカイブからディレクトリにファイルを抽出します。

1. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。

1. ストレージ アカウントのキーを追加します。次に例を示します。
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. プロジェクトの */src/main/java/com/example/wingtiptoysdemo* フォルダーに移動し、テキスト エディターで *WingtiptoysdemoApplication.java* ファイルを開きます。

1. 既存の Java コードを、コンテナー内の BLOB を一覧表示する次の例に置き換えます。

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
   > 上記の例では、*application.properties* ファイルで定義されているストレージ アカウント設定を Autowire します。
   >

1. アプリケーションをビルドし、実行します。
   ```shell
   mvn clean package spring-boot:run
   ```

   アプリケーションによってコンテナーが作成され、テキスト ファイルが BLOB としてコンテナーにアップロードされます。BLOB は、[Azure Portal](https://portal.azure.com) でストレージ アカウントの下に表示されます。

   ![Azure Portal での BLOB の表示](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > アプリケーションをコンパイルするときに、次のエラー メッセージが表示される場合があります。
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
   > この場合、Maven Surefire テストを無効にしてください。そのためには、*pom.xml* ファイルに次のプラグイン エントリを追加します。
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

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Microsoft Azure で利用できるその他の Spring Boot Starter の詳細については、「[Azure 向けの Spring Boot Starter](spring-boot-starters-for-azure.md)」をご覧ください。

Spring ベースのアプリケーションへの Azure 機能の統合の詳細については、[Azure の Spring Framework](/java/azure/spring-framework/) に関する記事をご覧ください。

Spring Boot アプリケーションから呼び出すことができるその他の Azure Storage API の詳細については、次の記事をご覧ください。
* [Java から Azure Blob Storage を使用する方法](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Java から Azure Queue Storage を使用する方法](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Java から Azure Table Storage を使用する方法](/azure/cosmos-db/table-storage-how-to-use-java)
* [Java から Azure File Storage を使用する方法](/azure/storage/files/storage-java-how-to-use-file-storage)
