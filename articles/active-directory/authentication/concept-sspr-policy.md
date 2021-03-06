---
title: 自助式密碼重設原則 - Azure Active Directory
description: 瞭解不同的 Azure Active Directory 自助式密碼重設原則選項
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8b6d08dd2073de80ac0f7fd08f510d9cda80545
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82143246"
---
# <a name="self-service-password-reset-policies-and-restrictions-in-azure-active-directory"></a>Azure Active Directory 中的自助式密碼重設原則和限制

本文說明與在 Azure Active Directory (Azure AD) 租用戶中的使用者帳戶相關聯的密碼原則和複雜性需求。

## <a name="administrator-reset-policy-differences"></a>系統管理員重設原則差異

**Microsoft 會針對任何 Azure 系統管理員角色強制執行強*式預設雙閘道*密碼重設原則**。 此原則可能與您為使用者定義的原則不同，且無法變更此原則。 請一律要以未指派任何 Azure 系統管理員角色的使用者身分測試密碼重設功能。

利用雙閘道原則，**系統管理員就不需要有使用安全性問題的能力**。

兩個閘道原則需要兩項驗證資料，例如*電子郵件地址*、*驗證器應用程式*或*電話號碼*。 兩個閘道原則適用於下列情況：

* 下列 Azure 系統管理員角色會受到影響：
  * 服務台管理員
  * 服務支援管理員
  * 計費管理員
  * 合作夥伴第 1 層支援
  * 合作夥伴第 2 層支援
  * Exchange 系統管理員
  * 商務用 Skype 的管理員
  * 使用者管理員
  * 目錄寫入器
  * 全域管理員或公司系統管理員
  * SharePoint 管理員
  * 規範管理員
  * 應用程式管理員
  * 安全性系統管理員
  * 特殊權限角色管理員
  * Intune 系統管理員
  * 應用程式 Proxy 服務管理員
  * Dynamics 365 管理員
  * Power BI 服務管理員
  * 驗證管理員
  * 具有特殊許可權的驗證管理員

* 如果試用版訂用帳戶已經過 30 天；或
* 已為您的 Azure AD 租使用者設定自訂網域，例如*contoso.com*;或
* Azure AD Connect 正在同步處理內部部署目錄中的身分識別

### <a name="exceptions"></a>例外狀況

一個閘道原則需要一項驗證資料，例如電子郵件地址或電話號碼。 一個閘道原則適用於下列情況：

* 處於試用訂用帳戶的第一個 30 天內；或
* 尚未為您的 Azure AD 租使用者設定自訂網域，因此會使用預設的 **. onmicrosoft.com*。 不建議在生產環境中使用預設的 **. onmicrosoft.com*網域;和
* Azure AD Connect 未同步身分識別

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>適用於所有使用者帳戶的 UserPrincipalName 原則

每個需要登入 Azure AD 的使用者帳戶都必須具有與其帳戶相關聯的唯一使用者主體名稱 (UPN) 屬性值。 下表概述同時適用于內部部署 Active Directory Domain Services 使用者帳戶（同步處理至雲端）和僅限雲端使用者帳戶的原則：

| 屬性 | UserPrincipalName 需求 |
| --- | --- |
| 允許的字元 |<ul> <li>A – Z</li> <li>a - z</li><li>0 – 9</li> <li> ' \. - \_ ! \# ^ \~</li></ul> |
| 不允許的字元 |<ul> <li>任何不是用來分隔使用者名稱和網域的 "\@\" 字元。</li> <li>"\@\" 符號前面不可包含緊鄰的句點字元 "."</li></ul> |
| 長度限制 |<ul> <li>總長度不得超過 113 個字元</li><li>"\@\" 符號前面最多可以有 64 個字元</li><li>"\@\" 符號後面最多可以有 48 個字元</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>僅適用於雲端使用者帳戶的密碼原則

下表描述在 Azure AD 中建立和管理的使用者帳戶所套用的密碼原則設定：

| 屬性 | 需求 |
| --- | --- |
| 允許的字元 |<ul><li>A – Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [] {} &#124; \： '，。 ? / \`~ " ( ) ;</li> <li>空格</li></ul> |
| 不允許的字元 | Unicode 字元。 |
| 密碼限制 |<ul><li>最少8個字元，最多256個字元。</li><li>需要下列四種字元中的三種：<ul><li>小寫字元。</li><li>大寫字元。</li><li>數字 (0-9)。</li><li>符號 (請參閱先前的密碼限制)。</li></ul></li></ul> |
| 密碼到期期限（密碼最長使用期限） |<ul><li>預設值： **90**天。</li><li>可透過適用於 Windows PowerShell 的 Azure Active Directory 模組使用 `Set-MsolPasswordPolicy` Cmdlet 設定該值。</li></ul> |
| 密碼到期通知（當使用者收到密碼到期的通知時） |<ul><li>預設值： **14**天（密碼到期之前）。</li><li>可使用 `Set-MsolPasswordPolicy` Cmdlet 設定此值。</li></ul> |
| 密碼到期（讓密碼永不過期） |<ul><li>預設值： **false** （表示密碼的到期日）。</li><li>可以使用 `Set-MsolUser` Cmdlet 針對個別使用者帳戶設定該值。</li></ul> |
| 密碼變更歷程記錄 | 當使用者變更密碼時，*不能*重複使用上次的密碼。 |
| 密碼重設歷程記錄 | 當使用者重設忘記密碼時，*不能*重複使用上次的密碼。 |
| 帳戶鎖定 | 使用錯誤的密碼於 10 次失敗的登入嘗試之後，使用者會被封鎖一分鐘。 後續錯誤的登入嘗試會增加使用者被封鎖的時間。 [智慧鎖定](howto-password-smart-lockout.md)會追蹤最後三個不正確的密碼雜湊，以避免因為相同密碼而累計鎖定計數器。 如果有人多次輸入相同的錯誤密碼，此行為將不會造成帳戶鎖定。 |

## <a name="set-password-expiration-policies-in-azure-ad"></a>在 Azure AD 中設定密碼到期原則

Microsoft 雲端服務的*全域管理員*或*使用者管理員*可以使用*適用於 Windows PowerShell 的 Microsoft Azure AD 模組*將使用者密碼設定為不過期。 您亦可使用 Windows PowerShell Cmdlet 移除永不到期組態，或是查看哪些使用者密碼設為永不到期。

本指引適用於其他提供者 (例如 Intune 和 Office 365)，它們也仰賴 Azure AD 提供身分識別和目錄服務。 密碼到期是原則中唯一可變更的部分。

> [!NOTE]
> 您僅可將未透過目錄同步作業執行同步處理的使用者帳戶密碼，設定為不會到期。 如需目錄同步作業的詳細資訊，請參閱 [Connect AD 與 Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)。

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>使用 PowerShell 設定或檢查密碼原則

若要開始使用，請[下載並安裝 Azure AD PowerShell 模組](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0)，並[將它連線到您的 Azure AD 租使用者](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0#examples)。 安裝模組之後，請使用下列步驟來設定每個欄位。

### <a name="check-the-expiration-policy-for-a-password"></a>檢查密碼的到期原則

1. 使用您的使用者系統管理員或公司系統管理員認證連接到 Windows PowerShell。
1. 執行下列其中一個命令：

   * 若要查看單一使用者的密碼是否設定為永不過期，請使用 UPN （例如*aprilr\@contoso.onmicrosoft.com*）或您想要檢查之使用者的使用者識別碼，執行下列 Cmdlet：

   ```powershell
   Get-AzureADUser -ObjectId <user ID> | Select-Object @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

   * 若要查看所有使用者的**密碼永久有效**設定，請執行下列 Cmdlet：

   ```powershell
   Get-AzureADUser -All $true | Select-Object UserPrincipalName, @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

### <a name="set-a-password-to-expire"></a>設定密碼到期

1. 使用您的使用者系統管理員或公司系統管理員認證連接到 Windows PowerShell。
1. 執行下列其中一個命令：

   * 若要將某位使用者的密碼設為會到期，請透過使用 UPN 或使用者的使用者識別碼，執行下列 Cmdlet：

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies None
   ```

   * 若要將組織中所有使用者的密碼設為會到期，請使用下列 Cmdlet：

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies None
   ```

### <a name="set-a-password-to-never-expire"></a>設定密碼為永久有效

1. 使用您的使用者系統管理員或公司系統管理員認證連接到 Windows PowerShell。
1. 執行下列其中一個命令：

   * 若要將某位使用者的密碼設為永不過期，請透過使用使用者主體名稱 (UPN) 或使用者的使用者識別碼，來執行下列 Cmdlet：

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies DisablePasswordExpiration
   ```

   * 若要將組織中所有使用者的密碼設定為永不過期，請執行下列指令程式：

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies DisablePasswordExpiration
   ```

   > [!WARNING]
   > 設定為 `-PasswordPolicies DisablePasswordExpiration` 的密碼仍然會根據 `pwdLastSet` 屬性計算時效。 根據 `pwdLastSet` 屬性，如果您將到期變更為 `-PasswordPolicies None`，`pwdLastSet` 早於 90 天的所有密碼會要求使用者在下次登入時變更密碼。 這項變更可能會影響大量使用者。

## <a name="next-steps"></a>後續步驟

若要開始使用 SSPR，請參閱[教學課程：讓使用者使用 Azure Active Directory 自助式密碼重設來解除鎖定其帳戶或重設密碼](tutorial-enable-sspr.md)。

如果您或使用者有 SSPR 的問題，請參閱針對[自助式密碼重設進行疑難排解](active-directory-passwords-troubleshoot.md)
