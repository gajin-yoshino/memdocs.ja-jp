---
title: セットアップ ダウンローダー
titleSuffix: Configuration Manager
description: サイトのインストールで主要なインストール ファイルの最新バージョンが使用されることを保証するように設計されているこのスタンドアロン アプリケーションについて説明します。
ms.date: 01/22/2020
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: conceptual
ms.assetid: bda87fc5-2e4c-4992-98a4-01770365038c
author: mestew
ms.author: mstewart
manager: dougeby
ms.openlocfilehash: 2fa1899f8e7dc14812f9f9ecf889350a153b2d25
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81700570"
---
# <a name="setup-downloader-for-configuration-manager"></a>Configuration Manager のセットアップ ダウンローダー

*適用対象:Configuration Manager (Current Branch)*

セットアップを実行して Configuration Manager サイトをインストールまたはアップグレードする前に、インストールするバージョンの Configuration Manager からこのセットアップ ダウンローダー スタンドアロン アプリケーションを使用して、最新のセットアップ ファイルをダウンロードすることができます。  

最新のセットアップ ファイルを使用することにより、サイト インストールで主要なインストール ファイルの最新バージョンが確実に使用されます。 概要:   
-   セットアップを開始する前にセットアップ ダウンローダーを使用してファイルをダウンロードするときは、ファイルを格納するフォルダーを指定します。  
-   セットアップ ダウンローダーの実行に使用するアカウントには、ダウンロード フォルダーへの**フル コントロール** アクセス許可が必要です。  
-   サイトをインストールまたはアップグレードするためのセットアップを実行するとき、このファイルのローカル コピー (過去にダウンロードしたもの) を使うようセットアップ プログラムに指示することができます。 その場合、サイトのインストールやアップグレードを開始する際、Microsoft に接続する必要はありません。  
-   以降のサイトのインストールまたはアップグレードには同じセットアップ ファイル (ローカル コピー) を使用することができます。  

セットアップ ダウンローダーでダウンロードされるファイルの種類は次のとおりです。  
-   前提条件となる必須の再頒布可能ファイル  
-   言語パック  
-   セットアップ用の最新の製品の更新プログラム  

セットアップ ダウンローダーを実行するための 2 つのオプションがあります。
- ユーザー インターフェイスを使用してアプリケーションを実行する
- コマンド ライン オプションの場合は、コマンド プロンプトでアプリケーションを実行する


## <a name="run-setup-downloader-with-the-user-interface"></a><a name="bkmk_ui"></a> ユーザー インターフェイスでセットアップ ダウンローダーを実行する  

1.  インターネットにアクセスできるコンピューターでエクスプローラーを開き、 **&lt;Configuration Manager のインストール メディア\>\SMSSETUP\BIN\X64** に移動します。  

2.  セットアップ ダウンローダーを開くには、**setupdl.exe** をダブルクリックします。   

3. 更新されたインストール ファイルをホストするフォルダーのパスを指定して、 **[ダウンロード]** をクリックします。 セットアップ ダウンローダーにより、現在ダウンロード フォルダーにあるファイルが確認されます。 失われているファイルまたは既存のファイルより新しいファイルだけをダウンロードします。 ダウンロードした言語ごとのサブフォルダーと他の必要なサブフォルダーが作成されます。  

4.  ダウンロード結果を確認するには、C ドライブのルートにある **ConfigMgrSetup.log** ファイルを開きます。  

## <a name="run-setup-downloader-from-a-command-prompt"></a><a name="bkmk_cmd"></a> コマンド プロンプトからセットアップ ダウンローダーを実行する  

1.  コマンド プロンプト ウィンドウで **&lt;*Configuration Manager インストール メディア*\>\SMSSETUP\BIN\X64** に移動します。   

2.  セットアップ ダウンローダーを開くには、**setupdl.exe** を実行します。

    次のコマンド ライン オプションを **Setupdl.exe** で使用できます。   

    -   **/VERIFY**:このオプションを使用して、ダウンロード先フォルダーにあるファイル (言語ファイルも含む) を確認します。 最新でないファイルのリストが、C ドライブのルート ディレクトリにある ConfigMgrSetup.log ファイルに記録されます。 このオプションを使用したときは、ファイルは何もダウンロードされません。  

    -   **/VERIFYLANG**:このオプションを使用して、ダウンロード先フォルダーにある言語ファイルを確認します。 最新でない言語ファイルのリストが、C ドライブのルート ディレクトリにある ConfigMgrSetup.log ファイルに記録されます。

    -   **/LANG**:このオプションを使用して、言語ファイルのみをダウンロード先フォルダーにダウンロードします。  

    -   **/NOUI**:このオプションを使用して、ユーザー インターフェイスを表示せずにセットアップ ダウンローダーを起動します。 このオプションを使用する場合は、**ダウンロード先のパス**をコマンド プロンプトで指定する必要があります。  

    -   **&lt;DownloadPath\>** :検証プロセスまたはダウンロードプロセスを自動的に開始するために、ダウンロード先フォルダーへのパスを指定することができます。 **/NOUI** オプションを使用するときは、ダウンロード パスを指定する必要があります。 ダウンロード先パスを指定しなかった場合は、セットアップ ダウンローダーが開いたときにパスを指定する必要があります。 指定したフォルダーが存在しない場合は、新しく作成されます。  

    コマンドの例:

    -   **setupdl &lt;DownloadPath\>**  

        -   セットアップ ダウンローダーを起動して、指定のダウンロード フォルダーにあるファイルを確認し、不足しているファイルと、既存のファイルより新しいバージョンだけをダウンロードします。     

    -   **setupdl /VERIFY &lt;DownloadPath\>**  

        -   セットアップ ダウンローダーを起動して、指定のダウンロード フォルダーにあるファイルを確認します。  

    -   **setupdl /NOUI &lt;DownloadPath\>**  

        -   セットアップ ダウンローダーを起動して、指定のダウンロード フォルダーにあるファイルを確認し、不足しているファイルと、既存のファイルより新しいファイルだけをダウンロードします。  

    -   **setupdl /LANG  &lt;DownloadPath\>**  

        -   セットアップ ダウンローダーが開始し、指定のダウンロード フォルダーにある言語ファイルを確認し、不足している言語ファイルと、既存のファイルより新しい言語ファイルだけをダウンロードします。  

    -   **setupdl /VERIFY**  

        -   セットアップ ダウンローダーを起動してから、ダウンロード フォルダーへのパスを指定する必要があります。 次に、 **[検証]** をクリックして、セットアップ ダウンローダーでダウンロード フォルダーにあるファイルを確認します。  

3.  ダウンロード結果を確認するには、C ドライブのルート ディレクトリにある **ConfigMgrSetup.log** ファイルを開きます。

## <a name="copy-setup-downloader-files-to-another-computer"></a><a name="bkmk_cp-files"></a> セットアップ ダウンローダー ファイルを別のコンピューターにコピーする

1. エクスプローラーで、次のいずれかの場所に移動します。

    - **&lt;Configuration Manager インストール メディア>\SMSSETUP\BIN\X64**
    - **&lt;Configuration Manager インストール パス>\BIN\X64**
    
1. 次のファイルを別のコンピューターの同じフォルダーにコピーします。
    
    - **setupdl.exe**
    - **.\\&lt;言語>\\setupdlres.dll**
      - このファイルはインストール言語のサブフォルダーにあります。 たとえば、英語は `00000409` サブフォルダーにあります。

    たとえば、デバイスの宛先フォルダーは次のようになります。
    - C:\ConfigManInstall\setupdl.exe
    - C:\ConfigManInstall\00000409\setupdlres.dll

1. [ユーザー インターフェイス](#bkmk_ui)または前のセクションで説明した[コマンド プロンプト](#bkmk_cmd)のどちらかを使用して、コンピューターからセットアップダウンローダーを起動します。