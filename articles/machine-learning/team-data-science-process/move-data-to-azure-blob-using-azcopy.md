---
title: Copiare i dati dell'archivio BLOB con AzCopy - Processo di data science per i team
description: Copiare dati da e verso Archivio BLOB di Azure tramite AzCopy
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6c0951eb6ad3b7651da97e1a49c5edf5ab55a199
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61044349"
---
# <a name="copy-data-to-and-from-azure-blob-storage-using-azcopy"></a>Copiare dati da e verso Archivio BLOB di Azure tramite AzCopy
AzCopy è un'utilità della riga di comando progettata per le operazioni di caricamento, download e copia dei dati in e da servizi di archiviazione BLOB, file e tabelle di Microsoft Azure.

Per istruzioni sull'installazione di AzCopy e informazioni aggiuntive sull'utilizzo della piattaforma Azure, vedere [Introduzione all’utilità della riga di comando di AzCopy](../../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Se si utilizza una macchina virtuale che è stata impostata con gli script forniti da [Macchine virtuali della scienza dei dati in Azure](virtual-machines.md), allora AzCopy è già installata nella macchina virtuale.
> 
> [!NOTE]
> Per un'introduzione completa all'archiviazione BLOB di Azure, vedere [Introduzione all'archivio BLOB di Azure](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) e [Servizio BLOB di Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Prerequisiti
In questo documento si presuppone di avere una sottoscrizione di Azure, un account di archiviazione e delle chiavi di archiviazione corrispondenti per quell'account. Prima di caricare/scaricare i dati, è necessario conoscere il nome e la chiave del proprio account di archiviazione di Azure.

* Per configurare una sottoscrizione di Azure, vedere [Versione di prova gratuita di un mese](https://azure.microsoft.com/pricing/free-trial/).
* Per istruzioni sulla creazione di un account di archiviazione e per ottenere informazioni sull’account e la chiave, vedere [Informazioni sugli account di archiviazione di Azure](../../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>Eseguire i comandi di AzCopy
Per eseguire i comandi di AzCopy, aprire una finestra di comando e passare alla directory di installazione di AzCopy contenente il file eseguibile AzCopy.exe. 

La sintassi di base per i comandi di AzCopy è la seguente:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> È possibile aggiungere il percorso di installazione di AzCopy al percorso di sistema ed eseguire i comandi da qualsiasi directory. Per impostazione predefinita, AzCopy viene installato in *%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy* o in *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-to-an-azure-blob"></a>Caricare file in un BLOB di Azure
Per caricare un file, usare il comando seguente:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Scaricare file da un BLOB di Azure
Per scaricare un file da un BLOB di Azure, utilizzare il comando seguente:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="copy-blobs-between-azure-containers"></a>Copiare BLOB tra contenitori di Azure
Per copiare BLOB tra contenitori di Azure, usare il comando seguente:

    # Copying blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be copied. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Suggerimenti per l'utilizzo di AzCopy
> [!TIP]
> 1. Quando si **caricano** file, */S* caricherà i file in modo ricorsivo. Senza questo parametro, i file nelle sottodirectory non vengono caricati.  
> 2. Durante il **download** di file, */S* cerca il contenitore in modo ricorsivo fino a quando non vengono scaricati tutti i file nella directory specificata e nelle relative sottodirectory o tutti i file corrispondenti al criterio specificato nella directory data e nelle relative sottodirectory.  
> 3. Non è possibile specificare un **file BLOB specifico** da scaricare utilizzando il parametro */Source* . Per scaricare un file specifico, specificare il nome del file BLOB per il download utilizzando il parametro */Pattern* . **/S** può essere utilizzato per fare in modo che AzCopy cerchi il nome del file in modo ricorsivo. Senza il parametro pattern, AzCopy scarica tutti i file nella directory.
> 
> 

