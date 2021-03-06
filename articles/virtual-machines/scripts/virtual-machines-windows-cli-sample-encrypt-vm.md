---
title: Azure CLI 指令碼範例 - 將 Windows VM 加密
description: Azure CLI 指令碼範例 - 將 Windows VM 加密
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 857bddaad32ea98635f5fbd2f8202975fb033b70
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "81459435"
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>如何在 Azure 中將 Windows 虛擬機器加密

此指令碼會建立一個安全的 Azure Key Vault、加密金鑰、Azure Active Directory 服務主體，以及 Window 虛擬機器 (VM)。 然後會使用 Key Vault 中的加密金鑰和服務主體認證將 VM 加密。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>範例指令碼

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>清除部署 

執行下列命令來移除資源群組、VM 和所有相關資源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令來建立資源群組、Azure Key Vault、服務主體、虛擬機器和所有相關資源。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | 建立用來存放所有資源的資源群組。 |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault) | 建立 Azure Key Vault 來儲存安全資料，例如加密金鑰。 |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key) | 在 Key Vault 中建立加密金鑰。 |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp) | 建立 Azure Active Directory 服務主體，以安全地進行驗證及控制對加密金鑰的存取。 |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault) | 在 Key Vault 上設定權限，以授與服務主體對加密金鑰的存取權。 |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | 建立虛擬機器，並將它連線到網路卡、虛擬網路、子網路及 NSG。 此命令也會指定要使用的虛擬機器映像和管理認證。  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption) | 使用服務主體認證和加密金鑰在 VM 上啟用加密。 |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption) | 顯示 VM 加密程序的狀態。 |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | 刪除資源群組，包括所有的巢狀資源。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](https://docs.microsoft.com/cli/azure)。

您可以在 [Azure Windows VM 文件](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json)中找到其他的虛擬機器 CLI 指令碼範例。
