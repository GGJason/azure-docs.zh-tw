---
title: 加入多面向導覽類別目錄階層
titleSuffix: Azure Cognitive Search
description: 在與 Azure 認知搜尋整合的搜尋應用程式中，為自我導向篩選新增多面向導覽。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 5f4435ca213584fff84f3ddad9bda6f7e06628a1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "79283156"
---
# <a name="how-to-implement-faceted-navigation-in-azure-cognitive-search"></a>如何在 Azure 認知搜尋中執行多面向導覽

多面向導覽是一個篩選機制，它在搜尋應用程式中提供自動導向的向下鑽研導覽。 「多面向導覽」一詞可能讓您感到陌生，但您可能早已使用過它。 如下列範例所示，多面向導覽其實就是用來篩選結果的類別。

 ![Azure 認知搜尋作業入口網站示範](media/search-faceted-navigation/azure-search-faceting-example.png "Azure 認知搜尋作業入口網站示範")

多面向導覽是替代的搜尋進入點。 它提供了方便的替代功能，讓您不必手動輸入複雜搜尋運算式。 Facet 可以協助您尋找所要的內容，同時確保不會得到零結果。 身為開發人員，facet 可讓您公開最有用的搜尋條件，以流覽您的搜尋索引。 在線上零售應用程式中，多面向導覽通常是根據品牌、部門（小孩的鞋）、大小、價格、熱門程度和評等來建立的。 

實作多面向導覽會因各搜尋技術而不同。 在 Azure 認知搜尋中，多面向導覽是使用您先前在架構中屬性化的欄位，在查詢階段建立。

-   在應用程式所建置的查詢中，查詢必須傳送「面向查詢參數」**，以取得該文件結果集的可用面向篩選值。

-   若要實際修剪文件結果集，應用程式還必須套用 `$filter` 運算式。

在您的應用程式開發中，撰寫建構查詢的程式碼，而這些查詢構成整體工作的一大部分。 此服務提供大部分您希望從多面向導覽獲得的應用程式行為，包括定義範圍和取得面向結果計數的內建支援。 服務也包含合理的預設值，能協助您避免難處理的導覽結構。 

## <a name="sample-code-and-demo"></a>程式碼範例和示範
本文使用作業搜尋入口網站來作為範例。 此範例會實作為 ASP.NET MVC 應用程式。

- 請參閱[Azure 認知搜尋作業入口網站示範](https://aka.ms/azjobsdemo)，並線上測試工作示範。

- 從 [GitHub 上的 Azure 範例儲存機制](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)下載程式碼。

## <a name="get-started"></a>開始使用
如果您不了解搜尋開發，最好的方法就是把多面向導覽想成它會顯示自動導向搜尋的可能選項。 這是一種以預先定義的篩選器為基礎的向下切入搜尋體驗，用於透過點按下動作快速縮小搜尋結果的範圍。 

### <a name="interaction-model"></a>互動模型

多面向導覽的搜尋體驗會反復進行，因此讓我們先以一系列的查詢來進行瞭解，以回應使用者動作。

起點是一個提供多面向導覽 (通常位在側邊) 應用程式頁面。 多面向導覽通常是樹狀結構，且每個值有核取方塊，或可點選的文字。 

1. 傳送至 Azure 認知搜尋的查詢會透過一或多個 facet 查詢參數來指定多面向導覽結構。 例如，查詢可能包括 `facet=Rating`，可能還有 `:values` 或 `:sort` 選項，以進一步精簡展示。
2. 展示層會使用要求上的面向來轉譯提供多面向導覽的頁面。
3. 舉包含「評分」的多面向導覽結構為例，您按一下「4」表示只會顯示評分為 4 或更高的產品。 
4. 應用程式會傳送包含 `$filter=Rating ge 4` 
5. 展示層會更新頁面，並顯示精簡過的結果集，其中只包含滿足新準則 (此案例中是評分為 4 或更高) 的產品。

面向是查詢參數，但不要將它與查詢輸入混淆了。 它在查詢中不是當作選取準則。 反之，將面向查詢參數想成是，返回的回應中所包含對導覽結構的輸入。 針對您提供的每個 facet 查詢參數，Azure 認知搜尋會評估每個 facet 值的部分結果中有多少檔。

請注意步驟 4 中的 `$filter` 。 此篩選是多面向導覽中的一個重要層面。 雖然面向和篩選條件在 API 中是獨立的，但您會需要兩者來提供您想要的體驗。 

### <a name="app-design-pattern"></a>應用程式設計模式

在應用程式程式碼中，模式為使用面向查詢參數來傳回多面向導覽結構和面向結果，並外加一個 $filter 運算式。  篩選條件運算式會處理面向值的點擊事件。 將 `$filter` 運算式想成修剪傳回展示層之結果的程式碼 舉「色彩」的面向為例，按一下 [紅色] 這個色彩是透過只選取有紅色這個色彩的項目之 `$filter` 運算式來實作。 

### <a name="query-basics"></a>查詢基本概念

在「Azure 認知搜尋」中，要求是透過一或多個查詢參數來指定（請參閱[搜尋檔](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)以取得每一項的說明）。 沒有任何一個查詢參數是必要的，但您至少要有一個才能讓查詢有效。

精確度 (視為篩選並排除不相關項目的能力) 是透過下列運算式而達到：

-   **搜尋 =**  
     此參數的值構成搜尋運算式。 它可能為單一的文字，或包括多個名詞和運算子的複雜搜尋運算式。 在伺服器上，搜尋運算式用於進行全文檢索搜尋、針對符合的單字查詢索引中可搜尋的欄位、按順位順序傳回結果。 如果您將 `search` 設為 null，查詢會在整個索引執行 (也就是 `search=*`)。 在此情況下，查詢的其他元素 (例如 `$filter` 或評分設定檔) 是影響所傳回文件 `($filter`，以及順序 (`scoringProfile` 或 `$orderby`) 的主要因素。

-   **$filter =**  
     篩選器是一個強大的機制，可以根據指定的文件屬性限制搜尋結果的大小。 `$filter` 會先被評估，接著是會產生每個值的可用值和對應計數的多面向邏輯。

複雜的搜尋運算式會降低查詢的效能。 盡可能使用建構良好的篩選條件運算式，來提高精確度並改善查詢效能。

為了更明白篩選器如何提高精確度，請將一個複雜的搜尋運算式，與包含下列篩選運算式的搜尋預算式進行比較：

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq 'Seattle' and Parking and Type ne 'motel'`

這兩個查詢都是有效的，但如果您要尋找位於西雅圖的非 motels，則第二個是較佳的。
-   第一個查詢依賴字串欄位 (例如「名稱」、「描述」及其他包含可搜尋資料的欄位) 是否提及指定的文字。
-   第二個查詢是尋找精確符合的結構化資料，且可能較為正確。

在包含多面向導覽的應用程式中，請確定每個透過多面向導覽結構進行的使用者動作都伴隨搜尋結果縮減。 若要縮減結果，請使用篩選條件運算式。

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>建置多面向導覽應用程式
您可以在建立搜尋要求的應用程式程式碼中，使用 Azure 認知搜尋來執行多面向導覽。 多面向導覽會依賴您先前在結構描述中定義的元素。

`Facetable [true|false]` 索引屬性預先定義在您的搜尋索引中，且設定在所選取的欄位，以便在多面向導覽結構中啟用或停用他們。 若沒有 `"Facetable" = true`，該欄位就不能在多面向導覽中使用。

您程式碼中的展示層提供使用者經驗。 它應該列示多面向導覽的組成部份，例如標籤、值、核取方塊和計數。 Azure 認知搜尋 REST API 與平臺無關，因此請使用您想要的任何語言和平臺。 重要的是要包含支援累加式重新整理的 UI 元素，當選取額外的面向便會更新 UI 狀態。 

查詢的時候，應用程式程式碼會建立包含 `facet=[string]`要求參數的要求，它會提供要進行面向篩選的欄位。 一個查詢可以有多個面向，例如 `&facet=color&facet=category&facet=rating`，每個面向都以 & 符號分隔。

應用程式程式碼也必須建構 `$filter` 運算式來處理多面向導覽中的點選事件。 `$filter` 會使用面向值當作篩選準則將搜尋結果縮減。

Azure 認知搜尋會根據您輸入的一或多個字詞，以及多面向導覽結構的更新，傳回搜尋結果。 在「Azure 認知搜尋」中，多面向導覽是單一層級的結構，其中包含 facet 值，以及為每個資料找到多少結果的計數。

在接下來的章節中，我們將進一步了解如何建立各個部份。

<a name="buildindex"></a>

## <a name="build-the-index"></a>建置索引
多面向的功能是透過索引中各欄位的 `"Facetable": true`索引屬性來啟用。  
根據預設，所有可能用於多面向導覽的欄位類型為 `Facetable` 。 這類欄位類型`Edm.String`包括`Edm.DateTimeOffset`、和所有數值欄位類型（基本上，所有欄位類型都是 facetable， `Edm.GeographyPoint`但不能用於多面向導覽）。 

在建置索引時，多面向導覽的最佳作法是關閉不應做為面向之欄位的多面向功能。  尤其是單一值的字串欄位 (例如識別碼或產品名稱)，應該將它們設為 `"Facetable": false` 以防止意外被用於多面向導覽。 關閉不需要的 facet 可協助您將索引的大小保持在最小，而且通常會改善效能。

以下是「作業入口網站示範」應用程式範例之結構描述的一部分 (已修剪一些屬性來減少大小)：

```json
{
  ...
  "name": "nycjobs",
  "fields": [
    { "name": "id",                 "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { "name": "job_id",             "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { "name": "agency",              "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "posting_type",        "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "num_of_positions",    "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { "name": "business_title",      "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "civil_service_title", "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "title_code_no",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "level",               "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "salary_range_from",   "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { "name": "salary_range_to",     "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { "name": "salary_frequency",    "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "work_location",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
…
    { "name": "geo_location",        "type": "Edm.GeographyPoint",     "searchable": false, "filterable": true, ...  "facetable": false, ... },
    { "name": "tags",                "type": "Collection(Edm.String)", "searchable": true,  "filterable": true, ...  "facetable": true, ...  }
  ],
…
}
```

如您在範例架構中所見， `Facetable`針對不應當做 facet 使用的字串欄位（例如識別碼值），會關閉。 關閉不需要的 facet 可協助您將索引的大小保持在最小，而且通常會改善效能。

> [!TIP]
> 根據最佳作法，建議每個欄位都包含整組完整的索引屬性。 雖然幾乎所有欄位的 `Facetable` 預設都為開啟，但刻意設定每個屬性，可協助您仔細思考每個結構描述的決定。 

<a name="checkdata"></a>

## <a name="check-the-data"></a>檢查資料
資料品質會直接影響所具體化的多面向導覽結構是否符合您的預期。 它也會影響建構篩選器來減少結果集的作業容易與否。

若您想要根據「品牌」或「價格」進行多面向導覽，每個文件就應包含 BrandName** 和 ProductPrice** 值，它們要像篩選選項般有效且一致，並能提昇效率。

以下是一些需要注意的提醒事項：

* 思考每個要用於多面向導覽的欄位，是否包含適合在自動導向搜尋中，當作篩選條件的值。 該值應簡短、具描述性，且能在對照的選項之間，提供可有效分辨的明確選擇。
* 拼字錯誤或近似符合的值。 如果您根據「色彩」進行多面向導覽，且欄位值包含 Orange 和 Ornage (拼字錯誤)，根據「色彩」欄位的面向應該兩者都採用。
* 混合大小寫的文字對多面向導覽也會遭成嚴重影響 (orange 和 Orange 為兩個不同的值)。 
* 同一個植的單數和附數版本會導致個別的面向。

正如您所想，準備資料的評鑑部份，是建置有效的多面向導覽之必要層面。

<a name="presentationlayer"></a>

## <a name="build-the-ui"></a>建置 UI
從展示層反向做起可協助您發現從另一方向可能忽略的需求，並了解搜尋體驗的必要功能有哪些。

以多面向導覽而言，您的網頁或應用程式頁面會顯示多面向導覽結構，偵測頁面上的使用者輸入，並插入變更的元素。 

對於 Web 應用程式，展示層通常是使用 AJAX，因為它可讓您重新整理累加的變更。 您也可以使用 ASP.NET MVC 或任何其他可透過 HTTP 連線至 Azure 認知搜尋服務的視覺效果平臺。 本文中所參考的範例應用程式（ **Azure 認知搜尋作業入口網站示範**）剛好是 ASP.NET 的 MVC 應用程式。

在範例中，多面向導覽會內建在搜尋結果頁面。 下列範例取自應用程式範例的 `index.cshtml` 檔案，它會顯示可在搜尋結果頁面顯示多面向導覽的靜態 HTML 結構。 當您提交搜尋詞彙或是選取或清除面向時，系統便會動態建置或重建面向清單。

```html
<div class="widget sidebar-widget jobs-filter-widget">
  <h5 class="widget-title">Filter Results</h5>
    <p id="filterReset"></p>
    <div class="widget-content">

      <h6 id="businessTitleFacetTitle">Business Title</h6>
      <ul class="filter-list" id="business_title_facets">
      </ul>

      <h6>Location</h6>
      <ul class="filter-list" id="posting_type_facets">
      </ul>

      <h6>Posting Type</h6>
      <ul class="filter-list" id="posting_type_facets"></ul>

      <h6>Minimum Salary</h6>
      <ul class="filter-list" id="salary_range_facets">
      </ul>

  </div>
</div>
```

下列來自 `index.cshtml` 頁面的程式碼片段會動態建置 HTML，以顯示第一個面向 (職稱)。 類似的函式會動態建置其他面向的 HTML。 每個面向都有標籤和計數，以顯示針對該面向結果所找到的項目數。

```js
function UpdateBusinessTitleFacets(data) {
  var facetResultsHTML = '';
  for (var i = 0; i < data.length; i++) {
    facetResultsHTML += '<li><a href="javascript:void(0)" onclick="ChooseBusinessTitleFacet(\'' + data[i].Value + '\');">' + data[i].Value + ' (' + data[i].Count + ')</span></a></li>';
  }

  $("#business_title_facets").html(facetResultsHTML);
}
```

> [!TIP]
> 在設計搜尋結果頁面時，請記得加入清除面向的機制。 如果您新增核取方塊，即可輕鬆地了解如何清除篩選條件。 針對其他配置，您可能需要階層模式或其他具創意的方法。 例如，在「作業搜尋入口網站」應用程式範例中，您可以按一下所選面向後的 `[X]` 以清除該面向。

<a name="buildquery"></a>

## <a name="build-the-query"></a>建置查詢
您所撰寫用於建置查詢的程式碼，應指定有效查詢的所有部份，包括搜尋運算式、面向、篩選條件、評分設定檔 (制訂要求使用的任何項目)。 在本節中，我們會探索面向在查詢中的位置，及篩選條件如何配合面向使用以提供縮減的結果集。

請注意，面向在此範例應用程式中是必要的。 「作業入口網站示範」中的搜尋體驗是根據多面向導覽和篩選條件而設計。 從多面向導覽是放置在頁面上的顯著位置這一點，可以看出其重要性。 

從範例著手是不錯的開始。 下列範例取自 `JobsSearch.cs` 檔案，它所建置的要求會根據「職稱」、「位置」、「過賬類型」和「最低工資」建立多面向導覽。 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

面向查詢參數已設定至欄位，且根據資料類型能夠由包含 `count:<integer>`、`sort:<>`、`interval:<integer>` 與 `values:<list>` 的逗號分隔清單進一步參數化。 當設定範圍時，針對數值資料支援值清單。 如需使用方式詳細資料，請參閱[搜尋檔（Azure 認知搜尋 API）](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 。

由您的應用程式制訂的要求也應該與面向一起建置篩選條件，以根據選取的面向值縮小候選文件集的範圍。 就單車店而言，多面向導覽可提供「有哪些顏色、製造商和單車類型可供販售？」** 等問題的線索。 篩選功能可解答「哪些單車確實是位於此價格帶內的紅色登山車？」** 之類的問題。 當您按一下 [紅色] 來表示僅顯示紅色的商品，應用程式傳送的下一個查詢會包含 `$filter=Color eq 'Red'`。

下列來自 `JobsSearch.cs` 頁面的程式碼片段會在您選取「職稱」面向的值時，在篩選中新增所選的職稱。

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>祕訣和最佳作法

### <a name="indexing-tips"></a>編製索引的秘訣
**在不使用搜尋方塊的情況下改善索引效率**

如果您的應用程式單獨使用多面向導覽 (也就是沒有搜尋方塊)，您可以將欄位標記為 `searchable=false`、`facetable=true` 以產生更多壓縮索引。 此外，僅會在完整的面向值 (沒有文字分行或多重文字值元件部分的索引) 上編製索引。

**指定可做為面向的欄位**

請記得索引結構描述會定義哪一個欄位可做為面向。 假設欄位可面向化，查詢會指定要執行面向化的欄位。 您執行面向化的欄位會提供顯示於標籤下方的值。 

顯示於各標籤下方的值會從索引擷取。 例如，如果面向欄位是 [色彩]**，其他篩選條件可用的值會是適用於該欄位的值 (紅色、黑色，依此類推)。

僅針對數值與日期時間值，您可以明確地在面向欄位上設定值 (例如， `facet=Rating,values:1|2|3|4|5`)。 這些欄位類型允許值清單，以簡化將面向結果分離至連續範圍 (根據數值或時間間隔其中之一的範圍) 的動作。 

**依預設您只能擁有一個多面向導覽層級** 

如附註所說明，階層中沒有直接支援巢狀面向。 根據預設，Azure 認知搜尋中的多面向導覽只支援一個層級的篩選準則。 不過有因應措施。 您可以在 `Collection(Edm.String)` 中利用各階層各一個進入點來編碼階層面向。 此因應措施的實作不是本文的討論範圍。 

### <a name="querying-tips"></a>查詢秘訣
**驗證欄位**

如果您根據未受信任的使用者輸入動態建置面向清單，請驗證多面向欄位的名稱是否有效。 或者，請在建置 URL 時，使用 .NET 中的 `Uri.EscapeDataString()` 或所選平台的同等功能來逸出這些名稱。

### <a name="filtering-tips"></a>篩選秘訣
**使用篩選條件提高搜尋精準度**

 使用篩選條件。 如果您只依賴搜尋運算式，詞幹分析可能會導致傳回的檔在其任何欄位中沒有精確的 facet 值。

**使用篩選條件提高搜尋效能**

 篩選條件能縮小候選文件集的範圍，以從排名中搜尋與排除它們。 如果您有大型文件集，使用經過挑選的面向向下切入通常能讓您的效能提升。
  
**僅篩選多面向欄位**

在多面向向下切入中，您通常希望僅包含在特定欄位中，而非在所有可搜尋欄位的任一處具有面向值的文件。 新增篩選條件能透過指示服務僅在多面向欄位中搜尋相符值，加強目標欄位。

**使用更多篩選條件修剪面向結果**

面向結果是在符合面向字詞的搜尋結果中找到的文件。 在下列範例中，「雲端運算」** 的搜尋結果中有 254 個項目也具有做為內容類型的「內部規格」**。 項目毋須互斥。 如果項目同時符合兩個篩選條件的條件，則會個別計數。 在 `Collection(Edm.String)` 欄位 (通常用來實作文件標記) 上進行面向化時可能發生此重複情形。

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

一般來說，如果您發現面向結果一直過大，建議您新增更多篩選條件，以便讓使用者有更多選項可縮小搜尋範圍。

### <a name="tips-about-result-count"></a>關於結果計數的秘訣

**限制多面向導覽中的項目數**

針對瀏覽樹狀目錄中的每個多面向欄位，預設限制為 10 個值。 對於導覽結構來說，此項預設值相當合理，因為它維持值清單在一個可管理的大小。 您可以透過指派計數值來覆寫預設值。

* `&facet=city,count:5` 指定只有最高排序結果中的前 5 個城市會傳回為面向結果。 請考慮具有搜尋詞彙 "機場" 和32相符專案的範例查詢。 若查詢指定 `&facet=city,count:5`，則只有搜尋結果中包含最多文件的前五個特定城市會包含在面向結果中。

請注意面向結果與搜尋結果之間的區別。 搜尋結果是符合查詢的所有文件。 面向結果是各面向值的符合項。 在此範例中，搜尋結果會包含不在面向分類清單中的「城市」名稱 (範例中為 5 個)。 當您清除面向或選擇「城市」以外的其他面向時，系統會顯示透過多面向導覽篩選出的結果。 

> [!NOTE]
> 當有一種類型以上時討論 `count` 可能會讓人混淆。 下表提供如何在 Azure 認知搜尋 API、範例程式碼和檔中使用此詞彙的簡短摘要。 

* `@colorFacet.count`<br/>
   在簡報程式碼中，您應該會在面向上面看見用來顯示面向結果數目的計數參數。 在面向結果中，計數表示符合面向字詞或範圍的文件數目。
* `&facet=City,count:12`<br/>
   在面向查詢中，您可以設定值的計數。  預設值為 10，但您可以將它設為更高或更低的值。 設定 `count:12` 會取得面向結果中依照文件計數排序的前 12 個相符項。
* "`@odata.count`"<br/>
   在查詢回應中，此值表示搜尋結果中相符項目的數目。 平均而言，它會大於所有 facet 結果的總和，因為符合搜尋詞彙的專案存在，但沒有 facet 值相符。

**取得面向結果中的計數**

新增篩選條件至多面向查詢時，您可能希望保留面向陳述式 (例如，`facet=Rating&$filter=Rating ge 4`)。 技術上而言，facet = 不需要評等，但會將其保留為分級4和更高的 facet 值計數。 例如，如果您按一下 "4" 且查詢包含大於或等於 "4" 的篩選條件，則會針對等於或大於 4 的各分級傳回計數。  

**確實取得精確的面向計數**

在某些情況下，您可能會發現 facet 計數不符合結果集（請參閱[Azure 認知搜尋中的多面向導覽（論壇文章）](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)）。

面向計數可能會因為分區結構而不正確。 每個搜尋索引都有多個分區，且每個分區都會依照文件計數報告前 N 個面向，然後結合為單一結果。 如果一些分區有許多相符值，而一些有較少相符值，您可能發現結果中有一些面向值遺失或短少。

雖然此行為隨時可能會變更，但如果您今天遇到這項行為，您可以透過人為因而誇大來解決此問題\<：數位> 為較大的數位，以強制執行每個分區的完整報告。 如果計數: 的值大於或等於欄位中唯一值的數目，您就能保證結果正確。 不過，當文件計數很高的時候，則會有效能的負面影響，因此請謹慎使用此選項。

### <a name="user-interface-tips"></a>使用者介面秘訣
**針對多面向導覽中的各欄位新增標籤**

標籤通常以 HTML 或表單定義 (應用程式範例中為 `index.cshtml`)。 在 Azure 認知搜尋中，facet 導覽標籤或其他任何中繼資料都沒有 API。

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>根據範圍進行篩選
透過值範圍進行面向化是常見的搜尋應用程式需求。 範圍支援數值資料與日期時間值。 您可以在[搜尋檔（Azure 認知搜尋 API）](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)中閱讀更多關於每個方法的資訊。

Azure 認知搜尋提供兩種方法來計算範圍，藉此簡化範圍結構。 針對這兩種方法，Azure 認知搜尋會根據您提供的輸入來建立適當的範圍。 例如，如果您指定 10|20|30 的值範圍，它會自動建立 0-10、10-20、20-30 的範圍。 您的應用程式可以選擇性地移除任何空白間隔。 

**方法 1︰使用間隔參數**  
若要設定 10 美元增量的價格面向，您會指定︰`&facet=price,interval:10`

**方法 2︰使用值清單**  
 針對數值資料，您可以使用值清單。  請設想 `listPrice` 欄位的面向範圍，其呈現如下：

  ![範例值清單](media/search-faceted-navigation/Facet-5-Prices.PNG "範例值清單")

若要指定與先前螢幕擷取畫面類似的面向範圍，請使用值清單︰

    facet=listPrice,values:10|25|100|500|1000|2500

已使用 0 做為起始點，清單中的值做為結束點建置各範圍，並且修剪之前的範圍來建立離散間隔。 Azure 認知搜尋會執行這些動作，做為多面向導覽的一部分。 您不需要撰寫程式碼來建構各個間隔。

### <a name="build-a-filter-for-a-range"></a>針對範圍建置篩選條件
若要根據您選取的範圍篩選文件，您可以在定義範圍端點的兩段式運算式中使用 `"ge"` 與 `"lt"` 篩選運算子。 例如，如果您選擇 10-25 作為 `listPrice` 欄位的範圍，篩選條件會是 `$filter=listPrice ge 10 and listPrice lt 25`。 在程式碼範例中，篩選條件運算式使用 **priceFrom** 與 **priceTo** 參數來設定端點。 

  ![查詢某範圍的值](media/search-faceted-navigation/Facet-6-buildfilter.PNG "查詢某範圍的值")

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>根據距離進行篩選
您通常會看到可協助您根據目前位置的鄰近性來選擇商店、餐廳或目的地的篩選準則。 雖然這種類型的篩選器看起來可能像多面向導覽，但它只是一個篩選準則。 對於那些特別要尋找特定設計問題之實作建議的使用者，我們會在此處說明。

Azure 認知搜尋、**地理**位置和**地理交集**中有兩個地理空間函數。

* **geo.distance** 函式會傳回兩點之間的距離 (以公里為單位)。 一個點是欄位，另一個點則是傳遞作為篩選條件一部分的常數。 
* 如果給定的點位於給定的多邊形內，**geo.distance** 函式會傳回 true。 該點為欄位，而多邊形則會指定作為座標的常數清單，並傳遞作為篩選條件的一部分。

您可以在[OData 運算式語法（Azure 認知搜尋）](query-odata-filter-orderby-syntax.md)中找到篩選準則範例。

<a name="tryitout"></a>

## <a name="try-the-demo"></a>試用示範
Azure 認知搜尋作業入口網站示範包含本文所參考的範例。

-   請參閱[Azure 認知搜尋作業入口網站示範](https://aka.ms/azjobsdemo)，並線上測試工作示範。

-   從 [GitHub 上的 Azure 範例儲存機制](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)下載程式碼。

當您使用搜尋結果的同時，請造訪該 URL 查看查詢建構中的變更。 當您選取各個結果時，此應用程式會將面向附加到 URI。

1. 若要使用示範應用程式的對應功能，請從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)取得 Bing 地圖服務的金鑰。 將它貼在 `index.cshtml` 頁面中的現有金鑰上。 系統不會使用 `Web.config` 檔案中的 `BingApiKey` 設定。 

2. 執行應用程式。 接受選擇性的導覽，或關閉對話方塊。
   
3. 輸入搜尋詞彙，例如「分析師」，然後按一下 [搜尋] 圖示。 查詢會快速執行。
   
   多面向導覽結構會隨搜尋結果一併傳回。 在搜尋結果頁面中，多面向導覽結構會包含各面向結果計數。 我們並未選取任何面向，因此會傳回所有符合的結果。
   
   ![選取面向之前的搜尋結果](media/search-faceted-navigation/faceted-search-before-facets.png "選取面向之前的搜尋結果")

4. 按一下 [職稱]、[位置] 或 [最低工資]。 初始搜尋時面向會是 Null，但是隨著它們取得值，搜尋結果會修剪不再符合的項目。
   
   ![選取面向之後的搜尋結果](media/search-faceted-navigation/faceted-search-after-facets.png "選取面向之後的搜尋結果")

5. 若要清除多面向查詢，以便能夠嘗試不同的查詢行為，請按一下所選面向後面的 `[X]` 以清除面向。
   
<a name="nextstep"></a>

## <a name="learn-more"></a>深入了解
觀看[Azure 認知搜尋深入探討](https://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410)。 在 45:25 時有示範如何實作多面向。

如需多面向導覽設計原則的深入見解，推薦您下列連結：

* [設計模式：多面向導覽](https://alistapart.com/article/design-patterns-faceted-navigation)
* [執行多面向搜尋時的前端考慮–第1部分](https://articles.uie.com/faceted_search2/)

