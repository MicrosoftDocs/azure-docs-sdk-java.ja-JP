---
title: Java 用 Azure 管理ライブラリ開発者ガイド
description: Java 用管理ライブラリを使用して Azure でクラウド リソースを管理する際のパターンと概念。
keywords: Azure, Java, SDK, API, Maven, Gradle, 認証, active directory, サービス プリンシパル
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: f452468b-7aae-4944-abad-0b1aaf19170d
ms.openlocfilehash: 8b52981ddfaadb7227cea4c7df014011196339cb
ms.sourcegitcommit: 1f6a80e067a8bdbbb4b2da2e2145fda73d5fe65a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
ms.locfileid: "26184646"
---
# <a name="patterns-and-best-practices-for-development-with-the-azure-libraries-for-java"></a>Java 用 Azure ライブラリを使用した開発のパターンとベスト プラクティス 

この記事では、プロジェクトで Java 用 Azure ライブラリを使用する際の一連のパターンとベスト プラクティスを示します。 これらのパターンやガイドラインに従って開発すると、管理するコードの量が減り、管理ライブラリの今後の更新時にリソースの追加や構成が容易になります。

## <a name="build-resources-through-a-fluent-interface"></a>fluent インターフェイスを通じたリソースの作成

fluent インターフェイスとは、オブジェクトの属性を正しく構成するメソッド チェーンを使ってオブジェクトを作成するパターンです。 たとえば新しい Azure ストレージ アカウントを作成するには、次のように記述します。

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

メソッド チェーンを記述する過程で、次に呼び出すメソッドの候補が、流暢な対話形式で IDE に表示されます。   

![IntelliJ の fluent チェーンによるコマンド補完を示す GIF](media/intelliJFluent.gif)

定義しようとする Azure リソースに合ったメソッドを、IDE に表示される候補の中から選んでつなげていきます。 必要なメソッドがチェーンに欠落している場合、その部分がエラーとして IDE によって強調表示されます。

## <a name="resource-collections"></a>リソースのコレクション

管理ライブラリは、最上位の `com.microsoft.azure.management.Azure` オブジェクトを通じてリソースを作成したり更新したりする際の、1 つの入口としての役割を果たします。 操作の対象となるリソースの種類は、`Azure` オブジェクトに定義されているリソース コレクションのメソッドを使って選択します。 たとえば SQL Database の場合は次のように記述します。

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a>リストと反復

それぞれのリソース コレクションには、そのリソースのすべてのインスタンスを現在のサブスクリプションから返す `list()` メソッドがあります。 たとえば、`azure.sqlServers().list()` とした場合、そのサブスクリプションに存在する SQL データベースがすべて返されます。

取得するリストの範囲を特定の [Azure リソース グループ](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)に限定するには、`listByResourceGroup(String groupname)` メソッドを使用します。  

返された `PagedList<T>` コレクションは、通常の `List<T>` とまったく同じように、検索したり反復処理を行ったりすることができます。

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a>クエリから返されるコレクション

管理ライブラリは、クエリ結果の構造に基づく特定のコレクション型を返します。

- `List<T>`: 順序付けされていないデータで、検索と反復処理を簡単に行うことができます。
- `Map<T>`: Map はキー/値のペアです。一意のキーを持ちますが、値が一意であるとは限りません。 Map の例としては、App Service Web Apps のアプリケーション設定があります。
- `Set<T>`: Set は、一意のキーと値を持ちます。 たとえば仮想マシンにアタッチされたネットワークは Set であり、一意の識別子 (キー) と一意のネットワーク構成 (値) の両方を持ちます。

## <a name="actionable-verbs"></a>アクション可能な動詞

名前に動詞を含んだメソッドを実行すると、Azure で何らかのアクションが直ちに実行されます。 これらのメソッドは同期的に動作するため、現在のスレッドで実行されている処理は、メソッドが完了するまでブロックされます。 

| 動詞   |  使用例 |
|--------|---------------|
| create | `azure.virtualMachines().create(listOfVMCreatables)` |
| apply  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| 削除 | `azure.disks().deleteById(id)` | 
| list   | `azure.sqlServers().list()` | 
| get    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> `define()` と `update()` は動詞ですが、その後で `create()` または `apply()` が続かない限り、他の処理がブロックされることはありません。
 
いくつかのメソッドには、[Reactive 拡張機能](https://github.com/ReactiveX/RxJava)を使った非同期版が存在し、`Async` というサフィックスが付きます。 

一部のオブジェクトには、他にも、Azure のリソースの状態を変更するメソッドが存在します。 たとえば `VirtualMachine` の `restart()` があります。

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
これらのメソッドには必ずしも非同期版があるとは限らず、同じスレッド上で実行されている処理は、メソッドの完了までブロックされます。

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a>リソースの遅延作成

Azure リソースを作成するとき、まだ存在しない別のリソースに新しいリソースが依存していると、作成には困難が伴います。 たとえば、新しい仮想マシンを作成するときに、パブリック IP アドレスを予約したり、ディスクを設定したりする場合があります。 開発者が求めているのは、仮想マシンを作成するときに、これらのリソースが確実に割り当てられるようにすることです。アドレスが正しく予約されたかどうかやディスクが正しく作成されたかどうかを確かめることではありません。

`Creatable<T>` オブジェクトを使用すると、コード内で使用する Azure リソースを事前に、つまり実際のサブスクリプションで作成されるのを待たずに定義することができます。 `Creatable<T>` オブジェクトの作成は、実際に必要になるまで、管理ライブラリによって先延ばしされます。

Azure リソースの `Creatable<T>` オブジェクトは、`define()` 動詞を使って作成します。

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

この例の `Creatable<PublicIPAddress>` によって定義される Azure リソースは、このコードが実行される時点では、サブスクリプションに存在しません。  この IP アドレスを使って他の Azure リソースを作成するには、`publicIPAddressCreatable` オブジェクトを使用します。 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

`Creatable<T>` のリソースがサブスクリプションに作成されるのは、そのオブジェクトを使って定義されたリソースが、`create()` を使って Azure に作成されたときです。 引き続き IP アドレスと仮想マシンの例を次に示します。

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

`create()` の呼び出しに `Creatable<T>` を渡すと、単一のリソース オブジェクトではなく `CreatedResources` オブジェクトが返されます。  `CreatedResources<T>` オブジェクトを使用すると、`create()` の呼び出しで作成された (呼び出しの中で型指定したリソースだけでなく) すべてのリソースにアクセスすることができます。 上の例で作成した仮想マシンに関して Azure に作成されたパブリック IP アドレスには、次のようにしてアクセスできます。

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a>例外処理

管理ライブラリの Exception クラスは `com.microsoft.rest.RestException` を拡張したものです。 管理ライブラリによって生成された例外は、対応する `try` ステートメントに続く `catch (RestException exception)` ブロックでキャッチします。

## <a name="logs-and-trace"></a>ログとトレース

エントリ ポイントとなる `Azure` オブジェクトを作成するときに管理ライブラリから出力されるログの量は、`withLogLevel()` を使って構成することができます。 次のトレース レベルが存在します。

| トレース レベル | 有効になるログ 
| ------------ | ---------------
| com.microsoft.rest.LogLevel.NONE | 出力なし
| com.microsoft.rest.LogLevel.BASIC | 基になる REST 呼び出しの URL、応答コード、時間がログに記録されます
| com.microsoft.rest.LogLevel.BODY | BASIC の全出力内容に加えて、REST 呼び出しの要求と応答の本文がログに記録されます
| com.microsoft.rest.LogLevel.HEADERS | BASIC の全出力内容に加えて、REST 呼び出しの要求と応答のヘッダーがログに記録されます
| com.microsoft.rest.LogLevel.BODY_AND_HEADERS | BODY と HEADERS の両ログ レベルの全出力内容が記録されます

[Log4J 2](https://logging.apache.org/log4j/2.x/) などのログ記録フレームワークに出力する必要がある場合は、[SLF4J のログ記録の実装](https://www.slf4j.org/manual.html)をバインドしてください。