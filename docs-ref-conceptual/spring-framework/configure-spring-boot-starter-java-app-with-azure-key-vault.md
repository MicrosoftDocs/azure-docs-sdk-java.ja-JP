---
title: Azure Key Vault 用の Spring Boot Starter の使用方法
description: Azure Key Vault スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 1dda697cac80a6cad3ebbbbf8a5a4f18b515dfd8
ms.sourcegitcommit: 798f4d4199d3be9fc5c9f8bf7a754d7393de31ae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a><span data-ttu-id="fdde4-103">Azure Key Vault 用の Spring Boot Starter の使用方法</span><span class="sxs-lookup"><span data-stu-id="fdde4-103">How to use the Spring Boot Starter for Azure Key Vault</span></span>

## <a name="overview"></a><span data-ttu-id="fdde4-104">概要</span><span class="sxs-lookup"><span data-stu-id="fdde4-104">Overview</span></span>

<span data-ttu-id="fdde4-105">この記事では、Azure Key Vault 用 Spring Boot Starter を使用した **[Spring Initializr]** を用いて、キー コンテナーにシークレットとして格納されている接続文字列を取得するアプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Key Vault to retrieve a connection string that is stored as a secret in a key vault.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fdde4-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="fdde4-106">Prerequisites</span></span>

<span data-ttu-id="fdde4-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="fdde4-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="fdde4-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="fdde4-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="fdde4-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="fdde4-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="fdde4-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="fdde4-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-the-spring-initialzr"></a><span data-ttu-id="fdde4-111">Spring Initialzr を使用したアプリの作成</span><span class="sxs-lookup"><span data-stu-id="fdde4-111">Create an app using the Spring Initialzr</span></span>

1. <span data-ttu-id="fdde4-112"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="fdde4-113">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Aritifact]\(アーティファクト\)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fdde4-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![グループとアーティファクトの名前を指定する][secrets-01]

1. <span data-ttu-id="fdde4-115">下へスクロールして **[Azure]** セクションを表示し、**[Azure Key Vault]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="fdde4-115">Scroll down to the **Azure** section and check the box for **Azure Key Vault**.</span></span>

   ![Azure Key Vault スターターを選択する][secrets-02]

1. <span data-ttu-id="fdde4-117">ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fdde4-117">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Boot プロジェクトを生成する][secrets-03]

1. <span data-ttu-id="fdde4-119">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="fdde4-119">When prompted, download the project to a path on your local computer.</span></span>

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="fdde4-120">Azure へのサインインと使用するサブスクリプションの選択</span><span class="sxs-lookup"><span data-stu-id="fdde4-120">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="fdde4-121">コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="fdde4-121">Open a command prompt.</span></span>

1. <span data-ttu-id="fdde4-122">Azure CLI を使って、Azure アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="fdde4-122">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="fdde4-123">指示に従って、サインインを完了します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-123">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="fdde4-124">サブスクリプションを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-124">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="fdde4-125">Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-125">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="fdde4-126">Azure で使用するアカウントの GUID を指定します。例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-126">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a><span data-ttu-id="fdde4-127">Azure CLI を使用した新しい Azure Key Vault の作成と構成</span><span class="sxs-lookup"><span data-stu-id="fdde4-127">Create and configure a new Azure Key Vault using the Azure CLI</span></span>

1. <span data-ttu-id="fdde4-128">キー コンテナーに使用する Azure リソースのリソース グループを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-128">Create a resource group for the Azure resources you will use for your key vault; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="fdde4-129">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="fdde4-129">Where:</span></span>
   | <span data-ttu-id="fdde4-130">パラメーター</span><span class="sxs-lookup"><span data-stu-id="fdde4-130">Parameter</span></span> | <span data-ttu-id="fdde4-131">説明</span><span class="sxs-lookup"><span data-stu-id="fdde4-131">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="fdde4-132">リソース グループの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-132">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="fdde4-133">リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-133">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="fdde4-134">次のように、Azure CLI に、リソース グループ作成の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fdde4-134">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

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

1. <span data-ttu-id="fdde4-135">アプリケーション登録から Azure サービス プリンシパルを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-135">Create an Azure service principal from your application registration; for example:</span></span>
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   <span data-ttu-id="fdde4-136">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="fdde4-136">Where:</span></span>
   | <span data-ttu-id="fdde4-137">パラメーター</span><span class="sxs-lookup"><span data-stu-id="fdde4-137">Parameter</span></span> | <span data-ttu-id="fdde4-138">[説明]</span><span class="sxs-lookup"><span data-stu-id="fdde4-138">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="fdde4-139">Azure サービス プリンシパルの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-139">Specifies the name for your Azure service principal.</span></span> |

   <span data-ttu-id="fdde4-140">Azure CLI から JSON ステータス メッセージが返されます。このメッセージに含まれている *appId* と *password* は、後でクライアント ID およびクライアント パスワードとして使用します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-140">The Azure CLI will return a JSON status message that contains the *appId* and *password*, which you will use later as the client id and client password; for example:</span></span>

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

1. <span data-ttu-id="fdde4-141">リソース グループに新しいキー コンテナーを作成します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-141">Create a new key vault in the resource group; for example:</span></span>
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   <span data-ttu-id="fdde4-142">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="fdde4-142">Where:</span></span>
   | <span data-ttu-id="fdde4-143">パラメーター</span><span class="sxs-lookup"><span data-stu-id="fdde4-143">Parameter</span></span> | <span data-ttu-id="fdde4-144">説明</span><span class="sxs-lookup"><span data-stu-id="fdde4-144">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="fdde4-145">キー コンテナーの一意の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-145">Specifies a unique name for your key vault.</span></span> |
   | `location` | <span data-ttu-id="fdde4-146">リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-146">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |
   | `enabled-for-deployment` | <span data-ttu-id="fdde4-147">[キー コンテナーのデプロイ オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-147">Specifies the [key vault deployment option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `enabled-for-disk-encryption` | <span data-ttu-id="fdde4-148">[キー コンテナーの暗号化オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-148">Specifies the [key vault encryption option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `enabled-for-template-deployment` | <span data-ttu-id="fdde4-149">[キー コンテナーの暗号化オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-149">Specifies the [key vault encryption option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `sku` | <span data-ttu-id="fdde4-150">[キー コンテナーの SKU オプション](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-150">Specifies the [key vault SKU option](https://docs.microsoft.com/en-us/cli/azure/keyvault).</span></span> |
   | `query` | <span data-ttu-id="fdde4-151">応答から取得する値を指定します。これは、このチュートリアルを完了するために必要なキー コンテナーの URI です。</span><span class="sxs-lookup"><span data-stu-id="fdde4-151">Specifies a value to retrieve from the response, which is the key vault URI that you will need to complete this tutorial.</span></span> |

   <span data-ttu-id="fdde4-152">Azure CLI にキー コンテナーの URI が表示されます。この URI は後で使用します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-152">The Azure CLI will display the URI for key vault, which you will use later; for example:</span></span>  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

1. <span data-ttu-id="fdde4-153">前の手順で作成した Azure サービス プリンシパルのアクセス ポリシーを設定します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-153">Set the access policy for the Azure service principal you created earlier; for example:</span></span>
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   <span data-ttu-id="fdde4-154">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="fdde4-154">Where:</span></span>
   | <span data-ttu-id="fdde4-155">パラメーター</span><span class="sxs-lookup"><span data-stu-id="fdde4-155">Parameter</span></span> | <span data-ttu-id="fdde4-156">説明</span><span class="sxs-lookup"><span data-stu-id="fdde4-156">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="fdde4-157">前の手順で作成したキー コンテナーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-157">Specifies your key vault name from earlier.</span></span> |
   | `secret-permission` | <span data-ttu-id="fdde4-158">キー コンテナーの[セキュリティ ポリシー](https://docs.microsoft.com/en-us/cli/azure/keyvault)を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-158">Specifies the [security policies](https://docs.microsoft.com/en-us/cli/azure/keyvault) for your key vault.</span></span> |
   | `spn` | <span data-ttu-id="fdde4-159">アプリケーションの登録時に取得した GUID を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-159">Specifies the GUID for your application registration from earlier.</span></span> |

   <span data-ttu-id="fdde4-160">Azure CLI にセキュリティ ポリシー作成の結果が表示されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-160">The Azure CLI will display the results of your security policy creation; for example:</span></span>  

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

1. <span data-ttu-id="fdde4-161">新しいキー コンテナーにシークレットを格納します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-161">Store a secret in your new key vault; for example:</span></span>
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   <span data-ttu-id="fdde4-162">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="fdde4-162">Where:</span></span>
   | <span data-ttu-id="fdde4-163">パラメーター</span><span class="sxs-lookup"><span data-stu-id="fdde4-163">Parameter</span></span> | <span data-ttu-id="fdde4-164">説明</span><span class="sxs-lookup"><span data-stu-id="fdde4-164">Description</span></span> |
   |---|---|
   | `vault-name` | <span data-ttu-id="fdde4-165">前の手順で作成したキー コンテナーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-165">Specifies your key vault name from earlier.</span></span> |
   | `name` | <span data-ttu-id="fdde4-166">シークレットの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-166">Specifies the name of your secret.</span></span> |
   | `value` | <span data-ttu-id="fdde4-167">シークレットの値を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-167">Specifies the value of your secret.</span></span> |

   <span data-ttu-id="fdde4-168">Azure CLI にシークレット作成の結果が表示されます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-168">The Azure CLI will display the results of your secret creation; for example:</span></span>  

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

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="fdde4-169">Spring Boot アプリケーションの構成とコンパイル</span><span class="sxs-lookup"><span data-stu-id="fdde4-169">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="fdde4-170">以前にディレクトリにダウンロードした Spring Boot プロジェクトのパッケージ ファイルからファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-170">Extract the files from the Spring Boot project archive files that you downloaded earlier into a directory.</span></span>

1. <span data-ttu-id="fdde4-171">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="fdde4-171">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="fdde4-172">このチュートリアルで既に完了した手順で取得した値を使用して、キー コンテナーの値を追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-172">Add the values for your key vault using values from the steps that you completed earlier in this tutorial; for example:</span></span>
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   <span data-ttu-id="fdde4-173">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="fdde4-173">Where:</span></span>
   | <span data-ttu-id="fdde4-174">パラメーター</span><span class="sxs-lookup"><span data-stu-id="fdde4-174">Parameter</span></span> | <span data-ttu-id="fdde4-175">説明</span><span class="sxs-lookup"><span data-stu-id="fdde4-175">Description</span></span> |
   |---|---|
   | `azure.keyvault.uri` | <span data-ttu-id="fdde4-176">キー コンテナーの作成時に取得した URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-176">Specifies the URI from when you created your key vault.</span></span> |
   | `azure.keyvault.client-id` | <span data-ttu-id="fdde4-177">サービス プリンシパルの作成時に取得した *appId* GUID を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-177">Specifies the *appId* GUID from when you created your service principal.</span></span> |
   | `azure.keyvault.client-key` | <span data-ttu-id="fdde4-178">サービス プリンシパルの作成時に取得した *password* GUID を指定します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-178">Specifies the *password* GUID from when you created your service principal.</span></span> |

1. <span data-ttu-id="fdde4-179">プロジェクトのメイン ソース コード ファイルに移動します (例: */src/main/java/com/wingtiptoys/secrets*)。</span><span class="sxs-lookup"><span data-stu-id="fdde4-179">Navigate to the main source code file of your project; for example: */src/main/java/com/wingtiptoys/secrets*.</span></span>

1. <span data-ttu-id="fdde4-180">テキスト エディターでアプリケーションのメイン Java ファイル (例: *SecretsApplication.java*) を開き、ファイルに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-180">Open the application's main Java file in a file in a text editor; for example: *SecretsApplication.java*, and add the following lines to the file:</span></span>

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
   <span data-ttu-id="fdde4-181">このコード例では、キー コンテナーから接続文字列を取得し、コマンド ラインに表示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-181">This code example retrieves the connection string from the key vault and displays it to the command line.</span></span>

1. <span data-ttu-id="fdde4-182">Java ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="fdde4-182">Save and close the Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="fdde4-183">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="fdde4-183">Build and test your app</span></span>

1. <span data-ttu-id="fdde4-184">Spring Boot アプリの *pom.xml* ファイルがあるディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-184">Navigate to the directory where the *pom.xml* file for your Spring Boot app is located:</span></span>

1. <span data-ttu-id="fdde4-185">Spring Boot アプリケーションを Maven でビルドします。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fdde4-185">Build your Spring Boot application with Maven; for example:</span></span>

   ```bash
   mvn clean package
   ```

   <span data-ttu-id="fdde4-186">Maven にビルドの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fdde4-186">Maven will display the results of your build.</span></span>

   ![Spring Boot アプリケーションのビルドの状態][build-application-01]

1. <span data-ttu-id="fdde4-188">Maven で Spring Boot アプリケーションを実行します。アプリケーションによって、キー コンテナーから取得した接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fdde4-188">Run your Spring Boot application with Maven; the application will display the connection string from your key vault.</span></span> <span data-ttu-id="fdde4-189">For example:</span><span class="sxs-lookup"><span data-stu-id="fdde4-189">For example:</span></span>

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot の実行時メッセージ][build-application-02]

## <a name="next-steps"></a><span data-ttu-id="fdde4-191">次のステップ</span><span class="sxs-lookup"><span data-stu-id="fdde4-191">Next steps</span></span>

<span data-ttu-id="fdde4-192">Azure Key Vault の使用方法の詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="fdde4-192">For more information about using Azure Key Vaults, see the following articles:</span></span>

* <span data-ttu-id="fdde4-193">[Key Vault のドキュメント]</span><span class="sxs-lookup"><span data-stu-id="fdde4-193">[Key Vault Documentation].</span></span>

* <span data-ttu-id="fdde4-194">[Azure Key Vault の概要]</span><span class="sxs-lookup"><span data-stu-id="fdde4-194">[Get started with Azure Key Vault]</span></span>

<span data-ttu-id="fdde4-195">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fdde4-195">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="fdde4-196">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="fdde4-196">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="fdde4-197">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="fdde4-197">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="fdde4-198">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Java Tools for Visual Studio Team Services] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fdde4-198">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Key Vault のドキュメント]: /azure/key-vault/
[Key Vault Documentation]: /azure/key-vault/
[Azure Key Vault の概要]: /azure/key-vault/key-vault-get-started
[Get started with Azure Key Vault]: /azure/key-vault/key-vault-get-started
[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
