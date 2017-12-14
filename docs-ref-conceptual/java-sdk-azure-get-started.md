---
title: "Java 用 Azure ライブラリの概要"
description: "Azure クラウド リソースを作成し、それらのリソースに接続して Java アプリケーションで使用する方法について説明します。"
keywords: "Azure, Java, SDK, API, 認証, 概要"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: get-started-article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: b1e10b79-f75e-4605-aecd-eed64873e2d3
ms.openlocfilehash: 69c75984f6274b5423614bd51c40957d3d509802
ms.sourcegitcommit: 1f6a80e067a8bdbbb4b2da2e2145fda73d5fe65a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
# <a name="get-started-with-cloud-development-using-the-azure-libraries-for-java"></a><span data-ttu-id="dfdac-104">Java 用 Azure ライブラリを使用したクラウド開発の開始</span><span class="sxs-lookup"><span data-stu-id="dfdac-104">Get started with cloud development using the Azure libraries for Java</span></span>

<span data-ttu-id="dfdac-105">このガイドでは、Java での Azure 開発用の開発環境を設定する手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-105">This guide walks you through setting up a development environment for Azure development in Java.</span></span> <span data-ttu-id="dfdac-106">Azure リソースをいくつか作成し、それらのリソースに接続して基本的なタスク (ファイルのアップロードや Web アプリケーションのデプロイなど) を実行します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-106">You'll then create some Azure resources and connect them to to perform some basic tasks, like uploading a file or deploying a web application.</span></span> <span data-ttu-id="dfdac-107">作業が完了すると、独自の Java アプリケーションで Azure サービスの使用を開始できるようになります。</span><span class="sxs-lookup"><span data-stu-id="dfdac-107">When you're done, you'll be ready to start using Azure services in your own Java applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dfdac-108">前提条件</span><span class="sxs-lookup"><span data-stu-id="dfdac-108">Prerequisites</span></span>

- <span data-ttu-id="dfdac-109">Azure アカウント。</span><span class="sxs-lookup"><span data-stu-id="dfdac-109">An Azure account.</span></span> <span data-ttu-id="dfdac-110">所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/)</span></span>
- <span data-ttu-id="dfdac-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) または [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。</span><span class="sxs-lookup"><span data-stu-id="dfdac-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>
- <span data-ttu-id="dfdac-112">[Java 8](https://www.azul.com/downloads/zulu/) (Azure Cloud Shell に付属)</span><span class="sxs-lookup"><span data-stu-id="dfdac-112">[Java 8](https://www.azul.com/downloads/zulu/) (included in Azure Cloud Shell)</span></span>
- <span data-ttu-id="dfdac-113">[Maven 3](http://maven.apache.org/download.cgi) (Azure Cloud Shell に付属)</span><span class="sxs-lookup"><span data-stu-id="dfdac-113">[Maven 3](http://maven.apache.org/download.cgi) (included in Azure Cloud Shell)</span></span>

## <a name="set-up-authentication"></a><span data-ttu-id="dfdac-114">認証の設定</span><span class="sxs-lookup"><span data-stu-id="dfdac-114">Set up authentication</span></span>

<span data-ttu-id="dfdac-115">このチュートリアルのサンプル コードを実行する Java アプリケーションには、Azure サブスクリプションの読み取りと作成のアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="dfdac-115">Your Java application needs read and create permissions in your Azure subscription to run the sample code in this tutorial.</span></span> <span data-ttu-id="dfdac-116">サービス プリンシパルを作成し、その資格情報で動作するようにアプリケーションを構成してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-116">Create a service principal and configure your application to run with its credentials.</span></span> <span data-ttu-id="dfdac-117">サービス プリンシパルによって、自分の ID に関連付けられた非対話型のアカウントを作成し、アプリの実行に必要な権限だけを付与することができます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-117">Service principals provide a way to create a non-interactive account associated with your identity to which you grant only the privileges your app needs to run.</span></span>

<span data-ttu-id="dfdac-118">[Azure CLI 2.0 を使ってサービス プリンシパルを作成](/cli/azure/create-an-azure-service-principal-azure-cli)し、その出力をキャプチャしてください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-118">[Create a service principal using the Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) and capture the output.</span></span> <span data-ttu-id="dfdac-119">password 引数には、`MY_SECURE_PASSWORD` ではなく、[セキュリティで保護されたパスワード](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy)を指定します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-119">Provide a [secure password](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) in the password argument instead of `MY_SECURE_PASSWORD`.</span></span> <span data-ttu-id="dfdac-120">パスワードは 8 ～ 16 文字にし、次の 4 つの条件のうち少なくとも 3 つの条件に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dfdac-120">Your password must be 8 to 16 characters and match at least 3 out of the 4 following criteria:</span></span>

* <span data-ttu-id="dfdac-121">小文字が含まれている</span><span class="sxs-lookup"><span data-stu-id="dfdac-121">Include lowercase characters</span></span>
* <span data-ttu-id="dfdac-122">大文字が含まれている</span><span class="sxs-lookup"><span data-stu-id="dfdac-122">Include uppercase characters</span></span>
* <span data-ttu-id="dfdac-123">数字が含まれている</span><span class="sxs-lookup"><span data-stu-id="dfdac-123">Include numbers</span></span>
* <span data-ttu-id="dfdac-124">次の記号のいずれかが含まれている: @ # $ % ^ & * - _ !</span><span class="sxs-lookup"><span data-stu-id="dfdac-124">Include one of the following symbols: @ # $ % ^ & * - _ !</span></span> <span data-ttu-id="dfdac-125">+ = [ ] { } | \ : ‘ , .</span><span class="sxs-lookup"><span data-stu-id="dfdac-125">+ = [ ] { } | \ : ‘ , .</span></span> <span data-ttu-id="dfdac-126">?</span><span class="sxs-lookup"><span data-stu-id="dfdac-126">?</span></span> <span data-ttu-id="dfdac-127">/ \` ~ “ ( ) ;</span><span class="sxs-lookup"><span data-stu-id="dfdac-127">/ \` ~ “ ( ) ;</span></span>


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

<span data-ttu-id="dfdac-128">次の形式で応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-128">Which gives you a reply in the following format:</span></span>

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

<span data-ttu-id="dfdac-129">次に、ご利用のシステム上のテキスト ファイルに次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="dfdac-129">Next, copy the following into a text file on your system:</span></span>

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

<span data-ttu-id="dfdac-130">上から 4 つの値は、次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-130">Replace the top four values with the following:</span></span>

- <span data-ttu-id="dfdac-131">subscription: Azure CLI 2.0 の `az account show` から得られる *id* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-131">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="dfdac-132">client: サービス プリンシパルの出力から得られる *appId* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-132">client: use the *appId* value from the output taken from a service principal output.</span></span>
- <span data-ttu-id="dfdac-133">key: サービス プリンシパルの出力から得られる *password* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-133">key: use the *password* value from the service principal output.</span></span>
- <span data-ttu-id="dfdac-134">tenant: サービス プリンシパルの出力から得られる *tenant* 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-134">tenant: use the *tenant* value from the service principal output.</span></span>

<span data-ttu-id="dfdac-135">このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-135">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="dfdac-136">このファイルは今後のコードに使用できるため、この記事のアプリケーションの外部の場所に保存することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dfdac-136">You may use this file for future code so it's recommended to store it somewhere external to the application in this article.</span></span>

<span data-ttu-id="dfdac-137">ご利用のシェルから、認証ファイルの完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-137">Set an environment variable `AZURE_AUTH_LOCATION` with the full path to the authentication file in your shell.</span></span>   

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="dfdac-138">Windows 環境で作業している場合は、システムのプロパティに変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-138">If you're working in a windows environment, add the variable to your system properties.</span></span> <span data-ttu-id="dfdac-139">PowerShell を開き、2 番目の変数をファイルのパスに置き換えた後、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-139">Open PowerShell and, after replacing the second variable with the path to your file, enter the following command:</span></span>

```powershell
[Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\<fullpath>\azureauth.properties", "Machine")
```

## <a name="create-a-new-maven-project"></a><span data-ttu-id="dfdac-140">新しい Maven プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="dfdac-140">Create a new Maven project</span></span>

> [!NOTE]
> <span data-ttu-id="dfdac-141">このガイドでは、Maven ビルド ツールを使って、サンプル コードをビルドして実行していますが、Java 用 Azure ライブラリは他のビルド ツール (Gradle など) で使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-141">This guide uses Maven build tool to build and run the sample code, but other build tools such as Gradle also work with the Azure libraries for Java.</span></span> 

<span data-ttu-id="dfdac-142">コマンド ラインを使って、ご利用のシステム上の新しいディレクトリに Maven プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-142">Create a Maven project from the command line in a new directory on your system:</span></span>

```
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp  \ 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

<span data-ttu-id="dfdac-143">これにより、基本的な Maven プロジェクトが `testAzureApp` フォルダーに作成されます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-143">This creates a basic Maven project under the `testAzureApp` folder.</span></span> <span data-ttu-id="dfdac-144">プロジェクトの `pom.xml` に次のエントリを追加して、このチュートリアルのサンプル コードで使用するライブラリをインポートします。</span><span class="sxs-lookup"><span data-stu-id="dfdac-144">Add the following entries into the project `pom.xml` to import the libraries used in the sample code in this tutorial.</span></span>

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

<span data-ttu-id="dfdac-145">[maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) を使ってサンプルを実行するために、最上位の `project` 要素に `build` エントリを追加します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-145">Add a `build` entry under the top-level `project` element to use the [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) to run the samples:</span></span>

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```
   
## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="dfdac-146">Linux 仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="dfdac-146">Create a Linux virtual machine</span></span>

<span data-ttu-id="dfdac-147">プロジェクトの `src/main/java` ディレクトリに `AzureApp.java` という名前の新しいファイルを作成し、次のコード ブロックを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-147">Create a new file named `AzureApp.java` in the project's `src/main/java` directory and paste in the following block of code.</span></span> <span data-ttu-id="dfdac-148">`userName` 変数と `sshKey` 変数は、ご利用のマシンの実際の値に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-148">Update the `userName` and `sshKey` variables with real values for your machine.</span></span> <span data-ttu-id="dfdac-149">このコードによって、米国東部 Azure リージョンで実行されるリソース グループ `sampleResourceGroup` に、`testLinuxVM` という名前の新しい Linux VM が作成されます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-149">The code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleResourceGroup` running in the US East Azure region.</span></span>

```java
package com.fabrikam.AzureApp;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
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

<span data-ttu-id="dfdac-150">コマンド ラインでサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-150">Run the sample from the command line:</span></span>

```
mvn compile exec:java
```

<span data-ttu-id="dfdac-151">コンソールには、REST の要求と応答がいくつか表示されます。これは、SDK が内部的に Azure REST API を呼び出して、仮想マシンとそのリソースを構成しているためです。</span><span class="sxs-lookup"><span data-stu-id="dfdac-151">You'll see some REST requests and responses in the console as the SDK makes the underlying calls to the Azure REST API to configure the virtual machine and its resources.</span></span> <span data-ttu-id="dfdac-152">プログラムの実行が完了したら、サブスクリプション内の仮想マシンを Azure CLI 2.0 で確認します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-152">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="dfdac-153">コードが正しく動作したことを確認したら、CLI で VM とそのリソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-153">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="dfdac-154">GitHub リポジトリからの Web アプリのデプロイ</span><span class="sxs-lookup"><span data-stu-id="dfdac-154">Deploy a web app from a GitHub repo</span></span>

<span data-ttu-id="dfdac-155">`AzureApp.java` の main メソッドを以下のメソッドに差し替えます。`appName` 変数には、コードを実行する前に一意の値を指定してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-155">Replace the main method in `AzureApp.java` with the one below, updating the `appName` variable to a unique value before running the code.</span></span> <span data-ttu-id="dfdac-156">このコードは、無料の価格レベルで稼働する新しい [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) に、パブリック GitHub リポジトリの `master` ブランチから Web アプリケーションをデプロイするものです。</span><span class="sxs-lookup"><span data-stu-id="dfdac-156">This code deploys a web application from the `master` branch in a public GitHub repo into a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free pricing tier.</span></span>

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

<span data-ttu-id="dfdac-157">先ほどと同様、このコードも Maven を使用して実行します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-157">Run the code as before using Maven:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="dfdac-158">ブラウザーを開いてアプリケーションにアクセスします。CLI から次のコマンドを入力してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-158">Open a browser pointed to the application using the CLI:</span></span>

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

<span data-ttu-id="dfdac-159">デプロイを検証したら、Web アプリとプランをサブスクリプションから削除します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-159">Remove the web app and plan from your subscription once you've verified the deployment.</span></span>

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a><span data-ttu-id="dfdac-160">Azure SQL Database に接続する</span><span class="sxs-lookup"><span data-stu-id="dfdac-160">Connect to an Azure SQL database</span></span>

<span data-ttu-id="dfdac-161">`AzureApp.java` にある現行の main メソッドを以下のコードに差し替えます。`dbPassword` 変数には、実際の値を設定してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-161">Replace the current main method in `AzureApp.java` with the code below, setting a real value for the `dbPassword` variable.</span></span>
<span data-ttu-id="dfdac-162">このコードは、リモート アクセスを許可するファイアウォール規則で新しい SQL データベースを作成し、SQL Database JDBC ドライバーを使ってそのデータベースに接続するものです。</span><span class="sxs-lookup"><span data-stu-id="dfdac-162">This code creates a new SQL database with a firewall rule allowing remote access,  and then connects to it using the SQL Database JBDC driver.</span></span> 

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
<span data-ttu-id="dfdac-163">コマンド ラインでサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-163">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="dfdac-164">その後、CLI で次のコマンドを入力して、リソースをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="dfdac-164">Then clean up the resources using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="dfdac-165">新しいストレージ アカウントへの BLOB の書き込み</span><span class="sxs-lookup"><span data-stu-id="dfdac-165">Write a blob into a new storage account</span></span>

<span data-ttu-id="dfdac-166">`AzureApp.java` にある現行の main メソッドを以下のコードに差し替えます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-166">Replace the current main method in `AzureApp.java` with the code below.</span></span> <span data-ttu-id="dfdac-167">このコードは、[Azure ストレージ アカウント](https://docs.microsoft.com/azure/storage/storage-introduction)を作成し、Azure Storage Libraries for Java を使って新しいテキスト ファイルをクラウドに作成するものです。</span><span class="sxs-lookup"><span data-stu-id="dfdac-167">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Java to create a new text file in the cloud.</span></span>

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

        // create a storage container to hold the file
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

<span data-ttu-id="dfdac-168">コマンド ラインでサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="dfdac-168">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="dfdac-169">ストレージ アカウント内の `helloazure.txt` ファイルは、Azure Portal または [Azure ストレージ エクスプローラー](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)から参照することができます。</span><span class="sxs-lookup"><span data-stu-id="dfdac-169">You can browse for the `helloazure.txt` file in your storage account through the Azure portal or with [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span></span>

<span data-ttu-id="dfdac-170">CLI で次のコマンドを入力して、ストレージ アカウントをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="dfdac-170">Clean up the storage account using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="dfdac-171">その他のサンプルを探す</span><span class="sxs-lookup"><span data-stu-id="dfdac-171">Explore more samples</span></span>

<span data-ttu-id="dfdac-172">Azure Management Libraries for Java を使ってリソースを管理したりタスクを自動化したりする方法をさらに詳しく知るには、[仮想マシン](java-sdk-azure-virtual-machine-samples.md)、[Web アプリ](java-sdk-azure-web-apps-samples.md)、[SQL データベース](java-sdk-azure-sql-database-samples.md)に関するサンプル コードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-172">To learn more about how to use the Azure management libraries for Java to manage resources and automate tasks, see our sample code for [virtual machines](java-sdk-azure-virtual-machine-samples.md), [web apps](java-sdk-azure-web-apps-samples.md) and [SQL database](java-sdk-azure-sql-database-samples.md).</span></span>

## <a name="reference-and-release-notes"></a><span data-ttu-id="dfdac-173">リファレンスとリリース ノート</span><span class="sxs-lookup"><span data-stu-id="dfdac-173">Reference and release notes</span></span>

<span data-ttu-id="dfdac-174">すべてのパッケージには、[リファレンス](http://docs.microsoft.com/java/api)が提供されています。</span><span class="sxs-lookup"><span data-stu-id="dfdac-174">A [reference](http://docs.microsoft.com/java/api) is available for all packages.</span></span>

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="dfdac-175">質問とフィードバック</span><span class="sxs-lookup"><span data-stu-id="dfdac-175">Get help and give feedback</span></span>

<span data-ttu-id="dfdac-176">ご質問は、[Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java) のコミュニティに投稿してください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-176">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span></span> <span data-ttu-id="dfdac-177">Java 用 Azure ライブラリに関する未解決の問題やバグは、[プロジェクト GitHub](https://github.com/Azure/azure-sdk-for-java) にご報告ください。</span><span class="sxs-lookup"><span data-stu-id="dfdac-177">Report bugs and open issues against the Azure libraries for Java on the [project GitHub](https://github.com/Azure/azure-sdk-for-java).</span></span>
