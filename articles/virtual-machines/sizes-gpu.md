---
title: Azure VM 大小-GPU |Microsoft Docs
description: 列出 Azure 中適用于虛擬機器的不同 GPU 優化大小。 列出 vCPU 數目、資料磁碟和 NIC 的相關資訊，以及此服務中各種大小之儲存體輸送量和網路頻寬的相關資訊。
services: virtual-machines
documentationcenter: ''
author: vikancha
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 5d36ba05d2138a06ebb2ef4e49aadb6032b62b92
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82627036"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU 最佳化的虛擬機器大小

GPU 優化的 VM 大小是使用單一、多個或小數 Gpu 提供的特製化虛擬機器。 這些大小是專門針對計算密集型、圖形密集型及視覺效果的工作負載所設計。 本文章提供有關 GPU、vCPU、資料磁碟及 NIC 之數量和類型的資訊。 另說明此群組中每個大小的輸送量和網路頻寬。

- [NC 系列](nc-series.md)、 [NCv2 系列](ncv2-series.md)、 [NCv3 系列](ncv3-series.md)大小已針對計算密集型和網路密集型應用程式和演算法進行優化。 其中一些範例包括 CUDA 和 OpenCL 架構的應用程式，以及模擬、AI 和深度學習。 NCv3 系列著重於高效能運算工作負載，採用 NVIDIA 的 Tesla V100 GPU。 NC 系列使用 Intel Haswell E5-2690 v3 2.60 GHz v3 （）處理器，而 NCv2 系列和 NCv3 系列 Vm 則使用 Intel 最強 E5-2690 v4 （Broadwell）處理器。

- [ND 系列](nd-series.md)和[NDv2 系列](ndv2-series.md)的大小著重于深度學習的訓練與推斷案例。 他們使用 NVIDIA Tesla P40 GPU 和 Intel 2690 E5-v4 （Broadwell）處理器。 NDv2 系列使用 Intel 8168 級的白金（Skylake）處理器。

- [NV 系列](nv-series.md)和[NVv3 系列](nvv3-series.md)大小已針對遠端視覺效果、串流、遊戲、編碼和 VDI 案例進行優化和設計，其使用 OpenGL 和 DirectX 這類架構。 這些 VM 是由 NVIDIA Tesla M60 GPU 提供支援。

- [NVv4 系列](nvv4-series.md)針對 VDI 和遠端視覺效果優化和設計的 VM 大小。 使用分割的 Gpu，NVv4 可為需要較小 GPU 資源的工作負載提供正確的大小。 這些 Vm 是由 AMD Radeon 直覺 MI25 GPU 支援。 NVv4 Vm 目前僅支援 Windows 客體作業系統。

## <a name="supported-operating-systems-and-drivers"></a>支援的作業系統和驅動程式

若要利用 Azure N 系列 Vm 的 GPU 功能，必須安裝 NVIDIA 或 AMD GPU 驅動程式。

- 針對 NVIDIA Gpu 支援的 Vm， [NVIDIA Gpu 驅動程式擴充](/azure/virtual-machines/extensions/hpccompute-gpu-windows)功能會安裝適當的 nvidia CUDA 或 GRID 驅動程式。 使用 Azure 入口網站或者 Azure PowerShell 或 Azure Resource Manager 範本之類的工具，安裝或管理擴充功能。 如需支援的作業系統和部署步驟，請參閱 [NVIDIA GPU 驅動程式擴充功能文件](/azure/virtual-machines/extensions/hpccompute-gpu-windows)。 如需有關虛擬機器擴充功能的一般資訊，請參閱 [Azure 虛擬機器擴充功能和功能](/azure/virtual-machines/extensions/overview)。

   或者，您可以手動安裝 NVIDIA GPU 驅動程式。 如需支援的作業系統、驅動程式、安裝和驗證步驟，請參閱[在執行 Windows 的 n 系列 vm 上安裝 NVIDIA gpu 驅動程式](/azure/virtual-machines/windows/n-series-driver-setup)或[在執行 Linux 的 n 系列 VM 上安裝 nvidia gpu 驅動程式](/azure/virtual-machines/linux/n-series-driver-setup)。

- 針對 AMD Gpu 支援的 Vm，請參閱[在執行 Windows 的 N 系列 vm 上安裝 AMD gpu 驅動程式](/azure/virtual-machines/windows/n-series-amd-driver-setup)，以取得支援的作業系統、驅動程式、安裝和驗證步驟。

## <a name="deployment-considerations"></a>部署考量因素

- 如需了解 N 系列 VM 的可用性，請參閱[依區域提供的產品](https://azure.microsoft.com/regions/services/)。

- N 系列 VM 只能部署在 Resource Manager 部署模型。

- N 系列 VM 對其磁碟支援的 Azure 儲存體類型有所不同。 NC 和 NV VM 只支援標準磁碟儲存體 (HDD) 所支持的 VM 磁碟。 NCv2、NCv3、ND、NDv2 和 NVv2 只支援進階磁碟儲存體 (SSD) 所支持的 VM 磁碟。

- 如果您想要部署不只一些 N 系列 VM ，請考慮隨用隨付訂用帳戶或其他購買選項。 如果您使用 [Azure 免費帳戶](https://azure.microsoft.com/free/)，您只能使用有限數目的 Azure 計算核心。

- 您可能需要增加 Azure 訂用帳戶的核心配額 (依地區)，以及增加 NC、NCv2、NCv3、ND、NDv2、NV 或 NVv2 核心的個別配額。 若要要求增加配額，可免費[開啟線上客戶支援要求](../azure-portal/supportability/how-to-create-azure-support-request.md)。 預設限制會視您的訂用帳戶類別而有所不同。

## <a name="other-sizes"></a>其他大小

- [一般用途](sizes-general.md)
- [計算最佳化](sizes-compute.md)
- [高效能計算](sizes-hpc.md)
- [記憶體最佳化](sizes-memory.md)
- [儲存體最佳化](sizes-storage.md)
- [上一代](sizes-previous-gen.md)

## <a name="next-steps"></a>後續步驟

深入了解 [Azure 計算單位 (ACU)](acu.md) 如何協助您比較各個 Azure SKU 的計算效能。
