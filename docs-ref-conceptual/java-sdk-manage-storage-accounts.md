---
title: "Azure ストレージ アカウントを Java で管理する | Microsoft Docs"
description: "Azure SDK for Java を使って Azure ストレージ アカウントを管理するためのサンプル コード"
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
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a>Java アプリケーションから Azure ストレージ アカウントを管理する

[このサンプル](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)では、[Java 管理ライブラリ](https://github.com/Azure/azure-sdk-for-java)を使用して、[Azure ストレージ](https://docs.microsoft.com/azure/storage/storage-introduction) アカウントを作成し、アカウント アクセス キーを操作します。 

## <a name="run-the-sample"></a>サンプルの実行

[認証ファイル](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)を作成し、そのファイルのコンピューター上における完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。 次に、以下を実行します。

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

[完全なコード サンプルについては GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) を参照してください。

## <a name="authenticate-with-azure"></a>Azure での認証

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a>ストレージ アカウントの作成

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

指定するストレージ名は、Azure に存在するいずれの名前とも重複しないこと、また小文字と数字だけで構成されていることが必要です。 このアカウントに使用される既定のパフォーマンスとレプリケーション プロファイルは [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage) です。

## <a name="list-keys-in-a-storage-account"></a>ストレージ アカウントに存在するキーの列挙
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

それぞれの Azure ストレージ アカウントには、キーを再生成する間も別のキーを使ってストレージにアクセスできる状態を確保するために、2 つのキーが存在します。

## <a name="regenerate-a-key-in-a-storage-account"></a>ストレージ アカウントのキーを再生成する

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

新しいキーを生成したら、すべての Azure リソースとアプリケーションを新しいキーで更新する必要があります。

## <a name="list-all-storage-accounts-in-a-resource-group"></a>リソース グループ内にあるすべてのストレージ アカウントの列挙
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) には、ストレージ アカウントの構成を調べる際に利用できるさまざまなメソッドが掲載されています。

## <a name="delete-a-storage-account"></a>ストレージ アカウントの削除
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> 仮想マシンに接続された使用中のディスク イメージがあるストレージ アカウントや、他のアーティファクトによって使用されているディスクがあるストレージ アカウントは、これらのメソッドでは削除できない場合があります。 アカウントを削除する前に、これらのリソースからあらかじめストレージをデタッチしておいてください。

## <a name="sample-explanation"></a>サンプルの説明

[GitHub のサンプル コード](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):

- ストレージ アカウントの作成
- アクセス キーの読み取りと再生成
- リソース グループに含まれるすべてのストレージ アカウントの列挙
- ストレージ アカウントの削除 

| サンプルで使われているクラス | メモ
|-------|-------|
| [StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | Azure ストレージ アカウントを表します。 ストレージ アカウントに関する情報は、このクラスのメソッドを使って取得します。
| [StorageAccountKey](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | `StorageAccount.getKeys()` は、ストレージ アカウント キーを返します。 キーを更新するには、`StorageAccount` の `regenerateKey` メソッドを使用します。

## <a name="next-steps"></a>次のステップ

[!INCLUDE [next-steps](includes/java-next-steps.md)]