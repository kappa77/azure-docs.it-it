---
title: Disponibilità di risorse per Istanze di Azure Container
description: Disponibilità delle risorse di calcolo e memoria per il servizio Istanze di Azure Container in aree di Azure differenti.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 05/14/2019
ms.author: danlep
ms.openlocfilehash: 64b60178413e470cc7fe9b3991c6fc29b5a0f860
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794309"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Disponibilità di risorse per Istanze di Azure Container nelle aree di Azure

Questo articolo illustra in dettaglio la disponibilità di risorse di calcolo e memoria per Istanze di Azure Container nelle aree di Azure. 

I valori presentati sono le risorse massime disponibili per distribuzione di un [gruppo di contenitori](container-instances-container-groups.md). I valori sono aggiornati al momento della pubblicazione. 

> [!NOTE]
> I gruppi di contenitori con questi limiti di risorse dipendono dell'area di distribuzione. Quando un'area ha un carico elevato, può verificarsi un errore durante la distribuzione delle istanze. Per mitigare tale errore di distribuzione, provare a distribuire le istanze con impostazioni di risorsa inferiori oppure provare a eseguire la distribuzione in un secondo momento.

Per informazioni su quote e altri limiti nelle distribuzioni, vedere [Quote e limiti per Istanze di Azure Container](container-instances-quotas.md).

## <a name="availability---general"></a>Disponibilità - Generale

Per i gruppi di contenitori con Linux e i contenitori basati su Windows Server 2016 [supportati](container-instances-faq.md#what-windows-base-os-images-are-supported) sono disponibili le aree geografiche e le risorse seguenti.

| Località | OS | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Canada centrale, India centrale, Stati Uniti centrali, Asia orientale, Stati Uniti orientali, Stati Uniti orientali 2, Europa settentrionale, Stati Uniti centro-meridionali, Asia sud-orientale, Regno Unito meridionale, Stati Uniti occidentali | Linux | 4 | 16 |
| Europa occidentale, Stati Uniti occidentali 2 | Linux | 4 | 14 |
| Australia orientale, Giappone orientale | Linux | 2 | 8 |
| Stati Uniti centro-settentrionali, India meridionale | Linux | 2 | 3,5 |
| Europa occidentale |  Windows | 4 | 16 |
| Stati Uniti orientali, Stati Uniti occidentali |  Windows | 4 | 14 |
| Australia orientale, Canada centrale, India centrale, Stati Uniti centrali, Asia orientale, Stati Uniti orientali 2, Giappone orientale, Stati Uniti centro-settentrionali, Europa settentrionale, Stati Uniti centro-meridionali, Asia sud-orientale, India meridionale, Regno Unito meridionale, Stati Uniti occidentali 2 |  Windows | 2 | 3,5 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Disponibilità - Distribuzioni di Windows Server 2019 LTSC, 1809 (anteprima)

Per i gruppi di contenitori con contenitori basati su Windows Server 2019 (anteprima) sono disponibili le aree geografiche e le risorse seguenti.

| Località | OS | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Asia sud-orientale, Europa settentrionale, Europa occidentale, Stati Uniti centrali, Stati Uniti orientali, Stati Uniti occidentali, Stati Uniti occidentali 2 |  Windows | 4 | 16 |
| Stati Uniti orientali 2 |  Windows | 2 | 3,5 |


## <a name="availability---virtual-network-deployment-preview"></a>Disponibilità - Distribuzione nella rete virtuale (anteprima)

Per un gruppo di contenitori distribuito in una [rete virtuale di Azure](container-instances-vnet.md) (anteprima) sono disponibili le aree geografiche e le risorse seguenti.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Disponibilità - Risorse della GPU (anteprima)

Per un gruppo di contenitori distribuito con [risorse della GPU](container-instances-gpu.md) (anteprima) sono disponibili le aree geografiche e le risorse seguenti.

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Passaggi successivi

Segnalare al team se si desidera vedere altre aree geografiche o una maggiore disponibilità delle risorse all'indirizzo [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Per informazioni sulla risoluzione dei problemi di distribuzione di Istanze di Container, vedere [Risolvere i problemi di distribuzione di Istanze di Azure Container](container-instances-troubleshooting.md).
