---
title: Connettersi a qualsiasi endpoint HTTP con App per la logica di Azure | Microsoft Docs
description: Automatizzare le attività e i flussi di lavoro in grado di comunicare con qualsiasi endpoint HTTP con le App per la logica di Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 22b21512c78a06f2639ca9339f3b7a20c7f5bfa3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713806"
---
# <a name="call-http-or-https-endpoints-with-azure-logic-apps"></a>Chiamare endpoint HTTP o HTTPS con le App per la logica di Azure

Con App per la logica di Azure e il connettore Hypertext Transfer Protocol (HTTP), è possibile automatizzare i flussi di lavoro in grado di comunicare con qualsiasi endpoint HTTP o HTTPS mediante la compilazione di App per la logica. Ad esempio, è possibile monitorare l'endpoint di servizio per il sito Web. Quando si verifica un evento su un endpoint, ad esempio il sito Web non funziona, questo attiva il flusso di lavoro dell'app per la logica ed esegue le azioni specificate.

È possibile usare il trigger HTTP come primo passaggio nel flusso di lavoro per la verifica o il *polling* di un endpoint a intervalli regolari. In ogni controllo, il trigger invia una chiamata o una *richiesta* all'endpoint. La risposta dell'endpoint determina se il flusso di lavoro dell'app per la logica è in esecuzione. Il trigger passa da qualsiasi contenuto, dalla risposta alle azioni, nell'app per la logica. 

È possibile usare l'azione HTTP come qualsiasi altro passaggio nel flusso di lavoro per chiamare l'endpoint quando si desidera. La risposta dell'endpoint determina l'esecuzione delle azioni rimanenti del flusso di lavoro. 

Base funzionalità dell'endpoint di destinazione, il connettore supporta Transport Layer Security (TLS) versioni 1.0, 1.1 e 1.2. App per la logica esegue la negoziazione con l'endpoint su utilizzando la versione supportata più alto possibile. Quindi, ad esempio, se l'endpoint supporta 1.2, il connettore Usa 1.2 prima di tutto. In caso contrario, il connettore Usa la versione supportata più recente successiva.

Se non si ha familiarità con le app per la logica, consultare [Informazioni su App per la logica di Azure](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Prerequisiti

* Una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, [iscriversi per creare un account Azure gratuito](https://azure.microsoft.com/free/). 

* L'URL per l'endpoint di destinazione che si desidera chiamare 

* Conoscenza di base di [come creare le app per la logica](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* L'app per la logica da cui si desidera chiamare l'endpoint di destinazione. Per iniziare con il trigger HTTP, [creare un'app per la logica vuota](../logic-apps/quickstart-create-first-logic-app-workflow.md). Per usare l'azione HTTP, avviare l'app per la logica con un trigger.

## <a name="add-http-trigger"></a>Aggiungere trigger HTTP

1. Accedere al [portale di Azure](https://portal.azure.com) e aprire l'app vuota per la logica in Progettazione app per la logica, se non è già aperta.

1. Nella casella di ricerca, immettere "HTTP" come filtro. Nell'elenco dei trigger selezionare il trigger **HTTP**. 

   ![Selezionare HTTP Trigger](./media/connectors-native-http/select-http-trigger.png)

1. Fornire i [parametri e valori del trigger HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) che si desidera includere nella chiamata all'endpoint di destinazione. Impostare per la frequenza con cui il trigger di ricorrenza per verificare l'endpoint di destinazione.

   ![Immettere i parametri del trigger HTTP](./media/connectors-native-http/http-trigger-parameters.png)

   Per altre informazioni sul trigger HTTP, parametri e valori, vedere [Riferimento ai tipi di trigger e azione](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger).

1. Proseguire con la creazione del flusso di lavoro dell'app per la logica con azioni da eseguire all'attivazione del trigger.

## <a name="add-http-action"></a>Aggiungere azione HTTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Accedere al [portale di Azure](https://portal.azure.com) e aprire l'app per la logica in Progettazione app per la logica, se non è già aperta.

1. Nell'ultimo passaggio in cui si vuole aggiungere un'azione HTTP, scegliere **Nuovo passaggio**. 

   In questo esempio l'app per la logica viene avviata inizialmente con il trigger HTTP.

1. Nella casella di ricerca, immettere "HTTP" come filtro. Nell'elenco delle azioni selezionare l'azione **HTTP**.

   ![Selezionare l'azione HTTP](./media/connectors-native-http/select-http-action.png)

   Per aggiungere un'azione tra i passaggi, spostare il puntatore del mouse sulla freccia tra i passaggi. 
   Scegliere il segno più (**+**) visualizzato e quindi selezionare **Aggiungi un'azione**.

1. Fornire i [parametri e valori dell'azione HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) che si desidera includere nella chiamata all'endpoint di destinazione. 

   ![Immettere i parametri dell'azione HTTP](./media/connectors-native-http/http-action-parameters.png)

1. Al termine, assicurarsi di salvare l'app per la logica. Nella barra degli strumenti della finestra di progettazione scegliere **Salva**. 

## <a name="authentication"></a>Authentication

Per impostare l'autenticazione, scegliere **Mostra opzioni avanzate** all'interno dell'azione o trigger. Per altre informazioni sui tipi di autenticazione disponibili per azioni e trigger HTTP, vedere [Riferimento ai tipi di trigger e azione](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

## <a name="get-support"></a>Supporto

* In caso di domande, visitare il [forum di App per la logica di Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Per votare o inviare idee relative alle funzionalità, visitare il [sito dei commenti e suggerimenti degli utenti di App per la logica](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passaggi successivi

* Informazioni su altri [connettori di App per la logica](../connectors/apis-list.md)
