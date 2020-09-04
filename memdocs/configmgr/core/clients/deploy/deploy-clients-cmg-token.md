---
title: CMG のトークンベースの認証
titleSuffix: Configuration Manager
description: 内部ネットワークにクライアントの一意のトークンを登録するか、インターネット ベースのデバイスの一括登録トークンを作成します。
ms.date: 08/17/2020
ms.prod: configuration-manager
ms.technology: configmgr-client
ms.topic: conceptual
ms.assetid: f0703475-85a4-450d-a4e8-7a18a01e2c47
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 55997c9185a221d105aa8ad40bbb14021463d07b
ms.sourcegitcommit: da5bfbe16856fdbfadc40b3797840e0b5110d97d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2020
ms.locfileid: "88512701"
---
# <a name="token-based-authentication-for-cloud-management-gateway"></a>クラウド管理ゲートウェイのトークン ベース認証

*適用対象:Configuration Manager (Current Branch)*

<!--5686290-->

クラウド管理ゲートウェイ (CMG) では多くの種類のクライアントがサポートされていますが、たとえ[拡張 HTTP](../../plan-design/hierarchy/enhanced-http.md) であっても、これらのクライアントには[クライアント認証証明書](../manage/cmg/certificates-for-cloud-management-gateway.md#for-internet-based-clients-communicating-with-the-cloud-management-gateway)が必要です。 この証明書の要件は、内部ネットワークに接続することがあまりなく、Azure Active Directory (Azure AD) に参加できず、PKI によって発行された証明書をインストールする方法を持たない、インターネットベースのクライアントでは、プロビジョニングするのが困難な場合があります。

これらの問題に対応するため、バージョン 2002 以降の Configuration Manager では、独自の認証トークンをデバイスに発行することによって、デバイスのサポートが拡張されています。 この機能を最大限に活用するには、サイトを更新した後、クライアントも最新バージョンに更新します。 完全なシナリオは、クライアントのバージョンも最新になるまで機能しません。 必要に応じて、[新しいクライアント バージョンを実稼働に昇格](../manage/upgrade/test-client-upgrades.md#to-promote-the-new-client-to-production)することを確認してください。

 クライアントでは、最初に、次の 2 つの方法のいずれかを使用して、これらのトークンに登録します。

- 内部ネットワーク

- 一括登録

Configuration Manager クライアントは管理ポイントと共にこのトークンを管理するため、OS バージョンの依存関係はありません。 この機能は、任意の[サポートされているクライアント OS バージョン](../../plan-design/configs/supported-operating-systems-for-clients-and-devices.md)で使用できます。

> [!NOTE]
> これらの方法では、デバイス中心の管理シナリオのみがサポートされます。
>
> デバイスを Azure AD に参加させることをお勧めします。 インターネットベースのデバイスでは、Azure AD を使用して、Configuration Manager での認証を行うことができます。 また、デバイスがインターネット上にあるか、内部ネットワークに接続されているかにかかわらず、デバイスとユーザーの両方のシナリオに対応できます。 詳細については、「[Azure AD の ID を使用してクライアントをインストールして登録する](deploy-clients-cmg-azure.md#install-and-register-the-client-using-azure-ad-identity)」を参照してください。

## <a name="internal-network-registration"></a>内部ネットワーク登録

この方法では、クライアントは最初に内部ネットワーク上の管理ポイントに登録する必要があります。 クライアントの登録は、通常、インストールの直後に行われます。 クライアントが自己署名証明書を使用していることを示す一意のトークンが、管理ポイントから提供されます。 クライアントがインターネット上でローミングすると、CMG と通信するために、自己署名証明書と管理ポイントによって発行されたトークンがペアリングされます。

サイトでは、既定でこの動作が有効になります。

## <a name="bulk-registration-token"></a>一括登録トークン

内部ネットワークにクライアントをインストールして登録できない場合は、一括登録トークンを作成します。 クライアントがインターネットベースのデバイスにインストールされ、CMG を介して登録されるときは、このトークンを使用します。 一括登録トークンは有効期間が短く、クライアントまたはサイトに保存されません。 これにより、クライアントは、自己署名証明書とペアリングされた一意のトークンを生成して、CMG で認証できるようになります。

> [!NOTE]
> 一括登録トークンと、Configuration Manager によって個々のクライアントに発行されるものを、混同しないようにしてください。 一括登録トークンを使用すると、クライアントは最初にインストールを行ってサイトと通信することができます。 この初期通信は、サイトにより独自の一意のクライアント認証トークンがクライアントに発行されるのに十分な長さです。 その後、クライアントでは、インターネットに接続されている間、サイトとのすべての通信にその認証トークンが使用されます。 初期登録以外では、クライアントによって一括登録トークンが使用または格納されることはありません。

インターネット ベースのデバイスへのクライアントのインストールの間に使用される一括登録トークンを作成するには、次の操作を実行します。

1. ローカル管理者特権を使用して、階層内の最上位のサイト サーバーにサインインします。

1. コマンド プロンプトを管理者として開きます。

1. サイト サーバー上の Configuration Manager インストール ディレクトリの `\bin\X64` フォルダーから、ツール `BulkRegistrationTokenTool.exe` を実行します。 `/new` パラメーターを使用して新しいトークンを作成します。 たとえば、`BulkRegistrationTokenTool.exe /new` となります。 詳細については、「[一括登録トークン ツールの使用方法](#bulk-registration-token-tool-usage)」を参照してください。

1. トークンをコピーし、安全な場所に保存します。

1. インターネットベースのデバイスに、Configuration Manager クライアントをインストールします。 クライアント インストール パラメーター [ **/regtoken**](about-client-installation-properties.md#regtoken) を含めます。 次のコマンド ラインの例には、他の必要なセットアップ パラメーターとプロパティが含まれています。

    `ccmsetup.exe /mp:https://CONTOSO.CLOUDAPP.NET/CCM_Proxy_MutualAuth/72186325152220500 CCMHOSTNAME=CONTOSO.CLOUDAPP.NET/CCM_Proxy_MutualAuth/72186325152220500 SMSSiteCode=ABC /regtoken:eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik9Tbzh2Tmd5VldRUjlDYVh5T2lacHFlMDlXNCJ9.eyJTQ0NNVG9rZW5DYXRlZ29yeSI6IlN7Q01QcmVBdXRoVG9rZW4iLCJBdXRob3JpdHkiOiJTQ0NNIiwiTGljZW5zZSI6IlNDQ00iLCJUeXBlIjoiQnVsa1JlZ2lzdHJhdGlvbiIsIlRlbmFudElkIjoiQ0RDQzVFOTEtMEFERi00QTI0LTgyRDAtMTk2NjY3RjFDMDgxIiwiVW5pcXVlSWQiOiJkYjU5MWUzMy1wNmZkLTRjNWItODJmMy1iZjY3M2U1YmQwYTIiLCJpc3MiOiJ1cm46c2NjbTpvYXV0aDI6Y2RjYzVlOTEtMGFkZi00YTI0LTgyZDAtMTk2NjY3ZjFjMDgxIiwiYXVkIjoidXJuOnNjY206c2VydmljZSIsImV4cCI6MTU4MDQxNbUwNSwibmJmIjoxNTgwMTU2MzA1fQ.ZUJkxCX6lxHUZhMH_WhYXFm_tbXenEdpgnbIqI1h8hYIJw7xDk3wv625SCfNfsqxhAwRwJByfkXdVGgIpAcFshzArXUVPPvmiUGaxlbB83etUTQjrLIk-gvQQZiE5NSgJ63LCp5KtqFCZe8vlZxnOloErFIrebjFikxqAgwOO4i5ukJdl3KQ07YPRhwpuXmwxRf1vsiawXBvTMhy40SOeZ3mAyCRypQpQNa7NM3adCBwUtYKwHqiX3r1jQU0y57LvU_brBfLUL6JUpk3ri-LSpwPFarRXzZPJUu4-mQFIgrMmKCYbFk3AaEvvrJienfWSvFYLpIYA7lg-6EVYRcCAA`

    > [!TIP]
    > このコマンド ラインの詳細については、「[Azure AD の ID を使用してクライアントをインストールして登録する](deploy-clients-cmg-azure.md#install-and-register-the-client-using-azure-ad-identity)」を参照してください。 このプロセスは似ており、Azure AD のプロパティを使わないだけです。

確かめるために、次のログ ファイルで同様のエントリを確認します。<!-- bug 7357499 -->

```ClientLocation.log
Rotating internet management point, new management point [1] is: https://CONTOSO.CLOUDAPP.NET/CCM_Proxy_MutualAuth/72186325152220500 (0) with capabilities: <Capabilities SchemaVersion ="1.0"><Property Name="SSL" Version="1" /></Capabilities>
```

インストールのトラブルシューティングを行うには、クライアント上の `%WinDir%\ccmsetup\logs\ccmsetup.log` を確認します。 インストール後は、`%WinDir%\ccm\logs\ClientIDManagerStartup.log` を確認します。

サーバー上では、次のログを確認します。

- [CMG ログ](../../plan-design/hierarchy/log-files.md#cloud-management-gateway)
- 管理ポイント
  - CCM_STS.log
  - MP_RegistrationManager.log
  - ClientAuth.log

### <a name="known-issues"></a>既知の問題

パッシブ モードのサイト サーバーを持つサイトに一括登録トークンを作成することはできません。<!-- 6399087 -->

### <a name="bulk-registration-token-tool-usage"></a>一括登録トークン ツールの使用方法

`BulkRegistrationTokenTool.exe` ツールは、サイト サーバー上の Configuration Manager インストール ディレクトリの `\bin\X64` フォルダーにあります。 サイト サーバーにサインインし、管理者として実行します。 次のコマンド ライン パラメーターをサポートしています。

- `/?`
- `/new`
- `/lifetime`

#### <a name=""></a>/?

この使用法の情報を表示します。

例: `BulkRegistrationTokenTool.exe /?`

#### <a name="new"></a>/new

新規一括登録トークンを作成します。

例: `BulkRegistrationTokenTool.exe /new`

このツールには、次の情報が示されます。
  
- 発行されたトークンを追跡するためにサイトで使用される GUID
- トークンの有効期間。既定では 3 日間です。
- 一括登録トークン。

トークンは、クライアントおよびサイトには保管されません。 必ずコマンド プロンプトからトークンをコピーし、安全な場所に保存してください。

#### <a name="lifetime"></a>/lifetime

`/new` パラメーターと共に使用して、トークンの有効期間を指定します。 分単位の整数値を指定してください。 既定値は 4,320 (3 日) です。 最大値は 10,080 (7 日) です。

例: `BulkRegistrationTokenTool.exe /lifetime 4320`

## <a name="bulk-registration-token-management"></a>一括登録トークンの管理

Configuration Manager コンソールで以前に作成した一括登録トークンとその有効期間を確認し、必要に応じてその使用をブロックできます。 ただし、サイト データベースには一括登録トークンが保存されません。

### <a name="review-a-bulk-registration-token"></a>一括登録トークンを確認する

1. Configuration Manager コンソールで、 **[管理]** ワークスペースに移動します。

2. **[セキュリティ]** を展開し、 **[証明書]** ノードを選択します。 コンソールでは、サイトに関連した証明書と一括登録トークンのすべてが、詳細ウィンドウに一覧表示されます。

3. 確認する一括登録トークンを選択します。

**[種類]** 列でフィルター処理または並べ替えを行うことができます。 GUID に基づいて特定の一括登録トークンを識別できます。 一括登録トークンを作成すると、ツールに GUID が表示されます。

### <a name="block-a-bulk-registration-token"></a>一括登録トークンをブロックする

1. Configuration Manager コンソールで、 **[管理]** ワークスペースに移動します。

2. **[セキュリティ]** を展開し、 **[証明書]** ノードを選択して、ブロックする一括登録トークンを選択します。

3. リボン バーの **[ホーム]** タブか右クリックのコンテキスト メニューで、 **[ブロック]** を選択します。 以前にブロックした一括登録トークンをブロック解除するには、 **[ブロック解除]** アクションを選択します。

## <a name="token-renewal"></a>トークンの更新

Configuration Manager で発行される一意のトークンは、クライアントによって月に 1 回更新され、90 日間有効です。 トークンを更新するために、クライアントが内部ネットワークに接続する必要はありません。 トークンがまだ有効である限り、CMG を使用してサイトに接続するだけで十分です。 90 日以内にトークンが更新されない場合は、クライアントで内部ネットワーク上の管理ポイントに直接接続して、新しいトークンを受け取る必要があります。

一括登録トークンを更新することはできません。 一括登録トークンの有効期限が切れたら、CMG を使用して、インターネット ベースのデバイス登録用に新しいものを生成します。

## <a name="see-also"></a>関連項目

- [クラウド管理ゲートウェイの計画](../manage/cmg/plan-cloud-management-gateway.md)

- [認証のため Azure AD を使用して、Configuration Manager の Windows 10 クライアントをインストールして割り当てる](deploy-clients-cmg-azure.md)
