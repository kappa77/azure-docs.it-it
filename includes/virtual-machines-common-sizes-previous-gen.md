---
title: File di inclusione
description: File di inclusione
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 05/16/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 464c7bcb510a2f6ab80fb11d722c241ec51a1b16
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391403"
---
Questa sezione vengono fornite informazioni su versioni precedenti di dimensioni delle macchine virtuali. Queste dimensioni possono ancora essere usate, ma sono disponibili generazioni più recenti. 

## <a name="f-series"></a>Serie F

La serie F è basata sul processore Intel Xeon® E5-2673 v3 (Haswell) a 2,4 GHz che può raggiungere velocità di clock fino a 3,1 GHz con la tecnologia Intel Turbo Boost 2.0. Si tratta delle stesse prestazioni CPU della serie Dv2 di VM.  

Le VM serie F sono un'ottima scelta per i carichi di lavoro che richiedono CPU più veloci, ma che non necessitano della stessa memoria o risorsa di archiviazione temporanea per ogni vCPU.  Carichi di lavoro come server Web, analisi, giochi ed elaborazione batch trarranno vantaggio dal valore della serie F.

ACU: 210 - 250

Archiviazione Premium:  Non supportato

Memorizzazione nella cache archiviazione Premium:  Non supportato

| Dimensione         | vCPU | Memoria: GiB | GiB di archiviazione temp (unità SSD) | Velocità effettiva massima di archiviazione temporanea: IOPS/MBps di lettura/MBps di scrittura | Velocità effettiva massima del disco dati: IOPS | Schede di interfaccia di rete max/larghezza di banda della rete prevista (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 4/4 x 500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 8/8 x 500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000 / 187 / 93                                         | 16/16 x 500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000 / 375 / 187                                        | 32/32 x 500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000 / 750 / 375                                        | 64/64x500                       | 8 / 12000           |

## <a name="fs-series-sup1sup"></a>Serie Fs <sup>1</sup>

La serie Fs offre tutti i vantaggi della serie F, oltre all'archiviazione Premium.

ACU: 210 - 250

Archiviazione Premium:  Supportato

Memorizzazione nella cache archiviazione Premium:  Supportato

| Dimensione | vCPU | Memoria: GiB | GiB di archiviazione temp (unità SSD) | Numero massimo di dischi dati | Velocità effettiva massima di archiviazione temporanea e memorizzazione nella cache: IOPS/MBps (dimensione della cache espressa in GiB) | Velocità effettiva massima del disco senza memorizzazione nella cache: IOPS/MBps | Schede di interfaccia di rete max/larghezza di banda della rete prevista (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4000 / 32 (12) |3200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8000 / 64 (24) |6400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16000 / 128 (48) |12800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32000 / 256 (96) |25600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64000 / 512 (192) |51200 / 768 |8 / 12000 |

MBps = 10^6 byte al secondo e GiB = 1024^3 byte.

<sup>1</sup> La massima velocità effettiva del disco (IOPS o MBps) possibile con una macchina virtuale serie Fs può essere limitata dal numero, dalle dimensioni e dallo striping dei dischi collegati.  Per maggiori dettagli, vedere [Progettazione per prestazioni elevate](../articles/virtual-machines/windows/premium-storage-performance.md).  

## <a name="ls-series"></a>Serie Ls

La serie Ls offre fino a 32 vCPU e usa il [processore Intel® Xeon® della famiglia E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). La serie Ls offre le stesse prestazioni CPU della serie G/GS ed è dotato di 8 GiB di memoria per ogni vCPU.

La serie Ls non supporta la creazione di una cache locale per aumentare il numero di operazioni di I/O al secondo che è possibile ottenere tramite i dischi dati permanenti. L'elevata velocità effettiva e IOPS del disco locale rende le VM serie Ls ideali per gli archivi NoSQL, ad esempio Apache Cassandra e MongoDB che replicano dati tra più macchine virtuali per eseguire il salvataggio permanente in caso di errore di una singola macchina virtuale.

ACU: 180-240

Archiviazione Premium:  Supportato

Memorizzazione nella cache archiviazione Premium:  Non supportato
 
| Dimensione          | vCPU | Memoria (GiB) | Spazio di archiviazione temp (GiB) | Numero massimo di dischi dati | Velocità effettiva massima di archiviazione temporanea (IOPS/Mbps) | Velocità effettiva massima del disco non memorizzato nella cache (IOPS/MBps) | Schede di interfaccia di rete max/larghezza di banda della rete prevista (Mbps) | 
|----------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4  | 32  | 678   | 16 | 20000 / 200 | 5000 / 125  | 2 / 4000  | 
| Standard_L8s   | 8  | 64  | 1388 | 32 | 40000 / 400 | 10000 / 250 | 4 / 8000  | 
| Standard_L16s  | 16 | 128 | 2807 | 64 | 80000 / 800 | 20000 / 500 | 8 / 16000 | 
| Standard_L32s&nbsp;<sup>1</sup> | 32   | 256  | 5630 | 64   | 160000 / 1600   | 40000 / 1000     | 8 / 20000 | 

La massima velocità effettiva del disco possibile con le macchine virtuali serie Ls può essere limitata dal numero, dalle dimensioni e dallo striping dei dischi collegati. Per maggiori dettagli, vedere [Progettazione per prestazioni elevate](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>1</sup> L'istanza è isolata e prevede hardware dedicato per un singolo cliente.

## <a name="nvv2-series-preview"></a>Serie NVv2 (anteprima)

**Indicazioni sulle dimensioni più recente**: [Serie NVv3 (anteprima)](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu#nvv2-series-preview)

Le macchine virtuali serie NVv2 sono basate su GPU [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) e sulla tecnologia NVIDIA GRID con CPU Intel Broadwell. Queste macchine virtuali sono specifiche per applicazioni con grafica accelerata per GPU e desktop virtuali in cui i clienti vogliono visualizzare i propri dati, simulare risultati da visualizzare, lavorare in CAD o eseguire il rendering e lo streaming di contenuti. Queste macchine virtuali possono anche eseguire carichi di lavoro con precisione singola, ad esempio per la codifica e il rendering. Le macchine virtuali NVv2 supportano Archiviazione Premium e offrono una memoria di sistema (RAM) doppia rispetto alla serie NV precedente.  

Ogni GPU nelle istanze NVv2 viene fornita con una licenza GRID. Questa licenza consente di usare un'istanza NV come workstation virtuale per un singolo utente oppure consente a 25 utenti simultanei di connettersi a una macchina virtuale per uno scenario di applicazione virtuale.

| Dimensione | vCPU | Memoria: GiB | GiB di archiviazione temp (unità SSD) | GPU | Memoria GPU: GiB | Numero massimo di dischi dati | Schede di interfaccia di rete max | Workstation virtuali | Applicazioni virtuali | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6s_v2 |6 |112 |320 | 1 | 8 | 12 | 4 | 1 | 25 |
| Standard_NV12s_v2 |12 |224 |640 | 2 | 16 | 24 | 8 | 2 | 50 |
| Standard_NV24s_v2 |24 |448 |1280 | 4 | 32 | 32 | 8 | 4 | 100 |

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0 - A4 che usa l'interfaccia della riga di comando e PowerShell

Nel modello di distribuzione classico, alcuni nomi di dimensioni VM sono leggermente diversi in PowerShell e nell'interfaccia della riga di comando:

* Standard_A0 è ExtraSmall
* Standard_A1 è Small
* Standard_A2 è Medium
* Standard_A3 è Large
* Standard_A4 è ExtraLarge
