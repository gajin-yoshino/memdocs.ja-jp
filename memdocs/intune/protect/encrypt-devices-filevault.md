---
title: Intune で FileVault ディスク暗号化を使用して macOS デバイスを暗号化する
titleSuffix: Microsoft Intune
description: 組み込みの暗号化方式 FileVault を使用して macOS デバイスを暗号化し、Intune ポータル内からそれらの暗号化されたデバイスの回復キーを管理します。
keywords: ''
author: brenduns
ms.author: brenduns
manager: dougeby
ms.date: 04/17/2020
ms.topic: how-to
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high
ms.technology: ''
ms.assetid: ''
ms.reviewer: annovich
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.openlocfilehash: 99facc87d068239962ab0d40874aa081f5e19189
ms.sourcegitcommit: 302556d3b03f1a4eb9a5a9ce6138b8119d901575
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/27/2020
ms.locfileid: "83989694"
---
# <a name="use-filevault-disk-encryption-for--macos-with-intune"></a>Intune で macOS 用の FileVault ディスク暗号化を使用する

Intune は macOS FileVault ディスク暗号化をサポートしています。 FileVault は、macOS に含まれるディスク全体の暗号化プログラムです。 Intune を使用して、**macOS 10.13 以降**を実行するデバイスで FileVault を構成することができます。

次の種類のポリシーのいずれかを使用して、マネージド デバイスで FileVault を構成します。

- **[macOS FileVault のエンドポイント セキュリティ ポリシー](#create-an-endpoint-security-policy-for-filevault)** 。 *エンドポイント セキュリティ*の FileVault プロファイルは、FileVault の構成専用の一連の設定です。

  [ディスク暗号化ポリシーのプロファイルで使用できる FileVault 設定](../protect/endpoint-security-disk-encryption-profile-settings.md)を表示します。

- **[macOS FileVault のエンドポイント保護のためのデバイス構成プロファイル](#create-an-endpoint-security-policy-for-filevault)** 。 FileVault 設定は、macOS エンドポイント保護で使用可能な設定カテゴリの 1 つです。 デバイス構成プロファイルの使用の詳細については、[Intune でのデバイス プロファイルの作成](../configuration/device-profile-create.md)に関するページを参照してください。

  [デバイス構成ポリシーのエンドポイント保護プロファイルで使用できる FileVault 設定](../protect/endpoint-protection-macos.md#filevault)を表示します。

Windows 10 の BitLocker を管理するには、[BitLocker ポリシーの管理](../protect/encrypt-devices.md)に関する記事を参照してください。

> [!TIP]
> すべてのマネージド デバイスのデバイスの暗号化ステータスに関する詳細を表示する、組み込みの[暗号化レポート](encryption-monitor.md)。

FileVault を使用してデバイスを暗号化するポリシーを作成すると、そのポリシーは 2 段階でデバイスに適用されます。 まず、Intune が回復キーを取得してバックアップできるようにデバイスが準備されます。 このアクションはエスクローと呼ばれます。 キーがエスクローされると、ディスクの暗号化が開始されます。

デバイス上で FileVault を機能させるためには、ユーザー承認済みのデバイス登録が必要です。 登録がユーザー承認済みであると見なされるためには、そのユーザーがシステム環境設定から手動で管理プロファイルを承認する必要があります。

## <a name="permissions-to-manage-filevault"></a>FileVault を管理するためのアクセス許可

Intune で FileVault を管理するには、アカウントに適切な Intune [ロールベースのアクセス制御](../fundamentals/role-based-access-control.md) (RBAC) アクセス許可を与える必要があります。

以下は FileVault アクセス許可です。これは**リモート タスク** カテゴリに含まれ、アクセス許可を付与する組み込み RBAC ロールです。

- **FileVault キーの取得**:
  - ヘルプ デスク オペレーター
  - エンドポイント セキュリティ マネージャー

- **FileVault キーのローテーション**
  - ヘルプ デスク オペレーター

## <a name="create-and-deploy-policy"></a>ポリシーの作成と展開

### <a name="create-an-endpoint-security-policy-for-filevault"></a>FileVault のエンドポイント セキュリティ ポリシーを作成する

1. [Microsoft Endpoint Manager 管理センター](https://go.microsoft.com/fwlink/?linkid=2109431)にサインインします。

2. **[エンドポイント セキュリティ]**  >  **[ディスク 暗号化]**  >  **[ポリシーの作成]** を選択します。

3. **[基本]** ページで、次のプロパティを入力し、 **[次へ]** を選択します。
   1. **プラットフォーム**: macOS
   2. **[プロファイル]** :FileVault

   ![FileVault プロファイルの選択](./media/encrypt-devices-filevault/select-macos-filevault-es.png)

4. **[構成設定]** ページで次のようにします。
   1. *[FileVault を有効にする]* を **[はい]** に設定します。
   2. *[回復キーの種類]* には、 **[個人用回復キー]** のみがサポートされています。
   3. 要件に合わせて追加の設定を構成します。

   自分のデバイスの回復キーを取得する方法について、ユーザーを支援するガイドにメッセージを追加することを検討してください。 この情報は、個人用回復キーの交換の設定を使用するときに、デバイスの新しい回復キーを定期的に自動生成することができるため、ユーザーにとって役立ちます。

   次に例を示します。紛失した、または最近交換した回復キーを取得するには、任意のデバイスから Intune ポータル Web サイトにサインインします。 ポータルで、[デバイス] に移動し、FileVault が有効になっているデバイスを選択し、 *[回復キーを取得する]* を選択します。 現在の回復キーが表示されます。

5. 設定の構成が完了したら、 **[次へ]** を選択します。

6. **[スコープ (タグ)]** タブで **[スコープ タグを選択]** を選択し、[タグを選択する] ウィンドウを開いて、プロファイルにスコープ タグを割り当てます。

   **[次へ]** を選択して続行します。

7. **[割り当て]** ページで、このプロファイルを受け取るグループを選択します。 プロファイルの割り当ての詳細については、ユーザーおよびデバイス プロファイルの割り当てに関するページを参照してください。
**[次へ]** を選択します。

8. **[確認および作成]** ページで、完了したら、 **[作成]** を選択します。 作成したプロファイルのポリシーの種類を選択すると、新しいプロファイルが一覧に表示されます。

### <a name="create-a-device-configuration-policy-for-filevault"></a>FileVault のデバイス構成ポリシーを作成する

1. [Microsoft Endpoint Manager 管理センター](https://go.microsoft.com/fwlink/?linkid=2109431)にサインインします。

2. **[デバイス]**  >  **[構成プロファイル]**  >  **[プロファイルの作成]** の順に選択します。

3. 次のオプションを設定します。
   1. **プラットフォーム**: macOS
   2. **[プロファイル]** :エンドポイント保護

   ![FileVault プロファイルの選択](./media/encrypt-devices-filevault/select-macos-filevault-dc.png)

4. **[設定]**  >  **[FileVault]** の順に選択します。

   ![FileVault の設定](./media/encrypt-devices-filevault/filevault-settings.png)

5. *FileVault* で **[有効]** を選択します。

6. *[回復キーの種類]* には、**個人用キー**のみがサポートされています。

   自分のデバイスの回復キーを取得する方法について、ユーザーを支援するガイドにメッセージを追加することを検討してください。 この情報は、個人用回復キーの交換の設定を使用するときに、デバイスの新しい回復キーを定期的に自動生成することができるため、ユーザーにとって役立ちます。

   次に例を示します。紛失した、または最近交換した回復キーを取得するには、任意のデバイスから Intune ポータル Web サイトにサインインします。 ポータルで、 *[デバイス]* に移動し、FileVault が有効になっているデバイスを選択し、 *[回復キーを取得する]* を選択します。 現在の回復キーが表示されます。

7. ご自身のビジネス ニーズに合うよう残りの [FileVault 設定](endpoint-protection-macos.md#filevault)を構成した後、 **[OK]** を選択します。

8. 追加設定の構成を完了したら、プロファイルを保存します。

## <a name="manage-filevault"></a>FileVault の管理

FileVault ポリシーを受信するデバイスに関する情報を表示するには、[ディスク暗号化の監視](../protect/encryption-monitor.md)に関する記事を参照してください。

Intune で FileVault を使用して macOS デバイスを最初に暗号化するときに、個人用回復キーが作成されます。 暗号化時に、デバイスではデバイス ユーザーに対して個人キーが一度だけ表示されます。

マネージド デバイスの場合、Intune で個人用回復キーのコピーをエスクローできます。 キーのエスクローにより、Intune 管理者は、デバイスの保護に役立つようにキーを交換したり、紛失または交換した個人用回復キーを回復したりすることができます。

Intune で FileVault を使用して macOS デバイスを暗号化すると:

- 管理者は、Intune の暗号化レポートを使用して、FileVault 回復キーを表示および管理できます。
- ユーザーは、デバイスの Web ポータル サイトからデバイスの個人用回復キーを表示できます。 Web ポータル サイトから、暗号化された macOS デバイスを選択した後、リモート デバイス アクションとして [回復キーを取得する] を選択します。

> [!IMPORTANT]
> Intune ではなく、ユーザーによって暗号化されたデバイスを、Intune で管理することはできません。 つまり、Intune では、これらのデバイスの個人用回復キーをエスクローしたり、回復キーの交換を管理したりすることはできません。 Intune でデバイスの FileVault および回復キーを管理できるようにするには、ユーザーがデバイスの暗号化を解除してから、Intune でデバイスを暗号化する必要があります。

### <a name="retrieve-personal-recovery-key"></a>個人用回復キーの取得

Intune を使用して暗号化された macOS デバイスの場合、エンド ユーザーは、iOS ポータル サイト アプリ、Android ポータル サイト アプリ、または Android Intune アプリを使用して、個人用回復キー (FileVault キー) を取得できます。

個人用回復キーを持つデバイスは、Intune に登録され、Intune により FileVault を使用して暗号化されている必要があります。 ユーザーは、iOS ポータル サイト アプリ、Android ポータル サイト アプリ、Android Intune アプリ、またはポータル サイト Web サイトを使用して、Mac デバイスへのアクセスに必要な **FileVault** 回復キーを確認できます。

デバイス ユーザーは、 **[デバイス]**  > *暗号化および登録された macOS デバイス* >  **[回復キーの取得]** を選択できます。 ブラウザーに Web ポータル サイトが表示され、回復キーが表示されます。

### <a name="rotate-recovery-keys"></a>回復キーを交換する

Intune では、個人用回復キーの交換と回復のための複数のオプションがサポートされています。 キーを交換する理由の 1 つは、現在の個人用キーが失われた場合や、リスクがあると考えられる場合です。

- **自動交換**:管理者は、定期的に新しい回復キーを自動生成するように、FileVault 設定の個人用回復キーの交換を構成できます。 デバイスに対して新しいキーが生成されると、そのキーはユーザーに表示されません。 代わりに、ユーザーは管理者から、またはポータル サイト アプリを使用して、キーを取得する必要があります。

- **手動交換**:管理者は、Intune で管理し、FileVault で暗号化されているデバイスの情報を表示できます。 その後、会社のデバイスの回復キーを手動で交換するように選択できます。 個人のデバイスの回復キーを交換することはできません。

  回復キーを交換するには、次のようにします。

  1. [Microsoft Endpoint Manager 管理センター](https://go.microsoft.com/fwlink/?linkid=2109431)にサインインします。

  2. **[デバイス]**  >  **[すべてのデバイス]** の順に選択します。

  3. デバイスのリストから、暗号化され、キーを交換するデバイスを選択します。 その後、[監視] で  **[回復キー]** を選択します。
  
  4. [回復キー] ウィンドウで、 **[FileVault 回復キーの交換]** を選択します。

     次回、デバイスで Intune にチェックインしたときに、個人用キーが交換されます。 必要に応じて、ユーザーがポータル サイトを介して新しいキーを取得できます。

### <a name="recover-recovery-keys"></a>回復キーを回復する

- **管理者**:管理者は、FileVault で暗号化されたデバイスの個人用回復キーを表示することはできません。

- **エンドユーザー**:エンドユーザーは、任意のデバイスからポータル Web サイトを使用して、マネージド デバイスのいずれかの現在の個人用回復キーを表示します。 ポータル サイト アプリから回復キーを表示することはできません。

  回復キーを表示するには、次のようにします。
  
  1. 任意のデバイスから、*Intune ポータル サイト* Web サイトにサインインします。

  2. ポータルで、 **[デバイス]** に移動し、FileVault で暗号化された macOS デバイスを選択します。

  3. **[回復キーを取得する]** を選択します。 現在の回復キーが表示されます。

## <a name="next-steps"></a>次のステップ

[BitLocker ポリシーの管理](../protect/encrypt-devices.md)

[ディスク暗号化の監視](../protect/encryption-monitor.md)