---
title: 使用 explorer Azure 時間序列深入解析探索資料 |Microsoft Docs
description: 瞭解如何使用 Azure 時間序列深入解析 explorer 來查看您的 IoT 資料。
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/14/2020
ms.custom: seodec18
ms.openlocfilehash: b564373780da5bba71bc46ddbac3dc69a0c94a4a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81382787"
---
# <a name="azure-time-series-insights-explorer"></a>Azure 時間序列深入解析總管

本文說明 Azure 時間序列深入解析[explorer web 應用程式](https://insights.timeseries.azure.com/)正式運作的功能和選項。 時間序列深入解析 explorer 會示範服務所提供的功能強大的資料視覺化功能，並可在您自己的環境中進行存取。

Azure 時間序列深入解析是完全受控的分析、儲存體及視覺效果服務，讓您可輕易同時探索及分析數十億筆的 IoT 事件。 它可為您提供資料的全域檢視，從而讓您快速驗證 IoT 解決方案，並避免關鍵任務裝置成本高昂的停機時間。 您可以幾近即時地探索隱藏趨勢、找出異常狀況，並進行根本原因分析。 時間序列深入解析總管目前處於公開預覽狀態。

> [!TIP]
> 如需示範環境的引導式導覽，請閱讀[Azure 時間序列深入解析快速入門](time-series-quickstart.md)。

## <a name="video"></a>影片

### <a name="learn-about-querying-data-by-using-the-time-series-insights-explorer-br"></a>瞭解如何使用時間序列深入解析 explorer 來查詢資料。 </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>播放先前的影片「<a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">使用 Azure IoT 解決方案加速器開始使用時間序列深入解析」。</a>

## <a name="prerequisites"></a>先決條件

在使用時間序列深入解析總管之前，您必須先：

- 建立時間序列深入解析環境。 如需詳細資訊，請參閱[如何開始使用時間序列深入解析](./time-series-insights-get-started.md)。
- 在環境中提供您帳戶的[存取權](time-series-insights-data-access.md)。
- 將[IoT 中樞](time-series-insights-how-to-add-an-event-source-iothub.md)或[事件中樞](time-series-insights-how-to-add-an-event-source-eventhub.md)的事件來源新增至其中。

## <a name="explore-and-query-data"></a>探索及查詢資料

在將事件來源連線至時間序列深入解析環境的幾分鐘內，您可以探索並查詢時間序列資料。

1. 若要開始，請在網頁瀏覽器中開啟 [[時間序列深入解析 explorer](https://insights.timeseries.azure.com/) ]。 在視窗左側，選取 [環境]。 您可以存取的所有環境都會依字母順序列出。

1. 選取環境之後，請使用頂端的 [**從**] 和 [**到**] 設定，或選取並拖曳您想要的 timespan。 選取右上角的放大鏡，或以滑鼠右鍵按一下選取的 timespan，然後選取 [**搜尋**]。

1. 您也可以選取 [**自動開啟**] 按鈕，每分鐘自動重新整理可用性。 [**自動開啟**] 按鈕僅適用于 [可用性] 圖表，而不是主要視覺效果的內容。

1. Azure 雲端圖示會帶您前往 Azure 入口網站中的環境。

   [![時間序列深入解析環境選擇](media/time-series-insights-explorer/tsi-ga-explorer-environments.png)](media/time-series-insights-explorer/tsi-ga-explorer-environments.png#lightbox)

1. 接下來，會顯示在選取的 timespan 期間顯示所有事件計數的圖表。 這裡您有許多控制項：

    - **詞彙編輯器面板**：字詞空間是您查詢環境的地方。 它位於畫面的左側：
      - **量值**：這個下拉式清單會顯示所有數值資料行（**雙精度**浮點數）。
      - **分割依據**：此下拉式清單會顯示類別資料行（**字串**）。
      - 您可以啟用步驟插補、顯示最小和最大值，以及調整 [**測量**] 旁 [控制台] 中的 y 軸。 您也可以調整顯示的資料是資料的計數、平均值或總和。
      - 您最多可以在同一個 X 軸上加入五個詞彙。 選取 [**新增**] 以新增新的字詞，或使用 [**複製此詞彙**] 按鈕來加入現有詞彙的複本。

        [![詞彙選取、篩選和查詢面板](media/time-series-insights-explorer/tsi-ga-explorer-add-or-clone.png)](media/time-series-insights-explorer/tsi-ga-explorer-add-or-clone.png#lightbox)

      - 述**詞：使用**述詞，使用下表所列的一組運算元來快速篩選事件。 如果您選取或按一下來進行搜尋，述詞會根據該搜尋自動進行更新。 支援的運算元類型包括：

         |作業  |支援的類型  |備忘錄  |
         |---------|---------|---------|
         |**<**, **>**, **<=**, **>=**    |  **Double**、 **DateTime**、 **TimeSpan**       |         |
         |**=**, **!=**,**<>**     | **String**、 **Bool**、 **Double**、 **DateTime**、 **TimeSpan**、 **Null**        |         |
         |**在**     | **String**、 **Bool**、 **Double**、 **DateTime**、 **TimeSpan**、 **Null**        |  所有運算元都應該是相同的類型，或為**Null**常數。        |
         |**HAS**     | **字串**        |  右側只允許常數位串常值。 不允許空字串和**Null** 。       |

      - **查詢範例**

         [![範例 GA 查詢](media/time-series-insights-explorer/tsi-ga-example-queries.png)](media/time-series-insights-explorer/tsi-ga-example-queries.png#lightbox)

1. 您可以使用 [**間隔大小**] 滑杆工具來放大和縮小相同 timespan 的間隔。 滑杆可讓您更精確地控制在大型時間配量之間的移動，以將平滑的趨勢向下顯示為與毫秒小的配量，讓您能夠顯示及分析資料的細微、高解析度的剪切。 滑杆的預設起始點會設定為從您的選取範圍最理想的觀點，以平衡解析度、查詢速度和資料細微性。

1. **時間筆刷**工具可讓您輕鬆地從一個 timespan 流覽至另一個時間範圍。

1. 選取**儲存**圖示以儲存您目前的查詢，並與環境的其他使用者共用。 當您選取 [**開啟**] 圖示時，可以在您可以存取的環境中，檢查所有已儲存的查詢和其他使用者的任何共用查詢。

   [![查詢](media/time-series-insights-explorer/tsi-ga-explorer-saved-queries.png)](media/time-series-insights-explorer/tsi-ga-explorer-saved-queries.png#lightbox)

## <a name="visualize-data"></a>顯現資料

1. 使用 [**透視視圖**] 工具，即可同時查看最多四個唯一的查詢。 [**透視視圖**] 按鈕位於圖表的右上角。

   [![選取要加入至 [透視] 窗格的查詢](media/time-series-insights-explorer/tsi-ga-explorer-perspective-panes.png)](media/time-series-insights-explorer/tsi-ga-explorer-perspective-panes.png#lightbox)

1. 觀看圖表以視覺化方式流覽您的資料，並使用**圖表**工具：

    - **選取**或**按一下**特定的 timespan 或單一資料數列。
    - 在 timespan 選取範圍內，您可以縮放或流覽事件。
    - 在資料序列內，您可以由另一個資料行分割序列、將序列新增為新的字詞、只顯示選取的序列、排除選取的序列、釘選該序列或從選取的序列探索事件。
    - 在圖表左邊的 [篩選] 區域中，您可以查看所有顯示的資料序列，並依值或名稱重新排序。 您也可以查看所有資料數列或任何已釘選或取消固定的數列。 您可以選取單一資料序列，並依另一個資料行分割序列、將數列新增為新的字詞、只顯示選取的數列、排除選取的序列、釘選該數列，或從選取的數列探索事件。
    - 當您同時查看多個詞彙時，可以堆疊、取消、查看有關資料數列的其他資料，並在所有詞彙上使用相同的 y 軸。 使用圖表右上角的按鈕。

    [![圖表工具右上角選項設定](media/time-series-insights-explorer/tsi-ga-example-chart-options.png)](media/time-series-insights-explorer/tsi-ga-example-chart-options.png#lightbox)

1. 使用**熱度圖**，快速找出給定查詢中的唯一或異常資料數列。 只有一個搜尋字詞可視覺化為熱度圖。

    [![GA explorer 熱度圖圖表](media/time-series-insights-explorer/tsi-ga-example-heatmap-charting.png)](media/time-series-insights-explorer/tsi-ga-example-heatmap-charting.png#lightbox)

1. 當您選取或以滑鼠右鍵按一下來流覽事件時，[**事件**] 面板就會成為可用。 在這裡，您可以查看您所有的原始事件，並將事件匯出為 JSON 或 CSV 檔案。 時間序列深入解析儲存所有原始資料。

    [![事件](media/time-series-insights-explorer/tsi-ga-explorer-events-panel.png)](media/time-series-insights-explorer/tsi-ga-explorer-events-panel.png#lightbox)

1. 探索事件之後，請選取 [**統計**資料] 索引標籤，以公開模式和資料行統計資料。

    - **模式**：這項功能會主動呈現所選資料區域中最具統計的重大模式。 您不需要查看數以千計的事件，即可瞭解哪些模式需要最多的時間和能源。 有了時間序列深入解析，您就可以直接跳到這些統計重要的模式，繼續進行分析。 這項功能對於事後調查歷史資料也很有幫助。
    - 資料**行統計**資料：資料行統計資料提供的圖表和資料表，會在選取的時間範圍內，細分所選資料數列中每個資料行的資料。

      [![統計資料行圖表和選項](media/time-series-insights-explorer/tsi-ga-explorer-stat-column.png)](media/time-series-insights-explorer/tsi-ga-explorer-stat-column.png#lightbox)

現在您已瞭解時間序列深入解析 explorer web 應用程式中可用的主要功能、設定和顯示選項。

## <a name="next-steps"></a>後續步驟

- 瞭解如何在您的時間序列深入解析環境中[診斷並解決問題](time-series-insights-diagnose-and-solve-problems.md)。

- 進行引導式[Azure 時間序列深入解析快速入門](time-series-quickstart.md)導覽。
