---
title: Configurer les paramètres NodeWeight pour un quorum de cluster | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: cb3fd9a6-39a2-4e9c-9157-619bf3db9951
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 313180f9fb4565d38e1b38e3a2c78331d03f5f58
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34773165"
---
# <a name="configure-cluster-quorum-nodeweight-settings"></a>Configurer les paramètres NodeWeight pour un quorum de cluster
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment configurer les paramètres NodeWeight pour un nœud membre dans un cluster de clustering de basculement Windows Server (WSFC). Les paramètres NodeWeight sont utilisés pendant le vote du quorum pour prendre en charge les scénarios de récupération d'urgence et de sous-réseaux multiples pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] et les instances de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Avant de commencer :**  [Conditions préalables](#Prerequisites), [Sécurité](#Security)  
  
-   **Pour afficher les paramètres NodeWeight du quorum avec :** [Utilisation de PowerShell](#PowerShellProcedure), [Utilisation de Cluster.exe](#CommandPromptProcedure)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Cette fonctionnalité est prise en charge uniquement dans [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] ou versions ultérieures.  
  
> [!IMPORTANT]  
>  Pour utiliser les paramètres NodeWeight, le correctif logiciel suivant doit être appliqué à tous les serveurs dans le cluster WSFC :  
>   
>  [KB2494036](http://support.microsoft.com/kb/2494036): un correctif est disponible pour vous permettre de configurer un nœud de cluster qui n'a pas de votes de quorum dans [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et dans [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]  
  
> [!TIP]  
>  Si ce correctif logiciel n'est pas installé, les exemples de cette rubrique retournent des valeurs vides ou NULL pour NodeWeight.  
  
###  <a name="Security"></a> Sécurité  
 L'utilisateur doit être un compte de domaine qui est membre du groupe Administrateurs local sur chaque nœud du cluster WSFC.  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
  
##### <a name="to-configure-nodeweight-settings"></a>Pour configurer les paramètres NodeWeight  
  
1.  Démarrez Windows PowerShell avec élévation de privilèges via **Exécuter en tant qu'administrateur**.  
  
2.  Importez le module `FailoverClusters` pour activer les applets de commande de cluster.  
  
3.  Utilisez l'objet `Get-ClusterNode` pour définir la propriété `NodeWeight` pour chaque nœud du cluster.  
  
4.  Générez la sortie des propriétés du nœud de cluster dans un format lisible.  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L'exemple suivant modifie le paramètre NodeWeight pour supprimer le vote du quorum pour le nœud « AlwaysOnSrv1 », puis génère les paramètres pour tous les nœuds du cluster.  
  
```powershell  
Import-Module FailoverClusters  
  
$node = “AlwaysOnSrv1”  
(Get-ClusterNode $node).NodeWeight = 0  
  
$cluster = (Get-ClusterNode $node).Cluster  
$nodes = Get-ClusterNode -Cluster $cluster  
  
$nodes | Format-Table -property NodeName, State, NodeWeight  
```  
  
##  <a name="CommandPromptProcedure"></a> Utilisation de Cluster.exe  
  
> [!NOTE]  
>  L'utilitaire cluster.exe est déconseillé dans [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] .  Utilisez PowerShell avec le clustering de basculement pour le développement futur.  L'utilitaire cluster.exe sera supprimé dans la prochaine version de Windows Server. Pour plus d'informations, consultez [Mappage des commandes Cluster.exe aux applets de commande Windows PowerShell pour les clusters de basculement](http://technet.microsoft.com/library/ee619744\(WS.10\).aspx).  
  
##### <a name="to-configure-nodeweight-settings"></a>Pour configurer les paramètres NodeWeight  
  
1.  Démarrez une invite de commandes avec élévation de privilèges via **Exécuter en tant qu'administrateur**.  
  
2.  Utilisez **cluster.exe** pour définir les valeurs `NodeWeight` .  
  
### <a name="example-clusterexe"></a>Exemple (Cluster.exe)  
 L'exemple suivant modifie la valeur NodeWeight pour supprimer le vote du quorum pour le nœud « AlwaysOnSrv1 » dans le cluster « Cluster001 ».  
  
```ms-dos  
cluster.exe Cluster001 node AlwaysOnSrv1 /prop NodeWeight=0  
```  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Afficher les événements et journaux pour un cluster de basculement](http://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Applets de commande de cluster de basculement Get-ClusterLog](http://technet.microsoft.com/library/ee461045.aspx)  
  
## <a name="see-also"></a> Voir aussi  
 [Modes de quorum WSFC et configuration de vote &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)   
 [Afficher les paramètres NodeWeight pour le quorum de cluster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)   
 [Applets de commande de cluster de basculement dans Windows PowerShell répertoriées par tâche](http://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
  
