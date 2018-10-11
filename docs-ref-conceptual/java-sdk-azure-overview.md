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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892733"
---
# <a name="azure-libraries-for-java"></a>Java 用 Azure ライブラリ

アプリケーション コードから Java 用 Azure ライブラリを使って Azure リソースを管理したりサービスに接続したりすることができます。 このライブラリは、Java プロジェクト用の [Maven インポート](java-sdk-azure-install.md)として利用できます。 

## <a name="manage-azure-resources"></a>Azure のリソースを管理する

[Azure Management Libraries for Java](java-sdk-azure-get-started.md) を使用すると、Java アプリケーションから Azure リソースを作成したり管理したりすることができます。 これらのライブラリを使用すれば、Azure 自動化ツールや Azure サービスを独自に作成することができます。 

たとえば Linux VM を作成するコードは、次のようになります。

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

ライブラリの全一覧とプロジェクトへのインポート方法については、[インストール手順](java-sdk-azure-install.md)を参照してください。そのうえで[概要の記事](java-sdk-azure-get-started.md)を参照し、自分の Azure サブスクリプションに対して認証を設定したり、サンプル コードを実行したりする方法を確認しましょう。 

## <a name="connect-to-azure-services"></a>Azure サービスへの接続

Java ライブラリを使用すると、Azure 内のリソースを作成したり管理したりするだけでなく、アプリ内からそれらのリソースに接続して利用することができます。 たとえば、SQL Database のテーブルを更新したり、Azure Storage にファイルを格納したりすることもできます。 [ライブラリの全一覧](java-sdk-azure-install.md)から、特定のサービスに必要なライブラリをお選びください。また、それらのライブラリをアプリ内で使用するためのチュートリアルやサンプル コードは、[Java デベロッパー センター](https://azure.microsoft.com/develop/java/)から入手できます。

たとえば、Azure ストレージ コンテナーに格納されているすべての BLOB のコンテンツを印刷するには、次のコードを使用します。

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

## <a name="sample-code-and-reference"></a>サンプル コードとリファレンス

以下のサンプルには、Azure Management Libraries for Java を使った一般的な自動化タスクが紹介されており、また、皆さんのアプリですぐに利用できるコードも用意されています。

- [仮想マシン](java-sdk-azure-virtual-machine-samples.md)
- [Web アプリ](java-sdk-azure-web-apps-samples.md)
- [SQL Database](java-sdk-azure-sql-database-samples.md)
   
サービス ライブラリと管理ライブラリの全パッケージについて[リファレンス](https://docs.microsoft.com/java/api)が公開されています。 新機能、重大な変更、以前のバージョンからの移行手順については、[リリース ノート](java-sdk-azure-release-notes.md)を参照してください。