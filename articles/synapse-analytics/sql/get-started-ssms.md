---
title: SSMS：連線及查詢 Synapse SQL
description: 使用 SQL Server Management Studio (SSMS) 連線及查詢 Azure Synapse Analytics 中的 Synapse SQL。
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 704da86fd1d816dbf5d6cd9cf67dfee53fce2622
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419632"
---
# <a name="connect-to-synapse-sql-with-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS) 連線到 Synapse SQL
> [!div class="op_single_selector"]
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)
> 
> 

您可以使用 [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms)，透過 SQL 隨選 (預覽) 或 SQL 集區資源，連線至 Azure Synapse Analytics 中的 Synapse SQL 並加以查詢。 

### <a name="supported-tools-for-sql-on-demand-preview"></a>支援的 SQL 隨選工具 (預覽)

從版本 18.5 開始，只有在連接和查詢等有限功能的情況之下，才支援 SSMS。 [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) 受到完整支援。

## <a name="prerequisites"></a>Prerequisites

開始之前，請先確定您已擁有下列必要條件：  

* [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms)。 
* 針對 SQL 集區，您需要現有的資料倉儲。 若要建立資料倉儲，請參閱[建立 SQL 集區](../quickstart-create-sql-pool.md)。 針對 SQL 隨選，系統已在建立時在您的工作區中加以佈建。 
* 完整的 SQL 伺服器名稱。 若要找到此名稱，請參閱[連線至 Synapse SQL](connect-overview.md)。

## <a name="connect"></a>連線

### <a name="sql-pool"></a>SQL 集區

若要使用 SQL 集區連線到 Synapse SQL，請遵循下列步驟： 

1. 開啟 SQL Server Management Studio (SSMS)。 
1. 在 [連線到伺服器]  對話方塊中填妥欄位，然後選取 [連線]  ： 
  
    ![連線到伺服器](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)
   
   * **伺服器名稱**：輸入先前找到的 **伺服器名稱** 。
   * **驗證**：選取驗證類型，像是 [SQL Server 驗證]  或 [Active Directory 整合式驗證]  。
   * **使用者名稱**和**密碼**：如果上面已選取 [SQL Server 驗證]，請輸入使用者名稱和密碼。

1. 在**物件總管**中展開您的 Azure SQL Server。 您可以檢視與伺服器相關聯的資料庫，例如範例 AdventureWorksDW 資料庫。 您可以展開資料庫以查看資料表：
   
    ![探索 AdventureWorksDW](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/explore-tables.png)


### <a name="sql-on-demand-preview"></a>SQL 隨選 (預覽)

若要使用 SQL 隨選連線到 Synapse SQL，請遵循下列步驟： 

1. 開啟 SQL Server Management Studio (SSMS)。
1. 在 [連線到伺服器]  對話方塊中填妥欄位，然後選取 [連線]  ： 
   
    ![連線到伺服器](./media/get-started-ssms/connect-object-explorer1.png)
   
   * **伺服器名稱**：輸入先前找到的 **伺服器名稱** 。
   * **驗證**：選取驗證類型，像是 [SQL Server 驗證]  或 [Active Directory 整合式驗證]  ：
   * **使用者名稱**和**密碼**：如果上面已選取 [SQL Server 驗證]，請輸入使用者名稱和密碼。
   * 按一下 [ **連接**]。

4. 若要瀏覽，請展開您的 Azure SQL 伺服器。 您可以檢視與伺服器相關聯的資料庫。 展開*示範*以查看範例資料庫中的內容。
   
    ![探索 AdventureWorksDW](./media/get-started-ssms/explore-tables.png)


## <a name="run-a-sample-query"></a>執行範例查詢

### <a name="sql-pool"></a>SQL 集區

既然已建立資料庫連線，您現在可以查詢資料。

1. 在 [SQL Server 物件總管] 中您的資料庫上按一下滑鼠右鍵。
2. 選取 [新增查詢]  。 隨即開啟 [新增查詢] 視窗。
   
    ![新增查詢](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/new-query.png)
3. 將此 T-SQL 查詢複製到查詢視窗中：
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. 執行查詢。 若要這麼做，請按一下 `Execute`，或使用下列快速鍵：`F5`。
   
    ![執行查詢](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/execute-query.png)
5. 查看查詢結果。 在此範例中，FactInternetSales 資料表有 60398 個資料列。
   
    ![查詢結果](../sql-data-warehouse/media/sql-data-warehouse-query-ssms/results.png)

### <a name="sql-on-demand"></a>SQL 隨選

既然已建立資料庫連線，您現在可以查詢資料。

1. 在 [SQL Server 物件總管] 中您的資料庫上按一下滑鼠右鍵。
2. 選取 [新增查詢]  。 隨即開啟 [新增查詢] 視窗。
   
    ![新增查詢](./media/get-started-ssms/new-query.png)
3. 將下列 T-SQL 查詢複製到查詢視窗：
   
    ```sql
    SELECT COUNT(*) FROM demo.dbo.usPopulationView
    ```
4. 執行查詢。 若要這麼做，請按一下 `Execute`，或使用下列快速鍵：`F5`。
   
    ![執行查詢](./media/get-started-ssms/execute-query.png)
5. 查看查詢結果。 在此範例中，usPopulationView 檢視有 3664512 個資料列。
   
    ![查詢結果](./media/get-started-ssms/results.png)

## <a name="next-steps"></a>後續步驟
您現在可以連線並查詢，請嘗試[使用 Power BI 將資料視覺化](get-started-power-bi-professional.md)。

若要針對 Azure Active Directory 驗證設定您的環境，請參閱[驗證 Synapse SQL](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)。

