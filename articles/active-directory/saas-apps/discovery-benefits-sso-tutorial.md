---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Discovery Benefits SSO 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Discovery Benefits SSO 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a788cd07-0eed-4067-b79d-19b840e8836d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64c4a6811ef5d7ed4f29c7dae89561616895a42a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "72271938"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-discovery-benefits-sso"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Discovery Benefits SSO 整合

在本教學課程中，您會了解如何整合 Discovery Benefits SSO 與 Azure Active Directory (Azure AD)。 在整合 Discovery Benefits SSO 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Discovery Benefits SSO 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Discovery Benefits SSO。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Discovery Benefits SSO 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Discovery Benefits SSO 支援 **IDP** 起始的 SSO

> [!NOTE]
> 此應用程式的識別碼是固定的字串值，因此一個租用戶中只能設定一個執行個體。

## <a name="adding-discovery-benefits-sso-from-the-gallery"></a>從資源庫新增 Discovery Benefits SSO

若要設定將 Discovery Benefits SSO 整合到 Azure AD 中，您需要從資源庫將 Discovery Benefits SSO 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **Discovery Benefits SSO**。
1. 從結果面板選取 [Discovery Benefits SSO]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-discovery-benefits-sso"></a>設定及測試 Discovery Benefits SSO 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Discovery Benefits SSO 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Discovery Benefits SSO 中相關使用者之間的連結關聯性。

若要設定及測試與 Discovery Benefits SSO 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Discovery Benefits SSO 的 SSO](#configure-discovery-benefits-sso-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 Discovery Benefits SSO 測試使用者](#create-discovery-benefits-sso-test-user)** - 使 Discovery Benefits SSO 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Discovery Benefits SSO]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [ **基本 SAML 組態**]  區段上，已預先以  **IDP**  起始的模式設定好應用程式，並已經為 Azure 預先填入必要的 URL。 使用者必須按一下 [ **儲存**]  按鈕，才能儲存組態。

1. Discovery Benefits SSO 應用程式會預期要有特定格式的 SAML 判斷提示，這會要求您在 SAML 權杖屬性設定中新增自訂的屬性對應。 以下螢幕擷取畫面顯示預設屬性清單。 按一下 [編輯]  圖示以開啟 [使用者屬性] 對話方塊。

    ![image](common/edit-attribute.png)

    a. 按一下 [編輯]  圖示，以開啟 [唯一使用者識別碼 (名稱識別碼)]  對話方塊。

    ![Discovery Benefits SSO 設定](./media/discovery-benefits-sso-tutorial/attribute01.png)

    ![Discovery Benefits SSO 設定](./media/discovery-benefits-sso-tutorial/attribute02.png)

    b. 按一下 [編輯]  圖示以開啟 [管理轉換]  對話方塊。

    c. 在 [轉換]  文字方塊中，輸入針對該資料列所顯示的  。

    d. 在 [參數 1]  文字方塊中，輸入 `<Name Identifier value>` 之類的參數。

    e. 按一下 [新增]  。

    > [!NOTE]
    > Discovery Benefits SSO 需要以固定字串值傳入 [唯一使用者識別碼 (名稱識別碼)]  欄位，才能讓此整合運作。 Azure AD 目前不支援這項功能，但您可以使用 NameID 的 **ToUpper** 或 **ToLower** 轉換來設定固定字串值，如上方螢幕擷取畫面所示。

    f. 我們已自動填入 SSO 設定所需的其他宣告 (`SSOInstance` 和 `SSOID`)。 請使用 [編輯]  圖示來根據您的組織對應這些值。

    ![Discovery Benefits SSO 設定](./media/discovery-benefits-sso-tutorial/attribute03.png)

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 Discovery Benefits SSO]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Discovery Benefits SSO 的存取權授與 B.Simon，使其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Discovery Benefits SSO]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-discovery-benefits-sso-sso"></a>設定 Discovery Benefits SSO 的 SSO

若要在 **Discovery Benefits SSO** 端設定單一登入，您必須將從 Azure 入口網站下載的 [憑證 (Base64)]  和複製的適當 URL 傳送給 [Discovery Benefits SSO 支援小組](mailto:Jsimpson@DiscoveryBenefits.com)。 他們會進行此設定，讓兩端的 SAML SSO 連線都設定正確。

### <a name="create-discovery-benefits-sso-test-user"></a>建立 Discovery Benefits SSO 的測試使用者

在本節中，您會在 Discovery Benefits SSO 中建立名為 Britta Simon 的使用者。 請與 [Discovery Benefits SSO 支援小組](mailto:Jsimpson@DiscoveryBenefits.com)合作，在 Discovery Benefits SSO 平台中新增使用者。 您必須先建立和啟動使用者，然後才能使用單一登入。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Discovery Benefits SSO] 圖格時，應該會自動登入您已設定 SSO 的 Discovery Benefits SSO。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 Discovery Benefits SSOO](https://aad.portal.azure.com/)

