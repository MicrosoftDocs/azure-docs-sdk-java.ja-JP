---
title: Azure 向けの Spring Boot Starter
description: この記事では、Azure で利用できる各種 Spring Boot Starter について説明します。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 678d4b279cecb83c95b3bf0f6bcdf1581924aa62
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954443"
---
# <a name="spring-boot-starters-for-azure"></a><span data-ttu-id="6d2ff-103">Azure 向けの Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="6d2ff-103">Spring Boot Starters for Azure</span></span>

<span data-ttu-id="6d2ff-104">この記事では、Java 開発者に Microsoft Azure で作業するための統合機能を提供する、[Spring Initializr] の各種 Spring Boot Starter について説明します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-104">This article describes the various Spring Boot Starters for the [Spring Initializr] that provide Java developers with integration features for working with Microsoft Azure.</span></span>

![Azure の Spring Boot Starter][spring-boot-starters]

<span data-ttu-id="6d2ff-106">Azure では、次の Spring Boot Starter を現在利用できます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-106">The following Spring Boot Starters are currently available for Azure:</span></span>

* <span data-ttu-id="6d2ff-107">**[Azure Support](#azure-support)**</span><span class="sxs-lookup"><span data-stu-id="6d2ff-107">**[Azure Support](#azure-support)**</span></span>

   <span data-ttu-id="6d2ff-108">Azure サービス (Service Bus、Storage、Active Directory など) の自動構成サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-108">Provides auto-configuration support for Azure Services; e.g. Service Bus, Storage, Active Directory, etc.</span></span>

* <span data-ttu-id="6d2ff-109">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="6d2ff-109">**[Azure Active Directory](#azure-active-directory)**</span></span>

   <span data-ttu-id="6d2ff-110">認証用に Azure Active Directory と Spring Security の統合サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-110">Provides integration support for Spring Security with Azure Active Directory for authentication.</span></span>

* <span data-ttu-id="6d2ff-111">**[Azure Key Vault](#azure-key-vault)**</span><span class="sxs-lookup"><span data-stu-id="6d2ff-111">**[Azure Key Vault](#azure-key-vault)**</span></span>

   <span data-ttu-id="6d2ff-112">Azure Key Vault のシークレットと統合するために、Spring の値アノテーション サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-112">Provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

* <span data-ttu-id="6d2ff-113">**[Azure Storage](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="6d2ff-113">**[Azure Storage](#azure-storage)**</span></span>

   <span data-ttu-id="6d2ff-114">Azure Storage サービスの Spring Boot サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-114">Provides Spring Boot support for Azure Storage services.</span></span>

<a name="azure-support"></a>
## <a name="azure-support"></a><span data-ttu-id="6d2ff-115">Azure Support</span><span class="sxs-lookup"><span data-stu-id="6d2ff-115">Azure Support</span></span>

<span data-ttu-id="6d2ff-116">この Spring Boot Starter は、Azure サービス (Service Bus、Storage、Active Directory、Cosmos DB、Key Vault など) の自動構成サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-116">This Spring Boot Starter provides auto-configuration support for Azure Services; for example: Service Bus, Storage, Active Directory, Cosmos DB, Key Vault, etc.</span></span>

<span data-ttu-id="6d2ff-117">このスターターが提供するさまざまな Azure 機能の使用方法の例については、次のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-117">For examples of how to use the various Azure features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="6d2ff-118"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples></span><span class="sxs-lookup"><span data-stu-id="6d2ff-118"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples></span></span>

<span data-ttu-id="6d2ff-119">このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-119">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="6d2ff-120">`<properties>` 要素に次のプロパティが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-120">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="6d2ff-121">`spring-boot-starter` の既定の依存関係が次のように置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-121">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* <span data-ttu-id="6d2ff-122">ファイルに次のセクションが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-122">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-active-directory"></a>
## <a name="azure-active-directory"></a><span data-ttu-id="6d2ff-123">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d2ff-123">Azure Active Directory</span></span>

<span data-ttu-id="6d2ff-124">この Spring Boot Starter は、認証用に Azure Active Directory と統合するために、Spring Security の自動構成サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-124">This Spring Boot Starter provides auto-configuration support for Spring Security in order to provide integration with Azure Active Directory for authentication.</span></span>

<span data-ttu-id="6d2ff-125">このスターターが提供する Azure Active Directory 機能の使用方法の例については、次のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-125">For examples of how to use the Azure Active Directory features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="6d2ff-126"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="6d2ff-126"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample></span></span>

<span data-ttu-id="6d2ff-127">このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-127">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="6d2ff-128">`<properties>` 要素に次のプロパティが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-128">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="6d2ff-129">`spring-boot-starter` の既定の依存関係が次のように置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-129">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="6d2ff-130">ファイルに次のセクションが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-130">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-key-vault"></a>
## <a name="azure-key-vault"></a><span data-ttu-id="6d2ff-131">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="6d2ff-131">Azure Key Vault</span></span>

<span data-ttu-id="6d2ff-132">この Spring Boot Starter は、Azure Key Vault のシークレットと統合するために、Spring の値アノテーション サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-132">This Spring Boot Starter provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

<span data-ttu-id="6d2ff-133">このスターターが提供する Azure Key Vault 機能の使用方法の例については、次のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-133">For examples of how to use the Azure Key Vault features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="6d2ff-134"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="6d2ff-134"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample></span></span>

<span data-ttu-id="6d2ff-135">このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-135">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="6d2ff-136">`<properties>` 要素に次のプロパティが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-136">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="6d2ff-137">`spring-boot-starter` の既定の依存関係が次のように置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-137">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="6d2ff-138">ファイルに次のセクションが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-138">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-storage"></a>
## <a name="azure-storage"></a><span data-ttu-id="6d2ff-139">Azure Storage (Azure Storage)</span><span class="sxs-lookup"><span data-stu-id="6d2ff-139">Azure Storage</span></span>

<span data-ttu-id="6d2ff-140">この Spring Boot Starter は、Azure Storage サービスの Spring Boot 統合サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-140">This Spring Boot Starter provides Spring Boot integration support for Azure Storage services.</span></span>

<span data-ttu-id="6d2ff-141">このスターターが提供する Azure Storage 機能の使用方法の例については、次のページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-141">For examples of how to use the Azure Storage features that are provided by this starter, see the following:</span></span>

* [<span data-ttu-id="6d2ff-142">Azure Storage 用の Spring Boot Starter の使用方法</span><span class="sxs-lookup"><span data-stu-id="6d2ff-142">How to use the Spring Boot Starter for Azure Storage</span></span>](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <span data-ttu-id="6d2ff-143"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="6d2ff-143"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample></span></span>

<span data-ttu-id="6d2ff-144">このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-144">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="6d2ff-145">`<properties>` 要素に次のプロパティが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-145">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="6d2ff-146">`spring-boot-starter` の既定の依存関係が次のように置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-146">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="6d2ff-147">ファイルに次のセクションが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-147">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

## <a name="next-steps"></a><span data-ttu-id="6d2ff-148">次の手順</span><span class="sxs-lookup"><span data-stu-id="6d2ff-148">Next steps</span></span>

<span data-ttu-id="6d2ff-149">Azure での [Spring Boot] アプリケーションの使用の詳細については、「[Azure の Spring]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-149">For more information about using [Spring Boot] applications on Azure, see [Spring on Azure].</span></span>

<span data-ttu-id="6d2ff-150">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-150">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="6d2ff-151">独自の Spring Boot アプリケーションの使用開始に関するヘルプについては、「**Spring Initializr**」(https://start.spring.io/) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d2ff-151">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<!-- URL List -->

[Java 開発者向けの Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure の Spring]: https://docs.microsoft.com/java/azure/spring-framework/
[Spring on Azure]: https://docs.microsoft.com/java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
