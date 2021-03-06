---
title: 將更新、變更追蹤和清查解決方案上線至 Azure 自動化
description: 了解如何將更新與變更追蹤解決方案上線至 Azure 自動化。
services: automation
ms.topic: tutorial
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 721157c333e381799ef08930c667c51a51a4fd6a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457615"
---
# <a name="onboard-update-change-tracking-and-inventory-solutions-to-azure-automation"></a>將更新、變更追蹤和清查解決方案上線至 Azure 自動化

在本教學課程中，您會了解如何將適用於 VM 的更新、變更追蹤與清查解決方案上線至 Azure 自動化：

> [!div class="checklist"]
> * 使 Azure 虛擬機器上線
> * 啟用解決方案
> * 安裝和更新模組
> * 匯入上線 Runbook
> * 啟動 Runbook

>[!NOTE]
>本文已更新為使用新的 Azure PowerShell Az 模組。 AzureRM 模組在至少 2020 年 12 月之前都還會持續收到錯誤 (Bug) 修正，因此您仍然可以持續使用。 若要深入了解新的 Az 模組和 AzureRM 的相容性，請參閱[新的 Azure PowerShell Az 模組簡介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 如需有關混合式 Runbook 背景工作角色的 Az 模組安裝指示，請參閱[安裝 Azure PowerShell 模組](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 針對您的自動化帳戶，您可以使用[如何更新 Azure 自動化中的 Azure PowerShell 模組](automation-update-azure-modules.md)，將模組更新為最新版本。

## <a name="prerequisites"></a>Prerequisites

若要完成此教學課程，需要有下列項目：

* Azure 訂用帳戶。 如果您沒有這類帳戶，可以[啟用自己的 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或註冊[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* [自動化帳戶](automation-offering-get-started.md)，以管理電腦。
* 要上架的[虛擬機器](../virtual-machines/windows/quick-create-portal.md)。

## <a name="onboard-an-azure-vm"></a>使 Azure 虛擬機器上線

讓機器上線的方式有很多種，您可以[從虛擬機器](automation-onboard-solutions-from-vm.md)、[瀏覽多部機器時](automation-onboard-solutions-from-browse.md)、[從您的自動化帳戶](automation-onboard-solutions-from-automation-account.md)，或透過 Runbook 讓解決方案上線。 本教學課程會引導您透過 Runbook 啟用「更新管理」。 若要使 Azure 虛擬機器大規模上線，現有的虛擬機器必須使用變更追蹤或更新管理解決方案來讓現有虛擬機器上線。 在此步驟中，您會使用更新管理和變更追蹤來讓虛擬機器上線。

### <a name="enable-change-tracking-and-inventory"></a>啟用變更追蹤和清查

變更追蹤和清查解決方案可讓您在虛擬機器上[追蹤變更](automation-vm-change-tracking.md)和[進行清查](automation-vm-inventory.md)。 在此步驟中，您會在虛擬機器上啟用解決方案。

1. 在 Azure 入口網站中，選取 [自動化帳戶]  ，然後在清單中選取您的自動化帳戶。
1. 在 [組態管理]  下選取 [清查]  。
1. 選取現有的 Log Analytics 工作區，或建立新的工作區。 
1. 按一下 [啟用]  。

    ![使更新解決方案上線](media/automation-onboard-solutions/inventory-onboard.png)

### <a name="enable-update-management"></a>啟用更新管理

更新管理解決方案可讓您管理 Azure Windows 虛擬機器的更新和修補程式。 您可以評估可用更新的狀態、排定何時安裝必要的更新，並檢閱部署結果來確認更新已成功套用至虛擬機器。 在此步驟中，您會為您的虛擬機器啟用解決方案。

1. 在您的自動化帳戶中，選取 [更新管理]  區段中的 [更新管理]  。
1. 選取的 Log Analytics 工作區是上一個步驟中使用的工作區。 按一下 [啟用]  ，讓更新管理解決方案上線。 更新管理解決方案正在安裝時，您會看到藍色橫幅。 

    ![使更新解決方案上線](media/automation-onboard-solutions/update-onboard.png)

### <a name="select-azure-vm-to-be-managed"></a>選取要管理的 Azure 虛擬機器

現在解決方案已啟用，您可以新增要上線至這些解決方案的 Azure 虛擬機器。

1. 從您的自動化帳戶中，選取 [組態管理]  下的 [變更追蹤]  。 
2. 在 [變更追蹤] 頁面上，按一下 [新增 Azure VM]  來新增您的 VM。

3. 從清單中選取您的虛擬機器，並按一下 [啟用]  。 此動作會啟用 VM 的變更追蹤和清查解決方案。

   ![啟用虛擬機器的更新解決方案](media/automation-onboard-solutions/enable-change-tracking.png)

4. 完成 VM 上線通知後，在 [更新管理]  底下選取 [更新管理]  。

5. 選取 [新增 Azure VM]  來新增您的 VM。

6. 從清單中選取您的虛擬機器，並選取 [啟用]  。 此動作會啟用 VM 的更新管理解決方案。

   ![啟用虛擬機器的更新解決方案](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> 如果您沒有等待另一個解決方案完成，在啟用下一個解決方案時，就會收到此訊息：`Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>安裝和更新模組

需要更新為最新的 Azure 模組並匯入 [Az.OperationalInsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0) 模組，才能成功將解決方案上線自動化。

## <a name="update-azure-modules"></a>更新 Azure 模組

1. 在您的自動化帳戶中，選取 [共用資源]  下的 [模組]  。 
2. 選取 [更新 Azure 模組]  ，可將 Azure 模組更新為最新版本。 
3. 按一下 [是]  ，即可將所有現有的 Azure 模組更新為最新版本。

![更新模組](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azoperationalinsights-module"></a>安裝 Az.OperationalInsights 模組

1. 在您的自動化帳戶中，選取 [共用資源]  下的 [模組]  。 
2. 選取 [瀏覽資源庫]  以開啟模組資源庫。 
3. 搜尋 Az.OperationalInsights，並將此模組匯入至自動化帳戶。

![匯入 OperationalInsights 模組](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>匯入上線 Runbook

1. 在您的自動化帳戶中，選取 [程序自動化]  下的 [Runbook]  。
1. 選取 [瀏覽資源庫]  。
1. 搜尋 `update and change tracking`。
3. 選取 Runbook，然後按一下 [檢視來源] 頁面上的 [匯入]  。 
4. 按一下 [確定]  ，然後將 Runbook 匯入至自動化帳戶。

   ![匯入上線 Runbook](media/automation-onboard-solutions/import-from-gallery.png)

6. 在 [Runbook] 頁面上，按一下 [編輯]  ，然後選取 [發佈]  。 
7. 在 [發佈 Runbook] 窗格中，按一下 [是]  來發佈 Runbook。

## <a name="start-the-runbook"></a>啟動 Runbook

您必須已將變更追蹤或更新解決方案上線至 Azure VM，才能啟動這個 Runbook。 它需要目前具有已針對參數上線之解決方案的虛擬機器和資源群組。

1. 開啟 **Enable-MultipleSolution** Runbook。

   ![多個解決方案 Runbook](media/automation-onboard-solutions/runbook-overview.png)

1. 按一下 [開始] 按鈕，並針對參數輸入下列值。

   * **VMNAME** - 保留為空白。 要上線至更新或變更追蹤解決方案的現有 VM 名稱。 藉由將此保留空白，系統會將資源群組中的所有虛擬機器上線。
   * **VMRESOURCEGROUP** - 要上線的虛擬機器資源群組名稱。
   * **SUBSCRIPTIONID** - 保留為空白。 要上線之新虛擬機器的訂用帳戶識別碼。 若保留空白，系統會使用工作區的訂用帳戶。 指定不同的訂用帳戶識別碼時，也應該將此自動化帳戶的 RunAs 帳戶新增為此訂用帳戶的參與者。
   * **ALREADYONBOARDEDVM** - 已手動上線至更新或變更追蹤解決方案的虛擬機器名稱。
   * **ALREADYONBOARDEDVMRESOURCEGROUP** - VM 所屬的資源群組名稱。
   * **SOLUTIONTYPE** - 輸入 **Updates** 或 **ChangeTracking**。

   ![Enable-MultipleSolution Runbook 參數。](media/automation-onboard-solutions/runbook-parameters.png)

1. 選取 [確定]  以啟動 Runbook 作業。
1. 在 Runbook 作業頁面上監視進度和任何錯誤。

## <a name="clean-up-resources"></a>清除資源

從「更新管理」中移除 VM：

1. 在您的 Log Analytics 工作區中，從 `MicrosoftDefaultScopeConfig-Updates` 範圍設定的已儲存搜尋中移除 VM。 您可以在工作區中的 [一般]  底下找到儲存的搜尋。
2. 移除[適用於 Windows 的 Log Analytics 代理程式](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources)或[適用於 Linux 的 Log Analytics 代理程式](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 手動將 Azure 虛擬機器上線。
> * 安裝及更新所需的 Azure 模組。
> * 匯入能將 Azure VM 上線的 Runbook。
> * 啟動會自動將 Azure VM 上線的 Runbook。

遵循此連結，即可深入了解 Runbook 排程。

> [!div class="nextstepaction"]
> [為 RunBook 安排排程](automation-schedules.md)。
