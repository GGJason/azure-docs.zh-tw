---
title: 在 Azure Kubernetes Service 中使用系統節點集區（AKS）
description: 瞭解如何在 Azure Kubernetes Service （AKS）中建立和管理系統節點集區
services: container-service
ms.topic: article
ms.date: 04/28/2020
ms.openlocfilehash: 85cc699d6ef8c632663775e91f2b5cad6ca7a7b6
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83125242"
---
# <a name="manage-system-node-pools-in-azure-kubernetes-service-aks"></a>管理 Azure Kubernetes Service 中的系統節點集區（AKS）

在 Azure Kubernetes Service （AKS）中，相同設定的節點會群組在一起成為*節點*集區。 節點集區包含執行應用程式的基礎 Vm。 針對您的 AKS 叢集，系統節點集區和使用者節點集區是兩個不同的節點集區模式。 系統節點集區提供裝載重要系統 pod （例如 CoreDNS 和 tunnelfront）的主要目的。 使用者節點集區提供裝載應用程式 pod 的主要目的。 不過，如果您想要在 AKS 叢集中只有一個集區，可以在系統節點集區上排程應用程式 pod。 每個 AKS 叢集至少必須包含一個具有至少一個節點的系統節點集區。 

> [!Important]
> 如果您在生產環境中針對 AKS 叢集執行單一系統節點集區，建議您針對節點集區使用至少三個節點。

## <a name="before-you-begin"></a>開始之前

* 您需要安裝並設定 Azure CLI 2.3.1 版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI][install-azure-cli]。

## <a name="limitations"></a>限制

當您建立和管理支援系統節點集區的 AKS 叢集時，適用下列限制。

* 請參閱[配額、虛擬機器大小限制，以及 Azure Kubernetes Service 中的區域可用性（AKS）][quotas-skus-regions]。
* AKS 叢集必須以虛擬機器擴展集作為 VM 類型來建立。
* 節點集區的名稱只可包含小寫英數位元，且必須以小寫字母開頭。 針對 Linux 節點集區，長度必須介於1到12個字元之間。 對於 Windows 節點集區，長度必須介於1到6個字元之間。
* 必須使用2020-03-01 或更高的 API 版本來設定節點集區模式。
* 節點集區的模式是必要屬性，而且必須在使用 ARM 範本或直接 API 呼叫時明確設定。

## <a name="system-and-user-node-pools"></a>系統和使用者節點集區

系統節點集區節點各有標籤**kubernetes.azure.com/mode： system**。 每個 AKS 叢集都包含至少一個系統節點集區。 系統節點集區具有下列限制：

* 系統集區 osType 必須是 Linux。
* 使用者節點集區 osType 可能是 Linux 或 Windows。
* 系統集區必須包含至少一個節點，且使用者節點集區可能包含零個或多個節點。
* 系統節點集區需要至少2個個 vcpu 和 4 GB 記憶體的 VM SKU。
* 系統節點集區必須支援至少30個 pod，如 pod 的[最小和最大值公式][maximum-pods]所述。
* 點節點集區需要使用者節點集區。

您可以使用節點集區執行下列作業：

* 將系統節點集區變更為使用者節點集區，前提是您有另一個系統節點集區，以便在 AKS 叢集中使用它。
* 將使用者節點集區變更為系統節點集區。
* 刪除使用者節點集區。
* 您可以刪除系統節點集區，但前提是您有另一個系統節點集區，以將其放在 AKS 叢集中。
* AKS 叢集可能會有多個系統節點集區，而且至少需要一個系統節點集區。
* 如果您想要變更現有節點集區上的各種不可變設定，可以建立新的節點集區來取代它們。 其中一個範例是使用新的 maxPods 設定來新增節點集區，並刪除舊的節點集區。

## <a name="create-a-new-aks-cluster-with-a-system-node-pool"></a>建立具有系統節點集區的新 AKS 叢集

當您建立新的 AKS 叢集時，您會自動建立具有單一節點的系統節點集區。 初始節點集區預設為「系統」類型的模式。 當您使用 az aks nodepool add 建立新的節點集區時，這些節點集區是使用者節點集區，除非您明確指定 mode 參數。

下列範例會在*eastus*區域中建立名為*myResourceGroup*的資源群組。

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

使用 [az aks create][az-aks-create] 命令來建立 AKS 叢集。 下列範例會建立名為*myAKSCluster*的叢集，其中一個系統集區包含一個節點。 針對生產工作負載，請確定您使用的系統節點集區至少有三個節點。 此作業可能需要幾分鐘的時間才能完成。

```azurecli-interactive
az aks create -g myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

## <a name="add-a-system-node-pool-to-an-existing-aks-cluster"></a>將系統節點集區新增至現有的 AKS 叢集中

您可以將一或多個系統節點集區新增至現有的 AKS 叢集。 下列命令會新增模式類型系統的節點集區，其預設計數為三個節點。

```azurecli-interactive
az aks nodepool add -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```
## <a name="show-details-for-your-node-pool"></a>顯示節點集區的詳細資料

您可以使用下列命令來檢查節點集區的詳細資料。  

```azurecli-interactive
az aks nodepool show -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

系統會為系統節點集區定義一種類型為**system**的模式，並為使用者節點集區定義**使用者**類型的模式。

```output
{
  "agentPoolType": "VirtualMachineScaleSets",
  "availabilityZones": null,
  "count": 3,
  "enableAutoScaling": null,
  "enableNodePublicIp": false,
  "id": "/subscriptions/666d66d8-1e43-4136-be25-f25bb5de5883/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/agentPools/mynodepool",
  "maxCount": null,
  "maxPods": 110,
  "minCount": null,
  "mode": "System",
  "name": "mynodepool",
  "nodeLabels": {},
  "nodeTaints": null,
  "orchestratorVersion": "1.15.10",
  "osDiskSizeGb": 100,
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "scaleSetEvictionPolicy": null,
  "scaleSetPriority": null,
  "spotMaxPrice": null,
  "tags": null,
  "type": "Microsoft.ContainerService/managedClusters/agentPools",
  "vmSize": "Standard_DS2_v2",
  "vnetSubnetId": null
}
```

## <a name="update-system-and-user-node-pools"></a>更新系統和使用者節點集區

您可以變更系統和使用者節點集區的模式。 只有當 AKS 叢集上已有另一個系統節點集區時，您才可以將系統節點集區變更為使用者集區。

此命令會將系統節點集區變更為使用者節點集區。

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode user
```

此命令會將使用者節點集區變更為系統節點集區。

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```

## <a name="delete-a-system-node-pool"></a>刪除系統節點集區

> [!Note]
> 若要在 API 版本2020-03-02 之前，在 AKS 叢集上使用系統節點集區，請新增系統節點集區，然後刪除原始預設節點集區。

先前您無法刪除系統節點集區，這是 AKS 叢集中的初始預設節點集區。 您現在可以彈性地從叢集刪除任何節點集區。 由於 AKS 叢集需要至少一個系統節點集區，因此您的 AKS 叢集上必須至少有兩個系統節點集區，才能刪除其中一個。

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

## <a name="next-steps"></a>後續步驟

在本文中，您已瞭解如何在 AKS 叢集中建立和管理系統節點集區。 如需有關如何使用多個節點集區的詳細資訊，請參閱[使用多個節點][use-multiple-node-pools]集區。

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubernetes-labels]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
[kubernetes-label-syntax]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set

<!-- INTERNAL LINKS -->
[aks-windows]: windows-container-cli.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-list
[az-aks-nodepool-update]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-update
[az-aks-nodepool-upgrade]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-delete
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[gpu-cluster]: gpu-cluster.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[quotas-skus-regions]: quotas-skus-regions.md
[supported-versions]: supported-kubernetes-versions.md
[tag-limitation]: ../azure-resource-manager/resource-group-using-tags.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[vm-sizes]: ../virtual-machines/linux/sizes.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[maximum-pods]: configure-azure-cni.md#maximum-pods-per-node