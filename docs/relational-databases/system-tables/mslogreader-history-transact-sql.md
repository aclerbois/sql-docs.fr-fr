---
title: MSlogreader_history (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSlogreader_history_TSQL
- MSlogreader_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSlogreader_history system table
ms.assetid: 2e399fa1-3591-4c1c-96b7-7964fe82c7c4
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 8041a5c3dfdb216d939ef5b99e22b36c0781eda9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mslogreaderhistory-transact-sql"></a>MSlogreader_history (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSlogreader_history** table contient des lignes d’historique des Agents de lecture du journal associé au serveur de distribution local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|ID de l'Agent de lecture du journal.|  
|**runstatus**|**int**|État d'exécution :<br /><br /> 1 = Démarrage.<br /><br /> 2 = Succès.<br /><br /> 3 = En cours.<br /><br /> 4 = Inactif.<br /><br /> 5 = Nouvel essai.<br /><br /> 6 = Échec.|  
|**start_time**|**datetime**|Heure de démarrage de l'exécution de la tâche|  
|**time**|**datetime**|Heure de consignation du message dans le journal|  
|**duration**|**int**|Durée, en secondes, de la session de message.|  
|**Commentaires**|**nvarchar(255)**|Texte du message.|  
|**xact_seqno**|**varbinary(16)**|Numéro de séquence de la dernière transaction réalisée.|  
|**delivery_time**|**int**|La première transaction de temps est remise.|  
|**delivered_transactions**|**int**|Nombre total des transactions transmises dans la session.|  
|**delivered_commands**|**int**|Nombre total des commandes transmises dans la session.|  
|**average_commands**|**int**|Nombre moyen des commandes transmises dans la session.|  
|**delivery_rate**|**float**|Moyenne des commandes transmises par seconde.|  
|**delivery_latency**|**int**|Temps de latence entre l'entrée de la commande dans la base de données publiée et son entrée dans la base de données de distribution. En millisecondes.|  
|**ID_erreur**|**int**|L’ID de l’erreur dans le **MSrepl_error** (table système).|  
|**timestamp**|**timestamp**|Colonne timestamp de cette table|  
|**updateable_row**|**bit**|La valeur **1** si la ligne d’historique peut être remplacée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
