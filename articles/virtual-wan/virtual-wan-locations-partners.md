---
title: Posizioni dei partner della rete WAN virtuale di Azure | Microsoft Docs
description: Questo articolo contiene un elenco dei partner Azure rete WAN virtuale e percorsi di hub.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: f38cd0565b2e90fe0803d8e815c622e22e954a18
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60459847"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Posizioni dei partner e degli hub virtuali della rete WAN virtuale di Azure

Questo articolo fornisce informazioni sulla rete WAN virtuale supportati aree e i partner per la connettività all'Hub virtuale.

La rete WAN virtuale di Azure è un servizio di rete che offre connettività tra succursali ottimizzata e automatizzata tramite Azure. La rete WAN virtuale consente di connettere e configurare i dispositivi delle succursali per la comunicazione con Azure. Questa operazione può essere eseguita manualmente oppure usando provider di dispositivi tramite un partner di rete WAN virtuale. Uso di dispositivi partner consente che la facilità d'uso, semplificare le operazioni di connettività e gestione della configurazione.

La connettività dal dispositivo locale viene stabilita in modo automatico per l'hub virtuale. Un hub virtuale è una rete virtuale gestita da Microsoft. L'hub contiene vari endpoint di servizio per abilitare la connettività dalla rete locale (vpnsite). È possibile avere solo un hub per area.

## <a name="automation"></a>Automazione dei partner di connettività

I dispositivi che si connettono alla rete WAN virtuale di Azure dispongono di un'automazione incorporata per la connessione. Questa è in genere impostata nell'interfaccia utente di gestione del dispositivo (o equivalente), che consente di impostare la gestione della configurazione e della connettività tra il dispositivo VPN della succursale e un endpoint VPN dell'hub virtuale di Azure (gateway VPN).

L'automazione di alto livello seguente è impostata nella console del dispositivo/centro di gestione:

* Autorizzazioni appropriate per il dispositivo per accedere al gruppo di risorse della rete WAN virtuale di Azure
* Caricamento del dispositivo della succursale nella rete WAN virtuale di Azure
* Download automatico di informazioni relative alla connettività di Azure
* Configurazione del dispositivo della succursale locale 

Alcuni partner di connettività possono estendere l'automazione in modo da includere la creazione della rete virtuale dell'hub virtuale di Azure e di un Gateway VPN. Se si vuole approfondire la conoscenza dell'automazione, vedere [Configurare l'automazione - partner WAN](virtual-wan-configure-automation-providers.md).

## <a name="partners"></a>Connettività tramite i partner

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

## <a name="locations"></a>Località

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni sulla rete WAN virtuale, vedere le [domande frequenti sulla rete WAN virtuale](virtual-wan-faq.md).

* Per altre informazioni su come automatizzare la connettività alla rete WAN virtuale di Azure, vedere la sezione relativa ai [partner della rete WAN virtuale - configurate l'automazione](virtual-wan-configure-automation-providers.md).
