---
title: Android Enterprise 仕事用プロファイルのセキュリティ設定
titleSuffix: Microsoft Intune
description: Android Enterprise デバイスの基本セキュリティと高セキュリティのために推奨される制限事項と設定について説明します。
keywords: ''
author: ErikjeMS
ms.author: erikje
manager: dougeby
ms.date: 06/24/2020
ms.topic: reference
ms.service: microsoft-intune
ms.subservice: enrollment
ms.localizationpriority: high
ms.technology: ''
ms.assetid: ''
ms.reviewer: rosssmi
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure;seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4283caf8f21e87736b09a3d6c7b31f8daf1f6075
ms.sourcegitcommit: e2cf3b80d1a4523d98542ccd7bba2439046c3830
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/04/2020
ms.locfileid: "87546828"
---
# <a name="android-enterprise-work-profile-security-configurations"></a>Android Enterprise 仕事用プロファイルのセキュリティ設定

[Android Enterprise セキュリティ構成フレームワーク](android-configuration-framework.md)の一部として、Android Enterprise 仕事用プロファイル モバイル ユーザーに対して次の設定を適用します。 各ポリシー設定の詳細については、「[Intune を使用してデバイスを準拠または非準拠としてマークするための Android Enterprise 設定](../protect/compliance-policy-create-android-for-work.md#work-profile)」と、「[Intune を使用して機能を許可または制限するように Android Enterprise デバイスを設定する](../configuration/device-restrictions-android-for-work.md#work-profile-only)」を参照してください。

設定を選択するときは、必ず使用シナリオを確認して分類してください。 その後、選択したセキュリティ レベルのガイダンスに従ってユーザーを構成します。 組織のニーズに基づいて、推奨される設定を調整できます。 セキュリティ チームによる脅威環境、リスク選好度、および使いやすさへの影響の評価を必ず受けてください。

個人所有の仕事用プロファイル デバイスには、2 つの推奨されるセキュリティ構成フレームワークがあります。

- [仕事用プロファイルの基本セキュリティ (レベル 1)](#work-profile-basic-security) 
- [仕事用プロファイルの高セキュリティ (レベル 3)](#work-profile-high-security) 

> [!Note]
> Android Enterprise の仕事用プロファイル デバイスで使用可能な設定により、強化セキュリティ (レベル 2) はありません。 使用可能な設定によって、レベル 1 とレベル 2 の差異が正当化されることはありません。

## <a name="work-profile-basic-security"></a>仕事用プロファイルの基本セキュリティ

ユーザーが職場または学校のデータにアクセスする個人用デバイスに対して推奨される最小限のセキュリティ構成は、レベル 1 です。 ほとんどのモバイル ユーザーに対して、この構成を適用できます。 一部の制御は、ユーザー エクスペリエンスに影響を与える可能性があります。

### <a name="device-compliance"></a>デバイスのポリシー準拠

| セクション | 設定 | 値 | 注 |
| ----- | ----- | ----- | ----- |
| Microsoft Defender ATP | デバイスは、次のマシン リスク スコア以下であることが必要 | 未構成 ||
| デバイスの正常性 | ルート化されたデバイス | ブロックする ||
| デバイスの正常性 | デバイスは、デバイス脅威レベル以下であることが必要 | 未構成||
| デバイスの正常性 | Google Play 開発者サービスが構成されている | 必須 ||
| デバイスの正常性 | 最新のセキュリティ プロバイダー | 必須 ||
| デバイスの正常性 | [SafetyNet デバイスの構成証明] | 基本的な整合性と認定デバイスのチェック | この設定により、エンド ユーザーのデバイス上に Google の SafetyNet 構成証明が構成されます。 基本的な整合性では、デバイスの整合性が検証されます。 ルート化されたデバイス、エミュレーター、仮想デバイス、改ざんの兆候があるデバイスは基本的な整合性のチェックで不合格になります。<p>基本的な整合性と認定デバイスでは、デバイスの Google のサービスとの互換性が検証されます。 Google に認められた、改造されていないデバイスのみがこのチェックに合格します。 |
| デバイスのプロパティ | 最小 OS バージョン | 形式: メジャー.マイナー<br>例:5.0| Microsoft では、Microsoft アプリに対してサポートされている Android のバージョンと一致するように、Android の最小のメジャー バージョンを構成することを推奨しています。 OEM と Android Enterprise Recommended の要件に準拠しているデバイスでは、現在出荷されているリリースと 1 つ後のアップグレードをサポートしている必要があります。 現在、Android では、ナレッジ ワーカーに対して Android 8.0 とその後が推奨されています。 Android の最新の推奨事項については、「[Android Enterprise Recommended の要件](https://www.android.com/enterprise/recommended/requirements/)」を参照してください。 |
| デバイスのプロパティ | 最大 OS バージョン | 未構成 ||
| システム セキュリティ | モバイル デバイスのロックを解除するパスワードを要求する | 必須 ||
| システム セキュリティ | 必要なパスワードの種類 | 数値複素数 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| システム セキュリティ | パスワードの最小文字数 | 6 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| システム セキュリティ | パスワードが要求されるまでの非アクティブの最長時間 (分)| 5 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。|
| システム セキュリティ | パスワードの有効期限が切れるまでの日数| 未構成 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| システム セキュリティ | 再使用を禁止するパスワード世代数 | 未構成 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| システム セキュリティ | デバイス上のデータ ストレージの暗号化 | 必須 ||
| システム セキュリティ | 提供元不明のアプリをブロックする | ブロックする ||
| システム セキュリティ | ポータル サイト アプリのランタイム整合性 | 必須 ||
| システム セキュリティ | デバイスでの USB デバッグをブロックする | ブロックする | この設定では、USB デバイスを使用したデバッグがブロックされますが、トラブルシューティング目的で役立つ可能性があるログを収集する機能も無効になります。 |
| システム セキュリティ | 最低限のセキュリティ パッチ レベル | 未構成 | Android デバイスでは、毎月のセキュリティ パッチを受け取ることができますが、そのリリースは OEM や通信事業者に依存します。 組織では、この設定を実装する前に、展開されている Android デバイスで確実にセキュリティ更新プログラムを受け取れるようにする必要があります。 最新のパッチ リリースについては、「[Android のセキュリティに関する公開情報](https://source.android.com/security/bulletin/)」を参照してください。 |
| コンプライアンス非対応に対するアクション | デバイスに非準拠のマークを付ける | 即時 | 既定では、ポリシーはデバイスを非準拠としてマークするように構成されています。 追加のアクションを使用できます。 詳細については、「[Intune で非準拠デバイスに対するアクションを構成する](../protect/actions-for-noncompliance.md)」を参照してください。|

### <a name="device-restrictions"></a>デバイスの制限

| セクション | 設定 | 値 | 注 |
| ----- | ----- | ----- | ----- |
| 仕事用プロファイル設定 | 仕事用プロファイルと個人用プロファイルの間でのコピー/貼り付け | ブロックする ||
| 仕事用プロファイル設定 | 仕事用プロファイルと個人プロファイル間のデータ共有 | 仕事用プロファイル内のアプリは、個人プロファイルからの共有要求を処理できます ||
| 仕事用プロファイル設定 | デバイス ロック中の仕事用プロファイルからの通知 | 未構成 | この設定をブロックすると、機密データが仕事用プロファイル通知に公開されなくなり、使いやすさに影響する可能性があります。 |
| 仕事用プロファイル設定 | 既定のアプリのアクセス許可 | デバイスの既定値 | 管理者は、展開するアプリによって付与されるアクセス許可を見直して調整する必要があります。 |
| 仕事用プロファイル設定 | アカウントの追加と削除 | ブロックする ||
| 仕事用プロファイル設定 | Bluetooth 経由での連絡先の共有 | [ウェイクアップ時間 (デスクトップ コンピューター)] を有効にして、デスクトップ コンピューターが、スケジュールされた更新またはソフトウェアのインストールを実行するために、スリープ状態または休止状態から復帰する時刻を指定します。 | 既定では、仕事用の連絡先へのアクセスは、Bluetooth 統合を使用した自動車などの他のデバイスでは利用できません。 この設定を有効にすると、ハンズ フリー ユーザー エクスペリエンスが向上します。 ただし、Bluetooth デバイスでは、最初の接続時に連絡先がキャッシュされる可能性があります。 組織は、この設定を実装する際に、ユーザビリティ シナリオとデータ保護に関する懸念のバランスを考慮する必要があります。 |
| 仕事用プロファイル設定 | 画面の取り込み | ブロックする ||
| 仕事用プロファイル設定 | 個人プロファイルに勤務先の連絡先の発信者番号を表示する | 未構成 ||
| 仕事用プロファイル設定 | 個人プロファイルから仕事用の連絡先を検索する | 未構成 | ユーザーの個人プロファイルから仕事用の連絡先へのアクセスをブロックすると、個人プロファイル内のテキスト メッセージングやダイヤラー エクスペリエンスなどの特定のユーザビリティ シナリオに影響を与える可能性があります。 組織は、この設定を実装する際に、ユーザビリティ シナリオとデータ保護に関する懸念のバランスを考慮する必要があります。 |
| 仕事用プロファイル設定 | カメラ | 未構成 ||
| 仕事用プロファイル設定 | 仕事用プロファイル アプリからのウィジェットを許可する | [ウェイクアップ時間 (デスクトップ コンピューター)] を有効にして、デスクトップ コンピューターが、スケジュールされた更新またはソフトウェアのインストールを実行するために、スリープ状態または休止状態から復帰する時刻を指定します。 ||
| 仕事用プロファイル設定 | 仕事用プロファイルのパスワードが必要 | 必須 ||
| 仕事用プロファイル設定 | パスワードの最小文字数 | 6 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| 仕事用プロファイル設定 | 仕事用プロファイルがロックされるまでの非アクティブな最長時間 (分)| 5 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| 仕事用プロファイル設定 | 仕事用プロファイルがワイプされるまでのサインイン失敗回数| 10 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| 仕事用プロファイル設定 | パスワードの有効期限 (日) | 未構成 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| 仕事用プロファイル設定 | 必要なパスワードの種類 | 数値複素数 ||
| 仕事用プロファイル設定 | 前のパスワードの再利用を防止 | 未構成 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。|
| 仕事用プロファイル設定 | 指紋によるロック解除 | 未構成 ||
| 仕事用プロファイル設定 | Smart Lock などの信頼できるエージェント | 未構成 |||
| デバイスのパスワード | パスワードの最小文字数 | 6 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| デバイスのパスワード | 画面がロックされるまでの非アクティブな最長時間 (分) | 5 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| デバイスのパスワード | デバイスがワイプされるまでのサインイン失敗回数 | 10 | この設定では、デバイスのワイプではなく、仕事用プロファイルのワイプがトリガーされます。 |
| デバイスのパスワード | パスワードの有効期限 (日) | 未構成 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| デバイスのパスワード | 必要なパスワードの種類 | 数値複素数 ||
| デバイスのパスワード | 前のパスワードの再利用を防止 | 未構成 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| デバイスのパスワード | 指紋によるロック解除 | 未構成 ||
| デバイスのパスワード | Smart Lock などの信頼できるエージェント | 未構成 ||
| システム セキュリティ | アプリの脅威のスキャン | 必須 | この設定によって、エンド ユーザー デバイス用に Google のアプリの検証スキャンが確実に有効になります。 構成されている場合、エンド ユーザーは自分の Android デバイス上で Google のアプリ スキャンを有効にするまでアクセスがブロックされます。 |
| システム セキュリティ | 個人プロファイルの不明なソースからのアプリのインストール禁止 | ブロックする ||

> [!Note]
> Android Enterprise 仕事用プロファイルが有効になっている場合、デバイスと仕事用プロファイルのパスコードを結合するために、"1 つのロック" が既定で構成されます。 必要に応じて、仕事用プロファイルの設定で 1 つのロックを無効にして、仕事用プロファイルとデバイスのパスコードを分離することができます。

## <a name="work-profile-high-security"></a>仕事用プロファイルの高セキュリティ

レベル 3 は、非常にリスクが高いユーザーまたはグループによって使用されるデバイスに対して推奨される構成です。 たとえば、不正に開示されると重大な物的損失が発生する極秘データを扱うユーザーです。 潤沢な資金を持つ世故に長けた攻撃者の標的となる可能性が高い組織は、以下に説明する追加の制約を設定する価値があります。 この構成では、レベル 1 の構成が以下によって拡張されています。
- モバイル脅威防御または Microsoft Defender ATP の実装。
- 仕事用プロファイル データ シナリオの制限。
- 強力なパスワード ポリシーの設定。

レベル 3 で適用されるポリシー設定には、レベル 1 で推奨されるすべてのポリシー設定が含まれます。 ただし、以下に示す設定には、追加または変更されたもののみが含まれています。 これらの設定によって、ユーザーまたはアプリケーションに与える影響がわずかに大きくなる可能性があります。 モバイル デバイスで機密情報にアクセスするユーザーが直面するリスクへの対応に適したレベルのセキュリティが提供されます。

### <a name="device-compliance"></a>デバイスのポリシー準拠

| セクション | 設定 | 値 | 注 |
| ----- | ----- | ----- | ----- |
| Microsoft Defender ATP | デバイスは、次のマシン リスク スコア以下であることが必要 | クリア | この設定には、Microsoft Defender ATP が必要です。 詳細については、「[Intune で条件付きアクセスによる Microsoft Defender ATP](../protect/advanced-threat-protection.md) のコンプライアンスを強制する」を参照してください。<p>お客様は、Microsoft Defender ATP またはモバイル脅威防御ソリューションの実装を検討する必要があります。 両方を展開する必要はありません。 |
| デバイスの正常性 | デバイスは、デバイス脅威レベル以下であることが必要 | セキュリティ保護 | この設定には、モバイル脅威防御製品が必要です。 詳細については、[登録デバイスのモバイル脅威防御](../protect/mtd-device-compliance-policy-create.md)に関するページをご覧ください。<p>お客様は、Microsoft Defender ATP またはモバイル脅威防御ソリューションの実装を検討する必要があります。 両方を展開する必要はありません。|
| デバイスのプロパティ | 最小 OS バージョン | 形式: メジャー.マイナー<br>例:8.0| Microsoft では、Microsoft アプリに対してサポートされている Android のバージョンと一致するように、Android の最小のメジャー バージョンを構成することを推奨しています。 OEM と Android Enterprise Recommended の要件に準拠しているデバイスでは、現在出荷されているリリースと 1 つ後のアップグレードをサポートしている必要があります。 現在、Android では、ナレッジ ワーカーに対して Android 8.0 とその後が推奨されています。 Android の最新の推奨事項については、「Android Enterprise Recommended の要件」を参照してください |
| システム セキュリティ | パスワードの有効期限が切れるまでの日数 | 365 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| システム セキュリティ | 再使用を禁止するパスワード世代数 | 5 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |


### <a name="device-restrictions"></a>デバイスの制限

| セクション | 設定 | 値 | 注 |
| ----- | ----- | ----- | ----- |
| 仕事用プロファイル設定 | デバイス ロック中の仕事用プロファイルからの通知 | ブロックする | この設定をブロックすると、機密データが仕事用プロファイル通知に公開されなくなり、使いやすさに影響する可能性があります。 |
| 仕事用プロファイル設定 | Bluetooth 経由での連絡先の共有 | 未構成 | 既定では、仕事用の連絡先へのアクセスは、Bluetooth 統合を使用した自動車などの他のデバイスでは利用できません。 この設定を有効にすると、ハンズ フリー ユーザー エクスペリエンスが向上します。 ただし、Bluetooth デバイスでは、最初の接続時に連絡先がキャッシュされる可能性があります。 組織は、この設定を実装する際に、ユーザビリティ シナリオとデータ保護に関する懸念のバランスを考慮する必要があります。 |
| 仕事用プロファイル設定 | 個人プロファイルから仕事用の連絡先を検索する | ブロックする | ユーザーの個人プロファイルから仕事用の連絡先へのアクセスをブロックすると、個人プロファイル内のテキスト メッセージングやダイヤラー エクスペリエンスなどの特定のユーザビリティ シナリオに影響を与える可能性があります。 組織は、この設定を実装する際に、ユーザビリティ シナリオとデータ保護に関する懸念のバランスを考慮する必要があります。 |
| 仕事用プロファイル設定 | 仕事用プロファイル アプリからのウィジェットを許可する | 未構成 ||
| 仕事用プロファイル設定 | 仕事用プロファイルがワイプされるまでのサインイン失敗回数 | 5 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| 仕事用プロファイル設定 | パスワードの有効期限 (日) | 365 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| 仕事用プロファイル設定 | 前のパスワードの再利用を防止 | 5 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| 仕事用プロファイル設定 | Smart Lock などの信頼できるエージェント | ブロックする ||
| デバイスのパスワード | デバイスがワイプされるまでのサインイン失敗回数 | 5 | この設定によって、デバイスのワイプではなく、仕事用プロファイルのワイプがトリガーされます。 |
| デバイスのパスワード | パスワードの有効期限 (日) | 365 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |
| デバイスのパスワード | 前のパスワードの再利用を防止 | 5 | 組織では、各自のパスワード ポリシーに合うようにこの設定の更新が必要になる可能性があります。 |

## <a name="next-steps"></a>次のステップ

管理者は、[Intune の PowerShell スクリプト](https://github.com/microsoftgraph/powershell-intune-samples)を使用してサンプルの [Android Enterprise セキュリティ構成フレームワーク JSON テンプレート](https://github.com/microsoft/Intune-Config-Frameworks/tree/master/AndroidEnterprise)をインポートすることにより、テスト用および運用環境用のリング展開方法の中に上記の構成レベルを組み込むことができます。
