---
title: 關於 Azure Site Recovery 中的容錯移轉和容錯回復
description: 深入瞭解 Azure Site Recovery 中的容錯移轉和 failable。
ms.topic: conceptual
ms.date: 12/24/2019
ms.openlocfilehash: d9b54f3c452212e12419a5ffd67b116c8660308d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "79281804"
---
# <a name="about-on-premises-disaster-recovery-failoverfailback"></a>關於內部部署嚴重損壞修復容錯移轉/容錯回復

本文概述使用[Azure Site Recovery](site-recovery-overview.md)在內部部署機器到 Azure 的災難復原期間的容錯移轉和容錯回復。

## <a name="recovery-stages"></a>復原階段

Site Recovery 中的容錯移轉和容錯回復有四個階段：

- **第1階段：從內部部署環境容錯移轉**：設定內部部署機器的 Azure 複寫之後，當您的內部部署網站停機時，會將這些機器容錯移轉至 azure。 容錯移轉之後，系統會從複寫的資料建立 Azure VM。
- **第2階段：重新保護 Azure vm**：在 azure 中，您會重新保護 azure vm，使其開始複寫回到內部部署網站。 內部部署 VM （如果有的話）會在重新保護期間關閉，以協助確保資料的一致性。
- **第3階段：從 Azure 進行容錯移轉**：當您的內部部署網站再次以正常方式執行時，您會執行另一個容錯移轉，這次會將 Azure vm 容錯回復到您的內部部署網站。 您可以容錯回復到您容錯回復的原始位置，或移至其他位置。
- **第4階段：** 重新保護內部部署機器：在容錯回復之後，再次啟用將內部部署機器複寫至 Azure 的工作。

## <a name="failover"></a>容錯移轉

您會執行容錯移轉，做為商務持續性和嚴重損壞修復（BCDR）策略的一部分。

- 在 BCDR 策略的第一個步驟中，您會持續將內部部署機器複寫到 Azure。 使用者會存取在內部部署來源機器上執行的工作負載和應用程式。
- 如果需要，例如如果內部部署發生中斷，您會將複寫電腦故障到 Azure。 Azure Vm 是使用複寫的資料所建立。
- 針對商務持續性，使用者可以繼續存取 Azure Vm 上的應用程式。

容錯移轉是兩個階段的活動：

- **容錯移轉**：使用選取的復原點建立和啟動 Azure VM 的容錯移轉。
- **認可**：在容錯移轉之後，您會在 Azure 中驗證 VM：
    - 接著，您可以將容錯移轉認可到選取的復原點，或為認可選取不同的點。
    - 認可容錯移轉之後，就無法變更復原點。


## <a name="connect-to-azure-after-failover"></a>在容錯移轉之後連線至 Azure

若要連線到使用 RDP/SSH 在容錯移轉之後建立的 Azure Vm，有一些需求。

**容錯移轉** | **位置** | **動作**
--- | --- | ---
**Azure VM （Windows （）** | 在容錯移轉前的內部部署機器上 | 透過**網際網路存取**：啟用 RDP。 請確定已針對 [**公用**] 新增 TCP 和 UDP 規則，且在 [ **Windows 防火牆** > ] [**允許的應用程式**] 中的所有設定檔允許 RDP。<br/><br/> 透過**站對站 VPN 存取**：在電腦上啟用 RDP。 檢查 [ **Windows 防火牆** -> ] [**允許的應用程式和功能**] 中的 [**網域] 和 [私人**] 網路允許 RDP。<br/><br/>  確定作業系統的 SAN 原則已設為 **OnlineAll**。 [深入了解](https://support.microsoft.com/kb/3031135)。<br/><br/> 觸發容錯移轉時，請確定 VM 上沒有任何暫止的 Windows 更新。 Windows Update 可能會在您故障時啟動，而且您將無法登入 VM，直到更新完成為止。
**執行 Windows 的 Azure VM** | 在容錯移轉後的 Azure VM 上 |  [新增 VM 的公用 IP 位址](https://aka.ms/addpublicip)。<br/><br/> 已故障 over 的 VM 上的網路安全性群組規則（以及它所連線的 Azure 子網）必須允許 RDP 埠的連入連線。<br/><br/> 勾選 [開機診斷]**** 以確認 VM 的螢幕擷取畫面。 如果您無法連線，請檢查 VM 是否正在執行，並檢查[疑難排解秘訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。
**執行 Linux 的 Azure VM** | 在容錯移轉前的內部部署機器上 | 確定 VM 上的安全殼層服務已設定為在系統開機時自動啟動。<br/><br/> 請檢查防火牆規則是否允許 SSH 連線。
**執行 Linux 的 Azure VM** | 在容錯移轉後的 Azure VM 上 | 已容錯移轉的 VM 上的網路安全性群組規則 (以及它所連線的 Azure 子網路) 必須允許 SSH 連接埠的連入連線。<br/><br/> [新增 VM 的公用 IP 位址](https://aka.ms/addpublicip)。<br/><br/> 勾選 [開機診斷]**** 以檢視 VM 的螢幕擷取畫面。<br/><br/>

## <a name="types-of-failover"></a>容錯移轉的類型

Site Recovery 提供不同的容錯移轉選項。

**容錯移轉** | **詳細資料** | **復原** | **工作流程**
--- | --- | --- | ---
**測試容錯移轉** | 用來執行演練，以驗證您的 BCDR 策略，而不會遺失任何資料或停機時間。| 在 Azure 中建立 VM 的複本，而不影響進行中的複寫或生產環境。 | 1. 在單一 VM 上或在復原方案中的多個 Vm 上執行測試容錯移轉。<br/><br/> 2. 選取要用於測試容錯移轉的復原點。<br/><br/> 3. 選取 azure VM 在容錯移轉後建立時將位於其中的 azure 網路。 網路僅用於測試容錯移轉。<br/><br/> 4. 確認演練是否如預期般運作。 Site Recovery 會在演練期間自動清除在 Azure 中建立的 Vm。
**規劃的容錯移轉-Hyper-v**  | 通常用於計畫的停機時間。<br/><br/> 來源 Vm 已關閉。 在起始容錯移轉之前，會同步處理最新的資料。 | 計畫的工作流程零資料遺失。 | 1. 規劃停機時間維護視窗並通知使用者。<br/><br/> 2. 讓使用者面向的應用程式離線。<br/><br/> 3. 使用最新的復原點來起始規劃的容錯移轉。 如果電腦未關閉或發生錯誤，則不會執行容錯移轉。<br/><br/> 4. 容錯移轉之後，請檢查複本 Azure VM 在 Azure 中是否作用中。<br/><br/> 5. 認可容錯移轉以完成。 認可動作會刪除所有復原點。
**容錯移轉-Hyper-v** | 通常會在發生未規劃的中斷或主要網站無法使用時執行。<br/><br/> 選擇性地關閉 VM，並在起始容錯移轉之前同步處理最後的變更。  | 應用程式的資料遺失最少。 | 1. 起始您的 BCDR 計畫。 <br/><br/> 2. 起始容錯移轉。 指定在觸發容錯移轉之前，Site Recovery 是否應該關閉 VM 並同步處理/複寫最新的變更。<br/><br/> 3. 您可以故障切換至數個復原點選項，如下表中摘要說明。<br/><br/> 如果您未啟用關閉 VM 的選項，或 Site Recovery 無法將它關閉，則會使用最新的復原點。<br/>即使無法關閉電腦，容錯移轉也會執行。<br/><br/> 4. 容錯移轉之後，您會檢查複本 Azure VM 在 Azure 中是否為作用中。<br/> 如有需要，您可以從 [保留] 視窗中選取不同的復原點24小時。<br/><br/> 5. 認可容錯移轉以完成。 認可動作會刪除所有可用的復原點。
**容錯移轉-VMware** | 通常會在發生未規劃的中斷或主要網站無法使用時執行。<br/><br/> 選擇性地指定 Site Recovery 應該嘗試觸發 VM 的關機，並在起始容錯移轉之前同步處理和複寫最後的變更。  | 應用程式的資料遺失最少。 | 1. 起始您的 BCDR 計畫。 <br/><br/> 2. 從 Site Recovery 起始容錯移轉。 指定在執行容錯移轉之前，Site Recovery 是否應該嘗試觸發 VM 關機和同步處理。<br/> 即使無法關閉電腦，容錯移轉也會執行。<br/><br/> 3. 容錯移轉之後，請確認複本 Azure VM 在 Azure 中為作用中。 <br/>如有需要，您可以從 [72 小時] 的 [保留] 視窗中選取不同的復原點。<br/><br/> 5. 認可容錯移轉以完成。 認可動作會刪除所有復原點。<br/> 對於 Windows Vm，Site Recovery 會在容錯移轉期間停用 VMware 工具。

## <a name="failover-processing"></a>容錯移轉處理

在某些情況下，容錯移轉需要額外的處理，這大約需要 8 到 10 分鐘才會完成。 您可能會發現下列各項的測試容錯移轉時間比較久：

* 執行的行動服務版本早於 9.8 的 VMware VM。
* 實體伺服器。
* VMware Linux VM。
* 視同實體伺服器受到保護的 Hyper-V VM。
* 未啟用 DHCP 服務的 VMware VM。
* 沒有下列開機驅動程式的 VMware VM：storvsc、vmbus、storflt、intelide、atapi。

## <a name="recovery-point-options"></a>復原點選項

在容錯移轉期間，您可以選取多個復原點選項。

**選項** | **詳細資料**
--- | ---
**最新（最低 RPO）** | 此選項提供最低的復原點目標（RPO）。 它會先處理所有已傳送至 Site Recovery 服務的資料，以建立每個 VM 的復原點，然後再進行容錯移轉。 此復原點會在觸發容錯移轉時，將所有資料複寫到 Site Recovery。
**最近處理的**  | 此選項會將 Vm 故障通過 Site Recovery 所處理的最新復原點。 若要查看特定 VM 的最新復原點，請檢查 VM 設定中的**最新復原點**。 此選項提供低 RTO (復原時間目標)，因為無須花費時間處理未處理的資料。
**最新應用程式一致** |  如果已啟用應用程式一致復原點，此選項會將 Vm 損毀修復至 Site Recovery 所處理的最新應用程式一致復原點。 檢查 VM 設定中的最新復原點。
**已處理最新多個 VM** | 此選項適用於一或多部 VM 已啟用多部 VM 一致性的復原計劃。 啟用這項設定的 VM 會容錯移轉至最新的一般多部 VM 一致復原點。 方案中的任何其他 Vm 會故障切換至最新處理的復原點。
**最新多 VM 應用程式一致** |  此選項適用於一或多部 VM 已啟用多部 VM 一致性的復原計劃。 屬於最新一般多部 VM 應用程式一致復原點的複寫群組容錯移轉一部分的 VM。 對於最新應用程式一致復原點進行的其他 VM 容錯移轉。
**Custom** | 使用此選項可將特定 VM 故障切換至特定的復原點。 此選項不適用於復原方案。

> [!NOTE]
> 復原點無法遷移至另一個復原服務保存庫。

## <a name="reprotectionfailback"></a>重新保護/容錯回復

容錯移轉至 Azure 之後，複寫的 Azure Vm 會處於未受保護的狀態。

- 在容錯回復至內部部署網站的第一個步驟中，您必須啟動複寫至內部部署的 Azure Vm。 重新保護程式會視您故障切換的機器類型而定。
- 在機器從 Azure 複寫到內部部署之後，您可以執行從 Azure 到內部部署網站的容錯移轉。
- 在機器再次執行內部部署後，您可以啟用複寫，使其複寫至 Azure 以進行嚴重損壞修復。

容錯回復的運作方式如下：

- 若要容錯回復，VM 必須至少有一個復原點才能進行容錯回復。 在復原方案中，方案中的所有 Vm 都至少需要一個復原點。
- 建議您使用**最新**的復原點來容錯回復（這是一個損毀一致的點）。
    - 有一個應用程式一致的復原點選項。 在此情況下，單一 VM 會復原至其最新可用的應用程式一致復原點。 針對具有複寫群組的復原方案，每個複寫群組都會復原到其一般可用的復原點。
    - 應用程式一致復原點可能會在一段時間後，而且資料可能會遺失。
- 從 Azure 容錯移轉至內部部署網站時，Site Recovery 會關閉 Azure Vm。 當您認可容錯移轉時，Site Recovery 會移除 Azure 中已容錯回復的 Azure Vm。


## <a name="vmwarephysical-reprotectionfailback"></a>VMware/實體重新保護/容錯回復

若要將 VMware 機器和實體伺服器從 Azure 重新保護並容錯回復到內部部署，您需要容錯回復基礎結構，而且有一些需求。

- **Azure 中的暫時處理序伺服器**︰若要從 Azure 容錯回復，您可以設定 Azure VM 作為處理序伺服器，以處理來自 Azure 的複寫。 容錯回復完成後，您可以刪除此 VM。
- **VPN 連線**：若要容錯回復，您必須要有從 Azure 網路到內部部署網站的 VPN 連線 (或 ExpressRoute)。
- **個別的主要目標伺服器**︰根據預設，隨組態伺服器安裝的主要目標伺服器 (在內部部署 VMware VM 上) 會處理容錯回復。 如果您需要容錯回復大量流量，請針對此用途設定個別的內部部署主要目標伺服器。
- **容錯回復原則**︰若要複寫回到內部部署網站，您需要容錯回復原則。 當您建立從內部部署至 Azure 的複寫原則後，此原則會自動建立。
    - 此原則會自動與設定伺服器產生關聯。
    - 您無法編輯此原則。
    - 原則值： RPO 閾值-15 分鐘;復原點保留期-24 小時;應用程式一致快照集頻率-60 分鐘。

深入瞭解 VMware/實體重新保護和容錯回復：
- [查看](vmware-azure-reprotect.md#before-you-begin)重新保護和容錯回復的其他需求。
- 在 Azure 中[部署](vmware-azure-prepare-failback.md#deploy-a-process-server-in-azure)進程伺服器。
- [部署](vmware-azure-prepare-failback.md#deploy-a-separate-master-target-server)個別的主要目標伺服器。

當您將 Azure Vm 重新保護至內部部署時，您可以指定要容錯回復到原始位置或替代位置。

- **原始位置**復原：這會從 Azure 容錯回復到相同的來源內部部署機器（如果有的話）。 在此案例中，只會將變更複寫回內部部署。
- **替代位置**復原：如果內部部署機器不存在，您可以從 Azure 容錯回復至替代位置。 當您將 Azure VM 重新保護至內部部署時，會建立內部部署機器。 完整的資料複寫會從 Azure 到內部部署進行。 --請[檢查](concepts-types-of-failback.md)位置容錯回復的需求和限制。



## <a name="hyper-v-reprotectionfailback"></a>Hyper-v 重新保護/容錯回復

若要將 Hyper-v Vm 從 Azure 重新保護和容錯回復至內部部署：

- 您只能使用儲存體帳戶來容錯回復 Hyper-v Vm 複寫。 不支援使用受控磁片複寫的 Hyper-v Vm 容錯回復。
- 內部部署 Hyper-v 主機（或已使用的 System Center VMM）應連線至 Azure。
- 您會執行規劃從 Azure 到內部部署的容錯回復。
- 不需要為 Hyper-v VM 容錯回復設定特定元件。
- 在規劃的容錯移轉期間，您可以選取在容錯回復之前同步處理資料的選項：
    - **在容錯移轉前同步處理資料**：此選項會將虛擬機器的停機時間降到最低，因為它會同步機器而不關閉它們。
        - 第1階段：取得 Azure VM 的快照集，並將其複製到內部部署 Hyper-v 主機。 機器會繼續在 Azure 中執行。
        - 第2階段：關閉 Azure VM，如此一來，就不會發生任何新的變更。 最後一組差異變更會傳送到內部部署伺服器，並啟動內部部署 VM。
    - **僅在容錯移轉期間同步處理資料**：此選項的速度較快，因為我們預期大部分的磁片都已變更，因此不會執行總和檢查碼計算。 它會下載磁碟。 如果 VM 已在 Azure 中執行一段時間（一個月或更多），或如果已刪除內部部署 VM，建議您使用此選項。

[深入瞭解](hyper-v-azure-failback.md)hyper-v 重新保護和容錯回復。

當您將 Azure Vm 重新保護至內部部署時，您可以指定要容錯回復到原始位置或替代位置。

- **原始位置**復原：這會從 Azure 容錯回復到相同的來源內部部署機器（如果有的話）。 在此案例中，您會選取先前程式中所述的其中一個同步處理選項。
- **替代位置**復原：如果內部部署機器不存在，您可以從 Azure 容錯回復至替代位置。 當您將 Azure VM 重新保護至內部部署時，會建立內部部署機器。 使用此選項時，建議您選取在容錯移轉前同步處理資料的選項。
- [檢查](hyper-v-azure-failback.md)位置容錯回復的需求和限制。


容錯回復至內部部署網站之後，您可以啟用**反向**複寫，以開始將 VM 複寫至 Azure，並完成迴圈。




## <a name="next-steps"></a>後續步驟
- 故障切換[特定的 VMware vm](vmware-azure-tutorial-failover-failback.md)
- 故障切換[特定的 Hyper-v vm](hyper-v-azure-failover-failback-tutorial.md)。
- [建立](site-recovery-create-recovery-plans.md)復原方案。
- 損毀修復復原[方案中的 vm](site-recovery-failover.md)。
- [準備](vmware-azure-failback.md)VMware 重新保護和容錯回復。
- 容錯回復[Hyper-v vm](hyper-v-azure-failback.md)。

