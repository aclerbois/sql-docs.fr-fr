---
title: Forcer un cluster WSFC à démarrer sans quorum | Microsoft Docs
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
ms.assetid: 4a121375-7424-4444-b876-baefa8fe9015
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: b7bf3d4cc778ae9c92a994d33223433ca3537422
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772095"
---
# <a name="force-a-wsfc-cluster-to-start-without-a-quorum"></a>Forcer un cluster WSFC à démarrer sans quorum
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment forcer un nœud de cluster de clustering de basculement Windows Server (WSFC) à démarrer sans quorum.  Cela peut être nécessaire dans les scénarios de récupération d'urgence et de sous-réseaux multiples pour récupérer des données et pour rétablir entièrement la haute disponibilité pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] et les instances de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Avant de commencer :**  [Recommandations](#Recommendations), [Sécurité](#Security)  
  
-   **Pour forcer un cluster à démarrer sans quorum :**  [Utilisation du Gestionnaire du cluster de basculement](#FailoverClusterManagerProcedure), [Utilisation de PowerShell](#PowerShellProcedure), [Utilisation de Net.exe](#CommandPromptProcedure)  
  
-   **Suivi :**  [Suivi : après avoir forcé le cluster à démarrer sans quorum](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Recommendations"></a> Recommandations  
 Sauf instructions spécifiques, les procédures de cette rubrique doivent fonctionner si vous les exécutez à partir de n'importe quel nœud du cluster WSFC.  Toutefois, vous pouvez obtenir de meilleurs résultats, et éviter des problèmes de connexion, en exécutant ces étapes à partir du nœud que vous envisagez de forcer à démarrer sans quorum.  
  
###  <a name="Security"></a> Sécurité  
 L'utilisateur doit être un compte de domaine qui est membre du groupe Administrateurs local sur chaque nœud du cluster WSFC.  
  
##  <a name="FailoverClusterManagerProcedure"></a> Utilisation du Gestionnaire du cluster de basculement  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Pour forcer un cluster à démarrer sans quorum  
  
1.  Ouvrez un Gestionnaire du cluster de basculement et connectez-vous au nœud de cluster souhaité pour forcer la mise en ligne.  
  
2.  Dans le volet **Actions** , cliquez sur **Forcer le démarrage du cluster**, puis cliquez sur **Oui – Forcer mon cluster à démarrer**.  
  
3.  Dans le volet gauche, dans l'arborescence du **Gestionnaire du cluster de basculement** , cliquez sur le nom de cluster.  
  
4.  Dans le volet résumé, vérifiez que la valeur active de **Configuration de quorum** est  **Avertissement : le cluster s'exécute dans l'état ForceQuorum**.  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Pour forcer un cluster à démarrer sans quorum  
  
1.  Démarrez Windows PowerShell avec élévation de privilèges via **Exécuter en tant qu'administrateur**.  
  
2.  Importez le module `FailoverClusters` pour activer les applets de commande de cluster.  
  
3.  Utilisez `Stop-ClusterNode` pour vous assurer que le service de cluster est arrêté.  
  
4.  Utilisez `Start-ClusterNode` avec `–FixQuorum` pour forcer le service de cluster à démarrer.  
  
5.  Utilisez `Get-ClusterNode` avec `–Propery NodeWieght = 1` pour définir la valeur qui garantit que le nœud est un membre du quorum disposant de droits de vote.  
  
6.  Générez la sortie des propriétés du nœud de cluster dans un format lisible.  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L’exemple suivant force le service de cluster du nœud Always OnSrv02 à démarrer sans quorum, définit `NodeWeight = 1`, puis énumère l’état du nœud de cluster à partir du nœud récemment forcé.  
  
```powershell  
Import-Module FailoverClusters  
  
$node = "Always OnSrv02"  
Stop-ClusterNode –Name $node  
Start-ClusterNode –Name $node -FixQuorum  
  
(Get-ClusterNode $node).NodeWeight = 1  
  
$nodes = Get-ClusterNode -Cluster $node  
$nodes | Format-Table -property NodeName, State, NodeWeight  
  
```  
  
##  <a name="CommandPromptProcedure"></a> Utilisation de Net.exe  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Pour forcer un cluster à démarrer sans quorum  
  
1.  Utilisez le Bureau à distance pour vous connecter au nœud de cluster souhaité pour forcer la mise en ligne.  
  
2.  Démarrez une invite de commandes avec élévation de privilèges via **Exécuter en tant qu'administrateur**.  
  
3.  Utilisez **net.exe** pour vous assurer que le service de cluster local est arrêté.  
  
4.  Utilisez **net.exe** avec `/forcequorum` pour forcer le service de cluster local à démarrer.  
  
### <a name="example-netexe"></a>Exemple (Net.exe)  
 L'exemple suivant force le service de cluster d'un nœud à démarrer sans quorum, définit `NodeWeight = 1`, puis énumère l'état du nœud de cluster à partir du nœud récemment forcé.  
  
```ms-dos  
net.exe stop clussvc  
net.exe start clussvc /forcequorum  
```  
  
##  <a name="FollowUp"></a> Suivi : après avoir forcé le cluster à démarrer sans quorum  
  
-   Vous devez réévaluer et reconfigurer les valeurs NodeWeight pour construire correctement un nouveau quorum avant de mettre en ligne d'autres nœuds. Sinon, le cluster peut de nouveau se trouver hors connexion.  
  
     Pour plus d’informations, consultez [Modes de quorum WSFC et configuration de vote &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
-   Les procédures de cette rubrique ne constituent qu'une étape de la mise en ligne du cluster WSFC si un échec non planifié du quorum se produit.  Vous pouvez aussi effectuer des étapes supplémentaires pour empêcher d'autres nœuds de cluster WSFC d'interférer avec la nouvelle configuration de quorum.  
  
-   D'autres fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , telles que [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], la mise en miroir de bases de données et la copie des journaux de transaction peuvent également nécessiter les actions suivantes pour récupérer les données et rétablir entièrement la haute disponibilité.  
  
     **Pour plus d'informations, consultez :**  
  
     [Effectuer un basculement manuel forcé d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
     [Forcer le service dans une session de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
  
     [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Afficher les événements et journaux pour un cluster de basculement](http://technet.microsoft.com/en-us/library/cc772342\(WS.10\).aspx)  
  
-   [Applets de commande de cluster de basculement Get-ClusterLog](http://technet.microsoft.com/library/ee461045.aspx)  
  
## <a name="see-also"></a> Voir aussi  
 [Récupération d’urgence WSFC par le quorum forcé &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Configurer les paramètres NodeWeight pour un quorum de cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)   
 [Applets de commande de cluster de basculement dans Windows PowerShell répertoriées par tâche](http://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
  
