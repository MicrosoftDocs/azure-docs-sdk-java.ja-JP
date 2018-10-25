---
title: Azure Virtual Machine Libraries for Java
description: ''
keywords: Azure, Java, SDK, API, Compute , Virtual Machines
author: douge
ms.author: douge
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: compute
ms.openlocfilehash: ea71c556f32a83d59d76b5fed8f3be26229f0cb1
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799818"
---
# <a name="azure-virtual-machine-libraries"></a>Azure 仮想マシン ライブラリ

## <a name="overview"></a>概要

Linux または Windows を実行するオンデマンドのスケーラブルなコンピューティング リソースです。

Azure 仮想マシンの概要については、「[Azure Portal で Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-portal)」を参照してください。

## <a name="management-api"></a>管理 API

Azure の Windows 仮想マシンと Linux 仮想マシンは、コードから Management API を使って作成、構成、スケールアウトすることができます。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-compute</artifactId>
    <version>1.3.0</version>
</dependency>
```   


## <a name="example"></a>例

新しい Azure リソース グループに新しい Linux 仮想マシンを作成します。

```java
VirtualMachine newLinuxVm = azure.virtualMachines().define(linuxVmName)
            .withRegion(Region.US_EAST)
            .withNewResourceGroup("myResourceGroup")
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
            .withRootUsername(userName)
            .withSshKey(key)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

> [!div class="nextstepaction"]
> [Management API を探す](/java/api/overview/azure/virtualmachines/management)


## <a name="samples"></a>サンプル

[仮想マシンの管理][1]   
[仮想ネットワークの管理][6]   
[カスタム イメージからの仮想マシンの作成][2]   
[複数のリージョンに対して同時に仮想マシンを作成する][5]    
[ロード バランサーを使って仮想マシン スケール セットを作成する][7]    

[1]: ../docs-ref-conceptual/java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[5]: ../docs-ref-conceptual/java-sdk-virtual-machines-in-parallel.md
[6]: ../docs-ref-conceptual/java-sdk-manage-virtual-networks.md
[7]: ../docs-ref-conceptual/java-sdk-manage-vm-scalesets.md

アプリで利用できる [Azure 仮想マシンのサンプル Java コード](https://azure.microsoft.com/resources/samples/?platform=java&term=VM)を探しましょう。