---
title: "Azure Toolkit for Eclipse のサインイン手順"
description: "Azure Toolkit for Eclipse を使用して Microsoft Azure にサインインする方法について説明します。"
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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: ee87b73841013eafb47d00e0cf5028d6b3ff932c
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/18/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse の Azure サインイン手順

Azure Toolkit for Eclipse には、Azure アカウントにサインインするための 2 つの方法が用意されています。

  * **自動** - この方法を使用する場合は、サービス プリンシパル データを含む資格情報ファイルを作成し、その後で、資格情報ファイルを使用して Azure アカウントに自動的にサインインできます。
  * **対話型** - この方法を使用する場合は、Azure アカウントにサインインするたびに Azure 資格情報を入力します。

以下のセクションの手順では、各方法の使用方法について説明します。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a>Azure アカウントに自動的にサインインし、今後使用する資格情報ファイルを作成する

次の手順は、サービス プリンシパル データを格納する資格情報ファイルを作成する方法を説明しています。 これらの手順を完了すると、Eclipse は自動的に資格情報ファイルを使用し、プロジェクトを開くたびに Azure に自動的にサインインします。

1. Eclipse でプロジェクトを開きます。

1. **[ツール]**、**[Azure]**、**[サインイン]** の順にクリックします。

   ![Azure サインイン用の Eclipse メニュー][A01]

1. **[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[自動]** を選択し、**[新規]** をクリックします。

   ![[サインイン] ダイアログ ボックス][A02]

1. **[Azure Log In (Azure ログイン)]** ダイアログ ボックスが表示されたら、Azure 資格情報を入力し、**[サインイン]** をクリックします。

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A03]

1. **[Create authentication files (認証ファイルの作成)]**  ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選んでから、**[開始]** をクリックします。

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A04]

1. **[Service Principal Creatation Status (サービス プリンシパル作成ステータス)]** ダイアログ ボックスが表示されます。ファイルが正常に作成されたら **[OK]** をクリックします。

   ![[Service Principal Creatation Status (サービス プリンシパル作成ステータス)] ダイアログ ボックス][A05]

1. **[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[サインイン]** をクリックします。

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A06]

1. **[サブスクリプションの選択]** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、**[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>自動的にサインインした場合に Azure アカウントからサインアウトする

前のセクションの手順を構成した後、Eclipse を再起動するたびに Azure Toolkit によって Azure アカウントに自動的にサインインします。 ただし、Azure アカウントからサインアウトし、Azure Toolkit が自動的にサインインしないようにするには、次の手順を使用します。

1. Eclipse で、**[ツール]**、**[Azure]**、**[サインアウト]** の順にクリックします。

   ![Azure サインアウト用の Eclipse メニュー][L01]

1. **[Azure Sign Out (Azure サインアウト)]** ダイアログ ボックスが表示されたら、**[はい]** をクリックします。

   ![[サインアウト] ダイアログ ボックス][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>既に作成した資格情報ファイルを使用して Azure アカウントに自動的にサインインする

Eclipse を使用しているときに Azure からサインアウトする場合、Azure アカウントに自動的にサインインするには、資格情報ファイルを使用するように Azure Toolkit for Eclipse を再構成する必要があります。 次の手順は、既存の資格情報ファイルを使用するように Azure Toolkit を構成する方法を説明しています。

1. Eclipse でプロジェクトを開きます。

1. **[ツール]**、**[Azure]**、**[サインイン]** の順にクリックします。

   ![Azure サインイン用の Eclipse メニュー][A01]

1. **[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[自動]** を選択し、**[参照]** をクリックします。

   ![[サインイン] ダイアログ ボックス][A02]

1. **[Select Authenticated File (認証ファイルの選択)**] ダイアログ ボックスが表示されたら、前に作成した資格情報ファイルを選び、**[選択]** をクリックします。

   ![[サインイン] ダイアログ ボックス][A08]

1. **[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[サインイン]** をクリックします。

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][A06]

1. **[サブスクリプションの選択]** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、**[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="signing-into-your-azure-account-interactively"></a>Azure アカウントに対話形式でサインインする

次の手順は、Azure 資格情報を手動で入力して Azure にサインインする方法を示しています。

1. Eclipse でプロジェクトを開きます。

1. **[ツール]**、**[Azure]**、**[サインイン]** の順にクリックします。

   ![Azure サインイン用の Eclipse メニュー][I01]

1. **[Azure Sign In (Azure サインイン)]** ダイアログ ボックスが表示されたら、**[Interactive (対話型)]** を選択し、**[サインイン]** をクリックします。

   ![[サインイン] ダイアログ ボックス][I02]

1. **[Azure Log In (Azure ログイン)]** ダイアログ ボックスが表示されたら、Azure 資格情報を入力し、**[サインイン]** をクリックします。

   ![[Azure Log In (Azure ログイン)] ダイアログ ボックス][I03]

1. **[サブスクリプションの選択]** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、**[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>対話形式でサインインした場合に Azure アカウントからサインアウトする

前のセクションの手順を構成した後、Eclipse を再起動するたびに Azure アカウントから自動的にサインアウトします。 ただし、Eclipse を再起動せずに Azure アカウントからサインアウトするには、次の手順を使用します。

1. Eclipse で、**[ツール]**、**[Azure]**、**[サインアウト]** の順にクリックします。

   ![Azure サインアウト用の Eclipse メニュー][L01]

1. **[Azure Sign Out (Azure サインアウト)]** ダイアログ ボックスが表示されたら、**[はい]** をクリックします。

   ![[サインアウト] ダイアログ ボックス][L02]

## <a name="next-steps"></a>次のステップ

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
