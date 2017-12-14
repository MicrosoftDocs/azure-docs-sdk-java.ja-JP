---
title: "Azure Key Vault 用の Spring Boot Starter の使用方法"
description: "Azure Key Vault スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。"
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/29/2017
ms.author: robmcm
ms.openlocfilehash: 8b35a972a00c995730dfa59b1b6a47fab7716b76
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a><span data-ttu-id="ab824-103">Azure Key Vault 用の Spring Boot Starter の使用方法</span><span class="sxs-lookup"><span data-stu-id="ab824-103">How to use the Spring Boot Starter for Azure Key Vault</span></span>

## <a name="overview"></a><span data-ttu-id="ab824-104">概要</span><span class="sxs-lookup"><span data-stu-id="ab824-104">Overview</span></span>

<span data-ttu-id="ab824-105">この記事では、**[Spring Initializr]** の Azure Key Vault 用 Spring Boot Starter を使用して、キー コンテナーにシークレットとして格納されている接続文字列を取得するアプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ab824-105">This article demonstrates creating an app with the **[Spring Initializr]** which uses the Spring Boot Starter for Azure Key Vault to retrieve a connection string that is stored as a secret in a key vault.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab824-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="ab824-106">Prerequisites</span></span>

<span data-ttu-id="ab824-107">この記事の手順に従うには、次の前提条件が必要です。</span><span class="sxs-lookup"><span data-stu-id="ab824-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="ab824-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="ab824-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="ab824-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="ab824-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="ab824-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="ab824-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-the-spring-initialzr"></a><span data-ttu-id="ab824-111">Spring Initialzr を使用したアプリの作成</span><span class="sxs-lookup"><span data-stu-id="ab824-111">Create an app using the Spring Initialzr</span></span>

1. <span data-ttu-id="ab824-112"><https://start.spring.io/> に移動します。</span><span class="sxs-lookup"><span data-stu-id="ab824-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="ab824-113">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab824-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![グループとアーティファクトの名前を指定する][secrets-01]

1. <span data-ttu-id="ab824-115">下へスクロールして **[Azure]** セクションを表示し、**[Azure Key Vault]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="ab824-115">Scroll down to the **Azure** section and check the box for **Azure Key Vault**.</span></span>

   ![Azure Key Vault スターターを選択する][secrets-02]

1. <span data-ttu-id="ab824-117">ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab824-117">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Boot プロジェクトを生成する][secrets-03]

1. <span data-ttu-id="ab824-119">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="ab824-119">When prompted, download the project to a path on your local computer.</span></span>

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="ab824-120">Azure へのサインインと使用するサブスクリプションの選択</span><span class="sxs-lookup"><span data-stu-id="ab824-120">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="ab824-121">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="ab824-121">Open a command prompt.</span></span>

1. <span data-ttu-id="ab824-122">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="ab824-122">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="ab824-123">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="ab824-123">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="ab824-124">サブスクリプションを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-124">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="ab824-125">Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-125">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. Specify the GUID for the account you want to use with Azure; for example:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a><span data-ttu-id="ab824-126">Azure CLI を使用した新しい Azure Key Vault の作成と構成</span><span class="sxs-lookup"><span data-stu-id="ab824-126">Create and configure a new Azure Key Vault using the Azure CLI</span></span>

1. <span data-ttu-id="ab824-127">キー コンテナーに使用する Azure リソースのリソース グループを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-127">Create a resource group for the Azure resources you will use for your key vault; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="ab824-128">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="ab824-128">Where:</span></span>
   | <span data-ttu-id="ab824-129">パラメーター</span><span class="sxs-lookup"><span data-stu-id="ab824-129">Parameter</span></span> | <span data-ttu-id="ab824-130">説明</span><span class="sxs-lookup"><span data-stu-id="ab824-130">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="ab824-131">リソース グループの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-131">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="ab824-132">リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-132">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="ab824-133">次のように、Azure CLI に、リソース グループ作成の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab824-133">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

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

1. <span data-ttu-id="ab824-134">アプリケーション登録から Azure サービス プリンシパルを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-134">Create an Azure service principal from your application registration; for example:</span></span>
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   | <span data-ttu-id="ab824-135">パラメーター</span><span class="sxs-lookup"><span data-stu-id="ab824-135">Parameter</span></span> | <span data-ttu-id="ab824-136">説明</span><span class="sxs-lookup"><span data-stu-id="ab824-136">Description</span></span> |
   |---|---|
   | `id` | <span data-ttu-id="ab824-137">アプリケーションの登録時に取得した GUID を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-137">Specifies the GUID from your application registration earlier.</span></span> |

   <span data-ttu-id="ab824-138">Azure CLI から JSON ステータス メッセージが返されます。このメッセージに含まれている *appId* と *password* は、後でクライアント ID およびクライアント パスワードとして使用します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-138">The Azure CLI will return a JSON status message that contains the *appId* and *password*, which you will use later as the client id and client password; for example:</span></span>

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

1. <span data-ttu-id="ab824-139">リソース グループに新しいキー コンテナーを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-139">Create a new key vault in the resource group; for example:</span></span>
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   <span data-ttu-id="ab824-140">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="ab824-140">Where:</span></span>
   | <span data-ttu-id="ab824-141">パラメーター</span><span class="sxs-lookup"><span data-stu-id="ab824-141">Parameter</span></span> | <span data-ttu-id="ab824-142">説明</span><span class="sxs-lookup"><span data-stu-id="ab824-142">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="ab824-143">キー コンテナーの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-143">Specifies a unique name for your key vault.</span></span> |
   | `location` | <span data-ttu-id="ab824-144">リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-144">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |
   | `enabled-for-deployment` | <span data-ttu-id="ab824-145">[キー コンテナーのデプロイ オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-145">Specifies the [key vault deployment option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `enabled-for-disk-encryption` | <span data-ttu-id="ab824-146">[キー コンテナーの暗号化オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-146">Specifies the [key vault encryption option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `enabled-for-template-deployment` | <span data-ttu-id="ab824-147">[キー コンテナーの暗号化オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-147">Specifies the [key vault encryption option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `sku` | <span data-ttu-id="ab824-148">[キー コンテナーの SKU オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-148">Specifies the [key vault SKU option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `query` | <span data-ttu-id="ab824-149">応答から取得する値を指定します。これは、このチュートリアルを完了するために必要なキー コンテナーの URI です。</span><span class="sxs-lookup"><span data-stu-id="ab824-149">Specifies a value to retrieve from the response, which is the key vault URI that you will need to complete this tutorial.</span></span> |

   <span data-ttu-id="ab824-150">Azure CLI にキー コンテナーの URI が表示されます。この URI は後で使用します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-150">The Azure CLI will display the URI for key vault, which you will use later; for example:</span></span>  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

1. <span data-ttu-id="ab824-151">前の手順で作成した Azure サービス プリンシパルのアクセス ポリシーを設定します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-151">Set the access policy for the Azure service principal you created earlier; for example:</span></span>
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   <span data-ttu-id="ab824-152">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="ab824-152">Where:</span></span>
   | <span data-ttu-id="ab824-153">パラメーター</span><span class="sxs-lookup"><span data-stu-id="ab824-153">Parameter</span></span> | <span data-ttu-id="ab824-154">説明</span><span class="sxs-lookup"><span data-stu-id="ab824-154">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="ab824-155">前の手順で作成したキー コンテナーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-155">Specifies your key vault name from earlier.</span></span> |
   | `secret-permission` | <span data-ttu-id="ab824-156">キー コンテナーの[セキュリティ ポリシー](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-156">Specifies the [security policies](https://docs.microsoft.com/en-us/cli/azure/keyvault) for your key vault.</span></span> |
   | `object-id` | <span data-ttu-id="ab824-157">アプリケーションの登録時に取得した GUID を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-157">Specifies the GUID for your application registration from earlier.</span></span> |

   <span data-ttu-id="ab824-158">Azure CLI にセキュリティ ポリシー作成の結果が表示されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-158">The Azure CLI will display the results of your security policy creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "wingtiptoyskeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "wingtiptoysresources",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

1. <span data-ttu-id="ab824-159">新しいキー コンテナーにシークレットを格納します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-159">Store a secret in your new key vault; for example:</span></span>
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   <span data-ttu-id="ab824-160">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="ab824-160">Where:</span></span>
   | <span data-ttu-id="ab824-161">パラメーター</span><span class="sxs-lookup"><span data-stu-id="ab824-161">Parameter</span></span> | <span data-ttu-id="ab824-162">説明</span><span class="sxs-lookup"><span data-stu-id="ab824-162">Description</span></span> |
   |---|---|
   | `vault-name` | <span data-ttu-id="ab824-163">前の手順で作成したキー コンテナーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-163">Specifies your key vault name from earlier.</span></span> |
   | `name` | <span data-ttu-id="ab824-164">シークレットの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-164">Specifies the name of your secret.</span></span> |
   | `value` | <span data-ttu-id="ab824-165">シークレットの値を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-165">Specifies the value of your secret.</span></span> |

   <span data-ttu-id="ab824-166">Azure CLI にシークレット作成の結果が表示されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-166">The Azure CLI will display the results of your secret creation; for example:</span></span>  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://wingtiptoyskeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://wingtiptoys.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="ab824-167">Spring Boot アプリケーションの構成とコンパイル</span><span class="sxs-lookup"><span data-stu-id="ab824-167">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="ab824-168">以前にディレクトリにダウンロードした Spring Boot プロジェクトのパッケージ ファイルからファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="ab824-168">Extract the files from the Spring Boot project archive files that you downloaded earlier into a directory.</span></span>

1. <span data-ttu-id="ab824-169">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="ab824-169">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="ab824-170">このチュートリアルで既に完了した手順で取得した値を使用して、キー コンテナーの値を追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-170">Add the values for your key vault using values from the steps that you completed earlier in this tutorial; for example:</span></span>
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   <span data-ttu-id="ab824-171">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="ab824-171">Where:</span></span>
   | <span data-ttu-id="ab824-172">パラメーター</span><span class="sxs-lookup"><span data-stu-id="ab824-172">Parameter</span></span> | <span data-ttu-id="ab824-173">説明</span><span class="sxs-lookup"><span data-stu-id="ab824-173">Description</span></span> |
   |---|---|
   | `azure.keyvault.uri` | <span data-ttu-id="ab824-174">キー コンテナーの作成時に取得した URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-174">Specifies the URI from when you created your key vault.</span></span> |
   | `azure.keyvault.client-id` | <span data-ttu-id="ab824-175">サービス プリンシパルの作成時に取得した *appId* GUID を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-175">Specifies the *appId* GUID from when you created your service principal.</span></span> |
   | `azure.keyvault.client-key` | <span data-ttu-id="ab824-176">サービス プリンシパルの作成時に取得した *password* GUID を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab824-176">Specifies the *password* GUID from when you created your service principal.</span></span> |

1. <span data-ttu-id="ab824-177">プロジェクトのメイン ソース コード ファイルに移動します (例: */src/main/java/com/wingtiptoys/secrets*)。</span><span class="sxs-lookup"><span data-stu-id="ab824-177">Navigate to the main source code file of your project; for example: */src/main/java/com/wingtiptoys/secrets*.</span></span>

1. <span data-ttu-id="ab824-178">テキスト エディターでアプリケーションのメイン Java ファイル (例: *SecretsApplication.java*) を開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="ab824-178">Open the application's main Java file in a file in a text editor; for example: *SecretsApplication.java*, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   <span data-ttu-id="ab824-179">このコード例では、キー コンテナーから接続文字列を取得し、コマンド ラインに表示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-179">This code example retrieves the connection string from the key vault and displays it to the command line.</span></span>

1. <span data-ttu-id="ab824-180">Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="ab824-180">Save and close the Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="ab824-181">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="ab824-181">Build and test your app</span></span>

1. <span data-ttu-id="ab824-182">Spring Boot アプリの *pom.xml* ファイルがあるディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="ab824-182">Navigate to the directory where the *pom.xml* file for your Spring Boot app is located:</span></span>

1. <span data-ttu-id="ab824-183">Spring Boot アプリケーションを Maven でビルドします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab824-183">Build your Spring Boot application with Maven; for example:</span></span>

   ```bash
   mvn clean package
   ```

   <span data-ttu-id="ab824-184">Maven にビルドの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab824-184">Maven will display the results of your build.</span></span>

   ![Spring Boot アプリケーションのビルドの状態][build-application-01]

1. <span data-ttu-id="ab824-186">Maven で Spring Boot アプリケーションを実行します。アプリケーションによって、キー コンテナーから取得した接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab824-186">Run your Spring Boot application with Maven; the application will display the connection string from your key vault.</span></span> <span data-ttu-id="ab824-187">For example:</span><span class="sxs-lookup"><span data-stu-id="ab824-187">For example:</span></span>

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot の実行時メッセージ][build-application-02]

## <a name="next-steps"></a><span data-ttu-id="ab824-189">次のステップ</span><span class="sxs-lookup"><span data-stu-id="ab824-189">Next steps</span></span>

<span data-ttu-id="ab824-190">Azure Key Vault の使用方法の詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ab824-190">For more information about using Azure Key Vaults, see the following articles:</span></span>

* <span data-ttu-id="ab824-191">[Key Vault のドキュメント]</span><span class="sxs-lookup"><span data-stu-id="ab824-191">[Key Vault Documentation].</span></span>

* <span data-ttu-id="ab824-192">[Azure Key Vault の概要]</span><span class="sxs-lookup"><span data-stu-id="ab824-192">[Get started with Azure Key Vault]</span></span>

<span data-ttu-id="ab824-193">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ab824-193">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="ab824-194">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="ab824-194">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="ab824-195">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="ab824-195">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="ab824-196">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Java Tools for Visual Studio Team Services] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ab824-196">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Key Vault のドキュメント]: /azure/key-vault/
[Azure Key Vault の概要]: /azure/key-vault/key-vault-get-started
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
