---
title: "Java 用 Azure 管理ライブラリ開発者ガイド"
description: "Java 用管理ライブラリを使用して Azure でクラウド リソースを管理する際のパターンと概念。"
keywords: "Azure, Java, SDK, API, Maven, Gradle, 認証, active directory, サービス プリンシパル"
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
---
# <a name="patterns-and-best-practices-for-development-with-the-azure-libraries-for-java"></a><span data-ttu-id="fcc5b-104">Java 用 Azure ライブラリを使用した開発のパターンとベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="fcc5b-104">Patterns and best practices for development with the Azure libraries for Java</span></span> 

<span data-ttu-id="fcc5b-105">この記事では、プロジェクトで Java 用 Azure ライブラリを使用する際の一連のパターンとベスト プラクティスを示します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-105">This article lists a series of patterns and best practices when using the Azure libraries for Java in your projects.</span></span> <span data-ttu-id="fcc5b-106">これらのパターンやガイドラインに従って開発すると、管理するコードの量が減り、管理ライブラリの今後の更新時にリソースの追加や構成が容易になります。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-106">Develop with these patterns and guidelines to reduce the amount of code to maintain and make it easier to add or configure additional resources in future updates to the management libraries.</span></span>

## <a name="build-resources-through-a-fluent-interface"></a><span data-ttu-id="fcc5b-107">fluent インターフェイスを通じたリソースの作成</span><span class="sxs-lookup"><span data-stu-id="fcc5b-107">Build resources through a fluent interface</span></span>

<span data-ttu-id="fcc5b-108">fluent インターフェイスとは、オブジェクトの属性を正しく構成するメソッド チェーンを使ってオブジェクトを作成するパターンです。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-108">A fluent interface is a pattern that creates objects using a method chain that correctly configures the object's attributes.</span></span> <span data-ttu-id="fcc5b-109">たとえば新しい Azure ストレージ アカウントを作成するには、次のように記述します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-109">For example, to create a new Azure Storage account</span></span>

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

<span data-ttu-id="fcc5b-110">メソッド チェーンを記述する過程で、次に呼び出すメソッドの候補が、流暢な対話形式で IDE に表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-110">As you go through the method chain, your IDE suggests the next method to call in the fluent conversation.</span></span>   

![IntelliJ の fluent チェーンによるコマンド補完を示す GIF](media/intelliJFluent.gif)

<span data-ttu-id="fcc5b-112">定義しようとする Azure リソースに合ったメソッドを、IDE に表示される候補の中から選んでつなげていきます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-112">Chain the methods suggested by the IDE as long as they make sense for the Azure resource being defined.</span></span> <span data-ttu-id="fcc5b-113">必要なメソッドがチェーンに欠落している場合、その部分がエラーとして IDE によって強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-113">If you are missing a required method in the chain your IDE will highlight it with an error.</span></span>

## <a name="resource-collections"></a><span data-ttu-id="fcc5b-114">リソースのコレクション</span><span class="sxs-lookup"><span data-stu-id="fcc5b-114">Resource collections</span></span>

<span data-ttu-id="fcc5b-115">管理ライブラリは、最上位の `com.microsoft.azure.management.Azure` オブジェクトを通じてリソースを作成したり更新したりする際の、1 つの入口としての役割を果たします。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-115">The management library has a single point of entry through the top-level `com.microsoft.azure.management.Azure` object to create and update resources.</span></span> <span data-ttu-id="fcc5b-116">操作の対象となるリソースの種類は、`Azure` オブジェクトに定義されているリソース コレクションのメソッドを使って選択します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-116">Select which type of resources to work with using the resource collection methods defined in the `Azure` object.</span></span> <span data-ttu-id="fcc5b-117">たとえば SQL Database の場合は次のように記述します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-117">For example, SQL Database:</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a><span data-ttu-id="fcc5b-118">リストと反復</span><span class="sxs-lookup"><span data-stu-id="fcc5b-118">Lists and iterations</span></span>

<span data-ttu-id="fcc5b-119">それぞれのリソース コレクションには、そのリソースのすべてのインスタンスを現在のサブスクリプションから返す `list()` メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-119">Each resource collection has a `list()` method to return every instance of that resource in your current subscription.</span></span> <span data-ttu-id="fcc5b-120">たとえば、`azure.sqlServers().list()` とした場合、そのサブスクリプションに存在する SQL データベースがすべて返されます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-120">For example, `azure.sqlServers().list()` returns all SQL databases in the subscription.</span></span>

<span data-ttu-id="fcc5b-121">取得するリストの範囲を特定の [Azure リソース グループ](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)に限定するには、`listByResourceGroup(String groupname)` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-121">Use the `listByResourceGroup(String groupname)` method to scope the returned List to a specific [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).</span></span>  

<span data-ttu-id="fcc5b-122">返された `PagedList<T>` コレクションは、通常の `List<T>` とまったく同じように、検索したり反復処理を行ったりすることができます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-122">Search and iterate over the returned `PagedList<T>` collection just as you would a normal `List<T>`:</span></span>

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a><span data-ttu-id="fcc5b-123">クエリから返されるコレクション</span><span class="sxs-lookup"><span data-stu-id="fcc5b-123">Collections returned from queries</span></span>

<span data-ttu-id="fcc5b-124">管理ライブラリは、クエリ結果の構造に基づく特定のコレクション型を返します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-124">The management libraries return specific collection types from queries based on the structure of the results.</span></span>

- <span data-ttu-id="fcc5b-125">`List<T>`: 順序付けされていないデータで、検索と反復処理を簡単に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-125">`List<T>`: Unordered data that is easy to search and iterate over.</span></span>
- <span data-ttu-id="fcc5b-126">`Map<T>`: Map はキー/値のペアです。一意のキーを持ちますが、値が一意であるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-126">`Map<T>`: Maps are key/value pairs with unique keys, but not necessarily unique values.</span></span> <span data-ttu-id="fcc5b-127">Map の例としては、App Service Web Apps のアプリケーション設定があります。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-127">An example of a Map would be app settings for an App Service Web App.</span></span>
- <span data-ttu-id="fcc5b-128">`Set<T>`: Set は、一意のキーと値を持ちます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-128">`Set<T>`: Sets have unique keys and values.</span></span> <span data-ttu-id="fcc5b-129">たとえば仮想マシンにアタッチされたネットワークは Set であり、一意の識別子 (キー) と一意のネットワーク構成 (値) の両方を持ちます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-129">An example of a Set would be networks attached to a virtual machine, which would have both an unique identifier (the key) and a unique network configuration (the value).</span></span>

## <a name="actionable-verbs"></a><span data-ttu-id="fcc5b-130">アクション可能な動詞</span><span class="sxs-lookup"><span data-stu-id="fcc5b-130">Actionable verbs</span></span>

<span data-ttu-id="fcc5b-131">名前に動詞を含んだメソッドを実行すると、Azure で何らかのアクションが直ちに実行されます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-131">Methods with verbs in their names take immediate action in Azure.</span></span> <span data-ttu-id="fcc5b-132">これらのメソッドは同期的に動作するため、現在のスレッドで実行されている処理は、メソッドが完了するまでブロックされます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-132">These methods work synchronously and block execution in the current thread until they complete.</span></span> 

| <span data-ttu-id="fcc5b-133">動詞</span><span class="sxs-lookup"><span data-stu-id="fcc5b-133">Verb</span></span>   |  <span data-ttu-id="fcc5b-134">使用例</span><span class="sxs-lookup"><span data-stu-id="fcc5b-134">Sample Usage</span></span> |
|--------|---------------|
| <span data-ttu-id="fcc5b-135">create</span><span class="sxs-lookup"><span data-stu-id="fcc5b-135">create</span></span> | `azure.virtualMachines().create(listOfVMCreatables)` |
| <span data-ttu-id="fcc5b-136">apply</span><span class="sxs-lookup"><span data-stu-id="fcc5b-136">apply</span></span>  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| <span data-ttu-id="fcc5b-137">削除</span><span class="sxs-lookup"><span data-stu-id="fcc5b-137">delete</span></span> | `azure.disks().deleteById(id)` | 
| <span data-ttu-id="fcc5b-138">list</span><span class="sxs-lookup"><span data-stu-id="fcc5b-138">list</span></span>   | `azure.sqlServers().list()` | 
| <span data-ttu-id="fcc5b-139">get</span><span class="sxs-lookup"><span data-stu-id="fcc5b-139">get</span></span>    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> <span data-ttu-id="fcc5b-140">`define()` と `update()` は動詞ですが、その後で `create()` または `apply()` が続かない限り、他の処理がブロックされることはありません。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-140">`define()` and `update()` are verbs but do not block unless followed by a `create()` or `apply()`.</span></span>
 
<span data-ttu-id="fcc5b-141">いくつかのメソッドには、[Reactive 拡張機能](https://github.com/ReactiveX/RxJava)を使った非同期版が存在し、`Async` というサフィックスが付きます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-141">Asynchronous versions of some of these  methods exist with the `Async` suffix using [Reactive extensions](https://github.com/ReactiveX/RxJava).</span></span> 

<span data-ttu-id="fcc5b-142">一部のオブジェクトには、他にも、Azure のリソースの状態を変更するメソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-142">Some objects have other methods with that change the state of the resource in Azure.</span></span> <span data-ttu-id="fcc5b-143">たとえば `VirtualMachine` の `restart()` があります。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-143">For example, `restart()` on a `VirtualMachine`.</span></span>

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
<span data-ttu-id="fcc5b-144">これらのメソッドには必ずしも非同期版があるとは限らず、同じスレッド上で実行されている処理は、メソッドの完了までブロックされます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-144">These methods do not always have asynchronous versions and will block execution on their thread until they complete.</span></span>

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a><span data-ttu-id="fcc5b-145">リソースの遅延作成</span><span class="sxs-lookup"><span data-stu-id="fcc5b-145">Lazy resource creation</span></span>

<span data-ttu-id="fcc5b-146">Azure リソースを作成するとき、まだ存在しない別のリソースに新しいリソースが依存していると、作成には困難が伴います。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-146">A challenge when creating Azure resources arises when a new resource depends on another resource that doesn't yet exist.</span></span> <span data-ttu-id="fcc5b-147">たとえば、新しい仮想マシンを作成するときに、パブリック IP アドレスを予約したり、ディスクを設定したりする場合があります。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-147">An example of this scenario is reserving a public IP address and setting up a disk when creating a new virtual machine.</span></span> <span data-ttu-id="fcc5b-148">開発者が求めているのは、仮想マシンを作成するときに、これらのリソースが確実に割り当てられるようにすることです。アドレスが正しく予約されたかどうかやディスクが正しく作成されたかどうかを確かめることではありません。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-148">You don't want to verify reserving the address or the creating the disk, you just want to ensure the virtual machine has those resources when it is created.</span></span>

<span data-ttu-id="fcc5b-149">`Creatable<T>` オブジェクトを使用すると、コード内で使用する Azure リソースを事前に、つまり実際のサブスクリプションで作成されるのを待たずに定義することができます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-149">`Creatable<T>` objects let you define Azure resources for use in your code without waiting around for them to be created in your subscription.</span></span> <span data-ttu-id="fcc5b-150">`Creatable<T>` オブジェクトの作成は、実際に必要になるまで、管理ライブラリによって先延ばしされます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-150">The management libraries defer creating  `Creatable<T>` objects until they are needed.</span></span>

<span data-ttu-id="fcc5b-151">Azure リソースの `Creatable<T>` オブジェクトは、`define()` 動詞を使って作成します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-151">Generate `Creatable<T>` objects for Azure resources through the `define()` verb:</span></span>

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

<span data-ttu-id="fcc5b-152">この例の `Creatable<PublicIPAddress>` によって定義される Azure リソースは、このコードが実行される時点では、サブスクリプションに存在しません。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-152">The Azure resource defined by the `Creatable<PublicIPAddress>` in this example does not yet exist in your subscription when this code executes.</span></span>  <span data-ttu-id="fcc5b-153">この IP アドレスを使って他の Azure リソースを作成するには、`publicIPAddressCreatable` オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-153">Use the `publicIPAddressCreatable` object to create other Azure resources with this IP address.</span></span> 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

<span data-ttu-id="fcc5b-154">`Creatable<T>` のリソースがサブスクリプションに作成されるのは、そのオブジェクトを使って定義されたリソースが、`create()` を使って Azure に作成されたときです。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-154">The `Creatable<T>` resources are generated in your subscription when any resource that is defined using the object is built in Azure using `create()`.</span></span> <span data-ttu-id="fcc5b-155">引き続き IP アドレスと仮想マシンの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-155">Continuing the IP address and virtual machine example:</span></span>

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

<span data-ttu-id="fcc5b-156">`create()` の呼び出しに `Creatable<T>` を渡すと、単一のリソース オブジェクトではなく `CreatedResources` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-156">Passing the `Creatable<T>` to `create()` calls returns a `CreatedResources` object instead of a single resource object.</span></span>  <span data-ttu-id="fcc5b-157">`CreatedResources<T>` オブジェクトを使用すると、`create()` の呼び出しで作成された (呼び出しの中で型指定したリソースだけでなく) すべてのリソースにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-157">The `CreatedResources<T>` object lets you access all resources created by the `create()` call, not just the typed resource in the call.</span></span> <span data-ttu-id="fcc5b-158">上の例で作成した仮想マシンに関して Azure に作成されたパブリック IP アドレスには、次のようにしてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-158">To access the public IP address created in Azure for the virtual machine created in the above example:</span></span>

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a><span data-ttu-id="fcc5b-159">例外処理</span><span class="sxs-lookup"><span data-stu-id="fcc5b-159">Exception handling</span></span>

<span data-ttu-id="fcc5b-160">管理ライブラリの Exception クラスは `com.microsoft.rest.RestException` を拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-160">The management libraries' Exception classes extend `com.microsoft.rest.RestException`.</span></span> <span data-ttu-id="fcc5b-161">管理ライブラリによって生成された例外は、対応する `try` ステートメントに続く `catch (RestException exception)` ブロックでキャッチします。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-161">Catch exceptions generated by the management libraries with a `catch (RestException exception)` block after the relevant `try` statement.</span></span>

## <a name="logs-and-trace"></a><span data-ttu-id="fcc5b-162">ログとトレース</span><span class="sxs-lookup"><span data-stu-id="fcc5b-162">Logs and trace</span></span>

<span data-ttu-id="fcc5b-163">エントリ ポイントとなる `Azure` オブジェクトを作成するときに管理ライブラリから出力されるログの量は、`withLogLevel()` を使って構成することができます。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-163">Configure the amount of logging from the management library when you build the entry point `Azure` object using `withLogLevel()`.</span></span> <span data-ttu-id="fcc5b-164">次のトレース レベルが存在します。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-164">The following trace levels exist:</span></span>

| <span data-ttu-id="fcc5b-165">トレース レベル</span><span class="sxs-lookup"><span data-stu-id="fcc5b-165">Trace level</span></span> | <span data-ttu-id="fcc5b-166">有効になるログ</span><span class="sxs-lookup"><span data-stu-id="fcc5b-166">Logging enabled</span></span> 
| ------------ | ---------------
| <span data-ttu-id="fcc5b-167">com.microsoft.rest.LogLevel.NONE</span><span class="sxs-lookup"><span data-stu-id="fcc5b-167">com.microsoft.rest.LogLevel.NONE</span></span> | <span data-ttu-id="fcc5b-168">出力なし</span><span class="sxs-lookup"><span data-stu-id="fcc5b-168">No output</span></span>
| <span data-ttu-id="fcc5b-169">com.microsoft.rest.LogLevel.BASIC</span><span class="sxs-lookup"><span data-stu-id="fcc5b-169">com.microsoft.rest.LogLevel.BASIC</span></span> | <span data-ttu-id="fcc5b-170">基になる REST 呼び出しの URL、応答コード、時間がログに記録されます</span><span class="sxs-lookup"><span data-stu-id="fcc5b-170">Logs the URLs to underlying REST calls, response codes and times</span></span>
| <span data-ttu-id="fcc5b-171">com.microsoft.rest.LogLevel.BODY</span><span class="sxs-lookup"><span data-stu-id="fcc5b-171">com.microsoft.rest.LogLevel.BODY</span></span> | <span data-ttu-id="fcc5b-172">BASIC の全出力内容に加えて、REST 呼び出しの要求と応答の本文がログに記録されます</span><span class="sxs-lookup"><span data-stu-id="fcc5b-172">Everything in BASIC plus request and response bodies for the REST calls</span></span>
| <span data-ttu-id="fcc5b-173">com.microsoft.rest.LogLevel.HEADERS</span><span class="sxs-lookup"><span data-stu-id="fcc5b-173">com.microsoft.rest.LogLevel.HEADERS</span></span> | <span data-ttu-id="fcc5b-174">BASIC の全出力内容に加えて、REST 呼び出しの要求と応答のヘッダーがログに記録されます</span><span class="sxs-lookup"><span data-stu-id="fcc5b-174">Everything in BASIC plus the request and response headers REST calls</span></span>
| <span data-ttu-id="fcc5b-175">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span><span class="sxs-lookup"><span data-stu-id="fcc5b-175">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span></span> | <span data-ttu-id="fcc5b-176">BODY と HEADERS の両ログ レベルの全出力内容が記録されます</span><span class="sxs-lookup"><span data-stu-id="fcc5b-176">Everything in both BODY and HEADERS log level</span></span>

<span data-ttu-id="fcc5b-177">[Log4J 2](https://logging.apache.org/log4j/2.x/) などのログ記録フレームワークに出力する必要がある場合は、[SLF4J のログ記録の実装](https://www.slf4j.org/manual.html)をバインドしてください。</span><span class="sxs-lookup"><span data-stu-id="fcc5b-177">Bind a [SLF4J logging implementation](https://www.slf4j.org/manual.html) if you need to log output to a logging framework like [Log4J 2](https://logging.apache.org/log4j/2.x/).</span></span>