---
title: Azure Explorer for Eclipse を使用してストレージ アカウントを管理する
description: Azure Explorer for Eclipse を使用して Azure ストレージ アカウントを管理する方法について説明します。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 310d95436189af09f794154f4c9f0e71c47d88c8
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48899446"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a>Azure Explorer for Eclipse を使用してストレージ アカウントを管理する

Azure Toolkit for Eclipse の一部である Azure Explorer は、Eclipse 統合開発環境 (IDE) 内から Azure アカウントのストレージ アカウントを管理するための使いやすいソリューションを Java 開発者に提供します。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-eclipse"></a>Eclipse でストレージ アカウントを作成する

Azure Explorer を使用してストレージ アカウントを作成するには、以下の手順を実行します。

1. 「[Azure Toolkit for Eclipse のサインイン手順](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)」を使用して Azure アカウントにサインインします。

1. **Azure Explorer** ビューで、**[Azure]** ノードを展開し、**[ストレージ アカウント]** を右クリックし、**[ストレージ アカウントの作成]** をクリックします。

   ![[ストレージ アカウントの作成] コマンド][CS01]

1. **[ストレージ アカウントの作成]** ダイアログ ボックスで、次のオプションを指定します。

   ![[新しいストレージ アカウントの作成] ダイアログ ボックス][CS02]

   * **[名前]**: 新しいストレージ アカウントの名前を指定します。

   * **[サブスクリプション]**: 新しいストレージ アカウントに使用する Azure サブスクリプションを指定します。

   * **[リソース グループ]**: 仮想マシン用のリソース グループを指定します。 次のいずれかのオプションを選択します。
      * **[新規作成]**: 新しいリソース グループを作成することを指定します。
      * **[既存のものを使用]**: Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。

   * **[リージョン]**: ストレージ アカウントが作成される場所 ("米国西部" など) を指定します。

   * **[アカウントの種類]**: 作成するストレージ アカウントの種類 ("Blob Storage" など) を指定します。 詳細については、「[Azure ストレージ アカウントについて]」を参照してください。

   * **[パフォーマンス]**: 選択された発行元からどのストレージ アカウント サービスを使用するかを指定します ("Premium" など)。 詳細については、「[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]」を参照してください。

   * **[レプリケーション]**: ストレージ アカウントのレプリケーション ("ゾーン冗長" など) を指定します。 詳細については、「[Azure Storage のレプリケーション]」を参照してください。

1. 上記のオプションをすべて指定したら、**[作成]** をクリックします。

## <a name="create-a-storage-container-in-eclipse"></a>Eclipse でストレージ コンテナーを作成する

Azure Explorer を使用してストレージ コンテナーを作成するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、コンテナーを作成するストレージ アカウントを右クリックし、**[BLOB コンテナーを作成する]** をクリックします。

   ![[BLOB コンテナーを作成する] コマンド][CC01]

1. **[BLOB コンテナーを作成する]** ダイアログ ボックスで、コンテナーの名前を指定し、**[OK]** をクリックします。 ストレージ コンテナーの名前付けの詳細については、「[コンテナー、BLOB、メタデータの名前付けと参照]」を参照してください。

   ![[BLOB コンテナーを作成する] ダイアログ ボックス][CC02]

## <a name="delete-a-storage-container-in-eclipse"></a>Eclipse でストレージ コンテナーを削除する

Azure Explorer を使用してストレージ コンテナーを削除するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、ストレージ コンテナーを右クリックし、**[削除]** をクリックします。

   ![[ストレージ コンテナーの削除] コマンド][DC01]

1. 確認ウィンドウで、**[OK]** をクリックします。

   ![[ストレージ コンテナーの削除] 確認ウィンドウ][DC02]

## <a name="delete-a-storage-account-in-eclipse"></a>Eclipse でストレージ アカウントを削除する

Azure Explorer を使用してストレージ アカウントを削除するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、ストレージ アカウントを右クリックし、**[削除]** をクリックします。

   ![[Delete storage account] (ストレージ アカウントの削除) コマンド][DS01]

1. 確認ウィンドウで、**[OK]** をクリックします。

   ![[Delete storage account] (ストレージ アカウントの削除) 確認ウィンドウ][DS02]

## <a name="next-steps"></a>次の手順

Azure ストレージ アカウント、サイズ、および料金の詳細については、次のリソースを参照してください。

* [Microsoft Azure Storage の概要]
* [Azure ストレージ アカウントについて]
* Azure ストレージ アカウントのサイズ
  * [Azure の Windows ストレージ アカウントのサイズ]
  * [Azure の Linux ストレージ アカウントのサイズ]
* Azure ストレージ アカウントの料金
  * [Windows ストレージ アカウントの料金]
  * [Linux ストレージ アカウントの料金]

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Microsoft Azure Storage の概要]: /azure/storage/storage-introduction
[Azure ストレージ アカウントについて]: /azure/storage/storage-create-storage-account
[Azure Storage のレプリケーション]: /azure/storage/storage-redundancy
[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]: /azure/storage/storage-scalability-targets
[コンテナー、BLOB、メタデータの名前付けと参照]: http://go.microsoft.com/fwlink/?LinkId=255555

[Azure の Windows ストレージ アカウントのサイズ]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure の Linux ストレージ アカウントのサイズ]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows ストレージ アカウントの料金]: /pricing/details/virtual-machines/windows/
[Linux ストレージ アカウントの料金]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png
