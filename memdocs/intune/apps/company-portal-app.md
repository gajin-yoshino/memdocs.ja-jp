---
title: Intune ポータル サイト アプリ、ポータル サイト Web サイト、および Intune アプリをカスタマイズする方法
titleSuffix: Microsoft Intune
description: Intune ポータル サイト アプリ、ポータル サイト Web サイト、および Intune アプリに会社固有のブランドを適用する方法について説明します。
keywords: ''
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 03/16/2020
ms.topic: conceptual
ms.service: microsoft-intune
ms.subservice: apps
ms.localizationpriority: high
ms.technology: ''
ms.assetid: dec6f258-ee1b-4824-bf66-29053051a1ae
ms.reviewer: esthermsft
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6a3152966dee507cde690d9be8f5a7e210c7945
ms.sourcegitcommit: e2877d21dfd70c4029c247275fa2b38e76bd22b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2020
ms.locfileid: "80407754"
---
# <a name="how-to-customize-the-intune-company-portal-apps-company-portal-website-and-intune-app"></a>Intune ポータル サイト アプリ、ポータル サイト Web サイト、および Intune アプリをカスタマイズする方法

Android 上のポータル サイト アプリ、ポータル サイト Web サイト、Intune アプリでは、ユーザーが会社のデータにアクセスし、一般的なタスクを実行できます。 一般的なタスクには、デバイスの登録、アプリのインストール、情報の検索 (IT 部門からのサポートを受けるためなど) が含まれます。 また、ユーザーは会社のリソースに安全にアクセスできます。 エンドユーザー エクスペリエンスには、[ホーム]、[アプリ]、[アプリの詳細]、[デバイス]、[デバイスの詳細] など、いくつかのページが用意されています。 ポータル サイト内のアプリをすばやく検索するため、[アプリ] ページでアプリをフィルター処理できます。

## <a name="customizing-the-user-experience"></a>ユーザー エクスペリエンスのカスタマイズ

エンドユーザー エクスペリエンスをカスタマイズすることで、エンド ユーザーに親しみやすく役立つエクスペリエンスを提供できます。 これを行うには、[Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431) に移動し、 **[テナント管理]**  >  **[カスタマイズ]** の順に選択し、必要な設定を構成します。 これらの設定は、Android 上のポータル サイト アプリ、ポータル サイト Web サイト、および Intune アプリに適用されます。

## <a name="branding"></a>ブランド化

エンドユーザー エクスペリエンスのブランド化のカスタマイズの詳細を次の表に示します。

| フィールド名 | 説明 |
|---|---|---|
| **組織名** | エンドユーザー エクスペリエンスのメッセージング全体でこの名前が表示されます。 **[ヘッダーに表示]** 設定を使用してヘッダーに表示するように設定することもできます。 最大長は 40 文字です。 |
| **色** | **[標準]** を選択して、5 つの標準色から選択します。 **[カスタム]** を選択して、16 進コード値に基づいて特定の色を選択します。 |
| **テーマの色** | エンドユーザー エクスペリエンス全体に表示するテーマの色を設定します。 テキストの色は、選択したテーマの色で最も見やすくなるように、自動的に黒または白に設定されます。 |
| **ヘッダーに表示** | エンドユーザー エクスペリエンスのヘッダーに**会社のロゴと名前**、**会社のロゴのみ**、または**会社の名前のみ**のいずれを表示するかを選択します。 以下のプレビュー ボックスには、名前ではなくロゴのみが表示されます。  |
| **テーマの色の背景のためのロゴをアップロードする** | 選択したテーマの色の上に表示するロゴをアップロードします。 最高の外観を実現するには、透明な背景のロゴをアップロードします。 設定の下にある [プレビュー] ボックスでどのように表示されるかを確認できます。<p>画像の最大サイズ:400 x 400 px<br>ファイルの最大サイズ: 750 KB<br>ファイルの種類:PNG、JPG、または JPEG |
| **白または明るい背景のためのロゴをアップロードする** | 白または明るい色の背景の上に表示するロゴをアップロードします。 最高の外観を実現するには、透明な背景のロゴをアップロードします。 これが白い背景でどのように見えるかは、設定の下のプレビュー ボックスで確認できます。<p>画像の最大サイズ:400 x 400 px<br>ファイルの最大サイズ:750 KB<br>ファイルの種類:PNG、JPG、または JPEG |
| **ブランド イメージをアップロードする** | 組織のブランドを反映した画像をアップロードします。<p><ul><li>推奨される画像の幅:1125 ピクセルより大きいサイズ (650 ピクセル以上にする必要があります)</li><li>画像の最大サイズ:1.3 MB</li><li>ファイルの種類:PNG、JPG、または JPEG</li><li>次の場所に表示されます。</li><ul><li>iOS/iPadOS ポータル サイト:ユーザーのプロファイル ページの背景画像。</li><li>ポータル サイト Web サイト: ユーザーのプロファイル ページの背景画像。</li><li>Android Intune アプリ:ユーザーのプロファイル ページのドロアー内と、背景画像として。</li></ul></ul> |

> [!NOTE]
> ユーザーがポータル サイトから iOS/iPadOS アプリケーションをインストールすると、プロンプトが表示されます。 これは、iOS/iPadOS アプリがアプリ ストアにリンクされている場合、ボリューム購入プログラム (VPP) にリンクされている場合、または基幹業務 (LOB) アプリにリンクされている場合に発生します。 プロンプトでは、ユーザーは操作を受け入れるか、アプリの管理を許可することができます。 プロンプトでは、会社名が表示されます。会社名が使用できない場合は、 **[ポータル サイト]** が表示されます。

### <a name="brand-image-best-practices"></a>ブランド イメージのベスト プラクティス

適切なブランド イメージを使用して、組織のブランド感を強く提示することで、ユーザーの信頼を高めることができます。 ここでは、表示場所に合わせて画像を取得、選択、最適化する場合に検討することができるヒントを紹介します。

- マーケティング部門またはアート部門に連絡します。 既に承認されている一連のブランド画像を持っている可能性があります。 また、画像を最適化する際に必要に応じてサポートを受けられる可能性もあります。
- 横長と縦長の両方の構図を検討します。 画像の中心を囲む十分な背景を用意するようにします。 画像は、デバイスのサイズ、向き、およびプラットフォームに基づいて、異なる方法でトリミングされることがあります。
- 汎用のストック画像は使用しないでください。 組織のブランドが反映され、ユーザーになじみのある画像にすることをお勧めします。 ない場合は、ユーザーにとって意味のない汎用的なものを使用するよりも、使用しないことをお勧めします。
- 不要なメタデータを削除します。 画像ファイルには、カメラ プロファイル、地理的位置、タイトル、キャプションなどのメタデータが含まれています。 画像の最適化ツールを使用して、この情報を削除し、ファイル サイズの制限を満たしながら品質を維持します。

### <a name="brand-image-examples"></a>ブランド画像の例

次の画像は、iPhone のブランド イメージの例を示しています。

<img alt="Screenshot of example iPhone branding image" src="./media/company-portal-app/company-portal-app-01.png" width="250">

Android 用 Intune アプリのブランド イメージの例を次に示します。

<img alt="Screenshot of example #1 for Intune app for Android branding image" src="./media/company-portal-app/company-portal-app-02.png" width="250">

<img alt="Screenshot of example #2 for Intune app for Android branding image" src="./media/company-portal-app/company-portal-app-03.png" width="250">

## <a name="support-information"></a>サポート情報

組織のサポート情報を入力して、従業員が質問できるようにします。 このサポート情報は、エンドユーザー エクスペリエンス全体の **[サポート]** 、 **[ヘルプとサポート]** 、および **[ヘルプデスク]** ページに表示されます。

| フィールド名 | 最大長 | 説明 |
|------------------------|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 連絡先の名前 | 40 | この名前は、ユーザーがサポートに連絡するときに表示される名前です。 |
| 電話番号 | 20 | ユーザーは、この番号に電話をかけてサポートを受けることができます。 |
| 電子メール アドレス | 40 | このメール アドレスは、ユーザーがサポートを受けるためにメールを送信できる先です。 `alias@domainname.com` の形式で有効な電子メール アドレスを入力する必要があります。 |
| Web サイト名 | 40 | 一部の場所で、サポート Web サイトの URL の代わりに表示されるフレンドリ名です。 サポート Web サイトの URL を指定し、フレンドリ名を指定しない場合、URL がそのままエンドユーザー エクスペリエンスに表示されます。  |
| Web サイトの URL | 150 | ユーザーが使用するサポート Web サイト。 URL は、`https://www.contoso.com` の形式である必要があります。  |
| 追加情報 | 120 | ユーザー向けのサポート関連の追加メッセージをここに含めます。 |

## <a name="configuration"></a>構成

その他の構成の詳細を次の表に示します。

| フィールド名 | 最大長 | 説明 |
|------------------------------------------------------|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| プライバシーに関する声明の URL | 79 | ユーザーがプライバシー リンクをクリックしたときに表示されるように、組織のプライバシーに関する声明を設定します。 `https://www.contoso.com` の形式で有効な URL を入力する必要があります。 |
| iOS/iPadOS のポータル サイトのプライバシー メッセージ | 520 | 既定値をそのまま使用するか、カスタム メッセージを設定し、組織で iOS/iPadOS のマネージド デバイス上に表示できるまたは表示できない項目を一覧表示します。 markdown を使用すると、箇条書き、太字、斜体、リンクを追加できます。 |
| デバイスの登録 | 該当なし | ユーザーにモバイル デバイス管理への登録を求めるかどうかと、その方法を指定します。 詳細は以下を参照してください。 |

### <a name="device-enrollment-setting-options"></a>デバイス登録設定のオプション

> [!NOTE]
> デバイス登録設定をサポートするには、エンド ユーザーに次のバージョンのポータル サイトが必要です。
> - iOS/iPadOS 上のポータル サイト: バージョン 4.4 以降
> - Android 上のポータル サイト: バージョン 5.0.4715.0 以降 

|    デバイス登録オプション    |    [説明]    |    チェックリストのプロンプト    |    通知    |    デバイスの詳細状態    |    アプリの (登録が必要なアプリの) 詳細状態    |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|--------------------|-----------------------------|--------------------------------------------------------------------|
|    使用可能、プロンプトあり    |    すべての可能な場所で登録を求める既定のエクスペリエンス。    |    はい    |    はい    |    はい    |    はい    |
|    使用可能、プロンプトなし    |    ユーザーは、現在のデバイスのデバイス詳細の状態から、または登録が必要なアプリから登録できます。    |    いいえ    |    いいえ    |    はい    |    はい    |
|    利用不可    |    ユーザーが登録する方法はありません。    |    いいえ    |    いいえ    |    いいえ    |    いいえ<sup>(1)</sup>    |

<sup>(1)</sup> **既知の問題:** インストールに登録を要求するようにアプリを設定し、デバイスの登録を [使用不可] に設定した場合でも、Android 上のポータル サイト アプリでは、ユーザーに登録が指示されます。 これは間もなく削除される予定です。

> [!NOTE]
> Azure Government を使用している場合、エンド ユーザーが問題に関するヘルプを入手するプロセスを開始すると、共有する方法を決定するためにアプリ ログが提供されます。 ただし、Azure Government を使用していない場合、ユーザーが問題に関するヘルプを入手するプロセスを開始すると、ポータル サイトでは Microsoft に直接アプリのログを送信するようになります。 Microsoft にアプリ ログを送信すると、トラブルシューティングと問題の解決がより簡単になります。

> [!NOTE]
> Microsoft と Apple のポリシーに従って、Microsoft では、いかなる理由でも、Microsoft のサービスによって収集されたデータを第三者に販売することはありません。

## <a name="company-portal-derived-credentials-for-iosipados-devices"></a>iOS/iPadOS デバイス用のポータル サイトの派生資格情報

Intune では、資格情報プロバイダーの DISA Purebred、Entrust Datacard、Intercede と連携して、Personal Identity Verification (PIV) と Common Access Card (CAC) 派生資格情報がサポートされています。 エンド ユーザーは、iOS/iPadOS デバイスの登録後に追加の手順を行い、ポータル サイト アプリケーションで自身の ID を確認します。 ユーザーに対して派生資格情報を有効にするには、最初にテナントに対して資格情報プロバイダーを設定してから、派生資格情報を使用するプロファイルをユーザーまたはデバイスに対して設定します。

> [!NOTE]
> ユーザーには、Intune を使用して指定したリンクに基づいて、派生資格情報に関する指示が表示されます。

iOS/iPadOS デバイスの派生資格情報の詳細については、「[Microsoft Intune で派生資格情報を使用する](../protect/derived-credentials.md)」を参照してください。

## <a name="dark-mode-for-iosipados-company-portal"></a>iOS/iPadOS ポータル サイトのダーク モード

iOS/iPadOS ポータル サイトでダーク モードを使用できます。 ユーザーは、デバイス設定に基づく任意の配色で、アプリをダウンロードし、デバイスを管理し、IT サポートを受けることができます。 iOS/iPadOS ポータル サイトは、ダーク モードまたはライト モードについて、エンド ユーザーのデバイス設定に自動的に一致します。

## <a name="windows-company-portal-keyboard-shortcuts"></a>Windows ポータル サイトのキーボード ショートカット

エンドユーザーは、Windows ポータル サイト内でキーボード ショートカット (アクセラレータ) を使用して、ナビゲーション、アプリおよびデバイスのアクションをトリガーできます。

Windows ポータル サイト アプリで使用できるキーボード ショートカットを次に示します。

| 領域 | [説明] | ショートカット キー |
|:------------------:|:--------------:|:-----------------:|
| [ナビゲーション] メニュー | ナビゲーション | Alt + M |
|  | Home | Alt + H |
|  | すべてのアプリ | Alt + A |
|  | インストール済みのアプリ | Alt + I |
|  | フィードバックを送信する | Alt + F |
|  | 個人用プロファイル | Alt + U |
|  | Settings | Alt + T |
| [ホーム] - [デバイス] タイル | ［名前の変更］ | F2 |
|  | 削除 | Ctrl + D または Delete |
|  | アクセスの確認 | Ctrl + M または F9 |
| デバイスの詳細 | ［名前の変更］ | F2 |
|  | 削除 | Ctrl + D または Delete |
|  | アクセスの確認 | Ctrl + M または F9 |
| アプリの詳細 | [インストール] | Ctrl + I |
| デバイス | 利用可能 | Ctrl + D |

エンドユーザーは、Windows ポータル サイトのアプリ内で使用できるショートカットを表示させることもできます。

![Windows ポータル サイト内で使用できるショートカットのスクリーンショット](./media/company-portal-app/company-portal-app-04.png)

## <a name="user-self-service-device-actions-from-the-company-portal"></a>ポータル サイトからのユーザーによるセルフサービス デバイス アクション

ユーザーは、Android 上のポータル サイト アプリまたは Web サイト、または Intune アプリを使用して、ローカルまたはリモートのデバイス上でアクションを実行できます。 ユーザーが実行できるアクションは、デバイスのプラットフォームと構成によって異なります。 どのような場合でも、リモート デバイス アクションを実行できるのはデバイスのプライマリ ユーザーだけです。

- **[Retire]\(インベントリから削除\)** – デバイスを Intune による管理から削除します。 ポータル サイト アプリと Web サイトでは、これは **[削除]** と表示されます。
- **[ワイプ]** – このアクションでは、デバイスのリセットが開始されます。 ポータル サイト Web サイトではこれは **[リセット]** と表示され、iOS/iPadOS ポータル サイト アプリでは **[出荷時の設定にリセット]** と表示されます。
- **[名前の変更]** – このアクションでは、ユーザーがポータル サイトで見ることのできるデバイス名が変更されます。 ローカル デバイス名は変更されず、ポータル サイトの一覧表示だけです。
- **[同期]** – このアクションでは、Intune サービスでのデバイスのチェックインが開始されます。 これは、ポータル サイトでは **[Check Status]\(状態の確認\)** と表示されます。
- **[リモート ロック]** – デバイスがロックされます。ロックを解除するには PIN が必要です。
- **[パスコードのリセット]** – このアクションは、デバイスのパスコードをリセットするために使用されます。 iOS/iPadOS デバイスでは、パスコードが削除され、エンド ユーザーは設定で新しいコードを入力する必要があります。 サポートされている Android デバイスでは、新しいパスコードが Intune によって生成され、一時的にポータル サイトに表示されます。
- **[キー回復]** - このアクションは、暗号化された macOS デバイスの個人用回復キーをポータル サイト Web サイトから回復するために使用されます。 

### <a name="self-service-actions"></a>セルフサービス アクション

一部のプラットフォームおよび構成では、セルフサービス デバイス アクションは許可されていません。 次の表では、セルフサービス アクションについての詳細を示します。

|  | Windows 10<sup>(3)</sup> | iOS/iPadOS<sup>(3)</sup> | MacOS<sup>(3)</sup> | Android<sup>(3)</sup> |
|----------------------|--------------------------|-------------------|-----------------------------------|-------------------------|
| インベントリから削除 | 利用可能<sup>(1)</sup> | 利用可能 | 利用可能 | 利用可能<sup>(7)</sup> |
| ワイプ | 利用可能 | 利用可能<sup>(5)</sup> | N/A | 利用可能<sup>(7)</sup> |
| 名前の変更 <sup>(4)</sup> | 利用可能 | 利用可能 | 利用可能 | 利用可能 |
| 同期 | 利用可能 | 利用可能 | 利用可能 | 利用可能 |
| リモート ロック | Windows Phone のみ | 利用可能 | 利用可能 | 利用可能 |
| パスコードのリセット | Windows Phone のみ | 使用可能<sup>(8)</sup> | N/A | 利用可能<sup>(6)</sup> |
| キー回復 | N/A | N/A | 利用可能<sup>(2)</sup> | N/A |

<sup>(1)</sup> Azure AD に参加している Windows デバイスでは、**インベントリからの削除**は常にブロックされます。<br>
<sup>(2)</sup> MacOS の**キー回復**は、Web ポータルでのみ使用できます。<br>
<sup>(3)</sup> デバイス登録マネージャーの登録を使用している場合は、すべてのリモート アクションが無効になります。<br>
<sup>(4)</sup>**名前の変更**では、ポータル サイト アプリまたは Web ポータル内のデバイス名が変更されるだけで、デバイス上では変更されません。<br>
<sup>(5)</sup>**ワイプ**は、ユーザーが登録した iOS/iPadOS デバイス上では利用できません。<br>
<sup>(6)</sup>**パスコードのリセット**は、一部の Android および Android Enterprise の構成ではサポートされません。 詳しくは、「[Intune でデバイスのパスコードをリセットまたは削除する](../remote-actions/device-passcode-reset.md)」をご覧ください。<br>
<sup>(7)</sup>**インベントリからの削除**と**ワイプ**は、Android Enterprise デバイス所有者のシナリオでは使用できません (COPE、COBO、COSU)。<br> 
<sup>(8)</sup>**パスコードのリセット**は、ユーザーが登録した iOS/iPadOS デバイス上ではサポートされません。

## <a name="next-steps"></a>次のステップ

- [アプリを追加する](apps-add.md)