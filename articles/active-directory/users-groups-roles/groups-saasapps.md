---
title: Uso di un gruppo per gestire l'accesso ad applicazioni SaaS - Azure Active Directory | Microsoft Docs
description: Come usare i gruppi in Azure Active Directory per assegnare l'accesso ad applicazioni SaaS integrate in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 873863016d6dc54c7439ec1f46180b3c063bda19
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60470416"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Uso di un gruppo per gestire l'accesso ad applicazioni SaaS

Con Azure Active Directory (Azure AD) con licenza Azure AD Premium o Azure AD Basic, è possibile usare i gruppi per assegnare l'accesso a un'applicazione SaaS integrata in Azure AD. Ad esempio, se si desidera assegnare al reparto marketing l'accesso per l'uso di cinque applicazioni SaaS diverse, è possibile creare un gruppo contenente gli utenti del reparto marketing e quindi assegnare a tale gruppo le cinque applicazioni SaaS necessarie. In questo modo è possibile velocizzare le operazioni grazie alla gestione dei membri del reparto marketing in un'unica posizione. Gli utenti vengono quindi assegnati all'applicazione quando vengono aggiunti come membri del gruppo marketing. In modo analogo, le relative assegnazioni vengono rimosse dall'applicazione quando gli utenti vengono rimossi dal gruppo marketing. Questa funzionalità può essere usata con centinaia di applicazioni che è possibile aggiungere dalla raccolta di applicazioni Azure AD.

> [!IMPORTANT]
> È possibile usare questa funzionalità solo dopo avere avviato una versione di valutazione di Azure AD Premium o avere acquistato una licenza di Azure AD Premium o Azure AD Basic. L'assegnazione basata su gruppo è supportata solo per i gruppi di sicurezza. In questo momento, le appartenenze ai gruppi annidati non sono supportate per l'assegnazione alle applicazioni in base al gruppo.

**Per assegnare a un utente o un gruppo l'accesso a un'applicazione SaaS**

1. Nell'[interfaccia di amministrazione di Azure AD](https://aad.portal.azure.com) selezionare **Applicazioni aziendali**.
2. Selezionare nella raccolta di applicazioni un'applicazione aggiunta per aprirla.
3. Selezionare **Utenti e gruppi** e quindi **Aggiungi utente**.
4. In **Aggiungi assegnazione** selezionare **Utenti e gruppi** per aprire l'elenco di selezione **Utenti e gruppi**.
6. Selezionare il numero di gruppi o utenti desiderati, quindi toccare o fare clic su **Seleziona** per aggiungerli all'elenco **Aggiungi assegnazione**. In questa fase è anche possibile assegnare un ruolo a un utente.
7. Selezionare **Assegna** per assegnare gli utenti o i gruppi all'applicazione aziendale selezionata.

### <a name="next-steps"></a>Passaggi successivi
Questi articoli forniscono informazioni aggiuntive su Azure Active Directory.

* [Gestione dell'accesso alle risorse tramite i gruppi di Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Gestione di applicazioni con Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Cmdlet di Azure Active Directory per la configurazione delle impostazioni di gruppo](groups-settings-cmdlets.md)
* [Informazioni su Azure Active Directory](../fundamentals/active-directory-whatis.md)
* [Integrazione delle identità locali con Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
