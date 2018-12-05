---
title: Azure のルート証明書を Java CA ストアに追加する
description: Microsoft Azure で使用する Java CA 証明書 (cacerts) ストアに証明機関 (CA) のルート証明書を追加する方法について説明します。
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/13/2018
ms.author: robmcm
ms.openlocfilehash: 477cb9347255928f8583af8fbe4ea90a42ce6c18
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339046"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a><span data-ttu-id="379e2-103">ルート証明書を Java CA 証明書ストアに追加する方法</span><span class="sxs-lookup"><span data-stu-id="379e2-103">Adding a root certificate to the Java CA certificates store</span></span>

<span data-ttu-id="379e2-104">Azure のサービス (Azure Service Bus など) を使用するアプリケーションでは、Baltimore CyberTrust Root 証明書を信頼する必要があります。</span><span class="sxs-lookup"><span data-stu-id="379e2-104">Applications that use Azure services (such as Azure Service Bus) need to trust the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="379e2-105">この証明書はご使用のシステムに既にインストールされている可能性があります。そうでない場合、このチュートリアルの手順では、Oracle の**keytool** を使用して、必要な証明書機関 (CA) のルート証明書を Azure サービスで使用する Java CA 証明書 (cacerts) ストアに追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="379e2-105">This certificate may already be installed on your system, but if it is not, the steps in this tutorial will show you how to use Oracle's **keytool** to add the required certificate authority (CA) root certificate to the Java CA certificate (cacerts) store that you will use for Azure services.</span></span>

<span data-ttu-id="379e2-106">Oracle の keytool ユーティリティは、"_キーと証明書管理ツール_" であり、開発者はこれを使用することで Java で使用する信頼された証明書の一覧を管理できます。</span><span class="sxs-lookup"><span data-stu-id="379e2-106">Oracle's keytool utility is a _Key and Certificate Management Tool_, which allows developers to manage the list of trusted certificates for use with Java.</span></span> <span data-ttu-id="379e2-107">JDK を圧縮して Azure プロジェクトの **approot** フォルダーに追加する前に、keytool を使用して CA 証明書を追加できます。または、keytool を使用して証明書を追加する Azure スタートアップ タスクを実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="379e2-107">You can use keytool to add the CA certificate before zipping your JDK and adding it to your Azure project's **approot** folder, or you could run an Azure start-up task that uses keytool to add the certificate.</span></span>

<span data-ttu-id="379e2-108">2013 年 4 月 15 日より、Azure は GTE CyberTrust Global ルート証明書から Baltimore CyberTrust ルート証明書への移行を開始しました。</span><span class="sxs-lookup"><span data-stu-id="379e2-108">Beginning April 15, 2013, Azure began migrating from the GTE CyberTrust Global root certificate to the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="379e2-109">次の手順では、keytool を使用して Baltimore CyberTrust ルート証明書を、お使いの Java CA 証明書 (cacerts) ストアに追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="379e2-109">The following steps show you how to use keytool to add the Baltimore CyberTrust root certificate to your Java CA certificate (cacerts) store.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="379e2-110">この記事の手順を使用すると、他の信頼された証明機関のルート証明書を信頼するように Java SDK を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="379e2-110">You can use the steps in this article to configure your Java SDK to trust the root certificates from other trusted certificate authorities.</span></span> <span data-ttu-id="379e2-111">たとえば、「[GeoTrust Root Certificates (GeoTrust ルート証明書)](http://www.geotrust.com/resources/root-certificates/)」に記載されている証明書の一覧からルート証明書を選択できます。</span><span class="sxs-lookup"><span data-stu-id="379e2-111">For example, you might choose a root certificate from the list of certificates at [GeoTrust Root Certificates](http://www.geotrust.com/resources/root-certificates/).</span></span>
> 

## <a name="determining-which-root-certificates-are-installed"></a><span data-ttu-id="379e2-112">インストールされているルート証明書を確認する</span><span class="sxs-lookup"><span data-stu-id="379e2-112">Determining which root certificates are installed</span></span>

<span data-ttu-id="379e2-113">Baltimore 証明書が cacerts ストアに既にインストールされている場合があるため、次の手順に従ってこれが既にインストールされているかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="379e2-113">The Baltimore certificate might already be installed in your cacerts store, so you need to use the following steps to determine if it has already been installed.</span></span>

1. <span data-ttu-id="379e2-114">管理者コマンド プロンプトで、JDK の **jdk\jre\lib\security** フォルダーに移動し、次のコマンドを実行して、お使いのシステムにインストールされている証明書を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="379e2-114">At an administrator command prompt, navigate to your JDK's **jdk\jre\lib\security** folder, and then run the following command to list the certificates that are installed on your system:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

1. <span data-ttu-id="379e2-115">ストアのパスワードを入力するように求められた場合、既定のパスワードは **changeit** です。</span><span class="sxs-lookup"><span data-stu-id="379e2-115">If you are prompted for the store password, the default password is **changeit**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="379e2-116">ストアのパスワードを変更する場合は、<http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html> にある keytool のドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="379e2-116">If you want to change the store password, see the keytool documentation at <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>
   > 

1. <span data-ttu-id="379e2-117">拇印 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` のある証明書が表示されない場合は、次のセクションの手順を使用して証明書をダウンロードし、インストールします。</span><span class="sxs-lookup"><span data-stu-id="379e2-117">If you do not see the certificate with the thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, use the steps in the following section to download and install the certificate.</span></span>

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a><span data-ttu-id="379e2-118">ルート証明書を cacerts ストアに追加するには</span><span class="sxs-lookup"><span data-stu-id="379e2-118">To add a root certificate to the cacerts store</span></span>

1. <span data-ttu-id="379e2-119">Baltimore CyberTrust ルート証明書を <https://cacert.omniroot.com/bc2025.crt> からダウンロードし、**jdk \jre\lib\security** フォルダー内のローカル ファイルとしてファイル名拡張子 **.cer** を付けて保存でします。</span><span class="sxs-lookup"><span data-stu-id="379e2-119">Download the Baltimore CyberTrust root certificate from <https://cacert.omniroot.com/bc2025.crt>, and save to a local file with extension **.cer** in your **jdk\jre\lib\security** folder.</span></span> <span data-ttu-id="379e2-120">この例では、Baltimore CyberTrust ルート証明書ファイルを **bc2025.cer** としてダウンロードしたものと想定しています。</span><span class="sxs-lookup"><span data-stu-id="379e2-120">For this example, assume that you downloaded the Baltimore CyberTrust root certificate file as **bc2025.cer**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="379e2-121">Baltimore CyberTrust ルート証明書には、シリアル番号 `02:00:00:b9` と SHA1 拇印 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` があります。</span><span class="sxs-lookup"><span data-stu-id="379e2-121">The Baltimore CyberTrust root certificate has a serial number of `02:00:00:b9`, and a SHA1 thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`.</span></span>
   > 

2. <span data-ttu-id="379e2-122">次のコマンドを使用して、この証明書を cacerts ストアにインポートします。</span><span class="sxs-lookup"><span data-stu-id="379e2-122">Import the certificate to the cacerts store by using the following command:</span></span>

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```
   <span data-ttu-id="379e2-123">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="379e2-123">Where:</span></span>

   |  <span data-ttu-id="379e2-124">パラメーター</span><span class="sxs-lookup"><span data-stu-id="379e2-124">Parameter</span></span>   |                              <span data-ttu-id="379e2-125">説明</span><span class="sxs-lookup"><span data-stu-id="379e2-125">Description</span></span>                               |
   |--------------|------------------------------------------------------------------------|
   | `keystore`   | <span data-ttu-id="379e2-126">証明書ストアを指定します。</span><span class="sxs-lookup"><span data-stu-id="379e2-126">Specifies the certificate store.</span></span>                                       |
   | `importcert` | <span data-ttu-id="379e2-127">証明書をインポートすることを指定します。</span><span class="sxs-lookup"><span data-stu-id="379e2-127">Specifies that you are importing a certificate.</span></span>                        |
   | `alias`      | <span data-ttu-id="379e2-128">証明書のエイリアスを指定します。</span><span class="sxs-lookup"><span data-stu-id="379e2-128">Specifies an alias for the certificate.</span></span>                                |
   | `file`       | <span data-ttu-id="379e2-129">インポートするルート証明書のファイル名を指定します。</span><span class="sxs-lookup"><span data-stu-id="379e2-129">Specifies the filename of the root certificate that you are importing.</span></span> |


3. <span data-ttu-id="379e2-130">証明書を信頼するよう求めるメッセージが表示された場合は、拇印が `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` であることを確認し、拇印が正しければ **y** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="379e2-130">If you are prompted to trust the certificate, verify the thumbprint as `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, and type **y** if the thumbprint is correct.</span></span>

4. <span data-ttu-id="379e2-131">次のコマンドを実行して CA 証明書が正常にインポートされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="379e2-131">Run the following command to ensure the CA certificate has been successfully imported:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

<span data-ttu-id="379e2-132">ルート証明書を JDK に正常に追加したら、JDK の内容を zip 圧縮し、Azure プロジェクトの **approot** フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="379e2-132">After you have successfully added the root certificate to your JDK, you can zip the contents of JDK and add it to your Azure project's **approot** folder.</span></span>

## <a name="next-steps"></a><span data-ttu-id="379e2-133">次の手順</span><span class="sxs-lookup"><span data-stu-id="379e2-133">Next steps</span></span>

<span data-ttu-id="379e2-134">keytool ユーティリティの詳細については、<http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="379e2-134">For more information about the keytool utility, see <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>

<span data-ttu-id="379e2-135">Java の詳細については、「[Azure for Java developers (Java 開発者向けの Azure)](/java/azure)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="379e2-135">For more information about Java, see [Azure for Java developers](/java/azure).</span></span>

<!-- For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx). -->

<span data-ttu-id="379e2-136">Azure での開発時に使用可能なサポート対象 JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="379e2-136">For more information about the supported JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>