---
title: 適用于 Azure Functions 的 Azure 安全性基準
description: 適用于 Azure Functions 的 Azure 安全性基準
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 040eeda3edc8aa1165915a157cb7e1bdd1594740
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796495"
---
# <a name="azure-security-baseline-for-azure-functions"></a>適用于 Azure Functions 的 Azure 安全性基準

適用于 Azure Functions 的 Azure 安全性基準包含可協助您改善部署之安全性狀態的建議。

此服務的基準取自[Azure 安全性基準測試版本 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview)，其中提供有關如何在 Azure 上使用最佳作法指引來保護雲端解決方案的建議。

如需詳細資訊，請參閱[Azure 安全性基準總覽](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)。

## <a name="network-security"></a>網路安全性

*如需詳細資訊，請參閱[安全性控制：網路安全性](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)。*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1：在您的虛擬網路上使用網路安全性群組或 Azure 防火牆來保護資源

**指引**：將您的 Azure Functions 應用程式與 Azure 虛擬網路整合。 在高階方案中執行的函式應用程式與 Azure App Service 中的 web 應用程式具有相同的裝載功能，其中包括「VNet 整合」功能。  Azure 虛擬網路可讓您將許多 Azure 資源（例如 Azure Functions）放在非網際網路可路由網路中。

- [如何整合函式與 Azure 虛擬網路](https://docs.microsoft.com/azure/azure-functions/functions-create-vnet)

- [瞭解 Azure Functions 和 Azure App Service 的 Vnet 整合](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2：監視並記錄 Vnet、子網和 Nic 的設定和流量

**指導**方針：使用 Azure 資訊安全中心並遵循網路保護建議，協助保護與您的 Azure Functions 應用程式相關的網路資源和網路設定。

如果搭配使用網路安全性群組（Nsg）與 Azure Functions 的執行，請啟用 NSG 流量記錄，並將記錄傳送到 Azure 儲存體帳戶以進行流量審核。 您也可以將 NSG 流量記錄傳送到 Log Analytics 工作區，並使用「分析」來提供 Azure 雲端中流量的深入解析。 流量分析的一些優點是能夠將網路活動視覺化，並識別作用點、識別安全性威脅、瞭解流量模式，以及找出網路錯誤配置。

- [瞭解 Azure 資訊安全中心所提供的網路安全性](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

- [如何啟用 NSG 流量記錄](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [如何啟用和使用流量分析](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="13-protect-critical-web-applications"></a>1.3：保護重要的 web 應用程式

**指導**方針：若要在生產環境中完全保護您的 Azure 函式端點，您應該考慮執行下列其中一個函數應用層級安全性選項：
- 開啟函數應用程式的 App Service 驗證/授權，
- 使用 Azure API 管理（APIM）來驗證要求，或
- 將您的函數應用程式部署至 Azure App Service 環境。

此外，請確定您的生產 Azure Functions 已停用遠端偵錯程式。 此外，跨原始來源資源分享（CORS）不應允許所有網域存取您的 Azure 函數應用程式。 僅允許必要網域與您的 Azure Function 應用程式互動。

請考慮將 Azure Web 應用程式防火牆（WAF）部署為網路設定的一部分，以額外檢查傳入流量。 啟用診斷設定以進行 WAF，並將記錄內嵌至儲存體帳戶、事件中樞或 Log Analytics 工作區。 

- [如何保護生產環境中的 Azure 函數端點](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#secure-an-http-endpoint-in-production)

- [如何部署 Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4：拒絕與已知惡意 IP 位址的通訊

**指引**：在與您的函式應用程式相關聯的虛擬網路上啟用 Ddos 保護標準，以防範 ddos 攻擊。 使用 Azure 資訊安全中心整合式威脅情報來拒絕與已知惡意或未使用的公用 IP 位址的通訊。
此外，設定前端閘道（例如 Azure Web 應用程式防火牆），以驗證所有傳入要求並篩選出惡意流量。 Azure Web 應用程式防火牆可以藉由檢查輸入的 Web 流量來封鎖 SQL 插入、跨網站腳本、惡意程式碼上傳和 DDoS 攻擊，協助保護您的 Azure 函數應用程式。 引進 WAF 需要 App Service 環境或使用私用端點（預覽）。 在搭配生產工作負載使用之前，請確定私人端點已不再處於（預覽）狀態。

- [Azure Functions 網路功能選項](https://docs.microsoft.com/azure/azure-functions/functions-networking-options)

- [Azure Functions Premium 方案](https://docs.microsoft.com/azure/azure-functions/functions-scale#premium-plan)

- [App Service 環境簡介](https://docs.microsoft.com/azure/app-service/environment/intro)

- [App Service 環境的網路考量](https://docs.microsoft.com/azure/app-service/environment/network-info)

- [如何設定 DDoS 保護](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

- [如何部署 Azure 防火牆](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [瞭解 Azure 資訊安全中心整合式威脅情報](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

- [瞭解 Azure 資訊安全中心的彈性網路強化](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

- [瞭解 Azure 資訊安全中心及時網路存取控制](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

- [使用 Azure Functions 的私用端點](https://docs.microsoft.com/azure/app-service/networking/private-endpoint)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="15-record-network-packets-and-flow-logs"></a>1.5：記錄網路封包和流量記錄

**指導**方針：如果搭配使用網路安全性群組（nsg）與 Azure Functions 的執行，請啟用網路安全性群組流量記錄，並將記錄傳送到儲存體帳戶以進行流量審核。 您也可以將流量記錄傳送到 Log Analytics 工作區，並使用流量分析來提供 Azure 雲端中流量的深入解析。 流量分析的一些優點是能夠將網路活動視覺化，並識別作用點、識別安全性威脅、瞭解流量模式，以及找出網路錯誤配置。

- [如何啟用 NSG 流量記錄](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [如何啟用和使用流量分析](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

- [如何啟用網路監看員](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6：部署以網路為基礎的入侵偵測/入侵預防系統（IDS/IPS）

**指引**.. 設定前端閘道（例如 Azure Web 應用程式防火牆）來驗證所有傳入的要求，並篩選出惡意流量。 Azure Web 應用程式防火牆可以藉由檢查輸入的 Web 流量來封鎖 SQL 插入、跨網站腳本、惡意程式碼上傳和 DDoS 攻擊，協助保護您的函數應用程式。 引進 WAF 需要 App Service 環境或使用私用端點（預覽）。 在搭配生產工作負載使用之前，請確定私人端點已不再處於（預覽）狀態。

或者，有多個 marketplace 選項，如 Barracuda WAF for Azure，其可在包含 IDS/IPS 功能的 Azure Marketplace 上取得。

- [Azure Functions 網路功能選項](https://docs.microsoft.com/azure/azure-functions/functions-networking-options)

- [Azure Functions Premium 方案](https://docs.microsoft.com/azure/azure-functions/functions-scale#premium-plan)

- [App Service 環境簡介](https://docs.microsoft.com/azure/app-service/environment/intro)

- [App Service 環境的網路考量](https://docs.microsoft.com/azure/app-service/environment/network-info)

- [瞭解 Azure Web 應用程式防火牆](https://docs.microsoft.com/azure/application-gateway/overview#web-application-firewall)

- [使用 Azure Functions 的私用端點](https://docs.microsoft.com/azure/app-service/networking/private-endpoint)

- [瞭解 Barracuda WAF 雲端服務](https://docs.microsoft.com/azure/app-service/environment/app-service-app-service-environment-web-application-firewall#configuring-your-barracuda-waf-cloud-service)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="17-manage-traffic-to-web-applications"></a>1.7：管理 web 應用程式的流量

**指導**方針：為您的網路設定前端閘道，例如具有端對端 TLS 加密的 Azure Web 應用程式防火牆。 引進 WAF 需要 App Service 環境或使用私用端點（預覽）。 在搭配生產工作負載使用之前，請確定私人端點已不再處於（預覽）狀態。

- [Azure Functions 網路功能選項](https://docs.microsoft.com/azure/azure-functions/functions-networking-options)

- [Azure Functions Premium 方案](https://docs.microsoft.com/azure/azure-functions/functions-scale#premium-plan)

- [App Service 環境簡介](https://docs.microsoft.com/azure/app-service/environment/intro)

- [App Service 環境的網路考量](https://docs.microsoft.com/azure/app-service/environment/network-info)

- [瞭解 Azure Web 應用程式防火牆](https://docs.microsoft.com/azure/application-gateway/overview#web-application-firewall)

- [如何透過入口網站使用應用程式閘道設定端對端 TLS](https://docs.microsoft.com/azure/application-gateway/end-to-end-ssl-portal)

- [使用 Azure Functions 的私用端點](https://docs.microsoft.com/azure/app-service/networking/private-endpoint)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：將網路安全性規則的複雜性和系統管理負擔降至最低

**指引**：使用虛擬網路服務標籤來定義網路安全性群組或 Azure 防火牆上的網路存取控制。 建立安全性規則時，您可以使用服務標籤取代特定的 IP 位址。 藉由在規則的適當 [來源] 或 [目的地] 欄位中指定服務標籤名稱（例如，AzureAppService），您可以允許或拒絕對應服務的流量。 Microsoft 會管理服務標籤所包含的位址前置詞，並隨著位址變更自動更新服務標記。

- [如需使用服務標記的詳細資訊](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9：維護網路裝置的標準安全性設定

**指引**：針對與您的 Azure Functions 相關的網路設定，定義及執行標準安全性設定。 使用 "Microsoft" 和 "Microsoft" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行 Azure Functions 的網路設定。 您也可以利用 Azure Functions 的內建原則定義，例如：
- CORS 不應允許每項資源存取函式應用程式
- 函式應用程式應只可經由 HTTPS 存取
- 您的函式應用程式應使用最新的 TLS 版本

您也可以使用 Azure 藍圖，在單一藍圖定義中封裝金鑰環境成品（例如 Azure Resource Manager 範本、角色型存取控制（RBAC）和原則），以簡化大規模的 Azure 部署。 您可以輕鬆地將藍圖套用至新的訂用帳戶、環境，以及透過版本控制來微調控制和管理。

- [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [如何建立 Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="110-document-traffic-configuration-rules"></a>1.10：檔流量設定規則

**指導**方針：如果搭配使用網路安全性群組（nsg）與 Azure Functions 的執行，請使用 nsg 的標籤以及與網路安全性和流量相關的其他資源。 針對個別的 NSG 規則，請使用 [描述] 欄位來指定允許進出網路流量的任何規則的商務需求和（或）持續時間（等）。

使用與標記相關的任何內建 Azure 原則定義，例如「需要標記和其值」，以確保所有資源都是以標籤建立，並通知您現有的未標記資源。

您可以使用 Azure PowerShell 或 Azure CLI，根據其標記來查閱或執行資源的動作。

- [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自動化工具來監視網路資源設定並偵測變更

**指引**：使用 Azure 活動記錄來監視網路資源設定，以及偵測與您的 Azure Functions 部署相關之網路設定和資源的變更。 在 Azure 監視器中建立警示，以在重大網路設定或資源的變更發生時觸發。 

- [如何查看和取出 Azure 活動記錄事件](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

- [如何在 Azure 監視器中建立警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="logging-and-monitoring"></a>記錄和監視

*如需詳細資訊，請參閱[安全性控制：記錄和監視](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)。*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1：使用已核准的時間同步處理來源

**指引**： Microsoft 會維護用於 Azure 資源的時間來源，例如記錄中的時間戳記 Azure Functions。

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2：設定中央安全性記錄管理

**指引**.. 針對控制平面的審核記錄，啟用 Azure 活動記錄診斷設定，並將記錄傳送到 Log Analytics 工作區、azure 事件中樞或 azure 儲存體帳戶進行封存。 使用 Azure 活動記錄資料，您可以判斷在 Azure 資源的控制平面層級執行的任何寫入作業（PUT、POST、DELETE）的「內容、物件和時間」。

Azure Functions 也提供與 Azure 應用程式 Insights 的內建整合，以監視功能。 Application Insights 會收集記錄、效能和錯誤資料。 它會自動偵測效能異常，並包含強大的分析工具，可協助您診斷問題，並瞭解如何使用您的函式。

如果您在 Azure 函數應用程式中有內建的自訂安全性/審核記錄，請啟用診斷設定 "FunctionAppLogs"，並將記錄傳送到 Log Analytics 工作區、Azure 事件中樞或 Azure 儲存體帳戶進行封存。 

（選擇性）您可以啟用和麵板上的資料，以 Azure Sentinel 或協力廠商 SIEM。 

- [如何啟用 Azure 活動記錄的診斷設定](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

- [如何使用 Azure 應用程式 Insights 設定 Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)

- [如何啟用 Azure Functions 的診斷設定（使用者產生的記錄）](https://docs.microsoft.com/azure/azure-functions/functions-monitor-log-analytics)

- [如何上架 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：啟用 Azure 資源的審核記錄

**指引**.. 針對控制平面的審核記錄，啟用 Azure 活動記錄診斷設定，並將記錄傳送到 Log Analytics 工作區、azure 事件中樞或 azure 儲存體帳戶進行封存。 使用 Azure 活動記錄資料，您可以判斷在 Azure 資源的控制平面層級執行的任何寫入作業（PUT、POST、DELETE）的「內容、物件和時間」。

如果您在 Azure 函數應用程式中有內建的自訂安全性/審核記錄，請啟用診斷設定 "FunctionAppLogs"，並將記錄傳送到 Log Analytics 工作區、Azure 事件中樞或 Azure 儲存體帳戶進行封存。 

- [如何啟用 Azure 活動記錄的診斷設定](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

- [如何啟用 Azure Functions 的診斷設定（使用者產生的記錄）](https://docs.microsoft.com/azure/azure-functions/functions-monitor-log-analytics)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4：從作業系統收集安全性記錄

**指導**方針：不適用;此指導方針適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="25-configure-security-log-storage-retention"></a>2.5：設定安全性記錄儲存體保留

**指引**：在 Azure 監視器中，根據貴組織的合規性規定，設定與您的 Azure Functions 應用程式相關聯之 log Analytics 工作區的記錄保留週期。

- [如何設定記錄檔保留參數](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="26-monitor-and-review-logs"></a>2.6：監視和審核記錄

**指導**方針：啟用 Azure 活動記錄診斷設定，以及 Azure Functions 應用程式的診斷設定，並將記錄傳送到 Log Analytics 工作區。 在 Log Analytics 中執行查詢來搜尋字詞、識別趨勢、分析模式，以及根據收集到的資料提供許多其他深入解析。

啟用 Azure Functions 應用程式的 Application Insights，以收集記錄、效能和錯誤資料。 您可以在 Azure 入口網站中，查看 Application Insights 所收集的遙測資料。

如果您在 Azure 函數應用程式中有內建的自訂安全性/審核記錄，請啟用診斷設定 "FunctionAppLogs"，並將記錄傳送到 Log Analytics 工作區、Azure 事件中樞或 Azure 儲存體帳戶進行封存。 

（選擇性）您可以啟用和麵板上的資料，以 Azure Sentinel 或協力廠商 SIEM。 

- [如何啟用 Azure 活動記錄的診斷設定](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

- [如何啟用 Azure Functions 的診斷設定](https://docs.microsoft.com/azure/azure-functions/functions-monitor-log-analytics)

- [如何使用 Azure 應用程式 Insights 設定 Azure Functions 並查看遙測資料](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)

- [如何上架 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7：啟用異常活動的警示

**指導**方針：啟用 Azure 活動記錄診斷設定，以及 Azure Functions 應用程式的診斷設定，並將記錄傳送到 Log Analytics 工作區。 在 Log Analytics 中執行查詢來搜尋字詞、識別趨勢、分析模式，以及根據收集到的資料提供許多其他深入解析。 您可以根據您的 Log Analytics 工作區查詢來建立警示。

啟用 Azure Functions 應用程式的 Application Insights，以收集記錄、效能和錯誤資料。 您可以查看 Application Insights 收集的遙測資料，並在 Azure 入口網站內建立警示。

（選擇性）您可以啟用和麵板上的資料，以 Azure Sentinel 或協力廠商 SIEM。 

- [如何啟用 Azure 活動記錄的診斷設定](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

- [如何啟用 Azure Functions 的診斷設定](https://docs.microsoft.com/azure/azure-functions/functions-monitor-log-analytics)

- [如何啟用 Azure Functions 的 Application Insights](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#enable-application-insights-integration)

- [如何在 Azure 中建立警示](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

- [如何上架 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="28-centralize-anti-malware-logging"></a>2.8：集中反惡意程式碼記錄

**指導**方針：不適用;Azure Functions 應用程式不會處理或產生與反惡意程式碼相關的記錄。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="29-enable-dns-query-logging"></a>2.9：啟用 DNS 查詢記錄

**指導**方針：不適用;Azure Functions 應用程式不會處理或產生使用者可存取的 DNS 相關記錄。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="210-enable-command-line-audit-logging"></a>2.10：啟用命令列審核記錄

**指導**方針：不適用;此指導方針適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

## <a name="identity-and-access-control"></a>身分識別與存取控制

*如需詳細資訊，請參閱[安全性控制：身分識別和存取控制](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)。*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：維護系統管理帳戶的清查

**指導**方針： AZURE ACTIVE DIRECTORY （AD）具有必須明確指派且可查詢的內建角色。 使用 Azure AD PowerShell 模組來執行臨機操作查詢，以探索屬於系統管理群組成員的帳戶。 

- [如何使用 PowerShell 取得 Azure AD 中的目錄角色](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [如何使用 PowerShell 在 Azure AD 中取得目錄角色的成員](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="32-change-default-passwords-where-applicable"></a>3.2：變更預設密碼（若適用）

**指導**方針： Azure Functions 的控制平面存取是透過 AZURE ACTIVE DIRECTORY （AD）來控制。 Azure AD 沒有預設密碼的概念。

您可以透過數種方式來控制資料平面存取，包括授權金鑰、網路限制，以及驗證 AAD 身分識別。 連線到您 Azure Functions HTTP 端點的用戶端會使用授權金鑰，而且可以隨時重新產生。 預設會針對新的 HTTP 端點產生這些金鑰。

函式應用程式可使用多個部署方法，其中一些可以利用一組產生的認證。 檢查將用於您應用程式的部署方法。

- [保護 HTTP 端點](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#secure-an-http-endpoint-in-production)

- [如何取得及重新產生授權金鑰](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#obtaining-keys)

- [Azure Functions 中的部署技術](https://docs.microsoft.com/azure/azure-functions/functions-deployment-technologies)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用專用的系統管理帳戶

**指導**方針：使用專用的系統管理帳戶來建立標準操作程式。 使用 Azure 資訊安全中心的身分識別與存取管理來監視系統管理帳戶的數目。

此外，為了協助您追蹤專用的系統管理帳戶，您可以使用來自 Azure 資訊安全中心或內建 Azure 原則的建議，例如：應該從您的訂用帳戶移除具有擁有者許可權之已淘汰帳戶的多個擁有者

- [如何使用 Azure 資訊安全中心來監視身分識別和存取（預覽）](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

- [如何使用 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4：搭配 Azure Active Directory 使用單一登入（SSO）

**指導**方針：盡可能使用 Azure Active Directory SSO，而不是設定個別的獨立認證來存取函數應用程式的資料。 使用 Azure 資訊安全中心身分識別和存取管理建議。 使用 App Service 驗證/授權功能，為您的 Azure Functions 應用程式執行單一登入。

- [瞭解 Azure Functions 中的驗證和授權](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization#identity-providers)

- [瞭解使用 Azure AD 的 SSO](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：針對所有以 Azure Active Directory 為基礎的存取使用多重要素驗證

**指引**：啟用 AZURE ACTIVE DIRECTORY （AD）多重要素驗證（MFA），並遵循 Azure 資訊安全中心身分識別和存取管理建議。

- [如何在 Azure 中啟用 MFA](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

- [如何監視 Azure 資訊安全中心內的身分識別和存取](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6：使用專用電腦（特殊許可權存取工作站）進行所有系統管理工作

**指導**方針：使用特殊許可權存取工作站（PAW）搭配設定用來登入和設定 Azure 資源的多重要素驗證（MFA）。

- [瞭解特殊許可權存取工作站](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [如何在 Azure 中啟用 MFA](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7：從系統管理客戶紀錄和警示可疑活動

**指引**：使用 AZURE ACTIVE DIRECTORY （AD） PRIVILEGED IDENTITY MANAGEMENT （PIM）來產生記錄，並在環境中發生可疑或不安全的活動時發出警示。

此外，您可以使用 Azure AD 風險偵測來查看警示和報告有風險的使用者行為。

- [如何部署 Privileged Identity Management （PIM）](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

- [瞭解 Azure AD 風險偵測](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8：僅從核准的位置管理 Azure 資源

**指導**方針：使用名為「位置」的條件式存取，只允許從 IP 位址範圍或國家/地區的特定邏輯群組存取 Azure 入口網站。

- [如何在 Azure 中設定命名位置](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指引**：使用 AZURE ACTIVE DIRECTORY （AD）做為您 Azure Functions 應用程式的集中驗證和授權系統。 Azure AD 使用強式加密來保護待用資料和傳輸中的資料。 Azure AD 也會 salts、雜湊並安全地儲存使用者認證。

- [如何設定 Azure Functions 應用程式以使用 Azure AD 登入](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad)

- [如何建立和設定 AAD 實例](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期審查並協調使用者存取

**指引**： AZURE ACTIVE DIRECTORY （AD）提供可協助您探索過時帳戶的記錄檔。 此外，您可以使用 Azure 身分識別存取審查來有效率地管理群組成員資格、企業應用程式的存取權，以及角色指派。 您可以定期檢查使用者存取權，以確保只有適當的使用者可以繼續進行存取。 

- [瞭解 Azure AD 報告](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

- [如何使用 Azure 身分識別存取評論](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11：監視嘗試存取停用的帳戶

**指引**：使用 AZURE ACTIVE DIRECTORY （AD）做為 Azure 函式應用程式的集中驗證和授權系統。 Azure AD 使用強式加密來保護待用資料和傳輸中的資料。 Azure AD 也會 salts、雜湊並安全地儲存使用者認證。

您可以存取 Azure AD 登入活動、audit 和風險事件記錄檔來源，讓您能夠與 Azure Sentinel 或協力廠商 SIEM 整合。

若要簡化此程式，您可以建立 Azure AD 使用者帳戶的診斷設定，並將 audit 記錄和登入記錄傳送到 Log Analytics 工作區。 您可以在 Log Analytics 中設定所需的記錄警示。

- [如何設定 Azure Functions 應用程式以使用 Azure AD 登入](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad)

- [如何將 Azure 活動記錄整合到 Azure 監視器](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [如何在面板上 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12：帳戶登入行為偏差的警示

**指引**：使用 AZURE ACTIVE DIRECTORY （AD）做為您 Azure Functions 應用程式的集中驗證和授權系統。 如需控制平面（Azure 入口網站）上的帳戶登入行為偏差，請使用 Azure Active Directory （AD） Identity Protection 和風險偵測功能，為偵測到與使用者身分識別相關的可疑動作設定自動回應。 您也可以將資料內嵌到 Azure Sentinel，以進行進一步的調查。

- [如何觀看 Azure AD 有風險的登入](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [如何設定和啟用身分識別保護風險原則](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

- [如何上架 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13：提供 Microsoft 在支援案例期間存取相關的客戶資料

**指導**方針：目前無法使用;客戶加密箱目前不支援 Azure Functions。

- [客戶加密箱支援的服務清單](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

## <a name="data-protection"></a>資料保護

*如需詳細資訊，請參閱[安全性控制：資料保護](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)。*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：維護機密資訊的清查

**指引**：使用標記來協助追蹤儲存或處理敏感資訊的 Azure 資源。

- [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔離儲存或處理敏感資訊的系統

**指引**：在開發、測試和生產環境中，執行不同的訂用帳戶和/或管理群組。 Azure 函數應用程式應該以虛擬網路（VNet）/子網區隔，並適當地標記。

您也可以使用私人端點來執行網路隔離。 Azure 私用端點是一種網路介面，可讓您私下且安全地連線至 Azure 私用連結所支援的服務（例如： Azure Functions 應用程式 HTTPs 端點）。 私人端點會使用您 VNet 中的私人 IP 位址，有效地將服務帶入您的 VNet。 在高階方案中執行的函式應用程式的私人端點處於（預覽）狀態。 在搭配生產工作負載使用之前，請確定私人端點已不再處於（預覽）狀態。

- [如何建立額外的 Azure 訂用帳戶](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [如何建立管理群組](https://docs.microsoft.com/azure/governance/management-groups/create)

- [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Azure Functions 網路功能選項](https://docs.microsoft.com/azure/azure-functions/functions-networking-options)

- [Azure Functions Premium 方案](https://docs.microsoft.com/azure/azure-functions/functions-scale#premium-plan)

- [瞭解私用端點](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)

- [使用 Azure Functions 的私用端點](https://docs.microsoft.com/azure/app-service/networking/private-endpoint)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3：監視並封鎖未經授權的機密資訊傳輸

**指引**：在網路周邊部署自動化工具，監視未經授權的機密資訊傳輸，並在警示資訊安全性專業人員時封鎖這類傳輸。 

Microsoft 會管理 Azure Functions 的基礎結構，並已實行嚴格的控制，以避免遺失或公開客戶資料。

- [瞭解 Azure 中的客戶資料保護](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：不適用

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密傳輸中的所有機密資訊

**指引**：在 Azure 函式應用程式的 Azure 入口網站的 [平臺功能：網路： SSL] 底下，啟用 [僅限 HTTPs] 設定，並將 [最低 TLS 版本] 設定為1.2。

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用 active discovery 工具來識別敏感性資料

**指導**方針：目前無法使用;Azure Functions 目前無法使用資料識別、分類和遺失防護功能。 標記函式應用程式可能會處理敏感資訊，並在必要時執行協力廠商解決方案，以符合規範目的。

針對由 Microsoft 管理的基礎平臺，Microsoft 會將所有客戶內容視為機密，並移至絕佳的長度，以防範客戶資料遺失和暴露。 為了確保 Azure 中的客戶資料保持安全，Microsoft 已實行並維護一套強大的資料保護控制和功能。

- [瞭解 Azure 中的客戶資料保護](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6：使用 Azure RBAC 來控制資源的存取權

**指引**：使用 AZURE ACTIVE DIRECTORY （AD）角色型存取控制（RBAC）來控制 Azure Function 控制平面（Azure 入口網站）的存取權。 

- [如何在 Azure 中設定 RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7：使用以主機為基礎的資料遺失防護來強制存取控制

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

Microsoft 會管理 Azure Functions 的基礎結構，並已實行嚴格的控制，以避免遺失或公開客戶資料。

- [瞭解 Azure 中的客戶資料保護](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：加密機密資訊待用

**指引**：建立函數應用程式時，您必須建立或連結至支援 Blob、佇列和資料表儲存體的一般用途 Azure 儲存體帳戶。 這是因為函式依賴 Azure 儲存體進行作業，例如管理觸發程式和記錄函數執行。 Azure 儲存體會加密待用儲存體帳戶中的所有資料。 根據預設，資料會使用 Microsoft 管理的金鑰進行加密。 若要進一步控制加密金鑰，您可以提供客戶管理的金鑰來加密 blob 和檔案資料。 這些金鑰必須存在於 Azure Key Vault 中，函數應用程式才能存取儲存體帳戶。

- [瞭解 Azure Functions 的儲存體考慮](https://docs.microsoft.com/azure/azure-functions/storage-considerations)

- [瞭解待用資料的 Azure 儲存體加密](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：對重要 Azure 資源的變更進行記錄和警示

**指引**：使用 Azure 監視器搭配 Azure 活動記錄，以針對生產 Azure 函式應用程式以及其他重要或相關資源進行變更時，建立警示。

- [如何建立 Azure 活動記錄事件的警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="vulnerability-management"></a>弱點管理

*如需詳細資訊，請參閱[安全性控制：弱點管理](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)。*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1：執行自動化弱點掃描工具

**指導**方針：採用 DevSecOps 實務來確保您的 Azure Functions 應用程式安全，並在其生命週期期間維持盡可能安全。 DevSecOps 將貴組織的安全性小組及其功能納入您的 DevOps 實務，讓安全性成為團隊中每個人的責任。

此外，請遵循 Azure 資訊安全中心的建議，以協助保護您的 Azure 函數應用程式。

- [如何將連續安全性驗證新增至您的 CI/CD 管線](https://docs.microsoft.com/azure/devops/migrate/security-validation-cicd-pipeline?view=azure-devops)

- [如何實行 Azure 資訊安全中心弱點評估建議](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="52-deploy-an-automated-operating-system-patch-management-solution"></a>5.2：部署自動化的作業系統修補程式管理解決方案

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3：部署自動化的協力廠商軟體修補程式管理解決方案

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4：比較回溯弱點掃描

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5：使用風險評等程式來排定所發現弱點的補救優先順序

**指導**方針： Microsoft 會在支援 Azure Functions 的基礎系統上執行弱點管理，不過，您可以使用 Azure 資訊安全中心內的建議嚴重性，以及在您的環境內測量風險的安全分數。 您的安全分數是根據您已緩和的資訊安全中心建議數而定。 若要優先處理建議以解決問題，請考慮每一個的嚴重性。

- [安全性建議參考指南](https://docs.microsoft.com/azure/security-center/recommendations-reference)

**Azure 資訊安全中心監視**：是

**責任**：共用

## <a name="inventory-and-asset-management"></a>清查和資產管理

*如需詳細資訊，請參閱[安全性控制：清查和資產管理](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)。*

### <a name="61-use-azure-asset-discovery"></a>6.1：使用 Azure 資產探索

**指引**：使用 Azure Resource Graph 來查詢/探索訂用帳戶內的所有資源（例如計算、儲存體、網路、埠及通訊協定等）。  請確定您的租使用者中有適當（讀取）許可權，並列舉所有 Azure 訂用帳戶以及訂用帳戶內的資源。

雖然可透過 Resource Graph 探索傳統的 Azure 資源，但強烈建議您建立並使用 Azure Resource Manager 的資源。

- [如何使用 Azure Resource Graph 建立查詢](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

- [如何查看您的 Azure 訂用帳戶](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [瞭解 Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="62-maintain-asset-metadata"></a>6.2：維護資產中繼資料

**指引**：將標籤套用至 Azure 資源，讓中繼資料以邏輯方式將其組織成分類法。

- [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：刪除未經授權的 Azure 資源

**指引**.. 適當地使用標記、管理群組和個別訂用帳戶來組織和追蹤 Azure 資源。 定期協調清查，並確保未經授權的資源會及時從訂用帳戶中刪除。

此外，您也可以使用 Azure 原則，對使用下列內建原則定義在客戶訂用帳戶中建立的資源類型施加限制：不允許的資源類型允許的資源類型

- [如何建立額外的 Azure 訂用帳戶](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [如何建立管理群組](https://docs.microsoft.com/azure/governance/management-groups/create)

- [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4：維護已核准 Azure 資源和軟體標題的清查

**指導**方針：定義已核准的 Azure 資源，以及適用于計算資源的已核准軟體。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：未核准的 Azure 資源的監視

**指引**：使用 Azure 原則來限制可在您的訂用帳戶中建立的資源類型。 

使用 Azure Resource Graph 來查詢/探索其訂用帳戶內的資源。  請確定已核准環境中的所有 Azure 資源。 

- [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [如何使用 Azure Graph 建立查詢](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6：監視計算資源內未經核准的軟體應用程式

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7：移除未核准的 Azure 資源和軟體應用程式

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="68-use-only-approved-applications"></a>6.8：僅使用已核准的應用程式

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="69-use-only-approved-azure-services"></a>6.9：僅使用已核准的 Azure 服務

**指引**：使用 Azure 原則來對可使用下列內建原則定義在客戶訂用帳戶中建立的資源類型施加限制：不允許的資源類型允許的資源類型

- [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [如何使用 Azure 原則拒絕特定的資源類型](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="610-implement-approved-application-list"></a>6.10：執行核准的應用程式清單

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11：限制使用者透過腳本與 Azure 資源管理員互動的能力

**指引**：設定 Azure 條件式存取，以限制使用者與 Azure Resource Manager 的互動能力，方法是針對「Microsoft Azure 管理」應用程式設定「封鎖存取」。

- [如何設定條件式存取以封鎖 Azure Resource Manager 的存取](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12：限制使用者在計算資源內執行腳本的能力

**指導**方針：不適用;這項建議適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13：實際或邏輯上隔離高風險應用程式

**指導**方針：對於敏感性或高風險的 Azure 函式應用程式，請執行個別的訂用帳戶和/或管理群組來提供隔離。

將高風險的 Azure 函式應用程式部署至自己的虛擬網路（VNet）。 Azure Functions 中的周邊安全性是透過 Vnet 來達成。 高階計畫或 App Service 環境（ASE）中執行的函式可以與 Vnet 整合。 為您的使用案例選擇最佳的架構。

- [Azure Functions 網路功能選項](https://docs.microsoft.com/azure/azure-functions/functions-networking-options)

- [Azure Functions Premium 方案](https://docs.microsoft.com/azure/azure-functions/functions-scale#premium-plan)

- [App Service 環境的網路考量](https://docs.microsoft.com/azure/app-service/environment/network-info)

- [如何建立外部 ASE](https://docs.microsoft.com/azure/app-service/environment/create-external-ase)

如何建立內部 ASE：

- [https://docs.microsoft.com/azure/app-service/environment/create-ilb-as](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [如何建立具有安全性設定的 NSG](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

## <a name="secure-configuration"></a>安全設定

*如需詳細資訊，請參閱[安全性控制：安全](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)設定。*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：為所有 Azure 資源建立安全設定

**指導**方針：使用 Azure 原則來定義和執行 Azure 函數應用程式的標準安全性設定。 使用 "Microsoft" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行 Azure Functions 應用程式的設定。 您也可以利用內建的原則定義，例如：
- 函式應用程式應該使用受控識別
- 函式應用程式的遠端偵錯應關閉
- 函式應用程式應只可經由 HTTPS 存取

- [如何查看可用的 Azure 原則別名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="72-establish-secure-operating-system-configurations"></a>7.2：建立安全的作業系統設定

**指導**方針：不適用;此指導方針適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：維護安全的 Azure 資源設定

**指引**：使用 azure 原則 [拒絕] 和 [不存在時部署]，在您的 Azure 資源上強制執行安全設定。

- [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [瞭解 Azure 原則效果](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4：維護安全的作業系統設定

**指導**方針：不適用;雖然可以部署內部部署函式，但這是適用于 IaaS 計算資源的指導方針。 部署內部部署功能時，您必須負責環境的安全設定。

- [瞭解內部部署函數](https://docs.microsoft.com/azure/azure-functions/functions-runtime-install)

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全地儲存 Azure 資源的設定

**指引**：在原始檔控制中安全地儲存及管理 ARM 範本和自訂 Azure 原則定義。

- [什麼是基礎結構即程式碼](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code)

- [設計原則作為程式碼工作流程](https://docs.microsoft.com/azure/governance/policy/concepts/policy-as-code)

- [如何將程式碼儲存在 Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos 檔](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="76-securely-store-custom-operating-system-images"></a>7.6：安全地儲存自訂的作業系統映射

**指導**方針：不適用;此指導方針適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="77-deploy-system-configuration-management-tools"></a>7.7：部署系統設定管理工具

**指導**方針：使用內建的 Azure 原則定義以及 "system.web" 命名空間中 Azure 原則別名，以建立自訂原則來警示、審查和強制執行系統組態。 此外，開發處理常式和管線來管理原則例外狀況。

- [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8：部署作業系統的系統建構管理工具

**指導**方針：不適用;此指導方針適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9：執行 Azure 服務的自動化設定監視

**指導**方針：使用內建的 Azure 原則定義以及 "system.web" 命名空間中 Azure 原則別名，以建立自訂原則來警示、審查和強制執行系統組態。 使用 Azure 原則 [audit]、[deny] 和 [deploy if not 存在] 自動強制執行 Azure 資源的設定。

- [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10：為作業系統執行自動化設定監視

**指導**方針：不適用;此指導方針適用于 IaaS 計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="711-manage-azure-secrets-securely"></a>7.11：安全地管理 Azure 秘密

**指引**：搭配使用受控識別與 Azure Key Vault，以簡化及保護雲端應用程式的秘密管理。 受控識別可讓您的函數應用程式向任何支援 Azure AD 驗證的服務進行驗證，包括 Key Vault，而不需要在程式碼中提供任何認證。

- [如何建立 Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

- [如何使用 App Service 和 Azure Functions 的受控身分識別](https://docs.microsoft.com/azure/app-service/overview-managed-identity)

- [如何使用受控識別提供 Key Vault 驗證](https://docs.microsoft.com/azure/key-vault/managed-identity)

- [使用 App Service 和 Azure Functions 的 Key Vault 參考](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全且自動地管理身分識別

**指引**：在 Azure AD 中使用受控識別，為您的 Azure 函式應用程式提供自動管理的身分識別。 受控識別可讓您向任何支援 Azure AD 驗證的服務進行驗證，包括 Key Vault，而您的程式碼中沒有任何認證。

- [如何使用 App Service 和 Azure Functions 的受控身分識別](https://docs.microsoft.com/azure/app-service/overview-managed-identity)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除非預期的認證暴露

**指導**方針：執行認證掃描器來識別程式碼中的認證。 認證掃描器也鼓勵將探索到的認證移至更安全的位置，例如 Azure Key Vault。 

- [如何設定認證掃描器](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="malware-defense"></a>惡意程式碼防禦

*如需詳細資訊，請參閱[安全性控制：惡意程式碼防護](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)。*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1：使用集中管理的反惡意程式碼軟體

**指導**方針：不適用;此指導方針適用于 IaaS 計算資源。

支援 Azure 服務的基礎主機（例如 Azure Functions）會啟用 Microsoft 反惡意程式碼，但不會在客戶內容上執行。

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：預先掃描要上傳至非計算 Azure 資源的檔案

**指導**方針：不適用;這項建議適用于設計用來儲存資料的非計算資源。


**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3：確認反惡意程式碼軟體和簽章已更新

**指導**方針：不適用;這項建議適用于設計用來儲存資料的非計算資源。

支援 Azure 服務的基礎主機（例如 Azure Functions）會啟用 Microsoft 反惡意程式碼，但不會在客戶內容上執行。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

## <a name="data-recovery"></a>資料復原

*如需詳細資訊，請參閱[安全性控制：資料](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery)復原。*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1：確保週期性自動備份

**指引**：使用備份和還原功能來排程應用程式的定期備份。 在高階方案中執行的函式應用程式與 Azure App Service 中的 web 應用程式具有相同的裝載功能，其中包括「備份和還原」功能。

也利用原始檔控制解決方案（例如 Azure Repos 和 Azure DevOps）來安全地儲存和管理您的程式碼。 Azure DevOps Services 利用許多 Azure 儲存體功能，確保發生硬體故障、服務中斷或區域嚴重損壞時的資料可用性。 此外，Azure DevOps 小組會遵循程式來防止資料遭到意外或惡意刪除。

- [在 Azure 中備份應用程式](https://docs.microsoft.com/azure/app-service/manage-backup)

- [瞭解 Azure DevOps 中的資料可用性](https://docs.microsoft.com/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability)

- [如何將程式碼儲存在 Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos 檔](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：執行完整的系統備份，並備份任何客戶管理的金鑰

**指引**：使用備份和還原功能來排程應用程式的定期備份。 在高階方案中執行的函式應用程式與 Azure App Service 中的 web 應用程式具有相同的裝載功能，其中包括「備份和還原」功能。 在 Azure Key Vault 內備份客戶管理的金鑰。

也利用原始檔控制解決方案（例如 Azure Repos 和 Azure DevOps）來安全地儲存和管理您的程式碼。 Azure DevOps Services 利用許多 Azure 儲存體功能，確保發生硬體故障、服務中斷或區域嚴重損壞時的資料可用性。 此外，Azure DevOps 小組會遵循程式來防止資料遭到意外或惡意刪除。

- [在 Azure 中備份應用程式](https://docs.microsoft.com/azure/app-service/manage-backup)

- [如何在 Azure 中備份金鑰保存庫金鑰](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

- [瞭解 Azure DevOps 中的資料可用性](https://docs.microsoft.com/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability)

- [如何將程式碼儲存在 Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos 檔](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：驗證所有備份，包括客戶管理的金鑰

**指導**方針：確保能夠從備份和還原功能定期執行還原。 如果使用其他離線位置來備份您的程式碼，請定期確認能夠執行完整還原。 已備份客戶管理金鑰的測試還原。

- [從備份還原 Azure 中的應用程式](https://docs.microsoft.com/azure/app-service/web-sites-restore)

- [在 Azure 中透過快照集還原應用程式](https://docs.microsoft.com/azure/app-service/app-service-web-restore-snapshots)

- [如何在 Azure 中還原金鑰保存庫金鑰](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：確保備份和客戶管理金鑰的保護

**指引**：備份和還原功能的備份會使用您訂用帳戶中的 Azure 儲存體帳戶。 Azure 儲存體會加密待用儲存體帳戶中的所有資料。 根據預設，資料會使用 Microsoft 管理的金鑰進行加密。 若要進一步控制加密金鑰，您可以提供客戶管理的金鑰來加密儲存體資料。

如果您使用客戶管理的金鑰，請確定已啟用 Key Vault 中的虛刪除，以防止意外或惡意刪除的金鑰。

- [Azure 儲存體待用加密](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)

- [如何在 Key Vault 中啟用虛刪除](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure 資訊安全中心監視**：是

**責任**：共用

## <a name="incident-response"></a>事件回應

*如需詳細資訊，請參閱[安全性控制：事件回應](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)。*

### <a name="101-create-an-incident-response-guide"></a>10.1：建立事件回應指南

**指引**：為您的組織建立事件回應指南。 請確定有寫入的事件回應計畫，可定義人員的所有角色，以及從偵測到事件處理/管理的階段，以進行後續事件審查。

- [如何設定 Azure 資訊安全中心內的工作流程自動化](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

- [建立您自己的安全性事件回應程式的指引](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft 安全性回應中心的事件剖析](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [客戶也可以利用 NIST 的「電腦安全性性」事件處理指南，協助建立自己的事件回應計畫](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：建立事件評分和優先順序程式

**指導**方針：資訊安全中心指派每個警示的嚴重性，以協助您排定應先調查哪些警示。 嚴重性是根據資訊安全中心在尋找中的信心，或用於發出警示的分析，以及導致警示的活動背後有惡意意圖的信賴等級。

此外，請清楚地標示訂閱（例如， 生產、非生產）並建立命名系統，以清楚識別和分類 Azure 資源。

**Azure 資訊安全中心監視**：是

**責任**：共用

### <a name="103-test-security-response-procedures"></a>10.3：測試安全性回應程式

**指導**方針：執行練習以定期測試系統的事件回應功能。 識別弱式點和間距，並視需要修訂計畫。

- [請參閱 NIST 的發行集：適用于 IT 計畫和功能的測試、訓練和練習計劃指南](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全性事件連絡人詳細資料，並設定安全性事件的警示通知

**指導**方針：如果 Microsoft 安全性回應中心（MSRC）發現客戶的資料已由非法或未經授權的合作物件存取，microsoft 將會使用安全性事件連絡人資訊來與您聯繫。  檢查事實後的事件，以確保解決問題。

- [如何設定 Azure 資訊安全中心安全性連絡人](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：將安全性警示納入事件回應系統中

**指引**：使用「連續匯出」功能來匯出您的 Azure 資訊安全中心警示和建議。 「連續匯出」可讓您以手動或持續的方式來匯出警示和建議。 您可以使用 Azure 資訊安全中心資料連線器，將警示串流至 Azure Sentinel。

- [如何設定連續匯出](https://docs.microsoft.com/azure/security-center/continuous-export)

- [如何將警示串流至 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自動回應安全性警示

**指導**方針：使用 Azure 資訊安全中心中的工作流程自動化功能，自動觸發 Logic Apps 的安全性警示和建議的回應。

- [如何設定工作流程自動化和 Logic Apps](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="penetration-tests-and-red-team-exercises"></a>滲透測試和 Red Team 練習

*如需詳細資訊，請參閱[安全性控制：滲透測試和 red team 練習](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)。*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1：定期滲透測試您的 Azure 資源，並確保修復所有重要的安全性結果

**指導**方針：遵循 Microsoft Engagement 規則，確保您的滲透測試不會違反 microsoft 原則。 針對 Microsoft 管理的雲端基礎結構、服務和應用程式，使用 Microsoft 的 Red 小組和即時網站滲透測試策略和執行。

- [Engagement 的滲透測試規則](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Cloud Red 小組](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

## <a name="next-steps"></a>後續步驟

- 請參閱[Azure 安全性基準測試](https://docs.microsoft.com/azure/security/benchmarks/overview)
- 深入瞭解[Azure 安全性基準](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
