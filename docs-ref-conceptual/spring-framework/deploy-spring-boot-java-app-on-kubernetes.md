---
title: Azure Kubernetes Service で Spring Boot アプリを Kubernetes にデプロイする
description: このチュートリアルでは、Microsoft Azure の Kubernetes クラスターに Spring Boot アプリケーションをデプロイする方法について説明します。
services: container-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 42bb030a916cc5aaf1e20242518a0a400b8baa88
ms.sourcegitcommit: f33befab25a66a252b4c91c7aeb1b77cb32821bb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "59745160"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a>Azure Kubernetes Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする

**[Kubernetes]** と **[Docker]** は、開発者が、コンテナーで実行されるアプリケーションのデプロイ、スケーリング、管理を自動化することを支援するオープン ソース ソリューションです。

このチュートリアルでは、この 2 つの一般的なオープンソース テクノロジを組み合わせて Spring Boot アプリケーションを開発し、Microsoft Azure にデプロイする方法について説明します。 具体的には、アプリケーション開発に *[Spring Boot]* を、コンテナーのデプロイに *[Kubernetes]* を、アプリケーションのホストとして [Azure Kubernetes Service (AKS)] をそれぞれ使用します。

### <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、[無料の Azure アカウント]にサインアップできます。
* [Azure コマンド ライン インターフェイス (CLI)]。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* Apache の [Maven] 構築ツール (バージョン 3)。
* [Git] クライアント。
* [Docker] クライアント。

> [!NOTE]
>
> このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Spring Boot on Docker Getting Started Web アプリを作成する

次の手順で、Spring Boot Web アプリケーションをビルドしてローカルでテストします。

1. コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成し、そのディレクトリに変更します。次に例を示します。
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- または --
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. [Docker での Spring Boot の使用開始] サンプル プロジェクトを、ディレクトリに複製します。
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 完成したプロジェクトにディレクトリを変更します。
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Maven を使用してサンプル アプリをビルドして実行します。
   ```
   mvn package spring-boot:run
   ```

1. Web アプリをテストするには、 `http://localhost:8080` を参照するか、次の `curl` コマンドを使用します。
   ```
   curl http://localhost:8080
   ```

1. 次のメッセージが表示されます。**Hello Docker World**

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Azure CLI を使用して Azure Container Registry を作成する

1. コマンド プロンプトを開きます。

1. Azure アカウントにログインします。
   ```azurecli
   az login
   ```

1. Azure サブスクリプションを選択します。
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. このチュートリアルで使用する Azure リソースのリソース グループを作成します。
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. リソース グループ内に、プライベートな Azure コンテナー レジストリを作成します。 このチュートリアルでは、後の手順で、このレジストリに Docker イメージとしてサンプル アプリをプッシュします。 `wingtiptoysregistry` を、レジストリの一意の名前に置き換えます。
   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>Jib を使用してアプリをコンテナー レジストリにプッシュする

1. Azure CLI から Azure Container Registry にログインします。
   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。

1. *pom.xml* ファイル内の `<properties>` コレクションを、Azure Container Registry のレジストリ名と [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) の最新バージョンで更新します。

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.0.2</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. *pom.xml* ファイルの `<plugins>` コレクションを、`jib-maven-plugin` に `<plugin>` が含まれるように更新します。

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>              
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>                
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行してイメージを作成し、そのイメージをレジストリにプッシュします。

   ```
   mvn compile jib:build
   ```

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a>Azure CLI を使用して AKS で Kubernetes クラスターを作成する

1. Azure Kubernetes Service で Kubernetes クラスターを作成します。 次のコマンドでは、*kubernetes* クラスターを *wingtiptoys-kubernetes* リソース グループに作成します。クラスター名として *wingtiptoys-akscluster* を、DNS プレフィックスとして *wingtiptoys-kubernetes* を使用します。
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   このコマンドは、完了するまで時間がかかる場合があります。

1. Azure Kubernetes Service (AKS) で Azure Container Registry (ACR) を使用する場合は、Azure Kubernetes Service に Azure Container Registry へのプル アクセス権を付与する必要があります。 Azure Kubernetes Service の作成時に、既定のサービス プリンシパルが作成されます。 Bash または PowerShell で次のスクリプトを使用して、ACR へのアクセス権を AKS に付与します。詳細については、「[Azure Kubernetes Service から Azure Container Registry の認証を受ける]」を参照してください。

```bash
   # Get the id of the service principal configured for AKS
   CLIENT_ID=$(az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv)
   
   # Get the ACR registry resource id
   ACR_ID=$(az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv)
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

  -- または --

```PowerShell
   # Get the id of the service principal configured for AKS
   $CLIENT_ID = az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv
   
   # Get the ACR registry resource id
   $ACR_ID = az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

1. Azure CLI を使用して `kubectl` をインストールします。 Kubernetes CLI は `/usr/local/bin` にデプロイされるため、Linux ユーザーはこのコマンドの前に `sudo` を付けなければならない場合があります。
   ```azurecli
   az aks install-cli
   ```

1. クラスター構成情報をダウンロードして、Kubernetes Web インターフェイスと `kubectl` からクラスターを管理できるようにします。 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>イメージを Kubernetes クラスターにデプロイする

このチュートリアルでは、`kubectl` を使用してアプリをデプロイします。これにより、Kubernetes Web インターフェイスを通してデプロイを調べることができます。

### <a name="deploy-with-kubectl"></a>kubectl を使用してデプロイする

1. コマンド プロンプトを開きます。

1. `kubectl run` コマンドを使用して、Kubernetes クラスターのコンテナーを実行します。 Kubernetes でのアプリのサービス名と完全なイメージ名を指定します。 例: 
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   このコマンドの説明:

   * コンテナー名 `gs-spring-boot-docker` は `run` コマンドの直後に指定します。

   * `--image` パラメーターは、結合されたログイン サーバーとイメージの名前を `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest` として指定します。

1. `kubectl expose` コマンドを使用して、Kubernetes クラスターを外部に公開します。 サービス名、アプリにアクセスするために使用される公開 TCP ポート、およびアプリがリッスンする内部ターゲット ポートを指定します。 例: 
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   このコマンドの説明:

   * コンテナー名 `gs-spring-boot-docker` は `expose deployment` コマンドの直後に指定します。

   * `--type` パラメーターは、クラスターでロード バランサーを使用することを指定します。

   * `--port` パラメーターは、公開 TCP ポートとして 80 を指定します。 このポートでアプリにアクセスします。

   * `--target-port` パラメーターは、内部 TCP ポートとして 8080 を指定します。 ロード バランサーは、このポートでアプリに要求を転送します。

1. クラスターにアプリがデプロイされたら、外部 IP アドレスを照会し、そのアドレスを Web ブラウザーで開きます。

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=default
   ```

   ![Azure でサンプル アプリを参照する][SB02]



### <a name="deploy-with-the-kubernetes-web-interface"></a>Kubernetes Web インターフェイスを使用してデプロイする

1. コマンド プロンプトを開きます。

1. Kubernetes クラスターの構成 Web サイトを既定のブラウザーで開きます。
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. Kubernetes 構成 Web サイトがブラウザーで開いたら、**コンテナー化されたアプリをデプロイする**ためのリンクをクリックします。

   ![Kubernetes 構成 Web サイト][KB01]

1. **[Resource Creation]\(リソースの作成)** ページが表示されたら、次のオプションを指定します。

   a. **[CREATE AN APP]\(アプリの作成)** を選択します。

   b. Spring Boot アプリケーション名を **[App name]\(アプリ名\)** に入力します (例: "*gs-spring-boot-docker*")。

   c. ログイン サーバーとコンテナー イメージを **[Container image]\(コンテナー イメージ\)** に入力します (例: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*")。

   d. **[Service]\(サービス)** で **[External]\(外部)** を選択します。

   e. 外部ポートと内部ポートを **[Port]\(ポート)** テキスト ボックスと **[Target port]\(ターゲット ポート\)** テキスト ボックスに指定します。

   ![Kubernetes 構成 Web サイト][KB02]


1. **[Deploy]\(デプロイ)** をクリックしてコンテナーをデプロイします。

   ![Kubernetes デプロイ][KB05]

1. アプリケーションがデプロイされると、**[Services]\(サービス)** の下に Spring Boot アプリケーションが表示されます。

   ![Kubernetes サービス][KB06]

1. **[External endpoints]\(外部エンドポイント)\\** のリンクをクリックすると、Spring Boot アプリケーションが Azure で実行されていることを確認できます。

   ![Kubernetes サービス][KB07]

   ![Azure でサンプル アプリを参照する][SB02]

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure での Spring Boot の使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-web-app-on-azure.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

Visual Studio Code を使用して Java アプリケーションを Kubernetes にデプロイする方法の詳細については、[Visual Studio Code Java チュートリアル]を参照してください。

Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。

次のリンクは、Spring Boot アプリケーションの作成に関する追加情報を提供します。

* 単純な Spring Boot アプリケーションの作成の詳細については、Spring Initializr (https://start.spring.io/) を参照してください。

次のリンクは、Azure での Kubernetes の使用に関する追加情報を提供します。

* [Azure Kubernetes Service で Kubernetes クラスターを使用する](/azure/aks/intro-kubernetes)

Kubernetes コマンド ライン インターフェイスの使用方法の詳細については、**kubectl** ユーザー ガイド (<https://kubernetes.io/docs/user-guide/kubectl/>) を参照してください。

Kubernetes web サイトには、プライベート レジストリでのイメージの使用に関するさまざまな記事があります。

* [Pods のサービス アカウントの構成]
* [名前空間]
* [プライベート レジストリからのイメージのプル]

Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。

Azure Kubernetes Service (AKS) で直接 Azure Dev Spaces を使用してコンテナーの実行とデバッグを繰り返す場合の詳細については、「[Azure Dev Spaces での Java の使用]」を参照してください。

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Kubernetes Service (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[Java 開発者向けの Azure]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Azure DevOps と Java の操作]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker での Spring Boot の使用開始]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Pods のサービス アカウントの構成]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[名前空間]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[プライベート レジストリからのイメージのプル]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[Azure Kubernetes Service から Azure Container Registry の認証を受ける]: /azure/container-registry/container-registry-auth-aks/
[Visual Studio Code Java チュートリアル]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Azure Dev Spaces での Java の使用]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
