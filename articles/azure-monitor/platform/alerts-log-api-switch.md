---
title: Passare dalla API degli avvisi di Log Analytics a nuova API degli avvisi di Azure
description: Panoramica di savedSearch legacy basato su API di avviso di Log Analitica e processo per passare le regole di avviso alla nuova API ScheduledQueryRules, con dettagli addressing preoccupazioni dei clienti.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 0e8cb18b3ea4b01db6b373ebbcb55c1e17614319
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399161"
---
# <a name="switch-api-preference-for-log-alerts"></a>Modificare la preferenza della API degli avvisi di Log Alerts

> [!NOTE]
> Contenuto dichiarato applicabile agli utenti solo cloud pubblico Azure e **non** per cloud di Azure per enti pubblici o Azure Cina.  

Fino a poco tempo fa le regole degli avvisi venivano gestite nel portale Microsoft Operations Management Suite (OMS). La nuova esperienza avvisi è stata integrata con diversi servizi in Microsoft Azure, tra cui Log Analytics e abbiamo chiesto di [estendere le regole di avviso dal portale OMS ad Azure](alerts-extend.md). Ma per assicurare un'interruzione minima per i clienti, il processo non ha modificato l'interfaccia programmatica per l'uso- [API degli avvisi di Log Analytics](api-alerts.md) basata su SavedSearch.

Ma ora si annuncia una vera alternativa programmatica di Azure per gli utenti degli avvisi di Log Analytics, [Monitoraggio di Azure - API ScheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), anch'esso riportato nella [fatturazione di Azure - per gli avvisi del log](alerts-unified-log.md#pricing-and-billing-of-log-alerts). Per altre informazioni su come gestire gli avvisi di log usando l'API, vedere [gestione degli avvisi del log usando il modello di risorse di Azure](alerts-log.md#managing-log-alerts-using-azure-resource-template) e [gestione degli avvisi del log con PowerShell](alerts-log.md#managing-log-alerts-using-powershell).

## <a name="benefits-of-switching-to-new-azure-api"></a>Vantaggi del passaggio alla nuova API di Azure

Vi sono numerosi vantaggi relativi alla creazione e gestione degli avvisi tramite [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) rispetto a [API legacy degli avvisi di Log Analytics](api-alerts.md); tra i principali:

- Possibilità di [ricercare log tra le aree di lavoro](../log-query/cross-workspace-query.md) nelle regole di avviso e di raggiungere le risorse esterne come ad esempio le aree di lavoro di Log Analytics o anche le app di Application Insights
- Quando vengono usati più campi per creare raggruppamenti nelle query, attraverso [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) l'utente può specificare il campo da aggregare nel portale di Azure
- Gli avvisi di log creati tramite l'[API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) possono avere come periodo definito un massimo di 48 ore e recuperare i dati per un periodo più lungo rispetto al passato
- Creare regole di avviso in un unico passaggio come un'unica risorsa senza la necessità di creare tre livelli di risorse come con [legacy API degli avvisi di Log Analytics](api-alerts.md)
- Unica interfaccia programmatica per tutte le varianti degli avvisi di log basati su query in Azure - la nuova [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) può essere usata per gestire le regole per Log Analytics, oltre che per Application Insights
- Gestire gli avvisi di log usando [i cmdlet di Powershell](alerts-log.md#managing-log-alerts-using-powershell)
- Tutte le nuove funzionalità di avviso e il futuro sviluppo dell'avviso del log saranno disponibili solo tramite la nuova [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Processo di commutazione dall'API legacy degli avvisi relativi ai log

Gli utenti sono liberi di usare [API legacy degli avvisi di Log Analytics](api-alerts.md) o la nuova [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Le regole di avviso create per ciascuna API saranno *rese gestibili solo dalla stessa API*, oltre che dal portale di Azure. Per impostazione predefinita, monitoraggio di Azure continuerà a usare [legacy API di avviso di Log Analitica](api-alerts.md) per la creazione di qualsiasi nuova regola di avviso dal portale di Azure per aree di lavoro esistente di Log Analitica. Come [annunciata una nuova area di lavoro di Log creato in corrispondenza o dopo il 1 giugno 2019](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/) -userà automaticamente nuovi [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) , includendo predefinito nel portale di Azure.

Gli effetti della commutazione delle preferenze API scheduledQueryRules sono i seguenti:

- Tutte le interazioni eseguite per la gestione degli avvisi relativi ai log tramite interfacce programmatiche ora devono essere create tramite [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Per altre informazioni, vedere, [uso di esempio tramite il modello di risorse di Azure](alerts-log.md#managing-log-alerts-using-azure-resource-template) e [uso di esempio tramite PowerShell](alerts-log.md#managing-log-alerts-using-powershell)
- Le nuove regole degli avvisi relativi ai log create nel portale di Azure verranno create solo tramite [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) e consentiranno agli utenti di usare [funzionalità aggiuntive della nuova API](#benefits-of-switching-to-new-azure-api) anche tramite il portale di Azure
- Livello di gravità per le regole di avviso di log verrà spostato dal: *Critico, avviso e informativi*, in *i valori di gravità di 0, 1 e 2*. Insieme all'opzione per creare o aggiornare le regole di avviso con gravità 4.

Il processo di estensione degli avvisi da [API legacy degli avvisi di Log Analytics](api-alerts.md) non comporta la modifica della definizione, della query o della configurazione degli avvisi. Le regole di avviso e al monitoraggio sono interessato e gli avvisi non verrà interrotta o essere bloccati, durante o dopo il cambio. L'unica modifica è una modifica delle preferenze di API e l'accesso per le regole con una nuova API.

> [!NOTE]
> Una volta che un utente sceglie per passare una preferenza per il nuovo [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), non è possibile scegliere Indietro o ripristinare l'uso delle versioni precedenti [legacy API degli avvisi di Log Analitica](api-alerts.md).

Qualsiasi cliente che desideri passare volontariamente al nuovo [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) e bloccare l'uso dalla [API legacy degli avvisi relativi a Log Analytics](api-alerts.md) può farlo effettuando una chiamata PUT sull'API per cambiare tutte le regole relative agli avvisi associate all'area di lavoro specifica di Log Analytics.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

In caso di un corpo della richiesta contenente il codice JSON seguente.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

È possibile accedere all'API anche dalla riga di comando di PowerShell tramite [ARMClient](https://github.com/projectkudu/ARMClient), uno strumento da riga di comando open source che semplifica la chiamata all'API di Azure Resource Manager. Come mostrato di seguito, nella chiamata PUT di esempio viene usato lo strumento ARMclient per cambiare tutte le regole relative agli avvisi associate all'area di lavoro specifica di Log Analytics.

```powershell
$switchJSON = '{"scheduledQueryRulesEnabled": "true"}'
armclient PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $switchJSON
```

Se la commutazione di tutte le regole di avviso nell'area di lavoro Log Analytics per usare i nuovi [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) viene completata correttamente, verrà fornita la risposta seguente.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Gli utenti possono anche controllare lo stato corrente dell'area di lavoro Log Analytics e vedere se è stata o no modificata per il solo uso di [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Per controllare, gli utenti possono eseguire una chiamata GET all'API seguente.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Per eseguire il codice precedente usando la riga di comando di PowerShell con lo strumento [ARMClient](https://github.com/projectkudu/ARMClient), vedere l'esempio seguente.

```powershell
armclient GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Se l'area di lavoro Log Analytics specificata è stata modificata solo per l'uso di [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), allora la risposta JSON sarà come la seguente.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
Altrimenti, se l'area di lavoro di Log Analytics specificata non è ancora stata modificata per il solo uso di [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), allora la risposta JSON sarà come la seguente.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Passaggi successivi

- Informazioni sugli [Avvisi del log - Monitoraggio di Azure](alerts-unified-log.md).
- Informazioni su come creare [avvisi del log in Avvisi di Azure](alerts-log.md).
- Altre informazioni sull'[esperienza di Avvisi di Azure](../../azure-monitor/platform/alerts-overview.md).
