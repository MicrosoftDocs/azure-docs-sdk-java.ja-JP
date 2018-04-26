---
title: Azure Active Directory Libraries for Java
description: Azure Active Directory 用 Java クライアントおよび管理ライブラリのリファレンス ドキュメント
keywords: Azure, Java, SDK, API, SQL, 認証, AAD, Active Directory, Graph, OAuth 2.0
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: active-directory
ms.openlocfilehash: 28063a1a4299fd78ba76533d0ffdc0346434eea2
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="azure-active-directory-libraries-for-java"></a>Azure Active Directory Libraries for Java

## <a name="overview"></a>概要

[Azure Active Directory](/azure/active-directory/active-directory-whatis) でユーザーのサインオンを行い、アプリケーションと API へのアクセスを制御します。

Azure AD の使用を開始するには、「[Azure AD を使用した Java Web アプリへのサインインおよびサインアウト](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java)」を参照してください。

## <a name="client-library"></a>クライアント ライブラリ

[Java 用 Azure Active Directory 認証ライブラリ (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-java) で、OAuth2、OpenID Connect、または Active Directory Graph 認証と [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) シングル サインオンを構成します。

プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a>例

Azure Active Directory の [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) を使用して、Active Directory テナント内のユーザーの JSON Web トークン (JWT) を取得します。 このトークンは、後でアプリケーションまたは API でのユーザーの認証に使用できます。

```java
ExecutorService service = Executors.newFixedThreadPool(1);
AuthenticationContext context = new AuthenticationContext(AUTHORITY, false, service);
Future<AuthenticationResult> future = context.acquireToken(
    "https://graph.windows.net", YOUR_TENANT_ID, username, password,
    null);
AuthenticationResult result = future.get();
System.out.println("Access Token - " + result.getAccessToken());
System.out.println("Refresh Token - " + result.getRefreshToken());
System.out.println("ID Token - " + result.getIdToken());
```

## <a name="management-api"></a>管理 API

Management API で、[ロール ベースのアクセス制御](/azure/active-directory/role-based-access-control-what-is)を構成し、それらのロールに ID (ユーザー、[サービス プリンシパル](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)など) を割り当てます。 

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a>例 

新しいサービス プリンシパルを作成し、共同作成者ロールに割り当てます。

```java
ServicePrincipal sp = Azure.servicePrincipals().define(spName)
    .withNewApplication("http://" + spName)
    .create();
RoleAssignment roleAssignment2 = authenticated.roleAssignments()
    .define("contribRoleAssignment")
    .forServicePrincipal(sp)
    .withBuiltInRole(BuiltInRole.CONTRIBUTOR)
    .withSubscriptionScope("862f67bc-d3ae-4243-bec7-3da6dca77717")
    .create();
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/activedirectory/management)


## <a name="samples"></a>サンプル

[グループ、ユーザー、およびロールを管理する](https://github.com/Azure-Samples/aad-java-browse-graph-and-manage-roles)    
[Java Web アプリでユーザーのサインインとサインアウトを行う](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)    
[コマンド ライン アプリを使用して Azure AD で API にアクセスする](https://github.com/Azure-Samples/active-directory-java-native-headless)   
[Java Web アプリから Active AD Graph API を呼び出す](https://github.com/Azure-Samples/active-directory-java-graphapi-web/)  

アプリで使用できる [Azure AD 用の Java コードのサンプル](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java)を参照してください。
