---
title: Azure Explorer for Eclipse を使用して仮想マシンを管理する
description: Azure Explorer for Eclipse を使用して Azure 仮想マシンを管理する方法について説明します。
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
ms.openlocfilehash: c04f5225f0bb99898f69b26a4782aa57d75c4f22
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2018
ms.locfileid: "48899367"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a>Azure Explorer for Eclipse を使用して仮想マシンを管理する

Azure Toolkit for Eclipse の一部である Azure Explorer は、Eclipse 統合開発環境 (IDE) 内から Azure アカウントの仮想マシンを管理するための使いやすいソリューションを Java 開発者に提供します。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a>Eclipse で仮想マシンを作成する

Azure Explorer を使用して仮想マシンを作成するには、以下の手順を実行します。

1. 「[Azure Toolkit for Eclipse のサインイン手順](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)」に従って Azure アカウントにサインインします。

2. **Azure Explorer** ビューで、**[Azure]** ノードを展開し、**[仮想マシン]** を右クリックし、**[VM の作成]** をクリックします。

   ![[VM の作成] コマンド][CR01]  

   **[新しい仮想マシンの作成]** ウィザードが開きます。

3. **[サブスクリプションの選択]** ウィンドウで、サブスクリプションを選択し、**[次へ]** をクリックします。

   ![[サブスクリプションの選択] ウィンドウ][CR02]

4. **[仮想マシン イメージの選択]** ウィンドウで、次の情報を入力します。

   * **[場所]**: 仮想マシンを作成する場所を指定します ("*米国西部*" など)。

   * **[発行者]**: 仮想マシンの作成に使用するイメージを作成した発行元を指定します (*Microsoft* など)。

   * **[プラン]**: 選択した発行元の、仮想マシンで使用するプランを指定します (*JDK* など)。

   * **[SKU]**: 選択したプランで使用する在庫保管単位 (SKU) を指定します (*JDK_8* など)。

   * **[Version #]\(バージョン番号\)**: 選択した SKU で使用するバージョンを指定します。

   ![[仮想マシン イメージの選択] ウィンドウ][CR03]

5. **[次へ]** をクリックします。

6. **[仮想マシンの基本設定]** ウィンドウで、次の情報を入力します。

   * **[仮想マシン名]**: 新しい仮想マシンの名前を指定します。英字、数字、ハイフンのみ使用でき、先頭には英字を使用する必要があります。

   * **[サイズ]**: 仮想マシンに割り当てるコアとメモリの数を指定します。

   * **[ユーザー名]**: 仮想マシンを管理するために作成する管理者アカウントを指定します。

   * **[パスワード]** と **[確認]**: 管理者アカウントのパスワードを指定します。

   ![[仮想マシンの基本設定] ウィンドウ][CR04]

7. **[次へ]** をクリックします。

8. **[新しいストレージ アカウントの作成]** ウィンドウで、次の情報を入力します。

   * **[リソース グループ]**: 仮想マシン用のリソース グループを指定します。 次のいずれかのオプションを選択します。
     * **[新規作成]**: 新しいリソース グループを作成することを指定します。
     * **[既存のものを使用]**: Azure アカウントに既に関連付けられているリソース グループの一覧から選択することを指定します。

       ![[新しいストレージ アカウントの作成] ダイアログ ボックス][CR05]

   * **[ストレージ アカウント]**: 仮想マシンを格納するために使用するストレージ アカウントを指定します。 既存のストレージ アカウントを使用するか、新しいアカウントを作成できます。

   * **[仮想ネットワーク]** と **[サブネット]**: 仮想マシンを接続する仮想ネットワークとサブネットを指定します。 既存のネットワークとサブネットを使用するか、新しいネットワークとサブネットを作成できます。 **[新規作成]** を選択した場合は、次のダイアログ ボックスが表示されます。

      ![[新しい仮想ネットワークの作成] ダイアログ ボックス][CR06]

9. **[関連するリソース]** ウィンドウで、次の情報を入力します。

   * **[パブリック IP アドレス]**: 仮想マシンの外部接続 IP アドレスを指定します。 新しい IP アドレスを作成できます。仮想マシンのパブリック IP アドレスがない場合は、**[(なし)]** を選択できます。

   * **[ネットワーク セキュリティ グループ]**: 仮想マシンのネットワーク ファイアウォールを指定します (省略可能)。 既存のファイアウォールを選択できます。仮想マシンでネットワーク ファイアウォールを使用しない場合は、**[(なし)]** を選択できます。

   * **[可用性セット]**: 仮想マシンが属することができる可用性セットを指定します (省略可能)。 既存の可用性セットを指定するか、新しい可用性セットを作成できます。仮想マシンが可用性セットに属さない場合は、**[(なし)]** を選択します。

   ![[関連するリソース] ウィンドウ][CR07]

10. **[完了]** をクリックします。  

    新しい仮想マシンが Azure エクスプローラーのツール ウィンドウに表示されます。

    ![新しい仮想マシン][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a>Eclipse で仮想マシンを再起動する

Eclipse で Azure Explorer を使用して仮想マシンを再起動するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、仮想マシンを右クリックし、**[再起動]** を選択します。

   ![仮想マシンの [再起動] コマンド][RE01]

1. 確認ウィンドウで **[はい]** をクリックします。

   ![再起動の確認ウィンドウ][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a>Eclipse で仮想マシンをシャットダウンする

Eclipse で Azure Explorer を使用して実行中の仮想マシンをシャットダウンするには、以下の手順を実行します。

1. **Azure Explorer** ビューで、仮想マシンを右クリックし、**[シャットダウン]** を選択します。

   ![仮想マシンの [シャットダウン] コマンド][SH01]

1. 確認ウィンドウで **[はい]** をクリックします。

   ![仮想マシンのシャットダウン確認ウィンドウ][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a>Eclipse で仮想マシンを削除する

Eclipse で Azure Explorer を使用して仮想マシンを削除するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、仮想マシンを右クリックし、**[削除]** を選択します。

   ![仮想マシンの [削除] コマンド][DE01]

1. 確認ウィンドウで **[はい]** をクリックします。

   ![仮想マシンの削除確認ウィンドウ][DE02]

## <a name="next-steps"></a>次の手順

Azure 仮想マシンのサイズと料金について詳しくは、次のリソースを参照してください。

* Azure 仮想マシンのサイズ
  * [Azure の Windows 仮想マシンのサイズ]
  * [Azure の Linux 仮想マシンのサイズ]
* Azure 仮想マシンの料金
  * [Windows 仮想マシンの料金]
  * [Linux 仮想マシンの料金]

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure の Windows 仮想マシンのサイズ]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure の Linux 仮想マシンのサイズ]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 仮想マシンの料金]: /pricing/details/virtual-machines/windows/
[Linux 仮想マシンの料金]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
