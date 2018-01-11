---
title: "Azure Toolkit for IntelliJ のインストール"
description: "Azure Toolkit for IntelliJ プラグインをインストールして、クラウド アプリケーションを作成し、Azure にデプロイする方法を説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 88476142dbd6fbe05d3d59d14cf71c83893e6452
ms.sourcegitcommit: 9c354a65b0f8ad49a528f40ddee647b091f7d246
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2018
---
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="5dd85-103">Azure Toolkit for IntelliJ のインストール</span><span class="sxs-lookup"><span data-stu-id="5dd85-103">Installing the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="5dd85-104">Azure Toolkit for IntelliJ は、IntelliJ IDEA 開発環境を使ってクラウド アプリケーションを簡単に作成、開発、およびテストし、Azure にデプロイできるテンプレートと機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="5dd85-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy cloud application to Azure using the IntelliJ IDEA development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5dd85-105">Azure Toolkit for IntelliJ はオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから MIT License で入手できます。URL は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5dd85-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <span data-ttu-id="5dd85-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="5dd85-106"><https://github.com/microsoft/azure-tools-for-java></span></span> 
> 

<span data-ttu-id="5dd85-107">Azure Toolkit for IntelliJ には 2 とおりのインストール方法があります。**[設定]** ダイアログ ボックスを使用する方法と、スタート画面の **[構成]** メニューを使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="5dd85-107">There are two methods of installing the Azure Toolkit for IntelliJ: by using the **Settings** dialog box, and by using the **Configure** menu on the start screen.</span></span> <span data-ttu-id="5dd85-108">以降の手順では、両方のインストール方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="5dd85-108">Both installation methods will be demonstrated in the following steps.</span></span>

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="5dd85-109">Azure Toolkit for IntelliJ を [Settings]\(設定) ダイアログ ボックスからインストールするには</span><span class="sxs-lookup"><span data-stu-id="5dd85-109">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>

1. <span data-ttu-id="5dd85-110">IntelliJ IDEA を起動します。</span><span class="sxs-lookup"><span data-stu-id="5dd85-110">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="5dd85-111">IntelliJ IDEA が起動したら **[File (ファイル)]** をクリックし、**[Settings (設定)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-111">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
   ![Open the IntelliJ IDEA Settings Dialog Box][01a]

1. <span data-ttu-id="5dd85-113">[Settings (設定)] ダイアログ ボックスの **[Plugins (プラグイン)]** をクリックし、**[Browse repositories (リポジトリの参照)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-113">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
   ![IntelliJ IDEA Settings Dialog Box][02a]

1. <span data-ttu-id="5dd85-115">**[Browse Repositories]** (リポジトリの参照) ダイアログ ボックスの検索ボックスに「Azure」と入力します。</span><span class="sxs-lookup"><span data-stu-id="5dd85-115">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="5dd85-116">**Azure Toolkit for IntelliJ** を強調表示して **Install (インストール)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-116">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Search for the Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="5dd85-118">インストールの進行状況がダイアログ ボックスに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5dd85-118">IntelliJ IDEA displays the installation progress in a dialog box.</span></span>
   
   ![Installation progress][04]

1. <span data-ttu-id="5dd85-120">インストールが完了したら、 **[Restart IntelliJ IDEA]**(IntelliJ IDEA の再起動) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-120">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![[Restart IntelliJ IDEA]][05]

1. <span data-ttu-id="5dd85-122">**[OK]** をクリックして [Settings]\(設定) ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="5dd85-122">Click **OK** to close the Settings dialog box.</span></span>
   
   ![Close IntelliJ IDEA Settings Dialog Box][06]

1. <span data-ttu-id="5dd85-124">IntelliJ IDEA をすぐに再起動するか、後から再起動するかを求められたら、 **[Restart]**(再起動) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-124">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
<span data-ttu-id="5dd85-125">1</span><span class="sxs-lookup"><span data-stu-id="5dd85-125">1</span></span>   ![[Restart IntelliJ IDEA]][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="5dd85-127">Azure Toolkit for IntelliJ をスタート画面からインストールするには</span><span class="sxs-lookup"><span data-stu-id="5dd85-127">To install the Azure Toolkit for IntelliJ from the start screen</span></span>

1. <span data-ttu-id="5dd85-128">IntelliJ IDEA を起動します。</span><span class="sxs-lookup"><span data-stu-id="5dd85-128">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="5dd85-129">IntelliJ IDEA のスタート画面が表示されたら、**[Configure (構成)]** をクリックし、**[Plugins (プラグイン)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-129">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
   ![Install IntelliJ IDEA Plugins][01b]

1. <span data-ttu-id="5dd85-131">**[Plugins (プラグイン)]** ダイアログ ボックスで **[Browse repositories (リポジトリの参照)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-131">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
   ![Browse IntelliJ IDEA Plugin Repositories][02b]

1. <span data-ttu-id="5dd85-133">**[Browse Repositories]** (リポジトリの参照) ダイアログ ボックスの検索ボックスに「Azure」と入力します。</span><span class="sxs-lookup"><span data-stu-id="5dd85-133">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="5dd85-134">**Azure Toolkit for IntelliJ** を強調表示して **Install (インストール)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-134">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![Search for the Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="5dd85-136">インストールの進行状況がダイアログ ボックスに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5dd85-136">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
   ![Installation progress][04]

1. <span data-ttu-id="5dd85-138">インストールが完了したら、 **[Restart IntelliJ IDEA]**(IntelliJ IDEA の再起動) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-138">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![[Restart IntelliJ IDEA]][05]

1. <span data-ttu-id="5dd85-140">IntelliJ IDEA をすぐに再起動するか、後から再起動するかを求められたら、 **[Restart]**(再起動) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5dd85-140">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
   ![[Restart IntelliJ IDEA]][07]

## <a name="next-steps"></a><span data-ttu-id="5dd85-142">次の手順</span><span class="sxs-lookup"><span data-stu-id="5dd85-142">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[01a]: media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
