---
title: Gestire l'accesso alle risorse di Azure usando il controllo degli accessi in base al ruolo e i modelli di Azure Resource Manager | Microsoft Docs
description: Informazioni su come gestire l'accesso alle risorse di Azure per utenti, gruppi e applicazioni tramite il controllo degli accessi in base al ruolo e i modelli di Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 537ee35e96a41cd02605319e244d39c6567c3bf1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60344590"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>Gestire l'accesso alle risorse di Azure usando il controllo degli accessi in base al ruolo e i modelli di Azure Resource Manager

Il [controllo degli accessi in base al ruolo](overview.md) è la modalità di gestione dell'accesso alle risorse di Azure. Oltre a usare Azure PowerShell o l'interfaccia della riga di comando di Azure, è possibile gestire l'accesso alle risorse di Azure tramite RBAC e [modelli di Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). I modelli possono essere usati per distribuire le risorse in modo coerente e ripetuto. Questo articolo descrive come è possibile gestire l'accesso tramite RBAC e modelli.

## <a name="example-template-to-create-a-role-assignment"></a>Modello di esempio per creare un'assegnazione di ruolo

Per concedere l'accesso mediante il controllo degli accessi in base al ruolo, si crea un'assegnazione di ruolo. Il modello seguente illustra:
- Come assegnare un ruolo a un utente, un gruppo o un applicazione nell'ambito di un gruppo di risorse
- Come specificare i ruoli proprietario, collaboratore e lettore come parametro

Per usare il modello, è necessario specificare gli input seguenti:
- Il nome di un gruppo di risorse
- L'identificatore univoco dell'utente, del gruppo o dell'applicazione che è stato assegnato al ruolo
- Il ruolo da assegnare
- Un identificatore univoco che verrà usato per l'assegnazione di ruolo

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "scope": "[resourceGroup().id]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2017-05-01",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('scope')]"
      }
    }
  ]
}
```

Di seguito viene riportato un esempio di un'assegnazione di ruolo lettore a un utente dopo la distribuzione del modello.

![Assegnazione di ruolo usando un modello](./media/role-assignments-template/role-assignment-template.png)

## <a name="deploy-template-using-azure-powershell"></a>Distribuire il modello mediante Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Per distribuire il modello precedente con Azure PowerShell, seguire questa procedura.

1. Creare un nuovo file denominato rbac-rg.json e copiare il modello precedente.

1. Accedere ad [Azure PowerShell](/powershell/azure/authenticate-azureps).

1. Recuperare l'identificatore univoco dell'utente, del gruppo o dell'applicazione. Ad esempio, è possibile usare il comando [Get-AzADUser](/powershell/module/az.resources/get-azaduser) per elencare gli utenti di Azure AD.

    ```azurepowershell
    Get-AzADUser
    ```

1. Usare uno strumento GUID per generare un identificatore univoco che verrà usato per l'assegnazione di ruolo. Il formato dell'identificatore è: `11111111-1111-1111-1111-111111111111`

1. Creare un gruppo di risorse di esempio.

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Usare il comando [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) per avviare la distribuzione.

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    ```

    Viene richiesto di specificare i parametri necessari. Il testo seguente è un esempio di output.

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    
    cmdlet New-AzResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    principalId: 22222222-2222-2222-2222-222222222222
    builtInRoleType: Reader
    roleNameGuid: 11111111-1111-1111-1111-111111111111
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="deploy-template-using-the-azure-cli"></a>Distribuire il modello tramite l'interfaccia della riga di comando di Azure

Per distribuire il modello precedente con interfaccia della riga di comando di Azure, seguire questa procedura.

1. Creare un nuovo file denominato rbac-rg.json e copiare il modello precedente.

1. Accedere a [Interfaccia della riga di comando di Azure](/cli/azure/authenticate-azure-cli).

1. Recuperare l'identificatore univoco dell'utente, del gruppo o dell'applicazione. Ad esempio, è possibile usare il comando [az ad user list](/cli/azure/ad/user#az-ad-user-list) per elencare gli utenti di Azure AD.

    ```azurecli
    az ad user list
    ```

1. Usare uno strumento GUID per generare un identificatore univoco che verrà usato per l'assegnazione di ruolo. Il formato dell'identificatore è: `11111111-1111-1111-1111-111111111111`

1. Creare un gruppo di risorse di esempio.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Usare il comando [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) per avviare la distribuzione.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    ```

    Viene richiesto di specificare i parametri necessari. Il testo seguente è un esempio di output.

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    Please provide string value for 'principalId' (? for help): 22222222-2222-2222-2222-222222222222
    Please provide string value for 'builtInRoleType' (? for help):
     [1] Owner
     [2] Contributor
     [3] Reader
    Please enter a choice [1]: 3
    Please provide string value for 'roleNameGuid' (? for help): 11111111-1111-1111-1111-111111111111
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```
    
## <a name="next-steps"></a>Passaggi successivi

- [Guida introduttiva: Creare e distribuire modelli di Azure Resource Manager con il portale di Azure](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [Comprendere la struttura e la sintassi dei modelli di Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
- [Modelli di avvio rapido di Azure](https://azure.microsoft.com/resources/templates/?term=rbac)
