---
title: Azure Sentinel 中的搜尋功能 |Microsoft Docs
description: 本文說明如何使用 Azure Sentinel 的搜尋功能。
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/10/2019
ms.author: yelevin
ms.openlocfilehash: 52af688917aa531d125f83844df29a988ed7cb7e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81686641"
---
# <a name="hunt-for-threats-with-azure-sentinel"></a>利用 Azure Sentinel 尋找威脅

如果您是想要主動找出安全性威脅的調查者，請 Azure Sentinel 強大的搜尋搜尋和查詢工具，來尋找整個組織資料來源的安全性威脅。 但是您的系統和安全性設備會產生山脈的資料，這可能很容易剖析並篩選成有意義的事件。 為了協助安全性分析師主動查看您的安全性應用程式未偵測到的新異常狀況，Azure Sentinel 的內建搜尋查詢會引導您詢問適當的問題，以找出您的網路上已有的資料問題。 

例如，其中一個內建查詢會提供在您的基礎結構上執行的最常見進程的相關資料-您不會想要在每次執行時都產生警示，它們可能完全無害，但您可能會想要查看是否有任何異常的查詢。 



有了 Azure Sentinel 搜尋，您就可以利用下列功能：

- 內建查詢：為了讓您開始使用，起始頁會提供預先載入的查詢範例，旨在協助您開始使用，並熟悉資料表和查詢語言。 這些內建的搜尋查詢是由 Microsoft 資訊安全研究人員連續開發，加入新的查詢，以及微調現有的查詢，以提供您尋找新偵測的進入點，並找出在何處開始搜尋新的攻擊。 

- IntelliSense 的強大查詢語言：建置於查詢語言之上，讓您有彈性地進入下一個層級。

- 建立您自己的書簽：在搜尋過程中，您可能會遇到看起來不尋常或可疑的相符專案或發現、儀表板或活動。 若要標記這些專案，讓您在未來可以返回它們，請使用書簽功能。 書簽可讓您儲存專案，以供稍後用來建立調查的事件。 如需書簽的詳細資訊，請參閱[在搜尋中使用書簽](hunting.md)。
- 使用筆記本自動進行調查：筆記本是您可以建立的逐步操作手冊，可讓您逐步完成調查和搜尋的步驟。  筆記本會將所有搜尋步驟封裝在可重複使用的腳本中，以與組織中的其他人共用。 
- 查詢已儲存的資料：您可以在資料表中存取資料，以供您查詢。 例如，您可以查詢進程建立、DNS 事件和許多其他事件種類。

- 社區連結：利用更大的社區功能來尋找額外的查詢和資料來源。
 
## <a name="get-started-hunting"></a>開始搜尋

1. 在 Azure Sentinel 入口網站中，按一下 [**搜尋**]。
  ![Azure Sentinel 開始搜尋](media/tutorial-hunting/hunting-start.png)

2. 當您開啟 [**搜尋**] 頁面時，所有的搜尋查詢都會顯示在單一資料表中。 此表格列出由 Microsoft 的安全性分析師和您所建立或修改的任何額外查詢所撰寫的所有查詢。 每個查詢都會提供其 digger 的描述，以及其執行的資料種類。 這些範本會依其各種策略分組-右邊的圖示會將威脅類型分類，例如初始存取、持續性和外泄。 您可以使用任何欄位來篩選這些搜尋查詢範本。 您可以將任何查詢儲存至我的最愛。 藉由將查詢儲存至 [我的最愛]，每次存取**搜尋**頁面時，查詢就會自動執行。 您可以建立自己的搜尋查詢，或複製和自訂現有的搜尋查詢範本。 
 
2. 按一下 [搜尋查詢詳細資料] 頁面中的 [**執行查詢**]，即可執行任何查詢，而不需要離開搜尋頁面。  相符專案的數目會顯示在資料表中。 檢查搜尋查詢的清單及其相符專案。 查看與相符相關聯之終止鏈中的哪個階段。

3. 在 [查詢詳細資料] 窗格中執行基礎查詢的快速審核，或按一下 [**查看查詢結果**]，在 Log Analytics 中開啟查詢。 在底部，檢查查詢的相符專案。

4.    按一下資料列並選取 [**新增書簽**]，以加入要調查的資料列-您可以針對任何看起來可疑的專案執行此動作。 

5. 然後，返回 [主要**搜尋**] 頁面，然後按一下 [**書簽**] 索引標籤，以查看所有可疑的活動。 

6. 選取書簽，然後按一下 [**調查**] 以開啟調查體驗。 您可以篩選書簽。 例如，如果您要調查活動，您可以為活動建立標記，然後根據活動篩選所有書簽。

1. 在探索到哪些搜尋查詢提供可能攻擊的高價值深入解析之後，您也可以根據您的查詢建立自訂偵測規則，並以警示的形式呈現這些深入解析，以作為安全性事件回應程式的警示。

 

## <a name="query-language"></a>查詢語言 

Azure Sentinel 中的搜尋是以 Kusto 查詢語言為基礎。 如需查詢語言和支援之運算子的詳細資訊，請參閱[查詢語言參考](/azure/azure-monitor/log-query/get-started-queries)。

## <a name="public-hunting-query-github-repository"></a>公開搜尋查詢 GitHub 存放庫

查看[搜尋查詢存放庫](https://github.com/Azure/Orion)。 參與並使用我們的客戶所共用的範例查詢。

 

## <a name="sample-query"></a>範例查詢

查詢通常以資料表名稱開頭，後面接著一連串以  隔開的「運算子」\|。

在上述範例中，請從 [資料表名稱] SecurityEvent 開始，並視需要新增管線元素。

1. 定義時間篩選器，只查看過去七天內的記錄。

2. 在查詢中新增篩選，只顯示事件識別碼4688。

3. 在命令列上的查詢中加入篩選，以便只包含 cscript.exe 的實例。

4. 只投影您想要探索的資料行，並將結果限制為1000，然後按一下 [**執行查詢**]。
5. 按一下綠色三角形，然後執行查詢。 您可以測試查詢並加以執行，以尋找異常行為。

## <a name="useful-operators"></a>實用運算子

查詢語言的功能強大，而且有許多可用的運算子，這裡列出一些有用的運算子：

**where** -篩選資料表，使其符合述詞的資料列子集。

**摘要**-產生資料表來匯總輸入資料表的內容。

**聯結**-合併兩個數據表的資料列，藉由比對每個資料表中指定之資料行的值，以形成新的資料表。

**count** -傳回輸入記錄集中的記錄數目。

**top** -傳回依指定資料行排序的前 N 筆記錄。

**limit** -傳回指定的資料列數目。

**專案**-選取要包含的資料行、重新命名或卸載，然後插入新的計算資料行。

**擴充**-建立匯出資料行，並將其附加至結果集。

**makeset** -傳回 Expr 在群組中的一組相異值的動態（JSON）陣列

**尋找**-在一組資料表中尋找符合述詞的資料列。

## <a name="save-a-query"></a>儲存查詢

您可以建立或修改查詢，並將它儲存為您自己的查詢，或與相同租使用者中的使用者共用。

   ![儲存查詢](./media/tutorial-hunting/save-query.png)

建立新的搜尋查詢：

1. 按一下 [追加**查詢**]，然後選取 [**儲存**]。
2. 填寫所有空白欄位，然後選取 [**儲存**]。

   ![新增查詢](./media/tutorial-hunting/new-query.png)

複製並修改現有的搜尋查詢：

1. 選取您想要修改之資料表中的搜尋查詢。
2. 選取您要修改之查詢行中的省略號（...），然後選取 [**複製查詢**]。

   ![複製查詢](./media/tutorial-hunting/clone-query.png)
 

3. 修改查詢，然後選取 [**建立**]。

   ![自訂查詢](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>後續步驟
在本文中，您已瞭解如何使用 Azure Sentinel 來執行搜尋調查。 若要深入了解 Azure Sentinel，請參閱下列文章：


- [使用筆記本來執行自動化的搜尋活動](notebooks.md)
- [搜尋時使用書簽來儲存有趣的資訊](bookmarks.md)
