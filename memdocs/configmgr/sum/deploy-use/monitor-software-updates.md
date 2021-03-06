---
title: ソフトウェア更新プログラムの監視
titleSuffix: Configuration Manager
description: Configuration Manager コンソールには、更新プログラムとコンプライアンスを監視するためのアラートとステータスがあります。
author: mestew
ms.author: mstewart
manager: dougeby
ms.date: 04/21/2020
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-sum
ms.assetid: 9afd7b0f-5c8e-48bc-9a65-1f7d74103688
ms.openlocfilehash: aa17e58f2687a48895502d1062a22d0c8630aea3
ms.sourcegitcommit: 2cafbba6073edca555594deb99ae29e79cd0bc79
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/24/2020
ms.locfileid: "82110374"
---
# <a name="monitor-software-updates-in-configuration-manager"></a>Configuration Manager でのソフトウェア更新プログラムの監視

*適用対象:Configuration Manager (Current Branch)*

Configuration Manager コンソールには、ソフトウェア更新プログラムのオブジェクト、プロセス、コンプライアンスの情報を監視するのに役立つさまざまな手段があります。 以下のセクションではソフトウェア更新プログラムの監視について説明します。

## <a name="software-updates-dashboard"></a>ソフトウェア更新プログラム ダッシュボード

*(バージョン 1610 で導入)*

Configuration Manager バージョン 1610 以降、ソフトウェア更新プログラム ダッシュボードを使用して、組織内にあるデバイスの現在のコンプライアンス状態を確認し、データをすばやく分析して危険な状態のデバイスを確認できるようになりました。 ダッシュボードを表示するには、 **[監視]**  >  **[概要]**  >  **[セキュリティ]**  >  **[ソフトウェア更新プログラム ダッシュボード]** に移動します。

## <a name="drill-through-required-updates"></a>必要な更新プログラムをドリルスルーする
<!--4224414-->
*(バージョン 1906 で導入)*

特定の Microsoft 365 アプリのソフトウェア更新プログラムが必要なデバイスを確認するため、コンプライアンスに関する統計情報をドリルダウンできます。 デバイスの一覧を表示するには、更新プログラムとデバイスが属するコレクションを表示するアクセス許可が必要です。 デバイスの一覧にドリルダウンするには、次のようにします。

1. **[ソフトウェア ライブラリ]**  >  **[ソフトウェア更新プログラム]**  >  **[すべてのソフトウェア更新プログラム]** の順に移動します。
1. 少なくとも 1 つのデバイスによって必要とされる任意の更新プログラムを選択します。
1. **[概要]** タブを表示して、 **[統計情報]** で円グラフを見つけます。
1. 円グラフの横にある **[View Required]\(必須の表示\)** ハイパーリンクを選択して、デバイスの一覧にドリルダウンします。
1. このアクションによって、更新プログラムが必要なデバイスを確認できる **[デバイス]** の下の一時ノードに移動されます。 一覧から新しいコレクションを作成するなど、ノードに対して操作を行うこともできます。


##  <a name="alerts-for-software-updates"></a><a name="BKMK_SUAlerts"></a> ソフトウェア更新プログラムのアラート  
 ソフトウェア更新プログラムのアラートを構成して、構成されている割合 (%) よりもソフトウェア更新プログラムの展開のコンプライアンス レベルが低い場合に管理ユーザーに通知できます。 ソフトウェア更新プログラムの展開のアラートは、次の場所で構成することができます。  

-   ADR の設定:自動展開規則の作成ウィザードおよび ADR のプロパティでアラートの設定を構成できます。  

-   展開の設定: ソフトウェア更新プログラムの展開ウィザードおよび展開プロパティでアラートの設定を構成することができます。  

アラートの設定を構成すると、指定されているアラート条件に一致した場合に、Configuration Manager でアラートが生成されます。 ソフトウェア更新プログラムのアラートは、次の場所で確認することができます。  

1.  最新のアラートを確認するには、 **[ソフトウェア ライブラリ]** ワークスペースの **[ソフトウェア更新プログラム]** ノードを使用します。  

2.  構成されているアラートを管理するには、 **[監視]** ワークスペースの **[アラート]** ノードを使用します。  

##  <a name="software-updates-synchronization-status"></a><a name="BKMK_SUSyncStatus"></a> ソフトウェア更新プログラムの同期ステータス  
 同期プロセスを開始した後に、階層内のすべてのソフトウェア更新プログラムの同期プロセスを Configuration Manager コンソールで監視することができます。 ソフトウェア更新プログラムの同期プロセスを監視するには、次の手順に従います。  

#### <a name="to-monitor-the-software-updates-synchronization-process"></a>ソフトウェア更新プログラムの同期プロセスを監視するには  

- Configuration Manager コンソールで、 **[監視]**  >  **[概要]**  >  **[ソフトウェアの更新ポイントの同期ステータス]** に移動します。  

    Configuration Manager 階層内のソフトウェアの更新ポイントが結果ウィンドウに表示されます。 このビューから、すべてのソフトウェアの更新ポイントの同期ステータスを監視することができます。 同期プロセスの詳細情報を確認するには、各サイト サーバーの <*Configuration Manager のインストール パス*>\Logs にある wsyncmgr.log ファイルを確認してください。  

##  <a name="software-update-deployment-status"></a><a name="BKMK_SUDeployStatus"></a> ソフトウェア更新プログラムの展開ステータス  
 ソフトウェア更新プログラム グループのソフトウェア更新プログラムを展開した後、または個々のソフトウェア更新プログラムを展開した後に、展開ステータスを監視することができます。 ソフトウェア更新プログラム グループまたはソフトウェア更新プログラムの展開ステータスを監視するには、次の手順に従います。  

#### <a name="to-monitor-deployment-status"></a>展開ステータスを監視するには  

1.  Configuration Manager コンソールで、 **[監視]**  >  **[概要]**  >  **[展開]** に移動します。  

2.  展開ステータスを監視するソフトウェア更新プログラム グループ、またはソフトウェア更新プログラムをクリックします。  

3.  **[ホーム]** タブの **[展開]** グループで、 **[ステータスの表示]** をクリックします。  

##  <a name="software-updates-reports"></a><a name="BKMK_SUReports"></a> ソフトウェア更新プログラムのレポート  
 ソフトウェアの更新の状態メッセージには、ソフトウェア更新プログラムのコンプライアンスに関する情報と、ソフトウェア更新プログラムの展開についての評価と強制実行状態に関する情報が示されます。 ソフトウェア更新プログラム レポートを実行して、これらの状態メッセージを表示できます。 事前定義されたソフトウェア更新プログラム レポートが 30 種類以上用意されています。 レポートは複数のカテゴリに分類され、ソフトウェア更新プログラムと展開に関する特定の情報のレポートに使用できます。 事前に構成されたレポートを使用するだけでなく、企業のニーズに応じて、ソフトウェア更新プログラムのカスタム レポートを作成することもできます。 詳しくは、「[レポートの操作とメンテナンス](../../core/servers/manage/operations-and-maintenance-for-reporting.md)」を参照してください。  

### <a name="recommended-software-updates-reports"></a>推奨されるソフトウェア更新プログラムのレポート
潜在的な問題を特定するのに便利なレポートの一部を次に示します。 

#### <a name="compliance-9---overall-health-and-compliance-starting-in-version-1806"></a>コンプライアンス 9 - 全体的な正常性とコンプライアンス (バージョン 1806 以降)
レポートには、次の部分が含まれます。

- **[正常なクライアント対合計クライアント]** :この棒グラフでは、指定した期間にサイトと通信した "正常な" クライアントと、指定したコレクション内のクライアントの合計数が比較されます。
- **[Compliance Overview]\(コンプライアンスの概要\)** :この円グラフでは、指定したコレクション内のアクティブなクライアントでの、特定のソフトウェア更新プログラム グループの全体的なコンプライアンスの状態が示されます。
- **[Top 5 Non-Compliant by Article ID]\(アーティクル ID 別非準拠トップ 5\)** :この棒グラフでは、指定したコレクション内のアクティブなクライアントで準拠していない、指定したグループ内の上位 5 つのソフトウェア更新プログラムが表示されます。
- レポートの下部には、さらなる詳細のテーブルがあります。ここでは、指定したグループ内のソフトウェア更新プログラムが一覧表示されます。

#### <a name="management-2---updates-required-but-not-deployed"></a>管理 2 - 更新が必要であるが展開されていない

このレポートは、クライアントに必須として検出されているがまだ特定のコレクションに展開されていない、特定の更新プログラムの分類に含まれるベンダー固有のソフトウェア更新プログラムを表示します。 

#### <a name="troubleshooting-2---deployment-errors"></a>トラブルシューティング 2 - 展開エラー

このレポートには、サイトで発生した展開エラーと、各エラーが発生したコンピューターの数が表示されます。 


##  <a name="monitor-content"></a><a name="BKMK_MonitorContent"></a> コンテンツの監視  
 Configuration Manager コンソールでコンテンツを監視して、関連する配布ポイントについて、すべての種類のパッケージのステータスを確認できます。 たとえば、パッケージのコンテンツのコンテンツ検証ステータス、特定の配布ポイント グループに割り当てられているコンテンツのステータス、配布ポイントに割り当てられているコンテンツの状態、各配布ポイントのオプション機能 (コンテンツ検証、PXE、マルチキャスト) のステータスなどを確認できます。  

###  <a name="content-status-monitoring"></a><a name="BKMK_ContentStatus"></a> コンテンツのステータスの監視  
 **[監視]** ワークスペースの **[コンテンツのステータス]** ノードには、コンテンツ パッケージについての情報が表示されます。 パッケージに関する全般情報、パッケージの配布ステータス、およびパッケージに関する詳細なステータス情報を確認することができます。 コンテンツのステータスを表示するには、次の手順に従います。  

#### <a name="to-monitor-content-status"></a>コンテンツのステータスを監視するには  

1.  Configuration Manager コンソールで、 **[監視]**  >  **[概要]**  >  **[配布ステータス]**  >  **[コンテンツのステータス]** に移動します。 パッケージが表示されます。  

2.  詳細なステータス情報を確認するパッケージを選択します。  

3.  **[ホーム]** タブで **[ステータスの表示]** をクリックします。 パッケージのステータスの詳細な情報が表示されます。  

###  <a name="distribution-point-group-status"></a><a name="BKMK_DPGroupStatus"></a> 配布ポイント グループのステータス  
 **[監視]** ワークスペースの **[配布ポイント グループのステータス]** ノードには、配布ポイント グループについての情報が表示されます。 配布ポイント グループに関する全般情報 (配布ポイント グループのステータスやコンプライアンス対応率など) に加え、配布ポイント グループの詳細なステータス情報も確認することができます。 次の手順に従って、配布ポイント グループのステータスを確認します。  

#### <a name="to-monitor-distribution-point-group-status"></a>配布ポイント グループのステータスを監視するには  

1.  Configuration Manager コンソールで、 **[監視]**  >  **[概要]**  >  **[配布ステータス]**  >  **[配布ポイント グループのステータス]** に移動します。 すると、配布ポイント グループが表示されます。  

2.  詳細なステータス情報を確認する配布ポイント グループを選択します。  

3.  **[ホーム]** タブで **[ステータスの表示]** をクリックします。 配布ポイント グループの詳細なステータス情報が表示されます。  

###  <a name="distribution-point-configuration-status"></a><a name="BKMK_DPConfigStatus"></a> 配布ポイントの構成ステータス  
 **[監視]** ワークスペースの **[配布ポイントの構成ステータス]** ノードには、配布ポイントについての情報が表示されます。 PXE、マルチキャスト、コンテンツの検証など、配布ポイントでどの属性が有効になっているかを確認できます。 また、配布ポイントの詳細なステータス情報も見ることができます。 次の手順に従って、配布ポイント グループの構成ステータスを確認します。  

#### <a name="to-monitor-distribution-point-configuration-status"></a>配布ポイントの構成ステータスを監視するには  

1.  Configuration Manager コンソールで、 **[監視]**  >  **[概要]**  >  **[配布ステータス]**  >  **[配布ポイントの構成のステータス]** に移動します。 すると、配布ポイントが表示されます。  

2.  配布ポイント ステータス情報を確認する配布ポイントを選択します。  

3.  [結果] ウィンドウで、 **[詳細]** タブをクリックします。すると、配布ポイントのステータス情報が表示されます。  

## <a name="next-steps"></a>次のステップ

- [ソフトウェア更新プログラムのログ ファイル](../../core/plan-design/hierarchy/log-files.md#BKMK_SU_NAPLog)

- [ソフトウェア更新プログラムの管理に関するホワイトペーパー](https://www.microsoft.com/download/confirmation.aspx?id=44578)
