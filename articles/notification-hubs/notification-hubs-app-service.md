---
title: Integrazione con le app per dispositivi mobili del servizio app
description: Informazioni sul funzionamento di Hub di notifica di Azure con le app per dispositivi mobili del servizio app di Azure.
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
documentationcenter: ''
ms.assetid: 83132dff-a01d-4b31-a426-b57496852b81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: bb39949bc21ece819b7ac6e279390f8bdb0ee00a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65157449"
---
# <a name="integration-with-app-service-mobile-apps"></a>Integrazione con le app per dispositivi mobili del servizio app

> [!NOTE]
> Microsoft si impegna a supportare completamente le app per dispositivi mobili del servizio app, inclusi il supporto per la versione più recente del sistema operativo, le correzioni di bug, i miglioramenti della documentazione e le revisioni delle richieste pull da parte della community. Si noti che il team del prodotto attualmente non sta investendo in nuove funzionalità per App per dispositivi mobili di Azure. I contributi della community a tutte le aree di App per dispositivi mobili di Azure sono molto apprezzati.

Per favorire un'esperienza lineare e uniforme nei servizi di Azure, le [app per dispositivi mobili del servizio app](../app-service-mobile/app-service-mobile-value-prop.md) comprendono il supporto predefinito per le notifiche push mediante Hub di notifica. [app per dispositivi mobili del servizio app](../app-service-mobile/app-service-mobile-value-prop.md) offrono una piattaforma di sviluppo di applicazioni mobili estremamente scalabile e disponibile a livello globale per sviluppatori aziendali e integratori di sistemi, che fornisce un set completo di funzionalità per gli sviluppatori di soluzioni per dispositivi mobili.

Gli sviluppatori di app per dispositivi mobili possono usare gli hub di notifica con il seguente flusso di lavoro:

1. Recuperare la gestione del dispositivo PNS
2. Registrare il dispositivo con gli Hub di notifica mediante il pratico API Register SDK client delle app per dispositivi mobili

    > [!NOTE]
    > Si noti che le app per dispositivi mobili consentono di eliminare tutti i tag nelle registrazioni per motivi di sicurezza. Usare le hub di notifica dal back-end direttamente per associare tag ai dispositivi.

3. Invio di notifiche dal back-end dell'app con hub di notifica

Ecco alcuni vantaggi per gli sviluppatori inclusi in questa integrazione:

- **SDK client delle app per dispositivi mobili.**:  Questi SDK multipiattaforma forniscono API semplici per la registrazione e per comunicare automaticamente con l'hub di notifica collegato all'app per dispositivi mobili. Gli sviluppatori non hanno bisogno di cercare tra le credenziali di Hub di notifica e lavorare con un servizio aggiuntivo.
  - *Push a utente*: Gli SDK assegnano automaticamente tag al dispositivo specificato con l'ID utente autenticato di App per dispositivi mobili per abilitare il push all'utente.
  - *Push a dispositivo*: L'ID di installazione di App per dispositivi mobili viene usato automaticamente dagli SDK come GUID per la registrazione in Hub di notifica. In questo modo gli sviluppatori non sono costretti a mantenere più GUID di servizio.
- **Modello di installazione**:  App per dispositivi mobili usa il modello push più recente di Hub di notifica per rappresentare tutte le proprietà push associate a un dispositivo in un'installazione JSON che sia in linea con i servizi di notifica push e facile da usare.
- **Flessibilità**:  Gli sviluppatori possono scegliere di usare direttamente Hub di notifica in qualsiasi momento, anche dopo l'integrazione.
- **Esperienza integrata nel [portale di Azure](https://portal.azure.com)**:  Il Push come funzionalità è rappresentato visivamente nelle app per dispositivi mobili e gli sviluppatori possono utilizzare facilmente l'hub di notifica associato tramite le App per dispositivi mobili.
