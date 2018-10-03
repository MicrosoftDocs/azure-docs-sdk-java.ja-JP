---
title: Azure Storage 用の Spring Boot Starter の使用方法
description: Azure Storage スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 1a219a066f0f89adbf3f541856b36b842520bfbb
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2018
ms.locfileid: "46505920"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>Azure Storage 用の Spring Boot Starter の使用方法

## <a name="overview"></a>概要

この記事では、**Spring Initializr** を使用してカスタム アプリケーションを作成し、そのアプリケーションに Azure Storage スターターを追加した後、アプリケーションを使用して Azure ストレージ アカウントに BLOB をアップロードする手順について説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を有効にするか、または[無料の Azure アカウント](https://azure.microsoft.com/pricing/free-trial/)にサインアップできます。
* [Azure コマンド ライン インターフェイス (CLI)](http://docs.microsoft.com/cli/azure/overview)。
* 最新の [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) (バージョン 1.7 以降)。
* Apache [Maven](http://maven.apache.org/) バージョン 3.0 以降。

> [!IMPORTANT]
>
> この記事の手順を完了するには、Spring Boot 2.0 以上のバージョンが必要です。
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a>アプリケーションの Azure ストレージ アカウントと BLOB コンテナーを作成する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[+ リソースの作成]** をクリックし、**[Storage]**、**[ストレージ アカウント]** の順にクリックします。

   ![Azure ストレージ アカウントを作成する][IMG01]

1. **[名前空間の作成]** ページで、次の情報を入力します。

   * 一意の**名前**を入力します。この名前は、ストレージ アカウントの URI の一部になります。 たとえば、**[名前]** に「**wingtiptoysstorage**」と入力した場合、URI は *wingtiptoysstorage.core.windows.net* になります。
   * **[アカウントの種類]** で **[Blob Storage]** を選択します。
   * ストレージ アカウントの**場所**を指定します。
   * ストレージ アカウントに使用する**サブスクリプション**を選択します。
   * ストレージ アカウントの新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。
   
   ![Azure ストレージ アカウントのオプションを指定する][IMG02]

1. 上記のオプションを指定したら、**[作成]** をクリックしてストレージ アカウントを作成します。

1. Azure portal でストレージ アカウントが作成されたら、**[BLOB]** をクリックし、**[+ コンテナー]** をクリックします。

   ![BLOB コンテナーを作成する][IMG03]

1. BLOB コンテナーの**名前**を入力し、**[OK]** をクリックします。

   ![BLOB コンテナーのオプションを指定する][IMG04]

1. BLOB コンテナーが作成されると、Azure portal に表示されます。

   ![BLOB コンテナーの一覧の表示][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Spring Initializr でシンプルな Spring Boot アプリケーションを作成する

1. <https://start.spring.io/> を参照します。

1. 次のオプションを指定します。

   * **Java** で **Maven** プロジェクトを生成します。
   * **Spring Boot** のバージョンとして、2.0 以上を指定します。
   * アプリケーションの**グループ (Group)** と**成果物 (Artifact)** の名前を指定します。
   * **Web** 依存関係を追加します。

      ![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   >
   > Spring Initializr では、**グループ (Group)** と**成果物 (Artifact)** の名前を使用してパッケージ名を作成します (例: *com.wingtiptoys.storage*)。
   >

1. 上記のオプションを指定したら、**[Generate Project]\(プロジェクトの生成\)** をクリックします。

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

   ![Spring プロジェクトをダウンロードする][SI02]

1. ファイルをローカル システム上に展開したら、シンプルな Spring Boot アプリケーションの編集を開始できます。

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a>Azure Storage スターターを使用するように Spring Boot アプリを構成する

1. アプリのルート ディレクトリで *pom.xml* ファイルを探します。次に例を示します。

   `C:\SpringBoot\storage\pom.xml`

   または

   `/users/example/home/storage/pom.xml`

1. テキスト エディターで *pom.xml* ファイルを開き、Spring Cloud Azure Storage スターターを `<dependencies>` のリストに追加します。

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![pom.xml ファイルを編集する][SI03]

1. *pom.xml* ファイルを保存して閉じます。

## <a name="create-an-azure-credential-file"></a>Azure 資格情報ファイルを作成する

1. コマンド プロンプトを開きます。

1. Spring Boot アプリの *resources* ディレクトリに移動します。次に例を示します。

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   または

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. Azure アカウントにサインインします。

   ```azurecli
   az login
   ```

1. サブスクリプションを一覧表示します。

   ```azurecli
   az account list
   ```
   Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。

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

1. Azure 資格情報ファイルを作成します。

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   このコマンドにより、*resources* ディレクトリに、次の例のような内容の *my.azureauth* ファイルが作成されます。

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a>Azure ストレージ アカウントを使用するように Spring Boot アプリを構成する

1. アプリの *resources* ディレクトリで *application.properties* を探します。次に例を示します。

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   または

   `/users/example/home/storage/src/main/resources/application.properties`

1.  テキスト エディターで *application.properties* ファイルを開きます。次の行を追加し、サンプルの値をストレージ アカウントの適切なプロパティに置き換えます。

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   各値の説明:
   | フィールド | 説明 |
   | ---|---|
   | `spring.cloud.azure.credential-file-path` | このチュートリアルで作成した Azure 資格情報ファイルを指定します。 |
   | `spring.cloud.azure.resource-group` | Azure ストレージ アカウントを含む Azure リソース グループを指定します。 |
   | `spring.cloud.azure.region` | Azure ストレージ アカウントの作成時に指定した地理的リージョンを指定します。 |
   | `spring.cloud.azure.storage.account` | このチュートリアルで作成した Azure ストレージ アカウントを指定します。

1. *application.properties* ファイルを保存して閉じます。

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a>Azure Storage の基本的な機能を実装するサンプル コードを追加する

このセクションでは、BLOB を Azure ストレージ アカウントに保存するために必要な Java クラスを作成します。

### <a name="modify-the-main-application-class"></a>アプリケーションのメイン クラスを変更する

1. アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   または

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。

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

1. メイン アプリケーションの Java ファイルを保存して閉じます。

### <a name="add-a-web-controller-class"></a>Web コントローラー クラスを追加する

1. アプリのパッケージ ディレクトリに、*WebController.java* という名前の新しい Java ファイルを作成します。次に例を示します。

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   または

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. テキスト エディターで Web コントローラー Java ファイルを開き、ファイルに次の行を追加します。

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
   
   `@Value("blob://[container]/[blob]")` 構文では、データを格納するコンテナーと BLOB の名前をそれぞれ定義します。

1. Web コントローラー Java ファイルを保存して閉じます。

1. コマンド プロンプトを開き、ディレクトリを *pom.xml* ファイルが置かれているフォルダーに変更します。次に例を示します。

   `cd C:\SpringBoot\storage`

   または

   `cd /users/example/home/storage`

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. アプリケーションが実行されたら、*curl* を使用してアプリケーションをテストできます。次に例を示します。

   a. POST 要求を送信して、ファイルの内容を更新します。

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      ファイルが更新されたことを示す応答が表示されます。

   b. GET 要求を送信して、ファイルの内容を確認します。

      ```shell
      curl -X GET http://localhost:8080/
      ```

     送信した "Hello World" というテキストが表示されます。

## <a name="next-steps"></a>次の手順

Microsoft Azure で利用できるその他の Spring Boot Starter の詳細については、「[Azure 向けの Spring Boot Starter](spring-boot-starters-for-azure.md)」をご覧ください。

Spring ベースのアプリケーションへの Azure 機能の統合の詳細については、[Azure の Spring Framework](/java/azure/spring-framework/) に関する記事をご覧ください。

Spring Boot アプリケーションから呼び出すことができるその他の Azure Storage API の詳細については、次の記事をご覧ください。
* [Java から Azure Blob Storage を使用する方法](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Java から Azure Queue Storage を使用する方法](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Java から Azure Table Storage を使用する方法](/azure/cosmos-db/table-storage-how-to-use-java)
* [Java から Azure File Storage を使用する方法](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
