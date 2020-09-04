---
title: クラウド管理ゲートウェイの計画
titleSuffix: Configuration Manager
description: インターネットを基盤とするクライアントの管理を簡素化するクラウド管理ゲートウェイ (CMG) を計画し、設計します。
ms.date: 08/11/2020
ms.prod: configuration-manager
ms.technology: configmgr-client
ms.topic: conceptual
ms.assetid: 2dc8c9f1-4176-4e35-9794-f44b15f4e55f
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: d5b9a65b768d02d02084d778fd36255341a808b2
ms.sourcegitcommit: 99084d70c032c4db109328a4ca100cd3f5759433
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2020
ms.locfileid: "88692844"
---
# <a name="plan-for-the-cloud-management-gateway-in-configuration-manager"></a>Configuration Manager でクラウド管理ゲートウェイを計画する

*適用対象:Configuration Manager (Current Branch)*

<!--1101764-->
クラウド管理ゲートウェイ (CMG) は、インターネット上で Configuration Manager クライアントを管理する簡単な方法を提供します。 Microsoft Azure のクラウド サービスとして CMG を展開することで、オンプレミスのインフラストラクチャを追加せずに、インターネット上を移動する従来のクライアントを管理できます。 また、オンプレミス インフラストラクチャをインターネットに公開する必要がありません。

> [!NOTE]
> Configuration Manager では、このオプション機能は既定で無効です。 この機能は、使用する前に有効にする必要があります。 詳細については、「[Enable optional features from updates](../../../servers/manage/install-in-console-updates.md#bkmk_options)」 (更新プログラムのオプション機能の有効化) を参照してください。<!--505213-->  

前提条件を確立した後、Configuration Manager コンソールで次の 3 つの手順を行うことで CMG が作成されます。

1. CMG クラウド サービスを Azure に展開します。
2. CMG 接続ポイント ロールを追加します。
3. サービスのサイトとサイト ロールを構成します。
展開と構成が完了すると、クライアントはイントラネットにあるかインターネットにあるかに関わらず、オンプレミス サイト ロールにシームレスにアクセスします。

この記事では、CMG について知る、自分の環境に合うように設計する、実装を計画するための基礎知識を提供します。

## <a name="scenarios"></a>シナリオ

CMG が有益となるシナリオがいくつかあります。 より一般的なシナリオは次のようなシナリオです。  

- Active Directory ドメイン参加 ID のある従来の Windows クライアントを管理します。 このようなクライアントには、Windows 8.1 と Windows 10 があります。 PKI 証明書を使用して通信チャネルをセキュリティで保護します。 管理作業には次が含まれます。  

  - ソフトウェア更新とエンドポイント保護
  - インベントリとクライアントの状態
  - コンプライアンス設定
  - デバイスにソフトウェアを配布
  - Windows 10 一括アップグレード タスク シーケンス

- 最新の ID を持つ、つまり、Azure Active Directory (Azure AD) に混種か単種でクラウド ドメイン参加している従来の Windows 10 クライアントを管理します。 クライアントでは、PKI 証明書ではなく、Azure AD が認証に利用されます。 Azure AD を利用する場合、複雑な PKI システムより設定、構成、保守管理が簡単です。 管理作業は最初のシナリオと同じものに次が加わります。  

  - ユーザーにソフトウェアを配布  

- インターネット経由で Configuration Manager クライアントを Windows 10 デバイスにインストールします。 Azure AD を利用すると、デバイスでは、クライアントの登録や割り当てのために CMG に認証できます。 クライアントは手動でインストールできます。あるいは、Microsoft Intune など、別のソフトウェア配布方法を利用できます。  

- 新しいデバイス プロビジョニングと共同管理。 既存のクライアントを自動登録する場合、共同管理に CMG は必要ありません。 Windows AutoPilot、Azure AD、Microsoft Intune、Configuration Manager が関わる新しいデバイスに対しては必要です。 詳しくは、「[共同管理へのパス](../../../../comanage/quickstart-paths.md)」をご覧ください。

### <a name="specific-use-cases"></a>特定のユース ケース

以上のシナリオでは、次のような特定のデバイス ユース ケースが適用されることがあります。

- ラップトップなどのローミング デバイス  

- WAN または VPN よりもインターネットでの管理が安価で効率性が良いリモート/支店デバイス。  

- 合併と買収。Azure AD にデバイスを参加させ、CMG 経由で管理するのが簡単な場合があります。  

- ワークグループ クライアント。 これらのデバイスでは、証明書などの追加の構成が必要になる場合があります。<!-- SCCMDocs#1925 -->

    バージョン 2002 以降、Configuration Manager ではトークン ベースの認証がサポートされています。これはリモート ワークグループ クライアントの管理に役立つ場合があります。 詳細については、[CMG のトークンベースの認証](../../deploy/deploy-clients-cmg-token.md)に関する記事をご覧ください。

> [!Important]
> 既定ではすべてのクライアントが CMG のポリシーを受け取り、インターネットベースになるとき、その使用を開始します。 組織に当てはまるシナリオとユース ケースによっては、CMG の使い方を調べる必要があります。 詳細については、「[クライアントでクラウド管理ゲートウェイを使用できるようにする](../../deploy/about-client-settings.md#enable-clients-to-use-a-cloud-management-gateway)」のクライアント設定を参照してください。

## <a name="topology-design"></a>トポロジ設計

### <a name="cmg-components"></a>CMG コンポーネント

CMG の展開と操作には、次のコンポーネントが含まれます。  

- Azure の **CMG クラウド サービス**は、Configuration Manager クライアントの要求を認証し、CMG 接続ポイントに転送します。  

- **CMG 接続ポイント** サイト システム ロールによって、オンプレミス ネットワークから Azure の CMG サービスまで、一貫性があり、パフォーマンスが高い接続が可能になります。 また、接続情報やセキュリティ設定などの各種設定を CMG に発行します。 CMG 接続ポイントは、URL マッピングに基づき、CMG からオンプレミス ロールにクライアント要求を転送します。

- [**サービス接続ポイント**](../../../servers/deploy/configure/about-the-service-connection-point.md) サイト システム ロールは、あらゆる CMG 展開タスクを処理する、クラウド サービス マネージャー コンポーネントを実行します。 また、このコンポーネントは、Azure AD のサービスの正常性とログ情報も監視し、レポートします。 サービス接続ポイントが[オンライン モード](../../../servers/deploy/configure/about-the-service-connection-point.md#bkmk_modes)であることを確認します。  

- **管理ポイント** サイト システム ロールは通常のようにクライアント要求にサービスを提供します。

- **ソフトウェア更新ポイント** サイト システム ロールは通常のようにクライアント要求にサービスを提供します。

    > [!NOTE]
    > オンプレミスのクライアントとインターネットベースのクライアントのどちらを使用する場合でも、管理ポイントとソフトウェアの更新ポイントのサイズ設定に関するガイダンスは変わりません。 詳細については、[サイズとスケールの数値](../../../plan-design/configs/size-and-scale-numbers.md#management-point)に関する記事をご覧ください。

- **インターネットベース クライアント**は CMG に接続し、オンプレミスの Configuration Manager コンポーネントにアクセスします。

- CMG では、**証明書ベースの HTTPS** Web サービスを使用し、クライアントとのネットワーク通信をセキュリティで保護します。  

- インターネットベースのクライアントでは、ID や認証に **PKI 証明書または Azure AD** が使用されます。  

- [**クラウド配布ポイント**](../../../plan-design/hierarchy/use-a-cloud-based-distribution-point.md)によって、必要に応じて、インターネットベースのクライアントにコンテンツを提供されます。  

  - CMG では、コンテンツをクライアントに提供することもできます。 この機能により、Azure VM の必要な証明書とコストが削減されます。 詳細については、[CMG の変更](setup-cloud-management-gateway.md#modify-a-cmg)に関するページを参照してください。<!--1358651-->  

### <a name="azure-resource-manager"></a>Azure Resource Manager

<!-- 1324735 -->
**Azure Resource Manager の展開**を使用して CMG を作成します。 [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) は、[リソース グループ](/azure/azure-resource-manager/resource-group-overview#resource-groups)と呼ばれる単一のエンティティとしてすべてのソリューション リソースを管理するための最新のプラットフォームです。 Azure Resource Manager で CMG を展開するとき、サイトは Azure Active Directory (Azure AD) を使って必要なクラウド リソースの認証と作成を行います。 この最新の展開では、従来の Azure 管理証明書は必要ありません。  

> [!NOTE]
> この機能では、Azure クラウド サービス プロバイダー (CSP) のサポートは有効になりません。 Azure Resource Manager での CMG の展開では引き続き従来のクラウド サービスが使われ、CSP はこれをサポートしません。 詳細については、「[Available Azure services in Azure CSP](/azure/cloud-solution-provider/overview/azure-csp-available-services)」(Azure CSP で使用可能な Azure サービス) を参照してください。

Configuration Manager バージョン 1902 以降、クラウド管理ゲートウェイの新しいインスタンスに対しては、Azure Resource Manager が唯一の展開メカニズムです。 既存の展開は引き続き機能します。<!-- 3605704 -->

Configuration Manager バージョン 1810 以前の CMG ウィザードでは、Azure 管理証明書を使う**従来のサービス展開**のためのオプションがまだ提供されています。 リソースの展開と管理を簡単にするため、すべての新しい CMG インスタンスに Azure Resource Manager デプロイ モデルをお勧めします。 可能であれば、Resource Manager で既存の CMG インスタンスを再展開してください。 詳細については、[CMG の変更](setup-cloud-management-gateway.md#modify-a-cmg)に関するページを参照してください。

> [!IMPORTANT]
> Configuration Manager での Azure の従来のサービス デプロイの使用は非推奨です。 このような Azure デプロイの作成がサポートされるのは、バージョン 1810 が最後になります。 この機能は、将来の Configuration Manager バージョンで削除されます。<!--SCCMDocs-pr issue #2993-->  

### <a name="hierarchy-design"></a>階層の設計

階層の一番上のサイトで CMG を作成します。 それが中央管理サイトである場合は、子プライマリ サイトで CMG 接続ポイントを作成します。 クラウド サービス マネージャー コンポーネントは、中央管理サイトにもある、サービス接続ポイントにあります。 この設計によって、必要に応じて、異なるプライマリ サイト間でサービスを共有できます。

Azure で複数の CMG サービスを作成できます。また、複数の CMG 接続ポイントを作成できます。 CMG 接続ポイントが複数あると、CMG からオンプレミス ロールへのクライアント トラフィックが負荷分散されます。

バージョン 1902 以降、CMG を境界グループに関連付けることができるようになりました。 クライアントにこの構成を使用すると、[境界グループのリレーションシップ](../../../servers/deploy/configure/boundary-groups.md)に従って、クライアント通信に対して CMG を既定に設定したり、それにフォールバックしたりすることができます。 この動作は、ブランチ オフィスと VPN のシナリオで特に役立ちます。 クライアント トラフィックに高価で低速な WAN リンクを使用せず、代わりに Microsoft Azure の高速なサービスを使用できます。<!--3640932-->

バージョン 2006 より、イントラネット クライアントは、境界グループに割り当てられたときに、CMG ソフトウェアの更新ポイントにアクセスできます。 詳細については、[境界グループの構成](../../../servers/deploy/configure/boundary-groups.md#bkmk_cmg-sup)に関するページを参照してください。 <!--7102873-->

> [!NOTE]
> インターネットベース クライアントはどの境界グループにも分類されません。
>
> Configuration Manager バージョン 1810 以前では、CMG はどの境界グループにも分類されません。

管理するクライアントの数など、その他の要因も CMG の設計に影響を与えます。 詳細については、「[パフォーマンスと拡張性](#performance-and-scale)」を参照してください。

#### <a name="example-1-standalone-primary-site"></a>例 1: スタンドアロン プライマリ サイト

Contoso は、ニューヨーク市にある本社のオンプレミス データセンターにスタンドアロン プライマリ サイトを置いています。

- 米国東部 Azure リージョンに CMG を作成し、ネットワークの待ち時間を減らします。
- CMG 接続ポイントを 2 つ作成します。いずれも 1 つの CMG サービスにリンクされています。  

クライアントがインターネットに接続するとき、米国東部 Azure リージョンの CMG と通信します。 CMG はこの通信を両方の CMG 接続ポイントに転送します。

#### <a name="example-2-hierarchy"></a>例 2: 階層

Fourth Coffee は、シアトルの本社にあるオンプレミス データセンターに中央管理サイトを置いています。 プライマリ サイトが 1 つ同じデータセンターにあり、別のプライマリ サイトがパリのヨーロッパ統括オフィスにあります。

- 中央管理サイトで、米国西部 Azure リージョンに CMG サービスを作成します。 階層全体で、予想されるローミング クライアントの負荷に合わせて VM の数をスケーリングします。
- シアトルベースのプライマリ サイトで、単一の CMG にリンクされている CMG 接続ポイントを作成します。
- パリベースのプライマリ サイトで、単一の CMG にリンクされている CMG 接続ポイントを作成します。

クライアントがインターネットにローミングするとき、米国西部 Azure リージョンの CMG と通信します。 CMG によって、この通信が、クライアントの割り当てられたプライマリ サイト内にある CMG 接続ポイントに転送されます。

> [!TIP]
> 位置情報の目的で、複数のクラウド管理ゲートウェイをデプロイする必要はありません。 構成マネージャー クライアントは、地理的に離れている場合でも、クラウド サービスで発生するわずかな待機時間の影響をほとんど受けません。

### <a name="test-environments"></a>テスト環境
<!-- SCCMDocs#1225 -->
多くの組織には、運用、テスト、開発、または品質保証用に個別の環境があります。 CMG のデプロイを計画するときは、次の点を考慮してください。

- ご自身の組織には Azure AD テナントがいくつありますか?
  - テスト用の個別のテナントがありますか?
  - ユーザーとデバイスの ID は同じテナント内にありますか?

- 各テナントにはサブスクリプションがいくつありますか?
  - テスト専用のサブスクリプションはありますか?

Configuration Manager の**クラウド管理**用 Azure サービスでは、複数のテナントがサポートされています。 複数の Configuration Manager サイトを同じテナントに接続できます。 1 つのサイトで、複数の CMG サービスを異なるサブスクリプションにデプロイできます。 複数のサイトで、CMG サービスを同じサブスクリプションにデプロイできます。 Configuration Manager には、お客様の環境やビジネス要件に応じた柔軟性が備わっています。

詳細については、次の FAQ を参照してください: 「[ユーザー アカウントは、CMG クラウド サービスをホストするサブスクリプションに関連付けられているテナントと同じ Azure AD テナント内にある必要がありますか?](cloud-management-gateway-faq.md#bkmk_tenant)」

## <a name="requirements"></a>要件

- CMG をホストするための **Azure サブスクリプション**。

    > [!IMPORTANT]
    > CMG では、Azure クラウド サービス プロバイダー (CSP) を使用するサブスクリプションはサポートされていません。<!-- MEMDocs#320 -->

- 使用するユーザー アカウントが、Configuration Manager で**完全な権限を持つ管理者**または**インフラストラクチャ管理者**である必要があります。<!-- SCCMDocs#2146 -->

- **Azure 管理者**は、設計によっては、特定のコンポーネントの初回作成に参加する必要があります。 このペルソナは、Configuration Manager 管理者と同じでも、別でもかまいません。 別にした場合は、Configuration Manager のアクセス許可は必要ありません。

  - CMG をデプロイするには、**サブスクリプションの所有者**が必要です
  - Azure Resource Manager を使用して CMG を展開するためにサイトを Azure AD と統合するには、**全体管理者**が必要です。

- **CMG 接続ポイント**をホストするためのオンプレミス Windows サーバーが少なくとも 1 つ。 このロールと他の Configuration Manager サイト システム ロールは同じ場所に置くことができます。  

- **サービス接続ポイント**は[オンライン モード](../../../servers/deploy/configure/about-the-service-connection-point.md#bkmk_modes)にする必要があります。  

- Azure Resource Manager でサービスを展開するための **Azure AD** との統合。 詳細については、「[Azure サービスの構成](../../../servers/deploy/configure/azure-services-wizard.md)」を参照してください。  

- CMG の[**サーバー認証証明書**](certificates-for-cloud-management-gateway.md#bkmk_serverauth)。  

- クライアントの OS バージョンと認証モデルによっては、**他の証明書**が必要になることがあります。 詳細については、「[CMG 証明書](certificates-for-cloud-management-gateway.md)」を参照してください。  

    **[HTTP サイト システムには Configuration Manager によって生成された証明書を使用する]** サイト オプションを使用するときは、管理ポイントは HTTP でもかまいません。 詳細については、「[Enhanced HTTP](../../../plan-design/hierarchy/enhanced-http.md)」(拡張 HTTP) をご覧ください。

- Configuration Manager バージョン 1810 以前で、Azure クラシック デプロイ方法を利用している場合は、[**Azure 管理証明書**](certificates-for-cloud-management-gateway.md#bkmk_azuremgmt)が必要です。  

    > [!TIP]  
    > **Azure Resource Manager**の展開モデルを使用します。 この管理証明書は必要ありません。
    >
    > バージョン 1810 以降では、従来の展開方法は推奨されません。  

- クライアントでは **IPv4** を使用する必要があります。  

## <a name="specifications"></a>仕様

- [クライアントとデバイスのサポートされるオペレーティング システム](../../../plan-design/configs/supported-operating-systems-for-clients-and-devices.md)に記載されているあらゆるバージョンの Windows が CMG でサポートされています。  

- CMG では、管理ポイント ロールとソフトウェア更新ポイント ロールのみサポートされます。  

- CMG では、IPv6 アドレスとのみ通信するクライアントはサポートされません。<!--495606-->  

- ネットワーク ロード バランサーを利用するソフトウェア更新ポイントは CMG と連動しません。 <!--505311-->  

- Azure Resource Model を使用する CMG 展開では、Azure クラウド サービス プロバイダー (CSP) のサポートが有効になりません。 Azure Resource Manager での CMG の展開では引き続き従来のクラウド サービスが使われ、CSP はこれをサポートしません。 詳細については、[Azure CSP プログラムで利用可能な Azure サービス](/partner-center/azure-plan-available)に関する記事をご覧ください。

### <a name="support-for-configuration-manager-features"></a>Configuration Manager の機能のサポート

次の表は、Configuration Manager 機能の CMG サポートをまとめたものです。

|機能  |サポート  |
|---------|---------|
| ソフトウェア更新プログラム     | ![サポート](media/green_check.png) |
| エンドポイント保護     | ![サポート](media/green_check.png) <sup>[注記&nbsp;1](#bkmk_note1)</sup> |
| ハードウェアとソフトウェアのインベントリ     | ![サポート](media/green_check.png) |
| クライアント ステータスと通知     | ![サポート](media/green_check.png) |
| スクリプトの実行     | ![サポート](media/green_check.png) |
| CMPivot     | ![サポート](media/green_check.png) |
| コンプライアンス設定     | ![サポート](media/green_check.png) |
| クライアント インストール<br>([Azure AD 統合](../../deploy/deploy-clients-cmg-azure.md)を使用) | ![サポート](media/green_check.png) |
| クライアント インストール<br>([トークン認証](../../deploy/deploy-clients-cmg-token.md)を使用) | ![サポート](media/green_check.png) (2002) |
| ソフトウェア配布 (デバイスを対象に)     | ![サポート](media/green_check.png) |
| ソフトウェア配布 (ユーザーを対象とし、必須)<br>(Azure AD 統合で)     | ![サポート](media/green_check.png) |
| ソフトウェア配布 (ユーザーを対象とし、利用可能)<br>([すべての要件](../../../../apps/deploy-use/deploy-applications.md#deploy-user-available-applications)) | ![サポート](media/green_check.png) |
| Windows 10 [一括アップグレード タスク シーケンス](../../../../osd/deploy-use/create-a-task-sequence-to-upgrade-an-operating-system.md) | ![サポート](media/green_check.png) |
| **[タスク シーケンスを開始する前にすべてのコンテンツをローカルにダウンロードする]** オプションを指定して展開される、起動イメージのないタスク シーケンス | ![サポート](media/green_check.png) |
| [いずれかのダウンロード オプション](../../../../osd/deploy-use/deploy-a-task-sequence.md#deploy-windows-10-in-place-upgrade-via-cmg)を指定して展開される、起動イメージのないタスク シーケンス | ![サポート](media/green_check.png) (1910) |
| ソフトウェア センターから開始される、起動イメージのあるタスク シーケンス | ![サポートされています](media/green_check.png) (2006) |
| その他のタスク シーケンス シナリオ     | ![サポートされていません](media/Red_X.png) |
| クライアント プッシュ     | ![サポートされていません](media/Red_X.png) |
| サイトの自動割り当て     | ![サポートされていません](media/Red_X.png) |
| ソフトウェア承認要求     | ![サポートされていません](media/Red_X.png) |
| Configuration Manager コンソール     | ![サポートされていません](media/Red_X.png) |
| ［リモート ツール］     | ![サポートされていません](media/Red_X.png) |
| Web サイトのレポート     | ![サポートされていません](media/Red_X.png) |
| Wake On LAN     | ![サポートされていません](media/Red_X.png) |
| Mac、Linux、UNIX クライアント     | ![サポートされていません](media/Red_X.png) |
| ピア キャッシュ     | ![サポートされていません](media/Red_X.png) |
| オンプレミス MDM     | ![サポートされていません](media/Red_X.png) |
| BitLocker の管理     | ![サポートされていません](media/Red_X.png) |

|キー|
|--|
|![サポート](media/green_check.png) = この機能はサポートされているあらゆるバージョンの Configuration Manager によって、CMG でサポートされています  |
|![サポート](media/green_check.png) (*YYMM*) = この機能は、バージョン *YYMM* 以降の Configuration Manager によって、CMG でサポートされています  |
|![サポートされていません](media/Red_X.png) = この機能は CMG でサポートされていません |

#### <a name="note-1-support-for-endpoint-protection"></a><a name="bkmk_note1"></a> 注 1:エンドポイント保護のサポート

バージョン 2006 以降、CMG を介して通信するクライアントでは、Active Directory へのアクティブな接続なしで、すぐにエンドポイント保護ポリシーを適用できます。<!--4773948-->

<!-- 4350561 -->
バージョン 2002 以前の場合、ドメインに参加しているデバイスでエンドポイント保護ポリシーを適用するには、デバイスからドメインへのアクセスが必要です。 内部ネットワークへのアクセス頻度が低いデバイスでは、エンドポイント保護ポリシーの適用で遅延が発生することがあります。 デバイスがエンドポイント保護ポリシーを受信した後、直ちにそのポリシーを適用する必要がある場合は、次のいずれかのオプションを検討してください。

- サイトとクライアントをバージョン 2006 に更新します。

- 共同管理を使用し、[エンドポイント保護のワークロード](../../../../comanage/workloads.md#endpoint-protection)を Intune に切り替え、クラウドから [Microsoft Defender ウイルス対策](../../../../../intune/configuration/device-restrictions-windows-10.md#microsoft-defender-antivirus)を管理する。

- ネイティブの[マルウェア対策ポリシー](../../../../protect/deploy-use/endpoint-antimalware-policies.md)機能の代わりに[構成項目](../../../../compliance/deploy-use/create-configuration-items.md)を使用して、エンドポイント保護ポリシーを適用する。


## <a name="cost"></a>コスト

> [!IMPORTANT]  
> 以下のコスト情報は、見積もり目的のみで提供されます。 ご利用の環境によっては、変動要素が他にも存在し、それが CMG 使用の総コストに影響を与えることがあります。

CMG では次の Azure コンポーネントが利用され、Azure サブスクリプション アカウントへの課金が発生します。

### <a name="virtual-machine"></a>仮想マシン

- CMG では、PaaS (サービスとしてのプラットフォーム) として Azure Cloud Services が使用されます。 このサービスでは、計算処理コストを発生させる仮想マシン (VM) が使用されます。  

- CMG は Standard A2 V2 VM を使用します。  

- CMG をサポートする VM インスタンスの数を選択します。 1 が既定値で、16 が最大値です。 この数値は CMG の作成時に設定されます。後で変更し、必要に応じてサービスの規模を変更できます。

- クライアントをサポートするために必要な VM の数については、「[パフォーマンスと拡張性](#performance-and-scale)」を参照してください。

- 予想されるコストについては、「[Azure の料金計算ツール](https://azure.microsoft.com/pricing/calculator/)」を参照してください。

    > [!NOTE]  
    > 仮想マシンのコストは地域によって異なります。

### <a name="outbound-data-transfer"></a>送信データの転送

- 課金は Azure から外に出るデータに基づきます (エグレスまたはダウンロード)。 Azure に入ってくるデータは無料です (イングレスまたはアップロード)。 Azure から外に出る CMG データには、クライアントのポリシー、クライアント通知、CMG からサイトに転送されるクライアント応答があります。 このような応答には、インベントリ レポート、ステータス メッセージ、コンプライアンス ステータスがあります。  

- CMG とクライアントの通信がなくても、何らかのバックグラウンド通信によって、CMG とオンプレミス サイトの間にネットワーク トラフィックが発生します。  

- Configuration Manager コンソールで**送信データ転送 (GB)** を表示してください。 詳細については、[CMG でクライアントを監視する方法](monitor-clients-cloud-management-gateway.md)に関するページを参照してください。  

- 予想されるコストについては、「[Azure 帯域幅の料金詳細](https://azure.microsoft.com/pricing/details/bandwidth/)」を参照してください。 データ転送の価格設定は階層になっています。 使えば使うほど、ギガバイトあたりで支払う料金が安くなります。  

- インターネットベースのクライアントの場合、クライアントあたり毎月約 100-300 MB が予想されます (*この数値は見積もり目的でのみ提供されます*)。 下の見積もりは、既定のクライアント構成用です。 上の見積もりは、もっと活動的なクライアント構成用です。 実際の使用は、クライアント設定に基づいて変わることがあります。  

    > [!NOTE]  
    > ソフトウェア更新やアプリケーションの展開など、他のアクションを実行すると、Azure から外に出るデータ転送の量が増えます。

- インターネットベース クライアントは、Windows Update から無料で Microsoft ソフトウェア更新コンテンツを受け取ります。 Microsoft 更新コンテンツが含まれる更新パッケージをクラウド配布ポイントに配布しないでください。配布すると、ストレージとデータ エグレスのコストが発生することがあります。  

- CMG オプションを誤って **[クライアント証明書の失効状態の検証]** に構成すると、クライアントから CMG への追加のトラフィックが発生する場合があり、 その追加のトラフィックにより、Azure の出力データが増加して、Azure のコストが増加する可能性があります。<!-- SCCMDocs#1434 --> 詳細については、「[証明書失効リストを発行する](security-and-privacy-for-cloud-management-gateway.md#bkmk_crl)」を参照してください。  

### <a name="content-storage"></a>コンテンツ ストレージ

- インターネットベース クライアントは、Windows Update から無料で Microsoft ソフトウェア更新コンテンツを受け取ります。 Microsoft 更新コンテンツが含まれる更新パッケージをクラウド配布ポイントに配布しないでください。配布すると、ストレージとデータ エグレスのコストが発生することがあります。  

- アプリケーションやサードパーティのソフトウェア更新など、他の必要なコンテンツの場合、クラウドの配布ポイントに配布する必要があります。 現在のところ、CMG では、コンテンツをクライアントに送信するクラウドの配布ポイントのみをサポートしています。
   - コンテンツの保存に CMG を使用する場合、[クライアント設定](../../deploy/about-client-settings.md#allow-clients-to-download-delta-content-when-available)の **[Download delta content when available]\(デルタ コンテンツが使用可能な場合はダウンロードする\)** が有効になっていると、サードパーティの更新プログラムのコンテンツはクライアントにダウンロードされません。 <!--6598587--> 

- 詳細については、[クラウド配布ポイント](../../../plan-design/hierarchy/use-a-cloud-based-distribution-point.md#bkmk_cost)の使用のコストに関するページを参照してください。  

- クライアントにコンテンツを提供するクラウド配布ポイントとして CMG も使用できるようになりました。 この機能により、Azure VM の必要な証明書とコストが削減されます。 詳細については、[CMG の変更](setup-cloud-management-gateway.md#modify-a-cmg)に関するページを参照してください。<!--1358651-->  

- CMG では Azure ローカル冗長ストレージ (LRS) が使用されています。 詳細については、[ローカル冗長ストレージ](/azure/storage/common/storage-redundancy-lrs)に関するページを参照してください。  

### <a name="other-costs"></a>その他のコスト

- クラウド サービスにはそれぞれ、動的 IP アドレスが与えられます。 個々の CMG で新しい動的 IP アドレスが使用されます。 CMG あたりの VM を追加しても、このようなアドレスは増えません。  

## <a name="performance-and-scale"></a>パフォーマンスと拡張性

CMG の拡張性については、「[サイジングとスケールの数値](../../../plan-design/configs/size-and-scale-numbers.md#bkmk_cmg)」をご覧ください。

次は、CMG パフォーマンスを改善するための推奨事項です。

- 構成マネージャー クライアントと CMG 間の接続は、リージョンを意識していません。 クライアントの通信の大部分は、待ち時間 / 地理的分離から影響を受けません。 geo 近接の目的で複数の CMG を展開する必要はありません。 階層内の最上位サイトに CMG を展開し、インスタンスを追加してスケールを拡大します。

- サービスの可用性を高くするには、少なくとも 2 つの CMG インスタンスとサイトあたり 2 つの CMG 接続ポイントと共に CMG を作成します。  

- VM インスタンスを追加することで、より多くのクライアントをサポートできるように CMG を拡張します。 Azure Load Balancer では、クライアントのサービスへの接続が制御されます。  

- CMG 接続ポイントを増やすことで、ポイント間の負荷を分散できます。 CMG では、それが接続している CMG 接続ポイントにラウンドロビンでトラフィックが送信されます。  

- サポートしているクライアント数を超えたことで CMG に高い負荷がかかっているときでも要求は処理されますが、遅延が発生することがあります。  

> [!NOTE]
> Configuration Manager では、CMG 接続ポイントのクライアント数に対して絶対的な上限が課されていませんが、Windows Server では、TCP 動的ポートの既定の最大範囲が 16,384 になっています。 Configuration Manager サイトで管理している (CMG 接続ポイントが 1 つの) クライアントが 16,384 を超える場合、Windows Server の上限を増やす必要があります。 すべてのクライアントでクライアント通知のためのチャネルが保守管理されています。このチャネルは CMG 接続ポイントでポートを開いています。 netsh コマンドを使用してこの上限を増やす方法については、[Microsoft サポート記事 929851](https://support.microsoft.com/help/929851) を参照してください。

## <a name="ports-and-data-flow"></a>ポートとデータ フロー

オンプレミス ネットワークに受信ポートを開く必要はありません。 サービス接続ポイントと CMG 接続ポイントによって、Azure と CMG とのあらゆる通信が開始されます。 これらの 2 つのサイト システム ロールでは、Microsoft クラウドへの送信接続を作成する必要があります。 サービス接続ポイントは Azure でサービスを展開し、監視します。そのため、オンライン モードにする必要があります。 CMG 接続ポイントは CMG に接続し、CMG とオンプレミス サイト システム ロールの間の通信を管理します。

CMG の概念を表す基本的なデータ フロー図:

[![CMG データ フロー](media/cmg-data-flow.png)](media/cmg-data-flow.png#lightbox)

1. サービス接続ポイントは HTTPS ポート 443 で Azure に接続します。 Azure AD または Azure 管理証明書を利用して認証します。 サービス接続ポイントによって Azure で CMG が展開されます。 CMG によって、サーバー認証証明書を利用し、HTTPS クラウド サービスが作成されます。  

2. CMG 接続ポイントは TCP TLS または HTTPS で Azure の CMG に接続します。 接続のオープン状態を維持し、将来の 2 方向通信のためのチャネルを構築します。  

3. クライアントは HTTPS ポート 443 で CMG に接続します。 Azure AD またはクライアント認証証明書を利用して認証します。  

    > [!NOTE]
    > CMG でコンテンツの提供またはクラウド配布ポイントの使用を有効にした場合、クライアントは HTTPS ポート 443 を介して Azure BLOB ストレージに直接接続されます。 詳細については、[クラウドベースの配布ポイントの使用](../../../plan-design/hierarchy/use-a-cloud-based-distribution-point.md#bkmk_dataflow)に関する記事をご覧ください。<!-- SCCMDocs#2332 -->

4. CMG では、既存の接続を利用し、クライアント接続がオンプレミス CMG 接続ポイントに転送されます。 受信ファイアウォール ポートを開く必要はありません。  

5. CMG 接続ポイントによってクライアント通信がオンプレミス管理ポイントとソフトウェア更新ポイントに転送されます。  

Azure でコンテンツをホストする場合の詳細については、「[クラウドベースの配布ポイントの使用](../../../plan-design/hierarchy/use-a-cloud-based-distribution-point.md#bkmk_dataflow)」を参照してください。

### <a name="required-ports"></a>必要なポート

この表は、必要なネットワーク ポートとプロトコルをまとめたものです。 *クライアント*は接続を開始し、送信ポートを必要とするデバイスです。 *サーバー*は接続を承認し、受信ポートを必要とするデバイスです。

| クライアント | プロトコル | ポート | サーバー | [説明] |
|--------|----------|------|--------|-------------|
| [サービス接続ポイント] | HTTPS | 443 | Azure | CMG のデプロイ |
| CMG 接続ポイント | TCP-TLS | 10140-10155 | CMG サービス | CMG チャネルを構築するための優先プロトコル <sup>[注 1](#bkmk_port-note1)</sup> |
| CMG 接続ポイント | HTTPS | 443 | CMG サービス | ただ 1 つの VM インスタンスに CMG チャネルを構築するためのフォールバック プロトコル <sup>[注 2](#bkmk_port-note2)</sup> |
| CMG 接続ポイント | HTTPS | 10124-10139 | CMG サービス | 複数の VM インスタンスに CMG チャネルを構築するためのフォールバック プロトコル <sup>[注 3](#bkmk_port-note3)</sup> |
| クライアント | HTTPS | 443 | CMG | 一般クライアント通信 |
| クライアント | HTTPS | 443 | BLOB ストレージ | クラウドベースのコンテンツをダウンロード |
| CMG 接続ポイント | HTTPS または HTTP | 443 または 80 | 管理ポイント | オンプレミス トラフィック、ポートは管理ポイント構成に依存 |
| CMG 接続ポイント | HTTPS または HTTP | 443 または 80 | ソフトウェアの更新ポイント | オンプレミス トラフィック、ポートはソフトウェア更新ポイント構成に依存 |

#### <a name="note-1-cmg-connection-point-tcp-tls-ports"></a><a name="bkmk_port-note1"></a> 注 1:CMG 接続ポイントの TCP-TLS ポート

CMG 接続ポイントは最初に、各 CMG VM インスタンスと長時間 TCP-TLS 接続を確立しようとします。 ポート 10140 の最初の VM インスタンスに接続します。 2 番目の VM インスタンスではポート 10141 が使用されます。最大で 16 番目のポートでポート 10155 が使用されます。 TCP-TLS 接続がパフォーマンスの面で最高ですが、インターネット プロキシをサポートしていません。 CMG 接続ポイントが TCP-TLS 経由で接続できない場合、HTTPS にフォールバックします <sup>[注 2](#bkmk_port-note2)</sup>。

#### <a name="note-2-cmg-connection-point-https-ports-for-one-vm"></a><a name="bkmk_port-note2"></a> 注 2:CMG 接続ポイントの 1 つの VM に対する HTTPS ポート

CMG 接続ポイントが TCP-TLS 経由で CMG に接続できない場合<sup>[注 1](#bkmk_port-note1)</sup>、1 つの VM インスタンスに対してのみ、HTTPS 443 経由で Azure ネットワーク ロード バランサーに接続します。  

#### <a name="note-3-cmg-connection-point-https-ports-for-two-or-more-vms"></a><a name="bkmk_port-note3"></a> 注 3:CMG 接続ポイントの複数の VM に対する HTTPS ポート

2 つ以上の VM インスタンスがある場合、CMG 接続ポイントでは、最初の VM インスタンスに接続するために、HTTPS 443 ではなく HTTPS 10124 が使用されます。 HTTPS 10125 で 2 番目の VM インスタンスに接続し、最大で 16 番目が HTTPS ポート 10139 で接続します。

### <a name="internet-access-requirements"></a>インターネット アクセス要件

組織がファイアウォールまたはプロキシ デバイスを使用してインターネットとのネットワーク通信を制限している場合は、CMG 接続ポイントとサービス接続ポイントからインターネット エンドポイントへのアクセスを許可する必要があります。

詳細については、「[Internet access requirements (インターネット アクセスの要件)](../../../plan-design/network/internet-endpoints.md#bkmk_cloud)」を参照してください。

## <a name="next-steps"></a>次のステップ

- [クラウド管理ゲートウェイの証明書](certificates-for-cloud-management-gateway.md)
- [クラウド管理ゲートウェイのセキュリティとプライバシー](security-and-privacy-for-cloud-management-gateway.md)
- [クラウド管理ゲートウェイのサイズとスケールの数値](../../../plan-design/configs/size-and-scale-numbers.md#bkmk_cmg)
- [クラウド管理ゲートウェイについてよくあるご質問](cloud-management-gateway-faq.md)
- [Set up cloud management gateway](setup-cloud-management-gateway.md) (クラウド管理ゲートウェイの設定)
