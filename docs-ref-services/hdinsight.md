---
title: Azure HDInsight Java SDK
description: Azure HDInsight Java SDK のリファレンス。 HDInsight Java SDK に用意されているクラスとメソッドを使用すると、お使いの HDInsight クラスターを管理することができます。
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: reference
ms.devlang: java
ms.date: 9/20/2018
ms.openlocfilehash: 5e3887341ddb2fdcab336f0a8a232e6e8bfbe0f2
ms.sourcegitcommit: bb7286fad75a2bb43e6ce1a8f1b09e701147c9f9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48047159"
---
# <a name="hdinsight-java-management-sdk-preview"></a>HDInsight Java Management SDK (プレビュー)

## <a name="overview"></a>概要

HDInsight Java SDK に用意されているクラスとメソッドを使用すると、お使いの HDInsight クラスターを管理することができます。 これには、スクリプト アクションを作成、削除、更新、一覧表示、スケーリング、実行したり、HDInsight クラスターのプロパティを監視、取得したりする操作が含まれます。

## <a name="prerequisites"></a>前提条件

* Azure アカウント。 所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。
* [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Maven](https://maven.apache.org/install.html)

## <a name="sdk-installation"></a>SDK のインストール

HDInsight Java SDK は、[こちら](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight)の Maven から使用できます。 次の依存関係をご自身の pom.xml に追加します。

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

また、次の依存関係もご自身の pom.xml に追加する必要があります。

* [Azure クライアント認証ライブラリ:](https://mvnrepository.com/artifact/com.microsoft.azure/azure-client-authentication/1.6.2)
```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.2</version>
    <scope>test</scope>
</dependency>
```

* [ARM 用 Azure Java クライアント ランタイム:](https://mvnrepository.com/artifact/com.microsoft.azure/azure-arm-client-runtime/1.6.2)
```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.2</version>
</dependency>
```

## <a name="authentication"></a>Authentication

SDK は最初に Azure サブスクリプションで認証する必要があります。  以下の例に従って、サービス プリンシパルを作成し、これを使用して認証します。 その後、`HDInsightManagementClientImpl` のインスタンスが生成されます。これには、管理操作の実行に使用できるメソッドが多数含まれています (以下のセクションで説明します)。

> [!NOTE]
> 認証方法は以下の例の他にもあり、そちらの方がご自身のニーズに適している可能性もあります。 すべての方法については、[Java 用 Azure 管理ライブラリを使った認証](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)に関するページをご覧ください

### <a name="authentication-example-using-a-service-principal"></a>サービス プリンシパルを使用した認証の例

まず、[Azure Cloud Shell](https://shell.azure.com/bash) にログインします。 現在、サービス プリンシパル作成対象のサブスクリプションを使用していることを確認します。 

```azurecli-interactive
az account show
```

ご自身のサブスクリプション情報が JSON として表示されます。

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

正しいサブスクリプションにログインしていない場合は、以下を実行して正しいサブスクリプションを選択します。 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> 他の方法で、たとえば Azure portal で HDInsight クラスターを作成する、といった方法で HDInsight リソース プロバイダーをまだ登録していない場合は、認証の前にこれを一度実行する必要があります。 これを [Azure Cloud Shell](https://shell.azure.com/bash) から実行するには、次のコマンドを実行します。
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

次に、ご自身のサービス プリンシパルの名前を選択し、次のコマンドを使用して作成します。

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

サービス プリンシパル情報が JSON として表示されます。

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
次のスニペットをコピーし、コマンド実行後に返された JSON の文字列を `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET`、および `SUBSCRIPTION_ID` に入力して、サービス プリンシパルを作成します。

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

        HDInsightManagementClientImpl client = new HDInsightManagementClientImpl(credentials);
```


## <a name="cluster-management"></a>クラスターの管理

> [!NOTE]
> このセクションでは、`HDInsightManagementClientImpl` インスタンスの認証と構成が既に完了していること、および `client` と呼ばれる変数にそのインスタンスが格納されていることを前提としています。 `HDInsightManagementClientImpl` を認証および取得する方法については、上記の「認証」セクションを参照してください。

### <a name="create-a-cluster"></a>クラスターの作成

新しいクラスターを作成するには、`client.clusters().create()` を呼び出します。

#### <a name="example"></a>例

この例は、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターを作成する方法を示しています。

> [!NOTE]
> 次に示すように、最初にリソース グループとストレージ アカウントを作成する必要があります。 これらが既に作成済みの場合、この手順はスキップできます。

##### <a name="creating-a-resource-group"></a>リソース グループの作成

[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、リソース グループを作成できます
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>ストレージ アカウントの作成

[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、ストレージ アカウントを作成できます。
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
ここで、次のコマンドを実行して、ストレージ アカウントに対するキーを取得します (これはクラスターを作成するときに必要になります)。
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
以下の Java スニペットでは、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターが作成されます。 コメントの説明に従って空白の変数を入力します。また、ご自身のニーズに合わせて他のパラメーターを変更します。

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

### <a name="get-cluster-details"></a>クラスターの詳細の取得

特定のクラスターのプロパティを取得するには:

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>例

`get` を使用すると、ご自身のクラスターを適切に作成できたことを確認できます。

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

出力は次のようになります。

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a>クラスターの一覧表示

#### <a name="list-clusters-under-the-subscription"></a>サブスクリプションのクラスターの一覧表示

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a>リソース グループ別のクラスターの一覧表示

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> `List()` と `ListByResourceGroup()` の両方が `PagedList<ClusterInner>` オブジェクトを返します。 `loadNext()` を呼び出すと、そのページ上にクラスターの一覧が返され、`ClusterPaged` オブジェクトが次のページに進められます。 この処理は、`hasNextPage()` によって `false` が返されて、それ以上ページがないことが示されるまで繰り返されます。

#### <a name="example"></a>例

次の例では、現在のサブスクリプションのすべてのクラスターのプロパティが出力されます。

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

### <a name="delete-a-cluster"></a>クラスターの削除

クラスターを削除するには:

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a>クラスター タグの更新

次のように、特定のクラスターのタグを更新できます。

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="scale-cluster"></a>クラスターのスケーリング

特定のクラスターの worker ノード数を増減するには、次のように新しいサイズを指定します。

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a>クラスターの監視

HDInsight 管理 SDK を使用して、Operations Management Suite (OMS) でご自身のクラスターの監視を管理することもできます。

### <a name="enable-oms-monitoring"></a>OMS 監視の有効化

> [!NOTE]
> OMS の監視を有効にするには、既存の Log Analytics ワークスペースが必要です。 まだ作成していない場合、その方法については、「[Azure ポータルで Log Analytics ワークスペースを作成する](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)」を参照してください。

ご自身のクラスターで OMS 監視を有効にするには:

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a>OMS 監視の状態の表示

ご自身のクラスターに対する OMS の状態を取得するには:

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a>OMS 監視の無効化

ご自身のクラスターに対する OMS を無効にするには:

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a>[スクリプト操作]

HDInsight には、クラスターをカスタマイズするためにカスタム スクリプトを呼び出すスクリプト アクションという構成方法があります。
> [!NOTE]
> スクリプト アクションを使用する方法の詳細については、「[スクリプト アクションを使用して Linux ベースの HDInsight クラスターをカスタマイズする](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)」を参照してください

### <a name="execute-script-actions"></a>スクリプト アクションの実行

次のように、特定のクラスターに対してスクリプト アクションを実行できます。

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

### <a name="delete-script-action"></a>スクリプト アクションの削除

特定のクラスターに対して指定した保存済みスクリプト アクションを削除するには:

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a>保存済みスクリプト アクションの一覧表示

> [!NOTE]
> 両方の `listByCluster()` によって `PagedList<RuntimeScriptActionDetailInner>` オブジェクトが返されます。 `currentPage().items()` を呼び出すと `RuntimeScriptActionDetailInner` の一覧が返され、`loadNextPage()` は次のページに進みます。 この処理は、`hasNextPage()` によって `false` が返されて、それ以上ページがないことが示されるまで繰り返されます。

指定したクラスターに対する保存済みスクリプト アクションを一覧表示するには:
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>例

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

### <a name="list-all-scripts-execution-history"></a>スクリプトの全実行履歴の一覧表示

指定したクラスターに対するスクリプトの実行履歴をすべて一覧表示するには:

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a>例

この例では、過去のすべてのスクリプト実行の詳細が出力されます。

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
