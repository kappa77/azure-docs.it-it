---
title: File di inclusione
description: File di inclusione
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: include file
ms.openlocfilehash: a4b0c2e2d7732c6d2a4f57411731f7145cf3e73e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967662"
---
## <a name="register-your-application"></a>Registra l'applicazione

È possibile registrare l'applicazione in uno dei due modi descritti nelle due sezioni successive.

### <a name="option-1-express-mode"></a>Opzione 1: modalità Rapida

È ora necessario registrare l'applicazione nel *portale di registrazione delle applicazioni Microsoft*:

1. Registrare l'applicazione tramite il [portale di registrazione delle applicazioni Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure).
2. Immettere un nome per l'applicazione e l'indirizzo di posta elettronica.
3. Assicurarsi che l'opzione per l'installazione guidata sia selezionata.
4. Seguire le istruzioni per ottenere l'ID dell'applicazione e incollarlo nel codice.

### <a name="option-2-advanced-mode"></a>Opzione 2: modalità Avanzata

1. Passare al [Portale di registrazione delle applicazioni di Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. Immettere un nome per l'applicazione.
3. Assicurarsi che l'opzione per l'installazione guidata sia deselezionata.
4. Selezionare `Add Platform` e quindi `Native Application`.
5. Selezionare `Save`.
6. Tornare a Xcode. In `ViewController.swift` sostituire la riga che inizia con "`let kClientID`" con l'ID dell'applicazione appena registrata:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Digitale Control+ clic su <code>Info.plist</code> per visualizzare il menu di scelta rapida e quindi fare clic su: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
Sotto il nodo radice <code>dict</code> aggiungere quanto segue:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Sostituire <i><code>[Your_Application_Id_Here]</code></i> con l'ID applicazione appena registrato
</li>
</ol>
