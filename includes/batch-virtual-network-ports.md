---
title: File di inclusione
description: File di inclusione
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 04/10/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 711b662c35b5f8fec96f1edee765696bc1028bf8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127503"
---
### <a name="general-requirements"></a>Requisiti generali

* La rete virtuale deve essere nella stessa sottoscrizione e area dell'account Batch usato per creare il pool.

* Il pool che usa la rete virtuale può avere un massimo di 4096 nodi.

* La subnet specificata per il pool deve disporre di indirizzi IP non assegnati sufficienti per contenere il numero di macchine virtuali usate come destinazione per il pool; questo valore corrisponde alla somma delle proprietà `targetDedicatedNodes` e `targetLowPriorityNodes` del pool. Se la subnet non dispone di sufficienti indirizzi IP non assegnati, il pool alloca parzialmente i nodi di calcolo e si verifica un errore di ridimensionamento. 

* L'endpoint di Archiviazione di Azure deve essere risolto da qualsiasi server DNS personalizzato che fornisce informazioni alla rete virtuale. In particolare, gli URL con formato `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` e `<account>.blob.core.windows.net` devono essere risolvibili. 

Altri requisiti della rete virtuale possono essere diversi a seconda che il pool di Batch sia nella configurazione della macchina virtuale o la configurazione di Servizi cloud. Per le nuove distribuzioni di pool in una rete virtuale, è consigliabile la configurazione della macchina virtuale.

### <a name="pools-in-the-virtual-machine-configuration"></a>Pool nella configurazione della macchina virtuale

**Reti virtuali supportate** - Solo reti virtuali basate su Azure Resource Manager

**ID subnet** - Quando si specifica la subnet usando le API Batch, usare l'*identificatore di risorsa* della subnet. L'identificatore della subnet è nel formato:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.Network/virtualNetworks/{network}/subnets/{subnet}
  ```

**Autorizzazioni** - Controllare se i criteri di sicurezza o i blocchi nel gruppo di risorse o nella sottoscrizione della rete virtuale limitano le autorizzazioni dell'utente per gestire la rete virtuale.

**Ulteriori risorse di rete** - Batch alloca automaticamente le risorse di rete aggiuntive nel gruppo di risorse contenente la rete virtuale. Per ogni 50 nodi dedicati (o ogni 20 nodi per priorità bassa), Batch alloca: 1 gruppo di sicurezza 1 rete (NSG) e 1 indirizzo IP pubblico di bilanciamento del carico. Queste risorse sono limitate in base alle [quote delle risorse](../articles/azure-subscription-service-limits.md) della sottoscrizione. Per pool di grandi dimensioni potrebbe essere necessario richiedere un aumento della quota per una o più di queste risorse.

#### <a name="network-security-groups"></a>Gruppi di sicurezza di rete

La subnet deve consentire le comunicazioni in ingresso dal servizio Batch per essere in grado di pianificare le attività nei nodi di calcolo e le comunicazioni in uscita per comunicare con Archiviazione di Azure o altre risorse. Per i pool nella configurazione della macchina virtuale, Batch aggiunge i gruppi di sicurezza di rete al livello delle interfacce di rete (NIC) collegate alle macchine virtuali. Questi gruppi di sicurezza di rete configurano automaticamente le regole in ingresso e in uscita per consentire il traffico seguente:

* Traffico TCP in ingresso sulle porte 29876 e 29877 da indirizzi IP del ruolo del servizio Batch. 
* Traffico TCP in ingresso sulla porta 22 (nodi di Linux) o sulla porta 3389 (nodi di Windows) per consentire l'accesso remoto. Per alcuni tipi di attività a istanze multiple in Linux (ad esempio MPI), è necessario inoltre consentire il traffico di porta 22 SSH per gli indirizzi IP nella subnet contenente i nodi di calcolo di Batch.
* Traffico in uscita su qualsiasi porta verso la rete virtuale.
* Traffico in uscita su qualsiasi porta verso Internet.

> [!IMPORTANT]
> Prestare attenzione se si modificano o aggiungono regole in ingresso o in uscita nei gruppi di sicurezza di rete configurati per Batch. Se la comunicazione ai nodi di calcolo nella subnet specificata viene negata da un gruppo di sicurezza di rete, il servizio Batch imposta lo stato dei nodi di calcolo su **Non utilizzabile**.

Non occorre specificare i gruppi di sicurezza di rete a livello di subnet perché Batch configura gruppi di sicurezza di rete propri. Tuttavia, se la subnet specificata dispone di gruppi di sicurezza di rete (NSG) e/o di un firewall associati, configurare le regole di sicurezza in ingresso e in uscita, come illustrato nelle tabelle seguenti. Configurare il traffico in ingresso sulla porta 3389 (Windows) o 22 (Linux) solo se è necessario consentire l'accesso remoto alle macchine virtuali del pool provenienti da fonti esterne. Non è necessario per rendere utilizzabili le macchine virtuali del pool. Si noti che è necessario consentire il traffico delle subnet rete virtuale sulla porta 22 per Linux, se si usa determinati tipi di attività a istanze multiple, ad esempio MPI.

**Regole di sicurezza in ingresso**

| Indirizzi IP di origine | Tag del servizio di origine | Porte di origine | Destination | Porte di destinazione | Protocol | Azione |
| --- | --- | --- | --- | --- | --- | --- |
| N/D | `BatchNodeManagement` [Tag di servizio](../articles/virtual-network/security-overview.md#service-tags) | * | Qualsiasi | 29876-29877 | TCP | CONSENTI |
| Utente IP di origine per l'accesso remoto ai nodi di calcolo e/o subnet di nodo di calcolo per attività a istanze multiple Linux, se necessario. | N/D | * | Qualsiasi | 3389 (Windows), 22 (Linux) | TCP | CONSENTI |

**Regole di sicurezza in uscita**

| `Source` | Porte di origine | Destination | Tag del servizio di destinazione | Protocol | Azione |
| --- | --- | --- | --- | --- | --- |
| Qualsiasi | 443 | [Tag di servizio](../articles/virtual-network/security-overview.md#service-tags) | `Storage` (nella stessa area dell'account Batch e della rete virtuale)  | Qualsiasi | CONSENTI |

### <a name="pools-in-the-cloud-services-configuration"></a>Pool nella configurazione di Servizi cloud

**Reti virtuali supportate** - Solo reti virtuali classiche

**ID subnet** - Quando si specifica la subnet usando le API Batch, usare l'*identificatore di risorsa* della subnet. L'identificatore della subnet è nel formato:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.ClassicVirtualNetwork /virtualNetworks/{network}/subnets/{subnet}
  ```

**Autorizzazioni** - L'entità servizio `MicrosoftAzureBatch` deve avere il ruolo di controllo degli accessi in base al ruolo (RBAC) `Classic Virtual Machine Contributor` per la rete virtuale specificata.

#### <a name="network-security-groups"></a>Gruppi di sicurezza di rete

La subnet deve consentire le comunicazioni in ingresso dal servizio Batch per essere in grado di pianificare le attività nei nodi di calcolo e le comunicazioni in uscita per comunicare con Archiviazione di Azure o altre risorse.

Non è necessario specificare un gruppo di sicurezza di rete, perché Batch configura le comunicazioni in ingresso solo da indirizzi IP di Batch verso i nodi del pool. Tuttavia, se alla subnet specificata sono associati gruppi di sicurezza di rete (NSG) e/o un firewall, configurare le regole di sicurezza in ingresso e in uscita, come illustrato nelle tabelle seguenti. Se la comunicazione ai nodi di calcolo nella subnet specificata viene negata da un gruppo di sicurezza di rete, il servizio Batch imposta lo stato dei nodi di calcolo su **Non utilizzabile**.

Configurare il traffico in ingresso sulla porta 3389 per Windows se si desidera consentire l'accesso RDP ai nodi del pool. Non è necessario per rendere utilizzabili i nodi del pool.

**Regole di sicurezza in ingresso**

| Indirizzi IP di origine | Porte di origine | Destination | Porte di destinazione | Protocol | Azione |
| --- | --- | --- | --- | --- | --- |
Qualsiasi <br /><br />Anche se per questa operazione sono richieste in effetti autorizzazioni di tipo "Consenti tutto", il servizio Batch applica una regola ACL a livello di ogni nodo per escludere tutti gli indirizzi IP di servizio non Batch. | * | Qualsiasi | 10100, 20100, 30100 | TCP | CONSENTI |
| Facoltativo, per consentire l'accesso RDP nei nodi di calcolo. | * | Qualsiasi | 3389 | TCP | CONSENTI |

**Regole di sicurezza in uscita**

| `Source` | Porte di origine | Destination | Porte di destinazione | Protocol | Azione |
| --- | --- | --- | --- | --- | --- |
| Qualsiasi | * | Qualsiasi | 443  | Qualsiasi | CONSENTI |
