---
title: Azure Virtual Machine Libraries for Java
description: 
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
ms.openlocfilehash: b414f4dc9289958c2562ddc6d41133279f47a45e
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2017
---
# <a name="azure-virtual-machine-libraries"></a>Azure 仮想マシン ライブラリ

## <a name="overview"></a>概要

Linux または Windows を実行するオンデマンドのスケーラブルなコンピューティング リソースです。

Azure 仮想マシンの概要については、「[Azure Portal で Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-portal)」を参照してください。

## <a name="management-api"></a>Management API

Azure の Windows 仮想マシンと Linux 仮想マシンは、コードから Management API を使って作成、構成、スケールアウトすることができます。

プロジェクトで Management API を使用するには、Maven の `pom.xml` ファイルに[依存関係を追加](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)します。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-compute</artifactId>
    <version>1.2.1</version>
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
> [Management API を探す](/java/api/overview/azure/virtualmachines/managementapi)


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