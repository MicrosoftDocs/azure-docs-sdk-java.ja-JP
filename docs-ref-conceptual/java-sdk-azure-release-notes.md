---
title: "Azure Management Libraries for Java リリース ノート | Microsoft Docs"
description: "Azure Management Libraries for Java の新機能や重大な変更が記載されています。"
keywords: "Azure,  Java, API, リファレンス,  ノート,  更新, 非推奨"
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: fc246d499b2f5f20a7efbaed16c4b4193d8d8e6c
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="release-notes"></a>リリース ノート 

## <a name="june-30-2017---110"></a>2017 年 6 月 30 日 - 1.1.0 

V1.0 で一般提供 (安定) 段階に達したパブリック ユース向け API に関して、V1.1 には V1.0 との下位互換性があります。

V.0 で @Beta の注釈を使ってマークされた API には、いくつかの重大な変更が導入されました。

コードを 1.1.0 に移行する場合は、[こちらの注意事項](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)を参照して、1.0.0 から 1.1.0 にコードを対応させてください。

### <a name="generally-availabile-in-v11"></a>V1.1 で一般提供

V1.0 でベータ版のままとなっている一部の API が、V1.1 で GA になりました。具体的な内容は次のとおりです。

- 非同期メソッド
- これまで Beta 版であった CDN のすべてのメソッド
- これまで Beta 版であった Application Gateway のすべてのメソッドおよびインターフェイス

 ライブラリの一部については引き続きプレビューとなります。 現在のライブラリの状態については、次の表を参照してください。

サービスまたは機能 | GA として利用可能 | プレビューとして利用可能  | 近日対応予定 |
---------|---------|---------|---------|
コンピューティング  | 仮想マシンと VM の拡張機能、仮想マシン スケール セット、管理ディスク   | Azure Container Service、Azure Container Registry |    |
Storage   |  ストレージ アカウント       |         |   暗号化      |
SQL Database  | データベース、ファイアウォール、エラスティック プール        |         |   その他の機能      |
ネットワーク    |  仮想ネットワーク、ネットワーク インターフェイス、IP アドレス、ルーティング テーブル、ネットワーク セキュリティ グループ、DNS、Traffic Manager、Application Gateway  |    ロード バランサー     |   VPN、Network Watcher   |
その他のサービス    |  リソース マネージャー、Key Vault、Redis、CDN、Batch       |  Web Apps、Function App、Service Bus、Graph RBAC、DocumentDB   | Monitor、Scheduler、Functions 管理、Search、Graph RBAC の各種機能        |
基礎     |   認証 - コア、非同期メソッド       |      |         |

### <a name="import-with-maven"></a>Maven でのインポート

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>質問とフィードバック

自分のコードでライブラリを使用する方法についてご不明な点がありましたら、[Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) コミュニティのサイトを参照してください。 バグを見つけた場合や、これらのライブラリの改善に向けた提案がある場合は、[GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 経由でお寄せください。

### <a name="migrate-from-previous-releases"></a>以前のリリースからの移行

[1.0.0-beta5 からの移行](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [1.1.0 からの移行](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)


