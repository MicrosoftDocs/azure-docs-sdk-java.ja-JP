---
title: "Java 用 Azure ライブラリ パッケージの Javadoc コンテンツの Eclipse での表示"
description: "Azure ライブラリの Javadoc コンテンツを Eclipse で表示する方法"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 2d1f03f993f0c88401efaa9985f773b9a4b92cc4
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a><span data-ttu-id="c3874-103">Java 用 Azure ライブラリ パッケージの Javadoc コンテンツの Eclipse での表示</span><span class="sxs-lookup"><span data-stu-id="c3874-103">Displaying Javadoc Content in Eclipse for the Azure Libraries Package for Java</span></span>

<span data-ttu-id="c3874-104">Java 用 Azure ライブラリの Javadoc コンテンツは、Javadoc コンテンツを Java 用 Azure ライブラリに関連付けることで、Eclipse 環境内で表示できます。</span><span class="sxs-lookup"><span data-stu-id="c3874-104">The Javadoc content for the Azure Libraries for Java can be viewed within your Eclipse environment by associating the Javadoc content to the Azure Libraries for Java.</span></span> <span data-ttu-id="c3874-105">次の手順は、Eclipse 内でこの機能を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="c3874-105">The following steps show you how to use this functionality within Eclipse.</span></span>

<span data-ttu-id="c3874-106">この手順は、Java 用 Azure ライブラリをビルド パスに既に追加していることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="c3874-106">This procedure assumes you have already added the Azure Library for Java to your build path.</span></span>

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a><span data-ttu-id="c3874-107">Java 用 Azure ライブラリの Javadoc コンテンツを Eclipse で表示するには</span><span class="sxs-lookup"><span data-stu-id="c3874-107">To display Javadoc content in Eclipse for the Azure Libraries for Java</span></span>

1. <span data-ttu-id="c3874-108">Eclipse の [Project Explorer (プロジェクト エクスプローラー)] で、プロジェクトの **[Referenced Libraries (参照ライブラリ)]** セクションにある Java JAR 用 Azure ライブラリ</span><span class="sxs-lookup"><span data-stu-id="c3874-108">Within Eclipse's Project Explorer, in the **Referenced Libraries** section of your project, open the context menu for the Azure Library for Java JAR.</span></span> <span data-ttu-id="c3874-109">(例: **microsoft-windowsazure-api-0.1.0.jar** (インストールされているバージョンによって、バージョン番号が異なることがあります)) のコンテキスト メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="c3874-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (the version number may be different, dependent upon which version you have installed).</span></span>

1. <span data-ttu-id="c3874-110">**[プロパティ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3874-110">Click **Properties**.</span></span>

1. <span data-ttu-id="c3874-111">**[Properties (プロパティ)]** ダイアログの左側のウィンドウで、**[Javadoc Location (Javadoc の場所)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3874-111">Within the **Properties** dialog, in the left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="c3874-112">**[Javadoc Location]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c3874-112">The **Javadoc Location** dialog is displayed.</span></span>

1. <span data-ttu-id="c3874-113">**[Javadoc URL (Javadoc の URL)]** または **[Javadoc in archive (アーカイブの Javadoc)]** を指定できます。</span><span class="sxs-lookup"><span data-stu-id="c3874-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="c3874-114">**[Javadoc URL (Javadoc の URL)]** を選択した場合は、**http://dl.windowsazure.com/javadoc** または **http://dl.windowsazure.com/storage/javadoc** などの URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="c3874-114">If you choose to specify a **Javadoc URL**, use the URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="c3874-115">**[Javadoc in archive]**を使用する場合は、外部ファイルまたはワークスペース ファイルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="c3874-115">If you choose to use **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="c3874-116">選択したら、必要に応じて参照または検証します。</span><span class="sxs-lookup"><span data-stu-id="c3874-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="c3874-117">次の例では、Java 用 Azure ライブラリを、**c:\MyAzureJARs** という名前のローカル フォルダーにダウンロードされている対応する Javadoc JAR に関連付けています。</span><span class="sxs-lookup"><span data-stu-id="c3874-117">The following example associates the Azure Libraries for Java with the corresponding Javadoc JAR that has been downloaded locally to a folder named **c:\MyAzureJARs**.</span></span>

   ![Javadoc コンテンツの表示][ic553487]

1. <span data-ttu-id="c3874-119">"*省略可能な手順*": **[検証]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3874-119">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="c3874-120">Javadoc JAR に関する潜在的な問題があれば、ここに表示されます。</span><span class="sxs-lookup"><span data-stu-id="c3874-120">Potential issues with the Javadoc JAR could be displayed here.</span></span>

1. <span data-ttu-id="c3874-121">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3874-121">Click **OK**.</span></span>

<span data-ttu-id="c3874-122">ライブラリに関連付けると、Javadoc コンテンツが Eclipse IDE 内で表示されます。</span><span class="sxs-lookup"><span data-stu-id="c3874-122">Once associated with the library, the Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="c3874-123">たとえば、コードで `CloudBlockBlob` 型の `blob` が定義されている場合、コードに「`blob.acquireLease`」と入力すると、次のような Javadoc コンテンツが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c3874-123">For example, if `blob` is defined of type `CloudBlockBlob` within your code, the following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![Javadoc コンテンツの表示][ic553488]

## <a name="next-steps"></a><span data-ttu-id="c3874-125">次のステップ</span><span class="sxs-lookup"><span data-stu-id="c3874-125">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
