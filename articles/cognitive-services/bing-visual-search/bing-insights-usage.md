---
title: Bing 深入解析的範例 - Bing 圖像式搜尋
titleSuffix: Azure Cognitive Services
description: 本文包含的範例會示範 Bing 圖像式搜尋如何在 Bing.com 上使用和顯示影像深入解析。
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: scottwhi
ms.openlocfilehash: df66dbeebb04209921ff91c4b99a14580f026718
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "74111647"
---
# <a name="examples-of-bing-insights-usage"></a>Bing 深入解析使用方式的範例

本文包含 Bing 可如何在 Bing.com 上使用和顯示影像深入解析的範例。

## <a name="pagesincluding-insight-example"></a>PagesIncluding 深入解析範例

以下顯示第一個網頁的連結，並可讓使用者展開和折迭包含影像的其他網頁清單：

![展開的頁面包括](./media/pages-including.PNG)

## <a name="shoppingsources-insight-example"></a>ShoppingSources 深入解析範例

以下顯示 Bing 可能如何顯示影像中所示產品的購物來源：

![購物來源](./media/shopping-sources.PNG)

## <a name="visualsearch-insight-example"></a>VisualSearch 深入解析範例

以下顯示 Bing 如何顯示以視覺效果相似的影像（請參閱範例中的**相關影像**）：

![在視覺效果上類似的影像](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Recipes 深入解析範例

以下顯示 Bing 對於影像顯示的食物如何顯示食譜。 此範例可讓使用者知道有可用的配方：

![食譜和頁面包括](./media/recipes-pages-including.PNG)

 和會在使用者展開清單時提供食譜的連結：

![展開的食譜頁面包括](./media/expanded-recipes-pages-including.PNG)

## <a name="relatedsearches-insight-example"></a>RelatedSearches 深入解析範例

以下顯示 Bing 如何顯示其他人對於影像進行的相關搜尋。 如果使用者按一下影像，使用者會進入該相關查詢的 Bing.com/images 搜尋結果頁面。

![影像的相關搜尋](./media/bordered-related-searches.PNG)

## <a name="entity-insight-example"></a>Entity 深入解析範例

以下顯示 Bing 如何顯示影像顯示的實體 (人、地、事) 相關的資訊。 如果使用者按一下實體連結，使用者會進入實體的 [Bing.com 搜尋結果] 頁面：

![影像中顯示的實體](./media/entity.PNG)

## <a name="displaying-other-insights-that-the-user-might-explore"></a>顯示使用者可以探索的其他深入解析

以下顯示 Bing 如何顯示使用者可以探索的影像有關的其他資訊。

![探索影像的其他深入解析](./media/apple-pie-more-tags.PNG)

## <a name="bounding-boxes-and-hot-spots"></a>週框方塊和作用點

非預設標記包含識別標籤套用的影像之中關切區域的週框方塊。 如果週框方塊並未識別整個影像，可使用週框方塊在影像上建立作用點。 使用者可以按一下作用點取得在作用點 (或矩形) 之下找到的內容有關的資訊。 例如，如果影像是高風格影像，則結果可能會包含影像中所顯示之配件的標籤（和周框方塊），例如錢包、珠寶、圍巾等等。 下列範例顯示影像中所顯示之太陽眼鏡的熱點矩形：

![週框方塊和作用點](./media/click-to-search.PNG)

## <a name="next-steps"></a>後續步驟

若要開始使用您的第一個要求，請參閱快速入門： [c #](quickstarts/csharp.md) | [JAVA](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | node.js[Python](quickstarts/python.md)





