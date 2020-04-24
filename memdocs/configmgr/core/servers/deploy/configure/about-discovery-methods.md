---
title: 探索方法
titleSuffix: Configuration Manager
description: Active Directory または Azure Active Directory から、ご利用のネットワーク上のデバイスを検索するために使用できる探索方法について説明します。
ms.date: 03/22/2018
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: conceptual
ms.assetid: ed931751-18f2-4230-a09e-a0a329fbfa1c
author: mestew
ms.author: mstewart
manager: dougeby
ms.openlocfilehash: 68e622d45de6575ec0f652f33934654cd1481dc2
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81705000"
---
# <a name="about-discovery-methods-for-configuration-manager"></a>Configuration Manager の探索方法について

*適用対象:Configuration Manager (Current Branch)*

Configuration Manager の探索方法では、ネットワーク上のさまざまなデバイスと、Active Directory のデバイスおよびユーザー、または Azure Active Directory (Azure AD) のユーザーを検索できます。 探索方法を効率的に使用するには、使用可能な構成および制限について理解する必要があります。  



##  <a name="active-directory-forest-discovery"></a><a name="bkmk_aboutForest"></a> Active Directory フォレストの探索  
 **構成可能:** はい  

 **既定で有効:** いいえ  

 この方法の実行に使用できる**アカウント**は次のとおりです。  

-   **Active Directory フォレスト探索アカウント** (ユーザー定義)  

-   サイト サーバーの**コンピューター アカウント**  

Active Directory のその他探索方法とは異なり、Active Directory フォレスト探索は、管理できるリソースを検出しません。 代わりに、この方法では Active Directory で構成されているネットワークの場所が検出されます。 これらの場所を階層全体で使用する境界に変換できます。  

この方法が実行されると、ローカルの Active Directory フォレスト、信頼されている各フォレスト、および Configuration Manager コンソールの **[Active Directory フォレスト]** ノードで構成した追加の各フォレストを検索します。  

Active Directory フォレスト探索は、次の目的で使用します。  

-   Active Directory サイトおよびサブネットを探索し、そのネットワークの場所に基づいて、Configuration Manager の境界を作成する。  

-   Active Directory サイトに割り当てられているスーパーネットを特定します。 各スーパーネットを IP アドレス範囲の境界に変換します。  

-   フォレスト内の Active Directory Domain Services (AD DS) に発行します (そのフォレストへの発行が有効になっている場合)。 指定された Active Directory フォレスト アカウントには、そのフォレストへのアクセス許可が必要です。  

Configuration Manager コンソールで、Active Directory フォレストの探索を管理できます。 **[管理]** ワークスペースに移動し、 **[階層の構成]** を展開します。   

-   **探索方法**:階層の最上位で実行する Active Directory フォレストの探索を有効にします。 探索を実行する単純なスケジュールを指定することもできます。 検出された IP サブネットと Active Directory サイトから自動的に境界が作成されるように構成します。 Active Directory フォレストの探索を、子プライマリ サイトやセカンダリ サイトで実行することはできません。  

-   **Active Directory フォレスト**:探索する追加のフォレストの構成、各 Active Directory フォレスト アカウントの指定、各フォレストへの発行の構成を行えます。 検出プロセスを監視します。 IP サブネットと Active Directory サイトを Configuration Manager の境界と境界グループのメンバーとして追加します。  

階層の各サイトの Active Directory フォレストへの発行を構成するには、まず、Configuration Manager コンソールで、階層の最上位サイトに接続します。 Active Directory サイトの **[プロパティ]** ダイアログ ボックスの **[発行]** タブに、現在のサイトとその子サイトだけが表示されます。 フォレストへの発行が有効になっており、そのフォレストのスキーマを Configuration Manager 用に拡張している場合は、そのフォレストへの発行が有効になっている各サイトの次の情報が発行されます。  

-    **SMS-Site-&lt;サイト コード>**

-   **SMS-MP-&lt;サイト コード>-&lt;サイト システム サーバー名>**  

-   **SMS-SLP-&lt;サイト コード>-&lt;サイト システム サーバー名>**  

-   **SMS-&lt;サイト コード>-&lt;Active Directory サイト名またはサブネット>**  

> [!NOTE]  
>  セカンダリ サイトでは、Active Directory に発行するときに、必ず、セカンダリ サイト サーバーのコンピューター アカウントを使います。 セカンダリ サイトの情報を Active Directory に発行したい場合は、そのセカンダリ サイト サーバーのコンピューター アカウントに Active Directory への発行権限があることを確認してください。 信頼されていないフォレストにセカンダリ サイトからデータを発行することはできません。  

> [!CAUTION]  
>  Active Directory フォレストにサイトを発行するオプションのチェックボックスをオフにすると、そのサイトの使用可能なサイト システムの役割の情報を含め、すべての情報が Active Directory から削除されます。  

Active Directory フォレストの探索時に行われた処理は、次のログに記録されます。  

-   フォレストへの発行に関係のある処理を除くすべての処理: サイト サーバーの **&lt;InstallationPath>\Logs** フォルダーの **ADForestDisc.Log** ファイル  

-   フォレストへの発行に関係のある処理: サイト サーバーの **&lt;InstallationPath>\Logs** フォルダーの **hman.log** ファイルと **sitecomp.log** ファイル  

この探索方法を構成する方法の詳細については、[探索方法の構成](configure-discovery-methods.md#BKMK_ConfigADForestDisc)に関するトピックを参照してください。  



##  <a name="active-directory-group-discovery"></a><a name="bkmk_aboutGroup"></a> Active Directory グループの探索  
**構成可能:** はい  

**既定で有効:** いいえ  

この方法の実行に使用できる**アカウント**は次のとおりです。  

-   **Active Directory グループ探索アカウント** (ユーザー定義)  

-   サイト サーバーの**コンピューター アカウント**  

> [!TIP]  
>  このセクションの情報だけでなく、「[Active Directory グループ、システム、およびユーザー探索の一般的な機能](#bkmk_shared)」を参照してください。  

この方法は、Active Directory Domain Services を検索して以下のいずれかを特定する場合に使用します。  

-   ローカル、グローバル、およびユニバーサル セキュリティ グループ。  

-   グループのメンバーシップ。  

-   グループのメンバーのコンピューターやユーザーに関する限られた情報。この情報は、これらのコンピューターやユーザーが別の探索方法で検出されていない場合も返されます。  

この探索方法は、グループ、およびグループのメンバーの関係を識別するのが目的ですが、 既定では、セキュリティ グループだけが探索されます。 配布グループのメンバーシップも見つける場合は、 **[Active Directory Group Discovery Properties (Active Directory グループ探索のプロパティ)]** ダイアログ ボックスの **[オプション]** タブで、 **[配布グループのメンバーシップを探索する]** オプションのチェック ボックスをオンにする必要があります。  

Active Directory グループ探索は、Active Directory システム探索や Active Directory ユーザー探索で検出可能な Active Directory の拡張属性を返すことはできません。 この探索方法は、コンピューター リソースやユーザー リソース自体の探索用には最適化されていません。そのため、Active Directory システムの探索と Active Directory ユーザーの探索を実行した後で、この探索を実行するようにしてください。 これを提案する理由は、この探索方法ではグループについては完全な探索データ レコード (DDR) が作成されるものの、グループのメンバーであるコンピューターおよびユーザーについては限られた DDR しか作成されないためです。  

次の探索スコープを構成して、この方法で情報がどのように検索されるかを指定することができます。  

-   **場所**: 場所は、1 つまたは複数の Active Directory コンテナーを検索する場合に使用します。 このスコープ オプションは、指定した Active Directory コンテナーの再帰検索をサポートします。 このプロセスは、指定したコンテナー配下の各子コンテナーを検索します。 このプロセスは、それ以上子コンテナーが見つからなくなるまで繰り返されます。  

-   **[グループ]** :グループは、1 つまたは複数の特定の Active Directory グループを検索する場合に使用します。 **[Active Directory ドメイン]** オプションで、探索スコープを既定のドメインとフォレストに構成するか、個々のドメイン コントローラーに制限します。 さらに、検索するグループを 1 つまたは複数指定することができます。 グループを何も指定しなかった場合は、 **[Active Directory ドメイン]** で指定した場所にあるすべてのグループが検索されます。  

> [!CAUTION]  
>  探索スコープを構成するときは、探索する必要のあるグループだけを選択してください。 これを推奨するのは、Active Directory グループ探索では、検出のスコープ内の各グループの各メンバーを検出しようとするからです。 グループが大きい場合は、大量の帯域幅と Active Directory のリソースが消費される可能性があります。  

> [!NOTE]  
>  Active Directory の拡張属性に基づいてコレクションを作成し、これにより、コンピューターとユーザーの正確な探索結果を得るには、探索する内容に応じて、まず Active Directory システム探索または Active Directory ユーザー探索を実行します。  

Active Directory グループの探索時に行われた処理は、サイト サーバーの **&lt;InstallationPath\>\LOGS** フォルダーの **adsgdis.log** ファイルに記録されます。  

この探索方法を構成する方法の詳細については、[探索方法の構成](configure-discovery-methods.md#BKMK_ConfigADDiscGeneral)に関するトピックを参照してください。  



##  <a name="active-directory-system-discovery"></a><a name="bkmk_aboutSystem"></a> Active Directory システムの探索  
**構成可能:** はい  

**既定で有効:** いいえ  

この方法の実行に使用できる**アカウント**は次のとおりです。  

-   **Active Directory システム探索アカウント** (ユーザー定義)  

-   サイト サーバーの**コンピューター アカウント**  

> [!TIP]  
>  このセクションの情報だけでなく、「[Active Directory グループ、システム、およびユーザー探索の一般的な機能](#bkmk_shared)」を参照してください。  

この探索方法は、Active Directory Domain Services の指定された場所で、コレクションやクエリの作成に使用できるコンピューター リソースを見つけるために使います。 クライアント プッシュ インストールを使用して、検出されたデバイスに Configuration Manager クライアントをインストールすることもできます。  

既定では、この方法では、次の属性を含む、コンピューターの基本的な情報が探索されます。  

-   コンピューター名  

-   オペレーティング システムのバージョン  

-   Active Directory コンテナー名  

-   IP アドレス  

-   Active Directory サイト  

-   前回のログオンのタイムスタンプ  

コンピューターの DDR を正しく作成するためには、Active Directory システムの探索時に、コンピューター アカウントが識別され、そのコンピューター名が IP アドレスに解決されなければなりません。  

**[Active Directory System Discovery Properties] (Active Directory システム探索のプロパティ\)** ダイアログ ボックスの **[Active Directory の属性]** タブで、検出する既定のオブジェクト属性をすべて一覧表示することができます。 また、追加の (拡張) 属性を探索する方法を構成することもできます。  

Active Directory システムの探索時に行われた処理は、サイト サーバーの **&lt;InstallationPath\>\LOGS** フォルダーの **adsysdis.log** ファイルに記録されます。  

この探索方法を構成する方法の詳細については、[探索方法の構成](configure-discovery-methods.md#BKMK_ConfigADDiscGeneral)に関するトピックを参照してください。  



##  <a name="active-directory-user-discovery"></a><a name="bkmk_aboutUser"></a> Active Directory ユーザー探索  
**構成可能:** はい  

**既定で有効:** いいえ  

この方法の実行に使用できる**アカウント**は次のとおりです。  

-   **Active Directory ユーザー探索アカウント** (ユーザー定義)  

-   サイト サーバーの**コンピューター アカウント**  

> [!TIP]  
>  このセクションの情報だけでなく、「[Active Directory グループ、システム、およびユーザー探索の一般的な機能](#bkmk_shared)」を参照してください。  

この探索方法は、Active Directory Domain Services を検索して、ユーザー アカウントとその属性を見つけるために使います。 既定では、この方法では、次の属性を含む、ユーザー アカウントの基本的な情報が探索されます。  

-   ユーザー名  

-   固有なユーザー名 (ドメイン名を含む)  

-   ドメイン  

-   Active Directory コンテナー名  

**[Active Directory User Discovery Properties] (Active Directory ユーザー探索のプロパティ\)** ダイアログ ボックスの **[Active Directory の属性]** タブで、検出する既定のオブジェクト属性をすべて一覧表示することができます。 また、追加の (拡張) 属性を探索する方法を構成することもできます。

Active Directory ユーザーの探索時に行われた処理は、サイト サーバーの **&lt;InstallationPath\>\LOGS** フォルダーの **adusrdis.log** ファイルに記録されます。  

この探索方法を構成する方法の詳細については、[探索方法の構成](configure-discovery-methods.md#BKMK_ConfigADDiscGeneral)に関するトピックを参照してください。  



## <a name="azure-active-directory-user-discovery"></a><a name="azureaddisc"></a> Azure Active Directory ユーザー探索

Azure Active Directory (Azure AD) ユーザー探索を使用して、最新のクラウド ID を持つユーザーの Azure AD サブスクリプションを検索します。 Azure AD ユーザーの探索では、次の属性を検索することができます。

- objectId
- displayName
- メール
- mailNickname
- onPremisesSecurityIdentifier
- userPrincipalName
- AAD tenantID
- onPremisesDomainName
- onPremisesSamAccountName
- onPremisesDistinguishedName

この方法は、Azure AD のユーザー属性の完全同期と差分同期をサポートしています。 この情報は、他の検索方法から回収した検索データと併用できます。

Azure AD ユーザー探索のアクションは、階層の最上位層サイト サーバーの **SMS_AZUREAD_DISCOVERY_AGENT.log** ファイルに記録されます。

Azure AD ユーザー探索を構成するには、クラウド管理の [Azure サービスを構成する](azure-services-wizard.md)のページを参照してください。 この探索方法を構成する方法の詳細については、「[Configure Azure AD User Discovery](configure-discovery-methods.md#azureaadisc)」 (Azure AD ユーザー探索の構成) を参照してください。

## <a name="azure-active-directory-user-group-discovery"></a><a name="bkmk_azuregroupdisco"></a> Azure Active Directory ユーザー グループの探索
<!--3611956-->
*(バージョン 1906 で[プレリリース機能](../../manage/pre-release-features.md)として導入)*

Azure Active directory (Azure AD) からユーザー グループとそのグループのメンバーを検出できます。 Azure AD ユーザー グループの探索では、次の属性を見つけることができます。

- objectId
- displayName
- mailNickname
- onPremisesSecurityIdentifier
- AAD tenantID

Azure AD ユーザー グループ探索のアクションは、階層の最上位層サイト サーバーの **SMS_AZUREAD_DISCOVERY_AGENT.log** ファイルに記録されます。 この探索方法を構成する方法については、[Azure AD ユーザー グループの探索](configure-discovery-methods.md#bkmk_azuregroupdisco)に関するページを参照してください。

##  <a name="heartbeat-discovery"></a><a name="bkmk_aboutHeartbeat"></a> 定期探索  
**構成可能:** はい  

**既定で有効:** はい  

この方法の実行に使用できる**アカウント**は次のとおりです。  

-   サイト サーバーの**コンピューター アカウント**  

定期探索は、Configuration Manager の他の探索方法とは大きく異なります。 定期探索は、既定で有効になっており、DDR を作成するために各クライアント コンピューター (サイト サーバーではなく) で実行されます。 モバイル デバイス クライアントの場合は、この DDR は、そのモバイル デバイス クライアントが使用している管理ポイントによって作成されます。 これは、Configuration Manager クライアントのデータベースのレコードを維持するためです。定期探索を無効にしないでください。 この方法は、データベース レコードを維持するだけでなく、新しいリソース レコードとしてコンピューターを強制的に検出することができます。 また、データベースから削除されたコンピューターのデータベース レコードを再作成がすることもできます。  

定期探索は、階層内のすべてのクライアント用に構成されたスケジュールで実行されます。 既定では、定期探索は 7 日おきに実行されるように設定されています。 この実行間隔を変更する場合は、必ず、"**期限切れの探索データの削除**" というメンテナンス タスクの実行間隔より短くしてください。 このタスクは、非アクティブのクライアント レコードをサイト データベースから削除します。 **"期限切れの探索データの削除"** タスクは、プライマリ サイトだけで構成できます。 

特定のクライアントに対して定期探索を手動で呼び出すこともできます。 クライアントの Configuration Manager コントロール パネルの **[アクション]** タブで **[データ探索収集サイクル]** を実行します。  

定期探索を実行すると、クライアントの現在の情報を含む DDR が作成されます。 その後クライアントは、この小さなファイル (約 1 KB のサイズ) をプライマリ サイトで処理できるように、管理ポイントにコピーします。 このファイルには、次の情報が含まれています。  

-   ネットワークの場所  

-   NetBIOS 名  

-   クライアント エージェントのバージョン  

-   動作状態の詳細  

クライアントのインストール ステータスの詳細を返すことができる探索方法は、定期探索だけです。 そのためには、クライアントが更新されるように、"システム リソース" 属性を **[はい]** に等しい値に設定します。  

> [!NOTE]  
>  アクティブなモバイル デバイス クライアントの DDR は、定期探索を無効にしても、作成されて送信されます。 この動作により、アクティブなモバイル デバイスは、**期限切れの探索データの削除** タスクの影響を受けません。 **期限切れの探索データの削除**タスクは、モバイル デバイスのデータベース レコードを削除するときに、デバイス証明書も失効させます。 このアクションにより、モバイル デバイスが管理ポイントに接続するのがブロックされます。  

定期探索時に行われた処理は、次のログに記録されます。  

-   コンピューター クライアントの場合: クライアントの *%Windir%\CCM\Logs* フォルダーの **InventoryAgent.log** ファイル  

-   モバイル デバイス クライアントの場合: モバイル デバイス クライアントが使用している管理ポイントの *%Program Files%\CCM\Logs* フォルダーの **DMPRP.log** ファイル  

この探索方法を構成する方法の詳細については、[探索方法の構成](configure-discovery-methods.md#BKMK_ConfigHBDisc)に関するトピックを参照してください。  



##  <a name="network-discovery"></a><a name="bkmk_aboutNetwork"></a> ネットワーク探索  
**構成可能:** はい  

**既定で有効:** いいえ  

この方法の実行に使用できる**アカウント**は次のとおりです。  

-   サイト サーバーの**コンピューター アカウント**  

この方法は、ネットワーク トポロジを検出したり、IP アドレスを持つネットワーク上のデバイスを検出するために使います。 ネットワーク探索では、次のエンティティをクエリすることによって、ネットワークで IP 対応リソースを検索します。 
- Microsoft の実装の DHCP を実行しているサーバー
- ネットワーク ルーターでのアドレス解決プロトコル (ARP) キャッシュ
- SNMP 対応デバイス
- Active Directory ドメイン  

ネットワーク探索を使用するには、まず探索を実行する*レベル*を指定する必要があります。 また、ネットワークのセグメントやデバイスを照会する方法も構成します。 さらに、検索時にネットワークで行われる処理を制御する設定もあります。 最後に、ネットワーク探索を実行するスケジュールを設定します。  

ネットワーク探索でリソースを見つけるには、そのリソースの IP アドレスとサブネット マスクを識別しなければなりません。 次の方法でオブジェクトのサブネット マスクを識別します。  

-   **ルーターの ARP キャッシュ:** ルーターの ARP キャッシュを照会して、サブネット情報を見つけます。 通常、ルーターの ARP キャッシュには、ごく短い時間しかデータが入っていません。 そのため、ネットワーク探索で ARP キャッシュを照会したときに、必要なオブジェクトの情報がなくなっている可能性もあります。  

-   **DHCP:** 指定された DHCP サーバーを照会して、DHCP サーバーがアドレスをリースしたデバイスを見つけます。 ネットワーク探索では、Microsoft の実装の DHCP を実行している DHCP サーバーしか照会できません。  

-   **SNMP デバイス:** SNMP デバイスを直接照会します。 ネットワーク探索時にデバイスを照会するには、そのデバイスにローカル SNMP エージェントがインストールされている必要があります。 また、SNMP エージェントで使われているコミュニティ名を、ネットワーク探索で使用するように構成します。  

ネットワーク探索時に、IP アドレスを持つことができるオブジェクトが識別され、そのサブネット マスクが判別されると、そのオブジェクトの DDR が作成されます。 ネットワークには多種多様なデバイスが接続するため、ネットワーク探索では、Configuration Manager クライアントをサポートしていないリソースが検出されます。 たとえば、プリンターやルーターが、このようなデバイスに当てはまります。  

ネットワーク探索では、作成される探索レコードの一部としていくつかの属性を返す場合があります。 次の属性が含まれます。  

-   NetBIOS 名  

-   IP アドレス  

-   リソースのドメイン  

-   システムの役割  

-   SNMP コミュニティ名  

-   MAC アドレス  

ネットワーク探索時に行われた処理は、探索を実行したサイト サーバーの *&lt;InstallationPath\>\Logs* フォルダーの **Netdisc.log** ファイルに記録されます。  

 この探索方法を構成する方法の詳細については、[探索方法の構成](configure-discovery-methods.md#BKMK_ConfigNetworkDisc)に関するトピックを参照してください。  

> [!NOTE]  
>  複雑なネットワークや帯域幅の狭い接続では、ネットワーク探索の実行に時間がかかり、大量のネットワーク トラフィックが発生する可能性があります。 ベスト プラクティスとして、検出しなければならないデバイスが、他の方法で見つからない場合だけ、ネットワーク探索を実行してください。 たとえば、ワークグループのコンピューターを見つけなければならない場合に、ネットワーク探索を実行します。 他の探索方法では、ワークグループのコンピューターが検出されません。  

###  <a name="levels-of-network-discovery"></a><a name="BKMK_NetDiscLevels"></a> ネットワーク探索のレベル  
ネットワーク探索を構成するときは、次の 3 つのレベルの 1 つを指定します。  

|探索のレベル|詳細|  
|------------------------|-------------|  
|トポロジ|ルーターとサブネットを探索しますが、オブジェクトのサブネット マスクは識別しません。|  
|トポロジとクライアント|トポロジに加え、クライアントになり得るオブジェクト (コンピューターなど) とリソース (プリンターやルーターなど) を探索します。 このレベルではさらに、見つかったオブジェクトのサブネット マスクも特定します。|  
|トポロジ、クライアント、およびクライアントのオペレーティング システム|トポロジ、クライアントになり得るオブジェクトに加え、コンピューターのオペレーティング システムとバージョンを探索します。 Windows ブラウザーと Windows ネットワークの呼び出しを使用します。|  

 レベルが上がるに従って、探索時に行われる処理の量と、消費されるネットワーク帯域幅が増えます。 最も高いレベルに設定する前に、発生する可能性のあるネットワーク トラフィックを考慮してください。  

 たとえば、ネットワーク探索を初めて実行するときは、トポロジ レベルに設定して、ネットワーク インフラストラクチャを確認するだけで十分かもしれません。 次に、デバイスやオペレーティング システムを確認しなければならなくなったときに、高いレベルに構成し直します。 ネットワーク探索の対象を特定の範囲のネットワーク セグメントに制限する設定を構成することもできます。 このように、必要なネットワークの場所にあるオブジェクトを検出して、不要なネットワーク トラフィックを避けます。 このプロセスでは、エッジ ルータやネットワーク外のオブジェクトを検出することもできます。  

###  <a name="network-discovery-options"></a><a name="BKMK_NetDiscOptions"></a> ネットワーク探索のオプション  
ネットワーク探索で IP アドレス指定可能なデバイスの検索を有効にするには、これらのオプションを 1 つ以上構成します。  

> [!NOTE]  
>  ネットワーク探索は、探索を実行するサイト サーバーのコンピューター アカウントのコンテキストで実行されます。 コンピューター アカウントに、信頼されていないドメインに対するアクセス許可がないと、ドメインと DHCP サーバーのどちらのオプションを構成した場合も、リソースを見つけることはできません。  

#### <a name="dhcp"></a>DHCP  

ネットワーク探索時に照会する各 DHCP サーバーを指定します。 (ネットワーク探索では、Microsoft の実装の DHCP を実行している DHCP サーバーしか照会できません。)  

-   情報を取得するために、DHCP サーバーにあるデータベースへのリモート プロシージャ コールが使われます。  

-   32 ビットと 64 ビットの両方の DHCP サーバーを照会して、各サーバーに登録されているデバイスのリストを取得できます。  

-   ネットワーク探索時に DHCP サーバーを照会するには、探索を実行するサーバーのコンピューター アカウントが、DHCP サーバーの DHCP ユーザー グループのメンバーでなければなりません。 これと同じレベルのアクセス権は、次のいずれかの条件を満たした場合に存在することになります。  

    -   指定した DHCP サーバーが、探索を実行するサーバーの DHCP サーバーである。  

    -   探索を実行するコンピューターと DHCP サーバーが同じドメインにある。  

    -   探索を実行するコンピューターと DHCP サーバーの間に双方向の信頼関係が成立している。  

    -   サイト サーバーが DHCP ユーザー グループのメンバーである。  

-   ネットワーク探索で、DHCP サーバーが列挙されるときに、常に静的な IP アドレスが検出されるとは限りません。 DHCP サーバーで割り当てから除外するように設定にされている IP アドレスの範囲に当てはまる IP アドレスはネットワーク探索で見つかりません。 手動で割り当てるために予約されている IP アドレスもネットワーク探索で見つかりません。  

#### <a name="domains"></a>ドメイン  

ネットワーク探索時に検索するドメインを指定します。  

-   ネットワーク探索を実行するサイト サーバーのコンピューター アカウントは、指定した各ドメインにあるドメイン コントローラーの読み取り権限を持っている必要があります。  

-   ローカル ドメインからコンピューターを見つけるには、少なくとも 1 台のコンピューターで、コンピューター ブラウザー サービスを有効にする必要があります。 このコンピューターは、ネットワーク探索を実行するサイト サーバーと同じサブネット上にある必要があります。  

-   ネットワークを閲覧するときにサイト サーバーから見ることができるコンピューターが検出されます。  

-   ネットワーク探索では、IP アドレスが取得されます。 そして、インターネット制御メッセージ プロトコル (ICMP) のエコー要求を使って、各デバイスが ping されます。 この **ping** コマンドを実行することにより、どのコンピューターが現在アクティブかがわかります。  

#### <a name="snmp-devices"></a>SNMP デバイス  

ネットワーク探索時に照会する各 SNMP デバイスを指定します。  

-   クエリに応答した SNMP デバイスの ipNetToMediaTable 値が取得されます。 この値は、コンピューターまたは他のリソース (プリンターやルーター、IP アドレスを持つことができる他のデバイス) の IP アドレスの配列です。  

-   デバイスを照会するには、そのデバイスの IP アドレスか NetBIOS 名を指定する必要があります。  

-   ネットワーク探索でデバイスのコミュニティ名を使用するように構成してください。このように構成しないと、SNMP クエリがデバイスによって拒否されます。  


###  <a name="limiting-network-discovery"></a><a name="BKMK_LimitNetDisc"></a> ネットワーク探索を制限する  
ネットワーク探索時に、ネットワークの端にある SNMP デバイスを照会する場合は、ネットワークのすぐ外にあるサブネットと SNMP デバイスの情報を識別することができます。 次の情報を使用してネットワーク探索の範囲を制限するには、ネットワーク探索時に通信する SNMP デバイスと、照会するネットワーク セグメントを構成します。  

#### <a name="subnets"></a>サブネット  

ネットワーク探索で [SNMP デバイス] オプションか [DHCP] オプションを使用するときに、照会するサブネットを構成します。 この 2 つのオプションで検索されるのは、有効にしたサブネットだけです。  

たとえば、DHCP サーバーの照会によって、ネットワーク全体のデバイスの情報が返されますが、 特定のサブネットにあるデバイスだけを見つけたい場合は、 **[ネットワーク探索のプロパティ]** ダイアログ ボックスの **[サブネット]** タブで、その特定のサブネットを指定して有効にします。 このように設定すると、今後実行される、DHCP サーバーを使用した探索と SNMP 検出タスクの範囲が、指定したサブネットに制限されます。  

> [!NOTE]  
>  ただし、このサブネットの構成は、 **[ドメイン]** オプションを設定した探索で検索されるオブジェクトを制限しません。  

#### <a name="snmp-community-names"></a>［SNMP コミュニティ名］  

ネットワーク探索で SNMP デバイスを照会するには、ネットワーク探索を構成するときに、そのデバイスのコミュニティ名を指定する必要があります。 SNMP デバイスのコミュニティ名を使用しないと、デバイスによってクエリが拒否されます。  

#### <a name="maximum-hops"></a>最大ホップ数  

ネットワーク探索で SNMP を使って照会できるネットワーク セグメントの数とルーターの数を制限します。  

ここで指定するホップの数によって、ネットワーク探索時に照会できる追加のデバイスの数とネットワーク セグメントのが決まります。  

たとえば、探索のレベルをトポロジに設定し、[最大ホップ数] を **0** に設定した場合は、照会元のサーバーが存在するサブネットが探索されます。 そのサブネットにルーターがあれば、それが含まれます。  

次の図は、探索のレベルをトポロジのみに設定し、[最大ホップ数] を 0 に指定したネットワーク探索クエリをサーバー 1 で実行する例を示しています。この場合、サブネット D とルーター 1 が見つかります。  

 ![ルーター ジャンプのない探索のイメージ](media/Disc-0.gif)  

 次の図は、探索のレベルを [トポロジとクライアント] に設定し、[最大ホップ数] を 0 に指定したネットワーク探索クエリをサーバー 1 で実行する例を示しています。この場合、サブネット D とルーター 1、およびサブネット D にある、クライアントになり得るデバイスすべてが見つかります。  

 ![1 つのルーター ジャンプを含む探索のイメージ](media/Disc-1.gif)  

 ルーターのホップ数を大きくすると、検出されるネットワーク リソースの数が増える可能性があることを、次のネットワークを例にして説明します。  

 ![2 つのルーター ジャンプを含む探索のイメージ](media/Disc-2.gif)  

 探索のレベルをトポロジに、[最大ホップ数] を 1 に設定したネットワーク探索をサーバー 1 で実行すると、次のエンティティが検出されます。  

-   ルーター 1 とサブネット 10.1.10.0 (最大ホップ数を 0 にした場合も検出されます)  

-   サブネット 10.1.20.0 と 10.1.30.0、サブネット A、ルーター 2 (1 番目のホップにあります)  

> [!WARNING]  
>  ルーターのホップ数を上げるに従って、検出可能なリソースの数が大幅に増える可能性があります。ただし、消費されるネットワーク帯域幅も大きくなります。  



##  <a name="server-discovery"></a><a name="bkmk_aboutServer"></a> サーバー探索  
**構成可能:** いいえ  

Configuration Manager では、ユーザーが構成できる探索方法だけでなく、**サーバー探索**というプロセス (SMS_WINNT_SERVER_DISCOVERY_AGENT) も使います。 このプロセスでは、サイト システムのコンピューター (管理ポイントとして構成されているコンピューターなど) のリソースのレコードが作成されます。  



##  <a name="common-features-of-active-directory-group-discovery-system-discovery-and-user-discovery"></a><a name="bkmk_shared"></a> Active Directory グループの探索、Active Directory システムの探索、Active Directory ユーザーの探索に共通の機能  
ここでは、次の探索方法に共通する機能ついて説明します。  

-   Active Directory グループの探索  

-   Active Directory システムの探索  

-   Active Directory ユーザー検出  

> [!NOTE]  
>  このセクションの情報は、Active Directory フォレストの探索には当てはまりません。  

この 3 つの探索方法には、同様の構成と操作が使用されます。 それぞれ Active Directory Domain Services に格納されたコンピューター、ユーザー、リソースのグループ メンバーシップに関する情報を探索します。 探索プロセスは、探索エージェントによって管理されます。 このエージェントは、検出を実行するように構成されている各サイトのサイト サーバーで実行されます。 どの方法でも、Active Directory の検索する 1 つまたは複数の場所をローカルまたはリモート フォレスト内の場所インスタンスとして構成することができます。  

信頼されていないフォレストでリソースの検索する場合は、次のことが必要です。  

-   Active Directory システムの探索でコンピューター リソースを探索する場合は、探索エージェントが、そのリソースの FQDN を解決できなければなりません。 FQDN を解決できない場合は、NetBIOS 名での解決を試みます。  

-   Active Directory ユーザー探索でユーザーを、または Active Directory グループ探索でグループを探索する場合は、探索エージェントが、Active Directory の場所として指定されたドメイン コントローラーの FQDN を解決できなければなりません。  

指定する場所ごとに、検索オプションを構成することができます。たとえば、Active Directory の子コンテナーの場所を繰り返し検索するオプションがあります。 また、この場所を検索するときに使用する固有なアカウントを構成することもできます。 このアカウントは、複数のフォレスト内の複数の Active Directory の場所を探索する方法を 1 つのサイトで構成する柔軟性を提供します。 すべての場所のアクセス許可を持っている単一のアカウントを構成する必要はありません。  

この 3 つの探索方法のいずれかを特定のサイトで実行すると、そのサイトの Configuration Manager サイト サーバーが、指定された Active Directory フォレスト内の最も近いドメイン コントローラーと通信して、Active Directory のリソースを見つけます。 ドメインとフォレストは、Active Directory のサポートされているどのモードでもかまいません。 それぞれの場所インスタンスに割り当てるアカウントには、指定した Active Directory の場所の**読み取り**アクセス許可が必要です。

まず、指定された場所でオブジェクトが検索され、そのオブジェクトに関する情報が収集されます。 リソースに関する十分な情報が収集されると、DDR が作成されます。 必要な情報は、使用している探索方法によって異なります。  

異なる Configuration Manager サイトでローカルの Active Directory サーバーを照会する同じ探索方法を構成する場合は、サイトごとに別々の探索オプションを構成することができます。 探索データは、階層にあるサイトで共有されるので、構成が重なり合うのを避け、各リソースが一度だけ検出されるようにしてください。

環境の規模が小さい場合は、それぞれの探索方法を階層内の 1 つのサイトだけで実行することを検討してください。 この構成により、管理上のオーバーヘッドを軽減し、同じリソースが何度も検出される可能性を減らすことができます。 探索を実行するサイトの数を最小限にすると、検索で使用されるネットワーク全体の帯域幅が減ります。 作成後、サイト サーバーで処理する必要がある DDR の全体的数も減らすことができます。  

探索方法の構成の多くは、一目見ただけで、その意味がわかります。 次のセクションでは、探索オプションを実際に構成する前に知っておく必要のある情報を説明します。  

次の表に、Active Directory の複数の探索方法で使用できるオプションを示します。  

-   [差分探索](#bkmk_delta)  

-   [古いコンピューター レコードをドメインへのログオン日時に基づいて除外する](#bkmk_stalelogon)  

-   [古いレコードをコンピューターのパスワードに基づいて除外する](#bkmk_stalepassword)  

-   [Active Directory のカスタマイズされた属性を検索する](#bkmk_customAD)  


###  <a name="delta-discovery"></a><a name="bkmk_delta"></a> 差分探索  
次の探索方法に使用できます。  

-   Active Directory グループの探索  

-   Active Directory システムの探索  

-   Active Directory ユーザー検出  

差分探索は独立した探索方法ではなく、該当する探索方法で選択できるオプションです。 差分探索では、該当する探索方法で完全な探索が最後に実行されてから行われた変更について、特定の Active Directory 属性を検索します。 属性の変更が Configuration Manager データベースに送信されて、リソースの探索レコードが更新されます。  

既定では、差分探索は 5 分間隔で実行されます。 このスケジュールは、完全な探索の通常のスケジュールよりもかなり高い頻度です。 差分探索では、使用するサイト サーバーおよびネットワーク リソースが完全な探索よりも少ないので、このような頻繁な実行が可能になります。 探索方法で差分探索を使用する場合は、その完全な探索を実行する頻度を下げることができます。  

次に、差分探索を使ってよく検出する変更点をいくつか示します。  

-   Active Directory に追加された新しいコンピューターとユーザー  

-   コンピューターとユーザーの基本的な情報の変更  

-   グループに追加された新しいコンピューターとユーザー  

-   グループから削除されたコンピューターとユーザー  

-   システム グループ オブジェクトに加えられた変更  

差分探索で、新しいリソースとグループのメンバーシップの変更を検出できますが、Active Directory からどの時点でリソースが削除されたかを検出することはできません。 差分探索で生成される DDR は、完全な探索で生成される DDR と同様に処理されます。  

差分検索を構成するには、該当する探索方法のプロパティの **[ポーリングのスケジュール]** タブを使います。  


###  <a name="filter-stale-computer-records-by-domain-logon"></a><a name="bkmk_stalelogon"></a> 古いコンピューター レコードをドメインへのログオン日時に基づいて除外する  
次の探索方法に使用できます。  

-   Active Directory グループの探索  

-   Active Directory システムの探索  

古いコンピューター レコードを持つコンピューターを探索から除外できます。 この除外は、コンピューターでのドメインへの最後のログオン日時に基づきます。 Active Directory システムの探索でこのオプションを有効にすると、見つかった各コンピューターが評価されます。 Active Directory グループの探索でこのオプションを有効にすると、見つかったグループに属する各コンピューターが評価されます。  

このオプションを使用するには、次のことが必要です。  

-   コンピューターが Active Directory Domain Services の **lastLogonTimeStamp** 属性を更新するように構成されている。  

-   Active Directory ドメインの機能レベルが Windows Server 2003 以降に設定されている。  

この設定に使用する、探索の基準にする最後のログイン日時を構成するときは、ドメイン コントローラー同士のレプリケーション間隔を考慮してください。  

フィルタリングは、 **[Active Directory System Discovery Properties (Active Directory システム探索のプロパティ)]** ダイアログ ボックスと **[Active Directory Group Discovery Properties (Active Directory グループの探索のプロパティ)]** ダイアログ ボックスの **[オプション]** タブで構成します。 **[指定した期間内にドメインにログオンしたコンピューターのみ探索する]** を選択します。  

> [!WARNING]  
>  このフィルターと、 **[古いレコードをコンピューターのパスワードに基づいて除外する]** フィルターを有効にした場合は、どちらかのフィルターの基準を満たしたコンピューターが探索から除外されます。  


###  <a name="filter-stale-records-by-computer-password"></a><a name="bkmk_stalepassword"></a> 古いレコードをコンピューターのパスワードに基づいて除外する  
次の探索方法に使用できます。  

-   Active Directory グループの探索  

-   Active Directory システムの探索  

古いコンピューター レコードを持つコンピューターを探索から除外できます。 この除外は、コンピューターで最後に更新されたコンピューター アカウントのパスワードに基づいています。 Active Directory システムの探索でこのオプションを有効にすると、見つかった各コンピューターが評価されます。 Active Directory グループの探索でこのオプションを有効にすると、見つかったグループに属する各コンピューターが評価されます。  

このオプションを使用するには、次のことが必要です。  

-   コンピューターが Active Directory Domain Services の **pwdLastSet** 属性を更新するように構成されている。  

このオプションを構成する際は、この属性の更新の間隔を検討してください。 また、ドメイン コントローラー間のレプリケーション間隔も検討してください。  

フィルタリングは、 **[Active Directory System Discovery Properties (Active Directory システム探索のプロパティ)]** ダイアログ ボックスと **[Active Directory Group Discovery Properties (Active Directory グループの探索のプロパティ)]** ダイアログ ボックスの **[オプション]** タブで構成します。 **[指定した期間内にアカウントのパスワードが更新されたコンピューターのみ探索する]** を選択します。  

> [!WARNING]  
>  このフィルターと、 **[古いレコードをドメインへのログイン日時に基づいて除外する]** フィルターを有効にした場合は、どちらかのフィルターの基準を満たしたコンピューターが探索から除外されます。  


###  <a name="search-customized-active-directory-attributes"></a><a name="bkmk_customAD"></a> Active Directory のカスタマイズされた属性を検索する  
 次の探索方法に使用できます。  

-   Active Directory システムの探索  

-   Active Directory ユーザー検出  

探索方法ごとに、検出可能な Active Directory 属性の一覧が用意されています。  

カスタマイズされた属性の一覧を表示および構成するには、 **[Active Directory System Discovery Properties (Active Directory システム探索のプロパティ)]** ダイアログ ボックス、または **[Active Directory User Discovery Properties (Active Directory ユーザー探索のプロパティ)]** ダイアログボックスの **[Active Directory の属性]** タブを使います。  