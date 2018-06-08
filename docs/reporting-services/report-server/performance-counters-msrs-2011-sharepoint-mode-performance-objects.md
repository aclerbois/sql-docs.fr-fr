---
title: Compteurs de performances - MSRS 2011 en mode SharePoint, objets de performance | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- counters [Reporting Services]
- Report Server Windows service, performance counters
- RS Windows Service performance object
- Scheduling and Delivery Processor performance object [Reporting Services]
- performance [Reporting Services]
ms.assetid: 70bf6980-7845-4ab5-8b2a-ebf526d811a6
caps.latest.revision: 52
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 4b71aab05f6e9fe43b5077ef04cf3d7bd8dbc41d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="performance-counters-msrs-2011-sharepoint-mode-performance-objects"></a>Compteurs de performances - MSRS 2011 en mode SharePoint, objets de performance
  Cette rubrique décrit les compteurs de performance des objets de performance du **service Web MSRS 2011 en mode SharePoint** et du **service Windows MSRS 2011 en mode SharePoint** qui font partie d’un déploiement de [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] en mode SharePoint.  
  
> [!NOTE]  
>  Ces objets de performance contrôlent des événements sur le serveur de rapports local. Si vous exécutez un serveur de rapports dans un déploiement avec montée en puissance parallèle, les chiffres s'appliquent au serveur actuel et non au déploiement avec montée en puissance parallèle dans son ensemble.  
  
 Les objets de performance sont disponibles dans l’Analyseur de performances Windows (**Perfmon.exe**). Pour plus d'informations, consultez la documentation Windows. [Profilage de runtime](http://msdn.microsoft.com/library/w4bz2147.aspx) (http://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 Pour obtenir des informations sur les compteurs de performance et les serveurs de rapports en mode natif, consultez [Compteurs de performance du service web MSRS 2011 et des objets de performance du service Windows MSRS 2011 &#40;mode natif&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md) et [Compteurs de performance du service web MSRS 2011 en mode SharePoint et des objets de performance du service Windows MSRS 2011 en mode SharePoint (mode SharePoint)](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md).  
  
 Dans cette rubrique :  
  
-   [Compteurs de performance du service Web MSRS 2011 en mode SharePoint](#bkmk_webservice)  
  
-   [Compteurs de performance du service Windows MSRS 2011 en mode SharePoint](#bkmk_windowsservice)  
  
-   [Utiliser des applets de commande PowerShell pour retourner des listes](#bkmk_powershell)  
  
##  <a name="bkmk_webservice"></a> Compteurs de performance du service Web MSRS 2011 en mode SharePoint  
 L'objet de performance **MSRS 2011 Web Service SharePoint Mode** contrôle les performances du serveur de rapports. Cet objet de performance inclut une collection de compteurs utilisée pour suivre le traitement du serveur de rapports initialisé en général via des opérations de consultation du rapport interactives. Lorsque vous configurez ce compteur, vous pouvez l'appliquer à toutes les instances de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou sélectionner des instances spécifiques. Ces compteurs sont réinitialisés à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .  
  
 Le tableau suivant répertorie les compteurs inclus dans l'objet de performance **MSRS 2011 Web Service SharePoint Mode** .  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Sessions actives**|Nombre de sessions actives. Ce compteur fournit un nombre cumulatif de toutes les sessions de navigateur générées à partir d'exécutions de rapports, qu'elles soient toujours actives ou non.<br /><br /> Le compteur est décrémenté à mesure que les enregistrements de session sont supprimés. Par défaut, les sessions sont supprimées après dix minutes sans activité.|  
|**Présences dans le cache/s**|Nombre de requêtes par seconde pour les rapports mis en cache. Il s'agit des requêtes relatives aux rapports qui font l'objet d'un nouveau rendu, et non des requêtes concernant les rapports traités directement à partir du cache. (Consultez **Total des présences dans le cache** plus loin dans cette rubrique.)|  
|**Présences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde pour le modèle mis en cache. Il s'agit des requêtes relatives aux rapports qui font l'objet d'un nouveau rendu, et non des requêtes concernant les rapports traités directement à partir du cache.|  
|**Absences dans le cache/s**|Nombre de requêtes par seconde qui n'ont pas retourné un rapport à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Absences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde qui n'ont pas retourné de modèle à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Premières demandes de sessions/s**|Nombre de nouvelles sessions utilisateur démarrées chaque seconde à partir du cache du serveur de rapports.|  
|**Présences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports sont récupérés du cache mémoire. Le*cache interne* fait partie de la mémoire cache qui stocke les rapports dans la mémoire de l’unité centrale. Quand le cache mémoire est utilisé, le serveur de rapports n’interroge pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le contenu mis en cache.|  
|**Absences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports n'ont pas pu être récupérés du cache mémoire.|  
|**Demandes de sessions suivantes/s**|Nombre de requêtes par seconde pour les rapports qui sont ouverts dans une session existante (tels que les rapports rendus à partir d'un instantané de session).|  
|**Demandes de rapports**|Nombre de rapports qui sont actuellement actifs et gérés par le serveur de rapports.|  
|**Rapports exécutés/s**|Nombre d'exécutions de rapport réussies par seconde. Ce compteur fournit des statistiques à propos du volume de rapports. Utilisez ce compteur avec **Demandes/s** pour comparer l’exécution de rapports et les demandes de rapports pouvant être retournées par le cache.|  
|**Demandes/s**|Nombre de demandes par seconde envoyées au serveur de rapports. Ce compteur suit tous les types de demandes gérées par le serveur de rapports.|  
|**Total des présences dans le cache**|Nombre total de demandes de rapports à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Total des présences dans le cache (modèles sémantiques)**|Nombre total de demandes de modèle à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par ASP.NET.|  
|**Total des absences dans le cache**|Nombre total d'échecs de retour d'un rapport à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Utilisez ce compteur pour déterminer si l'espace disque et la mémoire sont suffisants.|  
|**Total des absences dans le cache (modèles sémantiques)**|Nombre total d'échecs de retour d'un modèle à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par ASP.NET. Utilisez ce compteur pour déterminer si l'espace disque et la mémoire sont suffisants.|  
|**Total des présences dans le cache mémoire**|Nombre total de rapports mis en cache retournés par le cache mémoire après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Le*cache en mémoire* fait partie du cache qui stocke les rapports dans la mémoire de l’unité centrale. Quand le cache mémoire est utilisé, le serveur de rapports n’interroge pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le contenu mis en cache.|  
|**Total des absences dans le cache mémoire**|Nombre total d'absences dans le cache mémoire après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Total des échecs de traitement**|Nombre d'erreurs dans le traitement des demandes du service Web du serveur de rapports.|  
|**Total des threads rejetés**|Nombre total de threads rejetés pour le traitement asynchrone, puis gérés comme processus synchrones dans le même thread. Chaque source de données est traitée sur un thread. Si le volume de threads dépasse la capacité, les threads sont rejetés pour le traitement asynchrone et sont traités ensuite en série.|  
|**Total des rapports exécutés**|Nombre total de rapports qui ont été correctement exécutés après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Nombre total de demandes**|Nombre total des demandes envoyées au serveur de rapports après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
  
##  <a name="bkmk_windowsservice"></a> Compteurs de performance du service Windows MSRS 2011 en mode SharePoint  
 L'objet de performance **MSRS 2011 Windows Service SharePoint Mode** est utilisé pour contrôler le service Windows Report Server. Cet objet de performance inclut une collection de compteurs utilisée pour suivre le traitement des rapports initialisé via des opérations planifiées. Les opérations planifiées peuvent englober l'abonnement et la remise, les instantanés d'exécution de rapport et l'historique de rapport. Lorsque vous configurez ce compteur, vous pouvez l'appliquer à toutes les instances de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou sélectionner des instances spécifiques.  
  
 Le tableau suivant répertorie les compteurs inclus dans l'objet de performance **MSRS 2011 Windows Service SharePoint mode** .  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Sessions actives**|Nombre de sessions actives stockées dans la base de données du serveur de rapports. Ce compteur fournit un nombre cumulatif de toutes les sessions de navigateur utilisables générées à partir d'abonnements à un rapport, qu'elles soient encore actives ou non.|  
|**Alerte : longueur de la file d'attente des événements**||  
|**Alerte : événements traités - CreateSchedule**||  
|**Alerte : événements traités - DeleteSchedule**||  
|**Alerte : événements traités - DeliverAlert**||  
|**Alerte : événements traités - FireAlert**||  
|**Alerte : événements traités - FireSchedule**||  
|**Alerte : événements traités - GenerateAlert**||  
|**Alerte : événements traités - UpdateSchedule**||  
|**Vidages du cache/s**|Nombre de vidages du cache par seconde.|  
|**Présences dans le cache/s**|Nombre de requêtes par seconde pour les rapports mis en cache. Il s'agit des requêtes relatives aux rapports qui font l'objet d'un nouveau rendu, et non des requêtes concernant les rapports traités directement à partir du cache. (Consultez **Total des présences dans le cache** plus loin dans cette rubrique.)|  
|**Présences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde pour les modèles mis en cache.|  
|**Absences dans le cache/s**|Nombre de requêtes par seconde qui n'ont pas retourné un rapport à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Absences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde qui n'ont pas retourné de modèle à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Remises/s**|Nombre de remises de rapport par seconde, de toute extension de remise.|  
|**Événements/s**|Nombre d'événements traités par seconde. Les événements contrôlés incluent **SnapshotUpdated** et **TimedSubscription**.|  
|**Premières demandes de sessions/s**|Nombre de nouvelles sessions d'exécution de rapport crées par seconde.|  
|**Présences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports sont récupérés du cache mémoire. Le*cache interne* fait partie de la mémoire cache qui stocke les rapports dans la mémoire de l’unité centrale. Quand le cache mémoire est utilisé, le serveur de rapports n’interroge pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le contenu mis en cache.|  
|**Absences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports ne peuvent pas être récupérés du cache mémoire.|  
|**Demandes de sessions suivantes/s**|Nombre de requêtes par seconde pour les rapports qui sont ouverts dans une session existante (tels que les rapports rendus à partir d'un instantané de session).|  
|**Demandes de rapports**|Nombre de rapports qui sont actuellement actifs et gérés par le serveur de rapports. Utilisez ce compteur pour évaluer la stratégie de mise en cache. Il peut exister beaucoup plus de requêtes que de rapports générés.|  
|**Rapports exécutés/s**|Nombre de rapports correctement générés par seconde.|  
|**Demandes/s**|Nombre total des demandes réussies que le service du serveur de rapports a traitées par seconde.|  
|**Mises à jour d’instantanés/s**|Nombre total de mises à jour d'instantanés d'exécution de rapport par seconde.|  
|**Total des recyclages de domaine d’application**|Nombre total de cycles de domaine d'application après le démarrage du service Windows Report Server.|  
|**Total des vidages du cache**|Nombre total de mises à jour du cache du serveur de rapports après le démarrage du service. Ce compteur est remis à zéro lorsque le domaine d'application se recycle. Consultez **Vidages du cache/s**.|  
|**Total des présences dans le cache**|Nombre total de requêtes pour les rapports traités directement à partir du cache après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle. Consultez **Présences dans le cache/s**.|  
|**Total des présences dans le cache (modèles sémantiques)**|Nombre total de demandes de modèle traitées directement à partir du cache après le démarrage du service Windows du serveur de rapports. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des absences dans le cache**|Nombre total d'échecs de retour d'un rapport à partir du cache après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle. Consultez **Absences dans le cache/s**.|  
|**Total des absences dans le cache (modèles sémantiques)**|Nombre total d'échecs de retour d'un modèle à partir du cache après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des remises**|Nombre total de rapports remis par le processeur de planification et de livraison, pour toutes les extensions de remise. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Nombre total d'événements**|Nombre total d'événements après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des présences dans le cache mémoire**|Nombre total de rapports mis en cache retournés par le cache mémoire après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des absences dans le cache mémoire**|Nombre total d'absences dans le cache mémoire après le démarrage du service. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des échecs de traitement**|Nombre d'erreurs de traitement des demandes pour le service Windows du serveur de rapports.|  
|**Total des threads rejetés**|Nombre total de threads rejetés pour le traitement asynchrone, puis gérés comme processus synchrone dans le même thread. En cas de charge modérée ou élevée, ce compteur augmente régulièrement.|  
|**Total des rapports exécutés**|Nombre total de rapports exécutés.|  
|**Nombre total de demandes**|Nombre total de rapports qui ont été correctement exécutés après le démarrage du service. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des mises à jour d’instantanés**|Nombre total de mises à jour d'instantanés d'exécution de rapport.|  
  
##  <a name="bkmk_powershell"></a> Utiliser des applets de commande PowerShell pour retourner des listes  
 ![Contenu relatif à PowerShell](../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell")Le script Windows PowerShell suivant retourne les ensembles de compteurs dans lesquels CounterSetName commence par « msr » :  
  
```  
get-counter -listset msr*  
Returns a list with the following information  
CounterSetName     : MSRS 2011 Windows Service SharePoint Mode  
CounterSetName     : MSRS 2011 Web Service SharePoint Mode  
```  
  
 Le script Windows PowerShell suivant retourne la liste des compteurs de performance pour le compteur de performance CounterSetName « Service Windows MSRS 2011 en mode SharePoint ».  
  
```  
(get-counter -listset "MSRS 2011 Windows Service SharePoint Mode").paths  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Analyse des performances d'un serveur de rapports](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Compteurs de performances du service web MSRS 2011 et des objets de performance du service Windows MSRS 2011 &#40;mode natif&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [Compteurs de performances pour des objets de performances ReportServer:Service  et ReportServerSharePoint:Service](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
  
  
