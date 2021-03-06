---
title: 教學課程：Azure Active Directory 與 123FormBuilder SSO 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 123FormBuilder SSO 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 947a9d632089b18f6b950c5eecbcb74d061f32eb
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275748"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-123formbuilder-sso"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 123FormBuilder SSO 整合

在本教學課程中，您將了解如何整合 123FormBuilder SSO 與 Azure Active Directory (Azure AD)。 在整合 123FormBuilder SSO 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 123FormBuilder SSO 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 123FormBuilder SSO。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 123FormBuilder SSO 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* 123FormBuilder SSO 支援由 **SP 和 IDP** 起始的 SSO。
* 123FormBuilder SSO 支援 **Just In Time** 使用者佈建。
* 設定 123FormBuilder SSO 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

## <a name="adding-123formbuilder-sso-from-the-gallery"></a>從資源庫新增 123FormBuilder SSO

若要進行將 123FormBuilder SSO 整合到 Azure AD 中的設定，您必須從資源庫將 123FormBuilder SSO 新增至受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中，輸入 **123FormBuilder SSO**。
1. 從結果面板中選取 [123FormBuilder SSO]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-123formbuilder-sso"></a>設定及測試 123FormBuilder SSO 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 123FormBuilder SSO 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 123FormBuilder SSO 中相關使用者之間的連結關聯性。

若要設定及測試與 123FormBuilder SSO 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    * **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    * **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 123FormBuilder SSO](#configure-123formbuilder-sso)** - 在應用程式端設定單一登入設定。
    * **[建立 123FormBuilder SSO 測試使用者](#create-123formbuilder-sso-test-user)** - 使 123FormBuilder SSO 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [123FormBuilder SSO]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態]  區段上，若您想要以 **IDP** 起始模式設定應用程式，請執行下列步驟：

    a. 在 [識別碼]  文字方塊中，使用下列模式來輸入 URL：`https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/metadata`


    b. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/acs`

5. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE]
    > 這些都不是真正的值。 您將從實際的 URL 和識別碼更新這些值，本教學課程稍後將會說明。

1. 在 [以 SAML 設定單一登入]  頁面上的 [SAML 簽署憑證]  區段中，尋找 [同盟中繼資料 XML]  ，然後選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/metadataxml.png)

1. 在 [設定 123FormBuilder SSO]  區段上，根據您的需求複製適當的 URL。

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

在本節中，您會將 123FormBuilder SSO 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [123FormBuilder SSO]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-123formbuilder-sso"></a>設定 123FormBuilder SSO

1. 若要在 **123FormBuilder SSO** 端設定單一登入，請移至 [https://www.123formbuilder.com/form-2709121/](https://www.123formbuilder.com/form-2709121/)，然後執行下列步驟：

    ![設定單一登入](./media/123formbuilder-tutorial/submit.png) 

    a. 在 [電子郵件]  文字方塊中，輸入使用者的電子郵件，例如 `B.Simon@Contoso.com`。

    b. 按一下 [上傳]  ，然後瀏覽您已從 Azure 入口網站下載的「中繼資料 XML」檔案。

    c. 按一下 [送出表單]  。

2. 在 [Microsoft Azure AD] > [單一登入] > [設定應用程式設定]  上，執行下列步驟：

    ![設定單一登入](./media/123formbuilder-tutorial/url3.png)

    a. 如果您想要以「IDP 起始模式」  設定應用程式，請複製執行個體的 [識別碼]  值，然後將它貼到 Azure 入口網站上 [基本 SAML 設定]  區段中的 [識別碼]  文字方塊中。

    b. 如果您想要以「IDP 起始模式」  設定應用程式，請複製執行個體的 [回覆 URL]  值，然後將它貼到 Azure 入口網站上 [基本 SAML 設定]  區段中的 [回覆 URL]  文字方塊中。

    c. 如果您想要以「SP 起始模式」  設定應用程式，請複製執行個體的 [登入 URL]  值，然後將它貼到 Azure 入口網站上 [基本 SAML 設定]  區段中的 [登入 URL]  文字方塊中。

### <a name="create-123formbuilder-sso-test-user"></a>建立 123FormBuilder SSO 測試使用者

本節會在 123FormBuilder SSO 中建立名為 Britta Simon 的使用者。 123FormBuilder SSO 支援依預設啟用的 Just-In-Time 使用者佈建。 在這一節沒有您需要進行的動作項目。 如果 123FormBuilder SSO 中還沒有任何使用者存在，在驗證之後就會建立新的使用者。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [123FormBuilder SSO] 圖格時，應該會自動登入您已設定 SSO 的 123FormBuilder SSO。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [搭配 Azure AD 試用 123FormBuilder SSO](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
