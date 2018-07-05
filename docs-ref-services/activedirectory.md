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
ms.openlocfilehash: 4a610e2f0d9fb2e219c42155e2b0cb76fc78b09a
ms.sourcegitcommit: 5bfb3af5778167500a061157cbd0ad1cede8f90e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/04/2018
ms.locfileid: "37799700"
---
# <a name="azure-active-directory-libraries-for-java"></a><span data-ttu-id="67574-104">Azure Active Directory Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="67574-104">Azure Active Directory libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="67574-105">概要</span><span class="sxs-lookup"><span data-stu-id="67574-105">Overview</span></span>

<span data-ttu-id="67574-106">[Azure Active Directory](/azure/active-directory/active-directory-whatis) でユーザーのサインオンを行い、アプリケーションと API へのアクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="67574-106">Sign-on users and control access to applications and APIs with [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span></span>

<span data-ttu-id="67574-107">Azure AD の使用を開始するには、「[Azure AD を使用した Java Web アプリへのサインインおよびサインアウト](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="67574-107">To get started with Azure AD, see [Java web app sign-in and sign-out with Azure AD](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="67574-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="67574-108">Client library</span></span>

<span data-ttu-id="67574-109">[Java 用 Azure Active Directory 認証ライブラリ (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-java) で、OAuth2、OpenID Connect、または Active Directory Graph 認証と [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) シングル サインオンを構成します。</span><span class="sxs-lookup"><span data-stu-id="67574-109">Configure OAuth2, OpenID Connect, or Active Directory Graph authentication and [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) single-sign on with the [Azure Active Directory authentication library (ADAL) for Java](https://github.com/AzureAD/azure-activedirectory-library-for-java).</span></span>

<span data-ttu-id="67574-110">プロジェクトでクライアント ライブラリを使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="67574-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="67574-111">例</span><span class="sxs-lookup"><span data-stu-id="67574-111">Example</span></span>

<span data-ttu-id="67574-112">Azure Active Directory の [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) を使用して、Active Directory テナント内のユーザーの JSON Web トークン (JWT) を取得します。</span><span class="sxs-lookup"><span data-stu-id="67574-112">Retrieve a JSON Web Token (JWT) for a user in your an Active Directory tenant using Azure Active Directory's [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api).</span></span> <span data-ttu-id="67574-113">このトークンは、後でアプリケーションまたは API でのユーザーの認証に使用できます。</span><span class="sxs-lookup"><span data-stu-id="67574-113">This token can then be used to authenticate the user with an application or API.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="67574-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="67574-114">Management API</span></span>

<span data-ttu-id="67574-115">Management API で、[ロール ベースのアクセス制御](/azure/active-directory/role-based-access-control-what-is)を構成し、それらのロールに ID (ユーザー、[サービス プリンシパル](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)など) を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="67574-115">Configure [role based access control](/azure/active-directory/role-based-access-control-what-is) and assign identities (such as users and [service principals](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) to those roles with the management API.</span></span> 

<span data-ttu-id="67574-116">プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="67574-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="67574-117">例</span><span class="sxs-lookup"><span data-stu-id="67574-117">Example</span></span> 

<span data-ttu-id="67574-118">新しいサービス プリンシパルを作成し、共同作成者ロールに割り当てます。</span><span class="sxs-lookup"><span data-stu-id="67574-118">Create a new service principal and assign it the Contributor role.</span></span>

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
> [<span data-ttu-id="67574-119">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="67574-119">Explore the Management APIs</span></span>](/java/api/activedirectory/management)


## <a name="samples"></a><span data-ttu-id="67574-120">サンプル</span><span class="sxs-lookup"><span data-stu-id="67574-120">Samples</span></span>

<span data-ttu-id="67574-121">[グループ、ユーザー、およびロールを管理する](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)  </span><span class="sxs-lookup"><span data-stu-id="67574-121">[Manage groups, users, and roles](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)  </span></span>  
<span data-ttu-id="67574-122">[Java Web アプリでユーザーのサインインとサインアウトを行う](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span><span class="sxs-lookup"><span data-stu-id="67574-122">[Sign-in and sign-out users in a Java web app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span></span>  
<span data-ttu-id="67574-123">[コマンド ライン アプリを使用して Azure AD で API にアクセスする](https://github.com/Azure-Samples/active-directory-java-native-headless) </span><span class="sxs-lookup"><span data-stu-id="67574-123">[Access an API with Azure AD using a command line app](https://github.com/Azure-Samples/active-directory-java-native-headless) </span></span>  
[<span data-ttu-id="67574-124">Java Web アプリから Active AD Graph API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="67574-124">Call the Active AD Graph API from your Java web app</span></span>](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  

<span data-ttu-id="67574-125">アプリで使用できる [Azure AD 用の Java コードのサンプル](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="67574-125">Explore more [sample Java code for Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java) you can use in your apps.</span></span>
