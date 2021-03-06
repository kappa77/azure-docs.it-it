---
title: Domande frequenti relative al servizio Azure Kubernetes
description: Trova le risposte ad alcune delle domande frequenti su Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 6bfcd11dd6bfd31583fb2d0cd3f4229d3dd70065
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2019
ms.locfileid: "65887372"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Domande frequenti relative al servizio Azure Kubernetes

Questo articolo tratta alcune domande frequenti relative al servizio Azure Kubernetes.

## <a name="which-azure-regions-currently-provide-aks"></a>Quali aree di Azure forniscono attualmente AKS?

Per un elenco completo delle aree disponibili, vedere [AKS aree e disponibilità][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>servizio Azure Kubernetes supporta la scalabilità automatica dei nodi?

Sì, la scalabilità automatica è disponibile tramite il [ridimensionamento automatico di Kubernetes] [ auto-scaler] al momento della stesura 1.10 Kubernetes. Per informazioni su come configurare e usare la scalabilità automatica del cluster manualmente, vedere [scalabilità automatica di Cluster in AKS][aks-cluster-autoscale].

È inoltre possibile utilizzare il ridimensionamento automatico predefinita del cluster (attualmente in anteprima nel servizio contenitore di AZURE) per gestire la scalabilità dei nodi. Per altre informazioni, vedere [ridimensionare automaticamente un cluster per soddisfare le esigenze dell'applicazione nel servizio contenitore di AZURE][aks-cluster-autoscaler].

## <a name="does-aks-support-kubernetes-rbac"></a>AKS supporta RBAC Kubernetes?

Sì, Kubernetes controllo di accesso basato sui ruoli (RBAC) è abilitato per impostazione predefinita quando vengono creati i cluster con la CLI di Azure. È possibile abilitare RBAC per i cluster che sono stati creati tramite il portale di Azure o i modelli.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>È possibile distribuire servizio Azure Kubernetes nella rete virtuale esistente?

Sì, è possibile distribuire un cluster del servizio contenitore di AZURE in una rete virtuale esistente usando il [avanzate funzionalità di rete][aks-advanced-networking].

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>È possibile rendere il server API Kubernetes accessibile solo all'interno di una rete virtuale?

Attualmente non è possibile. Il server API Kubernetes viene esposto come nome di dominio pubblico completo (FQDN). È possibile controllare l'accesso al cluster usando [RBAC di Kubernetes e Azure Active Directory (Azure AD)][aks-rbac-aad].

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Gli aggiornamenti della sicurezza vengono applicati ai nodi agente servizio Azure Kubernetes?

Azure applica automaticamente le patch di sicurezza per i nodi di Linux nel cluster in una pianificazione notturna. Tuttavia, si è responsabile di garantire che tali Linux nodi vengano riavviati come richiesto. Sono disponibili diverse opzioni per il riavvio di nodi:

- Manualmente tramite il portale di Azure o l'interfaccia della riga di comando di Azure.
- Aggiornando il cluster servizio Azure Kubernetes. Gli aggiornamenti del cluster [bloccano e svuotano i nodi] [ cordon-drain] automaticamente e quindi portare online un nodo nuovo con l'immagine Ubuntu più recente e una nuova versione di patch o una versione di Kubernetes. Per altre informazioni, vedere [Aggiornare un cluster del servizio Azure Kubernetes][aks-upgrade].
- Usando [Kured](https://github.com/weaveworks/kured), un daemon di riavvio open source per Kubernetes. Kured viene eseguito come un [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) e monitora ogni nodo per la presenza di un file che indica che è necessario un riavvio. All'interno del cluster, i riavvii del sistema operativo vengono gestiti dallo stesso [bloccano e svuotano processo] [ cordon-drain] come un aggiornamento del cluster.

Per altre informazioni sull'utilizzo di Kured, vedere [Apply security and kernel updates to nodes in servizio Azure Kubernetes][node-updates-kured] (Applicare aggiornamenti di sicurezza e del kernel ai nodi di ASK).

### <a name="windows-server-nodes"></a>Nodi di Windows Server

Per i nodi Windows Server (attualmente in anteprima nel servizio contenitore di AZURE), Windows Update automaticamente eseguire e applicare gli aggiornamenti più recenti. A intervalli regolari tutto il ciclo di rilascio di Windows Update e il proprio processo di convalida, è consigliabile eseguire un aggiornamento sul pool di nodi di Windows Server nel cluster AKS. Questo processo di aggiornamento crea i nodi che eseguono l'immagine più recente di Windows Server e le patch, quindi rimuove i nodi precedenti. Per altre informazioni su questo processo, vedere [esegue l'aggiornamento di un pool di nodi in AKS][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Perché vengono creati due gruppi di risorse con servizio Azure Kubernetes?

Ogni distribuzione servizio Azure Kubernetes si estende a due gruppi di risorse:

1. Creare il primo gruppo di risorse. Questo gruppo contiene solo la risorsa del servizio Kubernetes. Il provider di risorse AKS crea automaticamente il secondo gruppo di risorse durante la distribuzione. È un esempio del secondo gruppo di risorse *MC_myResourceGroup_myAKSCluster_eastus*. Per informazioni su come specificare il nome del secondo gruppo di risorse, vedere la sezione successiva.
1. Il secondo gruppo di risorse, ad esempio *MC_myResourceGroup_myAKSCluster_eastus*, contiene tutte le risorse di infrastruttura associate al cluster. come ad esempio le macchine virtuali dei nodi Kubernetes, le risorse della rete virtuale e di archiviazione. Lo scopo di questo gruppo di risorse è per semplificare la pulizia delle risorse.

Se si creano le risorse da usare con il cluster AKS, ad esempio gli account di archiviazione o indirizzi IP pubblici riservati, inserirle nel gruppo di risorse generato automaticamente.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>È possibile fornire un nome per il gruppo di risorse di infrastruttura servizio contenitore di AZURE?

Sì. Per impostazione predefinita, il provider di risorse AKS crea automaticamente un gruppo di risorse secondario (ad esempio *MC_myResourceGroup_myAKSCluster_eastus*) durante la distribuzione. Per garantire la conformità ai criteri aziendali, è possibile fornire il proprio nome per il cluster gestito (*MC _*) gruppo di risorse.

Per specificare il proprio nome di gruppo di risorse, installare il [aks-preview] [ aks-preview-cli] la versione dell'estensione di comando di Azure *0.3.2* o versione successiva. Quando si crea un cluster AKS usando il [az aks create] [ az-aks-create] comando, utilizzare il *-nodo-resource-group* parametro e specificare un nome per il gruppo di risorse. Se si [usare un modello di Azure Resource Manager] [ aks-rm-template] per distribuire un cluster AKS, è possibile definire il nome del gruppo di risorse utilizzando il *nodeResourceGroup* proprietà.

* Il gruppo di risorse secondario viene creato automaticamente dal provider di risorse di Azure nella propria sottoscrizione.
* È possibile specificare il nome di un gruppo di risorse personalizzati solo quando si crea il cluster.

Quando si lavora con i *MC _* gruppo di risorse, tenere presente che non è possibile:

* Specificare un gruppo di risorse per la *MC _* gruppo.
* Specificare una sottoscrizione diversa per il *MC _* gruppo di risorse.
* Modifica il *MC _* nome gruppo di risorse dopo aver creato il cluster.
* Specificare nomi per le risorse gestite all'interno di *MC _* gruppo di risorse.
* Modificare o eliminare i tag delle risorse gestite all'interno di *MC _* gruppo di risorse. (Vedere altre informazioni nella sezione successiva).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>È possibile modificare i tag e le altre proprietà delle risorse nel gruppo di risorse MC _ AKS?

Se si modificano o eliminano i tag creata in Azure e altre proprietà della risorsa nella *MC _* gruppo di risorse, è possibile ottenere risultati imprevisti, ad esempio la scalabilità e l'aggiornamento degli errori. Servizio contenitore di AZURE consente di creare e modificare i tag personalizzati. È possibile creare o modificare i tag personalizzati, ad esempio, per assegnare un centro business unit o costi. Modificando le risorse con il *MC _* nel cluster AKS, si interrompe l'obiettivo del livello di servizio (SLO). Per altre informazioni, vedere [AKS viene offrono un contratto di servizio?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Quali controller di ammissione Kubernetes supporta servizio Azure Kubernetes? È possibile aggiungere o rimuovere i controller di ammissione?

servizio Azure Kubernetes supporta i seguenti [controller di ammissione][admission-controllers]:

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*

Attualmente, è possibile modificare l'elenco dei controller di ammissione nel servizio contenitore di AZURE.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault è integrato in servizio Azure Kubernetes?

Servizio contenitore di AZURE attualmente in modo nativo non sono state integrate con Azure Key Vault. Tuttavia, il [FlexVolume insieme di credenziali delle chiavi di Azure per il progetto Kubernetes] [ keyvault-flexvolume] consente di indirizzare l'integrazione di POD Kubernetes per i segreti di Key Vault.

## <a name="can-i-run-windows-server-containers-on-aks"></a>È possibile eseguire contenitori Windows Server in servizio Azure Kubernetes?

Sì, i contenitori di Windows Server sono disponibili in anteprima. Per eseguire contenitori Windows Server nel servizio contenitore di AZURE, si crea un pool di nodi che esegue Windows Server come sistema operativo guest. Contenitori di Windows Server possono usare solo Windows Server 2019. Per iniziare, vedere [creare un cluster AKS con un pool di nodi di Windows Server][aks-windows-cli].

Supporto per pool di nodi Server finestra include alcune limitazioni che fanno parte di Windows Server a monte nel progetto di Kubernetes. Per altre informazioni su queste limitazioni, vedere [i contenitori di Windows Server in limitazioni AKS][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>Servizio contenitore di AZURE offre un contratto di servizio?

In un contratto di servizio (SLA), il provider accetta di rimborsare il cliente per il costo del servizio se non viene soddisfatto il livello di servizio pubblicato. Poiché il servizio contenitore di AZURE è gratuito, senza alcun costo non è disponibile per rimborsarsi, quindi servizio contenitore di AZURE non dispone di alcun contratto di servizio formale. Servizio contenitore di AZURE, tuttavia, si cerca di mantenere la disponibilità di almeno il 99,5% per il server API Kubernetes.

## <a name="why-cant-i-set-maxpods-below-30"></a>Il motivo per cui non è possibile impostare maxPods seguito 30?

Nel servizio contenitore di AZURE, è possibile impostare il `maxPods` valore quando si crea il cluster usando i modelli di comando di Azure e Azure Resource Manager. Tuttavia, sia Kubenet CNI Azure richiedono un *valore minimo* (convalidati al momento della creazione):

| Rete | Minima | Massima |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

Poiché il servizio contenitore di AZURE è un servizio gestito, si distribuire e gestire i POD e i componenti aggiuntivi come parte del cluster. In passato, gli utenti è stato possibile definire un `maxPods` valore inferiore al valore i POD gestiti necessari per eseguire (ad esempio, 30). Servizio contenitore di AZURE calcola ora il numero minimo di POD tramite questa formula: ((maxPods o (maxPods * vm_count)) > minimo di componenti aggiuntivi gestiti i POD.

Gli utenti non è possibile ignorare il requisito minimo `maxPods` convalida.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /rest/api/aks/managedclusters/createorupdate#managedcluster
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
