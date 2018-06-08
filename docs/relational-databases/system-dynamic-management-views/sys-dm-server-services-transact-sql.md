---
title: Sys.dm_server_services (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/07/2018
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_server_services
- sys.dm_server_services
- sys.dm_server_services_TSQL
- dm_server_services_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_services dynamic management view
ms.assetid: 3f0defd0-478d-4e7f-96be-8795c9de4e3f
caps.latest.revision: 9
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 427dfc77aaf7178a9bd14a7243be4a44a9e1781c
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmserverservices-transact-sql"></a>sys.dm_server_services (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations relatives aux services SQL Server, Texte intégral et Agent SQL Server dans l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez cette vue de gestion dynamique pour signaler des informations d'état sur ces services.  
  
 
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|servicename|**nvarchar (256)**|Nom de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], recherche en texte intégral, ou un service de l’Agent SQL Server. Ne peut pas avoir la valeur null.|  
|startup_type|**int**|Indique le mode de démarrage du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> 0 : autres<br />1 : autres<br />2 : automatique<br />3 : manuel<br />4 : désactivé<br /><br /> Autorise la valeur NULL.|  
|startup_desc|**nvarchar (256)**|Décrit le mode de démarrage du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> Autre : Des autre (démarrage)<br />Autre : Des autre (démarrage système)<br />Automatique : Démarrage automatique<br />Manuel : Début de demande<br />Désactivé : désactivé<br /><br /> Ne peut pas avoir la valeur null.|  
|status|**int**|Indique l'état actuel du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> 1 : arrêté<br />2 : autre (démarrage en attente)<br />3 : autres (arrêt en attente)<br />4 : en cours d’exécution<br />5 : autre (continuation en attente)<br />6 : autre (suspension en attente)<br />7 : suspendu<br /><br /> Autorise la valeur NULL.|  
|status_desc|**nvarchar (256)**|Décrit l'état actuel du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> Arrêté : Le service est arrêté.<br />Autre (opération de démarrage en attente) : le service est en cours de démarrage.<br />Autre (opération d’arrêt en attente) : le service est en cours d’arrêt.<br />En cours d’exécution : Le service est en cours d’exécution.<br />Autres (continuer les opérations en attente) : le service est dans un état d’attente.<br />Autre (suspension en attente) : le service est en cours de suspension.<br />En pause : Le service est suspendu.<br /><br /> Ne peut pas avoir la valeur null.|  
|process_id|**int**|ID de processus du service. Ne peut pas avoir la valeur null.|  
|last_startup_time|**datetimeoffset(7)**|Date et heure du dernier démarrage du service. Autorise la valeur NULL.|  
|service_account|**nvarchar (256)**|Compte autorisé à contrôler le service. Ce compte peut démarrer ou arrêter le service ou modifier ses propriétés. Ne peut pas avoir la valeur null.|  
|filename|**nvarchar (256)**|Chemin d'accès et nom de fichier de l'exécutable du service. Ne peut pas avoir la valeur null.|  
|is_clustered|**nvarchar(1)**|Indique si le service est installé en tant que ressource d'un serveur en cluster. Ne peut pas avoir la valeur null.|  
|cluster_nodename|**nvarchar (256)**|Nom du nœud de cluster sur lequel le service est installé. Autorise la valeur NULL.|
|instant_file_initialization_enabled|**nvarchar(1)**|Spécifie si l’initialisation instantanée des fichiers est activée pour la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] service.<br /><br />Y = initialisation instantanée des fichiers est activée pour le service.<br /><br />N = initialisation instantanée des fichiers est désactivée pour le service.<br /><br /> Autorise la valeur NULL.<br /><br /> **Remarque :** ne s’applique pas à d’autres services tels que l’Agent SQL Server.<br /><br /> **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (en commençant par [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, et [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 via via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).|  

## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE` sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_server_registry &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-registry-transact-sql.md)  
  
