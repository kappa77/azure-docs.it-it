---
title: Abilitare l'agente di macchine virtuali nel Centro sicurezza di Azure | Microsoft Docs
description: In questo documento viene illustrato come implementare l'indicazione **Abilita l'agente di macchine virtuali** del Centro sicurezza di Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 12934ad53050d16b89dd5b4175ca19a24d1ec4d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60916279"
---
# <a name="enable-vm-agent-in-azure-security-center"></a>Abilita l'agente di macchine virtuali nel Centro sicurezza di Azure
Per [abilitare la raccolta dati](security-center-enable-data-collection.md), l'agente di macchine virtuali deve essere installato sulle macchine virtuali.  Il Centro sicurezza di Azure consente di visualizzare le VM che richiedono l'agente di macchine virtuali e consiglia di abilitare l'agente di macchine virtuali su queste VM.

Per impostazione predefinita, l'agente di macchine virtuali è installato nelle macchine virtuali distribuite da Azure Marketplace. L'articolo relativo all' [agente di macchine virtuali e relative estensioni, parte 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) fornisce informazioni su come installare l'agente di macchine virtuali.

> [!NOTE]
> Il documento introduce il servizio usando una distribuzione di esempio. Questa non è una guida dettagliata.
>
>

## <a name="implement-the-recommendation"></a>Implementare la raccomandazione
1. Nel pannello **Indicazioni** selezionare **Abilita l'agente di macchine virtuali**.
   ![Abilita l'agente di macchine virtuali][1]
2. Verrà aperto il pannello **L'agente di macchine virtuali non è presente o non risponde**. Questo pannello elenca le VM che richiedono l'agente di macchine virtuali. Seguire le istruzioni nel pannello per installare l'agente.
   ![L'agente di macchine virtuali non è present][2]

## <a name="see-also"></a>Vedere anche 
Per altre informazioni sul Centro sicurezza, vedere gli argomenti seguenti:

* [Impostazione dei criteri di sicurezza nel Centro sicurezza di Azure](tutorial-security-policy.md): informazioni su come configurare i criteri di sicurezza per le sottoscrizioni e i gruppi di risorse di Azure.
* [Gestione delle raccomandazioni di sicurezza nel Centro sicurezza di Azure](security-center-recommendations.md): informazioni sul modo in cui le raccomandazioni semplificano la protezione delle risorse di Azure.
* [Monitoraggio dell'integrità della sicurezza nel Centro sicurezza di Azure](security-center-monitoring.md): informazioni su come monitorare l'integrità delle risorse di Azure.
* [Gestione e risposta agli avvisi di sicurezza nel Centro sicurezza di Azure](security-center-managing-and-responding-alerts.md): informazioni su come gestire e rispondere agli avvisi di sicurezza.
* [Monitoraggio delle soluzioni dei partner con il Centro sicurezza di Azure](security-center-partner-solutions.md) : informazioni su come monitorare lo stato integrità delle soluzioni dei partner.
* [Domande frequenti sul Centro sicurezza di Azure](security-center-faq.md): domande frequenti sull'uso del servizio.
* [Blog sulla sicurezza di Azure](https://blogs.msdn.com/b/azuresecurity/): informazioni e notizie aggiornate sulla sicurezza di Azure.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
