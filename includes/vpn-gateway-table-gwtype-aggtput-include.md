---
title: File di inclusione
description: File di inclusione
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/22/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 211935aac56dff8d6e524706c416c126b1a0c3b8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66159332"
---
|**SKU**   | **Tunnel S2S/<br>rete virtuale-rete virtuale** | **P2S<br> Connessioni SSTP** | **P2S<br> Connessioni IKEv2/OpenVPN** | **Benchmark<br>velocità effettiva aggregata** | **BGP** |
|---       | ---        | ---       | ---            | ---       | --- |
|**Basic** | Max. 10    | Max. 128  | Non supportato  | 100 Mbps  | Non supportato|
|**VpnGw1**| Max. 30*   | Max. 128  | Max. 250       | 650 Mbps  | Supportato |
|**VpnGw2**| Max. 30*   | Max. 128  | Max. 500       | 1 Gbps    | Supportato |
|**VpnGw3**| Max. 30*   | Max. 128  | Max. 1000      | 1,25 Gbps | Supportato |


(*) Usare la [WAN virtuale](../articles/virtual-wan/virtual-wan-about.md) se servono più di 30 tunnel VPN S2S.

* Il benchmark della velocità effettiva aggregata si basa sulle misurazioni di più tunnel aggregati tramite un singolo gateway. Il benchmark della velocità effettiva aggregata per un gateway VPN è S2S + P2S combinati. **Se si hanno molte connessioni P2S, si può avere un impatto negativo su una connessione S2S a causa delle limitazioni relative alla velocità effettiva.** Il benchmark della velocità effettiva aggregata non rappresenta una velocità effettiva garantita, perché può variare anche in funzione delle condizioni del traffico Internet e dei comportamenti dell'applicazione.

* Questi limiti di connessione sono separati. Ad esempio, è possibile avere 128 connessioni SSTP e anche 250 connessioni IKEv2 in uno SKU VpnGw1.

* Le informazioni sui prezzi sono disponibili nella pagina [Prezzi](https://azure.microsoft.com/pricing/details/vpn-gateway) .

* Le informazioni sul contratto di servizio sono disponibili nella pagina [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) (Contratto di servizio).

* VpnGw1, VpnGw2 e VpnGw3 sono supportati solo per gateway VPN che usano il modello di distribuzione Resource Manager.

* Lo SKU Basic viene considerato uno SKU legacy. Lo SKU Basic presenta alcune limitazioni in termini di funzionalità. Non è possibile ridimensionare un gateway che utilizza uno SKU Basic a uno dei nuovi SKU del gateway. È invece necessario passare a un nuovo SKU, operazione che comporta l'eliminazione e la ricreazione del gateway VPN. Verificare che la funzionalità necessaria sia supportata prima di utilizzare lo SKU Basic.
