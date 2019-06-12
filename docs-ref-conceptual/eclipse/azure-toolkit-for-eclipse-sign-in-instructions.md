---
title: Azure Toolkit for Eclipse のサインイン手順
description: Azure Toolkit for Eclipse を使用して Microsoft Azure にサインインする方法について説明します。
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
ms.openlocfilehash: b4b13de38913ae6e7ae2bb09210ac742efc9d0ad
ms.sourcegitcommit: d18d9dce22b7f7af178f756bd341433d24e3c3b5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2019
ms.locfileid: "66575311"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse のサインイン手順

Azure Toolkit for Eclipse には、Azure アカウントにサインインするための 2 つの方法が用意されています。

  - [[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-device-login)
  - [[サービス プリンシパル] によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-service-principal)

[**サインアウト**](#sign-out-of-your-azure-account)方法も用意されています。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする

[Device Login]\(デバイスのログイン\) によって Azure にサインインするには、以下を実行します。

1. Eclipse でプロジェクトを開きます。

2. **[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。
   ![Azure サインイン用の Eclipse メニュー][I01]

3. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、 **[サインイン]** をクリックします。

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

4. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

> [!NOTE]
>
> ブラウザーが開かない場合は、Internet Explorer、Firefox、Chrome などの外部ブラウザーを使用するように Eclipse を構成します。
>
> 1. [Preferences]\(設定\) - > [General]\(全般\) - > [Web Browser ]\(Web ブラウザー\) - > [Use external web browser in Eclipse]\(Eclipse で外部 Web ブラウザーを使用する\) の順に開きます。
>
> 2. 使用するブラウザーを選択します
>

5. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

   ![デバイスのログイン ブラウザー][I04]

6. 最後に、 **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>[サービス プリンシパル] によって Azure アカウントにサインインする

このセクションでは、サービス プリンシパル データを格納する資格情報ファイルを作成します。 このプロセスが完了すると、Eclipse は資格情報ファイルを使用して、プロジェクトを開いたときにユーザーを自動的に Azure にサインインします。

1. Eclipse でプロジェクトを開きます。

2. **[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。
   ![Eclipse Azure サインイン コマンド][A01]

3. **[Azure サインイン]** ウィンドウで、 **[サービス プリンシパル]** を選択します。 サービス プリンシパル認証ファイルがまだない場合は、 **[新規]** をクリックして作成します。 そうでない場合は、 **[参照]** をクリックしてそれを開き、ステップ 8 に進むことができます。

   ![[サービス プリンシパル] が選択されている [Azure サインイン] ウィンドウ][A02]

4. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

   ![[Azure ログイン] ダイアログ ウィンドウ][A08]

> [!NOTE]
>
> ブラウザーが開かない場合は、IE や Chrome などの外部ブラウザーを使用するように Eclipse を構成します。
>
> 1. [Preferences]\(設定\) - > [General]\(全般\) - > [Web Browser ]\(Web ブラウザー\) - > [Use external web browser in Eclipse]\(Eclipse で外部 Web ブラウザーを使用する\) の順に開きます。
>
> 2. 使用するブラウザーを選択します
>

5. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

   ![デバイスのログイン ブラウザー][A03]

6. **[Create authentication files]\(認証ファイルの作成\)** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選択し、 **[開始]** をクリックします。

   ![[Create authentication files]\(認証ファイルの作成\) ウィンドウ][A04]

7. **[Service Principal Creatation Status]\(サービス プリンシパル作成状態\)** ダイアログ ボックスで、ファイルが正常に作成されたら **[OK]** をクリックします。

   ![[Service Principal Creatation Status]\(サービス プリンシパル作成ステータス\) ダイアログ ボックス][A05]

8. 作成されたファイルのアドレスは **[Azure サインイン]** ウィンドウに自動的に入力されるため、ここで **[サインイン]** をクリックします。

   ![Azure のログイン ダイアログ ボックス][A06]

9. 最後に、 **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][A07]

## <a name="sign-out-of-your-azure-account"></a>Azureアカウントからサインアウトする

前の手順によってアカウントを構成すると、Eclipse を起動するたびに自動的にサインインします。 ただし、Azure アカウントからサインアウトするには、次の手順を使用します。

1. Eclipse で、 **[ツール]** 、 **[Azure]** 、 **[サインアウト]** の順にクリックします。

   ![Azure サインアウト用の Eclipse メニュー][L01]

2. **[Azure Sign Out (Azure サインアウト)]** ダイアログ ボックスが表示されたら、 **[はい]** をクリックします。

   ![[サインアウト] ダイアログ ボックス][L02]

## <a name="next-steps"></a>次の手順

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

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
