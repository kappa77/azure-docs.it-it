---
title: Scopri in che usano una sandbox di Apache Hadoop - emulatore - Azure HDInsight
description: "Per iniziare ad apprendere l'uso dell'ecosistema Apache Hadoop, è possibile impostare un ambiente sandbox Hadoop di Hortonworks in una macchina virtuale Azure. "
keywords: emulatore hadoop, sandbox hadoop
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: hrasheed
ms.openlocfilehash: 2cb99cfe765e1d3444f362e591812f5088c78c0e
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393140"
---
# <a name="get-started-with-an-apache-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Introduzione a una sandbox di Apache Hadoop, un emulatore in una macchina virtuale

Informazioni su come installare l'ambiente sandbox Apache Hadoop da Hortonworks in una macchina virtuale per acquisire familiarità con l'ecosistema di Hadoop. L'ambiente sandbox è un ambiente di sviluppo locale per informazioni su Hadoop, Hadoop Distributed File System (HDFS) e l'invio di processi. Dopo aver acquisito familiarità con Hadoop è possibile iniziare a usare Hadoop in Azure creando un cluster HDInsight. Per altre informazioni sulle attività iniziali, vedere l'articolo [Introduzione all'uso di Hadoop basato su Linux in HDInsight](apache-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Prerequisiti
* [Oracle VirtualBox](https://www.virtualbox.org/). Scaricare e installare l'app da [qui](https://www.virtualbox.org/wiki/Downloads).


## <a name="download-and-install-the-virtual-machine"></a>Scaricare e installare la macchina virtuale
1. Selezionare il [download di Cloudera](https://www.cloudera.com/downloads/hortonworks-sandbox/hdp.html).

2. Fare clic su **VIRTUALBOX** sotto **scegliere il tipo di installazione** per scaricare il più recente di Hortonworks Sandbox in una macchina virtuale. Accedere o completare il modulo di interesse del prodotto.

1. Fare clic sul pulsante **SANDBOX HDP (versione più recente)** per avviare il download.

Per istruzioni sulla configurazione di sandbox, vedere [Sandbox Guida alla distribuzione e installare](https://hortonworks.com/tutorial/sandbox-deployment-and-install-guide/section/1/).

Per scaricare un sandbox di versione HDP meno recenti, vedere i collegamenti nella sezione **le versioni precedenti**.

## <a name="start-the-virtual-machine"></a>Avviare la macchina virtuale

1. Aprire Oracle VM VirtualBox.
2. Scegliere **Import Appliance** dal menu **File** e quindi specificare l'immagine di Hortonworks Sandbox.
1. Selezionare Hortonworks Sandbox, fare clic su **Start** e quindi su **Normal Start**. Al termine del processo di avvio della macchina virtuale, vengono visualizzate le istruzioni di accesso.

    ![Avvio normale](./media/apache-hadoop-emulator-get-started/normal-start.png)
2. Aprire un web browser e passare all'URL visualizzato (in genere `http://127.0.0.1:8888`).

## <a name="set-sandbox-passwords"></a>Impostare le password Sandbox

1. Dal passaggio **introduttivo** della pagina di Sandbox di Hortonworks, selezionare **View Advanced Options** (Visualizza opzioni avanzate). Utilizzare le informazioni in questa pagina per accedere alla sandbox con SSH. Utilizzare il nome e la password forniti.

   > [!NOTE]
   > Se non è stato installato un client SSH, è possibile usare l'SSH basato sul Web fornito dalla macchina virtuale all'indirizzo **http://localhost:4200/** .

    Al primo collegamento tramite SSH viene richiesto di cambiare la password per l'account radice. Immettere una nuova password da usare quando si accede tramite SSH.

2. Dopodiché immettere il comando seguente:

        ambari-admin-password-reset

    Quando richiesto, fornire una password per l'account di amministratore di Ambari. Questo viene utilizzato quando si accede all'interfaccia utente Web di Ambari.

## <a name="use-hive-commands"></a>Usare i comandi Hive

1. Da una connessione SSH a Sandbox, utilizzare il comando seguente per avviare la shell di Hive:

        hive
2. Una volta avviata la shell, utilizzarla per visualizzare le tabelle fornite con Sandbox:

        show tables;
3. Usare il codice seguente per recuperare 10 righe dalla tabella `sample_07` :

        select * from sample_07 limit 10;

## <a name="next-steps"></a>Passaggi successivi
* [Informazioni su come utilizzare Visual Studio con Sandbox di Hortonworks](../hdinsight-hadoop-emulator-visual-studio.md)
* [Acquisire dimestichezza con Sandbox di Hortonworks](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Esercitazione di Hadoop: introduzione a HDP](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

