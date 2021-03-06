---
author: robinsh
manager: philmea
ms.author: robinsh
ms.topic: include
ms.date: 05/20/2019
ms.openlocfilehash: c164433efc6a34a3a06676a3145feb18d3de80b9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "66248933"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>將取用者群組新增至 IoT 中樞

取用[者群組](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers)會將獨立的視圖提供給事件資料流程，讓應用程式和 Azure 服務獨立取用來自相同事件中樞端點的資料。 在本節中，您會將取用者群組新增至 IoT 中樞的內建端點，稍後將在本教學課程中用來從端點提取資料。

若要將取用者群組新增至 IoT 中樞，請遵循下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)中，開啟 IoT 中樞。

2. 在左窗格中，選取 [**內建端點**]，選取右窗格中的 [**事件**]，然後在 [取用**者群組**] 底下輸入名稱。 選取 [儲存]  。

   ![在 IoT 中樞中建立取用者群組](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
