---
title: Intune を使用して iOS および Android 用の Teams を管理する
titleSuffix: ''
description: iOS および Android 用の Teams で Intune アプリ保護および構成ポリシーを使用して、コラボレーション エクスペリエンスが常に保護機能付きでアクセスされるように確保することができます。
keywords: ''
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 07/13/2020
ms.topic: conceptual
ms.service: microsoft-intune
ms.subservice: apps
ms.localizationpriority: high
ms.technology: ''
ms.reviewer: smithre4
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ebc719d65024f26d1661d311bfbf9077bcdcbe3
ms.sourcegitcommit: 86c2c438fd2d87f775f23a7302794565f6800cdb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/16/2020
ms.locfileid: "86410914"
---
# <a name="manage-team-collaboration-access-by-using-teams-for-ios-and-android-with-microsoft-intune"></a>Microsoft Intune で iOS および Android 用の Teams を使用してチーム コラボレーションを管理する

Microsoft Teams は、Microsoft 365 において、チームがより積極的で効率的になるために必要な人材、コンテンツ、ツールが統合されたチーム コラボレーションのためのハブです。

Office 365 データの豊富で広範な保護機能は、Enterprise Mobility + Security スイートにサブスクライブすると利用できます。これには、条件付きアクセスなどの Microsoft Intune と Azure Active Directory Premium の機能が含まれます。 少なくとも、モバイル デバイスから iOS および Android 用の Teams への接続を許可する条件付きアクセス ポリシーと、コラボレーション エクスペリエンスが保護されることを保証する Intune アプリ保護ポリシーを展開することをお勧めします。

## <a name="apply-conditional-access"></a>条件付きアクセスを適用する
組織は Azure AD 条件付きアクセス ポリシーを使用して、ユーザーが iOS および Android 用の Teams を使用して確実に職場または学校のコンテンツにのみアクセスできるようにすることができます。 これを行うには、可能性のあるすべてのユーザーを対象とする条件付きアクセス ポリシーが必要です。 このポリシーの作成の詳細については、[条件付きアクセスを使用してクラウド アプリへのアクセスにアプリ保護ポリシーを要求する方法](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access)に関するページを参照してください。

1. 「[シナリオ 1: Office 365 アプリで、承認済みアプリとアプリ保護ポリシーの使用を必須にする](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access#scenario-1-office-365-apps-require-approved-apps-with-app-protection-policies)」 の「ステップ 1: Office 365 用の Azure AD 条件付きアクセス ポリシーを構成する」に従い、iOS および Android 用の Teams から Office 365 エンドポイントに接続することを許可し、サードパーティの OAuth 対応モバイル デバイス クライアントをブロックします。

   >[!NOTE]
   > このポリシーにより、モバイル ユーザーは適用可能なアプリを使用してすべての Office エンドポイントに確実にアクセスできます。

## <a name="create-intune-app-protection-policies"></a>Intune のアプリ保護ポリシーを作成する

アプリ保護ポリシー (APP) では、組織データの使用を許可されるアプリとそのデータに対して実行できる操作を定義します。 APP で利用できる選択肢を使用することで、組織は固有のニーズに合わせて保護を調整できます。 場合によっては、完全なシナリオを実装するためにどのポリシー設定が必要であるかが明確ではないことがあります。 組織がモバイル クライアント エンドポイントのセキュリティ強化を優先できるよう、Microsoft では、iOS および Android モバイル アプリ管理のための APP データ保護フレームワークの分類を導入しました。

APP データ保護フレームワークは 3 つの異なる構成レベルに編成されており、各レベルは前のレベルを基に構築されています。

- **エンタープライズ基本データ保護** (レベル 1) では、アプリが PIN で保護され、暗号化されており、選択的ワイプ操作を実行できるようにします。 Android デバイスの場合、このレベルでは Android デバイスの構成証明を検証します。 これは、Exchange Online メールボックス ポリシーに類似したデータ保護制御を提供し、IT 部門およびユーザー集団に APP を経験させる、エントリ レベルの構成です。
- **エンタープライズ拡張データ保護** (レベル 2) では、APP データ漏えい防止メカニズムと OS の最小要件が導入されています。 この構成は、職場または学校のデータにアクセスするほとんどのモバイル ユーザーに適用されます。
- **エンタープライズ高度データ保護** (レベル 3) では、高度なデータ保護メカニズム、強化された PIN の構成、および APP Mobile Threat Defense が導入されています。 この構成は、危険度の高いデータにアクセスするユーザーに適しています。

各構成レベルおよび、最低限保護する必要のあるアプリに関する具体的な推奨事項については、「[アプリ保護ポリシーを使用するデータ保護フレームワーク](app-protection-framework.md)」を参照してください。

デバイスが統合エンドポイント管理 (UEM) ソリューションに登録されているかどうかに関係なく、「[アプリ保護ポリシーを作成して割り当てる方法](app-protection-policies.md)」の手順を使用して、iOS アプリと Android アプリの両方に対して Intune アプリ保護ポリシーを作成する必要があります。 これらのポリシーは、少なくとも次の条件を満たしている必要があります。

1. Microsoft Edge、Outlook、OneDrive、Office、Teams など、すべての Microsoft 365 モバイル アプリケーションを含める。これにより、ユーザーは Microsoft アプリ内の職場または学校のデータに対し、セキュリティで保護された方法でアクセスして操作できることが保証されます。

2. すべてのユーザーに割り当てられている。 これにより、iOS および Android 用の Teams のどちらを使用しているかに関係なく、すべてのユーザーが確実に保護されます。

3. 要件を満たすフレームワーク レベルを決定する。 ほとんどの組織では、データ保護とアクセス要件の制御を可能にするために、**エンタープライズ拡張データ保護** (レベル 2) に定義されている設定を実装する必要があります。

利用可能な設定について詳しくは、「[Android アプリ保護ポリシー設定](app-protection-policy-settings-android.md)」と「[iOS アプリ保護ポリシー設定](app-protection-policy-settings-ios.md)」をご覧ください。

> [!IMPORTANT]
> Intune に登録されていない Android デバイス上のアプリに対して Intune アプリ保護ポリシーを適用するには、ユーザーが Intune ポータル サイトもインストールする必要があります。 詳しくは、「[アプリ保護ポリシーを使用して Android アプリを管理するときの注意点](../fundamentals/end-user-mam-apps-android.md)」を参照してください。

## <a name="utilize-app-configuration"></a>アプリ構成を利用する

iOS および Android 用の Teams でサポートされるアプリ設定を使用して、Microsoft エンドポイント マネージャーなどの統合エンドポイント管理の管理者がアプリの動作をカスタマイズできます。

アプリ構成を配信するには、登録済みデバイスのモバイル デバイス管理 (MDM) OS チャネル (iOS 用の[マネージド アプリ構成](https://developer.apple.com/library/content/samplecode/sc2279/Introduction/Intro.html)チャネルまたは Android 用の [Android in the Enterprise](https://developer.android.com/work/managed-configurations) チャネル) を使用するか、または Intune アプリ保護ポリシー (APP) チャネルを使用します。 iOS および Android 用の Teams により、次の構成シナリオがサポートされています。

- 職場または学校アカウントのみ許可

> [!IMPORTANT]
> Android でデバイスを登録する必要がある構成シナリオでは、デバイスを Android Enterprise に登録し、Android 用の Teams をマネージド Google Play ストアを使用して展開する必要があります。 詳しくは、「[Android Enterprise 仕事用プロファイル デバイスの登録を設定する](../enrollment/android-work-profile-enroll.md)」および「[マネージド Android Enterprise デバイス用にアプリ構成ポリシーを追加する](app-configuration-policies-use-android.md)」を参照してください。

各構成シナリオでは、固有の要件に焦点を当てています。 たとえば、構成シナリオにデバイスの登録が必要であるために UEM プロバイダーで動作するか、Intune アプリ保護ポリシーが必要かどうかを示します。

> [!NOTE]
> Microsoft Endpoint Manager を使用すると、MDM OS チャネルを介して配信されるアプリ構成は、**マネージド デバイス** アプリ構成ポリシー (ACP) と呼ばれます。アプリ保護ポリシー チャネルを介して配信されるアプリ構成は、**マネージド アプリ**のアプリ構成ポリシーと呼ばれます。

## <a name="only-allow-work-or-school-accounts"></a>職場または学校アカウントのみ許可

最大規模で規制の厳しいお客様のデータのセキュリティとコンプライアンス ポリシーを尊重することは、Microsoft 365 の価値の重要な柱です。 企業によっては、企業環境内のすべての通信情報をキャプチャし、デバイスが企業の通信にのみ使用されるようにする必要があります。 これらの要件をサポートするために、アプリ内で 1 つの企業アカウントのみをプロビジョニングできるように登録済みデバイスで iOS および Android 用の Teams を構成することができます。

組織の許可されたアカウント モード設定の構成については、以下を参照してください。

- [Android の設定](app-configuration-policies-use-android.md#allow-only-configured-organization-accounts-in-multi-identity-apps)
- [iOS の設定](app-configuration-policies-use-ios.md#allow-only-configured-organization-accounts-in-multi-identity-apps)

この構成シナリオは、登録済みデバイスでのみ機能します。 ただし、UEM プロバイダーはすべてサポートされています。 Microsoft Endpoint Manager を使用していない場合は、これらの構成キーの展開方法について UEM のドキュメントを参照する必要があります。

## <a name="next-steps"></a>次のステップ

- [アプリ保護ポリシーとは?](app-protection-policy.md) 
- [Microsoft Intune 用アプリ構成ポリシー](app-configuration-policies-overview.md)