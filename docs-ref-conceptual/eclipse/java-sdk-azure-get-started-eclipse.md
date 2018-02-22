---
title: "Eclipse を使用して Java 用 Azure ライブラリの使用を開始する"
description: "ご利用の Azure サブスクリプションで Java 用 Azure ライブラリを使用するための基本的な事柄について説明します。"
keywords: "Azure, Java, SDK, API, 認証, 概要"
services: 
documentationcenter: java
author: roygara
manager: timlt
editor: 
ms.author: v-rogara
ms.date: 02/01/2018
ms.devlang: java
ms.prod: azure
ms.technology: azure
ms.topic: get-started-article
ms.service: multiple
ms.openlocfilehash: 740679197981f49d99b8d8251e257963d3030fb1
ms.sourcegitcommit: 720c2eaf66532d277015610ec375c71e934d9ee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2018
---
# <a name="get-started-with-the-azure-libraries-using-eclipse"></a><span data-ttu-id="8c541-104">Eclipse を使用して Azure ライブラリの使用を開始する</span><span class="sxs-lookup"><span data-stu-id="8c541-104">Get started with the Azure libraries using Eclipse</span></span>

<span data-ttu-id="8c541-105">このガイドでは、開発環境を設定し、Java 用 Azure ライブラリを使用する手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="8c541-105">This guide walks you through setting up a development environment and using the Azure libraries for Java.</span></span> <span data-ttu-id="8c541-106">Azure で認証するためのサービス プリンシパルを作成し、サブスクリプションの Azure リソースを作成および使用するサンプル コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="8c541-106">You'll create a service principal to authenticate with Azure and run some sample code that creates and uses Azure resources in your subscription.</span></span> <span data-ttu-id="8c541-107">Azure での Java 開発で Eclipse の使用は任意です。</span><span class="sxs-lookup"><span data-stu-id="8c541-107">Using Eclipse is optional for Java development with Azure.</span></span> <span data-ttu-id="8c541-108">Maven が統合された IDE であれば動作します。</span><span class="sxs-lookup"><span data-stu-id="8c541-108">Any IDE that has Maven integration works.</span></span> <span data-ttu-id="8c541-109">IDE を使用しない場合は、Maven を使用してコマンド ラインからコードを実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="8c541-109">Alternatively, you can run your code from the commandline using Maven if you prefer not to use any IDE.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c541-110">前提条件</span><span class="sxs-lookup"><span data-stu-id="8c541-110">Prerequisites</span></span>

- <span data-ttu-id="8c541-111">Azure アカウント。</span><span class="sxs-lookup"><span data-stu-id="8c541-111">An Azure account.</span></span> <span data-ttu-id="8c541-112">所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-112">If you don't have one, [get a free trial](https://azure.microsoft.com/free/)</span></span>
- <span data-ttu-id="8c541-113">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) または [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。</span><span class="sxs-lookup"><span data-stu-id="8c541-113">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>
- <span data-ttu-id="8c541-114">最新の安定バージョンの [Eclipse](http://www.eclipse.org/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="8c541-114">The latest stable version of [Eclipse](http://www.eclipse.org/downloads/)</span></span>

## <a name="set-up-authentication"></a><span data-ttu-id="8c541-115">認証の設定</span><span class="sxs-lookup"><span data-stu-id="8c541-115">Set up authentication</span></span>

<span data-ttu-id="8c541-116">このチュートリアルのサンプル コードを実行する Java アプリケーションには、Azure サブスクリプションの読み取りと作成のアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="8c541-116">Your Java application needs read and create permissions in your Azure subscription to run the sample code in this tutorial.</span></span> <span data-ttu-id="8c541-117">サービス プリンシパルを作成し、その資格情報で動作するようにアプリケーションを構成してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-117">Create a service principal and configure your application to run with its credentials.</span></span> <span data-ttu-id="8c541-118">サービス プリンシパルによって、自分の ID に関連付けられた非対話型のアカウントを作成し、アプリの実行に必要な権限だけを付与することができます。</span><span class="sxs-lookup"><span data-stu-id="8c541-118">Service principals provide a way to create a non-interactive account associated with your identity to which you grant only the privileges your app needs to run.</span></span>

<span data-ttu-id="8c541-119">[サービス プリンシパルを作成](/cli/azure/create-an-azure-service-principal-azure-cli)して、アカウントの資格情報を直接使用せずに、サブスクリプションのリソースを作成および更新するアクセス許可をコードに付与します。</span><span class="sxs-lookup"><span data-stu-id="8c541-119">[Create a service principal](/cli/azure/create-an-azure-service-principal-azure-cli) to grant your code permission to create and update resources in your subscription without using your account credentials directly.</span></span> <span data-ttu-id="8c541-120">出力を必ずキャプチャしてください。</span><span class="sxs-lookup"><span data-stu-id="8c541-120">Make sure to capture the output.</span></span> <span data-ttu-id="8c541-121">password 引数には、`MY_SECURE_PASSWORD` ではなく、[セキュリティで保護されたパスワード](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy)を指定します。</span><span class="sxs-lookup"><span data-stu-id="8c541-121">Provide a [secure password](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) in the password argument instead of `MY_SECURE_PASSWORD`.</span></span> <span data-ttu-id="8c541-122">パスワードは 8 ～ 16 文字にし、次の 4 つの条件のうち少なくとも 3 つの条件に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8c541-122">Your password must be 8 to 16 characters and match at least 3 out of the 4 following criteria:</span></span>

* <span data-ttu-id="8c541-123">小文字が含まれている</span><span class="sxs-lookup"><span data-stu-id="8c541-123">Include lowercase characters</span></span>
* <span data-ttu-id="8c541-124">大文字が含まれている</span><span class="sxs-lookup"><span data-stu-id="8c541-124">Include uppercase characters</span></span>
* <span data-ttu-id="8c541-125">数字が含まれている</span><span class="sxs-lookup"><span data-stu-id="8c541-125">Include numbers</span></span>
* <span data-ttu-id="8c541-126">次の記号のいずれかが含まれている: @ # $ % ^ & \* - _ !</span><span class="sxs-lookup"><span data-stu-id="8c541-126">Include one of the following symbols: @ # $ % ^ & \* - _ !</span></span> <span data-ttu-id="8c541-127">+ = [ ] { } | \ : ‘ , .</span><span class="sxs-lookup"><span data-stu-id="8c541-127">+ = [ ] { } | \ : ‘ , .</span></span> <span data-ttu-id="8c541-128">?</span><span class="sxs-lookup"><span data-stu-id="8c541-128">?</span></span> <span data-ttu-id="8c541-129">/ \` ~ “ ( ) ;</span><span class="sxs-lookup"><span data-stu-id="8c541-129">/ \` ~ “ ( ) ;</span></span>


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

<span data-ttu-id="8c541-130">次の形式で応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="8c541-130">Which gives you a reply in the following format:</span></span>

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

<span data-ttu-id="8c541-131">次に、ご利用のシステム上のテキスト ファイルに次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="8c541-131">Next, copy the following into a text file on your system:</span></span>

```text
# sample management library properties file
subscription=ssssssss-ssss-ssss-ssss-ssssssssssss
client=cccccccc-cccc-cccc-cccc-cccccccccccc
key=kkkkkkkkkkkkkkkk
tenant=tttttttt-tttt-tttt-tttt-tttttttttttt
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

<span data-ttu-id="8c541-132">上から 4 つの値は、次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="8c541-132">Replace the top four values with the following:</span></span>

- <span data-ttu-id="8c541-133">subscription: Azure CLI 2.0 の `az account show` から得られる *id* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="8c541-133">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="8c541-134">client: サービス プリンシパルの出力から得られる *appId* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="8c541-134">client: use the *appId* value from the output taken from a service principal output.</span></span>
- <span data-ttu-id="8c541-135">key: サービス プリンシパルの出力から得られる *password* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="8c541-135">key: use the *password* value from the service principal output.</span></span>
- <span data-ttu-id="8c541-136">tenant: サービス プリンシパルの出力から得られる *tenant* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="8c541-136">tenant: use the *tenant* value from the service principal output.</span></span>

<span data-ttu-id="8c541-137">このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-137">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="8c541-138">このファイルは今後のコードに使用できるため、この記事のアプリケーションの外部の場所に保存することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8c541-138">You may use this file for future code so it's recommended to store it somewhere external to the application in this article.</span></span>

<span data-ttu-id="8c541-139">ご利用のシェルから、認証ファイルの完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="8c541-139">Set an environment variable `AZURE_AUTH_LOCATION` with the full path to the authentication file in your shell.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="8c541-140">Windows 環境で作業している場合は、システムのプロパティに変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="8c541-140">If you're working in a windows environment, add the variable to your system properties.</span></span> <span data-ttu-id="8c541-141">管理者特権で PowerShell ウィンドウを開き、2 番目の変数をファイルのパスで置き換えた後、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="8c541-141">Open a PowerShell window with administrator privledges and, after replacing the second variable with the path to your file, enter the following command:</span></span>

```powershell
setx AZURE_AUTH_LOCATION "C:\<fullpath>\azureauth.properties" /m
```

## <a name="create-a-new-maven-project"></a><span data-ttu-id="8c541-142">新しい Maven プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="8c541-142">Create a new Maven project</span></span>

> [!NOTE]
> <span data-ttu-id="8c541-143">このガイドでは、Maven ビルド ツールを使って、サンプル コードをビルドして実行していますが、Java 用 Azure ライブラリは他のビルド ツール (Gradle など) で使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="8c541-143">This guide uses Maven build tool to build and run the sample code, but other build tools such as Gradle also work with the Azure libraries for Java.</span></span> 

<span data-ttu-id="8c541-144">Eclipse を開き、**[File]\(ファイル\)** -> **[New]\(新規\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8c541-144">Open Eclipse, select **File** -> **New**.</span></span> <span data-ttu-id="8c541-145">表示された新しいウィンドウで、Maven フォルダーを開き、Maven プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="8c541-145">In the new window that appears open the Maven folder then select Maven Project.</span></span> 

<span data-ttu-id="8c541-146">次の画面の選択肢では既定値をそのまま使用し、**[Next]\(次へ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8c541-146">Leave the selections on the next screen defaults, and select **Next**.</span></span> <span data-ttu-id="8c541-147">アーキタイプに関するこの画面でも同じ操作を行います。</span><span class="sxs-lookup"><span data-stu-id="8c541-147">Do the same for this screen regarding archetypes.</span></span>

<span data-ttu-id="8c541-148">groupID、ArtifactID などの入力を求める画面が表示されます。groupID として「com.fabrikam」と入力し、artifactID として「AzureApp」と入力します。</span><span class="sxs-lookup"><span data-stu-id="8c541-148">When you come to the screen asking for groupID, ArtifactID, etc. Enter "com.fabrikam" for the groupID and enter "AzureApp" for the artifactID.</span></span>

<span data-ttu-id="8c541-149">次に、pom.xml ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8c541-149">Now, open the pom.xml file.</span></span> <span data-ttu-id="8c541-150">`dependencies` タグ内に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8c541-150">Inside the `dependencies` tag add the following code:</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.0.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

<span data-ttu-id="8c541-151">pom.xml を保存します。</span><span class="sxs-lookup"><span data-stu-id="8c541-151">Now, save the pom.xml.</span></span> <span data-ttu-id="8c541-152">これにより、Eclipse は指定されたすべての依存関係のダウンロードを開始します。</span><span class="sxs-lookup"><span data-stu-id="8c541-152">This prompts Eclipse to download all the specified dependencies.</span></span> <span data-ttu-id="8c541-153">これには少し時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="8c541-153">This may take a moment.</span></span>
   
## <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="8c541-154">Azure Toolkit for Eclipse をインストールする</span><span class="sxs-lookup"><span data-stu-id="8c541-154">Install the azure toolkit for Eclipse</span></span>

<span data-ttu-id="8c541-155">Web アプリや API をプログラムでデプロイする予定でも、その他の開発に現在使用していない場合は、[Azure Toolkit](azure-toolkit-for-eclipse.md) が必要です。</span><span class="sxs-lookup"><span data-stu-id="8c541-155">The [Azure toolkit](azure-toolkit-for-eclipse.md) is necessary if you're going to be deploying web apps or APIs programmatically but is not currently used for any other kinds of development.</span></span> <span data-ttu-id="8c541-156">インストール プロセスの概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="8c541-156">The following is a summary of the installation process.</span></span> <span data-ttu-id="8c541-157">詳しい手順については、「[Azure Toolkit for Eclipse のインストール](azure-toolkit-for-eclipse.md)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8c541-157">For detailed stpes, visit [Installing the Azure Toolkit for Eclipse](azure-toolkit-for-eclipse.md).</span></span>

<span data-ttu-id="8c541-158">**[Help]\(ヘルプ\)** メニューを選択し、**[Install New software]\(新しいソフトウェアのインストール\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8c541-158">Select the **Help** menu and then select **Install New software**.</span></span>

<span data-ttu-id="8c541-159">**[Work with:]\(処理:\)** フィールドに「`http://dl.microsoft.com/eclipse`」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8c541-159">In the **Work with:** field enter `http://dl.microsoft.com/eclipse` and press enter.</span></span>

<span data-ttu-id="8c541-160">次に、**[Azure Toolkit for Java]** の横のチェック ボックスをオンにし、**[Contact all update sites during install to find required software]\(インストール中にすべての更新サイトに接続して必要なソフトウェアを見つける\)** のチェック ボックスをオフにします。</span><span class="sxs-lookup"><span data-stu-id="8c541-160">Then, select the checkbox next to **Azure toolkit for Java** and uncheck the checkbox for **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="8c541-161">[Next]\(次へ\) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8c541-161">Then select next.</span></span>

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="8c541-162">Linux 仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="8c541-162">Create a Linux virtual machine</span></span>

<span data-ttu-id="8c541-163">プロジェクトの `src/main/java` ディレクトリに `AzureApp.java` という名前の新しいファイルを作成し、次のコード ブロックを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="8c541-163">Create a new file named `AzureApp.java` in the project's `src/main/java` directory and paste in the following block of code.</span></span> <span data-ttu-id="8c541-164">`userName` 変数と `sshKey` 変数は、ご利用のマシンの実際の値に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="8c541-164">Update the `userName` and `sshKey` variables with real values for your machine.</span></span> <span data-ttu-id="8c541-165">このコードによって、米国東部 Azure リージョンで実行されるリソース グループ `sampleResourceGroup` に、`testLinuxVM` という名前の新しい Linux VM が作成されます。</span><span class="sxs-lookup"><span data-stu-id="8c541-165">The code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleResourceGroup` running in the US East Azure region.</span></span>

<span data-ttu-id="8c541-166">`sshkey` を作成するには、Azure Cloud Shell を開き、「`ssh-keygen -t rsa -b 2048`」と入力します。</span><span class="sxs-lookup"><span data-stu-id="8c541-166">In order to create an `sshkey`, open the azure cloud shell and enter `ssh-keygen -t rsa -b 2048`.</span></span> <span data-ttu-id="8c541-167">ファイル名を入力し、.public ファイルにアクセスして、次のコードで使用するキーを取得します。キーをコピーして変数 `sshKey` に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="8c541-167">Name the file and then access the .public file to get the key, which you use in the following code, copy and paste it all into your variable `sshKey`.</span></span>

```java
package com.fabrikam.AzureApp;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.PricingTier;
import com.microsoft.azure.management.appservice.WebApp;
import com.microsoft.azure.management.storage.StorageAccount;
import com.microsoft.azure.management.storage.SkuName;
import com.microsoft.azure.management.storage.StorageAccountKey;
import com.microsoft.azure.management.sql.SqlDatabase;
import com.microsoft.azure.management.sql.SqlServer;
import com.microsoft.azure.management.resources.fluentcore.arm.Region;
import com.microsoft.azure.management.resources.fluentcore.utils.SdkContext;

import com.microsoft.rest.LogLevel;

import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

import java.io.File;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.List;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));    
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();
           
            // create a Ubuntu virtual machine in a new resource group 
            VirtualMachine linuxVM = azure.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();   

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```


<span data-ttu-id="8c541-168">コンソールには、REST の要求と応答がいくつか表示されます。これは、SDK が内部的に Azure REST API を呼び出して、仮想マシンとそのリソースを構成しているためです。</span><span class="sxs-lookup"><span data-stu-id="8c541-168">You see some REST requests and responses in the console as the SDK makes the underlying calls to the Azure REST API to configure the virtual machine and its resources.</span></span> <span data-ttu-id="8c541-169">プログラムの実行が完了したら、サブスクリプション内の仮想マシンを Azure CLI 2.0 で確認します。</span><span class="sxs-lookup"><span data-stu-id="8c541-169">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="8c541-170">コードが正しく動作したことを確認したら、CLI で VM とそのリソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="8c541-170">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="8c541-171">GitHub リポジトリからの Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="8c541-171">Deploy a web app from a GitHub repo</span></span>

<span data-ttu-id="8c541-172">`AzureApp.java` の main メソッドを以下のメソッドに差し替えます。`appName` 変数には、コードを実行する前に一意の値を指定してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-172">Replace the main method in `AzureApp.java` with the one below, updating the `appName` variable to a unique value before running the code.</span></span> <span data-ttu-id="8c541-173">このコードは、無料の価格レベルで稼働する新しい [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) に、パブリック GitHub リポジトリの `master` ブランチから Web アプリケーションをデプロイするものです。</span><span class="sxs-lookup"><span data-stu-id="8c541-173">This code deploys a web application from the `master` branch in a public GitHub repo into a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free pricing tier.</span></span>

```java
    public static void main(String[] args) {
        try {

            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            final String appName = "YOUR_APP_NAME";

            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            WebApp app = azure.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

<span data-ttu-id="8c541-174">先ほどと同様、このコードも Maven を使用して実行します。</span><span class="sxs-lookup"><span data-stu-id="8c541-174">Run the code as before using Maven:</span></span>

<span data-ttu-id="8c541-175">ブラウザーを開いてアプリケーションにアクセスします。CLI から次のコマンドを入力してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-175">Open a browser pointed to the application using the CLI:</span></span>

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```
<span data-ttu-id="8c541-176">デプロイを検証したら、Web アプリとプランをサブスクリプションから削除します。</span><span class="sxs-lookup"><span data-stu-id="8c541-176">Remove the web app and plan from your subscription once you've verified the deployment.</span></span>

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a><span data-ttu-id="8c541-177">Azure SQL Database に接続する</span><span class="sxs-lookup"><span data-stu-id="8c541-177">Connect to an Azure SQL database</span></span>

<span data-ttu-id="8c541-178">`AzureApp.java` にある現行の main メソッドを以下のコードに差し替えます。`dbPassword` 変数には、実際の値を設定してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-178">Replace the current main method in `AzureApp.java` with the code below, setting a real value for the `dbPassword` variable.</span></span>
<span data-ttu-id="8c541-179">このコードは、リモート アクセスを許可するファイアウォール規則で新しい SQL データベースを作成し、SQL Database JDBC ドライバーを使ってそのデータベースに接続するものです。</span><span class="sxs-lookup"><span data-stu-id="8c541-179">This code creates a new SQL database with a firewall rule allowing remote access,  and then connects to it using the SQL Database JBDC driver.</span></span> 

```java

    public static void main(String args[])
    {
        // create the db using the management libraries
        try {
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            final String adminUser = SdkContext.randomResourceName("db",8);
            final String sqlServerName = SdkContext.randomResourceName("sql",10);
            final String sqlDbName = SdkContext.randomResourceName("dbname",8);
            final String dbPassword = "YOUR_PASSWORD_HERE";


            SqlServer sampleSQLServer = azure.sqlServers().define(sqlServerName)
                            .withRegion(Region.US_EAST)
                            .withNewResourceGroup("sampleSqlResourceGroup")
                            .withAdministratorLogin(adminUser)
                            .withAdministratorPassword(dbPassword)
                            .withNewFirewallRule("0.0.0.0","255.255.255.255")
                            .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // assemble the connection string to the database
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // connect to the database, create a table and insert a entry into it
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```
<span data-ttu-id="8c541-180">コマンド ラインでサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="8c541-180">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="8c541-181">その後、CLI で次のコマンドを入力して、リソースをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="8c541-181">Then clean up the resources using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="8c541-182">新しいストレージ アカウントへの BLOB の書き込み</span><span class="sxs-lookup"><span data-stu-id="8c541-182">Write a blob into a new storage account</span></span>

<span data-ttu-id="8c541-183">`AzureApp.java` にある現行の main メソッドを以下のコードに差し替えます。</span><span class="sxs-lookup"><span data-stu-id="8c541-183">Replace the current main method in `AzureApp.java` with the code below.</span></span> <span data-ttu-id="8c541-184">このコードは、[Azure ストレージ アカウント](https://docs.microsoft.com/azure/storage/storage-introduction)を作成し、Azure Storage Libraries for Java を使って新しいテキスト ファイルをクラウドに作成するものです。</span><span class="sxs-lookup"><span data-stu-id="8c541-184">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Java to create a new text file in the cloud.</span></span>

```java
    public static void main(String[] args) {

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            // create a new storage account
            String storageAccountName = SdkContext.randomResourceName("st",8);
            StorageAccount storage = azure.storageAccounts().define(storageAccountName)
                        .withRegion(Region.US_WEST2)
                        .withNewResourceGroup("sampleStorageResourceGroup")
                        .create();

            // create a storage container to hold the files
            List<StorageAccountKey> keys = storage.getKeys();
            final String storageConnection = "DefaultEndpointsProtocol=https;"
                   + "AccountName=" + storage.name()
                   + ";AccountKey=" + keys.get(0).value()
                    + ";EndpointSuffix=core.windows.net";

            CloudStorageAccount account = CloudStorageAccount.parse(storageConnection);
            CloudBlobClient serviceClient = account.createCloudBlobClient();

            // Container name must be lower case.
            CloudBlobContainer container = serviceClient.getContainerReference("helloazure");
            container.createIfNotExists();

            // Make the container public
            BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // write a blob to the container
            CloudBlockBlob blob = container.getBlockBlobReference("helloazure.txt");
            blob.uploadText("hello Azure");

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

<span data-ttu-id="8c541-185">コマンド ラインでサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="8c541-185">Run the sample from the command line:</span></span>

<span data-ttu-id="8c541-186">ストレージ アカウント内の `helloazure.txt` ファイルは、Azure Portal または [Azure ストレージ エクスプローラー](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)から参照することができます。</span><span class="sxs-lookup"><span data-stu-id="8c541-186">You can browse for the `helloazure.txt` file in your storage account through the Azure portal or with [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span></span>

<span data-ttu-id="8c541-187">CLI で次のコマンドを入力して、ストレージ アカウントをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="8c541-187">Clean up the storage account using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="8c541-188">その他のサンプルを探す</span><span class="sxs-lookup"><span data-stu-id="8c541-188">Explore more samples</span></span>

<span data-ttu-id="8c541-189">Azure Management Libraries for Java を使ってリソースを管理したりタスクを自動化したりする方法をさらに詳しく知るには、[仮想マシン](../java-sdk-azure-virtual-machine-samples.md)、[Web アプリ](../java-sdk-azure-web-apps-samples.md)、[SQL データベース](../java-sdk-azure-sql-database-samples.md)に関するサンプル コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-189">To learn more about how to use the Azure management libraries for Java to manage resources and automate tasks, see our sample code for [virtual machines](../java-sdk-azure-virtual-machine-samples.md), [web apps](../java-sdk-azure-web-apps-samples.md) and [SQL database](../java-sdk-azure-sql-database-samples.md).</span></span>

## <a name="reference-and-release-notes"></a><span data-ttu-id="8c541-190">リファレンスとリリース ノート</span><span class="sxs-lookup"><span data-stu-id="8c541-190">Reference and release notes</span></span>

<span data-ttu-id="8c541-191">すべてのパッケージには、[リファレンス](http://docs.microsoft.com/java/api)が提供されています。</span><span class="sxs-lookup"><span data-stu-id="8c541-191">A [reference](http://docs.microsoft.com/java/api) is available for all packages.</span></span>

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="8c541-192">質問とフィードバック</span><span class="sxs-lookup"><span data-stu-id="8c541-192">Get help and give feedback</span></span>

<span data-ttu-id="8c541-193">ご質問は、[Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java) のコミュニティに投稿してください。</span><span class="sxs-lookup"><span data-stu-id="8c541-193">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span></span> <span data-ttu-id="8c541-194">Java 用 Azure ライブラリに関する未解決の問題やバグは、[プロジェクト GitHub](https://github.com/Azure/azure-sdk-for-java) にご報告ください。</span><span class="sxs-lookup"><span data-stu-id="8c541-194">Report bugs and open issues against the Azure libraries for Java on the [project GitHub](https://github.com/Azure/azure-sdk-for-java).</span></span>
