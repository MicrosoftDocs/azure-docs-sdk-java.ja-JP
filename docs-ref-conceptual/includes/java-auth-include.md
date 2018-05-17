<span data-ttu-id="47c3e-101">[認証ファイル](../java-sdk-azure-authenticate.md#mgmt-file)を作成し、環境変数 `AZURE_AUTH_LOCATION` と認証ファイルの完全なパスをコマンド ラインでエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="47c3e-101">Create an [authentication file](../java-sdk-azure-authenticate.md#mgmt-file) and export an environment variable `AZURE_AUTH_LOCATION` on the command line with the full path to the file.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

<span data-ttu-id="47c3e-102">Azure リソースを定義、作成、構成するために管理ライブラリで使用されるエントリ ポイント `Azure` オブジェクトを構成するために、この認証ファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="47c3e-102">The authentication file is used to configure the entry point `Azure` object used by the management libraries to define, create, and configure Azure resources.</span></span>

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

<span data-ttu-id="47c3e-103">Azure Management Libraries for Java を使用する際の認証オプションの詳細については、[こちら](../java-sdk-azure-authenticate.md#mgmt-auth)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="47c3e-103">[Learn more](../java-sdk-azure-authenticate.md#mgmt-auth) about authentication options when using the Azure management libraries for Java.</span></span>