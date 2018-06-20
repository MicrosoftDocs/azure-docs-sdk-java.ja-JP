---
title: Azure ストレージ アカウントを Java で管理する | Microsoft Docs
description: Azure SDK for Java を使って Azure ストレージ アカウントを管理するためのサンプル コード
author: rloutlaw
manager: douge
ms.assetid: 49be8b66-3b56-4c10-8f14-9d326d815cb4
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 5945164b2b04e1fa9169590a71f6c5f9f45842d6
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931058"
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a><span data-ttu-id="6e948-103">Java アプリケーションから Azure ストレージ アカウントを管理する</span><span class="sxs-lookup"><span data-stu-id="6e948-103">Manage Azure storage accounts from your Java applications</span></span>

<span data-ttu-id="6e948-104">[このサンプル](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)では、[Java 管理ライブラリ](https://github.com/Azure/azure-sdk-for-java)を使用して、[Azure ストレージ](https://docs.microsoft.com/azure/storage/storage-introduction) アカウントを作成し、アカウント アクセス キーを操作します。</span><span class="sxs-lookup"><span data-stu-id="6e948-104">[This sample](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) creates an [Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction) account and works with the account access keys using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="6e948-105">サンプルの実行</span><span class="sxs-lookup"><span data-stu-id="6e948-105">Run the sample</span></span>

<span data-ttu-id="6e948-106">[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="6e948-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="6e948-107">次に、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="6e948-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

<span data-ttu-id="6e948-108">[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6e948-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="6e948-109">Azure での認証</span><span class="sxs-lookup"><span data-stu-id="6e948-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a><span data-ttu-id="6e948-110">ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="6e948-110">Create a storage account</span></span>

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

<span data-ttu-id="6e948-111">指定するストレージ名は、Azure に存在するいずれの名前とも重複しないこと、また小文字と数字だけで構成されていることが必要です。</span><span class="sxs-lookup"><span data-stu-id="6e948-111">The storage name provided must be unique across all names in Azure and contain only lowercase letters and numbers.</span></span> <span data-ttu-id="6e948-112">このアカウントに使用される既定のパフォーマンスとレプリケーション プロファイルは [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage) です。</span><span class="sxs-lookup"><span data-stu-id="6e948-112">The default performance and replication profile used for this account is [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage).</span></span>

## <a name="list-keys-in-a-storage-account"></a><span data-ttu-id="6e948-113">ストレージ アカウントに存在するキーの列挙</span><span class="sxs-lookup"><span data-stu-id="6e948-113">List keys in a storage account</span></span>
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

<span data-ttu-id="6e948-114">それぞれの Azure ストレージ アカウントには、キーを再生成する間も別のキーを使ってストレージにアクセスできる状態を確保するために、2 つのキーが存在します。</span><span class="sxs-lookup"><span data-stu-id="6e948-114">Two keys are provided in each Azure storage account so that you can regenerate one key while still allowing access to storage using the other key.</span></span>

## <a name="regenerate-a-key-in-a-storage-account"></a><span data-ttu-id="6e948-115">ストレージ アカウントのキーを再生成する</span><span class="sxs-lookup"><span data-stu-id="6e948-115">Regenerate a key in a storage account</span></span>

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

<span data-ttu-id="6e948-116">新しいキーを生成したら、すべての Azure リソースとアプリケーションを新しいキーで更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6e948-116">You must update all Azure resources and applications with the new key after generating a new one.</span></span>

## <a name="list-all-storage-accounts-in-a-resource-group"></a><span data-ttu-id="6e948-117">リソース グループ内にあるすべてのストレージ アカウントの列挙</span><span class="sxs-lookup"><span data-stu-id="6e948-117">List all storage accounts in a resource group</span></span>
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

<span data-ttu-id="6e948-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) には、ストレージ アカウントの構成を調べる際に利用できるさまざまなメソッドが掲載されています。</span><span class="sxs-lookup"><span data-stu-id="6e948-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) provides a set of useful methods to inspect the configuration of a storage account.</span></span>

## <a name="delete-a-storage-account"></a><span data-ttu-id="6e948-119">ストレージ アカウントの削除</span><span class="sxs-lookup"><span data-stu-id="6e948-119">Delete a storage account</span></span>
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> <span data-ttu-id="6e948-120">仮想マシンに接続された使用中のディスク イメージがあるストレージ アカウントや、他のアーティファクトによって使用されているディスクがあるストレージ アカウントは、これらのメソッドでは削除できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="6e948-120">Storage accounts with in-use disk images connected to virtual machines or disks in use by other artifacts may not remove with these methods.</span></span> <span data-ttu-id="6e948-121">アカウントを削除する前に、これらのリソースからあらかじめストレージをデタッチしておいてください。</span><span class="sxs-lookup"><span data-stu-id="6e948-121">Detach the storage from these resources before removing the account.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="6e948-122">サンプルの説明</span><span class="sxs-lookup"><span data-stu-id="6e948-122">Sample explanation</span></span>

<span data-ttu-id="6e948-123">[GitHub のサンプル コード](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):</span><span class="sxs-lookup"><span data-stu-id="6e948-123">[The sample code on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):</span></span>

- <span data-ttu-id="6e948-124">ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="6e948-124">creates a storage account</span></span>
- <span data-ttu-id="6e948-125">アクセス キーの読み取りと再生成</span><span class="sxs-lookup"><span data-stu-id="6e948-125">reads and regenerates access keys</span></span>
- <span data-ttu-id="6e948-126">リソース グループに含まれるすべてのストレージ アカウントの列挙</span><span class="sxs-lookup"><span data-stu-id="6e948-126">lists all storage accounts in a resource group</span></span>
- <span data-ttu-id="6e948-127">ストレージ アカウントの削除</span><span class="sxs-lookup"><span data-stu-id="6e948-127">deletes the storage account</span></span> 

| <span data-ttu-id="6e948-128">サンプルで使われているクラス</span><span class="sxs-lookup"><span data-stu-id="6e948-128">Class used in sample</span></span> | <span data-ttu-id="6e948-129">メモ</span><span class="sxs-lookup"><span data-stu-id="6e948-129">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="6e948-130">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="6e948-130">StorageAccount</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | <span data-ttu-id="6e948-131">Azure ストレージ アカウントを表します。</span><span class="sxs-lookup"><span data-stu-id="6e948-131">Representation of an Azure storage account.</span></span> <span data-ttu-id="6e948-132">ストレージ アカウントに関する情報は、このクラスのメソッドを使って取得します。</span><span class="sxs-lookup"><span data-stu-id="6e948-132">Use the methods in the class to get information about the storage account.</span></span>
| [<span data-ttu-id="6e948-133">StorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="6e948-133">StorageAccountKey</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | <span data-ttu-id="6e948-134">`StorageAccount.getKeys()` は、ストレージ アカウント キーを返します。</span><span class="sxs-lookup"><span data-stu-id="6e948-134">`StorageAccount.getKeys()` returns the storage account keys.</span></span> <span data-ttu-id="6e948-135">キーを更新するには、`StorageAccount` の `regenerateKey` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6e948-135">Use the `regenerateKey` methods in `StorageAccount` to update the keys.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e948-136">次のステップ</span><span class="sxs-lookup"><span data-stu-id="6e948-136">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]