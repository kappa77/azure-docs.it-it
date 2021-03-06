---
title: La connessione dati di Azure AD Identity Protection all'anteprima di Azure Sentinel | Microsoft Docs
description: Informazioni su come connettere i dati di Azure AD Identity Protection per Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 10dc31e21f20618450de6d99b3fce40d63272d31
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204382"
---
# <a name="connect-data-from-azure-ad-identity-protection"></a>Connetti i dati da Azure AD Identity Protection

> [!IMPORTANT]
> Azure Sentinel è attualmente in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

È possibile trasmettere i log dal [Azure AD Identity Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) in Azure Sentinel agli avvisi in Azure Sentinel per visualizzare i dashboard, creare avvisi personalizzati e migliorare l'analisi di flusso. Azure Active Directory Identity Protection offre una visualizzazione consolidata di utenti a rischio, eventi di rischio e vulnerabilità, con la possibilità di correggere immediatamente il rischio e impostare i criteri per la correzione automatica di eventi in futuro. Il servizio è basato sull'esperienza Microsoft nella protezione delle identità degli utenti e garantisce una precisione straordinaria alle segnalazioni da oltre 13 miliardi di accessi al giorno. 


## <a name="prerequisites"></a>Prerequisiti

- È necessario disporre di un [licenza di Azure Active Directory Premium P1 o P2](https://azure.microsoft.com/pricing/details/active-directory/)
- Utenti con autorizzazioni di sicurezza amministratore o amministratore globale


## <a name="connect-to-azure-ad-identity-protection"></a>Connettersi ad Azure AD Identity Protection

Se hai già Azure AD Identity Protection, assicurarsi che sia [abilitata nella rete](../active-directory/identity-protection/enable.md).
Se è stato distribuito Azure AD Identity Protection e recupero di dati, i dati dell'avviso possono facilmente essere trasmessi a Sentinel di Azure.


1. In Azure Sentinel, selezionare **connettori di dati** e quindi fare clic sui **Azure AD Identity Protection** riquadro.

2. Fare clic su **Connect** per avviare lo streaming di eventi di Azure AD Identity Protection in Sentinel di Azure.


6. Per usare lo schema appropriato nel Log Analitica per gli avvisi di Azure AD Identity Protection, cercare **IdentityProtectionLogs_CL**.

## <a name="next-steps"></a>Passaggi successivi
In questo documento è stato descritto come connettere Azure AD Identity Protection per Azure Sentinel. Per altre informazioni su Azure Sentinel, vedere gli articoli seguenti:
- Informazioni su come [ottenere la visibilità di dati e le potenziali minacce](quickstart-get-visibility.md).
- Iniziare a usare [rilevando minacce con Azure Sentinel](tutorial-detect-threats.md).
