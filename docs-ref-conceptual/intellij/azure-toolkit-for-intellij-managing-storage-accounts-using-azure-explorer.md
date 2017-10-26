---
title: "Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する"
description: "Azure Explorer for IntelliJ を使用して Azure ストレージ アカウントを管理する方法について説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 10/19/2017
ms.author: robmcm
ms.openlocfilehash: cb79f4c10cdaf5597106590b7aaf36dec266391f
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する

Azure Toolkit for IntelliJ の一部である Azure Explorer は、IntelliJ 統合開発環境 (IDE) 内から Azure アカウントのストレージ アカウントを管理するための使いやすいソリューションを Java 開発者に提供します。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>IntelliJ でストレージ アカウントを作成する

Azure Explorer を使用してストレージ アカウントを作成するには、以下の手順を実行します。

1. 「[Azure Toolkit for IntelliJ のサインイン手順]」を使用して Azure アカウントにサインインします。 

2. **Azure Explorer** ビューで、**[Azure]** ノードを展開し、**[ストレージ アカウント]** を右クリックし、**[ストレージ アカウントの作成]** をクリックします。

   ![[ストレージ アカウントの作成] コマンド][CS01]

3. **[ストレージ アカウントの作成]** ダイアログ ボックスで、次のオプションを指定します。

   ![[新しいストレージ アカウントの作成] ダイアログ ボックス][CS02]

   * **[名前]**: 新しいストレージ アカウントの名前を指定します。

   * **[アカウントの種類]**: 作成するストレージ アカウントの種類 ("Blob Storage" など) を指定します。 詳細については、「[Azure ストレージ アカウントについて]」を参照してください。 

   * **[パフォーマンス]**: 選択された発行元からどのストレージ アカウント サービスを使用するかを指定します ("Premium" など)。 詳細については、「[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]」を参照してください。 

   * **[レプリケーション]**: ストレージ アカウントのレプリケーション ("ゾーン冗長" など) を指定します。 詳細については、「[Azure Storage のレプリケーション]」を参照してください。 

   * **[サブスクリプション]**: 新しいストレージ アカウントに使用する Azure サブスクリプションを指定します。

   * **[場所]**: ストレージ アカウントを作成する場所を指定します ("米国西部" など)。

   * **[リソース グループ]**: 仮想マシン用のリソース グループを指定します。 次のいずれかのオプションを選択します。
      * **[新規作成]**: 新しいリソース グループを作成することを指定します。
      * **[Use Existing\(既存の使用\)]**: Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。

4. 上記のオプションをすべて指定したら、**[OK]** をクリックします。

## <a name="create-a-storage-container-in-intellij"></a>IntelliJ にストレージ コンテナーを作成する

Azure Explorer を使用してストレージ コンテナーを作成するには、以下の手順を実行します。

1. Azure Explorer ビューで、コンテナーを作成するストレージ アカウントを右クリックし、**[BLOB コンテナーの作成]** をクリックします。

   ![[BLOB コンテナーの作成] コマンド][CC01]

2. **[BLOB コンテナーを作成する]** ダイアログ ボックスで、コンテナーの名前を指定し、**[OK]** をクリックします。 ストレージ コンテナーの名前付けについて詳しくは、「[コンテナー、BLOB、メタデータの名前付けと参照]」をご覧ください。

   ![[BLOB コンテナーの作成] ダイアログ ボックス][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>IntelliJ でストレージ コンテナーを削除する

Azure Explorer を使用してストレージ コンテナーを削除するには、以下の手順を実行します。

1. Azure Explorer ビューで、ストレージ コンテナーを右クリックし、**[削除]** をクリックします。

   ![[ストレージ コンテナーの削除] コマンド][DC01]

2. 確認ウィンドウで **[はい]** をクリックします。

   ![ストレージ コンテナー削除確認ウィンドウ][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>IntelliJ でストレージ アカウントを削除する

Azure Explorer を使用してストレージ アカウントを削除するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、ストレージ アカウントを右クリックし、**[削除]** を選択します。

   ![ストレージ アカウントの [削除] メニュー][DS01]

2. 確認ウィンドウで **[はい]** をクリックします。

   ![ストレージ アカウント削除確認ウィンドウ][DS02]

## <a name="next-steps"></a>次のステップ

Azure ストレージ アカウント、サイズ、および料金の詳細については、次のリソースを参照してください。

* [Microsoft Azure Storage の概要]
* [Azure ストレージ アカウントについて]
* Azure ストレージ アカウントのサイズ
  * [Azure の Windows ストレージ アカウントのサイズ]
  * [Azure の Linux ストレージ アカウントのサイズ]
* Azure ストレージ アカウントの料金
  * [Windows ストレージ アカウントの料金]
  * [Linux ストレージ アカウントの料金]

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Toolkit for IntelliJ のサインイン手順]: ./azure-toolkit-for-intellij-sign-in-instructions.md
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

[CS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
