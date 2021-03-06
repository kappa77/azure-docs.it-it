---
title: Creare una Data Science Virtual Machine per l'apprendimento avanzato
titleSuffix: Azure
description: Configurare e creare una macchina virtuale di data science per l'apprendimento avanzato in Azure per l'analisi e l'apprendimento automatico.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 318df03c7c4447d051dfa396098462c0f8bbf423
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410447"
---
# <a name="provision-a-deep-learning-virtual-machine-on-azure"></a>Effettuare il provisioning di una macchina virtuale per l'apprendimento avanzato in Azure 

La macchina virtuale per l'apprendimento avanzato (DLVM, Deep Learning Virtual Machine) è una variante configurata in modo particolare della più diffusa [macchina virtuale per data science](https://aka.ms/dsvm) (DSVM, Data Science Virtual Machine). Tale variante semplifica l'uso delle istanze di VM basate su GPU per il training rapido dei modelli di apprendimento avanzato. Supportata dalla DSVM Windows 2016 o Ubuntu, la DLVM condivide le stesse immagini di macchina virtuale di base e quindi tutti i set di strumenti avanzati disponibili nella DSVM. 

La DLVM contiene diversi strumenti per intelligenza artificiale, incluse le edizioni GPU dei framework di apprendimento avanzato più diffusi, ad esempio Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2, Chainer, nonché strumenti per l'acquisizione e la pre-elaborazione di immagini e dati testuali, strumenti per attività di modellazione e sviluppo di data science, ad esempio Microsoft R Server Developer Edition, Python Anaconda, Jupyter Notebook per Python e R, IDE per Python e R, database SQL e molti altri strumenti di data science e Machine Learning. 

## <a name="create-your-deep-learning-virtual-machine"></a>Creare una macchina virtuale per l'apprendimento avanzato
Ecco i passaggi necessari per creare un'istanza della macchina virtuale per l'apprendimento avanzato: 

1. Passare alla macchina virtuale nel [portale di Azure](https://portal.azure.com/#create/microsoft-ads.dsvm-deep-learningtoolkit
).
2. Fare clic sul pulsante **Crea** nella parte inferiore per visualizzare una procedura guidata.![create-dlvm](./media/dlvm-provision-wizard.PNG)
3. La procedura guidata usata per creare la DLVM richiede **input** per ognuno dei **quattro passaggi** elencati in ordine numerico sul lato destro della figura. Di seguito sono riportati gli input necessari per configurare ciascuno di questi passaggi:

   <a name="basics"></a>   
   1. **Nozioni di base**
      
      1. **Nome**: nome del server di data science che si sta creando.
      2. **Select OS type for the Deep Learning VM** (Selezionare il tipo di sistema operativo per la VM di apprendimento avanzato): scegliere Windows o Linux (per la DSVM di base Windows 2016 e Ubuntu Linux)
      2. **Nome utente**: ID di accesso dell'account amministratore.
      3. **Password**: password dell'account amministratore.
      4. **Sottoscrizione** Se si ha più di una sottoscrizione, selezionare quella in cui viene creata e fatturata la macchina virtuale.
      5. **Gruppo di risorse**: è possibile creare un nuovo gruppo di risorse di Azure oppure usarne uno **vuoto** esistente nella sottoscrizione.
      6. **Località**: selezionare il data center più appropriato. In genere è il data center contenente la maggior parte dei dati o più vicino alla posizione fisica per garantire la massima velocità di accesso alla rete. 
      
      > [!NOTE]
      > Il DLVM supporta tutte le istanze delle macchine virtuali GPU serie NC e ND. Durante il provisioning del DLVM, è necessario scegliere una delle posizioni in Azure che dispongono di GPU. Controllare la [pagina dei Prodotti Azure per area](https://azure.microsoft.com/regions/services/) per le posizioni disponibili e cercare **Serie NC**, **Serie NCv2**, **Serie NCv3** o **Serie ND** sotto **Calcolo**. 

   1. **Impostazioni**: selezionare una macchina virtuale GPU serie NC (NC, NCv2, NCv3) o ND che soddisfi i requisiti funzionali e i vincoli di costo. Creare un account di archiviazione per la macchina virtuale.  ![dlvm-settings](./media/dlvm-provision-step-2.PNG)
   
   1. **Riepilogo**: Verificare che tutte le informazioni immesse siano corrette.

   1. **Acquisto**: fare clic su **Acquista** per avviare il provisioning. Viene fornito un collegamento alle condizioni della transazione. La macchina virtuale non prevede costi aggiuntivi oltre a quelli per il calcolo delle dimensioni del server scelto nel passaggio **Size** . 

> [!NOTE]
> Per il provisioning sono necessari circa 10-20 minuti. Lo stato del provisioning viene visualizzato nel portale di Azure.
> 


## <a name="how-to-access-the-deep-learning-virtual-machine"></a>Come accedere alla macchina virtuale per l'apprendimento avanzato

### <a name="windows-edition"></a>Edizione per Windows
Dopo aver creato la VM, è possibile connettersi tramite desktop remoto con le credenziali dell'account amministratore configurato nella sezione **Nozioni di base** precedente. 

### <a name="linux-edition"></a>Edizione per Linux

Dopo aver creato la VM, è possibile accedervi tramite SSH. Usare le credenziali dell'account creato nel [ **nozioni di base** ](#basics) sezione del passaggio 3 per l'interfaccia della shell di testo. Per altre informazioni sulle connessioni SSH alle macchine virtuali di Azure, vedere [installare e configurare Desktop remoto per connettersi a una VM Linux in Azure](/azure/virtual-machines/linux/use-remote-desktop). In un client Windows, è possibile scaricare uno strumento client SSH come [Putty](https://www.putty.org). Se si preferisce un desktop con interfaccia grafica (X Windows System), è possibile usare X11 Forwarding su Putty o installare il client X2Go. 

> [!NOTE]
> Nei test Microsoft il client X2Go ha fornito prestazioni migliori di X11 Forwarding. È quindi consigliabile usare il client X2Go per un'interfaccia desktop grafica.
> 
> 

#### <a name="installing-and-configuring-x2go-client"></a>Installazione e configurazione del client X2Go
Per la DLVM Linux è già stato effettuato il provisioning con il server X2Go. La DLVM è pronta ad accettare connessioni client. Per connettersi al desktop con interfaccia grafica della VM Linux, è necessario completare la procedura seguente nel client:

1. Scaricare e installare il client X2Go per la piattaforma client da [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Eseguire il client X2Go e selezionare **New Session**(Nuova sessione). Viene visualizzata una finestra di configurazione con più schede. Immettere i parametri di configurazione seguenti:
   * **Scheda Session**(Sessione):
     * **Host**: nome host o indirizzo IP della Data Science VM per Linux.
     * **Accesso**: nome utente della VM Linux.
     * **Porta SSH**: lasciare il valore predefinito 22.
     * **Session Type** (Tipo di sessione): modificare il valore in **XFCE**. La DSVM Linux attualmente supporta solo l'ambiente desktop XFCE.
   * **Scheda Media** (Supporti): è possibile disattivare il supporto audio e la stampa client se non è necessario usarli.
   * **Shared folders** (Cartelle condivise): se si prevede di montare directory dei computer client nella VM Linux, aggiungere in questa scheda le directory dei computer client da condividere con la VM.

Dopo aver eseguito l'accesso alla VM con il client SSH o il desktop con interfaccia grafica XFCE tramite il client X2Go, è possibile iniziare a usare gli strumenti installati e configurati nella VM. In XFCE è possibile visualizzare i collegamenti di menu delle applicazioni e le icone del desktop per molti di questi strumenti.

Una volta creata la VM ed effettuato il provisioning, si è pronti per iniziare a usare gli strumenti installati e configurati nella VM. Sono disponibili riquadri del menu di avvio e icone del desktop per molti strumenti. 
