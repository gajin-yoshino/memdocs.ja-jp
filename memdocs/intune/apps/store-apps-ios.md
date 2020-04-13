---
title: iOS ストア アプリを Microsoft Intune に追加する
titleSuffix: ''
description: iOS ストア アプリを Microsoft Intune に追加する方法について説明します。 アプリ ストアでアプリが無料になっている場合は、この方法を使用してそのアプリを割り当てることができます。
keywords: Intune
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 01/22/2020
ms.topic: conceptual
ms.service: microsoft-intune
ms.subservice: apps
ms.localizationpriority: high
ms.technology: ''
ms.assetid: c59514d7-1256-4576-9380-e7a0b85a0378
ms.reviewer: mghadial
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c3dc9132bd21f04184107907c7c81dc90d2d9ca
ms.sourcegitcommit: e2567b5beaf6c5bf45a2d493b8ac05d996774cac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/27/2020
ms.locfileid: "80325882"
---
# <a name="add-ios-store-apps-to-microsoft-intune"></a>iOS ストア アプリを Microsoft Intune に追加する

[!INCLUDE [azure_portal](../includes/azure_portal.md)]

この記事の情報を使って、Microsoft Intune に iOS ストア アプリを追加できます。 iOS ストア アプリとは、Intune がユーザー デバイスにインストールするアプリです。 このユーザーとは、あなたの会社の従業員の一員です。 iOS ストア アプリは自動的に更新されます。

>[!NOTE]
>iOS/iPadOS デバイスのユーザーは Stocks や Maps などの一部の iOS/iPadOS 組み込みアプリを削除できますが、Intune を使ってこれらのアプリを再展開することはできません。 ユーザーがこれらのアプリを削除した場合は、アプリ ストアから手動で再インストールする必要があります。

## <a name="before-you-start"></a>開始する前に

アプリ ストアで無料になっている場合にのみ、この方法でアプリを割り当てることができます。 Intune を使用して有料のアプリを割り当てる場合、[iOS/iPadOS ボリューム購入プログラム](vpp-apps-ios.md)の利用を検討してください。

>[!NOTE]
>Microsoft Intune を使うときは、Microsoft Edge または Google Chrome ブラウザーを使うことをお勧めします。

1. [Microsoft Endpoint Manager 管理センター](https://go.microsoft.com/fwlink/?linkid=2109431)にサインインします。
2. **[アプリ]**  >  **[すべてのアプリ]**  >  **[追加]** の順に選択します。
3. **[アプリの種類の選択]** ペインの使用できる **[ストア アプリ]** の種類で、 **[iOS ストア アプリ]** を選択します。
4. **[選択]** をクリックします。<br>
   **[アプリの追加]** 手順が表示されます。
5. **[アプリ ストアを検索します]** を選びます。
6. **[アプリ ストアを検索します]** ウィンドウで、App Store の国/地域のロケールを選びます。
7. **[検索]** ボックスに、アプリの名前 (または名前の一部) を入力します。  
    Intune がストアを検索し、関連する結果の一覧を返します。
8. 結果の一覧で目的のアプリを選び、 **[選択]** を選びます。<br>

   **[アプリ情報]** ページが **[アプリの追加]** ペインに表示されます。 可能であれば、ストアから選択したアプリに基づいてアプリ情報が追加されます。

9. **[アプリ情報 ]** ページで、アプリの詳細を追加します。 選択したアプリによっては、このウィンドウ内の一部の値が自動的に入力されている場合があります。
    - **名前**:アプリの名前を入力します。この名前は会社のポータルに表示されます。 使用するアプリ名が一意であることを確認してください。 アプリ名が重複している場合、会社のポータルでは 1 つの名前のみがユーザーに表示されます。
    - **説明**:アプリの説明を入力します。 この説明は会社のポータルでユーザーに表示されます。
    - **[発行元]** : アプリの発行元の名前を入力します。
    - **[アプリ ストアの URL]** : 作成するアプリのアプリ ストア URL を入力します。
    - **[最低限のオペレーティング システム]** : アプリをインストールできる最も初期のバージョンのオペレーティング システムを一覧から選択します。 これよりも前のオペレーティング システムがアプリの割り当て先デバイスにインストールされている場合、そのアプリはインストールされません。
    - **[適用可能なデバイスの種類]** : 一覧から、アプリで使用されているデバイスを選択します。
    - **[カテゴリ]** : (省略可能) 1 つ以上の組み込みアプリ カテゴリ、または作成したカテゴリを選択します。 そうすると、会社のポータルを閲覧するときに、ユーザーがアプリを探しやすくなります。
    - **[会社のポータルでおすすめアプリとして表示します]** : ユーザーがアプリを参照するとき、会社のポータルのメイン ページにアプリ スイートが目立つように表示するには、このオプションを選びます。
    - **[情報 URL]** : このアプリに関する情報が含まれる Web サイトの URL を入力することもできます。 この URL は会社のポータルでユーザーに表示されます。
    - **[プライバシー URL]** : このアプリのプライバシー情報が含まれる Web サイトの URL を入力することもできます。 この URL は会社のポータルでユーザーに表示されます。
    - **[開発者]** : (省略可能) アプリ開発者の名前を入力します。 このフィールドは管理者のみに表示され、ユーザーには表示されません。
    - **[所有者]** : (省略可能) このアプリの所有者の名前 ("*人事部*" など) を入力します。 このフィールドは管理者のみに表示され、ユーザーには表示されません。
    - **[メモ]** : (省略可能) このアプリに関連付けるメモを入力します。 このフィールドは管理者にのみ表示され、エンド ユーザーには表示されません。
    - **[ロゴ]** : (省略可能) アプリに関連付けるアイコンをアップロードします。 ユーザーが会社のポータルを参照するとき、アプリにこのアイコンが表示されます。
10. **[次 へ]** をクリックして **[スコープ タグ]** ページを表示します。
11. **[スコープ タグを選択]** をクリックして、必要に応じてアプリのスコープ タグを追加します。 詳細については、「[分散 IT にロールベースのアクセス制御 (RBAC) とスコープのタグを使用する](../fundamentals/scope-tags.md)」を参照してください。
12. **[次へ]** をクリックして、 **[割り当て]** ページを表示します。
13. アプリのグループ割り当てを選択します。 詳細については、「[ユーザーとデバイスを整理するためのグループを追加する](../fundamentals/groups-add.md)」を参照してください。 
14. **[次へ]** をクリックして、 **[確認と作成]** ページを表示します。 アプリに入力した値と設定を確認します。
15. 完了したら、 **[作成]** をクリックしてアプリを Intune に追加します。

作成したアプリの **[概要]** ブレードが表示されます。

## <a name="next-steps"></a>次のステップ

- [アプリをグループに割り当てる](apps-deploy.md)