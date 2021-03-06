---
title: Panoramica di Monitoraggio di Azure per contenitori | Microsoft Docs
description: Questo articolo descrive Monitoraggio di Azure per contenitori, che consente di monitorare la soluzione relativa alle informazioni dettagliate sui contenitori del servizio Azure Kubernetes, e il valore che offre attraverso il monitoraggio dell'integrità dei cluster del servizio Azure Kubernetes e di Istanze di Container in Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: d6321564672097fbf901d1d33afac9f606fcb63a
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2019
ms.locfileid: "65521828"
---
# <a name="azure-monitor-for-containers-overview"></a>Panoramica di Monitoraggio di Azure per contenitori

Monitoraggio di Azure per i contenitori è una funzionalità progettata per monitorare le prestazioni dei carichi di lavoro dei contenitori distribuiti in Istanze di Azure Container o in cluster Kubernetes gestiti ospitati nel servizio Azure Kubernetes. Il monitoraggio dei contenitori ha un'importanza critica, soprattutto quando si gestisce un cluster di produzione su larga scala con più applicazioni.

Monitoraggio di Azure per contenitori assicura la visibilità sulle prestazioni raccogliendo metriche sulla memoria e sul processore da controller, nodi e contenitori disponibili in Kubernetes tramite l'API per le metriche. Vengono raccolti anche i log dei contenitori.  Dopo aver abilitato il monitoraggio di cluster Kubernetes, metriche e log vengono raccolti automaticamente per l'utente tramite una versione in contenitori dell'agente di Log Analitica per Linux. Le metriche vengono scritti nell'archivio di metriche e i dati di log vengono scritti nell'archivio di log associati i [Log Analitica](../log-query/log-query-overview.md) dell'area di lavoro. 

![Monitoraggio di Azure per l'architettura di contenitori](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Che cosa fornisce Monitoraggio di Azure per contenitori?

Monitoraggio di Azure per contenitori offre un'esperienza di monitoraggio completa tramite diverse funzionalità di monitoraggio di Azure che consente di comprendere le prestazioni e integrità del cluster Kubernetes e i carichi di lavoro contenitore. Con monitoraggio di Azure per contenitori è possibile:

* Identificare i contenitori servizio Azure Kubernetes in esecuzione nel nodo e il relativo uso medio del processore e della memoria, in modo da individuare facilmente i colli di bottiglia delle risorse.
* Identificare l'utilizzo di processori e memoria dei gruppi di contenitori e dei relativi contenitori ospitati in Istanze di Azure Container.  
* Identificare la posizione del contenitore in un controller o in un pod, in modo da visualizzare facilmente le prestazioni complessive del controller o del pod.
* Esaminare l'uso delle risorse dei carichi di lavoro in esecuzione nell'host non correlati ai processi standard che supportano il pod.
* Comprendere il comportamento del cluster con carichi medi e più pesanti. Queste informazioni sono utili per identificare i requisiti di capacità e determinare il carico massimo che può sostenere il cluster. 
* Configurare gli avvisi per ricevere una notifica in modo proattivo o della registrazione durante l'utilizzo di CPU e memoria su nodi o contenitori superano le soglie.  

## <a name="how-do-i-access-this-feature"></a>Come si accede a questa funzionalità?
È possibile accedere a Monitoraggio di Azure per contenitori in due modi: da Monitoraggio di Azure o direttamente dal cluster servizio Azure Kubernetes selezionato. Monitoraggio di Azure, si dispone di una prospettiva globale di tutti i contenitori distribuiti, monitorati e quali non lo sono, consentendo alla ricerca e filtro tra le sottoscrizioni e i gruppi di risorse e quindi analizzare in Monitoraggio di Azure per contenitori dal contenitore selezionato.  In caso contrario, è possibile accedere alla funzionalità direttamente da un contenitore AKS selezionato dalla pagina del servizio contenitore di AZURE.  

![Panoramica dei metodi di accesso a Monitoraggio di Azure per contenitori](./media/container-insights-overview/azmon-containers-experience.png)

Per informazioni sul monitoraggio e la gestione degli host di contenitori Docker e Windows per visualizzare la configurazione, il controllo e l'uso delle risorse, vedere la [soluzione Monitoraggio contenitori](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Passaggi successivi
Per iniziare il monitoraggio del cluster AKS, esaminare [come abilitare il monitoraggio di Azure per contenitori](container-insights-onboard.md) per comprendere i requisiti e i metodi disponibili per abilitare il monitoraggio.  
