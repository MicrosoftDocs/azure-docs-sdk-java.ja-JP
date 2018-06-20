---
title: Java 用 Azure ライブラリ
description: Java 用 Azure 管理/サービス ライブラリの概要
keywords: Azure, Java, SDK, API
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 9aaf22a2-382a-4b13-a8e3-0e467d607207
ms.openlocfilehash: 22a337e928085475e905a271f991147729d97671
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
ms.locfileid: "21930878"
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="277a0-104">Java 用 Azure ライブラリ</span><span class="sxs-lookup"><span data-stu-id="277a0-104">Azure libraries for Java</span></span>

<span data-ttu-id="277a0-105">アプリケーション コードから Java 用 Azure ライブラリを使って Azure リソースを管理したりサービスに接続したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="277a0-105">The Azure libraries for Java help you manage Azure resources and connect to services from your application code.</span></span> <span data-ttu-id="277a0-106">このライブラリは、Java プロジェクト用の [Maven インポート](java-sdk-azure-install.md)として利用できます。</span><span class="sxs-lookup"><span data-stu-id="277a0-106">The libraries are available as [Maven imports](java-sdk-azure-install.md) for use in your Java projects.</span></span> 

## <a name="manage-azure-resources"></a><span data-ttu-id="277a0-107">Azure のリソースを管理する</span><span class="sxs-lookup"><span data-stu-id="277a0-107">Manage Azure resources</span></span>

<span data-ttu-id="277a0-108">[Azure Management Libraries for Java](java-sdk-azure-get-started.md) を使用すると、Java アプリケーションから Azure リソースを作成したり管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="277a0-108">Create and manage Azure resources from your Java applications using the [Azure management libraries for Java](java-sdk-azure-get-started.md).</span></span> <span data-ttu-id="277a0-109">これらのライブラリを使用すれば、Azure 自動化ツールや Azure サービスを独自に作成することができます。</span><span class="sxs-lookup"><span data-stu-id="277a0-109">Use these libraries to build your own Azure automation tools and services.</span></span> 

<span data-ttu-id="277a0-110">たとえば Linux VM を作成するコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="277a0-110">For example, to create a Linux VM you would write the following code:</span></span>

```java
VirtualMachine linuxVM = azure.virtualMachines().define("myAzureVM")
           .withRegion(region)
           .withExistingResourceGroup("sampleResourceGroup")
           .withNewPrimaryNetwork("10.0.0.0/28")
           .withPrimaryPrivateIpAddressDynamic()
           .withoutPrimaryPublicIpAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withSsh(key)
           .withUnmanagedStorage()
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
 ```

<span data-ttu-id="277a0-111">ライブラリの全一覧とプロジェクトへのインポート方法については、[インストール手順](java-sdk-azure-install.md)を参照してください。そのうえで[概要の記事](java-sdk-azure-get-started.md)を参照し、自分の Azure サブスクリプションに対して認証を設定したり、サンプル コードを実行したりする方法を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="277a0-111">Review the [install instructions](java-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](java-sdk-azure-get-started.md) to set up your authentication and run sample code against your own Azure subscription.</span></span> 

## <a name="connect-to-azure-services"></a><span data-ttu-id="277a0-112">Azure サービスへの接続</span><span class="sxs-lookup"><span data-stu-id="277a0-112">Connect to Azure services</span></span>

<span data-ttu-id="277a0-113">Java ライブラリを使用すると、Azure 内のリソースを作成したり管理したりするだけでなく、アプリ内からそれらのリソースに接続して利用することができます。</span><span class="sxs-lookup"><span data-stu-id="277a0-113">In addition to using Java libraries to create and manage resources within Azure, you can also use Java libraries to connect  and use those resources in your apps.</span></span> <span data-ttu-id="277a0-114">たとえば、SQL Database のテーブルを更新したり、Azure Storage にファイルを格納したりすることもできます。</span><span class="sxs-lookup"><span data-stu-id="277a0-114">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="277a0-115">[ライブラリの全一覧](java-sdk-azure-install.md)から、特定のサービスに必要なライブラリをお選びください。また、それらのライブラリをアプリ内で使用するためのチュートリアルやサンプル コードは、[Java デベロッパー センター](https://azure.microsoft.com/develop/java/)から入手できます。</span><span class="sxs-lookup"><span data-stu-id="277a0-115">Select the library you need for a particular service from the [complete list of libraries](java-sdk-azure-install.md) and visit the [Java developer center](https://azure.microsoft.com/develop/java/) for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="277a0-116">たとえば、Azure ストレージ コンテナーに格納されているすべての BLOB のコンテンツを印刷するには、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="277a0-116">For example, to print out the contents of every blob in an Azure storage container:</span></span>

```java
// get the container from the blob client
CloudBlobContainer container = blobClient.getContainerReference("blobcontainer");

// For each item in the container
for (ListBlobItem blobItem : container.listBlobs()) {
    // If the item is a blob, not a virtual directory
    if (blobItem instanceof CloudBlockBlob) {
        // Download the text
        CloudBlockBlob retrievedBlob = (CloudBlockBlob) blobItem;
        System.out.println(retrievedBlob.downloadText());
    }
}
```

## <a name="sample-code-and-reference"></a><span data-ttu-id="277a0-117">サンプル コードとリファレンス</span><span class="sxs-lookup"><span data-stu-id="277a0-117">Sample code and reference</span></span>

<span data-ttu-id="277a0-118">以下のサンプルには、Azure Management Libraries for Java を使った一般的な自動化タスクが紹介されており、また、皆さんのアプリですぐに利用できるコードも用意されています。</span><span class="sxs-lookup"><span data-stu-id="277a0-118">The following samples cover common automation tasks with the Azure management libraries for Java and have code ready to use in your own apps:</span></span>

- [<span data-ttu-id="277a0-119">仮想マシン</span><span class="sxs-lookup"><span data-stu-id="277a0-119">Virtual machines</span></span>](java-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="277a0-120">Web アプリ</span><span class="sxs-lookup"><span data-stu-id="277a0-120">Web apps</span></span>](java-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="277a0-121">SQL Database</span><span class="sxs-lookup"><span data-stu-id="277a0-121">SQL Database</span></span>](java-sdk-azure-sql-database-samples.md)
   
<span data-ttu-id="277a0-122">サービス ライブラリと管理ライブラリの全パッケージについて[リファレンス](https://docs.microsoft.com/java/api)が公開されています。</span><span class="sxs-lookup"><span data-stu-id="277a0-122">A [reference](https://docs.microsoft.com/java/api) is available for all packages in both the service and management libraries.</span></span> <span data-ttu-id="277a0-123">新機能、重大な変更、以前のバージョンからの移行手順については、[リリース ノート](java-sdk-azure-release-notes.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="277a0-123">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](java-sdk-azure-release-notes.md).</span></span>