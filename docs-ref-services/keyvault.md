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
# <a name="azure-key-vault-libraries-for-java"></a>Azure Key Vault Libraries for Java

## <a name="overview"></a>概要

クラウド アプリケーションやクラウド サービスで使用される暗号化キーとシークレットをセキュリティで保護し、管理するには、[Azure Key Vault](/azure/key-vault/) を使用します。

Azure Key Vault の概要については、「[Azure Key Vault の概要](/azure/key-vault/key-vault-get-started)」を参照してください。

## <a name="client-library"></a>クライアント ライブラリ

Azure Key Vault に格納されるキーとシークレットの作成、更新、削除は、クライアント ライブラリを使って行います。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.1.0</version>
</dependency>
```   

## <a name="example"></a>例

キー コンテナーから [JSON Web キー](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)を取得します。

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = kvc.getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/java/api/overview/azure/keyvault/client)


## <a name="management-api"></a>管理 API

キー コンテナーの作成、アプリケーションの承認、アクセス許可の管理は、Azure Key Vault 管理ライブラリを使って行います。 

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.15.0</version>
</dependency>
```

## <a name="example"></a>例

`clientId` という[サービス プリンシパル](/azure/azure-resource-manager/resource-group-create-service-principal-portal)で動作するアプリケーションに対し、キー コンテナーに格納されたシークレットの列挙と取得を承認します。 

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
> [Management API を探す](/java/api/overview/azure/keyvault/management)


## <a name="samples"></a>サンプル

アプリで利用できる [Azure Key Vault のサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault)を探しましょう。
