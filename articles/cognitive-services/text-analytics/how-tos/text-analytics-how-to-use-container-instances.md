---
title: 執行 Azure 容器實例-文字分析
titleSuffix: Azure Cognitive Services
description: 將文字分析容器部署至 Azure 容器實例，並在網頁瀏覽器中進行測試。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e4b61c6fe2f62745d0f5268221cbb5c84803eb10
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876395"
---
# <a name="deploy-a-text-analytics-container-to-azure-container-instances"></a>將文字分析容器部署至 Azure 容器實例

瞭解如何將認知服務[文字分析][install-and-run-containers]容器部署至 Azure[容器實例][container-instances]。 此程式會」舉例說明建立文字分析資源、建立相關聯的情感分析映射，以及從瀏覽器執行這兩項協調流程的能力。 使用容器可以將開發人員的注意力轉移到管理基礎結構之外，改為專注于應用程式開發。

## <a name="prerequisites"></a>先決條件

* 使用 Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/)。

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

#### <a name="key-phrase-extraction"></a>[關鍵片語擷取](#tab/keyphrase)

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detection"></a>[語言偵測](#tab/language)

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysis"></a>[情感分析](#tab/sentiment)

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>後續步驟 

* 使用更多[認知服務容器](../../cognitive-services-container-support.md)
* 使用[文字分析聯機服務](../vs-text-connected-service.md)

[install-and-run-containers]: ./text-analytics-how-to-install-containers.md
[container-instances]: https://docs.microsoft.com/azure/container-instances