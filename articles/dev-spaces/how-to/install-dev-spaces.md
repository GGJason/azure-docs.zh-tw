---
title: 在 AKS 上啟用 Azure Dev Spaces & 安裝用戶端工具
services: azure-dev-spaces
ms.date: 07/24/2019
ms.topic: conceptual
description: 瞭解如何在 AKS 叢集上啟用 Azure Dev Spaces 並安裝用戶端工具。
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, 容器, Helm, 服務網格, 服務網格路由傳送, kubectl, k8s
ms.openlocfilehash: a6b3be5ceba5e60b99b2f75e060f3321cd3151f2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "78898957"
---
# <a name="enable-azure-dev-spaces-on-an-aks-cluster-and-install-the-client-side-tools"></a>在 AKS 叢集上啟用 Azure Dev Spaces 並安裝用戶端工具

本文說明在 AKS 叢集上啟用 Azure Dev Spaces 以及安裝用戶端工具的數種方式。

## <a name="enable-or-remove-azure-dev-spaces-using-the-cli"></a>使用 CLI 啟用或移除 Azure Dev Spaces

在您可以使用 CLI 啟用 Dev Spaces 之前，您需要：
* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，您可以建立[免費帳戶][az-portal-create-account]。
* [已安裝 Azure CLI][install-cli]。
* [受支援區域][supported-regions]中的[AKS][create-aks-cli]叢集。

使用 `use-dev-spaces` 命令在 AKS 叢集上啟用 Dev Spaces，並遵循提示來進行。

```azurecli
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster
```

上述命令會在*myResourceGroup*群組中的*myAKSCluster*叢集上啟用 Dev Spaces，並建立*預設*的開發人員空間。

```console
'An Azure Dev Spaces Controller' will be created that targets resource 'myAKSCluster' in resource group 'myResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'myAKSCluster' in resource group 'myResourceGroup' that targets resource 'myAKSCluster' in resource group 'myResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'myAKSCluster' in resource group 'myResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

此`use-dev-spaces`命令也會安裝 Azure Dev Spaces CLI。

若要從您的 AKS 叢集中移除 Azure Dev Spaces `azds remove` ，請使用命令。 例如：

```azurecli
$ azds remove -g MyResourceGroup -n MyAKS
Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup' will be deleted. This will remove Azure Dev Spaces instrumentation from the target resource for new workloads. Continue? (y/N): y

Deleting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAks' in resource group 'MyResourceGroup' (takes a few minutes)...
```

上述命令會從*MyResourceGroup*中的*MyAKS*叢集中移除 Azure Dev Spaces。 您使用 Azure Dev Spaces 建立的所有命名空間都會保留在其工作負載中，但這些命名空間中的新工作負載將不會使用 Azure Dev Spaces 進行檢測。 此外，如果您重新開機任何以 Azure Dev Spaces 檢測的現有 pod，您可能會看到錯誤。 這些 pod 必須重新部署，而不需要 Azure Dev Spaces 工具。 若要從您的叢集中完全移除 Azure Dev Spaces，請刪除已啟用 Azure Dev Spaces 的所有命名空間中的所有 pod。

## <a name="enable-or-remove-azure-dev-spaces-using-the-azure-portal"></a>使用 Azure 入口網站啟用或移除 Azure Dev Spaces

在您可以使用 Azure 入口網站啟用 Dev Spaces 之前，您需要：
* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，您可以建立[免費帳戶][az-portal-create-account]。
* [受支援區域][supported-regions]中的[AKS][create-aks-portal]叢集。

若要使用 Azure 入口網站啟用 Azure Dev Spaces：
1. 登入 [Azure 入口網站][az-portal]。
1. 流覽至您的 AKS 叢集。
1. 選取 [ *Dev Spaces* ] 功能表項目。
1. 將 [啟用 Dev Spaces]** 變更為 [是]**，然後按一下 [儲存]**。

![在 Azure 入口網站中啟用 Dev Spaces](../media/how-to-setup-dev-spaces/enable-dev-spaces-portal.png)

使用 Azure 入口網站啟用 Azure Dev Spaces，**並不會**安裝任何 Azure Dev Spaces 的用戶端工具。

若要從 AKS 叢集移除 Azure Dev Spaces，請將 [*啟用 Dev Spaces* ] 變更為 [*否*]，然後按一下 [*儲存*]。 您使用 Azure Dev Spaces 建立的所有命名空間都會保留在其工作負載中，但這些命名空間中的新工作負載將不會使用 Azure Dev Spaces 進行檢測。 此外，如果您重新開機任何以 Azure Dev Spaces 檢測的現有 pod，您可能會看到錯誤。 這些 pod 必須重新部署，而不需要 Azure Dev Spaces 工具。 若要從您的叢集中完全移除 Azure Dev Spaces，請刪除已啟用 Azure Dev Spaces 的所有命名空間中的所有 pod。

## <a name="install-the-client-side-tools"></a>安裝用戶端工具

您可以使用 Azure Dev Spaces 的用戶端工具，從您的本機電腦與 AKS 叢集上的開發人員空間進行互動。 有幾種方式可以安裝用戶端工具：

* 在[Visual Studio Code][vscode]中，安裝[Azure Dev Spaces 延伸][vscode-extension]模組。
* 在[Visual Studio 2019][visual-studio]中，安裝 Azure 開發工作負載。
* 在 Visual Studio 2017 中，安裝「Web 開發」工作負載和[適用於 Kubernetes 的 Visual Studio Tools][visual-studio-k8s-tools]。
* 下載並安裝[Windows][cli-win]、 [Mac][cli-mac]或[Linux][cli-linux] CLI。

## <a name="next-steps"></a>後續步驟

了解 Azure Dev Spaces 如何協助您跨多個容器開發更複雜的應用程式，以及如何藉由在不同的空間中使用不同的程式碼版本或分支，來簡化共同開發。

> [!div class="nextstepaction"]
> [在 Azure Dev Spaces 中進行小組開發][team-development-qs]

[create-aks-cli]: ../../aks/kubernetes-walkthrough.md#create-a-resource-group
[create-aks-portal]: ../../aks/kubernetes-walkthrough-portal.md#create-an-aks-cluster
[install-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-development-qs]: ../quickstart-team-development.md

[az-portal]: https://portal.azure.com
[az-portal-create-account]: https://azure.microsoft.com/free
[cli-linux]: https://aka.ms/get-azds-linux
[cli-mac]: https://aka.ms/get-azds-mac
[cli-win]: https://aka.ms/get-azds-windows
[visual-studio]: https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs
[visual-studio-k8s-tools]: https://aka.ms/get-vsk8stools
[vscode]: https://code.visualstudio.com/download
[vscode-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
