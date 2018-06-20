---
title: Java 用 Azure ライブラリ パッケージの Javadoc コンテンツの Eclipse での表示
description: Azure ライブラリの Javadoc コンテンツを Eclipse で表示する方法
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 6f1edcc1411e8ec716dbfe41271d21d2397515c5
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954503"
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Java 用 Azure ライブラリ パッケージの Javadoc コンテンツの Eclipse での表示

Java 用 Azure ライブラリの Javadoc コンテンツは、Javadoc コンテンツを Java 用 Azure ライブラリに関連付けることで、Eclipse 環境内で表示できます。 次の手順は、Eclipse 内でこの機能を使用する方法を示しています。

この手順は、Java 用 Azure ライブラリをビルド パスに既に追加していることを前提としています。

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Java 用 Azure ライブラリの Javadoc コンテンツを Eclipse で表示するには

1. Eclipse の [Project Explorer (プロジェクト エクスプローラー)] で、プロジェクトの **[Referenced Libraries (参照ライブラリ)]** セクションにある Java JAR 用 Azure ライブラリ (例: **microsoft-windowsazure-api-0.1.0.jar** (インストールされているバージョンによって、バージョン番号が異なることがあります)) のコンテキスト メニューを開きます。

1. **[プロパティ]** をクリックします。

1. **[Properties (プロパティ)]** ダイアログの左側のウィンドウで、**[Javadoc Location (Javadoc の場所)]** をクリックします。 **[Javadoc Location]** ダイアログが表示されます。

1. **[Javadoc URL (Javadoc の URL)]** または **[Javadoc in archive (アーカイブの Javadoc)]** を指定できます。

   * **[Javadoc URL (Javadoc の URL)]** を選択した場合は、**http://dl.windowsazure.com/javadoc** または **http://dl.windowsazure.com/storage/javadoc** などの URL を使用します。

   * **[Javadoc in archive]** を使用する場合は、外部ファイルまたはワークスペース ファイルを指定できます。

   選択したら、必要に応じて参照または検証します。 次の例では、Java 用 Azure ライブラリを、**c:\MyAzureJARs** という名前のローカル フォルダーにダウンロードされている対応する Javadoc JAR に関連付けています。

   ![Javadoc コンテンツの表示][ic553487]

1. "*省略可能な手順*": **[検証]** をクリックします。 Javadoc JAR に関する潜在的な問題があれば、ここに表示されます。

1. Click **OK**.

ライブラリに関連付けると、Javadoc コンテンツが Eclipse IDE 内で表示されます。 たとえば、コードで `CloudBlockBlob` 型の `blob` が定義されている場合、コードに「`blob.acquireLease`」と入力すると、次のような Javadoc コンテンツが表示されます。

![Javadoc コンテンツの表示][ic553488]

## <a name="next-steps"></a>次の手順

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
