---
title: SQL Server, objet Buffer Manager | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Memory Manager
- Memory Manager object
ms.assetid: dbf49000-eeb0-4e9c-a361-5092363920dc
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 89b69f14e131453519bbaae77f16d629444eb169
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-memory-manager-object"></a>Objet SQLServer:Memory Manager
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'objet **Gestionnaire de mémoire** de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des compteurs permettant de surveiller l'utilisation globale de la mémoire du serveur. La surveillance globale de l'utilisation de la mémoire du serveur afin de mesurer l'activité de l'utilisateur et son exploitation des ressources peut être utile pour détecter les goulots d'étranglement des performances. La surveillance de la mémoire utilisée par une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous aide à déterminer :  
  
-   si des goulots d'étranglement existent en raison d'une quantité de mémoire physique disponible inadéquate pour le stockage de données fréquemment accédées dans le cache ; Si tel est le cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit extraire les données du disque.  
  
-   si les performances des requêtes peuvent être améliorées en ajoutant de la mémoire ou en mettant plus de mémoire à la disposition du cache des données ou des structures internes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="memory-manager-counters"></a>Compteurs du Gestionnaire de mémoire  
 Ce tableau décrit les compteurs du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Gestionnaire de mémoire** .  
  
|Compteurs du Gestionnaire de mémoire de SQL Server|Description|  
|----------------------------------------|-----------------|  
|**Mémoire de connexion (Ko)**|Spécifie la quantité totale de mémoire dynamique utilisée par le serveur pour maintenir les connexions.|  
|**Mémoire du cache de base de données (Ko)**|Spécifie la quantité de mémoire actuellement utilisée par le serveur pour le cache de la base de données.|  
|**Avantage externe de mémoire**|Quantité externe de mémoire, en millisecondes par page par milliseconde, multipliée par 10 milliards et tronquée en entier.| 
|**Mémoire disponible (Ko)**|Spécifie la quantité de mémoire allouée actuellement non utilisée par le serveur.|  
|**Mémoire réservée de l'espace de travail (Ko)**|Spécifie la quantité totale de mémoire actuellement réservée à l'exécution de processus tels que les opérations de hachage, de tri, de copie en bloc et de créations d'index.|  
|**Blocs de verrous**|Spécifie le nombre actuel de blocs de verrous utilisés sur le serveur (mis à jour régulièrement). Un bloc de verrous représente une ressource individuelle verrouillée, comme une table, une page ou une ligne.|  
|**Blocs de verrous alloués**|Spécifie le nombre actuel de blocs de verrous alloués. Au démarrage du serveur, le nombre de blocs de verrous alloués plus le nombre de blocs propriétaires de verrous alloués dépendent de l'option de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **de** . Si un plus grand nombre de blocs de verrous est nécessaire, cette valeur augmente.|  
|**Mémoire des verrous (Ko)**|Spécifie la quantité totale de mémoire dynamique qu'utilise le serveur pour les verrous.|  
|**Blocs propriétaires de verrous**|Spécifie le nombre actuel de blocs propriétaires de verrous utilisés sur le serveur (mis à jour régulièrement). Un bloc propriétaire de verrous représente l'appropriation d'un objet par un verrou via un thread individuel. Par conséquent, si trois threads disposent chacun d'un verrou partagé sur une page, il y aura trois blocs propriétaires de verrous.|  
|**Blocs propriétaires de verrous alloués**|Spécifie le nombre actuel de blocs propriétaires de verrous alloués. Au démarrage du serveur, le nombre de blocs propriétaires de verrous alloués plus le nombre de blocs de verrous alloués dépendent de l'option de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **de** . Si un plus grand nombre de blocs propriétaires de verrous est nécessaire, cette valeur augmente dynamiquement.|  
|**Mémoire du pool de journal (Ko)**|Quantité de mémoire dynamique utilisée par le serveur pour le pool du journal.| 
|**Mémoire maximale de l'espace de travail (Ko)**|Indique la quantité maximale de mémoire disponible pour exécuter des processus, tels que les opérations de hachage, de tri, de copie en bloc et de création d'index.|  
|**Demandes de mémoire satisfaites**|Spécifie le nombre total de processus qui ont acquis avec succès une allocation de mémoire de l'espace de travail.|  
|**Demandes de mémoire en attente**|Spécifie le nombre total de processus en attente d'une allocation de mémoire de l'espace de travail.|  
|**Mémoire de l'optimiseur (Ko)**|Spécifie la quantité totale de mémoire dynamique utilisée par le serveur pour l'optimisation des requêtes.|  
|**Mémoire réservée du serveur (Ko)**|Indique la quantité de mémoire réservée par le serveur pour un usage futur. Ce compteur affiche la quantité actuellement inutilisée de **Mémoire réservée de l’espace de travail (Ko)** octroyée initialement.|  
|**Mémoire du cache SQL (Ko)**|Spécifie la quantité totale de mémoire dynamique utilisée par le serveur pour le cache SQL dynamique.|  
|**Mémoire détournée du serveur (Ko)**|Spécifie la quantité de mémoire utilisée par le serveur à d'autres fins que les pages de bases de données.|  
|**Mémoire du serveur cible (Ko)**|Indique la quantité idéale de mémoire que le serveur peut consommer.|  
|**Mémoire totale du serveur (Ko)**|Spécifie la quantité de mémoire allouée par le serveur à l'aide du Gestionnaire de mémoire.|  
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server, objet Gestionnaire de tampons](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)   
[sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
