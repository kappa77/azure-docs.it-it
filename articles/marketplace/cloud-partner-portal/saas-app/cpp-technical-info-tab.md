---
title: SaaS applicazione offerta tecnico configurazione di Azure | Azure Marketplace
description: Configurare le informazioni tecniche per un'offerta di applicazione SaaS in Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 46dcf4aeb7ddb67028eb23dde9236f2b7709f86d
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/30/2019
ms.locfileid: "64941735"
---
# <a name="saas-application-technical-info-tab"></a>Scheda Informazioni tecniche per le applicazioni SaaS

La scheda Informazioni tecniche include il modulo Configurazione tecnica, che è possibile usare per scegliere il tipo di applicazione (app) SaaS che si sta creando e configurare il modo in cui l'app viene fornita ai clienti.

![Modulo Configurazione tecnica](./media/saas-techinfo-techconfig.png)


## <a name="technical-configuration-form"></a>Modulo Configurazione tecnica

Questo modulo ha due campi: Prodotto e Invito all'azione.


### <a name="product-field"></a>Campo Prodotto

È possibile fornire un'app SaaS per entrambe le vetrine seguenti:
- Per un utente aziendale selezionando l'opzione **Inserzione**.
- Per un utente amministratore IT selezionando **Sell through Microsoft** (Vendi tramite Microsoft).
Per decidere quale tipo di app SaaS si sta creando, leggere [Informazioni sulla selezione della vetrina](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type#understand-storefront-selection).


#### <a name="sell-through-microsoft"></a>Vendita tramite Microsoft
Per creare questa esperienza, è necessario configurare gli elementi seguenti:

- Connettere il sito Web del servizio SaaS alle API SaaS Microsoft. L'articolo [Vendita SaaS tramite Azure - API](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-subscription-apis) illustra come creare questa connessione.
- Abilitare Sell through Azure (Vendi tramite Azure) nel modulo Configurazione tecnica del portale Cloud Partner e specificare le informazioni richieste. Per altre informazioni su questo modello di fatturazione e la relativa implementazione, vedere [Vendita di app SaaS tramite Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-offer-subscriptions).

  ![Modulo Sell through Microsoft (Vendi tramite Microsoft)](./media/saas-techinfo-sellthrough-ms.png)

Nella tabella seguente vengono descritti i campi obbligatori per **Vendi tramite Microsoft**.  I campi obbligatori sono indicati da un asterisco (*).

|  **Nome campo**   |  **Descrizione**  |
|  ---------------  |  ---------------  |
|  **ID sottoscrizione di anteprima\***   |  Tutti gli identificatori della sottoscrizione di Azure usati per testare l'offerta in anteprima prima che sia pubblicamente disponibile.  |
|  **Visualizzare in anteprima gli account AAD/MSA\***   |  Gli account Azure AD/MSA, separati da virgole, che vengono concesso l'accesso per la versione di anteprima. |
|  **Introduzione a istruzioni** |  Indicazioni da condividere con i clienti per aiutarli a connettersi all'app SaaS. Sono consentiti i tag HTML di base, ad esempio &lt;p&gt;, &lt;h1&gt;, &lt;li&gt; e così via.    |
|  **URL della pagina di destinazione\***           |  L'URL del sito al quale verranno indirizzati i clienti dopo l'acquisto nel portale di Azure. Questo URL sarà anche l'endpoint che riceverà le API di connessione che agevolano l'attività commerciale con Microsoft.   |
| **Connection Webhook\***            |  Per tutti gli eventi asincroni che Microsoft deve inviare all'utente per conto del cliente (ad esempio, se la sottoscrizione di Azure non è più valida), viene richiesto di fornire un webhook di connessione. Se non si ha un sistema di webhook, la configurazione più semplice è costituita da un'app per la logica con endpoint HTTP che rimane in ascolto degli eventi inseriti e li gestisce in modo appropriato. Per altre informazioni, vedere <a href="https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint">Chiamare, attivare o annidare i flussi di lavoro con endpoint HTTP in app per la logica</a>.    |
|  **ID Tenant di Azure AD\***  e **ID App\***      |   All'interno del portale di Azure è necessario creare un'app Active Directory in modo che la connessione tra i due servizi sia protetta da una comunicazione autenticata. Per questi campi, creare un'app Active Directory e inserire l'ID tenant e l'ID app corrispondenti. L'ID dell'app è associato all'ID dell'editore. Di conseguenza, verificare che l’ID sia lo stesso in tutte le offerte.   |
|   |   |

Infine, se si seleziona **Sell through Microsoft** (Vendi tramite Microsoft), è disponibile un'altra scheda per la nuova offerta denominata **Piani**. 

La [scheda Piani](./cpp-plans-tab.md) elenca i piani specifici e i prezzi corrispondenti supportati dall'app SaaS. Al momento sono consentiti prezzi mensili, con la possibilità di offrire 1 o 3 mesi di accesso gratuito. Questi piani e prezzi devono corrispondere esattamente ai piani e ai prezzi indicati nel sito dell'app SaaS del partner.

>[!NOTE] 
>I piani sono necessari solo se si sceglie Sell through Microsoft (Vendi tramite Microsoft).

### <a name="call-to-action-field"></a>Campo Invito all'azione

Il campo Invito all'azione consente di selezionare il messaggio visualizzato sul pulsante di acquisizione dell'offerta. Sono disponibili le opzioni seguenti:

- Gratuita: se si seleziona questa opzione, viene chiesto di immettere un URL di prova da cui i clienti possono accedere all'app SaaS. Ad esempio: https://contoso.com/trial
- Versione di valutazione gratuita: se si seleziona questa opzione, viene chiesto di immettere un URL di prova da cui i clienti possono accedere all'app SaaS. Ad esempio: https://contoso.com/trial
- Contact me (Contattami)

Per altre informazioni sulle opzioni di Invito all'azione, vedere Scegliere un'opzione di pubblicazione.

>[!Note]
>Canale di partner cloud Solution Provider (CSP) acconsentire esplicitamente a questo punto disponibili.  Vedi [Cloud Solution Provider](../../cloud-solution-providers.md) per altre informazioni sul marketing dell'offerta tramite Microsoft CSP partner canali.

## <a name="next-steps"></a>Passaggi successivi

- [Scheda Piani (facoltativa)](./cpp-plans-tab.md)
- [Scheda Informazioni canale](./cpp-channel-info-tab.md)
