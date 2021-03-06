---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 19331f35ea2fa773325ec61e728677e37767ab54
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156389"
---
> [!div class="op_single_selector"]
> * [Dispositivo: Node. js Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Dispositivo: C#Servizio: C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Dispositivo: Servizio di linguaggio: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Dispositivo: Servizio Python: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Le app back-end possono usare primitive dell'hub IoT di Azure, ad esempio i [metodi diretti][lnk-c2dmethod] e il [dispositivo gemello][lnk-devtwin], per avviare e monitorare le azioni di gestione nei dispositivi in modalità remota. Questa esercitazione mostra il funzionamento combinato di un'app back-end e un'app per dispositivo per avviare e monitorare il riavvio remoto di un dispositivo tramite l'hub IoT.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Usare un metodo diretto per avviare le operazioni di gestione dei dispositivi, ad esempio il riavvio, il ripristino delle impostazioni predefinite e l'aggiornamento del firmware, da un'applicazione back-end nel cloud. Il dispositivo è responsabile per:

* La gestione della richiesta di metodo inviata dall'hub IoT.
* L'avvio, nel dispositivo, dell'azione corrispondente specifica del dispositivo.
* Gli aggiornamenti di stato tramite le *proprietà segnalate* nell'hub IoT.

È possibile usare un'applicazione back-end nel cloud per eseguire query di un dispositivo gemello in modo da creare report sullo stato di avanzamento delle operazioni di gestione del dispositivo.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
