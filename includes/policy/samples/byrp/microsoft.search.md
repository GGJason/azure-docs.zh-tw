---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/13/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 929869c1aff759c9ba4c13c736e000bcc53032b4
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83658955"
---
|名稱 |描述 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[將搜尋服務的診斷設定部署至事件中樞](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3d5da587-71bd-41f5-ac95-dd3330c2d58d) |針對搜尋服務部署診斷設定，以在任何缺少此診斷設定的搜尋服務建立或更新時串流至區域事件中樞。 |DeployIfNotExists, Disabled |2.0.0 |[連結](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Search_DeployDiagnosticLog_Deploy_EventHub.json) |
|[將搜尋服務的診斷設定部署至 Log Analytics 工作區](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F08ba64b8-738f-4918-9686-730d2ed79c7d) |針對搜尋服務部署診斷設定，以在任何缺少此診斷設定的搜尋服務建立或更新時串流至區域 Log Analytics 工作區。 |DeployIfNotExists, Disabled |1.0.0 |[連結](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Search_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[應在搜尋服務中啟用診斷記錄](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb4330a05-a843-4bc8-bf9a-cacce50c67f4) |稽核診斷記錄的啟用情形。 這可讓您在發生安全性事件或網路遭到損害時，重新建立活動線索以供調查之用 |AuditIfNotExists, Disabled |2.0.0 |[連結](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Search/Search_AuditDiagnosticLog_Audit.json) |
