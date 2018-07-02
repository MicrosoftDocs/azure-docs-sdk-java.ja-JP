---
title: Azure Toolkit for Eclipse を使用して Docker コンテナーを発行する
description: Azure Toolkit for Eclipse を使用して、Web アプリを Docker コンテナーとして Microsoft Azure に発行する方法について説明します。
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
ms.openlocfilehash: be76733bffa36160d6e366c383672a15374a9996
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090845"
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="9bfca-103">Azure Toolkit for Eclipse を使用して Web アプリを Docker コンテナーとして発行する</span><span class="sxs-lookup"><span data-stu-id="9bfca-103">Publish a web app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="9bfca-104">Docker コンテナーは、Web アプリケーションをデプロイするための広範に使用されている方法です。</span><span class="sxs-lookup"><span data-stu-id="9bfca-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="9bfca-105">Docker コンテナーを使用すると、開発者はすべてのプロジェクト ファイルおよび依存関係を、サーバーにデプロイするための 1 つのパッケージに統合できます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="9bfca-106">Azure Toolkit for Eclipse は、Microsoft Azure にデプロイするための *[Docker コンテナーとして発行]* 機能を追加することによって、Java 開発者のこのプロセスを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="9bfca-107">この記事では、アプリケーションを Docker コンテナーとして Azure に発行するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="9bfca-108">Docker の詳細については、[Docker の Web サイト]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9bfca-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="9bfca-109">Docker コンテナーを使用して Web アプリを Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="9bfca-109">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="9bfca-110">Eclipse で、Web アプリ プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-110">Open your web app project in Eclipse.</span></span>

2. <span data-ttu-id="9bfca-111">**[Docker コンテナーとして発行]** ウィザードを起動するには、次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-111">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="9bfca-112">**[ナビゲーター]** ビューで、プロジェクトを右クリックし、**[Azure]** をクリックしてから **[Docker コンテナーとして発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-112">In the **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![ナビゲーター ビューの [Docker コンテナーとして発行] コマンド][PUB01]

   * <span data-ttu-id="9bfca-114">Eclipse ツールバーの **[発行]** ボタンをクリックしてから、**[Docker コンテナーとして発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-114">On the Eclipse toolbar, click the **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Eclipse ツールバーの [Docker コンテナーとして発行] コマンド][PUB02]
      
   <span data-ttu-id="9bfca-116">**[Deploy Docker Container on Azure] \(Azure への Docker コンテナーのデプロイ)** ウィザードが開きます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-116">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![[Deploy Docker Container on Azure] \(Azure への Docker コンテナーのデプロイ) ウィザード][PUB03]

3. <span data-ttu-id="9bfca-118">**[Type image name, select artifact's path and check Docker host to be used] \(イメージ名を入力し、アーティファクトのパスを選択して、使用される Docker ホストを確認する)** ウィンドウで、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-118">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span>

   <span data-ttu-id="9bfca-119">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-119">a.</span></span> <span data-ttu-id="9bfca-120">**[Docker image name] \(Docker イメージ名)** ボックスに、Docker ホストの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-120">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="9bfca-121">(このウィザードでは名前が自動的に作成されますが、それは変更できます)。</span><span class="sxs-lookup"><span data-stu-id="9bfca-121">(The wizard automatically creates a name, but you can modify it.)</span></span>

   <span data-ttu-id="9bfca-122">b.</span><span class="sxs-lookup"><span data-stu-id="9bfca-122">b.</span></span> <span data-ttu-id="9bfca-123">**[ホスト]** 領域には、既に作成しているすべての Docker ホストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-123">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="9bfca-124">次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-124">Do either of the following:</span></span>

   * <span data-ttu-id="9bfca-125">既存の Docker ホストがある場合は、それに Web アプリをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-125">If you have an existing Docker host, you can deploy your web app to it.</span></span>
   * <span data-ttu-id="9bfca-126">新しい Docker ホストを作成するには、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-126">To create a new Docker host, click **Add**.</span></span>  
      
      <span data-ttu-id="9bfca-127">**[Create Docker Host] \(Docker ホストの作成)** ダイアログ ボックスが開きます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-127">The **Create Docker Host** dialog box opens.</span></span>

   ![[Deploy Docker Container on Azure] \(Azure への Docker コンテナーのデプロイ) ウィザード][PUB04a]

4. <span data-ttu-id="9bfca-129">**[Configure the new virtual machine] \(新しい仮想マシンの構成)** ウィンドウで、Docker ホストの次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-129">In the **Configure the new virtual machine** window, specify the following options for your Docker host.</span></span> <span data-ttu-id="9bfca-130">(このウィザードではほとんどのオプションが自動的に生成されますが、いずれのオプションも変更できます)。</span><span class="sxs-lookup"><span data-stu-id="9bfca-130">(The wizard automatically generates most of the options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="9bfca-131">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-131">a.</span></span> <span data-ttu-id="9bfca-132">**[名前]**: Docker ホストの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-132">**Name**: Enter a unique name for the Docker host.</span></span> <span data-ttu-id="9bfca-133">(これは、前に指定した Docker イメージ名と同じではありません)。</span><span class="sxs-lookup"><span data-stu-id="9bfca-133">(It is not the same as the Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="9bfca-134">b.</span><span class="sxs-lookup"><span data-stu-id="9bfca-134">b.</span></span> <span data-ttu-id="9bfca-135">**[サブスクリプション]**: ホストに使用する Azure サブスクリプションを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-135">**Subscription**: Enter the Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="9bfca-136">c.</span><span class="sxs-lookup"><span data-stu-id="9bfca-136">c.</span></span> <span data-ttu-id="9bfca-137">**[リージョン]**: ホストが配置される地理的リージョンを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-137">**Region**: Enter the geographical region where your host is located.</span></span>

   <span data-ttu-id="9bfca-138">d.</span><span class="sxs-lookup"><span data-stu-id="9bfca-138">d.</span></span> <span data-ttu-id="9bfca-139">**[Host OS and Size] \(ホスト OS とサイズ)** タブ:</span><span class="sxs-lookup"><span data-stu-id="9bfca-139">On the **Host OS and Size** tab:</span></span> 
   * <span data-ttu-id="9bfca-140">**[ホスト OS]**: ホストが含まれている仮想マシンのオペレーティング システムを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-140">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span>
   * <span data-ttu-id="9bfca-141">**[サイズ]**: ホストの仮想マシンのサイズを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-141">**Size**: Enter the virtual-machine size for your host.</span></span>

   <span data-ttu-id="9bfca-142">e.</span><span class="sxs-lookup"><span data-stu-id="9bfca-142">e.</span></span> <span data-ttu-id="9bfca-143">**[リソース グループ]** タブ:</span><span class="sxs-lookup"><span data-stu-id="9bfca-143">On the **Resource Group** tab:</span></span> 
   * <span data-ttu-id="9bfca-144">**[新しいリソース グループ]**: ホストの新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-144">**New resource group**: Create a new resource group for your host.</span></span>
   * <span data-ttu-id="9bfca-145">**[Existing resource group] \(既存のリソース グループ)**: Azure アカウントの既存のリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="9bfca-146">f.</span><span class="sxs-lookup"><span data-stu-id="9bfca-146">f.</span></span> <span data-ttu-id="9bfca-147">**[ネットワーク]** タブ:</span><span class="sxs-lookup"><span data-stu-id="9bfca-147">On the **Network** tab:</span></span> 
   * <span data-ttu-id="9bfca-148">**[新しい仮想ネットワーク]**: ホストの新しい仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-148">**New virtual network**: Create a new virtual network for your host.</span></span>
   * <span data-ttu-id="9bfca-149">**[既存の仮想ネットワーク]**: Azure アカウントの既存の仮想ネットワークを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="9bfca-150">g.</span><span class="sxs-lookup"><span data-stu-id="9bfca-150">g.</span></span> <span data-ttu-id="9bfca-151">**[ストレージ]** タブ:</span><span class="sxs-lookup"><span data-stu-id="9bfca-151">On the **Storage** tab:</span></span> 
   * <span data-ttu-id="9bfca-152">**[新しいストレージ アカウント]**: ホストの新しいストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-152">**New storage account**: Create a new storage account for your host.</span></span>
   * <span data-ttu-id="9bfca-153">**[既存のストレージ アカウント]**: Azure アカウントの既存のストレージ アカウントを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

5. <span data-ttu-id="9bfca-154">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-154">Click **Next**.</span></span>

6. <span data-ttu-id="9bfca-155">**[Configure log in credentials and port settings] \(ログイン資格情報とポート設定の構成)** ウィンドウで、次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-155">In the **Configure log in credentials and port settings** window, select one of the following options:</span></span>

   * <span data-ttu-id="9bfca-156">**[Import credentials from Azure Key Vault] \(Azure Key Vault からの資格情報のインポート)**: Azure サブスクリプションに格納されている、以前に保存された一連の資格情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span> 

   >[!NOTE]
   ><span data-ttu-id="9bfca-157">特定のアカウントまたはサービス プリンシパルで作成された Azure Key Vault は、サブスクリプションを共有する別のアカウントまたはサービス プリンシパルから自動的にアクセスできるようにはなりません。</span><span class="sxs-lookup"><span data-stu-id="9bfca-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="9bfca-158">別のアカウントまたはサービス プリンシパルが Key Vault を使用できるようにするには、Azure Portal を使用して、アカウントまたはサービス プリンシパルを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9bfca-158">To allow another account or service principal to use the Key Vault, you must use the Azure portal to add the account or service principal.</span></span>
   >

   * <span data-ttu-id="9bfca-159">**[New log in credential] \(新しいログイン資格情報)**: 新しい一連のログイン資格情報を作成します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="9bfca-160">このオプションを選択する場合は、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="9bfca-160">If you select this option, do the following:</span></span> 
    
     * <span data-ttu-id="9bfca-161">**[VM Credentials] \(VM 資格情報)** タブで、Docker ホストの仮想マシン ログイン資格情報の次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-161">On the **VM Credentials** tab, choose one of the following options for the virtual-machine login credentials of your Docker host:</span></span> 

       * <span data-ttu-id="9bfca-162">**[ユーザー名]**: 仮想マシン ログイン資格情報のユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-162">**Username**: Enter the username for your virtual machine login credentials.</span></span> 
       * <span data-ttu-id="9bfca-163">**[パスワード]** および **[確認]**: 仮想マシン ログイン資格情報のパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-163">**Password** and **Confirm**: Enter the password for your virtual machine login credentials.</span></span> 
       * <span data-ttu-id="9bfca-164">**[SSH]**: Docker ホストの Secure Shell (SSH) 設定を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-164">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="9bfca-165">次のオプションから選択できます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-165">You can choose from the following options:</span></span> 
          * <span data-ttu-id="9bfca-166">**[なし]**: 仮想マシンが SSH 接続を許可しないことを指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span> 
          * <span data-ttu-id="9bfca-167">**[自動生成]**: SSH 経由で接続するために必要な設定を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-167">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span> 
          * <span data-ttu-id="9bfca-168">**[Import from directory] \(ディレクトリからのインポート)**: 以前に保存された一連の SSH 設定を含むディレクトリを指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="9bfca-169">このディレクトリには、次の 2 つのファイルが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="9bfca-169">The directory must contain the following two files:</span></span> 
             * <span data-ttu-id="9bfca-170">*id_rsa*: ユーザーの RSA ID が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9bfca-170">*id_rsa*: Contains the RSA identification for a user.</span></span> 
             * <span data-ttu-id="9bfca-171">*id_rsa.pub*: 認証に使用される RSA 公開キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9bfca-171">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span> 
        
         ![[Create Docker Host] \(Docker ホストの作成)][PUB05]

     * <span data-ttu-id="9bfca-173">**[Docker Daemon Credentials] \(Docker デーモン資格情報)** タブで、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-173">On the **Docker Daemon Credentials** tab, specify the following options:</span></span> 

       * <span data-ttu-id="9bfca-174">**[Docker Daemon port] \(Docker デーモン ポート)**: Docker ホストの固有の TCP ポートを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-174">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span> 
       * <span data-ttu-id="9bfca-175">**[TLS Security] \(TLS セキュリティ)**: Docker ホストのトランスポート層セキュリティ設定を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-175">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="9bfca-176">次のオプションから選択できます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-176">You can choose from the following options:</span></span> 
          * <span data-ttu-id="9bfca-177">**[なし]**: 仮想マシンが TLS 接続を許可しないことを指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span> 
          * <span data-ttu-id="9bfca-178">**[自動生成]**: TLS 経由で接続するために必要な設定を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-178">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span> 
          * <span data-ttu-id="9bfca-179">**[Import from directory] \(ディレクトリからのインポート)**: 以前に保存された一連の TLS 設定を含むディレクトリを指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="9bfca-180">具体的には、ディレクトリには次の 6 つのファイルが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="9bfca-180">More specifically, the directory must contain the following six files:</span></span> 
             * <span data-ttu-id="9bfca-181">*ca.pem* と *ca-key.pem*: TLS 証明機関の証明書と公開キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9bfca-181">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span> 
             * <span data-ttu-id="9bfca-182">*cert.pem* と *key.pem*: TLS 認証に使用されるクライアント証明書と公開キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9bfca-182">*cert.pem* and *key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span> 
             * <span data-ttu-id="9bfca-183">*server.pem* と *server-key.pem*: ホストのサーバー証明書と公開キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9bfca-183">*server.pem* and *server-key.pem*: Contain the server certificate and public key for the host.</span></span> 

         ![[Create Docker Host] \(Docker ホストの作成)][PUB06]

7. <span data-ttu-id="9bfca-185">上記の情報をすべて入力したら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-185">After you have entered all of the preceding information, click **Finish**.</span></span>

8. <span data-ttu-id="9bfca-186">**[Deploy Docker Container on Azure] \(Azure への Docker コンテナーのデプロイ)** ウィザードで、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-186">In the **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![[Deploy Docker Container on Azure] \(Azure への Docker コンテナーのデプロイ) ウィザード][PUB07]

9. <span data-ttu-id="9bfca-188">**[Configure the Docker container to be created] \(作成される Docker コンテナーの構成)** ウィンドウで、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-188">In the **Configure the Docker container to be created** window, do the following:</span></span>

   <span data-ttu-id="9bfca-189">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-189">a.</span></span> <span data-ttu-id="9bfca-190">**[Docker container name] \(Docker コンテナー名)** ボックスに、Docker コンテナーの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-190">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="9bfca-191">b.</span><span class="sxs-lookup"><span data-stu-id="9bfca-191">b.</span></span> <span data-ttu-id="9bfca-192">次のいずれかの Docker イメージを選びます。</span><span class="sxs-lookup"><span data-stu-id="9bfca-192">Choose one of the following Docker images:</span></span> 

   * <span data-ttu-id="9bfca-193">**[Predefined Docker image] \(定義済みの Docker イメージ)**: Azure の既存の Docker イメージを指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span> 

     >[!NOTE]
     ><span data-ttu-id="9bfca-194">このボックス内の Docker イメージの一覧は、アーティファクトが自動的にデプロイされるように、Azure Toolkit が修正するように構成されているいくつかのイメージで構成されています。</span><span class="sxs-lookup"><span data-stu-id="9bfca-194">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span>
     >

   * <span data-ttu-id="9bfca-195">**[Custom Dockerfile] \(カスタム Dockerfile)**: ローカル コンピューターから以前に保存された Dockerfile を指定します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

     >[!NOTE]
     ><span data-ttu-id="9bfca-196">これは、独自の Dockerfile をデプロイする開発者向けのより高度な機能です。</span><span class="sxs-lookup"><span data-stu-id="9bfca-196">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="9bfca-197">ただし、Dockerfile が正しく構築されていることを確認するのは、このオプションを使用する開発者の責任です。</span><span class="sxs-lookup"><span data-stu-id="9bfca-197">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="9bfca-198">Azure Toolkit はカスタム Dockerfile 内のコンテンツを検証しないため、Dockerfile に問題がある場合はデプロイが失敗することがあります。</span><span class="sxs-lookup"><span data-stu-id="9bfca-198">The Azure Toolkit does not validate the content in a custom Dockerfile, so the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="9bfca-199">さらに、Azure Toolkit はカスタム Dockerfile に Web アプリ アーティファクトが含まれていることを期待し、また HTTP 接続を開こうとします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-199">In addition, the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, and it will attempt to open an HTTP connection.</span></span> <span data-ttu-id="9bfca-200">開発者が別の種類のアーティファクトを発行した場合は、デプロイ後に無害のエラーが表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="9bfca-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>
     >

   <span data-ttu-id="9bfca-201">c.</span><span class="sxs-lookup"><span data-stu-id="9bfca-201">c.</span></span> <span data-ttu-id="9bfca-202">**[ポートの設定]**: Docker コンテナーの固有の TCP ポート バインドを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-202">**Port settings**: Enter the unique TCP port binding for your Docker container.</span></span>

      ![[Configure the Docker container to be created] \(作成される Docker コンテナーの構成) ウィンドウ][PUB08]

10. <span data-ttu-id="9bfca-204">上記の手順をすべて完了したら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bfca-204">After you have completed all of the preceding steps, click **Finish**.</span></span>

<span data-ttu-id="9bfca-205">Azure Toolkit が Docker コンテナーでの Azure への Web アプリのデプロイを開始します。</span><span class="sxs-lookup"><span data-stu-id="9bfca-205">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9bfca-206">次の手順</span><span class="sxs-lookup"><span data-stu-id="9bfca-206">Next steps</span></span>

<span data-ttu-id="9bfca-207">Docker の他のリソースについては、公式の [Docker の Web サイト]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9bfca-207">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Docker の Web サイト]: https://www.docker.com/
[Docker website]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png