---
title: Facilità di gestione e monitoraggio di SQL Data Warehouse di Azure - Attività di query, utilizzo delle risorse | Microsoft Docs
description: Informazioni sulle funzionalità disponibili per gestire e monitorare Azure SQL Data Warehouse. Usare il portale di Azure e Dynamic Management View (DMV, viste a gestione dinamica) per comprendere l'attività di query e l'utilizzo delle risorse del data warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/12/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: f80c1817d5c0ce79f2dc53f40a2cc4e00dd5c72b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61420967"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>Monitoraggio dell'attività di query e dell'utilizzo delle risorse in Azure SQL Data Warehouse
Azure SQL Data Warehouse offre una ricca esperienza di monitoraggio nel portale di Azure per esplorare informazioni dettagliate sul carico di lavoro del data warehouse. Il portale di Azure è lo strumento consigliato per il monitoraggio del data warehouse, in quanto fornisce periodi di conservazione, avvisi, raccomandazioni, grafici personalizzabili, nonché dashboard di metriche e log configurabili. Il portale consente inoltre di integrare con altri servizi di monitoraggio di Azure, ad esempio Operations Management Suite (OMS) e il monitoraggio di Azure (log) per fornire un'esperienza olistica di monitoraggio non solo il data warehouse, ma anche l'intero analitica di Azure piattaforma per un'esperienza di monitoraggio integrata. Questa documentazione descrive le funzionalità di monitoraggio disponibili per ottimizzare e gestire la piattaforma analitica con SQL Data Warehouse. 

## <a name="resource-utilization"></a>Utilizzo delle risorse 
Le metriche seguenti sono disponibili nel portale di Azure per SQL Data Warehouse. Tali metriche vengono rilevate tramite il [Monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics).

> [!NOTE]
> Livello del nodo della CPU e IO metriche attualmente non riflettono correttamente sull'utilizzo di data warehouse. Queste metriche verranno rimossi in un prossimo futuro come il team migliora il monitoraggio e la risoluzione dei problemi di prestazioni per SQL Data Warehouse. 

| Nome della metrica                           | DESCRIZIONE     | Tipo di aggregazione |
| --------------------------------------- | ---------------- | --------------------------------------- |
| Percentuale CPU                          | Utilizzo della CPU in tutti i nodi per il data warehouse | Massima      |
| Percentuale di I/O di dati                      | Utilizzo di I/O in tutti i nodi per il data warehouse | Massima   |
| Connessioni riuscite                  | Numero di connessioni ai dati riuscite | Totale            |
| Connessioni non riuscite                      | Numero di connessioni al data warehouse non riuscite | Totale            |
| Blocco da parte del firewall                     | Numero di accessi al data warehouse bloccati | Totale            |
| Limite DWU                              | Obiettivo del livello di servizio del data warehouse | Massima   |
| Percentuale DWU                          | Valore massimo tra percentuale di CPU e percentuale di I/O | Massima   |
| Uso DWU                                | Limite DWU * percentuale DWU | Massima   |
| Percentuale dei riscontri nella cache | (Riscontri nella cache/Mancato riscontro nella cache) * 100, dove "riscontri nella cache" è la somma di tutte le occorrenze di segmenti columnstore della cache SSD locale e "mancato riscontro nella cache" è il mancato riscontro di segmenti columnstore nella cache SSD locale sommato tra tutti i nodi | Massima |
| Percentuale della cache utilizzata | (Cache usata/Capacità della cache) * 100, dove "cache usata" è la somma di tutti i byte nella cache SSD locale in tutti i nodi e "capacità della cache" è la somma della capacità di archiviazione della cache SSD locale in tutti i nodi | Massima |
| Percentuale di tempdb locale | Uso di tempdb locale in tutti i nodi di calcolo: i valori vengono generati ogni cinque minuti | Massima |

## <a name="query-activity"></a>Attività di query
Per un'esperienza programmatica durante il monitoraggio di SQL Data Warehouse tramite T-SQL, il servizio fornisce un set di viste a gestione dinamica (DMV). Queste viste sono utili durante la risoluzione dei problemi e l'identificazione dei colli di bottiglia nelle prestazioni con il carico di lavoro.

Per visualizzare l'elenco delle viste a gestione dinamica fornite da SQL Data Warehouse, fare riferimento a questa [documentazione](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Metriche e registrazione diagnostica
Log e metriche possono essere esportati in modo specifico per il monitoraggio di Azure, il [log di monitoraggio di Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) componente ed è possibile accedervi a livello di programmazione tramite [registrare query](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata). La latenza di log per SQL Data Warehouse è di circa 10-15 minuti. Per altre informazioni sui fattori influire sulla latenza, vedere la documentazione seguente.


## <a name="next-steps"></a>Passaggi successivi
Le seguenti guide introduttive descrivono scenari e casi d'uso comuni in cui avviene il monitoraggio e la gestione del data warehouse:

- [Monitorare il carico di lavoro del data warehouse con viste a gestione dinamica](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)

