---
title: Continuità aziendale delle reti virtuali | Microsoft Docs
description: Informazioni sulle operazioni da eseguire in caso di un'interruzione del servizio Azure con impatto sulle reti virtuali di Azure.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: 68a9523dcc9c4dd84399c68fc7e31a692c011487
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523266"
---
# <a name="virtual-network--business-continuity"></a>Rete virtuale - Continuità aziendale

## <a name="overview"></a>Panoramica
Una rete virtuale è una rappresentazione logica della propria rete personalizzata nel cloud. Consente di definire il proprio spazio di indirizzi IP privato e segmentare la rete in subnet. Le reti virtuali vengono usate come limite di attendibilità per ospitare le risorse di calcolo quali macchine virtuali di Azure e servizi cloud (ruoli Web/di lavoro). Una rete virtuale consente la comunicazione IP privata diretta tra le risorse ospitate al suo interno. È possibile collegare una rete virtuale a una rete locale tramite Gateway VPN o ExpressRoute.

Una rete virtuale viene creata nell'ambito di un'area. È possibile *creare* le reti virtuali con stesso spazio indirizzi in due aree diverse (ad esempio, Stati Uniti orientali e Stati Uniti occidentali), ma perché hanno lo stesso spazio degli indirizzi, è non connetterle tra loro. 

## <a name="business-continuity"></a>Continuità aziendale

Le applicazioni potrebbero essere soggette a interruzioni di varia natura. Un'area può essere interessata da un'interruzione totale dovuta a una calamità naturale o da un'emergenza parziale causata da un guasto di più dispositivi o servizi. L'impatto sul servizio di rete virtuale è diverso in ognuna di queste situazioni.

**D: Se si verifica un'interruzione del servizio per un'intera area, come procedere? ad esempio se un'area subisce un'interruzione totale a causa di una calamità naturale? Cosa accade alle reti virtuali ospitate nell'area?**

R: La rete virtuale e le risorse nell'area interessata restano inaccessibili durante il periodo di interruzione del servizio.

![Diagramma della rete virtuale semplice](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**D: Ciò che è possibile creare nuovamente la stessa rete virtuale in un'area diversa?**

R: Le reti virtuali sono risorse piuttosto semplici. Per creare una rete virtuale con lo stesso spazio di indirizzi in un'area diversa, è possibile richiamare le API di Azure. Per ricreare lo stesso ambiente presente nell'area interessata, è necessario eseguire chiamate API per ridistribuire i ruoli Web e di lavoro di Servizi cloud e le macchine virtuali disponibili in precedenza. Se si dispone di connettività locale, ad esempio in una distribuzione ibrida, è necessario distribuire un nuovo servizio Gateway VPN e connettersi alla rete locale.

Per creare una rete virtuale, vedere [Create a virtual network](manage-virtual-network.md#create-a-virtual-network) (Creare una rete virtuale).

**D: Una replica di una rete virtuale in una determinata area potere ricreare in un'altra area anticipo?**

R: Sì, è possibile creare due reti virtuali usando lo stesso spazio di indirizzi IP privato e le risorse nel due diverse aree di anticipo. Se nella rete virtuale si ospitano servizi con connessione Internet, è possibile configurare Gestione traffico in modo da distribuire il traffico a livello geografico nell'area attiva. Tuttavia, non è possibile connettere due reti virtuali con lo stesso spazio degli indirizzi a una rete locale perché ciò comporterebbe problemi di routing. Al momento di un'emergenza e della perdita di una rete virtuale in un'area, è possibile connettere l'altra rete virtuale presente nell'area con il corrispondente spazio degli indirizzi alla rete locale.

Per creare una rete virtuale, vedere [Create a virtual network](manage-virtual-network.md#create-a-virtual-network) (Creare una rete virtuale).

