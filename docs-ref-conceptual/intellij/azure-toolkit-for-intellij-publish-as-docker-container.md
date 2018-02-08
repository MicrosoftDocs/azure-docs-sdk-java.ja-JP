---
title: "Azure Toolkit for IntelliJ を使用して Docker コンテナーを発行する"
description: "Azure Toolkit for IntelliJ を使用して、Web アプリを Docker コンテナーとして Microsoft Azure に発行する方法について説明します。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: f92040b34b9897d9feea8d2ec5e8748e75fff7f7
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="90c09-103">Azure Toolkit for IntelliJ を使用して Web アプリを Docker コンテナーとして発行する</span><span class="sxs-lookup"><span data-stu-id="90c09-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="90c09-104">Docker コンテナーは、Web アプリケーションをデプロイするために広く使用されている方法です。</span><span class="sxs-lookup"><span data-stu-id="90c09-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="90c09-105">Docker コンテナーを使用すると、開発者は、すべてのプロジェクト ファイルと依存関係を、サーバーにデプロイするために 1 つのパッケージに統合できます。</span><span class="sxs-lookup"><span data-stu-id="90c09-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="90c09-106">Java 開発者のこのプロセスを簡略化するために、Azure Toolkit for IntelliJ には、Microsoft Azure にデプロイするための "*Docker コンテナーとして発行*" 機能が追加されました。</span><span class="sxs-lookup"><span data-stu-id="90c09-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="90c09-107">この記事では、アプリケーションを Docker コンテナーとして Azure に発行するために必要な手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="90c09-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="90c09-108">Docker の詳細については、[Docker の Web サイト]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="90c09-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="90c09-109">Docker コンテナーを使用して Web アプリを Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="90c09-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="90c09-110">Web アプリを発行するには、デプロイの準備ができたアーティファクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="90c09-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="90c09-111">詳細については、「[アーティファクトの作成に関する追加情報](#artifacts)」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="90c09-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="90c09-112">デプロイ ウィザードを少なくとも 1 回完了した後、ウィザードをもう一度実行するときには、設定の大部分が既定値として使用されます。</span><span class="sxs-lookup"><span data-stu-id="90c09-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="90c09-113">IntelliJ で、Web アプリ プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="90c09-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="90c09-114">**Publish as Docker Container (Docker コンテナーとして発行)** ウィザードを起動するには、次のいずれかの操作を行います。</span><span class="sxs-lookup"><span data-stu-id="90c09-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="90c09-115">**[プロジェクト]** ツール ウィンドウでプロジェクトを右クリックし、**[Azure]** をクリックしてから **[Publish as Docker Container]\(Docker コンテナーとして発行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![[Publish as Docker Container]\(Docker コンテナーとして発行\) コマンド][PUB01]

   * <span data-ttu-id="90c09-117">IntelliJ ツール バーの **[Publish Group]\(発行グループ\)** ボタンをクリックし、**[Publish as Docker Container]\(Docker コンテナーとして発行\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="90c09-118">![[Publish as Docker Container]\(Docker コンテナーとして発行\) コマンド][PUB02]</span><span class="sxs-lookup"><span data-stu-id="90c09-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="90c09-119">**Deploy Docker Container on Azure (Azure に Docker コンテナーをデプロイ)** ウィザードが開かれます。</span><span class="sxs-lookup"><span data-stu-id="90c09-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![[Deploy Docker Container on Azure] \(Azure への Docker コンテナーのデプロイ) ウィザード][PUB03]

3. <span data-ttu-id="90c09-121">**[Type image name, select artifact's path and check Docker host to be used] \(イメージ名を入力し、アーティファクトのパスを選択して、使用される Docker ホストを確認する)** ウィンドウで、以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="90c09-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="90c09-122">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-122">a.</span></span> <span data-ttu-id="90c09-123">**[Docker image name] \(Docker イメージ名)** ボックスに、Docker ホストの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="90c09-124">(このウィザードでは名前が自動的に作成されますが、それは変更できます)。</span><span class="sxs-lookup"><span data-stu-id="90c09-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="90c09-125">b.</span><span class="sxs-lookup"><span data-stu-id="90c09-125">b.</span></span> <span data-ttu-id="90c09-126">**[ホスト]** 領域には、既に作成しているすべての Docker ホストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="90c09-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="90c09-127">次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="90c09-127">Do either of the following:</span></span> 
      * <span data-ttu-id="90c09-128">既存の Docker ホストがある場合は、それに Web アプリをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="90c09-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="90c09-129">Docker ホストを作成するには、緑色のプラス記号 (**+**) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="90c09-130">**[Create Docker Host]\(Docker ホストの作成\)** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="90c09-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![[Deploy Docker Container on Azure] \(Azure への Docker コンテナーのデプロイ) ウィザード][PUB04a]

4. <span data-ttu-id="90c09-132">**[Configure the new virtual machine]\(新しい仮想マシンの構成\)** ウィンドウで、Docker ホストについての以下の情報を指定します </span><span class="sxs-lookup"><span data-stu-id="90c09-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="90c09-133">(ほとんどの情報はウィザードによって自動的に生成されますが、どの情報も変更することができます)。</span><span class="sxs-lookup"><span data-stu-id="90c09-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="90c09-134">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-134">a.</span></span> <span data-ttu-id="90c09-135">**[名前]** ボックスに、Docker ホストの一意の名前を入力します </span><span class="sxs-lookup"><span data-stu-id="90c09-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="90c09-136">(これは前に指定した Docker イメージの名前と同じではありません)。</span><span class="sxs-lookup"><span data-stu-id="90c09-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="90c09-137">b.</span><span class="sxs-lookup"><span data-stu-id="90c09-137">b.</span></span> <span data-ttu-id="90c09-138">**[サブスクリプション]** ボックスで、ホストのために使用する Azure サブスクリプションを入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="90c09-139">c.</span><span class="sxs-lookup"><span data-stu-id="90c09-139">c.</span></span> <span data-ttu-id="90c09-140">**[リージョン]** ボックスで、ホストが配置される地理的なリージョンを入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="90c09-141">d.</span><span class="sxs-lookup"><span data-stu-id="90c09-141">d.</span></span> <span data-ttu-id="90c09-142">**[OS and Size]\(OS とサイズ\)** タブで、以下の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="90c09-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="90c09-143">**[Host OS]\(ホスト OS\)**: ホストがある仮想マシンのオペレーティング システムを入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="90c09-144">**[サイズ]**: ホストの仮想マシンのサイズを入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="90c09-145">e.</span><span class="sxs-lookup"><span data-stu-id="90c09-145">e.</span></span> <span data-ttu-id="90c09-146">**[リソース グループ]** タブで、以下のいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="90c09-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="90c09-147">**[新しいリソース グループ]**: ホストのリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="90c09-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="90c09-148">**[既存のリソース グループ]**: Azure アカウントの既存のリソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="90c09-149">f.</span><span class="sxs-lookup"><span data-stu-id="90c09-149">f.</span></span> <span data-ttu-id="90c09-150">**[ネットワーク]** タブで、以下のいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="90c09-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="90c09-151">**[新しい仮想ネットワーク]**: ホストの仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="90c09-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="90c09-152">**[Existing virtual network]\(既存の仮想ネットワーク\)**: Azure アカウントの既存の仮想ネットワークを指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="90c09-153">g.</span><span class="sxs-lookup"><span data-stu-id="90c09-153">g.</span></span> <span data-ttu-id="90c09-154">**[ストレージ]** タブで、以下のいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="90c09-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="90c09-155">**[新規ストレージ アカウント]**: ホストのストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="90c09-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="90c09-156">**[Existing storage account]\(既存のストレージ アカウント\)**: Azure アカウントの既存のストレージ アカウントを指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="90c09-157">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-157">Click **Next**.</span></span>  
     <span data-ttu-id="90c09-158">**[Configure log in credentials and port settings]\(ログイン資格情報とポート設定の構成\)** ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="90c09-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![[Configure log in credentials and port settings]\(ログイン資格情報とポート設定の構成\) ウィンドウ][PUB05]

6. <span data-ttu-id="90c09-160">次のいずれかのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="90c09-160">Select one of the following options:</span></span>

      * <span data-ttu-id="90c09-161">**[Import credentials from Azure Key Vault]\(Azure Key Vault から資格情報をインポート\)**: 以前に保存した、Azure サブスクリプションに格納されている資格情報のセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="90c09-162">特定のアカウントまたはサービス プリンシパルで作成された Azure Key Vault は、サブスクリプションを共有する別のアカウントまたはサービス プリンシパルから自動的にアクセスできるようにはなりません。</span><span class="sxs-lookup"><span data-stu-id="90c09-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="90c09-163">別のアカウントまたはサービス プリンシパルが Key Vault を使用できるようにするには、Azure Portal を使用して、アカウントまたはサービス プリンシパルを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="90c09-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="90c09-164">**[New log in credentials]\(新しいログイン資格情報\)**: ログイン資格情報の新しいセットを作成します。</span><span class="sxs-lookup"><span data-stu-id="90c09-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="90c09-165">このオプションを選択する場合は、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="90c09-165">If you select this option, do the following:</span></span>

        <span data-ttu-id="90c09-166">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-166">a.</span></span> <span data-ttu-id="90c09-167">**[VM Credentials]\(VM 資格情報)** タブで、Docker ホストの仮想マシン ログイン資格情報について、以下の情報を指定します。 \* **[ユーザー名]**: 仮想マシン ログイン資格情報のユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host: \* **Username**: Enter the username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="90c09-168">\* **[パスワード]** および **[確認]** : 仮想マシン ログイン資格情報のパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-168">\* **Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="90c09-169">\* **[SSH]**: Docker ホストの Secure Shell (SSH) 設定を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-169">\* **SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="90c09-170">以下のいずれかのオプションを選択できます。 \* **[なし]**: 仮想マシンが SSH 接続を許可しないことを指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-170">You can select one of the following options: \* **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="90c09-171">\* **[自動生成]**: SSH 経由で接続するために必要な設定を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="90c09-171">\* **Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="90c09-172">\* **[Import from directory]\(ディレクトリからインポート\)**: 以前に保存した SSH 設定のセットがあるディレクトリを指定できます。</span><span class="sxs-lookup"><span data-stu-id="90c09-172">\* **Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="90c09-173">このディレクトリには、次の 2 つのファイルが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="90c09-173">The directory must contain the following two files:</span></span>
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        <span data-ttu-id="90c09-174">b.</span><span class="sxs-lookup"><span data-stu-id="90c09-174">b.</span></span> <span data-ttu-id="90c09-175">**[Docker Daemon Access]\(Docker デーモン アクセス\)** タブで、以下の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-175">On the **Docker Daemon Access** tab, provide the following information:</span></span>

          ![Docker ホストを作成する][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="90c09-177">必要な情報を入力したら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-177">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="90c09-178">**Deploy Docker Container on Azure (Azure に Docker コンテナーをデプロイ)** ウィザードが再び表示されます。</span><span class="sxs-lookup"><span data-stu-id="90c09-178">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Deploy Docker Container on Azure (Azure に Docker コンテナーをデプロイ) ウィザード][PUB07]

8. <span data-ttu-id="90c09-180">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-180">Click **Next**.</span></span>  
    <span data-ttu-id="90c09-181">**[Configure the Docker container to be created]\(作成される Docker コンテナーの構成\)** ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="90c09-181">The **Configure the Docker container to be created** window opens.</span></span>

   ![[Configure the Docker container to be created]\(作成される Docker コンテナーの構成\) ウィンドウ][PUB08]

9. <span data-ttu-id="90c09-183">**[Configure the Docker container to be created]\(作成される Docker コンテナーの構成\)** ウィンドウで、以下の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-183">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="90c09-184">a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-184">a.</span></span> <span data-ttu-id="90c09-185">**[Docker container name] \(Docker コンテナー名)** ボックスに、Docker コンテナーの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-185">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="90c09-186">b.</span><span class="sxs-lookup"><span data-stu-id="90c09-186">b.</span></span> <span data-ttu-id="90c09-187">次のいずれかの Docker イメージを選びます。</span><span class="sxs-lookup"><span data-stu-id="90c09-187">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="90c09-188">**[Predefined Docker image]\(定義済みの Docker イメージ\)**: Azure から既存の Docker イメージを指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="90c09-189">このボックス内の Docker イメージの一覧は、アーティファクトが自動的にデプロイされるように Azure Toolkit が修正する複数のイメージで構成されています。</span><span class="sxs-lookup"><span data-stu-id="90c09-189">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="90c09-190">**[Custom Dockerfile]\(カスタム Dockerfile\)**: ローカル コンピューターから、以前に保存した Dockerfile を指定します。</span><span class="sxs-lookup"><span data-stu-id="90c09-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="90c09-191">これは、独自の Dockerfile をデプロイする開発者向けのより高度な機能です。</span><span class="sxs-lookup"><span data-stu-id="90c09-191">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="90c09-192">ただし、Dockerfile が正しく構築されていることを確認するのは、このオプションを使用する開発者の責任です。</span><span class="sxs-lookup"><span data-stu-id="90c09-192">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="90c09-193">Azure Toolkit はカスタム Dockerfile の内容を検証しないため、Dockerfile に問題がある場合はデプロイが失敗します。</span><span class="sxs-lookup"><span data-stu-id="90c09-193">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="90c09-194">また、Azure Toolkit はカスタム Dockerfile に Web アプリ アーティファクトが含まれていると想定しているため、HTTP 接続を開こうとします。</span><span class="sxs-lookup"><span data-stu-id="90c09-194">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="90c09-195">開発者が別の種類のアーティファクトを発行すると、デプロイ後に無害なエラーが表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="90c09-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="90c09-196">c.</span><span class="sxs-lookup"><span data-stu-id="90c09-196">c.</span></span> <span data-ttu-id="90c09-197">**[ポートの設定]** ボックスで、Docker コンテナーの一意の TCP ポート バインドを入力します。</span><span class="sxs-lookup"><span data-stu-id="90c09-197">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="90c09-198">上記の手順を完了したら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-198">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="90c09-199">Azure Toolkit により、Docker コンテナーでの Azure への Web アプリのデプロイが開始されます。</span><span class="sxs-lookup"><span data-stu-id="90c09-199">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="90c09-200">バックグラウンドでデプロイされるように IntelliJ を構成していない場合は、**[Deploying to Azure]\(Azure にデプロイ中\)** 進行状況バーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="90c09-200">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![デプロイの進行状況バー][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="90c09-202">アーティファクトの作成に関する追加情報</span><span class="sxs-lookup"><span data-stu-id="90c09-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="90c09-203">デプロイの準備ができたアーティファクトを作成するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="90c09-203">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="90c09-204">IntelliJ で、Web アプリ プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="90c09-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="90c09-205">**[ファイル]** をクリックし、**[プロジェクトの構造]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-205">Click **File**, and then click **Project Structure**.</span></span>

   ![[プロジェクトの構造] コマンド][ART01]

3. <span data-ttu-id="90c09-207">アーティファクトを追加するには、緑のプラス記号 (**+**) をクリックし、**[Web Application: Archive]\(Web アプリケーション: アーカイブ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-207">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![[Web Application: Archive]\(Web アプリケーション: アーカイブ\) コマンド][ART02]

4. <span data-ttu-id="90c09-209">**[名前]** ボックスで、アーティファクトの名前を入力し (*.war* 拡張子は含めないでください)、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90c09-209">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![アーティファクトの [名前] ボックス][ART03]

<span data-ttu-id="90c09-211">IntelliJ でのアーティファクトの作成の詳細については、JetBrains Web サイトの「[Configuring Artifacts (アーティファクトの構成)]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="90c09-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90c09-212">次の手順</span><span class="sxs-lookup"><span data-stu-id="90c09-212">Next steps</span></span>

<span data-ttu-id="90c09-213">Docker の他のリソースについては、公式の [Docker の Web サイト]を参照してください。</span><span class="sxs-lookup"><span data-stu-id="90c09-213">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Docker の Web サイト]: https://www.docker.com/
[Configuring Artifacts (アーティファクトの構成)]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
