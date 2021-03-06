---
title: Attività Get Metadata in Azure Data Factory | Microsoft Docs
description: Informazioni su come è possibile usare l'attività GetMetadata in una pipeline di Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: jingwang
ms.openlocfilehash: 78f63b4f46fe5479d4d0fd5849ad80536d8a137c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61346903"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Attività Get Metadata in Azure Data Factory

Questa attività può essere usata per recuperare i **metadati** di tutti i dati in Azure Data Factory. Questa attività può essere usata negli scenari seguenti:

- Convalidare le informazioni sui metadati di tutti i dati
- Attivare una pipeline quando i dati sono pronti o disponibili

La funzionalità seguente è disponibile nel flusso di controllo:

- L'output dall'attività GetMetadata può essere usato nelle espressioni condizionali per eseguire la convalida.
- Una pipeline può essere attivata quando viene soddisfatta una condizione specificata nel ciclo Do-Until

## <a name="supported-capabilities"></a>Funzionalità supportate

L'attività GetMetadata accetta un set di dati come input obbligatorio e restituisce informazioni sui metadati disponibili come output dell'attività. Attualmente sono supportati i connettori seguenti, con i corrispondenti metadati recuperabili; la dimensione massima supportata per i metadati è **1 MB**.

>[!NOTE]
>Se si esegue l'attività GetMetadata in un runtime di integrazione self-hosted, la funzionalità più recente è supportata a partire dalla versione 3.6. 

### <a name="supported-connectors"></a>Connettori supportati

**Archiviazione file:**

| Connettore/Metadati | itemName<br>(file/cartella) | itemType<br>(file/cartella) | size<br>(file) | created<br>(file/cartella) | LastModified<br>(file/cartella) |childItems<br>(cartella) |contentMD5<br>(file) | structure<br/>(file) | columnCount<br>(file) | exists<br>(file/cartella) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Amazon S3 | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| Google Cloud Storage | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| BLOB Azure | √/√ | √/√ | √ | x/x | √/√* | √ | √ | √ | √ | √/√ |
| Azure Data Lake Storage Gen1 | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure Data Lake Storage Gen2 | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Archiviazione file di Azure | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| File system | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| SFTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| FTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Per Amazon S3 e Google Sloud archiviazione, il `lastModified` si applica ai bucket e chiave, ma non virtuale cartella; e `exists` si applica ai bucket e chiave, ma non con prefisso o cartella virtuale.
- Per BLOB di Azure, `lastModified` si applica al contenitore e al BLOB, ma non alla cartella virtuale.

**Database relazionale:**

| Connettore/Metadati | structure | columnCount | exists |
|:--- |:--- |:--- |:--- |
| Database SQL di Azure | √ | √ | √ |
| Istanza gestita di database SQL di Azure | √ | √ | √ |
| Azure SQL Data Warehouse | √ | √ | √ |
| SQL Server | √ | √ | √ |

### <a name="metadata-options"></a>Opzioni dei metadati

Nell'elenco dei campi attività GetMetadata da recuperare è possono specificare i tipi di metadati seguenti:

| Tipo di metadati | DESCRIZIONE |
|:--- |:--- |
| itemName | Nome del file o della cartella. |
| itemType | Tipo di file o di cartella. Il valore di output è `File` o `Folder`. |
| size | Dimensioni del file in byte. Applicabile soltanto al file. |
| created | Data/ora di creazione del file o della cartella. |
| LastModified | Data/ora dell'ultima modifica del file o della cartella. |
| childItems | Elenco di sottocartelle e file all'interno della cartella specificata. Applicabile solo alla cartella. Il valore di output è un elenco di nomi e tipo di ogni elemento figlio. |
| contentMD5 | MD5 del file. Applicabile soltanto al file. |
| structure | Struttura di dati all'interno del file o della tabella di database relazionale. Il valore di output è un elenco di nomi di colonna e di tipi di colonna. |
| columnCount | Numero di colonne all'interno del file o della tabella relazionale. |
| exists| Indica se una file/cartella/tabella esiste o meno. Notare che se exists viene specificato nell'elenco dei campi GetMetadata, l'attività non ha esito negativo anche se l'elemento (file/cartella/tabella) non esiste. Nell'output viene invece restituito il valore `exists: false`. |

>[!TIP]
>Per convalidare se un file/cartella/tabella esiste o meno, specificare `exists` nell'elenco dei campi attività GetMetadata; a questo punto è possibile controllare il risultato `exists: true/false` dell'output dell'attività. Se `exists` non è configurato nell'elenco dei campi, l'attività GetMetadata avrà esito negativo se l'oggetto non viene trovato.

## <a name="syntax"></a>Sintassi

**Attività GetMetadata:**

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```

**Set di dati:**

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Proprietà del tipo

Attualmente l'attività GetMetadata può recuperare i tipi di informazioni dei metadati seguenti.

Proprietà | Descrizione | Obbligatoria
-------- | ----------- | --------
fieldList | Elenca i tipi di informazioni dei metadati necessarie. Vedere i dettagli nella sezione [Opzioni dei metadati](#metadata-options) sui metadati supportati. | Sì 
dataset | Set di dati di riferimento la cui attività dei metadati deve essere recuperata dall'attività GetMetadata. Vedere la sezione relativa alle [funzionalità supportate](#supported-capabilities) sui connettori supportati e fare riferimento all'argomento sui connettori nei dettagli sulla sintassi del set di dati. | Sì

## <a name="sample-output"></a>Output di esempio

Il risultato di GetMetadata è mostrato nell'output dell'attività. Di seguito due esempi con opzioni di metadati complete selezionate nell'elenco dei campi come riferimento. Per usare il risultato in un'attività successiva, usare il criterio di `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>Ottenere i metadati di un file

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>Ottenere i metadati di una cartella

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Passaggi successivi
Vedere altre attività del flusso di controllo supportate da Data Factory: 

- [Attività ExecutePipeline](control-flow-execute-pipeline-activity.md)
- [Attività ForEach](control-flow-for-each-activity.md)
- [Attività Lookup](control-flow-lookup-activity.md)
- [Attività Web](control-flow-web-activity.md)
