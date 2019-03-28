---
title: Azure HDInsight Java SDK
description: Azure HDInsight Java SDK のリファレンス。 HDInsight Java SDK に用意されているクラスとメソッドを使用すると、お使いの HDInsight クラスターを管理することができます。
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: reference
ms.devlang: java
ms.date: 11/21/2018
ms.openlocfilehash: 0ae8d78a0618c4dbcc5e734fce311f7c2e5684bd
ms.sourcegitcommit: a108a82414bd35be896e3c4e7047f5eb7b1518cb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489650"
---
# <a name="hdinsight-java-management-sdk-preview"></a><span data-ttu-id="731f4-104">HDInsight Java Management SDK (プレビュー)</span><span class="sxs-lookup"><span data-stu-id="731f4-104">HDInsight Java Management SDK (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="731f4-105">概要</span><span class="sxs-lookup"><span data-stu-id="731f4-105">Overview</span></span>

<span data-ttu-id="731f4-106">HDInsight Java SDK に用意されているクラスとメソッドを使用すると、お使いの HDInsight クラスターを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="731f4-106">The HDInsight Java SDK provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="731f4-107">これには、スクリプト アクションを作成、削除、更新、一覧表示、サイズ変更、実行したり、HDInsight クラスターのプロパティを監視、取得したりする操作が含まれます。</span><span class="sxs-lookup"><span data-stu-id="731f4-107">It includes operations to create, delete, update, list, resize, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="731f4-108">前提条件</span><span class="sxs-lookup"><span data-stu-id="731f4-108">Prerequisites</span></span>

* <span data-ttu-id="731f4-109">Azure アカウント。</span><span class="sxs-lookup"><span data-stu-id="731f4-109">An Azure account.</span></span> <span data-ttu-id="731f4-110">所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。</span><span class="sxs-lookup"><span data-stu-id="731f4-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="731f4-111">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="731f4-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="731f4-112">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="731f4-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* [<span data-ttu-id="731f4-113">Maven</span><span class="sxs-lookup"><span data-stu-id="731f4-113">Maven</span></span>](https://maven.apache.org/download.cgi)

## <a name="sdk-installation"></a><span data-ttu-id="731f4-114">SDK のインストール</span><span class="sxs-lookup"><span data-stu-id="731f4-114">SDK Installation</span></span>

<span data-ttu-id="731f4-115">HDInsight Java SDK は、[こちら](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight)の Maven から使用できます。</span><span class="sxs-lookup"><span data-stu-id="731f4-115">The HDInsight Java SDK is available through Maven [here](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight).</span></span> <span data-ttu-id="731f4-116">次の依存関係をご自身の pom.xml に追加します。</span><span class="sxs-lookup"><span data-stu-id="731f4-116">Add the following dependency to your pom.xml:</span></span>

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

<span data-ttu-id="731f4-117">また、次の依存関係もご自身の pom.xml に追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="731f4-117">You will also need to add the following dependencies to your pom.xml:</span></span>

* [<span data-ttu-id="731f4-118">Azure クライアント認証ライブラリ:</span><span class="sxs-lookup"><span data-stu-id="731f4-118">Azure Client Authentication Library:</span></span>](https://mvnrepository.com/artifact/com.microsoft.azure/azure-client-authentication/1.6.2)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.2</version>
  </dependency>
  ```

* [<span data-ttu-id="731f4-119">ARM 用 Azure Java クライアント ランタイム:</span><span class="sxs-lookup"><span data-stu-id="731f4-119">Azure Java Client Runtime For ARM:</span></span>](https://mvnrepository.com/artifact/com.microsoft.azure/azure-arm-client-runtime/1.6.2)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.2</version>
  </dependency>
  ```

## <a name="authentication"></a><span data-ttu-id="731f4-120">Authentication</span><span class="sxs-lookup"><span data-stu-id="731f4-120">Authentication</span></span>

<span data-ttu-id="731f4-121">SDK は最初に Azure サブスクリプションで認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="731f4-121">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="731f4-122">以下の例に従って、サービス プリンシパルを作成し、これを使用して認証します。</span><span class="sxs-lookup"><span data-stu-id="731f4-122">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="731f4-123">その後、`HDInsightManagementClientImpl` のインスタンスが生成されます。これには、管理操作の実行に使用できるメソッドが多数含まれています (以下のセクションで説明します)。</span><span class="sxs-lookup"><span data-stu-id="731f4-123">After this is done, you will have an instance of an `HDInsightManagementClientImpl`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="731f4-124">認証方法は以下の例の他にもあり、そちらの方がご自身のニーズに適している可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="731f4-124">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="731f4-125">すべてのメソッドの概要については、[Azure Management Libraries for Java を使って認証する](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span><span class="sxs-lookup"><span data-stu-id="731f4-125">All methods are outlined here: [Authenticate with the Azure management libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="731f4-126">サービス プリンシパルを使用した認証の例</span><span class="sxs-lookup"><span data-stu-id="731f4-126">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="731f4-127">まず、[Azure Cloud Shell](https://shell.azure.com/bash) にログインします。</span><span class="sxs-lookup"><span data-stu-id="731f4-127">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="731f4-128">現在、サービス プリンシパル作成対象のサブスクリプションを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="731f4-128">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="731f4-129">ご自身のサブスクリプション情報が JSON として表示されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-129">Your subscription information is displayed as JSON.</span></span>

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

<span data-ttu-id="731f4-130">正しいサブスクリプションにログインしていない場合は、以下を実行して正しいサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="731f4-130">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="731f4-131">他の方法で、たとえば Azure portal で HDInsight クラスターを作成する、といった方法で HDInsight リソース プロバイダーをまだ登録していない場合は、認証の前にこれを一度実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="731f4-131">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="731f4-132">これを [Azure Cloud Shell](https://shell.azure.com/bash) から実行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="731f4-132">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="731f4-133">次に、ご自身のサービス プリンシパルの名前を選択し、次のコマンドを使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="731f4-133">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="731f4-134">サービス プリンシパル情報が JSON として表示されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-134">The service principal information is displayed as JSON.</span></span>

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
<span data-ttu-id="731f4-135">次のスニペットをコピーし、コマンド実行後に返された JSON の文字列を `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET`、および `SUBSCRIPTION_ID` に入力して、サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="731f4-135">Copy the below snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

```java
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.*;
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.HDInsightManagementClientImpl;

public class Main {
    public static void main (String[] args) {

        // Tenant ID for your Azure Subscription
        String TENANT_ID = "";
        // Your Service Principal App Client ID
        String CLIENT_ID = "";
        // Your Service Principal Client Secret
        String CLIENT_SECRET = "";
        // Azure Subscription ID
        String SUBSCRIPTION_ID = "";

        ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(
                CLIENT_ID,
                TENANT_ID,
                CLIENT_SECRET,
                AzureEnvironment.AZURE);

        HDInsightManagementClientImpl client = new HDInsightManagementClientImpl(credentials)
                .withSubscriptionId(SUBSCRIPTION_ID);
```


## <a name="cluster-management"></a><span data-ttu-id="731f4-136">クラスターの管理</span><span class="sxs-lookup"><span data-stu-id="731f4-136">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="731f4-137">このセクションでは、`HDInsightManagementClientImpl` インスタンスの認証と構成が既に完了していること、および `client` と呼ばれる変数にそのインスタンスが格納されていることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="731f4-137">This section assumes you have already authenticated and constructed an `HDInsightManagementClientImpl` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="731f4-138">`HDInsightManagementClientImpl` を認証および取得する方法については、上記の「認証」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="731f4-138">Instructions for authenticating and obtaining an `HDInsightManagementClientImpl` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="731f4-139">クラスターの作成</span><span class="sxs-lookup"><span data-stu-id="731f4-139">Create a Cluster</span></span>

<span data-ttu-id="731f4-140">新しいクラスターを作成するには、`client.clusters().create()` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="731f4-140">A new cluster can be created by calling `client.clusters().create()`.</span></span>

#### <a name="example"></a><span data-ttu-id="731f4-141">例</span><span class="sxs-lookup"><span data-stu-id="731f4-141">Example</span></span>

<span data-ttu-id="731f4-142">この例は、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターを作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="731f4-142">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="731f4-143">次に示すように、最初にリソース グループとストレージ アカウントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="731f4-143">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="731f4-144">これらが既に作成済みの場合、この手順はスキップできます。</span><span class="sxs-lookup"><span data-stu-id="731f4-144">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="731f4-145">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="731f4-145">Creating a Resource Group</span></span>

<span data-ttu-id="731f4-146">[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、リソース グループを作成できます</span><span class="sxs-lookup"><span data-stu-id="731f4-146">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="731f4-147">ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="731f4-147">Creating a Storage Account</span></span>

<span data-ttu-id="731f4-148">[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、ストレージ アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="731f4-148">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="731f4-149">ここで、次のコマンドを実行して、ストレージ アカウントに対するキーを取得します (これはクラスターを作成するときに必要になります)。</span><span class="sxs-lookup"><span data-stu-id="731f4-149">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="731f4-150">以下の Java スニペットでは、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターが作成されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-150">The below Java snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="731f4-151">コメントの説明に従って空白の変数を入力します。また、ご自身のニーズに合わせて他のパラメーターを変更します。</span><span class="sxs-lookup"><span data-stu-id="731f4-151">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

```java
// The name for the cluster you are creating
String clusterName = "";
// The name of your existing Resource Group
String resourceGroupName = "";
// Choose a username
String username = "";
// Choose a password
String password = "";
// Replace <> with the name of your storage account
String storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
String storageAccountKey = "";
// Choose a region
String location = "";
String container = "default";

HashMap<String, HashMap<String, String>> configurations = new HashMap<String, HashMap<String, String>>();
        HashMap<String, String> gateway = new HashMap<String, String>();
        gateway.put("restAuthCredential.enabled_credential", "True");
        gateway.put("restAuthCredential.username", username);
        gateway.put("restAuthCredential.password", password);
        configurations.put("gateway", gateway);

        ClusterCreateParametersExtended parameters = new ClusterCreateParametersExtended()
            .withLocation(location)
            .withTags(Collections.EMPTY_MAP)
            .withProperties(
                new ClusterCreateProperties()
                    .withClusterVersion("3.6")
                    .withOsType(OSType.LINUX)
                    .withClusterDefinition(new ClusterDefinition()
                            .withKind("spark")
                            .withConfigurations(configurations)
                    )
                    .withTier(Tier.STANDARD)
                    .withComputeProfile(new ComputeProfile()
                        .withRoles(List.of(
                            new Role()
                                .withName("headnode")
                                .withTargetInstanceCount(2)
                                .withHardwareProfile(new HardwareProfile()
                                    .withVmSize("Large")
                                )
                                .withOsProfile(new OsProfile()
                                    .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                    )
                                ),
                            new Role()
                                    .withName("workernode")
                                    .withTargetInstanceCount(1)
                                    .withHardwareProfile(new HardwareProfile()
                                        .withVmSize("Large")
                                    )
                                    .withOsProfile(new OsProfile()
                                        .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                        )
                                    )
                        ))
                    )
                    .withStorageProfile(new StorageProfile()
                        .withStorageaccounts(List.of(
                                new StorageAccount()
                                    .withName(storageAccount)
                                    .withKey(storageAccountKey)
                                    .withContainer(container)
                                    .withIsDefault(true)
                        ))
                    )
            );
        client.clusters().create(resourceGroupName, clusterName, parameters);
```

### <a name="get-cluster-details"></a><span data-ttu-id="731f4-152">クラスターの詳細の取得</span><span class="sxs-lookup"><span data-stu-id="731f4-152">Get Cluster Details</span></span>

<span data-ttu-id="731f4-153">特定のクラスターのプロパティを取得するには:</span><span class="sxs-lookup"><span data-stu-id="731f4-153">To get properties for a given cluster:</span></span>

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="731f4-154">例</span><span class="sxs-lookup"><span data-stu-id="731f4-154">Example</span></span>

<span data-ttu-id="731f4-155">`get` を使用すると、ご自身のクラスターを適切に作成できたことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="731f4-155">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

<span data-ttu-id="731f4-156">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="731f4-156">The output should look like:</span></span>

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a><span data-ttu-id="731f4-157">クラスターの一覧表示</span><span class="sxs-lookup"><span data-stu-id="731f4-157">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="731f4-158">サブスクリプションのクラスターの一覧表示</span><span class="sxs-lookup"><span data-stu-id="731f4-158">List Clusters Under The Subscription</span></span>

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="731f4-159">リソース グループ別のクラスターの一覧表示</span><span class="sxs-lookup"><span data-stu-id="731f4-159">List Clusters By Resource Group</span></span>

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> <span data-ttu-id="731f4-160">`List()` と `ListByResourceGroup()` の両方が `PagedList<ClusterInner>` オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="731f4-160">Both `List()` and `ListByResourceGroup()` return a `PagedList<ClusterInner>` object.</span></span> <span data-ttu-id="731f4-161">`loadNext()` を呼び出すと、そのページ上にクラスターの一覧が返され、`ClusterPaged` オブジェクトが次のページに進められます。</span><span class="sxs-lookup"><span data-stu-id="731f4-161">Calling `loadNext()` returns a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="731f4-162">この処理は、`hasNextPage()` によって `false` が返されて、それ以上ページがないことが示されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-162">This can be repeated until `hasNextPage()` return `false`, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="731f4-163">例</span><span class="sxs-lookup"><span data-stu-id="731f4-163">Example</span></span>

<span data-ttu-id="731f4-164">次の例では、現在のサブスクリプションのすべてのクラスターのプロパティが出力されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-164">The following example prints the properties of all clusters for the current subscription:</span></span>

```java
PagedList<ClusterInner> clusterPages = client.clusters().list();
while (true) {
    for (ClusterInner cluster : clusterPages.currentPage().items()) {
        System.out.println(cluster.name());
    }
    if (clusterPages.hasNextPage()) {
        clusterPages.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="delete-a-cluster"></a><span data-ttu-id="731f4-165">クラスターの削除</span><span class="sxs-lookup"><span data-stu-id="731f4-165">Delete a Cluster</span></span>

<span data-ttu-id="731f4-166">クラスターを削除するには:</span><span class="sxs-lookup"><span data-stu-id="731f4-166">To delete a cluster:</span></span>

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a><span data-ttu-id="731f4-167">クラスター タグの更新</span><span class="sxs-lookup"><span data-stu-id="731f4-167">Update Cluster Tags</span></span>

<span data-ttu-id="731f4-168">次のように、特定のクラスターのタグを更新できます。</span><span class="sxs-lookup"><span data-stu-id="731f4-168">You can update the tags of a given cluster like so:</span></span>

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="resize-cluster"></a><span data-ttu-id="731f4-169">クラスターのサイズ変更</span><span class="sxs-lookup"><span data-stu-id="731f4-169">Resize Cluster</span></span>

<span data-ttu-id="731f4-170">特定のクラスターの worker ノード数をサイズ変更するには、次のように新しいサイズを指定します。</span><span class="sxs-lookup"><span data-stu-id="731f4-170">You can resize a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="731f4-171">クラスターの監視</span><span class="sxs-lookup"><span data-stu-id="731f4-171">Cluster Monitoring</span></span>

<span data-ttu-id="731f4-172">HDInsight 管理 SDK を使用して、Operations Management Suite (OMS) でご自身のクラスターの監視を管理することもできます。</span><span class="sxs-lookup"><span data-stu-id="731f4-172">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="731f4-173">OMS 監視の有効化</span><span class="sxs-lookup"><span data-stu-id="731f4-173">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="731f4-174">OMS の監視を有効にするには、既存の Log Analytics ワークスペースが必要です。</span><span class="sxs-lookup"><span data-stu-id="731f4-174">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="731f4-175">まだ作成していない場合、その方法については、「[Azure portal で Log Analytics ワークスペースを作成する](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="731f4-175">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="731f4-176">ご自身のクラスターで OMS 監視を有効にするには:</span><span class="sxs-lookup"><span data-stu-id="731f4-176">To enable OMS Monitoring on your cluster:</span></span>

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="731f4-177">OMS 監視の状態の表示</span><span class="sxs-lookup"><span data-stu-id="731f4-177">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="731f4-178">ご自身のクラスターに対する OMS の状態を取得するには:</span><span class="sxs-lookup"><span data-stu-id="731f4-178">To get the status of OMS on your cluster:</span></span>

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="731f4-179">OMS 監視の無効化</span><span class="sxs-lookup"><span data-stu-id="731f4-179">Disable OMS Monitoring</span></span>

<span data-ttu-id="731f4-180">ご自身のクラスターに対する OMS を無効にするには:</span><span class="sxs-lookup"><span data-stu-id="731f4-180">To disable OMS on your cluster:</span></span>

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a><span data-ttu-id="731f4-181">[スクリプト操作]</span><span class="sxs-lookup"><span data-stu-id="731f4-181">Script Actions</span></span>

<span data-ttu-id="731f4-182">HDInsight には、クラスターをカスタマイズするためにカスタム スクリプトを呼び出すスクリプト アクションという構成方法があります。</span><span class="sxs-lookup"><span data-stu-id="731f4-182">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="731f4-183">スクリプト アクションの使用方法の詳細については、「[スクリプト アクションを使用して Linux ベースの HDInsight クラスターをカスタマイズする](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)」を参照してください</span><span class="sxs-lookup"><span data-stu-id="731f4-183">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="731f4-184">スクリプト アクションの実行</span><span class="sxs-lookup"><span data-stu-id="731f4-184">Execute Script Actions</span></span>

<span data-ttu-id="731f4-185">次のように、特定のクラスターに対してスクリプト アクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="731f4-185">You can execute script actions on a given cluster like so:</span></span>

```java
RuntimeScriptAction scriptAction1 = new RuntimeScriptAction()
    .withName("<Script Name>")
    .withUri("<URL To Script>")
    .withRoles(<List<String> of roles>);
client.clusters().executeScriptActions(
    resourceGroupName, 
    clusterName, 
    new ExecuteScriptActionParameters().withPersistOnSuccess(false).withScriptActions(new LinkedList<>(Arrays.asList(scriptAction1)))); //add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a><span data-ttu-id="731f4-186">スクリプト アクションの削除</span><span class="sxs-lookup"><span data-stu-id="731f4-186">Delete Script Action</span></span>

<span data-ttu-id="731f4-187">特定のクラスターに対して指定した保存済みスクリプト アクションを削除するには:</span><span class="sxs-lookup"><span data-stu-id="731f4-187">To delete a specified persisted script action on a given cluster:</span></span>

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="731f4-188">保存済みスクリプト アクションの一覧表示</span><span class="sxs-lookup"><span data-stu-id="731f4-188">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="731f4-189">両方の `listByCluster()` によって `PagedList<RuntimeScriptActionDetailInner>` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-189">Both `listByCluster()` returns a `PagedList<RuntimeScriptActionDetailInner>` object.</span></span> <span data-ttu-id="731f4-190">`currentPage().items()` を呼び出すと `RuntimeScriptActionDetailInner` の一覧が返され、`loadNextPage()` は次のページに進みます。</span><span class="sxs-lookup"><span data-stu-id="731f4-190">Calling `currentPage().items()` returns a list of `RuntimeScriptActionDetailInner`, and `loadNextPage()` advances to the next page.</span></span> <span data-ttu-id="731f4-191">この処理は、`hasNextPage()` によって `false` が返されて、それ以上ページがないことが示されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-191">This can be repeated until `hasNextPage()` returns `false`, indicating that there are no more pages.</span></span>

<span data-ttu-id="731f4-192">指定したクラスターに対する保存済みスクリプト アクションを一覧表示するには:</span><span class="sxs-lookup"><span data-stu-id="731f4-192">To list all persisted script actions for the specified cluster:</span></span>
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="731f4-193">例</span><span class="sxs-lookup"><span data-stu-id="731f4-193">Example</span></span>

```java
PagedList<RuntimeScriptActionDetailInner> scriptsPaged = client.scriptActions().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptsPaged.hasNextPage()) {
        scriptsPaged.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="731f4-194">スクリプトの全実行履歴の一覧表示</span><span class="sxs-lookup"><span data-stu-id="731f4-194">List All Scripts' Execution History</span></span>

<span data-ttu-id="731f4-195">指定したクラスターに対するスクリプトの実行履歴をすべて一覧表示するには:</span><span class="sxs-lookup"><span data-stu-id="731f4-195">To list all scripts' execution history for the specified cluster:</span></span>

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="731f4-196">例</span><span class="sxs-lookup"><span data-stu-id="731f4-196">Example</span></span>

<span data-ttu-id="731f4-197">この例では、過去のすべてのスクリプト実行の詳細が出力されます。</span><span class="sxs-lookup"><span data-stu-id="731f4-197">This example prints all the details for all past script executions.</span></span>

```java
PagedList<RuntimeScriptActionDetailInner> scriptExecutionsPaged = client.scriptExecutionHistorys().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptExecutionsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptExecutionsPaged.hasNextPage()) {
        scriptExecutionsPaged.loadNextPage();
    } else {
        break;
    }
}
```
