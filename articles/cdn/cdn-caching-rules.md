---
title: 使用快取規則來控制 Azure CDN 快取行為 | Microsoft Docs
description: 您可以使用 CDN 快取規則，以全域及有條件 (例如 URL 路徑和副檔名) 的方式設定或修改預設快取到期行為。
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: allensu
ms.openlocfilehash: 874ec75fb9173b6cee50bf8880510464fa13e9d2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81254235"
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>使用快取規則來控制 Azure CDN 快取行為

> [!NOTE] 
> 快取規則僅適用於「**來自 Verizon 的 Azure CDN 標準**」和「**來自 Akamai 的 Azure CDN 標準**」的設定檔。 針對**來自 Microsoft 的 Azure CDN** 設定檔，您必須使用 [標準規則引擎](cdn-standard-rules-engine-reference.md)。針對**來自 Verizon 的 Azure CDN 進階**設定檔，您必須使用 [管理]**** 入口網站中的 [Verizon 進階規則引擎](cdn-rules-engine.md)來執行類似功能。
 
「Azure 內容傳遞網路」(CDN) 提供兩種方式來控制如何快取檔案： 

- 快取規則：本文說明如何使用內容傳遞網路 (CDN) 快取規則，以全域及有自訂條件 (例如 URL 路徑和副檔名) 的方式設定或修改預設快取到期行為。 Azure CDN 提供兩種類型的快取規則：

   - 全域快取規則：您可以針對設定檔中的每個端點設定一個全域快取規則，這會影響針對端點的所有要求。 全域快取規則會覆寫任何 HTTP 快取指示詞標頭 (如果已設定)。

   - 自訂快取規則：您可以針對設定檔中的每個端點設定一或多個自訂快取規則。 自訂快取規則會比對特定路徑和副檔名、會依序處理，並會覆寫全域快取規則 (如果已設定)。 

- 查詢字串快取：您可以調整 Azure CDN 使用查詢字串處理要求快取的方式。 如需相關資訊，請參閱[使用查詢字串控制 Azure CDN 快取行為](cdn-query-string.md)。 如果檔案無法快取，根據快取規則和 CDN 預設行為，查詢字串快取設定不會有任何效果。

如需預設快取行為和快取指示詞標頭的相關資訊，請參閱[快取如何運作](cdn-how-caching-works.md)。 


## <a name="accessing-azure-cdn-caching-rules"></a>存取 Azure CDN 快取規則

1. 開啟 Azure 入口網站，選取 CDN 設定檔，然後選取端點。

2. 在左窗格的 [設定] 下方，選取 [快取規則]****。

   ![CDN [快取規則] 按鈕](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   [快取規則]**** 頁面隨即出現。

   ![CDN 快取規則頁面](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>快取行為設定
針對全域和自訂快取規則，您可以指定下列**快取行為**設定：

- **略過快取**：不要快取並忽略原始提供的快取指示詞標頭。

- 覆**寫**：忽略原始提供的快取持續時間;請改用提供的快取持續時間。 這不會覆寫快取控制：無快取。

- **缺少時才設定**：如果原始提供的快取指示詞標頭存在，就優先採用它們；否則，使用提供的快取持續時間。

![全域快取規則](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![自訂快取規則](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>快取到期期間
針對全域和自訂快取規則，您能以天數、小時數、分鐘數和秒數來指定快取到期期間：

- 針對 [覆寫]**** 和 [缺少時才設定]** 的** **快取行為**設定，有效快取持續時間的範圍介於 0 秒到 366 天。 若值為 0 秒，CDN 會快取內容，但必須向原始伺服器重新驗證每個要求。

- 針對 [略過快取]**** 設定，快取持續時間會自動設為 0 秒且無法變更。

## <a name="custom-caching-rules-match-conditions"></a>自訂快取規則比對條件

針對自訂快取規則，有兩個可用的比對條件：
 
- **路徑**：這個條件會比對 URL 的路徑，排除網域名稱，並支援萬用字元符號 (\*)。 例如，_/myfile.html_、_/my/folder/*_ 和 _/my/images/*.jpg_。 長度上限是 260 個字元。

- **副檔名**：這個條件會比對所要求檔案的副檔名。 您可以提供一份要比對的副檔名清單 (以逗號分隔)。 例如，_.jpg_、_.mp3_ 或 _.png_。 副檔名的個數上限是 50，且每個副檔名的最大字元數目為 16。 

## <a name="global-and-custom-rule-processing-order"></a>全域和自訂規則處理順序
全域和自訂快取規則會依下列順序處理：

- 全域快取規則的優先順序高於預設的 CDN 快取行為 (HTTP 快取指示詞標頭設定)。 

- 自訂快取規則的優先順序高於全域快取規則 (在其適用處)。 自訂快取規則的處理順序為從上而下。 也就是說，如果要求符合這兩個條件，位於清單底部之規則的優先順序會高於位於清單頂端的規則。 因此，您應該將更特定的規則置於清單的較低處。

**範例**：
- 全域快取規則： 
   - 快取行為：**覆寫**
   - 快取到期期間：1 天

- 自訂快取規則 #1：
   - 比對條件：**路徑**
   - 比對值：_/home/*_
   - 快取行為：**覆寫**
   - 快取到期期間：2 天

- 自訂快取規則 #2：
   - 比對條件：**副檔名**
   - 比對值：_.html_
   - 快取行為：**缺少時才設定**
   - 快取到期期間：3 天

設定這些規則時， _ &lt;端點主機名稱&gt;_. azureedge.net/home/index.html 的要求會觸發自訂快取規則 #2，這會設定為：**如果遺漏則設定**為3天。 因此，如果 index.html** 檔案具有 `Cache-Control` 或 `Expires` HTTP 標頭，則會優先採用它們，否則，如果未設定這些標頭，就會將檔案快取 3 天。

> [!NOTE] 
> 在規則變更之前快取的檔案會維持其原始的快取持續時間設定。 若要重設它們的快取持續時間，您必須[清除檔案](cdn-purge-endpoint.md)。 
>
> Azure CDN 組態變更會需要一些時間才能傳播至整個網路： 
> - 若為**來自 Akamai 的標準 Azure CDN** 設定檔，通常會在一分鐘內完成傳播。 
> - 若為**來自 Verizon 的標準 Azure CDN** 設定檔，則通常會在 10 分鐘內完成傳播。  
>

## <a name="see-also"></a>請參閱

- [快取的運作方式](cdn-how-caching-works.md)
- [教學課程：設定 Azure CDN 快取規則](cdn-caching-rules-tutorial.md)
