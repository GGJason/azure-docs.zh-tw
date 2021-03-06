---
title: SMOTE
titleSuffix: Azure Machine Learning
description: 瞭解如何使用 Azure Machine Learning 中的 SMOTE 模組，藉由使用超取樣來增加資料集內的低程度範例數目。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: ed6d9e86143c3a5d6c97c4bd92a07c258bbd1bbc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "79477454"
---
# <a name="smote"></a>SMOTE

本文說明如何使用 Azure Machine Learning 設計工具（預覽）中的 SMOTE 模組，來增加用於機器學習服務之資料集中的不具代表性案例數目。 SMOTE 是較好的方法，可增加罕見案例的數目，而不只是複製現有的案例。  

您將 SMOTE 模組連接到*不平衡*的資料集。 資料集可能不平衡的原因有很多。 例如，您的目標類別在擴展中可能很罕見，或資料可能很難收集。 一般而言，當您想要分析的*類別*是不具代表性時，您會使用 SMOTE。 
  
模組會傳回包含原始範例的資料集。 它也會根據您指定的百分比，傳回一些綜合少數樣本。  
  
## <a name="more-about-smote"></a>深入瞭解 SMOTE

綜合少數超取樣技術（SMOTE）是一種統計技術，可讓您以平衡的方式增加資料集中的案例數目。 此模組的運作方式是從您提供做為輸入的現有少數案例產生新的實例。 這種 SMOTE 的執行*不會變更多數*案例的數目。

新的實例不只是現有少數案例的複本。 相反地，此演算法會針對每個目標類別及其最近的鄰近專案，取得*功能空間*的範例。 然後，此演算法會產生新的範例，將目標案例的功能與鄰近專案的功能結合在一起。 此方法能增加每個類別可用的特徵，並讓樣本更加一般化。
  
SMOTE 會將整個資料集當做輸入，但只會增加少數案例的百分比。 例如，假設您有一個不平衡資料集，其中只有1% 的案例具有目標值 A （少數類別），而99% 的案例具有值 B。若要將少數案例的百分比增加到前一個百分比的兩倍，您可以在模組的屬性中輸入**200**的**SMOTE 百分比**。  
  
## <a name="examples"></a>範例  

我們建議您嘗試將 SMOTE 用於小型的資料集，藉此了解它的作用為何。 下列範例會使用 Azure Machine Learning 設計工具中提供的血捐贈資料集。
  
如果您將資料集加入至管線，並在資料集的輸出上選取 [**視覺化**]，您可以看到資料集的748個數據列或案例中，570案例（76百分比）是類別0，而178案例（24%）是類別1。 雖然這項結果並不不平衡，但類別1代表捐贈了血壓的人，因此這些資料列包含您想要建立模型的*功能空間*。
 
若要增加案例數目，您可以使用100的倍數來設定**SMOTE 百分比**的值，如下所示：

||類別 0|類別1|total|  
|-|-------------|-------------|-----------|  
|原始資料集<br /><br /> （相當於**SMOTE 百分比** = **0**）|570<br /><br /> 76%|178<br /><br /> 天|748|  
|**SMOTE 百分比** = **100**|570<br /><br /> 62%|356<br /><br /> 38%|926|  
|**SMOTE 百分比** = **200**|570<br /><br /> 52%|534<br /><br /> 48%|1,104|  
|**SMOTE 百分比** = **300**|570<br /><br /> 44%|712<br /><br /> 56%|1282|  
  
> [!WARNING]
> 藉由使用 SMOTE 來增加案例數目，並不保證會產生更精確的模型。 請嘗試以不同的百分比、不同的功能集和不同數目的最近鄰近專案進行管線處理，以查看新增案例如何影響您的模型。  
  
## <a name="how-to-configure-smote"></a>如何設定 SMOTE
  
1.  將 SMOTE 模組新增至您的管線。 您可以在 [**資料轉換模組**] 底下的 [**操作**] 分類中找到模組。

2. 連接您想要提升的資料集。 如果您想要指定用來建立新案例的功能空間，只要使用特定資料行或排除部分，就可以使用 [[選取資料集中的資料行](select-columns-in-dataset.md)] 模組。 接著，您可以在使用 SMOTE 之前，先找出您想要使用的資料行。
  
    否則，透過 SMOTE 建立新案例的方式是以您提供作為輸入的*所有*資料行為基礎。 特徵資料行的至少一個資料行是數值。
  
3.  請確定已選取包含標籤或目標類別的資料行。 SMOTE 只接受二進位標籤。
  
4.  SMOTE 模組會自動識別 [標籤] 資料行中的少數類別，然後取得少數類別的所有範例。 所有資料行都不能有 NaN 值。
  
5.  在 [ **SMOTE 百分比**] 選項中，輸入一個整數，指出輸出資料集中少數案例的目標百分比。 例如：  
  
    - 您輸入**0**。 SMOTE 模組傳回的資料集與您提供的輸入完全相同。 它不會加入任何新的少數案例。 在此資料集中，類別比例尚未變更。  
  
    - 您輸入**100**。 SMOTE 模組會產生新的少數案例。 它會加上原始資料集內相同數目的少數案例。 因為 SMOTE 不會增加多數案例的數目，所以每個類別的案例比例已改變。  
  
    - 您輸入**200**。 相較于原始資料集，此模組會使少數案例的百分比加倍。 這不*會*產生與之前相同的少數案例數兩倍。 相反地，資料集的大小會增加，因此大部分案例的數目會維持不變。 少數案例的數目會增加，直到符合所需的百分比值為止。  
  
    > [!NOTE]
    > SMOTE 百分比僅使用100的倍數。

6.  使用 [**最近的鄰近專案數**] 選項，決定 SMOTE 演算法在建立新案例時所使用的功能空間大小。 最近的鄰近是類似于目標案例的資料列（案例）。 任兩個案例之間的距離是藉由結合所有功能的加權向量來測量。  
  
    + 藉由增加最近鄰近專案的數目，您就可以從更多案例中取得功能。
    + 藉由保持最接近的鄰近專案數目，您可以使用與原始範例相同的功能。  
  
7. 如果您想要使用相同的資料來確保相同管線執行相同的結果，請在 [**隨機種子**] 方塊中輸入值。 否則，此模組會在部署管線時，根據處理器時鐘值產生隨機種子。 隨機種子的產生會在執行時產生稍微不同的結果。

8. 提交管線。  
  
   模組的輸出是一種資料集，其中包含原始資料列加上一些已加入的資料列，並包含少數案例。  

## <a name="technical-notes"></a>技術說明

+ 當您發行使用**SMOTE**模組的模型時，請先從預測性管線中移除**SMOTE** ，再將其發佈為 web 服務。 這是因為 SMOTE 的目的是要在定型期間改善模型，而不是用於計分。 如果已發佈的預測管線包含 SMOTE 模組，您可能會收到錯誤。

+ 如果您清除遺漏值，或在套用 SMOTE 之前套用其他轉換來修正資料，通常會得到更好的結果。 

+ 有些研究人員調查了 SMOTE 對高維度或鬆散資料（例如文字分類或 genomics 資料集中所使用的資料）是否有效。 這份檔對於在這類情況下套用 SMOTE 的效果和理論有效度，有很好的摘要： [Blagus 和 Lusa： SMOTE 適用于高維度類別不平衡資料](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-106)。

+ 如果 SMOTE 在您的資料集中無法生效，您可能會考慮的其他方法包括：
  + 超取樣少數案例或 undersampling 多數案例的方法。
  + 集團技術可使用叢集、封袋或彈性提升來直接協助學習模組。


## <a name="next-steps"></a>後續步驟

請參閱可用來 Azure Machine Learning 的[模組集合](module-reference.md)。 

