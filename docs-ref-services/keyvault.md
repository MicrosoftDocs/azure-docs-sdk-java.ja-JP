---
title: Azure Key Vault Libraries for Java
description: Azure Key Vault Libraries for Java の概要
keywords: Azure, Java, SDK, API, keyvault, セキュア, キー, シークレット, vault
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: keyvault
ms.openlocfilehash: 84ea77a19c326409f453f62359cf46c90398daeb
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799928"
---
# <a name="azure-key-vault-libraries-for-java"></a><span data-ttu-id="39426-104">Azure Key Vault Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="39426-104">Azure Key Vault libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="39426-105">概要</span><span class="sxs-lookup"><span data-stu-id="39426-105">Overview</span></span>

<span data-ttu-id="39426-106">クラウド アプリケーションやクラウド サービスで使用される暗号化キーとシークレットをセキュリティで保護し、管理するには、[Azure Key Vault](/azure/key-vault/) を使用します。</span><span class="sxs-lookup"><span data-stu-id="39426-106">Safeguard and manage cryptographic keys and secrets used by cloud applications and services with [Azure Key Vault](/azure/key-vault/).</span></span>

<span data-ttu-id="39426-107">Azure Key Vault の概要については、「[Azure Key Vault の概要](/azure/key-vault/key-vault-get-started)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="39426-107">To get started with Azure Key Vault, see [Get started with Azure Key Vault](/azure/key-vault/key-vault-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="39426-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="39426-108">Client library</span></span>

<span data-ttu-id="39426-109">Azure Key Vault に格納されるキーとシークレットの作成、更新、削除は、クライアント ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="39426-109">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="39426-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="39426-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.1.0</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="39426-111">例</span><span class="sxs-lookup"><span data-stu-id="39426-111">Example</span></span>

<span data-ttu-id="39426-112">キー コンテナーから [JSON Web キー](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)を取得します。</span><span class="sxs-lookup"><span data-stu-id="39426-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = kvc.getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="39426-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="39426-113">Explore the Client APIs</span></span>](/java/api/overview/azure/keyvault/client)


## <a name="management-api"></a><span data-ttu-id="39426-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="39426-114">Management API</span></span>

<span data-ttu-id="39426-115">キー コンテナーの作成、アプリケーションの承認、アクセス許可の管理は、Azure Key Vault 管理ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="39426-115">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="39426-116">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="39426-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.15.0</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="39426-117">例</span><span class="sxs-lookup"><span data-stu-id="39426-117">Example</span></span>

<span data-ttu-id="39426-118">`clientId` という[サービス プリンシパル](/azure/azure-resource-manager/resource-group-create-service-principal-portal)で動作するアプリケーションに対し、キー コンテナーに格納されたシークレットの列挙と取得を承認します。</span><span class="sxs-lookup"><span data-stu-id="39426-118">Authorize and application running with [service principal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` to list and retrieve secrets from a key vault.</span></span> 

```java
vault1 = vault1.update()
            .defineAccessPolicy()
                .forServicePrincipal(clientId)
                .allowKeyAllPermissions()
                .allowSecretPermissions(SecretPermissions.GET)
                .allowSecretPermissions(SecretPermissions.LIST)
                .attach()
            .apply();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="39426-119">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="39426-119">Explore the Management APIs</span></span>](/java/api/overview/azure/keyvault/management)


## <a name="samples"></a><span data-ttu-id="39426-120">サンプル</span><span class="sxs-lookup"><span data-stu-id="39426-120">Samples</span></span>

<span data-ttu-id="39426-121">アプリで利用できる [Azure Key Vault のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault)を探しましょう。</span><span class="sxs-lookup"><span data-stu-id="39426-121">Explore more [sample Java code for Azure Key Vault](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault) you can use in your apps.</span></span>
