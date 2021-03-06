---
title: 診斷 VM 網路路由問題-Azure PowerShell
titleSuffix: Azure Network Watcher
description: 在本文章中，您將了解如何使用 Azure 網路監看員的下一個躍點功能，以診斷虛擬機器網路路由問題。
services: network-watcher
documentationcenter: network-watcher
author: damendo
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: damendo
ms.custom: ''
ms.openlocfilehash: b5a636471eab188dc8648761afedd81694331953
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "76834698"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>診斷虛擬機器網路路由問題 - Azure PowerShell

在本文章中，您將部署虛擬機器 (VM)，然後檢查送至 IP 位址和 URL 的通訊。 您可判斷通訊失敗的原因及其解決方式。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

如果您選擇在本機安裝和使用 PowerShell，本文需要 Azure PowerShell `Az`模組。 若要尋找已安裝的版本，請執行 `Get-Module -ListAvailable Az`。 如果您需要升級，請參閱[安裝 Azure PowerShell 模組](/powershell/azure/install-Az-ps)。 如果您在本機執行 PowerShell，則也需要執行 `Connect-AzAccount` 以建立與 Azure 的連線。



## <a name="create-a-vm"></a>建立 VM

您必須先建立資源群組來包含 VM，才能建立 VM。 使用 [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup) 來建立資源群組。 下列範例會在 eastus  位置建立名為 myResourceGroup  的資源群組。

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

使用 [New-AzVM](/powershell/module/az.compute/new-azvm) 建立 VM。 執行此步驟時，系統會提示您輸入認證。 您輸入的值會設定為 VM 的使用者名稱和密碼。

```azurepowershell-interactive
$vM = New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

建立 VM 需要幾分鐘的時間。 在建立 VM 且 PowerShell 傳回輸出之前，請勿繼續進行其餘步驟。

## <a name="test-network-communication"></a>測試網路通訊

若要測試網路監看員的網路通訊，您必須先在欲測試的 VM 所在區域啟用網路監看員，然後使用網路監看員的下一個躍點功能來測試通訊。

## <a name="enable-network-watcher"></a>啟用網路監看員

如果您已經在美國東部區域啟用網路監看員，請使用 [New-AzNetworkWatcher](/powershell/module/az.network/get-aznetworkwatcher) 來擷取網路監看員。 下列範例會擷取名為 *NetworkWatcher_eastus* 的現有網路監看員，其位於 *NetworkWatcherRG* 資源群組中：

```azurepowershell-interactive
$networkWatcher = Get-AzNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

如果您尚未在美國東部區域啟用網路監看員，請使用 [New-AzNetworkWatcher](/powershell/module/az.network/new-aznetworkwatcher) 在美國東部區域建立網路監看員：

```azurepowershell-interactive
$networkWatcher = New-AzNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>使用下一個躍點

Azure 會自動建立通往預設目的地的路由。 您可以建立覆寫預設路由的自訂路由。 有時候，自訂路由可能會導致通訊失敗。 若要測試來自 VM 的路由，請使用[AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop)命令來判斷流量以特定位址為目標時的下一個路由躍點。

測試從 VM 輸出至 www.bing.com 其中一個 IP 位址的通訊：

```azurepowershell-interactive
Get-AzNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

幾秒之後，輸出會通知您 **NextHopType** 是 **Internet**，而 **RouteTableId** 是 **System Route**。 此結果可讓您知道通往目的地的路由是有效的。

測試從 VM 輸出至 172.31.0.100 的通訊：

```azurepowershell-interactive
Get-AzNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

傳回的輸出會通知您 **NextHopType** 是 **None**，而 **RouteTableId** 也是 **System Route**。 此結果可讓您知道，雖然通往目的地的系統路由是有效的，但沒有下一個躍點可將流量路由至目的地。

## <a name="view-details-of-a-route"></a>檢視路由的詳細資料

若要進一步分析路由，請使用[AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)命令來檢查網路介面的有效路由：

```azurepowershell-interactive
Get-AzEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

會傳回包含下列文字的輸出：

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

如先前輸出所示，**AddressPrefix** 為 **0.0.0.0/0** 的路由會路由傳送下列所有流量：目的地位址不在其他路由位址前置詞內且下一個躍點為 **Internet**。 亦如輸出所示，雖然有通往 172.16.0.0/12 前置詞的預設路由，且其中包含 172.31.0.100 位址，但 **nextHopType** 是 **None**。 Azure 會建立通往 172.16.0.0/12 的預設路由，但不會指定下一個躍點類型，除非有理由這麼做。 例如，如果您將 172.16.0.0/12 位址範圍新增至虛擬網路的位址空間，則 Azure 會將此路由的 **nextHopType** 變更為 **Virtual network**。 執行檢查就會看到 **Virtual network** 顯示為 **nextHopType**。

## <a name="clean-up-resources"></a>清除資源

您可以使用 [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) 來移除不再需要的資源群組，以及其所包含的所有資源：

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>後續步驟

在本文章中，您已建立 VM 並診斷來自 VM 的網路路由。 您已了解 Azure 會建立數個預設路由，並測試通往兩個不同目的地的路由。 深入了解 [Azure 中的路由](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)以及如何[建立自訂路由](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route)。

針對輸出 VM 連線，您也可以使用網路監看員的[連線疑難排解](network-watcher-connectivity-powershell.md)功能，以判斷 VM 與端點之間的延遲，以及所允許和拒絕的網路流量。 您可以使用網路監看員的連線監視功能，以監視某段時間內 VM 與端點 (例如 IP 位址或 URL) 之間的通訊情形。 若要了解方法，請參閱[監視網路連線](connection-monitor.md)。
