---
title: Configurare una risposta personalizzata per web application firewall all'ingresso principale di Azure
description: Informazioni su come configurare un codice di risposta personalizzata e un messaggio quando il firewall applicazione web (WAF) Blocca una richiesta.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: tyao;kumud
ms.openlocfilehash: d6d73055abe972cd3b6fee253b6bdb2b082ceca8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242981"
---
# <a name="configure-a-custom-response-for-azure-web-application-firewall"></a>Configurare una risposta personalizzata per il firewall applicazione web di Azure

Per impostazione predefinita, quando il firewall applicazione web di Azure (WAF) con l'ingresso principale di Azure consente di bloccare una richiesta a causa di una regola corrispondente, restituisce un codice di 403 stato con **la richiesta è bloccata** messaggio. Questo articolo descrive come configurare un codice di stato risposta personalizzata e un messaggio di risposta quando una richiesta è bloccata da Web Application firewall.

## <a name="set-up-your-powershell-environment"></a>Configurare l'ambiente PowerShell
Azure PowerShell offre un set di cmdlet che usano il modello [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) per la gestione delle risorse di Azure. 

È possibile installare [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) nel computer locale e usarlo in qualsiasi sessione di PowerShell. Seguire le istruzioni nella pagina per accedere con le proprie credenziali di Azure e installare il modulo Az PowerShell.

### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Connettersi ad Azure con una finestra di dialogo interattiva per l'accesso
```
Connect-AzAccount
Install-Module -Name Az
```
Assicurarsi di avere la versione corrente di PowerShellGet installata. Eseguire il comando seguente e riaprire PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
### <a name="install-azfrontdoor-module"></a>Installare il modulo Az.FrontDoor 

```
Install-Module -Name Az.FrontDoor
```

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

In Azure, si allocano le risorse correlate a un gruppo di risorse. In questo esempio, creare un gruppo di risorse usando [New-AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup).

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupWAF
```

## <a name="create-a-new-waf-policy-with-custom-response"></a>Creare un nuovo criterio di Web Application firewall con risposta personalizzata 

Di seguito è riportato un esempio di creazione di un nuovo criterio di Web Application firewall con codice di stato risposta personalizzata impostata su messaggio e 405 **sia bloccato.** using [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```azurepowershell
# WAF policy setting
New-AzFrontDoorWafPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 405 `
-CustomBlockResponseBody "<html><head><title>You are blocked.</title></head><body></body></html>"
```

Modificare il codice di risposta personalizzata o le impostazioni di corpo della risposta di un criterio di Web Application firewall esistente, usando [Update-AzFrontDoorFireWallPolicy ](/powershell/module/az.frontdoor/Update-AzFrontDoorWafPolicy).

```azurepowershell
# modify WAF response code
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 403
```

```azurepowershell
# modify WAF response body
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-CustomBlockResponseBody "<html><head><title> Forbidden</title></head><body></body></html>"
```

## <a name="next-steps"></a>Passaggi successivi
- Altre informazioni su [ingresso principale](front-door-overview.md)