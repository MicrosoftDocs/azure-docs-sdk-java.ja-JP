---
uid: com.microsoft.azure.management.compute._virtual_machine
summary: *content
---

次のコードが構文を使用する Java SDK の fluent を新規作成する方法を示します[標準 D3](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-sizes/#d-series)米国東部データ センター内の Linux 仮想マシン実行中の Ubuntu 16.04 サーバー。

```java
Azure azure = Azure.authenticate(propertiesFile).withDefaultSubscription();
System.out.println("Creating a Linux VM");

VirtualMachine linuxVM = azure.virtualMachines().define("myLinuxVM")
        .withRegion(Region.US_EAST)
        .withNewResourceGroup("myResourceGroup")
        .withNewPrimaryNetwork("10.0.0.0/28")
        .withPrimaryPrivateIpAddressDynamic()
        .withNewPrimaryPublicIpAddress("mylinuxvmdns")
        .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
        .withRootUserName("tirekicker")
        .withSsh("your-ssh-key")
        .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
        .create();

System.out.println("Created a Linux VM: " + linuxVM.id());
```

---
uid: com.microsoft.azure.management.compute._virtual_machine.computerName()
summary: 仮想マシンの名前を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.dataDisks()
summary: 仮想マシンにアタッチされているデータ ディスクの一覧を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.extensions()
summary: 仮想マシンにインストールされている拡張機能を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.getPrimaryPublicIpAddressId()
summary: この仮想マシンのプライマリ ネットワーク インターフェイスに関連付けられているパブリック IP アドレスの識別子を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.licenseType()
summary: 仮想マシンのライセンスの種類を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.osDiskCachingType()
summary: オペレーティング システム ディスクのキャッシュの種類を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.osDiskSize()
summary: オペレーティング システム ディスクのサイズを取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.osDiskVhdUri()
summary: オペレーティング システムを含むバーチャル ハード ディスクの URI を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.osType()
summary: オペレーティング システムの種類を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.plan()
summary: バーチャル マシンのイメージのプランを取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.powerState()
summary: 仮想マシンの電源の状態を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.provisioningState()
summary: 仮想マシンの prrovisioning 状態を取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.size()
summary: 仮想マシンのサイズを取得します。
---

---
uid: com.microsoft.azure.management.compute._virtual_machine.vmId()
summary: 仮想マシンの識別子を取得します。
---