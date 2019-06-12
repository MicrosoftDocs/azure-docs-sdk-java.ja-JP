---
title: Azure および Azure Stack 用の Azul Zulu JDK をインストールする
description: Windows、Linux、Mac での Azure 開発用 Azul Zulu Java Development Kit (JDK) のインストール方法
author: erickson-doug
manager: douge
ms.author: douge
ms.date: 4/19/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 33d2206f9a0a1cc9fadc60b17c1a93f66c592fe3
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568595"
---
# <a name="install-the-jdk-for-azure-and-azure-stack"></a><span data-ttu-id="d5bd2-103">Azure 用の JDK および Azure Stack をインストールする</span><span class="sxs-lookup"><span data-stu-id="d5bd2-103">Install the JDK for Azure and Azure Stack</span></span>

<span data-ttu-id="d5bd2-104">OpenJDK の Azul Zulu Enterprise ビルドは、Microsoft および Azul Systems でサポートされる、Azure と Azure Stack 用の OpenJDK の無料でマルチプラット フォーム対応の実稼働可能なディストリビューションです。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-104">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="d5bd2-105">これらには、Java SE アプリケーションを構築および実行するためのすべてのコンポーネントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-105">They contain all the components for building and running Java SE applications.</span></span>

<span data-ttu-id="d5bd2-106">[クライアント OS ごとに複数のダウンロード パッケージ タイプがサポート](https://www.azul.com/downloads/azure-only/zulu/)されています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-106">There are [multiple download package types supported for each client OS](https://www.azul.com/downloads/azure-only/zulu/).</span></span> <span data-ttu-id="d5bd2-107">Azure Marketplace ギャラリーから、次のプラットフォーム用の仮想マシン イメージを入手することもできます。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-107">You can also get a virtual machine image from the Azure Marketplace Gallery for the following platforms:</span></span>

  * [<span data-ttu-id="d5bd2-108">Azul Zulu:Ubuntu 18.04 上の Java 8</span><span class="sxs-lookup"><span data-stu-id="d5bd2-108">Azul Zulu: Java 8 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-ubuntu-1804)
  * [<span data-ttu-id="d5bd2-109">Azul Zulu:Windows Server 2019 上の Java 8</span><span class="sxs-lookup"><span data-stu-id="d5bd2-109">Azul Zulu: Java 8 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-windows-2019)
  
  * [<span data-ttu-id="d5bd2-110">Azul Zulu:Ubuntu 18.04 上の Java 11</span><span class="sxs-lookup"><span data-stu-id="d5bd2-110">Azul Zulu: Java 11 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-ubuntu-1804)
  * [<span data-ttu-id="d5bd2-111">Azul Zulu:Windows Server 2019 上の Java 11</span><span class="sxs-lookup"><span data-stu-id="d5bd2-111">Azul Zulu: Java 11 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-windows-2019)


> [!NOTE]
> <span data-ttu-id="d5bd2-112">これらの手順は、64 ビット Java 8 バージョンの JDK を対象としています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-112">These instructions target the 64-bit Java 8 version of the JDK.</span></span> <span data-ttu-id="d5bd2-113">Azul には、スタンドアロン インストールとして Java Runtime Environment (JRE) も用意されています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-113">Azul also provides the Java Run-time Environment (JRE) as a stand-alone installation.</span></span> <span data-ttu-id="d5bd2-114">JRE は JDK のインストールに含まれています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-114">The JRE is included with the JDK install.</span></span>
>
>  <span data-ttu-id="d5bd2-115">Java 11 パッケージも [Azul の Azure ダウンロードページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-115">Java 11 packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-windows"></a><span data-ttu-id="d5bd2-116">Windows 用の Azul Zulu JDK をダウンロードしてインストールする</span><span class="sxs-lookup"><span data-stu-id="d5bd2-116">Download and install the Azul Zulu JDKs for Windows</span></span> 

1. <span data-ttu-id="d5bd2-117">お使いのクライアント上の場所 (`C:\Users\<your_login>\Downloads` など) に [64 ビット Azul Zulu JDK 8 を MSI としてダウンロード](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi)します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-117">[Download the 64-bit Azul Zulu JDK 8 as an MSI](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi) to a location on your client, such as `C:\Users\<your_login>\Downloads`.</span></span> <span data-ttu-id="d5bd2-118">(.ZIP パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)</span><span class="sxs-lookup"><span data-stu-id="d5bd2-118">(.ZIP packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="d5bd2-119">そのディレクトリに移動し、ダウンロードした MSI ファイルをダブルクリックしてインストールを開始します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-119">Navigate to the directory and double-click the downloaded MSI file to begin installation.</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-mac"></a><span data-ttu-id="d5bd2-120">Mac 用の Azul Zulu JDK をダウンロードしてインストールする</span><span class="sxs-lookup"><span data-stu-id="d5bd2-120">Download and install the Azul Zulu JDKs for Mac</span></span> 

<span data-ttu-id="d5bd2-121">これらの手順では、ZIP ファイルを Mac にダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-121">These steps download a ZIP file to your Mac.</span></span> <span data-ttu-id="d5bd2-122">DMG バージョンも利用できます。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-122">There is also a DMG version available.</span></span>

1. <span data-ttu-id="d5bd2-123">お使いのクライアント上の場所 (`/Library/Java/JavaVirtualMachines/` など) に [64 ビット Azul Zulu JDK 8 を ZIP ファイルとしてダウンロード](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip)します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-123">[Download the 64-bit Azul Zulu JDK 8 as a ZIP file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip) to a location on your client, such as `/Library/Java/JavaVirtualMachines/`.</span></span> <span data-ttu-id="d5bd2-124">(.DMG パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)</span><span class="sxs-lookup"><span data-stu-id="d5bd2-124">(.DMG packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="d5bd2-125">Finder を起動し、ダウンロード ディレクトリに移動して、ZIP ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-125">Launch Finder, navigate to the download directory, and double-click the ZIP file.</span></span> <span data-ttu-id="d5bd2-126">または、ターミナル コマンド ウィンドウを起動し、そのディレクトリに移動して、次を実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-126">Alternatively, you can launch a terminal command window, navigate to the directory, and run:</span></span>

```cli
unzip <name_of_zulu_package>.zip
```

## <a name="download-and-install-the-azul-zulu-jdks-for-alpine-linux"></a><span data-ttu-id="d5bd2-127">Alpine Linux 用の Azul Zulu JDK をダウンロードしてインストールする</span><span class="sxs-lookup"><span data-stu-id="d5bd2-127">Download and install the Azul Zulu JDKs for Alpine Linux</span></span>

1. <span data-ttu-id="d5bd2-128">お使いのクライアント上の場所 (`/usr/lib/jvm` など) に [64 ビット Azul Zulu JDK 8 を TAR ファイルとしてダウンロード](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz)します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-128">[Download the 64-bit Azul Zulu JDK 8 as a TAR file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz) to a location on your client, such as `/usr/lib/jvm`.</span></span> <span data-ttu-id="d5bd2-129">(.RPM と .DEB パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)</span><span class="sxs-lookup"><span data-stu-id="d5bd2-129">(.RPM and .DEB packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="d5bd2-130">該当のディレクトリに移動し、次のコマンドを実行してファイルを解凍し、展開します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-130">Go to your directory and run the following command to unzip and expand the file:</span></span>

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a><span data-ttu-id="d5bd2-131">インストールを確認する</span><span class="sxs-lookup"><span data-stu-id="d5bd2-131">Confirm your installation</span></span>

<span data-ttu-id="d5bd2-132">インストールを確認するには、コマンド ラインに移動し、`java -version` を実行します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-132">To confirm your installation, go to the command-line and run `java -version`.</span></span>

<span data-ttu-id="d5bd2-133">このコマンドの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-133">The output of the command should be:</span></span>

```cli
$ java -version

openjdk version "1.8.0_212"
OpenJDK Runtime Environment (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 1.8.0_212-b04)
OpenJDK 64-Bit Server VM (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 25.212-b04, mixed mode)

```

## <a name="download-and-install-the-azul-zulu-jdks-from-a-yum-repository"></a><span data-ttu-id="d5bd2-134">Yum リポジトリから Azul Zulu JDK をダウンロードしてインストールする</span><span class="sxs-lookup"><span data-stu-id="d5bd2-134">Download and install the Azul Zulu JDKs from a Yum repository</span></span>

<span data-ttu-id="d5bd2-135">Azul Zulu JDK は、Azul によって [Yum リポジトリ](http://repos.azul.com/azure-only/zulu-azure.repo)で提供されています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-135">The Azul Zulu JDKs are provided in a [Yum repository](http://repos.azul.com/azure-only/zulu-azure.repo) by Azul.</span></span>

<span data-ttu-id="d5bd2-136">**Java 8 用の Azul Zulu JDK をインストールするには、CLI から次のコマンドを実行します。**</span><span class="sxs-lookup"><span data-stu-id="d5bd2-136">**To install the Azul Zulu JDK for Java 8, run the following commands from your CLI:**</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="d5bd2-137">Java 11 の場合は、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-137">For Java 11, run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

<span data-ttu-id="d5bd2-138">Java 12 (プレビュー) の場合は、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-138">For Java 12 (Preview), run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-12-azure-jdk
```

<span data-ttu-id="d5bd2-139">**Yum リポジトリから Zulu JDK 8 パッケージを更新するには:**</span><span class="sxs-lookup"><span data-stu-id="d5bd2-139">**To update a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="d5bd2-140">(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)</span><span class="sxs-lookup"><span data-stu-id="d5bd2-140">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="d5bd2-141">**Yum リポジトリから Zulu JDK 8 パッケージを削除するには:**</span><span class="sxs-lookup"><span data-stu-id="d5bd2-141">**To remove a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -y erase zulu-8-azure-jdk
```
<span data-ttu-id="d5bd2-142">(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)</span><span class="sxs-lookup"><span data-stu-id="d5bd2-142">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a><span data-ttu-id="d5bd2-143">apt-get リポジトリから Azul Zulu JDK をダウンロードしてインストールする</span><span class="sxs-lookup"><span data-stu-id="d5bd2-143">Download and install the Azul Zulu JDKs from an apt-get repository</span></span>

<span data-ttu-id="d5bd2-144">Azul Zulu JDK は、Azul によって [apt-get リポジトリ](http://repos.azul.com/azure-only/zulu/apt)でも提供されています。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-144">The Azul Zulu JDKs are also provided in an [apt-get repository](http://repos.azul.com/azure-only/zulu/apt) by Azul.</span></span>

<span data-ttu-id="d5bd2-145">**apt-get で Java 8 用の Azul Zulu JDK をインストールするには、CLI から次のコマンドを実行します。**</span><span class="sxs-lookup"><span data-stu-id="d5bd2-145">**To install the Azul Zulu JDK for Java 8 with apt-get, run the following commands from your CLI:**</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="d5bd2-146">Java 11 の場合は、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-146">For Java 11, run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

<span data-ttu-id="d5bd2-147">Java 12 (プレビュー) の場合は、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-147">For Java 12 (Preview), run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-12-azure-jdk
```

<span data-ttu-id="d5bd2-148">**apt-get リポジトリから Zulu JDK 8 パッケージを更新するには:**</span><span class="sxs-lookup"><span data-stu-id="d5bd2-148">**To update a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="d5bd2-149">以前のリリースは自動的に削除されます。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-149">The previous release will be automatically removed.</span></span>
<span data-ttu-id="d5bd2-150">(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)</span><span class="sxs-lookup"><span data-stu-id="d5bd2-150">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="d5bd2-151">**apt-get リポジトリから Zulu JDK 8 パッケージを削除するには:**</span><span class="sxs-lookup"><span data-stu-id="d5bd2-151">**To remove a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

<span data-ttu-id="d5bd2-152">(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)</span><span class="sxs-lookup"><span data-stu-id="d5bd2-152">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="d5bd2-153">Azure 開発用 Azul Zulu JDK の準備、インストール、管理に関する詳細なガイダンスについては、[Zulu の公式ドキュメント](https://docs.azul.com/zulu/zuludocs/index.htm)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d5bd2-153">For more detailed guidance on preparing, installing, and managing your Azul Zulu JDKs for Azure development, read [the official Zulu docs](https://docs.azul.com/zulu/zuludocs/index.htm).</span></span>

