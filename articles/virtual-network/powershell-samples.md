---
title: 虛擬網路的 Azure PowerShell 範例
description: 虛擬網路的 Azure PowerShell 範例。
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: de752cdacf17193d5be95b2b9f887938ace2d50f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "74091877"
---
# <a name="azure-powershell-samples-for-virtual-network"></a>虛擬網路的 Azure PowerShell 範例

下表包含 Azure PowerShell 指令碼的連結：

| | |
|----|----|
| [建立多層式應用程式的虛擬網路](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | 建立具有前端和後端子網路的虛擬網路。 傳送到前端子網路的流量會限制為 HTTP，而傳送到後端子網路的流量則限制為 SQL 且連接埠為 1433。 |
| [對等互連兩個虛擬網路](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | 在相同區域中建立和連線兩個虛擬網路。 |
| [透過網路虛擬設備路由傳送流量](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | 建立具有前端和後端子網路的虛擬網路，以及可在兩個子網路間路由傳送流量的 VM。 |
| [篩選輸入和輸出 VM 網路流量](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | 建立具有前端和後端子網路的虛擬網路。 傳送到前端子網路的輸入網路流量限制為 HTTP 和 HTTPS。 不允許從後端子網路傳輸至網際網路的輸出流量。 |
|[設定具有 Basic Load Balancer 的 IPv4 + IPv6 雙重堆疊虛擬網路](./scripts/virtual-network-powershell-sample-ipv6-dual-stack.md)|部署包含兩個 VM 和一個具備 IPv4 與 IPv6 公用 IP 位址的 Azure Basic Load Balancer 的雙重堆疊 (IPv4 + IPv6) 虛擬網路。 |
|[設定具有 Standard Load Balancer 的 IPv4 + IPv6 雙重堆疊虛擬網路](./scripts/virtual-network-powershell-sample-ipv6-dual-stack-standard-load-balancer.md)|部署包含兩個 VM 和一個具備 IPv4 與 IPv6 公用 IP 位址的 Azure Standard Load Balancer 的雙重堆疊 (IPv4 + IPv6) 虛擬網路。 |
