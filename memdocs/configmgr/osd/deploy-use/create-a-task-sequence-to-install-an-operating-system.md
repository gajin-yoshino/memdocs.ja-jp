---
title: OS をインストールするタスク シーケンスの作成
titleSuffix: Configuration Manager
description: Configuration Manager でタスク シーケンスを使用して、対象のコンピューターに OS イメージとその他のコンテンツを自動的にインストールします。
ms.date: 07/26/2019
ms.prod: configuration-manager
ms.technology: configmgr-osd
ms.topic: conceptual
ms.assetid: 217c8a0e-5112-420e-a325-2a6d75326290
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 6e1b298856edea3f81cab2e9cd5ab75af49dff51
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81707310"
---
# <a name="create-a-task-sequence-to-install-an-os"></a>OS をインストールするタスク シーケンスの作成

*適用対象:Configuration Manager (Current Branch)*

Configuration Manager でタスク シーケンスを使用して、対象のコンピューターに OS イメージを自動的にインストールします。 セットアップ先のコンピューターを起動する起動イメージ、セットアップ先のコンピューターにインストールする OS イメージ、およびアプリケーションやソフトウェア更新プログラムなどといったその他のインストールする追加のコンテンツを参照するタスク シーケンスを作成します。 その後、セットアップ先のコンピューターを含んでいるコレクションにタスク シーケンスを展開します。  

## <a name="create-a-task-sequence-to-install-an-os"></a><a name="BKMK_InstallOS"></a> OS をインストールするタスク シーケンスの作成

環境内のコンピューターに OS を展開するシナリオは多様です。 ほとんどの場合、タスク シーケンスを作成し、タスク シーケンスの作成ウィザードで **[既存のイメージ パッケージをインストールする]** を選択します。 このオプションを選択すると、OS のインストール、ユーザー設定の移行、ソフトウェア更新プログラムの適用、およびアプリケーションのインストールを行うタスク シーケンスが作成されます。

### <a name="prerequisites"></a>[前提条件]

OS をインストールするタスク シーケンスを作成する前に、次を要件を満たす必要があります。

#### <a name="required"></a>必須

- [ブート イメージ](../get-started/manage-boot-images.md)  

- [OS イメージ](../get-started/manage-operating-system-images.md)  

#### <a name="required-if-used"></a>必須 (使用する場合)

- [ソフトウェア更新プログラム](../../sum/get-started/synchronize-software-updates.md)の同期  

- [アプリケーション](../../apps/deploy-use/create-applications.md)の追加  

### <a name="process-to-create-a-task-sequence-that-installs-an-os"></a>OS をインストールするタスク シーケンスを作成するプロセス  

1. Configuration Manager コンソールで、 **[ソフトウェア ライブラリ]** ワークスペースに移動し、 **[オペレーティング システム]** を展開して **[タスク シーケンス]** ノードを選択します。  

2. リボンの **[ホーム]** タブにある **[作成]** グループで、 **[タスク シーケンスの作成]** を選択します。 このアクションにより、タスク シーケンスの作成ウィザードが起動します。  

3. **[新しいタスク シーケンスの作成]** ページで、 **[既存のイメージ パッケージをインストールする]** を選択し、 **[次へ]** を選択します。  

4. **[タスク シーケンス情報]** ページで次の設定を指定します。  

    - **タスク シーケンス名**:タスク シーケンスを識別する名前を指定します。  

    - **説明**:タスクシーケンスの実行内容の説明を指定します。  

    - **ブート イメージ**:対象コンピューターに OS をインストールするためにタスク シーケンスで使用されるブート イメージを指定します。 ブート イメージには、Windows PE のバージョンと、その他の必要なデバイス ドライバーが含まれています。 詳細については、「[ブート イメージの管理](../get-started/manage-boot-images.md)」をご覧ください。  

        > [!IMPORTANT]  
        > ブート イメージのアーキテクチャは、対象となるコンピューターのハードウェア アーキテクチャと互換性がある必要があります。  

5. **[Windows のインストール]** ページで次の設定を指定します。  

    - **イメージ パッケージ**:インストールする OS を含むパッケージを指定します。 詳しくは、[OS イメージの管理](../get-started/manage-operating-system-images.md)に関するページをご覧ください。  

    - **イメージ**:OS イメージ パッケージに複数のイメージがある場合は、インストールする OS イメージのインデックスを指定します。  

    - **オペレーティング システムをインストールする前にターゲット コンピューターのパーティションを作成してフォーマットする**:タスク シーケンスによる展開先コンピューターのパーティション作成とフォーマットを、OS がインストールされる前に行うかどうかを指定します。  

    - **プロダクト キー**:必要に応じて、Windows プロダクト キーを指定します。 エンコードされたボリューム ライセンス キーと標準のプロダクト キーを指定できます。 エンコードされていないプロダクト キーを使用する場合は、5 文字の各グループをダッシュ (`-`) で区切る必要があります。 次に例を示します。*XXXXX-XXXXX-XXXXX-XXXXX-XXXXX*  

    - **サーバー ライセンス モード**:サーバー ライセンスが **[接続クライアント数]** と **[同時使用ユーザー数]** のいずれか、または決められていないかを指定します。 サーバー ライセンスが **[同時使用ユーザー数]** の場合は、サーバー接続の最大数も指定します。  

    - 新しい OS の管理者アカウントを処理する方法を指定します。  

        - **ローカル管理者アカウントのパスワードをランダムに生成し、サポートされているすべてのプラットフォームのアカウントを無効にする (推奨)** :タスク シーケンスによって OS イメージが展開された後、Windows によってローカル管理者アカウントが無効化されます。  

        - **アカウントを有効にし、ローカル管理者パスワードを指定する**:タスク シーケンスによって OS イメージが展開されたすべてのコンピューターで、Windows によってローカル管理者アカウントに対して同じパスワードが使用されます。  

6. **[ネットワークの構成]** ページで次の設定を指定します。  

    - **ワークグループに参加**:対象コンピューターをワークグループに追加します。  

    - **ドメインに参加**:対象コンピューターをドメインに追加します。 **[ドメイン]** にドメインの名前を指定します。  

        > [!IMPORTANT]  
        > ローカル フォレストではドメインを参照できますが、リモート フォレストではドメイン名を指定する必要があります。  

        **[ドメイン OU]** フィールドで組織単位 (OU) を指定することもできます。 この設定は省略可能で、OU の LDAP x.500 識別名を指定します。 コンピューター アカウントがまだ存在しない場合は、Windows によってこの OU でコンピューター アカウントが作成されます。  

    - **アカウント**:指定したドメインに参加する、アクセス許可を持つアカウントのユーザー名とパスワード。 例: *domain\user* または *%variable%* 。  

        > [!IMPORTANT]  
        > ドメイン設定またはワークグループ設定を移行することを計画している場合は、適切なドメイン資格情報を入力してください。  

7. **[Configuration Manager のインストール]** ページで、インストール先コンピューターにインストールする Configuration Manager クライアント パッケージを指定します。 インストール プロパティも含めることができます。  

8. **[状態移行]** ページで次の設定を指定します。  

    - **ユーザー設定のキャプチャ**:タスク シーケンスで、ユーザーの状態をキャプチャします。 ユーザー状態をキャプチャして復元する方法の詳細については、「[ユーザー状態の管理](../get-started/manage-user-state.md)」をご覧ください。  

    - **ネットワーク設定のキャプチャ**:タスク シーケンスで、展開先コンピューターからネットワーク設定をキャプチャします。 ドメインまたはワークグループのメンバーシップや、ネットアーク アダプター設定もキャプチャされます。  

    - **Microsoft Windows の設定のキャプチャ**:タスク シーケンスによって、OS イメージがインストールされる前に、対象コンピューターから Windows 設定がキャプチャされます。 コンピューター名、登録ユーザー名と組織名、およびタイム ゾーン設定がキャプチャされます。  

9. **[更新プログラムを含める]** ページで、必要なソフトウェア更新プログラム、すべてのソフトウェア更新プログラム、またはソフトウェア更新プログラムなしを指定します。 ソフトウェア更新プログラムをインストールするように指定する場合、Configuration Manager は、セットアップ先のコンピューターがメンバーとなっているコレクション向けのソフトウェア更新プログラムのみをインストールします。  

10. **[アプリケーションのインストール]** ページで、展開先コンピューターにインストールするアプリケーションを指定します。 複数のアプリケーションを指定する場合は、特定のアプリケーションのインストールに失敗したときにタスク シーケンスを続行するかどうかも指定することができます。  

11. ウィザードを完了します。  

コンピューターのコレクションにタスク シーケンスを展開できるようになりました。 詳細については、「 [Deploy a task sequence](deploy-a-task-sequence.md)」をご覧ください。


## <a name="pre-cache-content"></a>コンテンツの事前キャッシュ

<!--4224642-->
バージョン 1906 以降では、コンテンツを事前キャッシュするために、この種類のタスク シーケンスを有効にすることができます。 タスク シーケンスの利用可能な展開に事前キャッシュ機能を使用すると、ユーザーがタスク シーケンスをインストールする前に、関連するコンテンツをクライアントにダウンロードさせることができます。  

詳細については、「[コンテンツの事前キャッシュを構成する](configure-precache-content.md)」を参照してください。


## <a name="example-task-sequence"></a><a name="BKMK_InstallExistingOSImageTSExample"></a> タスク シーケンスの例

既存のイメージを使用して OS を展開するタスク シーケンスを作成する場合は、次の表をガイドとして使用してください。 この表を利用して、タスク シーケンス ステップの全般的な順序と、これらのタスク シーケンス ステップを論理的なグループに整理および構造化する方法を決定できます。 実際に作成するタスク シーケンスは、この例とは異なり、タスク シーケンス ステップやグループの数が違う場合があります。  

> [!NOTE]  
> タスク シーケンスの作成ウィザードを使用して、このタスク シーケンスを作成します。  
>
> タスク シーケンスの作成ウィザードを使用してこの新しいタスク シーケンスを作成すると、一部のステップ名は、これらのタスク シーケンス ステップを既存のタスク シーケンスに手動で追加した場合の名前と異なります。

|タスク シーケンス グループまたはステップ|[説明]|  
|---------------------------------|-----------------|  
|ファイルと設定のキャプチャ - **(新しいタスク シーケンス グループ)**|タスク シーケンス グループを作成します。 タスク シーケンス グループを使用すると、類似のタスク シーケンス ステップをまとめて、整理とエラー制御を向上させることができます。<br /><br /> このグループには、参照コンピューターのオペレーティング システムからファイルと設定をキャプチャするために必要なステップが含まれています。|  
|Windows 設定のキャプチャ|このタスク シーケンス ステップを使用して、参照コンピューターからキャプチャする Microsoft Windows 設定を確認できます。 コンピューター名、ユーザーと組織の情報、およびタイム ゾーン設定をキャプチャできます。|  
|ネットワーク設定のキャプチャ|このタスク シーケンス ステップを使用して、参照コンピューターからネットワーク設定をキャプチャできます。 参照コンピューターのドメインまたはワークグループのメンバーシップ、およびネットワーク アダプター設定情報をキャプチャできます。|  
|ユーザー ファイルと設定のキャプチャ - **(新しいタスク シーケンス サブグループ)**|タスク シーケンス グループ内にタスク シーケンス グループを作成します。 このサブグループには、ユーザー状態データのキャプチャに必要なステップが含まれています。 追加した最初のグループと同様に、このサブグループは類似のタスク シーケンス ステップをまとめて、整理とエラー制御を向上させることができます。|  
|ユーザー状態ストアの要求|このタスク シーケンス ステップを使用して、ユーザー状態データを保存する状態移行ポイントへのアクセスを要求することができます。 このタスク シーケンス ステップを構成して、ユーザー状態情報をキャプチャまたは復元できます。|  
|ユーザー ファイルと設定のキャプチャ|このタスク シーケンス ステップでは、ユーザー状態移行ツール (USMT) を使用し、このタスク ステップに関連付けられたタスク シーケンスを受信する参照コンピューターからユーザーの状態と設定をキャプチャできます。 標準オプションをキャプチャするか、キャプチャするオプションを構成できます。|  
|ユーザー状態ストアのリリース|このタスク シーケンス ステップでは、キャプチャ処理または復元処理が完了した状態移行ポイントが通知されます。|  
|オペレーティング システムのインストール - **(新規タスク シーケンス グループ)**|別のタスク シーケンス サブグループを作成します。 このサブグループには、Windows PE 環境のインストールと構成に必要なステップが含まれています。|  
|Windows PE での再起動|このタスク シーケンス ステップを使用して、このタスク シーケンスを受信する対象のコンピューターの再起動オプションを指定できます。 このステップでは、インストールを続行するためにコンピューターを再起動することをユーザーに示すメッセージが表示されます。<br /><br /> このステップでは、読み取り専用の **_SMSTSInWinPE** タスク シーケンス変数が使用されます。 関連する値が **false** の場合に、タスク シーケンス ステップが続行されます。|  
|ディスク 0 のパーティション作成|このタスク シーケンス ステップを使用して、対象のコンピューターのハード ドライブのフォーマットに必要な処理を指定できます。 既定のディスク番号は **0**です。<br /><br /> このステップでは、読み取り専用の **_SMSTSClientCache** タスク シーケンス変数が使用されます。 このステップは、構成マネージャー クライアントのキャッシュが存在しない場合に実行されます。|  
|オペレーティング システムの適用|このタスク シーケンス手順を使用すると、セットアップ先コンピューターにオペレーティング システム イメージをインストールできます。 このステップでは最初に、Configuration Manager 固有のコントロール ファイルを除く、ボリューム上のすべてのファイルが削除されます。 その後、WIM ファイルに含まれるすべてのボリューム イメージが、ターゲット コンピューター上の対応する順次ディスク ボリュームに適用されます。 **sysprep** 応答ファイルを指定したり、インストールに使用するディスク パーティションを構成することもできます。|  
|Windows 設定の適用|このタスク シーケンス ステップを使用して、対象コンピューターの Windows 設定の構成情報を構成できます。 適用できる Windows 設定は、ユーザーと組織の情報、製品またはライセンス キーの情報、タイム ゾーン、およびローカル管理者パスワードです。|  
|ネットワーク設定の適用|このタスク シーケンス ステップを使用して、対象コンピューターのネットワークまたはワークグループの構成情報を指定できます。 コンピューターが DHCP サーバーを使用するかどうか、または IP アドレス情報を静的に割り当てられるかどうかも指定できます。|  
|デバイス ドライバーの適用|このタスク シーケンス ステップを使用して、オペレーシング システムの展開の一部としてドライバーをインストールできます。 **[すべてのカテゴリのドライバーを検討する]** を選択して、Windows セットアップですべての既存ドライバー カテゴリを検索するか、 **[ドライバーの一致条件を制限して、選択したカテゴリに属するドライバーだけを検討する]** を選択して、Windows セットアップが検索するドライバー カテゴリを制限できます。<br /><br /> このステップでは、読み取り専用の **_SMSTSMediaType** タスク シーケンス変数が使用されます。 このタスク シーケンス ステップは、変数の値が **FullMedia** に等しくない場合にのみ実行されます。|  
|ドライバー パッケージの適用|このタスク シーケンス ステップを使用して、ドライバー パッケージ内のデバイス ドライバーをすべて Windows セットアップで使用できるようにします。|  
|オペレーティング システムのセットアップ - **(新規タスク シーケンス グループ)**|別のタスク シーケンス サブグループを作成します。 このサブグループには、インストール済みのオペレーティング システムのセットアップに必要なステップが含まれています。|  
|Windows と ConfigMgr のセットアップ|このタスク シーケンス ステップを使用して Configuration Manager クライアント ソフトウェアをインストールします。 Configuration Manager は、Configuration Manager クライアントの GUID をインストールして登録します。 必要なインストール パラメーターは、 **[インストールのプロパティ]** ウィンドウで割り当てることができます。|  
|更新のインストール|このタスク シーケンス ステップを使用して、ソフトウェア更新を対象のコンピューターにインストールする方法を指定できます。 このタスク シーケンス ステップが実行されるまで、対象のコンピューターは適用可能なソフトウェア更新プログラムが評価されません。 このとき、セットアップ先のコンピューターは、Configuration Manager が管理している他のクライアントと同様にソフトウェア更新プログラムが評価されます。<br /><br /> このステップでは、読み取り専用の **_SMSTSMediaType** タスク シーケンス変数が使用されます。 このタスク シーケンス ステップは、変数の値が **FullMedia** に等しくない場合にのみ実行されます。|  
|ユーザー ファイルと設定の復元 - **(新しいタスク シーケンス サブグループ)**|別のタスク シーケンス サブグループを作成します。 このサブグループには、ユーザー ファイルと設定の復元に必要なステップが含まれています。|  
|ユーザー状態ストアの要求|このタスク シーケンス ステップを使用して、ユーザー状態データを保存する状態移行ポイントへのアクセスを要求することができます。|  
|ユーザー ファイルと設定の復元|このタスク シーケンス ステップを使用すると、ユーザー状態移行ツール (USMT) を実行して、ターゲット コンピューターにユーザーの状態および設定を復元できます。|  
|ユーザー状態ストアのリリース|このタスク シーケンス ステップを使用すると、ユーザー状態データが必要ではなくなった状態移行ポイントが通知されます。|  