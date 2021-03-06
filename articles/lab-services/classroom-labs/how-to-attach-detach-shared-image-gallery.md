---
title: 在 Azure Lab Services 中連結共用映像庫或中斷其連結 | Microsoft Docs
description: 本文說明如何將共用映像庫連結至 Azure Lab Services 中的教室實驗室。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2020
ms.author: spelluru
ms.openlocfilehash: aef5cd13742c0265851f5ba2918d557b4e1026d0
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592639"
---
# <a name="attach-or-detach-a-shared-image-gallery-in-azure-lab-services"></a>在 Azure Lab Services 中連結共用映像庫或中斷其連結
本文說明如何將共用映像庫連結至實驗室帳戶或中斷其連結。 

## <a name="scenarios"></a>案例
以下是這項功能所支援的幾個案例： 

- 實驗室帳戶管理員會將共用映像庫連結至實驗室帳戶，並將映像上傳至實驗室內容外部的共用映像庫。 然後，實驗室建立者可以使用共用映像庫中的該映像來建立實驗室。 
- 實驗室帳戶管理員會將共用映像庫連結至實驗室帳戶。 實驗室建立者 (講師) 會將其實驗室的自訂映像儲存到共用映像庫。 然後，其他實驗室建立者可以從共用映像庫中選取此映像，為其實驗室建立範本。 

    當映像儲存到共用映像庫時，Azure Lab Services 會將儲存的映像複寫到相同[地理位置](https://azure.microsoft.com/global-infrastructure/geographies/)中的其他可用區域。 其可確保映像適用於相同地理位置的其他區域中建立的實驗室。 將映像儲存到共用映像庫會產生額外成本，包括所有複寫映像的成本。 此成本與 Azure Lab Services 成本分開。 如需共用映像庫定價的詳細資訊，請參閱[共用映像庫 – 計費]( https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#billing)。

    > [!NOTE]
    > Azure Lab Services 支援建立以共用映像庫中**一般化**和**特殊化**映像為基礎的範本 VM。 


## <a name="configure-at-the-time-of-lab-account-creation"></a>在建立實驗室帳戶時設定
當您建立實驗室帳戶時，您可以將共用映像庫連結至實驗室帳戶。 您可以從下拉式清單中選取現有共用映像庫，或建立新的共用映像庫。 若要建立共用映像庫並將其連結至實驗室帳戶，請選取 [新建]，輸入映像庫的名稱，然後按一下 [確定]。 

![在建立實驗室帳戶時設定共用映像庫](../media/how-to-use-shared-image-gallery/new-lab-account.png)

## <a name="configure-after-the-lab-account-is-created"></a>在建立實驗室帳戶後進行設定
建立實驗室帳戶之後，您可以執行下列工作：

- 建立和連結共用映像庫
- 將共用映像庫連結至實驗室帳戶
- 中斷共用映像庫與實驗室帳戶的連結

## <a name="create-and-attach-a-shared-image-gallery"></a>建立和連結共用映像庫
1. 登入 [Azure 入口網站](https://portal.azure.com)。
2. 選取左側功能表上的 [所有服務]。 選取 **DEVOPS** 區段中的 [實驗室服務]。 如果您選取 [實驗室服務] 旁邊的星號 (`*`)，它會新增到左功能表上的 [我的最愛] 區段。 從下一次開始，您可選取 [我的最愛] 下方的 [實驗室服務]。

    ![[所有服務] -> [實驗室服務]](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. 選取您的實驗室帳戶，以查看 [實驗室帳戶] 頁面。 
4. 選取左側功能表上的 [共用映像庫]，然後選取工具列上的 [+ 建立]。  

    ![建立共用映像庫按鈕](../media/how-to-use-shared-image-gallery/new-shared-image-gallery-button.png)
5. 在 [建立共用映像庫] 視窗中，輸入映像庫的**名稱**，然後按一下 [確定]。 

    ![建立共用映像庫視窗](../media/how-to-use-shared-image-gallery/create-shared-image-gallery-window.png)

    Azure Lab Services 會建立共用映像庫，並將其連結至實驗室帳戶。 在此實驗室帳戶中建立的所有實驗室都會擁有已連結共用映像庫的存取權。 

    ![連結的映像庫](../media/how-to-use-shared-image-gallery/image-gallery-in-list.png)

    在底部窗格中，您會在共用映像庫中看到映像。 在這個新映像庫中，沒有任何映像。 當您將映像上傳至映像庫時，您會在此頁面上看到映像。     

    預設會啟用已連結共用映像庫中的所有映像。 您可以在清單中選取映像，然後使用 [啟用選取的映像] 或 [停用選取的映像] 按鈕來啟用或停用選取的映像。

## <a name="attach-an-existing-shared-image-gallery"></a>連結現有的共用映像庫
下列程序說明如何將現有的共用映像庫連結至實驗室帳戶。 

1. 在 [實驗室帳戶] 頁面上，選取左側功能表上的 [共用映像庫]，然後選取工具列上的 [連結]。 

    ![共用映像庫 - 新增按鈕](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. 在 [連結現有的共用映像庫] 頁面上，選取您的共用映像庫，然後選取 [確定]。

    ![選取現有的映像庫](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. 您會看到以下畫面： 

    ![清單中我的映像庫](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    在此範例中，共用映像庫中還沒有任何映像。

    Azure Lab Services 身分識別會當作參與者新增到已連結至實驗室的共用映像庫。 其可讓教師/IT 系統管理員將虛擬機器映像儲存到共用映像庫。 在此實驗室帳戶中建立的所有實驗室都會擁有已連結共用映像庫的存取權。 

    預設會啟用已連結共用映像庫中的所有映像。 您可以在清單中選取映像，然後使用 [啟用選取的映像] 或 [停用選取的映像] 按鈕來啟用或停用選取的映像。 

## <a name="detach-a-shared-image-gallery"></a>中斷共用映像庫的連結
只有一個共用映像庫可以連結至實驗室。 如果您想要連結另一個共用映像庫，請先中斷目前共用映像庫的連結，然後連結新的共用映像庫。 若要中斷共用映像庫與您的實驗室的連結，請選取工具列上的 [中斷連結]，然後確認中斷連結作業。 

![中斷共用映像庫與實驗室帳戶的連結](../media/how-to-use-shared-image-gallery/detach.png)

## <a name="next-steps"></a>後續步驟
若要了解如何將實驗室映像儲存到共用映像庫，或使用共用映像庫中的映像來建立 VM，請參閱[如何使用共用映像庫](how-to-use-shared-image-gallery.md)。

如需一般共用映像庫的詳細資訊，請參閱[共用映像庫](../../virtual-machines/windows/shared-image-galleries.md)。
