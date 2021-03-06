---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 03/31/2020
ms.author: juliako
ms.openlocfilehash: d6ff823951e6474a35a514036f02c0aabb17bf01
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82997712"
---
> [!NOTE]
> 對於未修正的資源，請開啟支援票證以要求增加配額。 在嘗試取得更高的限制時，請勿建立額外的 Azure 媒體服務帳戶。

### <a name="account-limits"></a>帳戶限制

| 資源 | 預設限制 | 
| --- | --- | 
| 單一訂用帳戶的媒體服務帳戶 | 25 (固定) |

### <a name="asset-limits"></a>資產限制

| 資源 | 預設限制 | 
| --- | --- | 
| 每個媒體服務帳戶的資產 | 1,000,000|

### <a name="storage-media-limits"></a>存放裝置（媒體）限制

| 資源 | 預設限制 | 
| --- | --- | 
| 檔案大小| 在某些情況下，對於媒體服務中支援處理的檔案大小上限有所限制。 <sup>sha-1</sup> |
| 儲存體帳戶 | 100<sup>（2）</sup> （固定） |

<sup>1</sup> 單一 blob 支援的大小上限目前在 Azure Blob 儲存體是最多 5 TB。 根據服務所使用的 VM 大小，媒體服務中會套用額外的限制。 大小限制適用于您上傳的檔案，以及因媒體服務處理（編碼或分析）而產生的檔案。 如果原始程式檔超過 260 GB，您的工作可能會失敗。 

下表顯示媒體保留單元 S1、S2 和 S3 的限制。 如果您的原始程式檔大於資料表中所定義的限制，則編碼工作會失敗。 如果您編碼長時間的4K 解析來源，就必須使用 S3 媒體保留單元來達到所需的效能。 如果您的4K 內容大於 S3 媒體保留單元的 260 GB 限制，請開啟支援票證。

|媒體保留單元類型|輸入大小上限（GB）|
|---|---|
|S1 |    26|
|S2    | 60|
|S3    |260|

<sup>2</sup>儲存體帳戶必須來自相同的 Azure 訂用帳戶。

### <a name="jobs-encoding--analyzing-limits"></a>作業（編碼 & 分析）限制

| 資源 | 預設限制 | 
| --- | --- | 
| 每個媒體服務帳戶的工作 | 500000 <sup>（3）</sup> （固定）|
| 每個作業的作業輸入 | 50 (固定)|
| 每個作業的作業輸出 | 20 (固定) |
| 每個媒體服務帳戶的轉換 | 100 (固定)|
| 轉換轉換中的輸出 | 20 (固定) |
| 每個作業輸入的檔案|10 (固定)|

<sup>3</sup>此數目包括已佇列、已完成、作用中和已取消的工作。 不包含已刪除的工作。 

您的帳戶中任何超過 90 天的工作記錄，都會自動刪除，即使記錄總數低於配額上限亦然。 

### <a name="live-streaming-limits"></a>即時串流限制

| 資源 | 預設限制 | 
| --- | --- | 
| 每個媒體服務帳戶的實況活動<sup>（4）</sup> |5|
| 每個即時事件的即時輸出 |3 <sup>（5）</sup> |
| 即時輸出持續時間上限 | [DVR 視窗的大小](../articles/media-services/latest/live-event-cloud-dvr.md) |

<sup>4</sup>如需有關即時事件限制的詳細資訊，請參閱[即時事件種類比較和限制](../articles/media-services/latest/live-event-types-comparison.md)。

<sup>5</sup>即時輸出會在建立時開始，並在刪除時停止。

### <a name="packaging--delivery-limits"></a>封裝 & 傳遞限制

| 資源 | 預設限制 | 
| --- | --- | 
| 每個媒體服務帳戶的串流端點 (已停止或執行中)|2（固定）|
| 動態資訊清單篩選條件|100|
| 串流原則 | 100 <sup>（6）</sup> |
| 一次與資產相關聯的唯一串流定位器 | 100<sup>（7）</sup> （固定） |

<sup>6</sup>使用自訂[串流原則](https://docs.microsoft.com/rest/api/media/streamingpolicies)時，您應該為媒體服務帳戶設計一組有限的這類原則，並且在需要相同的加密選項和通訊協定時，針對您的 streaminglocator 重複使用它們。 不建議您對每個串流定位器建立新的串流原則。

<sup>7</sup>串流定位器並非設計來管理每個使用者的存取控制。 若要給予個別使用者不同的存取權限，請使用數位版權管理 (DRM) 方案。

### <a name="protection-limits"></a>保護限制

| 資源 | 預設限制 | 
| --- | --- | 
| 每個內容金鑰原則的選項 |30 | 
| 針對每個帳戶媒體服務金鑰傳遞服務上的每個 DRM 類型，每月的授權|1,000,000|

### <a name="support-ticket"></a>支援票證

對於未修正的資源，您可以藉由開啟[支援票證](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)來要求產生配額。 請在要求中包含所需的配額變更、使用案例情況及所需區域的詳細資訊。 <br/>**請勿**為了嘗試取得更高的限制而建立其他 Azure 媒體服務。
