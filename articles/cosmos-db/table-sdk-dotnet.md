---
title: SDK .NET e risorse dell'API Tabella di Azure Cosmos DB
description: Informazioni sull'API Table di Azure Cosmos DB, incluse le date di rilascio, le date di ritiro e le modifiche apportate nelle diverse versioni.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/17/2018
ms.openlocfilehash: db7cc556525ab57f14984232bf1797764865fca3
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65606244"
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>API .NET tabella per Azure Cosmos DB: download e note sulla versione

> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**Download dell'SDK**|[NuGet](https://aka.ms/acdbtablenuget)|
|**Documentazione sull'API**|[Documentazione di riferimento API .NET](https://aka.ms/acdbtableapiref)|
|**Guida introduttiva**|[Azure Cosmos DB: compilare un'app con .NET e l'API Tabella](create-table-dotnet.md)|
|**Esercitazione**|[Azure Cosmos DB: sviluppare con l'API Tabella in .NET](tutorial-develop-table-dotnet.md)|
|**Framework attualmente supportato**|[Microsoft .NET Framework 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=40779)|

> [!IMPORTANT]
> .NET Framework SDK [cosmosdb](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) è in modalità manutenzione modalità e lo saranno presto deprecati. Eseguire l'aggiornamento alla nuova libreria .NET Standard [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) per continuare a ottenere le funzionalità più recenti supportate dall'API di tabella.

> Se si è creato un account dell'API di tabella durante l'anteprima, creare un [nuovo account dell'API di tabella](create-table-dotnet.md#create-a-database-account) per usare gli SDK dell'API di tabella disponibili a livello generale.
>

## <a name="release-notes"></a>Note sulla versione

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* Correzioni di bug

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* È stato aggiunto il supporto per la scrittura in più aree
* Sono state corrette le dipendenze dei pacchetti NuGet da Microsoft.Azure.DocumentDB, Microsoft.OData.Core, Microsoft.OData.Edm, Microsoft.Spatial

### <a name="a-name113113"></a><a name="1.1.3"/>1.1.3

* Dipendenze risolte dei pacchetti NuGet in Microsoft.Azure.Storage.Common e Microsoft.Azure.DocumentDB.
* Correzioni di bug nella serializzazione di tabella quando JsonConvert.DefaultSettings sono configurati.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* È stata aggiunta la convalida per ETag in formato non valido in modalità diretta.
* È stato corretto il bug relativo alle query LINQ in modalità gateway.
* Le API sincrone vengono ora eseguite sul pool di thread con SynchronizationContext.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Aggiunta di TableQueryMaxItemCount, TableQueryEnableScan, TableQueryMaxDegreeOfParallelism e TableQueryContinuationTokenLimitInKb a TableRequestOptions
* Correzioni di bug

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Versione con disponibilità generale

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-preview

* Versione di anteprima iniziale

## <a name="release-and-retirement-dates"></a>Date di rilascio e di ritiro

Microsoft invia una notifica almeno **12 mesi** prima del ritiro di un SDK per agevolare la transizione a una versione più recente o supportata.

Il `Microsoft.Azure.CosmosDB.Table` libreria è attualmente disponibile per .NET Framework e si trova in modalità di manutenzione e saranno presto deprecata. Nuove funzionalità e le funzionalità e le ottimizzazioni vengono aggiunte solo alla libreria .NET Standard [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table), come tale è consigliabile eseguire l'aggiornamento a [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table).

Il [windowsazure. Storage-PremiumTable](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable/0.1.0-preview) pacchetto di anteprima è stata deprecata. L'SDK WindowsAzure.Storage-PremiumTable verrà ritirato il 15 novembre 2018. Da quella data, le richieste all'SDK ritirato non saranno consentite. 

Qualsiasi richiesta inviata ad Azure Cosmos DB con un SDK ritirato viene rifiutata dal servizio.
<br/>

| Version | Data di rilascio | Data di ritiro |
| --- | --- | --- |
| [2.1.0](#2.1.0) |22 gennaio 2019|01 aprile 2020 |
| [2.0.0](#2.0.0) |26 settembre 2018|01 marzo 2020 |
| [1.1.3](#1.1.3) |17 luglio 2018|01 dicembre 2019 |
| [1.1.1](#1.1.1) |26 marzo 2018|01 dicembre 2019 |
| [1.1.0](#1.1.0) |21 febbraio 2018|01 dicembre 2019 |
| [1.0.0](#1.0.0) |15 novembre 2017|Dal 15 novembre 2019 |
| 0.9.0-preview |11 novembre 2017 |11 novembre 2019 |

## <a name="troubleshooting"></a>risoluzione dei problemi

Se si ottiene l'errore 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

Quando si tenta di usare il pacchetto NuGet Microsoft.Azure.CosmosDB.Table, sono disponibili due opzioni per risolvere il problema:

* Usare la console di gestione pacchetti per installare il pacchetto Microsoft.Azure.CosmosDB.Table e le relative dipendenze. A tale scopo, digitare quanto segue nella console di gestione pacchetti per la soluzione. 

    ```powershell
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```

    
* Usando lo strumento di gestione pacchetti NuGet preferito, installare il pacchetto Microsoft.Azure.Storage.Common NuGet prima di installare Microsoft.Azure.CosmosDB.Table.

## <a name="faq"></a>Domande frequenti

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Vedere anche 

Per altre informazioni sull'API Table di Azure Cosmos DB, vedere [Introduzione ad Azure Cosmos DB: API Table](table-introduction.md). 