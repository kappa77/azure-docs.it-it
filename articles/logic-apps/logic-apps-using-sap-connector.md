---
title: Connettersi a sistemi SAP - App per la logica di Azure
description: Accedere e gestire le risorse SAP automatizzando i flussi di lavoro con le App per la logica di Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 05/09/2019
tags: connectors
ms.openlocfilehash: bccefec80ef3afd6d312bb1048d3be5d8e708728
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258160"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Connettersi a sistemi SAP con App per la logica di Azure

Questo articolo illustra come accedere alle risorse SAP in locale all'interno di un'app per la logica usando il connettore SAP. Il connettore funziona con le versioni classiche di SAP, ad esempio R/3 e i sistemi locali ECC. Il connettore consente anche l'integrazione con sistemi SAP basati su HANA più recenti di SAP, ad esempio S/4 HANA, ogni volta che sono ospitati - in locale o nel cloud. Il connettore SAP supporta l'integrazione di messaggi o dati da e verso i sistemi basati su SAP NetWeaver in Intermediate Document (IDoc) o Business Application Programming Interface (BAPI) o chiamate di funzione remota (RFC).

Il connettore SAP Usa la [libreria del connettore SAP .NET (NCo)](https://support.sap.com/en/product/connectors/msnet.html) e fornisce le operazioni o azioni:

* **Inviare a SAP**: Inviare IDoc su tRFC, chiamare funzioni BAPI tramite RFC o chiamare RFC/tRFC in sistemi SAP.
* **Ricevere da SAP**: Ricevere IDoc su tRFC, chiamare funzioni BAPI tramite tRFC o chiamare RFC/tRFC in sistemi SAP.
* **Generare schemi**: Generare gli schemi per gli elementi SAP per IDoc, BAPI e RFC.

Per tutte le operazioni indicate in precedenza, il connettore SAP supporta l'autenticazione di base tramite nome utente e password. Il connettore supporta anche [comunicazioni di rete sicura (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true), che può essere usato per SAP NetWeaver Single Sign-On o per capacità aggiuntiva di sicurezza fornite da un prodotto di protezione esterne.

Il connettore SAP si integra con i sistemi SAP locali tramite il [gateway dati locale](../logic-apps/logic-apps-gateway-connection.md). Negli scenari di trasmissione, ad esempio, quando si invia un messaggio dall'App per la logica a un sistema SAP, il gateway dati agisce come un client di RFC e inoltra le richieste ricevute dall'App per la logica a SAP.
Analogamente, in scenari di ricezione, il gateway dati funge da server RFC che riceve le richieste da SAP e inoltra all'app per la logica.

Questo articolo spiega come creare esempi di app per la logica che si integrano con SAP, illustrando allo stesso tempo gli scenari di integrazione descritti in precedenza.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Prerequisiti

Per proseguire con questo articolo, sono necessari questi elementi:

* Una sottoscrizione di Azure. Se non si dispone ancora di una sottoscrizione di Azure, [registrarsi per creare un account Azure gratuito](https://azure.microsoft.com/free/).

* l'app per la logica da cui si desidera accedere al sistema SAP e un trigger che avvia il flusso di lavoro dell'app per la logica. Se non si ha familiarità con le app per la logica, consultare [Informazioni su App per la logica di Azure](../logic-apps/logic-apps-overview.md) e [Avvio rapido: Creare la prima app per la logica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Il [server applicazioni SAP](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) o il [server di messaggistica SAP](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)

* Scaricare e installare la versione più recente del [gateway dati locale](https://www.microsoft.com/download/details.aspx?id=53127) in qualsiasi computer locale. Prima di proseguire, assicurarsi di configurare il gateway nel portale di Azure. Il gateway consente in modo sicuro i dati in locale l'accesso e le risorse. Per altre informazioni, vedere [Installare il gateway dati locale per le App per la logica di Azure](../logic-apps/logic-apps-gateway-install.md).

* Se si usa SNC con Single Sign-On (SSO), assicurarsi che il gateway è in esecuzione come utente che viene eseguito il mapping rispetto all'utente SAP. Per modificare l'account predefinito, selezionare **cambiare account**e immettere le credenziali dell'utente.

  ![Modifica account del gateway](./media/logic-apps-using-sap-connector/gateway-account.png)

* Se si sta abilitando SNC con un prodotto di protezione esterne, copiare la libreria SNC o i file nello stesso computer in cui è installato il gateway. Alcuni esempi di prodotti SNC [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos, NTLM, e così via.

* Scaricare e installare la libreria client SAP più recente, che è attualmente [SAP Connector (NCo) 3.0.21.0 per Microsoft .NET Framework 4.0 e Windows 64 bit (x64)](https://softwaredownloads.sap.com/file/0020000001865512018), nello stesso computer del gateway dati locale. Installare questa versione o versioni successive per i motivi seguenti:

  * Le versioni precedenti di NCo SAP possono diventare deadlock quando vengono inviati più messaggi IDoc contemporaneamente. Questa condizione blocca tutti i messaggi successivi che vengono inviati alla destinazione SAP, causando il timeout dei messaggi.

  * Il gateway dati locale viene eseguito solo su sistemi a 64 bit. In caso contrario, viene visualizzato l'errore "immagine non valida" perché il servizio host del gateway dati non supporta gli assembly a 32 bit.

  * Il servizio host del gateway dati e l'adattatore SAP Microsoft usano .NET Framework 4.5. SAP NCo per .NET Framework 4.0 funziona con processi che usano runtime .NET dalla versione 4.0 alla 4.7.1. NCo SAP per .NET Framework 2.0 funziona con processi che usano il runtime .NET da 2.0 a 3.5 e non funziona più con il gateway dati locale più recente.

* Contenuto del messaggio che è possibile inviare al server SAP, ad esempio un file di esempio IDoc. Il contenuto deve essere in formato XML e includere lo spazio dei nomi per l'azione SAP che si desidera usare.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Inviare a SAP

In questo esempio viene usata un'app per la logica che è possibile attivare con una richiesta HTTP. L'app per la logica invia un documento intermedio (IDoc) a un server SAP e restituisce una risposta al richiedente che ha chiamato l'app per la logica. 

### <a name="add-http-request-trigger"></a>Aggiungere un trigger della richiesta HTTP

In App per la logica di Azure, ogni app per la logica deve essere avviata con un [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), che viene attivato quando si verifica un determinato evento o quando viene soddisfatta una condizione specifica. Ogni volta che il trigger viene attivato, il motore di App per la logica crea un'istanza dell'app per la logica e avvia l'esecuzione del flusso di lavoro dell'app.

In questo esempio, viene creata un'app per la logica con un endpoint in Azure in modo che sia possibile inviare *richieste HTTP POST* all'app per la logica. Quando l'app per la logica riceve le richieste HTTP, il trigger viene attivato ed esegue il passaggio successivo nel flusso di lavoro.

1. Nel [portale di Azure](https://portal.azure.com) creare un'app per la logica vuota, che apre la Progettazione app per la logica.

1. Nella casella di ricerca, digitare "richiesta http" come filtro. Nell'elenco di trigger selezionare questo trigger: **Quando viene ricevuta una richiesta HTTP**

   ![Aggiungere un trigger della richiesta HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Salvare l'app per la logica per poter generare un URL endpoint per l'app per la logica. Nella barra degli strumenti della finestra di progettazione scegliere **Salva**.

   L'URL endpoint verrà visualizzato nel trigger, ad esempio:

   ![Generare l'URL per l'endpoint](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-sap-action"></a>Aggiungere l'azione SAP

In App per la logica di Azure, un'[azione](../logic-apps/logic-apps-overview.md#logic-app-concepts) è un passaggio del flusso di lavoro che segue un trigger o un'altra azione. Se non è ancora stato aggiunto un trigger all'app per la logica e si desidera seguire questo esempio, [aggiungere il trigger descritto in questa sezione](#add-trigger).

1. In Progettazione App per la logica, sotto il trigger, scegliere **nuovo passaggio**.

   ![Scegliere "Nuovo passaggio"](./media/logic-apps-using-sap-connector/add-action.png)

1. Nella casella di ricerca, immettere "sap" come filtro. Nell'elenco di azioni selezionare questa azione: **Send message to SAP** (Invia messaggio a SAP)
  
   ![Selezionare l'azione di invio a SAP](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   In alternativa, invece di una ricerca, scegliere la scheda **Enterprise** e selezionare l'azione SAP.

   ![Selezionare l'azione di invio a SAP dalla scheda Enterprise](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Se vengono richiesti i dettagli della connessione, creare ora la connessione SAP. In caso contrario, se la connessione esiste già, continuare con il passaggio successivo per impostare l'azione SAP.

   **Creare una connessione SAP locale**

   1. Specificare le informazioni di connessione per il server SAP. Per la proprietà **Gateway dati**, selezionare il gateway dati creato nel portale di Azure per l'installazione del gateway.

      Se la proprietà **Tipo di accesso** è impostata su **Server applicazioni**, sono necessarie le seguenti proprietà (in genere facoltative):

      ![Creare una connessione al server applicazioni SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Se la proprietà **Tipo di accesso** è impostata su **Gruppo**, sono necessarie le seguenti proprietà (in genere facoltative):

      ![Creare una connessione al server di messaggistica SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Per impostazione predefinita, la tipizzazione forte consente di verificare la presenza di valori non validi eseguendo la convalida di XML rispetto allo schema. Questo comportamento può aiutarti a rilevare i problemi in precedenza. Il **tipizzazione sicura** opzione è disponibile per compatibilità con le versioni precedenti e controlla solo la lunghezza della stringa. Altre informazioni sul [ **sicura digitando** opzione](#safe-typing).

   1. Al termine dell'operazione, scegliere **Crea**.

      Le App per la logica configurano ed eseguono il test della connessione, assicurandosi che funzioni correttamente.

1. Ora individuare e selezionare un'azione dal server SAP.

   1. Nel **azione SAP** scegliere l'icona della cartella. Dall'elenco file, individuare e selezionare il messaggio SAP che si desidera usare. Per spostarsi nell'elenco, usare le frecce.

      In questo esempio consente di selezionare con un IDoc **ordini** tipo.

      ![Trovare e selezionare l'azione IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Se non è possibile trovare l'azione desiderata, è possibile immettere manualmente un percorso, ad esempio:

      ![Specificare manualmente il percorso all'azione IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Specificare il valore per l'azione SAP tramite l'editor espressioni. In questo modo, è possibile usare la stessa azione per diversi tipi di messaggio.

      Per altre informazioni sulle operazioni IDoc, consultare [Schemi di messaggi per operazioni IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   1. Fare clic all'interno della casella **Messaggio di input** in modo che venga visualizzato l'elenco di contenuti dinamici. In tale elenco, sotto **Alla ricezione di una richiesta HTTP** selezionare il campo **Corpo**.

      Questo passaggio include il contenuto del corpo del trigger della richiesta HTTP e invia questo output al server SAP.

      ![Selezionare il campo "Corpo"](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Al termine, l'azione SAP avrà un aspetto simile a quello in questo esempio:

      ![Completare l'operazione SAP](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Salvare l'app per la logica. Nella barra degli strumenti della finestra di progettazione scegliere **Salva**.

<a name="add-response"></a>

### <a name="add-http-response-action"></a>Aggiungere l'azione di risposta HTTP

Ora aggiungere un'azione di risposta al flusso di lavoro dell'app logica e includere l'output dall'azione SAP. In questo modo, l'app per la logica restituisce i risultati dal server SAP per il richiedente originale.

1. In Progettazione App per la logica, sotto l'azione di SAP, scegliere **nuovo passaggio**.

1. Nella casella di ricerca digitare "risposta" come filtro. Nell'elenco di azioni selezionare questa azione: **Risposta**

1. Fare clic all'interno della casella **Corpo** casella in modo che venga visualizzato l'elenco di contenuto dinamico. Da quell'elenco sotto **invia un messaggio a SAP**, selezionare la **corpo** campo.

   ![Completare l'operazione SAP](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Salvare l'app per la logica.

### <a name="test-your-logic-app"></a>Testare l'app per la logica

1. Se l'app per la logica non è ancora abilitata, scegliere **Panoramica** nel menu dell'app per la logica. Nella barra degli strumenti scegliere **Abilita**.

1. Nella barra degli strumenti di Progettazione app per la logica selezionare **Esegui**. Questo passaggio avvia manualmente l'app per la logica.

1. Attivare l'app per la logica inviando una richiesta HTTP POST all'URL nel trigger della richiesta HTTP e includere il contenuto del messaggio nella richiesta. Per inviare la richiesta, è possibile usare uno strumento, ad esempio [Postman](https://www.getpostman.com/apps).

   In questo articolo, la richiesta invia un file IDoc, che deve essere in formato XML e includere lo spazio dei nomi per l'azione SAP che si sta usando, ad esempio:

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. Dopo aver inviato la richiesta HTTP, attendere la risposta dall'app per la logica.

   > [!NOTE]
   > L'app per la logica può andare in timeout se tutti i passaggi necessari per la risposta non terminano entro il [limite di timeout della richiesta](./logic-apps-limits-and-config.md). Se si verifica questa condizione, le richieste potrebbero venire bloccate. Per diagnosticare i problemi, vedere le informazioni su come [controllare e monitorare le app per la logica](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Complimenti, è stata creata un'app per la logica che può comunicare con il server SAP. Dopo aver impostato una connessione a SAP per l'app per la logica, è possibile esplorare altre azioni SAP disponibili, ad esempio BAPI e RFC.

## <a name="receive-from-sap"></a>Ricevere da SAP

Questo esempio usa un'app per la logica che si attiva quando si riceve un messaggio da un sistema SAP.

### <a name="add-sap-trigger"></a>Aggiungere trigger SAP

1. Nel portale di Azure creare un'app per la logica vuota, che apre la Progettazione app per la logica.

1. Nella casella di ricerca, immettere "sap" come filtro. Nell'elenco di trigger selezionare questo trigger: **When a message is received from SAP** (Quando si riceve un messaggio da SAP)

   ![Aggiungere trigger SAP](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   In alternativa, è possibile passare per il **Enterprise** scheda e selezionare il trigger:

   ![Aggiungere trigger SAP dalla scheda Enterprise](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Se vengono richiesti i dettagli della connessione, creare ora la connessione SAP. In caso contrario, se la connessione esiste già, continuare con il passaggio successivo per impostare l'azione SAP.

   **Creare una connessione SAP locale**

   1. Specificare le informazioni di connessione per il server SAP. Per la proprietà **Gateway dati**, selezionare il gateway dati creato nel portale di Azure per l'installazione del gateway.

      Se la proprietà **Tipo di accesso** è impostata su **Server applicazioni**, sono necessarie le seguenti proprietà (in genere facoltative):

      ![Creare una connessione al server applicazioni SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Se la proprietà **Tipo di accesso** è impostata su **Gruppo**, sono necessarie le seguenti proprietà (in genere facoltative):

      ![Creare una connessione al server di messaggistica SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Per impostazione predefinita, la tipizzazione forte consente di verificare la presenza di valori non validi eseguendo la convalida di XML rispetto allo schema. Questo comportamento può aiutarti a rilevare i problemi in precedenza. Il **tipizzazione sicura** opzione è disponibile per compatibilità con le versioni precedenti e controlla solo la lunghezza della stringa. Altre informazioni sul [ **sicura digitando** opzione](#safe-typing).

1. Specificare i parametri necessari in base alla configurazione del sistema SAP.

   È possibile anche specificare una o più azioni di SAP. Questo elenco di azioni consente di specificare i messaggi che riceve il trigger dal server SAP tramite il gateway dati. Un elenco vuoto specifica che il trigger riceve tutti i messaggi. Se l'elenco contiene più di un messaggio, il trigger riceve solo i messaggi specificati nell'elenco. Tutti gli altri messaggi inviati dal server SAP vengono rifiutati dal gateway.

   È possibile selezionare un'azione SAP dalla selezione file:

   ![Selezionare l'azione SAP](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   In alternativa, è possibile specificare manualmente l'azione:

   ![Immettere manualmente l'azione SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Di seguito un esempio che illustra come viene visualizzata l'azione quando si configura il trigger per ricevere più messaggi.

   ![Esempio di trigger](media/logic-apps-using-sap-connector/example-trigger.png)  

   Per altre informazioni sull'azione SAP, consultare [Schemi di messaggi per operazioni IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

1. Salvare l'app per la logica in modo da poter iniziare a ricevere messaggi dal sistema SAP.
Nella barra degli strumenti della finestra di progettazione scegliere **Salva**.

App per la logica è ora pronta per ricevere messaggi dal sistema SAP.

> [!NOTE]
> Il trigger SAP non è un trigger di poll, ma un trigger basato su webhook. Il trigger viene chiamato dal gateway solo quando esiste un messaggio, pertanto non è necessario alcun polling.

### <a name="test-your-logic-app"></a>Testare l'app per la logica

1. Per attivare l'app per la logica, inviare un messaggio dal sistema SAP.

1. Dal menu dell'app per la logica, scegliere **Panoramica** ed esaminare **Cronologia di esecuzione** per eventuali nuove esecuzioni dell'app per la logica.

1. Aprire l'esecuzione più recente, che mostra il messaggio inviato dal sistema SAP nella sezione output di trigger.

## <a name="generate-schemas-for-artifacts-in-sap"></a>Generare gli schemi per gli artefatti in SAP

In questo esempio viene usata un'app per la logica che è possibile attivare con una richiesta HTTP. L'azione SAP invia una richiesta a un sistema SAP per generare gli schemi per specifici Intermediate Document (IDoc) e BAPI. Gli schemi che ritornano nella risposta vengono caricati in un Account di integrazione tramite il connettore Azure Resource Manager.

### <a name="add-http-request-trigger"></a>Aggiungere un trigger della richiesta HTTP

1. Nel portale di Azure creare un'app per la logica vuota, che apre la Progettazione app per la logica.

1. Nella casella di ricerca, digitare "richiesta http" come filtro. Nell'elenco di trigger selezionare questo trigger: **Quando viene ricevuta una richiesta HTTP**

   ![Aggiungere un trigger della richiesta HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Salvare l'app per la logica per poter generare un URL endpoint per l'app per la logica.
Nella barra degli strumenti della finestra di progettazione scegliere **Salva**.

   L'URL endpoint verrà visualizzato nel trigger, ad esempio:

   ![Generare l'URL per l'endpoint](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-sap-action-to-generate-schemas"></a>Aggiungere l'azione SAP per generare schemi

1. In Progettazione App per la logica, sotto il trigger, scegliere **nuovo passaggio**.

   ![Scegliere "Nuovo passaggio"](./media/logic-apps-using-sap-connector/add-action.png)

1. Nella casella di ricerca, immettere "sap" come filtro. Nell'elenco di azioni selezionare questa azione: **Genera schemi**
  
   ![Selezionare l'azione di invio a SAP](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   In alternativa, scegliere la scheda **Enterprise** e selezionare l'azione SAP.

   ![Selezionare l'azione di invio a SAP dalla scheda Enterprise](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Se vengono richiesti i dettagli della connessione, creare ora la connessione SAP. In caso contrario, se la connessione esiste già, continuare con il passaggio successivo per impostare l'azione SAP.

   **Creare una connessione SAP locale**

   1. Specificare le informazioni di connessione per il server SAP. Per la proprietà **Gateway dati**, selezionare il gateway dati creato nel portale di Azure per l'installazione del gateway.

      Se la proprietà **Tipo di accesso** è impostata su **Server applicazioni**, sono necessarie le seguenti proprietà (in genere facoltative):

      ![Creare una connessione al server applicazioni SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Se la proprietà **Tipo di accesso** è impostata su **Gruppo**, sono necessarie le seguenti proprietà (in genere facoltative):

      ![Creare una connessione al server di messaggistica SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Per impostazione predefinita, la tipizzazione forte consente di verificare la presenza di valori non validi eseguendo la convalida di XML rispetto allo schema. Questo comportamento può aiutarti a rilevare i problemi in precedenza. Il **tipizzazione sicura** opzione è disponibile per compatibilità con le versioni precedenti e controlla solo la lunghezza della stringa. Altre informazioni sul [ **sicura digitando** opzione](#safe-typing).

   1. Al termine dell'operazione, scegliere **Crea**. 
   
      Le App per la logica configurano ed eseguono il test della connessione, assicurandosi che funzioni correttamente.

1. Specificare il percorso dell'artefatto per cui si desidera generare lo schema.

   È possibile selezionare l'azione SAP dalla selezione file:

   ![Selezionare l'azione SAP](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   In alternativa, è possibile immettere manualmente l'azione:

   ![Immettere manualmente l'azione SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Per generare gli schemi per più artefatti, specificare i dettagli dell'azione SAP per ogni artefatto, ad esempio:

   ![Selezionare Aggiungi nuovo elemento](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![Mostrare due elementi](media/logic-apps-using-sap-connector/schema-generator-example.png)

   Per altre informazioni sull'azione SAP, vedere [Schemi di messaggi per operazioni IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Salvare l'app per la logica. Nella barra degli strumenti della finestra di progettazione scegliere **Salva**.

### <a name="test-your-logic-app"></a>Testare l'app per la logica

1. Nella barra degli strumenti della finestra di progettazione, scegliere **Eseguire** per attivare l'esecuzione per l'app per la logica.

1. Aprire l'esecuzione e controllare gli output per il **generare schemi** azione.

   Gli output mostrano gli schemi generati per l'elenco specificato di messaggi.

### <a name="upload-schemas-to-integration-account"></a>Caricare gli schemi nell'account di integrazione

Facoltativamente, è possibile scaricare o archiviare gli schemi generati nei repository, quali blob, risorsa di archiviazione o account di integrazione. Gli account di integrazione offrono un'esperienza all'avanguardia con altre azioni XML, quindi l'esempio illustra come caricare gli schemi in un account di integrazione per la stessa app per la logica usando il connettore Azure Resource Manager.

1. In Progettazione app per la logica, nel trigger scegliere **Nuovo passaggio**.

1. Nella casella di ricerca immettere "Resource Manager" come filtro. Selezionare questa azione: **Create or update a resource** (Crea o aggiorna una risorsa)

   ![Selezionare l'azione Azure Resource Manager](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Immettere i dettagli per l'azione, tra cui la sottoscrizione di Azure, gruppo di risorse di Azure e account di integrazione. Per aggiungere SAP token ai campi, fare clic all'interno di finestre per quei campi e selezionare dall'elenco del contenuto dinamico visualizzato.

   1. Aprire il **Aggiungi nuovo parametro** elencare e selezionare il **posizione** e **proprietà** campi.

   1. Specificare i dettagli per questi nuovi campi, come illustrato in questo esempio.

      ![Immettere i dettagli per l'azione Azure Resource Manager](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   L'azione SAP **Generare schemi** genera schemi come una raccolta, in modo che la finestra di progettazione aggiunga automaticamente un ciclo **For each** all'azione. Di seguito è riportato un esempio che illustra come viene visualizzata questa azione:

   ![Azione di Azure Resource Manager con ciclo "for each"](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)  

   > [!NOTE]
   > Gli schemi usano il formato con codifica base64. Per caricare gli schemi in un account di integrazione, devono essere decodificati usando la funzione `base64ToString()`. Di seguito è riportato un esempio che mostra il codice per l'elemento `"properties"`:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Salvare l'app per la logica. Nella barra degli strumenti della finestra di progettazione scegliere **Salva**.

### <a name="test-your-logic-app"></a>Testare l'app per la logica

1. Sulla barra degli strumenti della finestra di progettazione, scegliere **Esegui** per attivare manualmente l'app per la logica.

1. Dopo una corretta esecuzione, passare all'account di integrazione e verificare che esistano gli schemi generati.

## <a name="enable-secure-network-communications-snc"></a>Abilita le comunicazioni di rete sicura (SNC)

Prima di iniziare, assicurarsi di aver soddisfatto elencate in precedenza [prerequisiti](#pre-reqs):

* Il gateway dati locale è installato in un computer che si trova nella stessa rete come il sistema SAP.

* Per Single Sign-On, i gateway è in esecuzione come utente che viene eseguito il mapping all'utente SAP.

* Libreria SNC che fornisce le funzioni di sicurezza aggiuntivo è stata installata nello stesso computer come gateway di dati. Alcuni esempi di questi elementi includono [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos, NTLM, e così via.

Per abilitare SNC per le richieste da o verso il sistema SAP, selezionare la **Use SNC** casella di controllo nella connessione SAP e fornire queste proprietà:

   ![Configurare SNC SAP nella connessione](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Proprietà | Descrizione |
   |----------| ------------|
   | **Libreria SNC** | Il nome della libreria SNC o percorso al percorso di installazione NCo o percorso assoluto. Ad esempio, `sapsnc.dll` o `.\security\sapsnc.dll` o `c:\security\sapsnc.dll` |
   | **SNC SSO** | Quando ci si connette tramite SNC, l'identità SNC è in genere utilizzata per l'autenticazione del chiamante. Un'altra opzione consiste nell'eseguire l'override in modo che le informazioni utente e password possono essere usate per l'autenticazione del chiamante, ma la riga è comunque crittografata. |
   | **Mi chiamo SNC** | Nella maggior parte dei casi, questa proprietà può essere omessa. La soluzione SNC installata SA in genere il proprio nome SNC. Solo per le soluzioni che supportano "più identità", si potrebbe essere necessario specificare l'identità da usare per questo server o di destinazione particolare. |
   | **Nome del Partner SNC** | Il nome per il back-end SNC |
   | **Qualità SNC di protezione** | Qualità del servizio può essere usato per la comunicazione SNC questa particolare/del server di destinazione. Valore predefinito è definito dal sistema back-end. Il valore massimo è definito in base al prodotto di sicurezza utilizzato per SNC. |
   |||

   > [!NOTE]
   > Variabili di ambiente SNC_LIB e SNC_LIB_64 non devono essere impostate nel computer in cui si dispone di gateway di dati e libreria SNC. Se impostato, verrebbe hanno la precedenza sul valore libreria SNC passato tramite il connettore.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Tipizzazione sicura

Per impostazione predefinita, quando si crea la connessione a SAP, la tipizzazione forte consente di verificare la presenza di valori non validi eseguendo la convalida di XML rispetto allo schema. Questo comportamento può aiutarti a rilevare i problemi in precedenza. Il **tipizzazione sicura** opzione è disponibile per compatibilità con le versioni precedenti e controlla solo la lunghezza della stringa. Se si sceglie **tipizzazione sicura**, nel tipo DATS e digitare TIMS SAP vengono trattati come stringhe anziché come gli equivalenti XML `xs:date` e `xs:time` in cui `xmlns:xs="http://www.w3.org/2001/XMLSchema"`. Tipizzazione sicura influisce sul comportamento per tutte la generazione dello schema, il messaggio di trasmissione per il payload "stato inviato" e la risposta "ricevuto" sia trigger. 

Quando si usa la tipizzazione forte (**tipizzazione sicura** non abilitata), lo schema esegue il mapping di tipi DATS e TIMS ai tipi più semplici di XML:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Quando si inviano messaggi usando la tipizzazione forte, la risposta DATS e TIMS è conforme al formato di tipo XML corrisponda:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

Quando **tipizzazione sicura** è abilitata, lo schema viene eseguito il mapping di DATS e tipi TIMS in XML, ad esempio i campi con solo le restrizioni di lunghezza stringa:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

Durante l'invio di messaggi con **tipizzazione sicura** abilitata, la risposta DATS e TIMS simile all'esempio:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```


## <a name="known-issues-and-limitations"></a>Problemi noti e limitazioni

Di seguito sono riportati i problemi attualmente noti e le limitazioni per il connettore SAP:

* Solo una singola chiamata o messaggio Inviare a SAP funziona con tRFC. Il criterio di commit Business Application Programming Interface (BAPI), ad esempio effettuare più chiamate tRFC nella stessa sessione, non è supportato.

* Il trigger SAP non supporta la ricezione di IDOC batch da SAP. Questa azione potrebbe causare errori di connessione RFC tra il sistema SAP e il gateway dati.

* Il trigger SAP non supporta i cluster di gateway dati. In alcuni casi di failover, il nodo del gateway dati che comunica con il sistema SAP potrebbe essere diverso dal nodo attivo, causando un comportamento imprevisto. Per gli scenari di invio, sono supportati i cluster di gateway dati.

* Il connettore SAP attualmente non supporta le stringhe di route SAP. Il gateway dati locale deve esistere nella stessa rete LAN del sistema SAP a cui si desidera connettersi.

## <a name="connector-reference"></a>Informazioni di riferimento sui connettori

Per informazioni tecniche dettagliate sui limiti, azioni e trigger, che sono descritte da OpenAPI del connettore (in precedenza Swagger) descrizione, rivedere le [pagina di riferimento del connettore](/connectors/sap/).

## <a name="next-steps"></a>Passaggi successivi

* [Connettersi ai sistemi locali](../logic-apps/logic-apps-gateway-connection.md) dalle app per la logica
* Informazioni su come convalidare, trasformare e altre operazioni messaggi con [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Informazioni su altri [connettori di App per la logica](../connectors/apis-list.md)
