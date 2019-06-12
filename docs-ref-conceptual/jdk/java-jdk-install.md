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
# <a name="install-the-jdk-for-azure-and-azure-stack"></a>Azure 用の JDK および Azure Stack をインストールする

OpenJDK の Azul Zulu Enterprise ビルドは、Microsoft および Azul Systems でサポートされる、Azure と Azure Stack 用の OpenJDK の無料でマルチプラット フォーム対応の実稼働可能なディストリビューションです。 これらには、Java SE アプリケーションを構築および実行するためのすべてのコンポーネントが含まれています。

[クライアント OS ごとに複数のダウンロード パッケージ タイプがサポート](https://www.azul.com/downloads/azure-only/zulu/)されています。 Azure Marketplace ギャラリーから、次のプラットフォーム用の仮想マシン イメージを入手することもできます。

  * [Azul Zulu:Ubuntu 18.04 上の Java 8](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-ubuntu-1804)
  * [Azul Zulu:Windows Server 2019 上の Java 8](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-windows-2019)
  
  * [Azul Zulu:Ubuntu 18.04 上の Java 11](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-ubuntu-1804)
  * [Azul Zulu:Windows Server 2019 上の Java 11](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-windows-2019)


> [!NOTE]
> これらの手順は、64 ビット Java 8 バージョンの JDK を対象としています。 Azul には、スタンドアロン インストールとして Java Runtime Environment (JRE) も用意されています。 JRE は JDK のインストールに含まれています。
>
>  Java 11 パッケージも [Azul の Azure ダウンロードページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。

## <a name="download-and-install-the-azul-zulu-jdks-for-windows"></a>Windows 用の Azul Zulu JDK をダウンロードしてインストールする 

1. お使いのクライアント上の場所 (`C:\Users\<your_login>\Downloads` など) に [64 ビット Azul Zulu JDK 8 を MSI としてダウンロード](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi)します。 (.ZIP パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)

2. そのディレクトリに移動し、ダウンロードした MSI ファイルをダブルクリックしてインストールを開始します。

## <a name="download-and-install-the-azul-zulu-jdks-for-mac"></a>Mac 用の Azul Zulu JDK をダウンロードしてインストールする 

これらの手順では、ZIP ファイルを Mac にダウンロードします。 DMG バージョンも利用できます。

1. お使いのクライアント上の場所 (`/Library/Java/JavaVirtualMachines/` など) に [64 ビット Azul Zulu JDK 8 を ZIP ファイルとしてダウンロード](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip)します。 (.DMG パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)

2. Finder を起動し、ダウンロード ディレクトリに移動して、ZIP ファイルをダブルクリックします。 または、ターミナル コマンド ウィンドウを起動し、そのディレクトリに移動して、次を実行することもできます。

```cli
unzip <name_of_zulu_package>.zip
```

## <a name="download-and-install-the-azul-zulu-jdks-for-alpine-linux"></a>Alpine Linux 用の Azul Zulu JDK をダウンロードしてインストールする

1. お使いのクライアント上の場所 (`/usr/lib/jvm` など) に [64 ビット Azul Zulu JDK 8 を TAR ファイルとしてダウンロード](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz)します。 (.RPM と .DEB パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)

2. 該当のディレクトリに移動し、次のコマンドを実行してファイルを解凍し、展開します。

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a>インストールを確認する

インストールを確認するには、コマンド ラインに移動し、`java -version` を実行します。

このコマンドの出力は次のようになります。

```cli
$ java -version

openjdk version "1.8.0_212"
OpenJDK Runtime Environment (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 1.8.0_212-b04)
OpenJDK 64-Bit Server VM (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 25.212-b04, mixed mode)

```

## <a name="download-and-install-the-azul-zulu-jdks-from-a-yum-repository"></a>Yum リポジトリから Azul Zulu JDK をダウンロードしてインストールする

Azul Zulu JDK は、Azul によって [Yum リポジトリ](http://repos.azul.com/azure-only/zulu-azure.repo)で提供されています。

**Java 8 用の Azul Zulu JDK をインストールするには、CLI から次のコマンドを実行します。**

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

Java 11 の場合は、次を実行します。

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

Java 12 (プレビュー) の場合は、次を実行します。

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-12-azure-jdk
```

**Yum リポジトリから Zulu JDK 8 パッケージを更新するには:**

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)

**Yum リポジトリから Zulu JDK 8 パッケージを削除するには:**

```cli
sudo yum -y erase zulu-8-azure-jdk
```
(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a>apt-get リポジトリから Azul Zulu JDK をダウンロードしてインストールする

Azul Zulu JDK は、Azul によって [apt-get リポジトリ](http://repos.azul.com/azure-only/zulu/apt)でも提供されています。

**apt-get で Java 8 用の Azul Zulu JDK をインストールするには、CLI から次のコマンドを実行します。**

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

Java 11 の場合は、次を実行します。

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

Java 12 (プレビュー) の場合は、次を実行します。

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-12-azure-jdk
```

**apt-get リポジトリから Zulu JDK 8 パッケージを更新するには:**

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

以前のリリースは自動的に削除されます。
(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)

**apt-get リポジトリから Zulu JDK 8 パッケージを削除するには:**

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

(バージョン 11 または 12 を使用している場合は、上記のコマンドでバージョン番号を変更します。)

Azure 開発用 Azul Zulu JDK の準備、インストール、管理に関する詳細なガイダンスについては、[Zulu の公式ドキュメント](https://docs.azul.com/zulu/zuludocs/index.htm)をご覧ください。

