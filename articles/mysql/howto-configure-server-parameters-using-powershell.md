---
title: 設定伺服器參數-Azure PowerShell 適用於 MySQL 的 Azure 資料庫
description: 本文說明如何使用 PowerShell 在適用於 MySQL 的 Azure 資料庫中設定服務參數。
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurepowershell
ms.topic: conceptual
ms.date: 4/29/2020
ms.openlocfilehash: 0de816d25bbc1563885413d8dbd52dc7bda7d538
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2020
ms.locfileid: "82615080"
---
# <a name="customize-azure-database-for-mysql-server-parameters-using-powershell"></a>使用 PowerShell 自訂適用於 MySQL 的 Azure 資料庫伺服器參數

您可以使用 PowerShell 來列出、顯示和更新適用於 MySQL 的 Azure 資料庫伺服器的設定參數。 有一部分的引擎設定會在伺服器層級公開而且可供修改。

## <a name="prerequisites"></a>Prerequisites

若要完成本操作說明指南，您需要：

- [Az PowerShell 模組](/powershell/azure/install-az-ps)已安裝在本機或[Azure Cloud Shell](https://shell.azure.com/)在瀏覽器中
- [適用於 MySQL 的 Azure 資料庫伺服器](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> 當 Az MySql PowerShell 模組處於預覽狀態時，您必須使用下列命令，將它與 Az PowerShell 模組分開安裝： `Install-Module -Name Az.MySql -AllowPrerelease`。
> 在 Az MySql PowerShell 模組正式推出之後，它會成為未來 Az PowerShell 模組版本的一部分，並可從 Azure Cloud Shell 內以原生方式使用。

如果您選擇在本機使用 PowerShell，請使用[disconnect-azaccount](/powershell/module/az.accounts/Connect-AzAccount) Cmdlet 連接到您的 Azure 帳戶。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>列出適用於 MySQL 的 Azure 資料庫伺服器的伺服器設定參數

若要列出伺服器中所有可修改的參數及其值，請`Get-AzMySqlConfiguration`執行 Cmdlet。

下列範例會列出資源群組**myresourcegroup**中伺服器**mydemoserver**的伺服器設定參數。

```azurepowershell-interactive
Get-AzMySqlConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

如需每個列出參數的定義，請參閱[伺服器系統變數](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html) \(英文\) 中的 MySQL 參考小節。

## <a name="show-server-configuration-parameter-details"></a>顯示伺服器設定參數的詳細資料

若要顯示有關伺服器特定設定參數的詳細資料，請執行`Get-AzMySqlConfiguration` Cmdlet，並指定**Name**參數。

這個範例會針對資源群組**myresourcegroup**下的伺服器**mydemoserver** ，顯示**\_緩慢查詢\_記錄**伺服器設定參數的詳細資料。

```azurepowershell-interactive
Get-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>修改伺服器設定參數值

您也可以修改特定伺服器設定參數的值，以更新適用於 MySQL 伺服器引擎的基礎設定值。 若要更新設定，請使用`Update-AzMySqlConfiguration` Cmdlet。

若要更新資源群組**myresourcegroup**下伺服器**mydemoserver**的**\_緩慢查詢\_記錄**伺服器設定參數。

```azurepowershell-interactive
Update-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [使用 PowerShell 在適用於 MySQL 的 Azure 資料庫伺服器中自動增加儲存空間](howto-auto-grow-storage-powershell.md)。
