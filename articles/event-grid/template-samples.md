---
title: Azure Resource Manager 範本範例 - Event Grid | Microsoft Docs
description: 本文提供 GitHub 上適用於 Azure 事件方格的 Azure Resource Manager 範本範例清單。
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 38d8db0bcc504760595fe51b63072f63e785577a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "76720617"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Event Grid 的 Azure Resource Manager 範本

如需要在範本中使用的 JSON 語法和屬性，請參閱 [Microsoft.EventGrid 資源類型](/azure/templates/microsoft.eventgrid/allversions)。 下表包含 Event Grid 的 Azure Resource Manager 範本連結。

| | |
|-|-|
|**Event Grid 訂用帳戶**||
| [使用 WebHook 端點的自訂主題和訂用帳戶](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| 部署 Event Grid 自訂主題。 為使用 WebHook 端點的自訂主題建立訂用帳戶。 |
| [使用 EventHub 端點的自訂主題訂用帳戶](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| 為自訂主題建立「事件方格」訂閱帳戶。 訂用帳戶會使用端點的事件中樞。 |
| [Azure 訂用帳戶或資源群組訂用帳戶](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| 訂閱資源群組或 Azure 訂用帳戶的事件。 在部署期間指定為目標的資源群組為事件來源。 訂用帳戶會使用端點的 WebHook。 |
| [Blob 儲存體帳戶和訂用帳戶](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| 部署 Azure Blob 儲存體帳戶及訂閱該儲存體帳戶的事件。 |
| | |
