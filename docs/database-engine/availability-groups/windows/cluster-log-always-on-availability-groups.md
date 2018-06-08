---
title: CLUSTER.LOG (groupes de disponibilité Always On) (SQL Server) | Microsoft Docs
ms.custom: ag-guide
ms.date: 06/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: ''
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 01a9e3c1-2a5f-4b98-a424-0ffc15d312cf
caps.latest.revision: 7
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 6e2cc0bda94d980535c014c1f52cf83f9ee80910
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clusterlog-always-on-availability-groups"></a>CLUSTER.LOG (groupes de disponibilité Always On)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  En tant que ressource de cluster de basculement, certaines interactions externes entre SQL Server, le cluster du service WSFC (Windows Server Failover Cluster) et la DLL de ressource SQL Server (hadrres.dll) ne peuvent pas faire l’objet d’un monitoring dans SQL Server. Le journal WSFC, CLUSTER.LOG, peut diagnostiquer des problèmes dans le cluster WSFC ou la DLL de ressource SQL Server.  
  
 Le diagramme suivant illustre la relation entre les applications comme SQL Server et le gestionnaire de clusters Windows qui déclenchent la création de ressources de groupe de disponibilité, leur destruction ou des changements d’état.  
  
## <a name="generate-cluster-log"></a>Générer le journal de cluster  
 Vous pouvez générer les journaux de cluster de deux manières :  
  
1.  À l’aide de la commande `cluster /log /g` à partir de l’invite de commandes. Cette commande génère les journaux de cluster dans le répertoire \windows\cluster\reports sur chaque nœud WSFC. L’avantage de cette méthode est que vous pouvez spécifier le niveau de détail dans les journaux générés à l’aide de l’option `/level`. L’inconvénient, c’est que vous ne pouvez pas spécifier le répertoire de destination pour les journaux de cluster générés. Pour plus d’informations, consultez [How to create the cluster.log in Windows Server 2008 Failover Clustering](http://blogs.msdn.com/b/clustering/archive/2008/09/24/8962934.aspx).  
  
2.  Utilisez l’applet de commande PowerShell [Get-ClusterLog](http://technet.microsoft.com/library/ee461045.aspx). L’avantage de cette méthode est que vous pouvez générer le journal de cluster de tous les nœuds dans un seul répertoire de destination (sur le nœud où vous exécutez l’applet de commande). L’inconvénient, c’est que vous ne pouvez pas spécifier le niveau de détail dans les journaux générés.  
  
 Les commandes PowerShell suivantes génèrent les journaux de cluster de tous les nœuds de cluster au cours des 15 dernières minutes et les placent dans le répertoire actif. Exécutez les commandes dans une fenêtre PowerShell avec des privilèges administratifs.  
  
```powershell  
Import-Modeul FailoverClusters   
Get-ClusterLog –TimeSpan 15 –Destination .  
```  
  
## <a name="always-on-log-verbosity"></a>Niveau de détail des journaux Always On  
 Vous pouvez augmenter le niveau de détail des journaux dans CLUSTER.LOG pour un groupe de disponibilité. Pour modifier le niveau de détail, effectuez les étapes suivantes :  
  
1.  Dans le menu **Démarrer**, ouvrez le **Gestionnaire du cluster de basculement**.  
  
2.  Développez votre cluster et le nœud **Services et applications**, puis cliquez sur le nom du groupe de disponibilité.  
  
3.  Dans le volet de détails, cliquez avec le bouton droit sur la ressource du groupe de disponibilité, puis cliquez sur **Propriétés**.  
  
4.  Cliquez sur l’onglet **Propriétés** .  
  
5.  Modifiez la propriété **VerboseLogging**. Par défaut, **VerboseLogging** a la valeur `0` qui signale les informations, les avertissements et les erreurs. La valeur **VerboseLogging** peut aller de `0` à `2`.  
  
6.  Cliquez sur **OK**.  
  
7.  Recliquez avec le bouton droit sur la ressource de groupe de disponibilité, puis cliquez sur **Mettre cette ressource hors connexion**.  
  
8.  Recliquez avec le bouton droit sur la ressource de groupe de disponibilité, puis cliquez sur **Mettre cette ressource en ligne**.  
  
## <a name="availability-group-resource-events"></a>Événements de la ressource de groupe de disponibilité  
 Le tableau ci-dessous montre les différents genres d’événements que vous pouvez voir dans CLUSTER.LOG et qui appartiennent à la ressource de groupe de disponibilité. Pour plus d’informations sur le RHS (sous-système d’hébergement de ressources) et le RCM (moniteur de contrôle de ressource) dans WSFC, consultez [Resource Hosting Subsystem (RHS) In Windows Server 2008 Failover Clusters](http://blogs.technet.com/b/askcore/archive/2009/11/23/resource-hosting-subsystem-rhs-in-windows-server-2008-failover-clusters.aspx).  
  
|Identificateur|Source|Exemple de CLUSTER.LOG|  
|----------------|------------|------------------------------|  
|Messages dotés des préfixes `[RES]` et `[hadrag]`|hadrres.dll (DLL de ressource Always On)|00002cc4.00001264::2011/08/05-13:47:42.543 INFO  [RES] SQL Server Availability Group \<ag>: `[hadrag]` Offline request.<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.558 ERR   [RES] SQL Server Availability Group \<ag>: `[hadrag]` Lease Thread terminated<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.605 INFO  [RES] SQL Server Availability Group \<ag>: `[hadrag]` Free SQL statement<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.902 INFO  [RES] SQL Server Availability Group \<ag>: `[hadrag]` Disconnect from SQL Server|  
|Messages dotés du préfixe `[RHS]`|RHS.EXE (Sous-système d’hébergement de ressources, processus hôte de hadrres.dll)|00000c40.00000a34::2011/08/10-18:42:29.498 INFO  [RHS] Resource ag has come offline. RHS is about to report resource status to RCM.|  
|Messages dotés du préfixe `[RCM]`|Moniteur de contrôle de ressource (service de cluster)|000011d0.00000f80::2011/08/05-13:47:42.480 INFO  [RCM] rcm::RcmGroup::Move: Bringing group ’ag’ offline first...<br /><br /> 000011d0.00000f80::2011/08/05-13:47:42.496 INFO  [RCM] TransitionToState(ag) Online-->OfflineCallIssued.|  
|RcmApi/ClusAPI|Appel d’API, ce qui signifie plus ou moins que SQL Server demande l’action|000011d0.00000f80::2011/08/05-13:47:42.465 INFO  [RCM] rcm::RcmApi::MoveGroup: (ag, 2)|  
  
## <a name="debug-always-on-resource-dll-in-isolation"></a>Déboguer la DLL de ressource Always On en mode isolation  
 En matière de débogage, il est recommandé de configurer votre cluster de manière à ce qu’il exécute la DLL de ressource Always On (hadrres.dll) en mode isolation par rapport à d’autres DLL de ressource. Par défaut, le cluster WSFC exécute toutes les DLL de ressource dans une seule instance de rhs.exe. Ainsi, toutes les ressources au sein du cluster partagent la même instance de rhs.exe. Quand vous essayez de déboguer hadrres.dll avec un débogueur, le fait de faire une pause au niveau d’un point d’arrêt peut entraîner la mise en pause d’autres ressources qui partagent l’instance de rhs.exe. Par ailleurs, en cas d’exécution de plusieurs groupes de disponibilité dans le même cluster, la même configuration peut entraîner la mise en pause de tous les groupes de disponibilité quand vous faites une pause au niveau d’un point d’arrêt pour déboguer un groupe de disponibilité.  
  
 Pour isoler un groupe de disponibilité des autres DLL de ressource de cluster, notamment d’autres groupes de disponibilité, effectuez les étapes suivantes afin d’exécuter hadrres.dll dans un processus rhs.exe distinct :  
  
1.  Ouvrez **l’Éditeur du Registre** et accédez à la clé suivante : HKEY_LOCAL_MACHINE\Cluster\Resources. Cette clé contient les clés de toutes les ressources, chacune d’elles ayant un GUID différent.  
  
2.  Recherchez la clé de ressource dont la valeur **Nom** correspond au nom de votre groupe de disponibilité.  
  
3.  Affectez à **SeparateMonitor** la valeur **1**.  
  
4.  Redémarrez le service en cluster pour votre groupe de disponibilité dans le cluster WSFC.  
  
  