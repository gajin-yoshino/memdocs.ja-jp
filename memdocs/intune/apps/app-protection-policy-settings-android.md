---
title: Android アプリ保護ポリシー設定
titleSuffix: Microsoft Intune
description: このトピックでは、Android デバイスのアプリ保護ポリシーの設定について説明します。
keywords: ''
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 02/28/2020
ms.topic: conceptual
ms.service: microsoft-intune
ms.subservice: apps
ms.localizationpriority: high
ms.technology: ''
ms.assetid: 9e9ef9f5-1215-4df1-b690-6b21a5a631f8
ms.reviewer: demerson
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5dbe2abf7a23be9f9e0051a2ff26590e749f98c
ms.sourcegitcommit: 3d895be2844bda2177c2c85dc2f09612a1be5490
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79341933"
---
# <a name="android-app-protection-policy-settings-in-microsoft-intune"></a>Microsoft Intune の Android アプリ保護ポリシー設定
この記事では、Android デバイスのアプリ保護ポリシーの設定について説明します。 説明されているポリシーの設定は、Azure portal の **[設定]** ウィンドウでアプリ保護ポリシー用に[構成](app-protection-policies.md)することができます。
ポリシーの設定には、"データ保護設定"、"アクセス要件"、"条件付き起動" の 3 つのカテゴリがあります。 この記事の "*ポリシーで管理されているアプリ*" という用語は、アプリ保護ポリシーで構成されるアプリを示します。

> [!IMPORTANT]
> Android デバイスでアプリ保護ポリシーを受信するには、デバイスに Intune ポータル サイトが必要です。 詳細については、[Intune ポータル サイト アクセス アプリの要件](../fundamentals/end-user-mam-apps-android.md)に関するページを参照してください。

## <a name="data-protection"></a>データの保護 
### <a name="data-transfer"></a>データ転送
| 設定 | 使用する方法 | 既定値 |
|------|------|------|
| **Android バックアップ サービスに組織データをバックアップ** | このアプリが職場や学校のデータを [Android バックアップ サービス](https://developer.android.com/google/backup/index.html)にバックアップすることを禁止する場合、 **[ブロック]** を選択します。<br><br> このアプリが職場や学校のデータをバックアップすることを許可する場合、 **[許可]** を選択します。| **許可** |
| **組織のデータを他のアプリに送信** | このアプリからデータを受け取ることができるアプリを指定します。 <ul><li> **ポリシーで管理されているアプリ**:他のポリシーで管理されているアプリへの転送のみを許可します。</li> <li>**[すべてのアプリ]** :すべてのアプリへの転送を許可します。 </li> <li>**[なし]** :他のポリシーで管理されているアプリを含め、アプリへのデータ転送を許可しません。</li></ul> <p>Intune でデータ転送先として既定で許可される可能性のある除外対象アプリとサービスがいくつかあります。 さらに、Intune アプリをサポートしていないアプリへのデータ転送を許可する必要がある場合、独自の例外を作成できます。 詳細については、「[データ転送の除外対象](app-protection-policy-settings-android.md#data-transfer-exemptions)」を参照してください。<p>このポリシーは、Android アプリ リンクにも該当する場合があります。  一般的な Web リンクは、 **[Intune Managed Browser でアプリ リンクを開く]** ポリシー設定によって管理されます。<p><div class="NOTE"><p>メモ</p><p>Intune は現在のところ、Android Instant Apps 機能に対応していません。 Intune はこのアプリとのデータ接続をすべてブロックします。 詳細については、Android 開発者向けドキュメントの [Android Instant Apps](https://developer.android.com/topic/instant-apps/index.html) に関するページを参照してください。</p><p>**[他のアプリに組織データを送信]** が **[すべてのアプリ]** に構成されているデータは引き続き OS の共有を経由してクリップボードに転送できます。</p></div> | **すべてのアプリ** | 
|<ul><ui>**除外するアプリを選択します** | このオプションは、上記のオプションで *[ポリシーで管理されているアプリ]* を選択した場合に使用できます。 | |
|<ul><ui>**組織データのコピーを保存** | このアプリで [名前を付けて保存] オプションの使用を無効にするには、 **[ブロック]** を選択します。 [名前を付けて保存] の使用を許可する場合は、 **[許可]** を選択します。 **注:** *この設定は、Microsoft Excel、OneNote、PowerPoint、および Word でサポートされています。サード パーティ アプリと LOB アプリでもサポートされている場合があります*。| **許可** |  
|<ul><ui>**選択したサービスにユーザーがコピーを保存することを許可** |ユーザーは、選択したサービス (OneDrive for Business、SharePoint、ローカル ストレージ) に保存できます。 他のすべてのサービスはブロックされます。  | **選択済み 0** |
| **他のアプリからデータを受信** | このアプリにデータを転送できるアプリを以下のように指定します。 <ul><li>**[ポリシーで管理されているアプリ]** :他のポリシーで管理されているアプリからの転送のみを許可します。</li><li>**すべてのアプリ**:すべてのアプリからのデータ転送を許可します。</li><li>**なし**: 他のポリシーで管理されているアプリを含め、アプリからのデータ転送を許可しません。 </li></ul> <p>一部の除外対象アプリとサービスは、Intune でデータ転送元として許可される可能性があります。 アプリとサービスの完全な一覧については、「[データ転送の除外対象](app-protection-policy-settings-android.md#data-transfer-exemptions)」を参照してください。 | **すべてのアプリ** |
|<ul><ui>**データを開いて組織ドキュメントに読み込む** | このオプションは、上記のオプションで *[ポリシーで管理されているアプリ]* を選択した場合に使用できます。 次の中から選択します。 <ul><li>**[ブロック]** :このアプリで、アカウント間でデータを共有するための *[開く]* オプションやその他のオプションの使用を無効にします。 </li><li>**[許可]** :このアプリで、アカウント間でデータを共有するための *[開く]* オプションやその他のオプションの使用を許可します。||
|<ul><ui>**選択したサービスからデータを開くことをユーザーに許可する** | このオプションは、上記のオプションで *[ブロック]* を選択した場合に使用できます。 ユーザーがデータを開くことができるアプリケーション ストレージ サービスを選択します。 他のすべてのサービスはブロックされます。 サービスが選択されていないと、ユーザーはデータを開けません。| |
| **他のアプリとの間で切り取り、コピー、貼り付けを制限する** | このアプリで切り取り、コピー、および貼り付け操作をいつ使用できるようにするかを指定します。 次の中から選択します。 <ul><li>**[ブロック]** :このアプリと他のアプリの間で、切り取り、コピー、および貼り付け操作を許可しません。</li><li>**ポリシーで管理されているアプリ**:このアプリと他のポリシーで管理されているアプリ間で、切り取り、コピー、および貼り付け操作を許可します。</li><li>**貼り付けを使用する、ポリシーで管理されているアプリ**:このアプリと他のポリシーで管理されているアプリ間で切り取りまたはコピーを許可します。 任意のアプリからこのアプリへのデータの貼り付けを許可します。</li><li>**任意のアプリ**:このアプリに対する切り取り、コピー、貼り付けに制限はありません。 | **任意のアプリ** |
| <ul><ui>**あらゆるアプリの切り取りとコピーの文字制限** | 組織のデータとアカウントから切り取りまたはコピーできる文字数を指定します。  これで、指定した文字数を共有できます。これを指定しないと、[他のアプリとの間で切り取り、コピー、貼り付けを制限する] の設定によってブロックされます。<p>既定値 = 0<p>**注**:Intune ポータル サイト バージョン 5.0.4364.0 以降が必要です。  | **0** |
| **Screen capture and Google Assistant (スクリーン キャプチャと Google アシスタント)** | **[ブロック]** を選択すると、このアプリを使用するときに、デバイスのスクリーン キャプチャ機能と **Google Assistant** 機能がブロックされます。 また、 **[許可]** を選択すると、職場または学校アカウントでこのアプリを使用するときに、使用中のアプリ一覧プレビュー イメージがぼかし表示になります。| **ブロック** |
| **承認済みキーボード**  | *[必要]* を選択してから、このポリシーに対して承認されているキーボードの一覧を指定します。 <p>承認済みキーボードを使用していないユーザーは、保護されたアプリを使用するには、承認済みキーボードをダウンロードしてインストールするように求められます。 この設定では、アプリに Intune SDK for Android バージョン6.2.0 以降が必要です。 | **必要なし** |
| <ul><ui>**承認するキーボードの選択** | このオプションは、上記のオプションで *[必要]* を選択した場合に使用できます。 このポリシーで保護されているアプリで使用できるキーボードと入力方法の一覧を管理するには、 *[選択]* を選びます。 一覧にキーボードを追加することも、既定のオプションのいずれかを削除することもできます。 設定を保存するには、承認済みキーボードが少なくとも 1 つ必要です。 キーボードを追加するには、次のように指定します。 <ul><li>**名前**:キーボードを識別し、ユーザーに表示されるフレンドリ名。 </li><li>**パッケージ ID**:Google Play ストアでのアプリのパッケージ ID。 たとえば、Play ストアでのアプリの URL が `https://play.google.com/store/details?id=com.contoskeyboard.android.prod` の場合、パッケージ ID は `com.contosokeyboard.android.prod` になります。 このパッケージ ID は、Google Play からキーボードをダウンロードするための簡単なリンクとしてユーザーに提示されます。 <p><div class="NOTE"><p>メモ</p><p>複数のアプリ保護ポリシーが割り当てられているユーザーは、すべてのポリシーに共通する承認済みキーボードのみを使用できます。</p> | |

### <a name="encryption"></a>暗号化
| 設定 | 使用する方法 | 既定値 |
|------|------|------|
| **組織データを暗号化** | **[必要]** を選択すると、このアプリの職場や学校のデータを暗号化できます。 Intune では、Android キーストア システムと共に OpenSSL の 256 ビット AES 暗号化スキームを使用して、アプリ データを安全に暗号化します。 データは、ファイル I/O タスク中に同期的に暗号化されます。 デバイス ストレージ上のコンテンツは常に暗号化されます。 新しいファイルは、256 ビット キーで暗号化されます。 既存の 128 ビット暗号化ファイルは、256 ビット キーへの移行が試みられますが、プロセスは保証されません。 128 ビット キーで暗号化されたファイルは、読み取り可能な状態で残されます。 <br><br> 暗号化方法は FIPS 140-2 で検証されます。詳細については、「[OpenSSL FIPS ライブラリと Android ガイド](https://wiki.openssl.org/images/7/76/OpenSSL_FIPS_Library_and_Android_Guide.pdf)」を参照してください。     |  **必要**|  
| <ul><ui>**登録済みデバイスで組織データを暗号化** | **[必要]** を選択すると、すべてのデバイス上で、Intune アプリ層の暗号化を使った組織データの暗号化が適用されます。 **[不要]** を選択すると、登録されたデバイス上での Intune アプリ層の暗号化を使った組織データの暗号化が適用されません。| **必要** |


### <a name="functionality"></a>機能
| 設定 | 使用する方法 | 既定値 |
|------|------|------|
| **アプリとネイティブ連絡先アプリを同期** | **[ブロック]** を選択すると、アプリでデバイス上のネイティブ連絡先アプリにデータを保存できなくなります。 **[許可]** を選択した場合は、アプリでデバイス上のネイティブ連絡先アプリにデータを保存できます。 <br><br>選択的ワイプを実行し、アプリから職場または学校のデータを削除すると、アプリからネイティブ連絡先アプリに直接同期されている連絡先が削除されます。 ネイティブ アドレス帳から別の外部ソースに同期された連絡先はワイプできません。 現在、これは Microsoft Outlook アプリにのみ適用されます。 | **許可** |
| **組織データを出力する** | **[ブロック]** を選択すると、アプリで職場または学校のデータを印刷できなくなります。 この設定を **[許可]** (規定値) のままにした場合、ユーザーはすべての組織データをエクスポートおよび印刷できるようになります。 | **許可** |
|**その他のアプリでの Web コンテンツの転送を制限する** | ポリシーで管理されているアプリケーションから Web コンテンツ (http/https リンク) をどのように開くか指定します。 次の中から選択します。 <ul><li>**任意のアプリ**:任意のアプリで Web リンクを許可します。</li><li>**Intune Managed Browser**:Intune Managed Browser でのみ Web コンテンツを開くことを許可します。 このブラウザーは、ポリシーで管理されたブラウザーです。</li><li>**Microsoft Edge**:Microsoft Edge でのみ Web コンテンツを開くことを許可します。 このブラウザーは、ポリシーで管理されたブラウザーです。</li><li>**アンマネージド ブラウザー**: **[アンマネージド ブラウザー プロトコル]** 設定で定義されたアンマネージド ブラウザーのみで Web コンテンツを開くことを許可します。 Web コンテンツは、対象のブラウザーで管理されなくなります。<br>**注**:Intune ポータル サイト バージョン 5.0.4415.0 以降が必要です。</li><br><br>**ポリシーで管理されているブラウザー**<br>Android で Intune Managed Browser も Microsoft Edge もインストールされていない場合、http/https リンクをサポートするポリシーで管理されているその他のアプリを、エンド ユーザーが選択することができます。<p>ポリシーで管理されているブラウザーが必要であるにもかかわらず、インストールされていない場合、エンド ユーザーには Microsoft Edge のインストールが求められます。<p>ポリシーで管理されているブラウザーが必要な場合、Android アプリ リンクは **[アプリで他のアプリへのデータ転送を許可する]** ポリシー設定で管理されます。<p>**Intune デバイスの登録**<br>デバイスの管理に Intune を使用している場合は、「[Managed Browser のポリシーを使用したインターネット アクセスを Microsoft Intune で管理する](app-configuration-managed-browser.md)」をご参照ください。<p>**ポリシーで管理されている Microsoft Edge**<br>モバイル デバイス (iOS/iPadOS および Android) 向けの Microsoft Edge ブラウザーで、Intune アプリ保護ポリシーがサポートされます。 企業の Azure AD アカウントで Microsoft Edge ブラウザー アプリケーションにサインインしたユーザーは、Intune によって保護されます。 Microsoft Edge ブラウザーは APP SDK を組み込み、そのデータ保護ポリシーをすべてサポートします。ただし、例外として次が禁止されます。<br><ul><li>**名前を付けて保存**:Microsoft Edge ブラウザーでは、クラウド ストレージ プロバイダー (OneDrive など) にアプリ内で直接接続を追加することをユーザーに許可していません。</li><li>**連絡先の同期**:Microsoft Edge ブラウザーは、ネイティブの連絡先一覧に保存しません。</li></ul>**注:** *APP SDK では、対象のアプリがブラウザーであるかどうかは判別できません。Android デバイスでは、http/https インテントをサポートするその他の管理されたブラウザー アプリが許可されます*。 | **未構成** |
|<ul><ui>**アンマネージド ブラウザー ID** | 1 つのブラウザーのアプリケーション ID を入力します。 ポリシーで管理されているアプリケーションの Web コンテンツ (http/https リンク) は、指定されたブラウザーで開かれます。  Web コンテンツは、対象のブラウザーで管理されなくなります。 | **空白** |
|<ul><ui>**アンマネージド ブラウザー名** | **アンマネージド ブラウザー ID** に関連付けられているブラウザーのアプリケーション名を入力します。 指定されたブラウザーがインストールされていない場合は、この名前がユーザーに表示されます。  | **空白** |
| **組織のデータ通知** | 組織のアカウントに関して、OS 通知経由でどのくらいの組織データを共有するか指定します。 このポリシー設定は、ローカル デバイスと、ウェアラブルやスマート スピーカーなど、接続されているあらゆるデバイスに影響を与えます。 アプリにより、通知動作をカスタマイズする目的で制御が追加されたり、すべての値が無視されたりすることがあります。 次の中から選択します。 <ul><li>**[ブロック]** :通知を共有しません。</li><ul><li>アプリケーションでサポートされていない場合、通知は許可されます。</li></ul><li>**組織データをブロックする**:通知で組織データを共有しません。 例: "新着メールがあります"、"会議が予定されています"</li><UL><li>アプリケーションでサポートされていない場合、通知はブロックされます。</li></ul><li>**[許可]** :通知で組織データを共有します。</li></ul> <p>**注**:*この設定には、アプリのサポートが必要です。現時点では、Android 用 Outlook 4.95.0 以降でこの設定がサポートされます。これは 2019 年 12 月 16 日の週にリリースされる予定です。* | **許可**   |

## <a name="data-transfer-exemptions"></a>データ転送の除外対象

Intune アプリ保護ポリシーによってデータ転送が許可される除外対象のアプリとプラットフォーム サービスがいくつかあります。 たとえば、Android 上のすべての Intune 管理対象アプリは、モバイル デバイス画面のテキストを読み上げることができるように、Google テキスト読み上げとの間でデータをやり取りできる必要があります。 このリストは、セキュリティで保護された生産性向上に役立つと考えられるサービスとアプリを表しており、変更される可能性があります。

### <a name="full-exemptions"></a>完全除外対象

  これらのアプリとサービスは、Intune で管理されたアプリとの間のデータ転送を完全に許可されます。

  |アプリ/サービス名 | [説明] |
  | ------ | ---- |
  | com.android.phone | ネイティブの電話アプリ
  | com.android.vending | Google Play ストア |
  | com.android.documentsui | Android ドキュメント ピッカー|
  | com.google.android.webview | Outlook を含む多くのアプリに必要な [WebView](https://developer.android.com/reference/android/webkit/WebView.html)。 |
  | com.android.webview |Outlook を含む多くのアプリに必要な [WebView](https://developer.android.com/reference/android/webkit/WebView.html)。|
  | com.google.android.tts | Google テキスト読み上げ |
  | com.android.providers.settings | Android システム設定 |
  | com.android.settings | Android システム設定 |
  | com.azure.authenticator | 多くのシナリオで認証の成功に必要な Azure Authenticator アプリ。 |
  | com.microsoft.windowsintune.companyportal | Intune [ポータル サイト]|

### <a name="conditional-exemptions"></a>条件付き除外対象
  これらのアプリとサービスは、特定の条件下でのみ Intune で管理されたアプリとの間のデータ転送を許可されます。

  |アプリ/サービス名 | [説明] | 除外条件|
  | ------ | ---- | --- |
  | com.android.chrome | Google Chrome ブラウザー | Chrome は、Android 7.0 以降の一部の WebView コンポーネントで使用され、ビューから非表示になることはありません。 ただし、アプリとの間のデータ フローは常に制限されます。  |
  | com.skype.raider | Skype | Skype アプリは、電話を呼び出す特定のアクションでのみ使用できます。 |
  | com.android.providers.media | Android のメディア コンテンツ プロバイダー | メディア コンテンツ プロバイダーは、着信音の選択アクションでのみ使用できます。 |
  | com.google.android.gms、com.google.android.gsf | Google Play Services パッケージ | これらのパッケージは、プッシュ通知など、Google Cloud Messaging のアクションで使用できます。 |
  | com.google.android.apps.maps | Google Maps | ナビゲーションで住所が許可されます |

詳細については、「[Data transfer policy exceptions for apps](app-protection-policies-exception.md)」(アプリのデータ転送ポリシーの例外) を参照してください。

## <a name="access-requirements"></a>アクセス要件

| 設定 | 使用方法 |  
|------|------| 
| **アクセスに PIN を使用** | **[必要]** を選択すると、このアプリを使用する際に PIN が要求されます。 ユーザーは、職場または学校のコンテキストでアプリを初めて実行するときに、この PIN のセットアップを求められます。 <br><br> 既定値 = **必要**<br><br> [アクセスに PIN を使用] セクションの下の使用可能な設定を使用して、PIN 強度を構成できます。
| <ul><ui>**PIN の種類** | アプリ保護ポリシーが適用されているアプリにアクセスする前に、数値またはパスコードのどちらの種類の PIN を入力する必要があるかを設定します。 数値の場合は数字だけですが、パスコードの場合は少なくとも 1 つのアルファベット**または**少なくとも 1 つの特殊文字で定義できます。 <br><br> 既定値 = **数値**<br><br> **注:** 許可されている特殊文字には、Android 英語キーボードの特殊文字と記号が含まれます。 |
| <ul><ui> **単純な PIN** | **[許可]** を選択すると、ユーザーは *1234*、*1111*、*abcd*、*aaaa* などの単純な PIN シーケンスを使えるようになります。 **[ブロック]** を選択すると、単純なシーケンスは使用できなくなります。 単純なシーケンスは、3 文字のスライディング ウィンドウで確認されます。 **[ブロック]** に構成した場合、エンド ユーザーによって設定される PIN として 1235 または 1112 は受け付けられませんが、1122 は許可されます。 <br><br>既定値 = **許可** <br><br>**注:** PIN の種類としてパスコードが構成されていて、[単純な PIN] が [許可] に設定されている場合、ユーザーは少なくとも 1 つの文字**または**少なくとも 1 つの特殊文字を PIN に使用する必要があります。 PIN の種類としてパスコードが構成されていて、[単純な PIN] が [ブロック] に設定されている場合は、少なくとも 1 つの数字**かつ** 1 つの文字**かつ**少なくとも 1 つの特殊文字を、PIN で使用する必要があります。 </li> |
| <ul><ui> **PIN の最小長を選択** | PIN シーケンスの最小桁数を指定します。 <br><br>既定値 = **4** | 
| <ul><ui> **アクセスに PIN ではなく指紋を使用 (Android 6.0 以降)** | **[許可]** を選択すると、ユーザーは PIN の代わりに[指紋認証](https://developer.android.com/about/versions/marshmallow/android-6.0.html#fingerprint-authentication)を使用してアプリにアクセスできるようになります。 <br><br>既定値 = **許可** <br><br>**注:** この機能では、Android デバイスでの生体認証用の汎用的な制御がサポートされています。 Samsung Pass のような OEM 固有の生体認証設定は、"*サポートされていません*"。 <br><br>Android では、PIN の代わりに [Android の指紋認証](https://developer.android.com/about/versions/marshmallow/android-6.0.html#fingerprint-authentication)を使用して、ユーザーの身元を確かめることができます。 ユーザーが職場または学校のアカウントでこのアプリを使用しようとすると、PIN を入力する代わりに指紋認証を求められます。 <br><br> Android 仕事用プロファイルの登録デバイスでは、 **[アクセスに PIN ではなく指紋を使用]** ポリシーを適用するために別の指紋を登録する必要があります。 このポリシーは、Android 仕事用プロファイルにインストールされているポリシーで管理されているアプリの場合にのみ有効です。 ポータル サイトに登録して Android 仕事用プロファイルを作成した後に、別の指紋をデバイスに登録する必要があります。 Android 仕事用プロファイルを使用する仕事用プロファイルの指紋の詳細については、「[仕事用プロファイルのロック](https://support.google.com/work/android/answer/7029958)」を参照してください。 |
| <ul><ui>**Override fingerprint with PIN after timeout (タイムアウト後に指紋を PIN でオーバーライド)**| この設定を使用するには、 **[必要]** を選択し、非アクティブ タイムアウトを構成します。 <br><br>既定値 = **必要** |
| <ul><ui><ul><ui> **タイムアウト (非アクティブ分数)**| 時間を分単位で指定します。この時間を過ぎると、指紋の代わりにパスコード、数値のいずれか (構成のとおり) の PIN が使用されます。 このタイムアウト値は、[(非アクティブ分数) 後にアクセス要件を再確認する] に指定した値よりも大きい必要があります。<br><br>既定値 = **30** |
| <ul><ui>**PIN をリセットするまでの日数** | **[はい]** を選択すると、ユーザーは設定された期間の経過後にアプリの PIN を変更する必要があります。  <br><br>*[はい]* に設定した場合は、PIN のリセットが要求されるまでの日数を構成します。 <br><br> 既定値 = **いいえ**。  |  
| <ul><ui><ul><ui> **日数** | PIN のリセットが要求されるまでの日数を構成します。 <br><br> 既定値 = **90**  |
| <ul><ui>**維持対象の以前の PIN 値の数を選択する** | この設定では、Intune が維持する以前の PIN の数を指定します。 新しい PIN は、それらの Intune で維持されているものとは別のものにする必要があります。 <br><br> 既定値 = **0** | 
| <ul><ui>**デバイスの PIN が設定されている場合のアプリ PIN** | ポータル サイトが構成されている登録済みデバイスでデバイス ロックが検出された場合にアプリの PIN を無効にするには、 **[必要なし]** を選択します。 <br><br> 既定値 = **必要**。 | 
| **アクセスに職場または学校アカウントの資格情報を使用** | **[必要]** を選択すると、ユーザーは、アプリへのアクセスに、PIN を入力する代わりに職場または学校のアカウントでのサインインが求められます。 これを **[必要]** に設定し、PIN または生体認証プロンプトが有効になっている場合は、会社の資格情報と、PIN または生体認証プロンプトの両方が表示されます。 <br><br>既定値 = **必要なし** |
| **(非アクティブ分数) 後にアクセス要件を再確認する** | 次の設定を構成します。 <ul><li>**[タイムアウト]** :これは、(以前にポリシーで定義した) アクセス要件が再確認されるまでの分数です。 たとえば、管理者がポリシーで PIN をオンにしてルート化されたデバイスをブロックする、ユーザーが Intune で管理されているアプリを開く、PIN を入力しなければならない、およびルート化されていないデバイスでアプリを使用していなければならないなどです。 この設定を使用する場合、ユーザーはすべての Intune で管理されているアプリで、構成されている値と同じ期間、PIN を入力したり、別のルート検出チェックを行ったりする必要はありません。  <br><br>このポリシー設定の形式は、正の整数をサポートします。 <br><br> 既定値 = **30 分** <br><br> **注:** Android では、Intune で管理されたすべてのアプリで PIN が共有されます。 デバイスで、アプリがフォアグラウンドを離れると、PIN のタイマーがリセットされます。 この設定で定義されているタイムアウトまでの間は、PIN を共有する Intune で管理されたすべてのアプリで、PIN を入力する必要はありません。 <br><br></li> |

> [!NOTE]  
> [アクセス] セクションで同じアプリとユーザーの組み合わせに対して構成された複数の Intune アプリ保護設定が Android 上でどのように動作するかについては、[Intune MAM についてよく寄せられる質問](mam-faq.md)に関するページおよび「[Intune でアプリ保護ポリシーのアクセス アクションを利用し、データを選択的にワイプする](app-protection-policies-access-actions.md)」を参照してください。


## <a name="conditional-launch"></a>条件付き起動
条件付き起動設定を構成し、アクセス保護ポリシーのサインイン セキュリティ要件を設定します。 

既定では、値とアクションが事前構成された設定がいくつか提供されます。 *[OS の最小バージョン]* など、一部の設定を削除できます。 **[1 つ選んでください]** ドロップダウンから追加の設定を選択することもできます。 

| 設定 | 使用方法 |  
|---------|------------| 
| **PIN の最大試行回数** | PIN の入力試行回数を指定します。成功せずにこの回数を超えると、構成されているアクションが実行されます。 このポリシー設定の形式は、正の整数をサポートします。 *"アクション"* に含まれている項目: <br><ul><li>**[PIN のリセット]** - ユーザーは自分の PIN をリセットする必要があります。</li></ul> <ul><li>**[データのワイプ]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。  </li></ul> </li></ul> 既定値 = **5** |
| **オフラインの猶予期間** | MAM アプリをオフラインで実行できる時間 (分)。 アプリのアクセス要件を再確認するまでの時間 (分) を指定します。 *"アクション"* に含まれている項目: <br><ul><li>**[アクセスのブロック (分)]** : MAM アプリをオフラインで実行できる時間 (分)。 アプリのアクセス要件を再確認するまでの時間 (分) を指定します。 この期間が終わると、アプリが実行を継続できるように、Azure Active Directory (Azure AD) に対するユーザー認証が求められます。 <br><br>このポリシー設定の形式は、正の整数をサポートします。 <br><br>既定値 = **720** 分 (12 時間) </li></ul> <ul><li>**[データをワイプ (日)]** - オフラインでの実行期間がこの日数 (管理者が定義) を超えると、アプリはユーザーに対してネットワークへの接続と再認証を要求します。 ユーザーが正常に認証された場合、引き続きデータにアクセスでき、オフライン間隔がリセットされます。  ユーザーの認証が失敗した場合、アプリはユーザーのアカウントとデータの選択的ワイプを実行します。 詳細については、「[Intune で管理されているアプリから会社のデータをワイプする方法](apps-selective-wipe.md)」を参照してください。</li></ul> このポリシー設定の形式は、正の整数をサポートします。 <br><br>  既定値 = **90 日** </li></ul> <br><br>  このエントリは複数回表示できます。インスタンスごとに異なるアクションがサポートされます。 |   
| **脱獄またはルート化されたデバイス** |この設定に設定する値はありません。 *"アクション"* に含まれている項目: <br><ul><li>**[アクセスのブロック]** - このアプリが脱獄またはルート化されたデバイスで実行されないようにします。 ユーザーは引き続き個人のタスクにこのアプリを使用できますが、このアプリの職場または学校のデータにアクセスする場合は別のデバイスを使用する必要があります。</li></ul> <ul><li>**[データのワイプ]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。  </li></ul> |
| **OS の最小バージョン** | このアプリを使用するために必要な最小 Android オペレーティング システムを指定します。 *"アクション"* に含まれている項目: <br><ul><li>**警告** - デバイスの Android バージョンが要件を満たさない場合、通知が表示されます。 この通知は閉じることができます。  </li></ul> <ul><li>**[アクセスのブロック]** - デバイスの Android バージョンがこの要件を満たさない場合、アクセスがブロックされます。</li></ul> <ul><li>**[データのワイプ]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。  </li></ul> </li></ul>このポリシー設定では、major.minor、major.minor.build、major.minor.build.revision のいずれの形式もサポートされます。 |
| **アプリの最小バージョン** | 最小オペレーティング システムの値を指定します。 *"アクション"* に含まれている項目: <br><ul><li>**[警告]** - デバイス上のアプリ バージョンが要件を満たさない場合、通知が表示されます。 この通知は閉じることができます。</li></ul> <ul><li>**[アクセスのブロック]** - デバイスのアプリ バージョンが要件を満たさない場合、アクセスがブロックされます。 </li></ul> <ul><li>**[データのワイプ]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。 </li></ul> </li></ul> 多くの場合、アプリには個別のバージョン管理スキームがあるため、1 つのアプリを対象とした、1 つのアプリの最小バージョンでポリシー (*Outlook バージョン ポリシー*など) を作成します。<br><br> このエントリは複数回表示できます。インスタンスごとに異なるアクションがサポートされます。<br><br> このポリシー設定では、major.minor、major.minor.build、major.minor.build.revision のいずれの形式もサポートされます。<br><br> さらに、エンド ユーザーが基幹業務 (LOB) アプリの更新バージョンを取得できる**場所**を構成できます。 これがエンド ユーザーに対して表示されるのは **[アプリの最小バージョン]** 条件付き起動ダイアログであり、エンド ユーザーは最小バージョンの LOB アプリに更新することを求められます。 Android では、この機能にポータル サイトが使われます。 エンド ユーザーが LOB アプリを更新する必要がある場所を構成するには、キー `com.microsoft.intune.myappstore` を含むマネージド [アプリ構成ポリシー](app-configuration-policies-managed-app.md)をアプリに送信する必要があります。 送信される値により、エンド ユーザーがアプリをダウンロードするストアが定義されます。 アプリがポータル サイト経由で展開される場合、値は `CompanyPortal` である必要があります。 他のストアの場合は、完全な URL を入力する必要があります。 |
| **パッチの最小バージョン** | Google によってリリースされた Android の最小セキュリティ パッチがデバイスに適用されることを要求します。  <br><ul><li>**警告** - デバイスの Android バージョンが要件を満たさない場合、通知が表示されます。 この通知は閉じることができます。  </li></ul> <ul><li>**[アクセスのブロック]** - デバイスの Android バージョンがこの要件を満たさない場合、アクセスがブロックされます。</li></ul> <ul><li>**[データのワイプ]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。  </li></ul></li></ul> このポリシー設定は、*YYYY-MM-DD* の日付形式をサポートしています。 |
| **デバイスの製造元** | セミコロンで区切られた製造元の一覧を指定します。 これらの値では大文字と小文字が区別されません。 *"アクション"* に含まれている項目: <br><ul><li>**指定されたものを許可します (未指定のものはブロック)** - 指定の製造元に一致するデバイスのみでアプリを使用できます。 他のデバイスはすべてブロックされます。 </li></ul> <ul><li>**[指定されたものを許可します (未指定のものはワイプ)]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。 </li></ul> この設定の使用方法について詳しくは、[条件付き起動アクション](app-protection-policies-access-actions.md#android-policy-settings)に関する記事をご覧ください。 |
| **SafetyNet デバイスの構成証明** | アプリ保護ポリシーでは、Google Play Protect の API の一部がサポートされています。 この設定は、特にエンド ユーザー デバイス上で Google の SafetyNet 構成証明を構成します。 **基本的な整合性**または**基本的な整合性と認定デバイス**のいずれかを指定します。 **基本的な整合性**からは、デバイスの全体的な整合性がわかります。 ルート化されたデバイス、エミュレーター、仮想デバイス、改ざんの兆候があるデバイスは基本的な整合性のチェックで不合格になります。 **基本的な整合性と認定デバイス**からは、デバイスと Google のサービスとの互換性がわかります。 Google に認められた、改造されていないデバイスのみがこのチェックに合格します。 *"アクション"* に含まれている項目: <br><ul><li>**[警告]** - 構成された値に基づいて、デバイスが Google の SafetyNet 構成証明スキャンを満たさない場合、ユーザーに通知が表示されます。 この通知は閉じることができます。 </li></ul><ul><li>**[アクセスのブロック]** - 構成された値に基づいて、デバイスが Google の SafetyNet 構成証明スキャンを満たさない場合、ユーザーはアクセスがブロックされます。 </li></ul> <ul><li>**[データのワイプ]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。 </li></ul> </li></ul> この設定に関してよく寄せられる質問については、「[MAM とアプリの保護に関してよく寄せられる質問](mam-faq.md#app-experience-on-android)」を参照してください。 |
| **アプリの脅威のスキャンが必要** | アプリ保護ポリシーでは、Google Play Protect の API の一部がサポートされています。 この設定によって、特にエンド ユーザー デバイスに上で Google のアプリ スキャンの検証が確実に有効になります。 構成されている場合、エンド ユーザーは自分の Android デバイス上で Google のアプリ スキャンを有効にするまでアクセスがブロックされます。 *"アクション"* に含まれている項目: <br><ul><li>**[警告]** - デバイス上で Google のアプリの検証スキャンが有効ではない場合、ユーザーに通知が表示されます。 この通知は閉じることができます。 </li></ul><ul><li>**[アクセスのブロック]** - デバイス上で Google のアプリの検証スキャンが有効ではない場合、ユーザーはアクセスがブロックされます。 </li></ul></li></ul> Google のアプリの検証スキャンの結果は、コンソールの **[Potentially Harmful Apps]\(有害な可能性があるアプリ\)** レポートに表示されます。 |
| **ポータル サイトの最小バージョン** | **[ポータル サイトの最小バージョン]** を使用することにより、エンド ユーザーのデバイスに適用される、ポータル サイトの特定の定義済み最小バージョンを指定できます。 この条件付き起動の設定を使用すると、 **[アクセスのブロック]** 、 **[データのワイプ]** 、 **[警告]** に対して、各値が満たされなかった場合の実行可能なアクションとして値を設定できます。 この値に使用できる形式は、" *[メジャー].[マイナー]* "、" *[メジャー].[マイナー].[ビルド]* "、または " *[メジャー].[マイナー].[ビルド].[リビジョン]* " というパターンに従います。 一部のエンド ユーザーが、アプリをその場で強制的に更新されることを好まない場合は、この設定を構成するときに、[警告] オプションが最適となる場合があります。 Google Play ストアには、アプリの更新に対して差分バイトのみを送信する優れた機能が備わっていますが、これが更新時のデータに含まれる場合は、やはり、ユーザーが利用したいと思わないような大量のデータになる場合があります。 更新が強制され、その結果更新されたアプリがダウンロードされると、更新時に予期しないデータ料金が発生する可能性があります。 詳細については、「[Android のポリシーの設定](app-protection-policies-access-actions.md#android-policy-settings)」をご覧ください。 |
| **許容される最大デバイス脅威レベル** | アプリ保護ポリシーでは、Intune-MTD コネクタを利用できます。 このアプリの使用に対して許容される最大脅威レベルを指定します。 脅威は、エンド ユーザー デバイスで選択された Mobile Threat Defense (MTD) ベンダー アプリによって決定されます。 "*セキュリティ保護*"、"*低*"、"*中*"、"*高*" のいずれかを指定します。 "*セキュリティ保護*" は、デバイスに対する脅威は不要で、構成可能な最も制限の厳しい値であるのに対し、"*高*" では基本的に Intune と MTD の間のアクティブな接続が必要です。 *"アクション"* に含まれている項目: <br><ul><li>**アクセスのブロック** - エンド ユーザー デバイスで選択されている Mobile Threat Defense (MTD) ベンダー アプリによって決定された脅威レベルがこの要件を満たしていない場合、ユーザーはアクセスをブロックされます。</li></ul> <ul><li>**[データのワイプ]** - アプリケーションに関連付けられているユーザー アカウントがデバイスからワイプされます。</li></ul>この設定の使用方法について詳しくは、「[未登録デバイスに対して Intune で Mobile Threat Defense コネクタを有効にする](../protect/mtd-enable-unenrolled-devices.md)」をご覧ください。 |