---
title: Aumentare le funzionalità e la capacità - Servizio app di Azure | Microsoft Docs
description: Informazioni su come aumentare le prestazioni di un'app nel servizio app di Azure per aggiungere capacità e funzionalità.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 98d3d1f6fc0f2f30196f360811808579dfbab312
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60832105"
---
# <a name="scale-up-an-app-in-azure"></a>Aumentare le prestazioni di un'app in Azure

> [!NOTE]
> Il nuovo livello **PremiumV2** consente di avere CPU più veloci e spazio di archiviazione SSD, nonché di raddoppiare il rapporto tra memoria e core rispetto ai piani tariffari esistenti. I vantaggi relativi alle prestazioni che ne derivano consentono di eseguire le app con un minor numero di istanze e garantiscono quindi un risparmio economico. Per passare al livello **PremiumV2**, vedere [Configure PremiumV2 tier for App Service](app-service-configure-premium-tier.md) (Configurare il livello PremiumV2 per il servizio app).
>

Questo articolo illustra come passare a un piano superiore per un'app nel servizio app di Azure. Sono disponibili due flussi di lavoro per la scalabilità, l'aumento delle prestazioni e l'aumento del numero di istanze. Questo articolo illustra come aumentare le prestazioni.

* [Aumentare le prestazioni](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): è possibile ottenere più CPU, memoria, spazio su disco e altre funzionalità, ad esempio macchine virtuali dedicate, domini e certificati personalizzati, slot di staging, ridimensionamento automatico e altro ancora. È possibile aumentare le prestazioni cambiando il piano tariffario del piano di servizio app a cui appartiene l'app.
* [Aumentare il numero di istanze](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): è possibile incrementare il numero di istanze di macchine virtuali che eseguono l'app.
  È possibile specificare al massimo 20 istanze, in base al piano tariffario. Gli [ambienti del servizio app](environment/intro.md) nel piano **Isolato** aumentano ulteriormente il numero di istanze facendolo passare a 100. Per altre informazioni sull'aumento del numero di istanze, vedere [Ridimensionare il conteggio delle istanze manualmente o automaticamente](../monitoring-and-diagnostics/insights-how-to-scale.md). Questo articolo illustra come usare la scalabilità automatica, ovvero come ridimensionare automaticamente il numero di istanze sulla base di regole e pianificazioni predefinite.

Le impostazioni di scalabilità diventano effettive in pochi secondi e interessano tutte le app nel [piano di servizio app](../app-service/overview-hosting-plans.md).
Non richiedono di modificare il codice o ridistribuire l'applicazione.

Per informazioni sui prezzi e le funzionalità dei singoli piani di servizio app, vedere [Dettagli prezzi del servizio app](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Prima di modificare il livello **Gratuito** di un piano di servizio app, è necessario innanzitutto rimuovere i [limiti di spesa](https://azure.microsoft.com/pricing/spending-limits/) applicati alla sottoscrizione di Azure. Per visualizzare o modificare le opzioni per la sottoscrizione del servizio app di Microsoft Azure, vedere [Sottoscrizioni di Microsoft Azure][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Passare al piano tariffario superiore
1. Accedere al [portale di Azure][portal] dal browser.
2. Nella pagina dell'app del servizio app fare clic su **Tutte le impostazioni** e quindi su **Scale Up** (Aumenta prestazioni).
   
    ![Passare al ridimensionamento di un'app di Azure.][ChooseWHP]
3. Scegliere il livello e quindi fare clic su **Applica**.
   
    Al termine dell'operazione, nella scheda **Notifiche** verrà visualizzata la scritta verde lampeggiante **OPERAZIONE RIUSCITA**.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Risorse correlate alla scalabilità
Se l'app dipende da altri servizi, ad esempio database SQL di Azure o Archiviazione di Azure, è possibile aumentare le prestazioni di tali risorse separatamente. Queste risorse non vengono gestite dal piano di servizio app.

1. In **Informazioni di base** fare clic sul collegamento **Gruppo di risorse**.
   
    ![Aumentare le prestazioni delle risorse correlate all'app di Azure](./media/web-sites-scale/RGEssentialsLink.png)
2. Nella parte **Riepilogo** della pagina **Gruppo di risorse** fare clic sulla risorsa da ridimensionare. La schermata seguente mostra una risorsa database SQL e una risorsa di Archiviazione di Azure.
   
    ![Passare alla pagina del gruppo di risorse per aumentare le prestazioni dell'app di Azure](./media/web-sites-scale/ResourceGroup.png)
3. Per una risorsa database SQL, fare clic su **Impostazioni** > **Piano tariffario** per passare al piano tariffario superiore.
   
    ![Aumentare le prestazioni di back-end del database SQL per l'app di Azure](./media/web-sites-scale/ScaleDatabase.png)
   
    Per l'istanza di database SQL è anche possibile attivare la [replica geografica](../sql-database/sql-database-geo-replication-overview.md) .
   
    Per una risorsa di Archiviazione di Azure, fare clic su **Impostazioni** > **Configurazione** per aumentare le prestazioni delle opzioni di archiviazione.
   
    ![Aumentare le prestazioni dell'account di Archiviazione di Azure usato dall'app di Azure](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Confrontare i piani tariffari
Per informazioni dettagliate, ad esempio sulle dimensioni delle macchine virtuali per ogni piano tariffario, vedere [Informazioni sui prezzi del servizio app](https://azure.microsoft.com/pricing/details/web-sites/).
Per una tabella dei limiti, delle quote e dei vincoli del servizio e per le funzionalità supportate di ogni piano, vedere [Limiti relativi a Servizio app](../azure-subscription-service-limits.md#app-service-limits).

<a name="Next Steps"></a>

## <a name="next-steps"></a>Passaggi successivi
* Per informazioni su prezzi, supporto e contratti di servizio, visitare i collegamenti seguenti:
  
    [Dettagli prezzi dei trasferimenti di dati](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Piani di supporto per Azure](https://azure.microsoft.com/support/plans/)
  
    [Contratti di servizio](https://azure.microsoft.com/support/legal/sla/)
  
    [Dettagli prezzi - Database SQL](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Dimensioni delle macchine virtuali e dei servizi cloud per Microsoft Azure][vmsizes]
  
* Per informazioni sulle procedure consigliate per il Servizio app di Azure, inclusa la creazione di un'architettura scalabile e resiliente, vedere [Best Practices: Azure App Service Web Apps](https://azure.microsoft.com/blog/best-practices-windows-azure-websites-waws/) (Procedure consigliate per le app Web del Servizio app di Azure).
* Per i video sul ridimensionamento delle app del servizio app, vedere le risorse seguenti:
  
  * [Quando è necessario ridimensionare i siti Web di Azure - con Stefan Schackow](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Scalabilità automatica per i siti Web di Azure, pianificata o in base alla CPU - con Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Che cosa accade durante il ridimensionamento dei siti Web di Azure - con Stefan Schackow](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
