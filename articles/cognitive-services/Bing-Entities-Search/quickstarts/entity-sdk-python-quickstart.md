---
title: 'Guida introduttiva: SDK di Ricerca entità Bing, Python'
titlesuffix: Azure Cognitive Services
description: Configurazione per l'applicazione console Bing Entity Search SDK.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 9a66df03ceb03885ba5f61b4adeb54cb1876338d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813621"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Guida introduttiva: SDK di Ricerca entità Bing con Python

Usare questa guida introduttiva per iniziare a cercare entità con l'SDK di Ricerca entità Bing per Python. Mentre Ricerca entità Bing ha un'API REST compatibile con la maggior parte dei linguaggi di programmazione, l'SDK offre un modo semplice per integrare il servizio nelle applicazioni. Il codice sorgente per questo esempio è disponibile su [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py).

## <a name="prerequisites"></a>Prerequisiti

* Python [2.x o 3.x](https://www.python.org/)

* [SDK di Ricerca entità Bing per Python](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

È consigliabile usare un ambiente virtuale Python. È possibile installare e inizializzare un ambiente virtuale con il modulo venv. È possibile installare virtualenv per Python 2.7 con:

```Console
python -m venv mytestenv
```

Installare l'SDK di Ricerca entità Bing con:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creare e inizializzare l'applicazione

1. Creare un nuovo file Python nell'IDE o nell'editor preferito e aggiungere le istruzioni di importazione seguenti. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchAPI
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Creare una variabile per la chiave della sottoscrizione e usarla per creare un nuovo oggetto `CognitiveServicesCredentials` e creare un'istanza del client.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Inviare una richiesta di ricerca e ricevere una risposta

1. Inviare una richiesta di ricerca a Ricerca entità Bing con `client.entities.search()` e una query di ricerca. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Se vengono restituite entità, convertire `entity_data.entities.value` in un elenco e stampare il primo risultato.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare app Web a pagina singola](../tutorial-bing-entities-search-single-page-app.md)

* [Informazioni sull'API Ricerca entità Bing](../overview.md )