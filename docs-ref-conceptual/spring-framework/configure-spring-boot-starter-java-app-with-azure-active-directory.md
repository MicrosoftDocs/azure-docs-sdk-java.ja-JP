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
ms.date: 07/02/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 665768ffe7bec977d553ffa62e1dbd6b968eb9de
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799908"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="f0181-103">Azure Active Directory 用の Spring Boot Starter の使用方法</span><span class="sxs-lookup"><span data-stu-id="f0181-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="f0181-104">概要</span><span class="sxs-lookup"><span data-stu-id="f0181-104">Overview</span></span>

<span data-ttu-id="f0181-105">この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した **[Spring Initializr]** を用いてアプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f0181-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0181-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="f0181-106">Prerequisites</span></span>

<span data-ttu-id="f0181-107">この記事の手順を実行するには、次の前提条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0181-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="f0181-108">Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。</span><span class="sxs-lookup"><span data-stu-id="f0181-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="f0181-109">[Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) バージョン 1.7 以降。</span><span class="sxs-lookup"><span data-stu-id="f0181-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="f0181-110">[Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。</span><span class="sxs-lookup"><span data-stu-id="f0181-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="f0181-111">Spring Initializr を使用してカスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="f0181-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="f0181-112"><https://start.spring.io/> を参照します。</span><span class="sxs-lookup"><span data-stu-id="f0181-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="f0181-113">**Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(アーティファクト)** に名前を入力して、Spring Initializr の **[Switch to the full version]\(完全バージョンへの切り替え\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![グループとアーティファクトの名前を指定する][security-01]

1. <span data-ttu-id="f0181-115">下へスクロールして **[Core]\(コア\)** セクションを表示し、**[Security]\(セキュリティ\)** チェック ボックスをオンにします。また、**[Web]** セクションの **[Web]** チェック ボックスもオンにします。</span><span class="sxs-lookup"><span data-stu-id="f0181-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![セキュリティと Web のスターターを選択する][security-02]

1. <span data-ttu-id="f0181-117">下へスクロールして **[Azure]** セクションを表示し、**[Azure Active Directory]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="f0181-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![Azure Active Directory スターターを選択する][security-03]

1. <span data-ttu-id="f0181-119">ページの下部までスクロールし、**[Generate Project]\(プロジェクトの生成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Boot プロジェクトを生成する][security-04]

1. <span data-ttu-id="f0181-121">メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f0181-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="f0181-122">新しい Azure Active Directory インスタンスの作成と構成</span><span class="sxs-lookup"><span data-stu-id="f0181-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="f0181-123">Active Directory インスタンスを作成する</span><span class="sxs-lookup"><span data-stu-id="f0181-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="f0181-124"><https://portal.azure.com> にログインします。</span><span class="sxs-lookup"><span data-stu-id="f0181-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="f0181-125">**[+新規]**、**[セキュリティ + ID]**、**[Azure Active Directory]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![新しい Azure Active Directory インスタンスを作成する][directory-01]

1. <span data-ttu-id="f0181-127">**組織名**と**初期ドメイン名**を入力します。</span><span class="sxs-lookup"><span data-stu-id="f0181-127">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="f0181-128">ディレクトリの完全な URL をコピーします。このチュートリアルでは後ほど、これを使用してユーザー アカウントを追加します </span><span class="sxs-lookup"><span data-stu-id="f0181-128">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="f0181-129">(例: `wingtiptoysdirectory.onmicrosoft.com`)。操作が完了したら、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-129">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![Azure Active Directory の名前を指定する][directory-02]

1. <span data-ttu-id="f0181-131">Azure Portal の上部にあるツール バーのドロップダウン メニューから、新しく作成した Azure Active Directory を選択します。</span><span class="sxs-lookup"><span data-stu-id="f0181-131">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![Azure Active Directory を選択する][directory-03]

1. <span data-ttu-id="f0181-133">ポータル メニューから **[Azure Active Directory]** を選択し、**[プロパティ]** をクリックして **[ディレクトリ ID]** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="f0181-133">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![Azure Active Directory ID をコピーする][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="f0181-135">Spring Boot アプリのアプリケーション登録を追加する</span><span class="sxs-lookup"><span data-stu-id="f0181-135">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="f0181-136">ポータルのメニューで **[Azure Active Directory]** を選択し、**[概要]** をクリックして、**[アプリの登録]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-136">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![新しいアプリ登録を追加する][directory-04]

2. <span data-ttu-id="f0181-138">**[新しいアプリケーションの登録]** をクリックします。アプリケーションの **名前** を指定し、**[サインオン URL]** に「 http://localhost:8080」と入力して、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-138">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![新しいアプリ登録を作成する][directory-05]

3. <span data-ttu-id="f0181-140">アプリケーション登録が作成されたら、それをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-140">Click your application registration after it has been created.</span></span>

   ![アプリ登録を選択する][directory-06]

4. <span data-ttu-id="f0181-142">アプリ登録のページが表示されたら、**アプリケーション ID** をコピーします。このチュートリアルでは後ほど、この値を使用して *application.properties* ファイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="f0181-142">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="f0181-143">**[設定]** をクリックし、**[キー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-143">Click **Settings**, and then click **Keys**.</span></span>

   ![アプリの登録キーを作成する][directory-07]

5. <span data-ttu-id="f0181-145">**説明**を追加し、新しいキーの**期間**を指定して、**[保存]** をクリックします。**[保存]** アイコンをクリックすると、キーの値が自動的に入力されます。このチュートリアルで後ほど *application.properties* ファイルを構成できるように、キーの値をコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0181-145">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="f0181-146">(この値は後で取得することはできません)。</span><span class="sxs-lookup"><span data-stu-id="f0181-146">(You will not be able to retrieve this value later.)</span></span>

   ![アプリの登録キーのパラメーターを指定する][directory-08]

6. <span data-ttu-id="f0181-148">アプリ登録のメイン ページで、**[設定]**、**[必要なアクセス許可]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-148">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![アプリ登録の必要なアクセス許可][directory-09]

7. <span data-ttu-id="f0181-150">**[Windows Azure Active Directory]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-150">Click **Windows Azure Active Directory**.</span></span>

   ![[Windows Azure Active Directory] を選択する][directory-10]

8. <span data-ttu-id="f0181-152">**[サインインしたユーザーとしてディレクトリにアクセスします]** と **[サインインとユーザー プロファイルの読み取り]** の各チェック ボックスをオンにし、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-152">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![アクセス許可を有効にする][directory-11]

9. <span data-ttu-id="f0181-154">**[必要なアクセス許可]** ページで **[アクセス許可の付与]** をクリックし、メッセージが表示されたら **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-154">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![アクセス許可を付与する][directory-12]

10. <span data-ttu-id="f0181-156">アプリ登録のメイン ページで、**[設定]**、**[応答 URL]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-156">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![応答 URL を編集する][directory-14]

11. <span data-ttu-id="f0181-158">新しい応答 URL として「<http://localhost:8080/login/oauth2/code/azure>」と入力し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-158">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![新しい応答 URL を追加する][directory-15]

12. <span data-ttu-id="f0181-160">アプリ登録のメイン ページで、**[マニフェスト]** をクリックして `oauth2AllowImplicitFlow` パラメーターの値を `true` に設定し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-160">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![アプリケーション マニフェストの構成][directory-16]

    > [!NOTE]
    > 
    > <span data-ttu-id="f0181-162">`oauth2AllowImplicitFlow` パラメーターとその他のアプリケーション設定の詳細については、「[Azure Active Directory アプリケーション マニフェスト][AAD app manifest]」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f0181-162">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="f0181-163">ディレクトリにユーザー アカウントを追加し、そのアカウントをグループに追加する</span><span class="sxs-lookup"><span data-stu-id="f0181-163">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="f0181-164">Active Directory の **[概要]** ページで、**[ユーザー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-164">From the **Overview** page of your Active Directory, click **Users**.</span></span>

   ![ユーザー パネルを開く][directory-17]

1. <span data-ttu-id="f0181-166">**[ユーザー]** パネルが表示されたら、**[新しいユーザー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0181-166">When the **Users** panel is displayed, click **New user**.</span></span>

   ![新しいユーザー アカウントを追加する][directory-18]

1. <span data-ttu-id="f0181-168">**[ユーザー]** パネルが表示されたら、**[名前]** と **[ユーザー名]** に入力します。</span><span class="sxs-lookup"><span data-stu-id="f0181-168">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![ユーザー アカウント情報を入力する][directory-19]

   > [!NOTE]
   > 
   > <span data-ttu-id="f0181-170">ユーザー名を入力するときに、以下のように、このチュートリアルで先に出てきたディレクトリの URL を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0181-170">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="f0181-171">**[グループ]** をクリックして、アプリケーションの承認で使用するグループを選択し、**[選択]** をクリックします </span><span class="sxs-lookup"><span data-stu-id="f0181-171">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="f0181-172">(このチュートリアルの目的では、アカウントを _Users_ グループに追加します)。</span><span class="sxs-lookup"><span data-stu-id="f0181-172">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![ユーザーのグループを選択する][directory-20]

1. <span data-ttu-id="f0181-174">**[パスワードの表示]** をクリックしてパスワードをコピーします。このチュートリアルで後ほどアプリケーションにログインするときに、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="f0181-174">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span>

   ![パスワードを表示する][directory-21]

1. <span data-ttu-id="f0181-176">**[作成]** をクリックして、新しいユーザー アカウントをディレクトリに追加します。</span><span class="sxs-lookup"><span data-stu-id="f0181-176">Click **Create** to add the new user account to your directory.</span></span>

   ![新しいユーザー アカウントを作成する][directory-22]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="f0181-178">Spring Boot アプリケーションの構成とコンパイル</span><span class="sxs-lookup"><span data-stu-id="f0181-178">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="f0181-179">このチュートリアルで先ほど作成しダウンロードしたプロジェクト アーカイブからディレクトリにファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="f0181-179">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="f0181-180">プロジェクトの親フォルダーに移動し、テキスト エディターで *pom.xml* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="f0181-180">Navigate to the parent folder for your project, and open the *pom.xml* file in a text editor.</span></span>

1. <span data-ttu-id="f0181-181">Spring OAuth2 セキュリティの依存関係を追加します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f0181-181">Add the dependencies for Spring OAuth2 security; for example:</span></span>

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

1. <span data-ttu-id="f0181-182">*pom.xml* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="f0181-182">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="f0181-183">プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="f0181-183">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="f0181-184">前に作成した値を使用して、アプリ登録の設定を指定します。たとえば、以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f0181-184">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="f0181-185">各値の説明:</span><span class="sxs-lookup"><span data-stu-id="f0181-185">Where:</span></span>

   | <span data-ttu-id="f0181-186">パラメーター</span><span class="sxs-lookup"><span data-stu-id="f0181-186">Parameter</span></span> | <span data-ttu-id="f0181-187">説明</span><span class="sxs-lookup"><span data-stu-id="f0181-187">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="f0181-188">前の Active Directory の **ディレクトリ ID** を指定します。</span><span class="sxs-lookup"><span data-stu-id="f0181-188">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="f0181-189">以前に完了したアプリ登録の**アプリケーション ID** を指定します。</span><span class="sxs-lookup"><span data-stu-id="f0181-189">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="f0181-190">以前に完了したアプリ登録キーの**値** を指定します。</span><span class="sxs-lookup"><span data-stu-id="f0181-190">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="f0181-191">承認に使用する Active Directory グループの一覧を指定します。</span><span class="sxs-lookup"><span data-stu-id="f0181-191">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="f0181-192">*application.properties* ファイルで使用できる値の完全な一覧については、GitHub の「[Azure Active Directory Spring Boot Sample (Azure Active Directory Spring Boot のサンプル)][AAD Spring Boot Sample]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0181-192">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="f0181-193">*application.properties* ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="f0181-193">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="f0181-194">アプリケーションの Java ソース フォルダーに、"*controller*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/controller*)。</span><span class="sxs-lookup"><span data-stu-id="f0181-194">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="f0181-195">*controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="f0181-195">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="f0181-196">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="f0181-196">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="f0181-197">`@PreAuthorize("hasRole('')")` メソッドに指定するグループ名には、*application.properties* ファイルの `azure.activedirectory.active-directory-groups` フィールドに指定したグループのいずれかが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0181-197">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   >

   > [!NOTE]
   > 
   > <span data-ttu-id="f0181-198">異なる要求マッピングには異なる承認設定を指定できます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f0181-198">You can specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="f0181-199">アプリケーションの Java ソース フォルダーに、"*security*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/security*)。</span><span class="sxs-lookup"><span data-stu-id="f0181-199">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="f0181-200">*security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="f0181-200">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="f0181-201">次のコードを入力し、ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="f0181-201">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="f0181-202">アプリのビルドとテスト</span><span class="sxs-lookup"><span data-stu-id="f0181-202">Build and test your app</span></span>

1. <span data-ttu-id="f0181-203">コマンド プロンプトを開き、ディレクトリをアプリの *pom.xml* ファイルがあるフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="f0181-203">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="f0181-204">Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f0181-204">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![アプリをビルドする][build-application]

1. <span data-ttu-id="f0181-206">Maven によってアプリケーションがビルドされ、起動したら、Web ブラウザーで <http://localhost:8080> を開きます。ユーザー名とパスワードを入力するように求めるメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0181-206">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![アプリケーションへのログイン][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="f0181-208">新しいユーザー アカウントに初めてログインする場合、パスワードの変更を求められることがあります。</span><span class="sxs-lookup"><span data-stu-id="f0181-208">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![パスワードの変更][update-password]
   > 

1. <span data-ttu-id="f0181-210">正常にログインしたら、コントローラーにサンプルの "Hello World" テキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0181-210">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![正常なログイン][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="f0181-212">承認されていないユーザー アカウントには、**HTTP 403 Unauthorized** メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0181-212">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="f0181-213">次の手順</span><span class="sxs-lookup"><span data-stu-id="f0181-213">Next steps</span></span>

<span data-ttu-id="f0181-214">Azure Active Directory の使用方法の詳細については、次の記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f0181-214">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="f0181-215">[Azure Active Directory のドキュメント]</span><span class="sxs-lookup"><span data-stu-id="f0181-215">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="f0181-216">Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0181-216">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="f0181-217">Spring Boot アプリケーションを Azure App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="f0181-217">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="f0181-218">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)</span><span class="sxs-lookup"><span data-stu-id="f0181-218">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="f0181-219">Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および [Visual Studio Team Services 用の Java ツール] を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0181-219">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="f0181-220">**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="f0181-220">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="f0181-221">このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="f0181-221">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="f0181-222">Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="f0181-222">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="f0181-223">基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、**[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。</span><span class="sxs-lookup"><span data-stu-id="f0181-223">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="f0181-224">より詳細なサンプルについては、GitHub の「[Azure Active Directory Spring Boot Sample (Azure Active Directory Spring Boot のサンプル)][AAD Spring Boot Sample]」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0181-224">For a more-detailed sample, see the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>

<!-- URL List -->

[Azure Active Directory のドキュメント]: /azure/active-directory/
[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Java 開発者向けの Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services 用の Java ツール]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[security-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-01.png
[security-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-02.png
[security-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-03.png
[security-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-04.png

[directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-01.png
[directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-02.png
[directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-03.png
[directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-04.png
[directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-05.png
[directory-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-06.png
[directory-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-07.png
[directory-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-08.png
[directory-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-09.png
[directory-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-10.png
[directory-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-11.png
[directory-12]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-12.png
[directory-13]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-13.png
[directory-14]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-14.png
[directory-15]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-15.png
[directory-16]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-16.png
[directory-17]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-17.png
[directory-18]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-18.png
[directory-19]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-19.png
[directory-20]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-20.png
[directory-21]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-21.png
[directory-22]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-22.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
