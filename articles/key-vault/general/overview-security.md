---
title: Azure Key Vault 安全性 |Microsoft Docs
description: 針對 Azure Key Vault、金鑰和祕密進行存取權限管理。 包含 Key Vault 的驗證和授權模型以及如何保護金鑰保存庫。
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: mbaldwin
Customer intent: As a key vault administrator, I want to learn the options available to secure my vaults
ms.openlocfilehash: dbe13b2d1291f212af7da9d1176bc3d90997978b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81428962"
---
# <a name="azure-key-vault-security"></a>Azure Key Vault 安全性

您需要在雲端中保護加密金鑰和秘密，例如憑證、連接字串和密碼，以使用 Azure Key Vault。 由於您要儲存機密和商務關鍵資料，因此您必須採取步驟，將保存庫和其中所儲存之資料的安全性最大化。 本文將討論您在設計 Azure Key Vault 安全性時，應該考慮的一些概念。

## <a name="identity-and-access-management"></a>身分識別和存取管理

當您在 Azure 訂用帳戶中建立金鑰保存庫時，它會自動與該訂用帳戶的 Azure AD 租用戶建立關聯。 嘗試從保存庫管理或抓取內容的任何人，都必須透過 Azure AD 進行驗證。

- 驗證會建立呼叫者的身分識別。
- 授權會決定呼叫者可以執行的作業。 Key Vault 中的授權會使用[角色型存取控制](../../role-based-access-control/overview.md)（RBAC）和 Azure Key Vault 存取原則的組合。

### <a name="access-model-overview"></a>存取模型概觀

保存庫的存取會透過兩個介面或平面進行。 這些平面是管理平面和資料平面。

- *管理平面*是您管理 Key Vault 本身的位置，而它是用來建立和刪除保存庫的介面。 您也可以讀取 key vault 屬性和管理存取原則。
- *資料平面*可讓您使用儲存在金鑰保存庫中的資料。 您可以新增、刪除和修改金鑰、祕密和憑證。

若要在任一平面存取金鑰保存庫，所有呼叫者（使用者或應用程式）都必須經過驗證和授權。 兩個平面都會使用 Azure Active Directory (Azure AD) 來進行驗證。 至於授權，管理平面會使用角色型存取控制 (RBAC)，資料平面則會使用 Key Vault 存取原則。

這兩個平面的單一驗證機制模型有幾個優點：

- 組織可以集中控制其組織內所有金鑰保存庫的存取權。
- 使用者如果離職，就會立即失去組織中所有金鑰保存庫的存取權。
- 組織可以使用 Azure AD 中的選項來自訂驗證，例如啟用多重要素驗證以提高安全性

### <a name="managing-administrative-access-to-key-vault"></a>管理 Key Vault 的系統管理存取權

當您在資源群組中建立金鑰保存庫時，您可以使用 Azure AD 來管理存取權。 您可以對使用者或群組授與在資源群組中管理金鑰保存庫的能力。 您可以藉由指派適當的 RBAC 角色，在特定範圍層級授與存取權。 若要對使用者授與管理金鑰保存庫的權限，您可以在特定範圍對使用者指派預先定義的 `key vault Contributor` 角色。 您可以對 RBAC 角色指派下列範圍層級：

- **訂**用帳戶：在訂用帳戶層級指派的 RBAC 角色會套用至該訂用帳戶內的所有資源群組和資源。
- **資源群組**：在資源群組層級指派的 RBAC 角色會套用至該資源群組中的所有資源。
- **特定資源**：指派給特定資源的 RBAC 角色會套用至該資源。 在此情況下，資源會是特定的金鑰保存庫。

有數個預先定義的角色。 如果預先定義的角色不符合您的需求，您可以定義您自己的角色。 如需詳細資訊，請參閱[RBAC：內建角色](../../role-based-access-control/built-in-roles.md)。

> [!IMPORTANT]
> 如果使用者具有金鑰保存庫管理平面的 `Contributor` 權限，就可以透過設定 Key Vault 存取原則，對自己授與資料平面的存取權。 因此，請嚴格控制誰可以有金鑰保存庫的 `Contributor` 角色存取權。 請確定只有獲得授權的人員可以存取和管理您的金鑰保存庫、金鑰、祕密和憑證。

<a id="data-plane-access-control"></a>
### <a name="controlling-access-to-key-vault-data"></a>控制 Key Vault 資料的存取

Key Vault 存取原則會分別授與許可權給金鑰、秘密或憑證。 您只能對使用者授與存取金鑰 (而非祕密) 的權限。 金鑰、秘密和憑證的存取權限是在保存庫層級進行管理。

> [!IMPORTANT]
> Key Vault 存取原則不支援細微物件層級的權限，例如特定金鑰、祕密或憑證。 對使用者授與金鑰的建立和刪除權限時，其便可對該金鑰保存庫的所有金鑰執行這些作業。

若要設定金鑰保存庫的存取原則，請使用 [Azure 入口網站](https://portal.azure.com/)、[Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)、[Azure PowerShell](/powershell/azureps-cmdlets-docs) 或 [Key Vault 管理 REST API](/rest/api/keyvault/)。

您可以使用[Azure Key Vault 的虛擬網路服務端點](overview-vnet-service-endpoints.md)）來限制資料平面存取。 您可以設定[防火牆和虛擬網路規則](network-security.md)來額外加上一層安全性。

## <a name="network-access"></a>網路存取

您可以藉由指定可存取的 IP 位址，來減少保存庫的暴露。 Azure Key Vault 的虛擬網路服務端點可讓您將存取權限制為指定的虛擬網路。 這些端點也可讓您將存取權限制為 IPv4 (網際網路通訊協定第 4 版) 位址範圍的清單。 任何從這些來源之外連線到金鑰保存庫的使用者都會被拒絕存取。

防火牆規則生效後，使用者只能在其要求源自允許的虛擬網路或 IPv4 位址範圍時，從 Key Vault 讀取資料。 這也適用於從 Azure 入口網站存取 Key Vault。 雖然使用者可以從 Azure 入口網站瀏覽金鑰保存庫，但是如果其用戶端電腦不在允許的清單中，他們就無法列出金鑰、祕密或憑證。 這也會影響其他 Azure 服務的 [金鑰保存庫選擇器]。 使用者可以看到金鑰保存庫清單，但是如果防火牆規則阻止其用戶端電腦，則不會列出金鑰。

如需 Azure Key Vault 網路位址的詳細資訊[，請參閱 Azure Key Vault 的虛擬網路服務端點](overview-vnet-service-endpoints.md)）

## <a name="monitoring"></a>監視

Key Vault 記錄會儲存在保存庫上執行之活動的相關資訊。 Key Vault 記錄：

- 所有已驗證的 REST API 要求，包括失敗的要求
  - 金鑰保存庫本身的作業。 這些作業包括建立、刪除、設定存取原則，以及更新金鑰保存庫屬性（例如標記）。
  - 金鑰保存庫中的金鑰和祕密作業，包括：
    - 建立、修改或刪除這些金鑰或祕密。
    - 簽署、驗證、加密、解密、包裝和解除包裝金鑰、取得秘密，以及列出金鑰和祕密 (及其版本)。
- 產生 401 回應的未經驗證要求。 例如，沒有持有人權杖的要求、格式不正確或已過期的要求，或具有無效權杖的要求。

記錄資訊可在金鑰保存庫作業之後的10分鐘記憶體取。 您可以在儲存體帳戶中管理您的記錄。

- 請使用標準的 Azure 存取控制方法限制可存取記錄的人員，藉此來保護記錄。
- 刪除不想繼續保留在儲存體帳戶中的記錄。

如需安全管理儲存體帳戶的建議，請參閱[Azure 儲存體安全性指南](../../storage/blobs/security-recommendations.md)

## <a name="next-steps"></a>後續步驟

- [Azure Key Vault 的虛擬網路服務端點](overview-vnet-service-endpoints.md)）
- [RBAC：內建角色](../../role-based-access-control/built-in-roles.md)
- [Azure Key Vault 的虛擬網路服務端點](overview-vnet-service-endpoints.md)）
