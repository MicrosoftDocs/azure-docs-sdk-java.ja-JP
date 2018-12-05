---
title: Azure Active Directory 用の Spring Boot Starter の使用方法
description: Azure Active Directory スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: active-directory
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/21/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 89e294fa739b59f87667f901e914fd5f050b5b8c
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338776"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="df708-103">チュートリアル: Azure Active Directory 用の Spring Boot Starter を使用して Java Web アプリをセキュリティで保護する</span><span class="sxs-lookup"><span data-stu-id="df708-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="df708-104">概要</span><span class="sxs-lookup"><span data-stu-id="df708-104">Overview</span></span>

<span data-ttu-id="df708-105">この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した **[Spring Initializr]** で Java アプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="df708-105">This article demonstrates creating a Java app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="df708-106">このチュートリアルでは、以下の内容を学習します。</span><span class="sxs-lookup"><span data-stu-id="df708-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="df708-107">Spring Initializr を使用して Java アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="df708-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="df708-108">Azure Active Directory を構成する</span><span class="sxs-lookup"><span data-stu-id="df708-108">Configure Azure Active Directory</span></span>
> * <span data-ttu-id="df708-109">Spring Boot クラスと注釈を使用してアプリケーションをセキュリティで保護する</span><span class="sxs-lookup"><span data-stu-id="df708-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="df708-110">Java アプリケーションをビルドしてテストする</span><span class="sxs-lookup"><span data-stu-id="df708-110">Build and test your Java application</span></span>

<span data-ttu-id="df708-111">Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。</span><span class="sxs-lookup"><span data-stu-id="df708-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df708-112">前提条件</span><span class="sxs-lookup"><span data-stu-id="df708-112">Prerequisites</span></span>

<span data-ttu-id="df708-113">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="df708-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="df708-114">サポートされている Java Development Kit (JDK)。</span><span class="sxs-lookup"><span data-stu-id="df708-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="df708-115">Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="df708-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="df708-116">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="df708-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="df708-117">Spring Initializr を使用したアプリの作成</span><span class="sxs-lookup"><span data-stu-id="df708-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="df708-118"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="df708-118">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="df708-119">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![グループとアーティファクトの名前を指定する][create-spring-app-01]

1. <span data-ttu-id="df708-121">下へスクロールして **[Core]\(コア\)** セクションに移動し、**[Security]\(セキュリティ\)** チェックボックスをオンにし、**[Web]** セクションで **[Web]** チェックボックスをオンにします。次に、下へスクロールして **[Azure]** セクションに移動し、**[Azure Active Directory]** チェックボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="df708-121">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**, then scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![セキュリティ、Web、および Azure Active Directory スターターを選択する][create-spring-app-02]

1. <span data-ttu-id="df708-123">ページの上部または下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-123">Scroll to the top or bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Boot プロジェクトを生成する][create-spring-app-03]

1. <span data-ttu-id="df708-125">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="df708-125">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="df708-126">Azure Active Directory インスタンスの作成</span><span class="sxs-lookup"><span data-stu-id="df708-126">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="df708-127">Active Directory インスタンスを作成する</span><span class="sxs-lookup"><span data-stu-id="df708-127">Create the Active Directory instance</span></span>

1. <span data-ttu-id="df708-128"><https://portal.azure.com> にログインします。</span><span class="sxs-lookup"><span data-stu-id="df708-128">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="df708-129">**[+リソースの作成]**、**[ID]**、**[Azure Active Directory]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-129">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory**.</span></span>

   ![新しい Azure Active Directory インスタンスを作成する][create-directory-01]

1. <span data-ttu-id="df708-131">**組織名**と**初期ドメイン名**を入力します。</span><span class="sxs-lookup"><span data-stu-id="df708-131">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="df708-132">ディレクトリの完全な URL をコピーします。このチュートリアルでは後ほど、これを使用してユーザー アカウントを追加します </span><span class="sxs-lookup"><span data-stu-id="df708-132">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="df708-133">(例: `wingtiptoysdirectory.onmicrosoft.com`)。操作が完了したら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-133">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![Azure Active Directory の名前を指定する][create-directory-02]

1. <span data-ttu-id="df708-135">Azure portal ツール バーの右上にあるご自身のアカウント名を選択し、**[ディレクトリの切り替え]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-135">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![Azure アカウント名を選択する][create-directory-03]

1. <span data-ttu-id="df708-137">ドロップダウン メニューから新しく作成した Azure Active Directory を選択します。</span><span class="sxs-lookup"><span data-stu-id="df708-137">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![Azure Active Directory を選択する][create-directory-04]

1. <span data-ttu-id="df708-139">ポータル メニューから **[Azure Active Directory]** を選択し、**[プロパティ]** をクリックして **[ディレクトリ ID]** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="df708-139">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![Azure Active Directory ID をコピーする][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="df708-141">Spring Boot アプリのアプリケーション登録を追加する</span><span class="sxs-lookup"><span data-stu-id="df708-141">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="df708-142">ポータルのメニューで **[Azure Active Directory]** を選択し、**[アプリの登録]**、**[新しいアプリケーションの登録]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-142">Select **Azure Active Directory** from the portal menu, click **App registrations**, and then click **New application registration**, .</span></span>

   ![新しいアプリ登録を追加する][create-app-registration-01]

2. <span data-ttu-id="df708-144">アプリケーションの**名前**を指定し、**[サインオン URL]** に「 http://localhost:8080」と入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-144">Specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![新しいアプリ登録を作成する][create-app-registration-02]

4. <span data-ttu-id="df708-146">アプリ登録のページが表示されたら、**アプリケーション ID** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="df708-146">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="df708-147">**[設定]** をクリックし、**[キー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-147">Click **Settings**, and then click **Keys**.</span></span>

   ![アプリの登録キーを作成する][create-app-registration-03]

5. <span data-ttu-id="df708-149">**説明**を追加し、新しいキーの**期間**を指定して、**[保存]** をクリックします。**[保存]** アイコンをクリックすると、キーの値が自動的に入力されます。このチュートリアルで後ほど *application.properties* ファイルを構成できるように、キーの値をコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="df708-149">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="df708-150">(この値は後で取得することはできません)。</span><span class="sxs-lookup"><span data-stu-id="df708-150">(You will not be able to retrieve this value later.)</span></span>

   ![アプリの登録キーのパラメーターを指定する][create-app-registration-04]

6. <span data-ttu-id="df708-152">アプリ登録のメイン ページで、**[設定]**、**[必要なアクセス許可]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-152">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![アプリ登録の必要なアクセス許可][create-app-registration-05]

7. <span data-ttu-id="df708-154">**[Windows Azure Active Directory]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-154">Click **Windows Azure Active Directory**.</span></span>

   ![[Windows Azure Active Directory] を選択する][create-app-registration-06]

8. <span data-ttu-id="df708-156">**[サインインしたユーザーとしてディレクトリにアクセスします]** と **[サインインとユーザー プロファイルの読み取り]** の各チェック ボックスをオンにし、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-156">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![アクセス許可を有効にする][create-app-registration-07]

9. <span data-ttu-id="df708-158">**[必要なアクセス許可]** ページで **[アクセス許可の付与]** をクリックし、メッセージが表示されたら **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-158">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![アクセス許可を付与する][create-app-registration-08]

10. <span data-ttu-id="df708-160">アプリ登録のメイン ページで、**[設定]**、**[応答 URL]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-160">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![応答 URL を編集する][create-app-registration-09]

11. <span data-ttu-id="df708-162">新しい応答 URL として「<http://localhost:8080/login/oauth2/code/azure>」と入力し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-162">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![新しい応答 URL を追加する][create-app-registration-10]

12. <span data-ttu-id="df708-164">アプリ登録のメイン ページで、**[マニフェスト]** をクリックして `oauth2AllowImplicitFlow` パラメーターの値を `true` に設定し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-164">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![アプリケーション マニフェストの構成][create-app-registration-11]

    > [!NOTE]
    > 
    > <span data-ttu-id="df708-166">`oauth2AllowImplicitFlow` パラメーターとその他のアプリケーション設定の詳細については、「[Azure Active Directory アプリケーション マニフェスト][AAD app manifest]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="df708-166">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="df708-167">ディレクトリにユーザー アカウントを追加し、そのアカウントをグループに追加する</span><span class="sxs-lookup"><span data-stu-id="df708-167">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="df708-168">Active Directory の **[概要]** ページで、**[すべてのユーザー]** をクリックし、次に **[新しいユーザー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="df708-168">From the **Overview** page of your Active Directory, click **All Users**, and then click **New user**.</span></span>

   ![新しいユーザー アカウントを追加する][create-user-01]

1. <span data-ttu-id="df708-170">**[ユーザー]** パネルが表示されたら、**[名前]** と **[ユーザー名]** に入力します。</span><span class="sxs-lookup"><span data-stu-id="df708-170">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![ユーザー アカウント情報を入力する][create-user-02]

   > [!NOTE]
   > 
   > <span data-ttu-id="df708-172">ユーザー名を入力するときに、以下のように、このチュートリアルで先に出てきたディレクトリの URL を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="df708-172">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="df708-173">**[グループ]** をクリックして、アプリケーションの承認で使用するグループを選択し、**[選択]** をクリックします </span><span class="sxs-lookup"><span data-stu-id="df708-173">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="df708-174">(このチュートリアルの目的では、アカウントを _Users_ グループに追加します)。</span><span class="sxs-lookup"><span data-stu-id="df708-174">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![ユーザーのグループを選択する][create-user-03]

1. <span data-ttu-id="df708-176">**[パスワードの表示]** をクリックしてパスワードをコピーします。このチュートリアルで後ほどアプリケーションにログインするときに、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="df708-176">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span> <span data-ttu-id="df708-177">パスワードをコピーしたら、**[作成]** をクリックして新しいユーザー アカウントをディレクトリに追加します。</span><span class="sxs-lookup"><span data-stu-id="df708-177">When you have copied the password, click **Create** to add the new user account to your directory.</span></span>

   ![パスワードを表示する][create-user-04]

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="df708-179">アプリの構成およびコンパイル</span><span class="sxs-lookup"><span data-stu-id="df708-179">Configure and compile your app</span></span>

1. <span data-ttu-id="df708-180">このチュートリアルで先ほど作成しダウンロードしたプロジェクト アーカイブからディレクトリにファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="df708-180">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="df708-181">プロジェクトの親フォルダーに移動し、テキスト エディターで `pom.xml` Maven プロジェクト ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="df708-181">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

1. <span data-ttu-id="df708-182">Spring OAuth2 セキュリティの依存関係を `pom.xml` に追加します。</span><span class="sxs-lookup"><span data-stu-id="df708-182">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-client</artifactId>
   </dependency>
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-jose</artifactId>
   </dependency>
   ```

1. <span data-ttu-id="df708-183">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="df708-183">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="df708-184">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="df708-184">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="df708-185">前に作成した値を使用して、アプリ登録の設定を指定します。たとえば、以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="df708-185">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.active-directory-groups=Users
   ```
   <span data-ttu-id="df708-186">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="df708-186">Where:</span></span>

   | <span data-ttu-id="df708-187">パラメーター</span><span class="sxs-lookup"><span data-stu-id="df708-187">Parameter</span></span> | <span data-ttu-id="df708-188">説明</span><span class="sxs-lookup"><span data-stu-id="df708-188">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="df708-189">前の Active Directory の **ディレクトリ ID** を指定します。</span><span class="sxs-lookup"><span data-stu-id="df708-189">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="df708-190">以前に完了したアプリ登録の**アプリケーション ID** を指定します。</span><span class="sxs-lookup"><span data-stu-id="df708-190">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="df708-191">以前に完了したアプリ登録キーの**値** を指定します。</span><span class="sxs-lookup"><span data-stu-id="df708-191">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="df708-192">承認に使用する Active Directory グループの一覧を指定します。</span><span class="sxs-lookup"><span data-stu-id="df708-192">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="df708-193">*application.properties* ファイルで使用できる値の完全な一覧については、GitHub の「[Azure Active Directory Spring Boot Sample (Azure Active Directory Spring Boot のサンプル)][AAD Spring Boot Sample]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="df708-193">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="df708-194">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="df708-194">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="df708-195">アプリケーションの Java ソース フォルダーに、"*controller*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/controller*)。</span><span class="sxs-lookup"><span data-stu-id="df708-195">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="df708-196">*controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="df708-196">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="df708-197">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="df708-197">Enter the following code, then save and close the file:</span></span>

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.oauth2.client.OAuth2AuthorizedClient;
   import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
   import org.springframework.ui.Model;

   @RestController
   public class HelloController {
      @Autowired
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String helloWorld() {
         return "Hello World!";
      }
   }
   ```
   > [!NOTE]
   > 
   > <span data-ttu-id="df708-198">`@PreAuthorize("hasRole('')")` メソッドに指定するグループ名には、*application.properties* ファイルの `azure.activedirectory.active-directory-groups` フィールドに指定したグループのいずれかが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="df708-198">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   > 
   > <span data-ttu-id="df708-199">異なる要求マッピングには異なる承認設定を指定することもできます。以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="df708-199">You can also specify different authorization settings for different request mappings; for example:</span></span>
   >
   > ``` java
   > public class HelloController {
   >    @Autowired
   >    @PreAuthorize("hasRole('Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('Group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('Group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```
   >    

1. <span data-ttu-id="df708-200">アプリケーションの Java ソース フォルダーに、"*security*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/security*)。</span><span class="sxs-lookup"><span data-stu-id="df708-200">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="df708-201">*security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="df708-201">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="df708-202">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="df708-202">Enter the following code, then save and close the file:</span></span>

    ```java
    package com.wingtiptoys.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.oauth2.client.oidc.userinfo.OidcUserRequest;
    import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
    import org.springframework.security.oauth2.core.oidc.user.OidcUser;

    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Autowired
        private OAuth2UserService<OidcUserRequest, OidcUser> oidcUserService;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .oidcUserService(oidcUserService);
        }
    }
    ```

## <a name="build-and-test-your-app"></a><span data-ttu-id="df708-203">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="df708-203">Build and test your app</span></span>

1. <span data-ttu-id="df708-204">コマンド プロンプトを開き、ディレクトリをアプリの *pom.xml* ファイルがあるフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="df708-204">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="df708-205">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="df708-205">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![アプリをビルドする][build-application]

1. <span data-ttu-id="df708-207">Maven によってアプリケーションがビルドされ、起動したら、Web ブラウザーで <http://localhost:8080> を開きます。ユーザー名とパスワードを入力するように求めるメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="df708-207">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![アプリケーションへのログイン][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="df708-209">新しいユーザー アカウントに初めてログインする場合、パスワードの変更を求められることがあります。</span><span class="sxs-lookup"><span data-stu-id="df708-209">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![パスワードの変更][update-password]
   > 

1. <span data-ttu-id="df708-211">正常にログインしたら、コントローラーにサンプルの "Hello World" テキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="df708-211">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![正常なログイン][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="df708-213">承認されていないユーザー アカウントには、**HTTP 403 Unauthorized** メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="df708-213">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="summary"></a><span data-ttu-id="df708-214">まとめ</span><span class="sxs-lookup"><span data-stu-id="df708-214">Summary</span></span>

<span data-ttu-id="df708-215">このチュートリアルでは、Azure Active Directory スターターを使用した新しい Java Web アプリケーションの作成、新しい Azure AD テナントの構成とそのテナントへの新しいアプリケーションの登録を行いました。また、Spring の注釈とクラスを使用して Web を保護するようにアプリケーションを構成しました。</span><span class="sxs-lookup"><span data-stu-id="df708-215">In this tutorial, you created a new Java web application using the Azure Active Directory starter, configured a new Azure AD tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df708-216">次の手順</span><span class="sxs-lookup"><span data-stu-id="df708-216">Next steps</span></span>

<span data-ttu-id="df708-217">Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="df708-217">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="df708-218">Azure の Spring</span><span class="sxs-lookup"><span data-stu-id="df708-218">Spring on Azure</span></span>](/java/azure/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /java/azure/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png
[create-spring-app-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-02.png
[create-spring-app-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-03.png

[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png
[create-directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-05.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-06.png
[create-app-registration-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-07.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png
[create-app-registration-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-11.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
