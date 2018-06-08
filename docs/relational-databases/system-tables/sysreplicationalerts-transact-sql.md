---
title: sysreplicationalerts (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
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
- sysreplicationalerts_TSQL
- sysreplicationalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysreplicationalerts system table
ms.assetid: 6ed15828-8cca-4cf0-b2ff-1ecd0d8db11a
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a6579b75ab8d8af66ed62bd9af021f65235423f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysreplicationalerts-transact-sql"></a>sysreplicationalerts (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient des informations sur les conditions déclenchant une alerte de réplication. Cette table est stockée dans le **msdb** base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|Identificateur de l'alerte.|  
|**status**|**int**|Valeur définie par l'utilisateur :<br /><br /> **0** = non traitée.<br /><br /> **1** = prises en charge.|  
|**agent_type**|**int**|Type d'agent :<br /><br /> **1** = Agent de capture instantanée.<br /><br /> **2** = Agent de lecture du journal.<br /><br /> **3** = Agent de distribution.<br /><br /> **4** = Agent de fusion.|  
|**agent_id**|**int**|L’ID de l’agent à partir des tables **MSsnapshot_agents**, **MSlogreader_agents**, **MSdistribution_agents**, ou **MSmerge_agents**.|  
|**ID_erreur**|**int**|L’ID de l’erreur stockée dans **MSrepl_errors**.|  
|**alert_error_code**|**int**|ID de message de l'alerte déclenchée lors de l'écriture de cet enregistrement dans le journal.|  
|**time**|**datetime**|Date et heure d'insertion de l'enregistrement.|  
|**publisher** (serveur de publication)|**sysname**|Nom du serveur de publication associé à l'Agent qui a déclenché l'alerte.|  
|**publisher_db**|**sysname**|Base de données du serveur de publication associée à l'Agent qui a déclenché l'alerte.|  
|**Publication**|**sysname**|Publication associée à l'Agent qui a déclenché l'alerte.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = instantané.<br /><br /> **1** = transactionnelle.<br /><br /> **2** = fusion.|  
|**subscriber** (Abonné)|**sysname**|Nom de l'Abonné associé à l'Agent qui a déclenché l'alerte.|  
|**bd_abonné**|**sysname**|Nom de la base de données de l'Abonné associée à l'Agent qui a déclenché l'alerte.|  
|**article**|**sysname**|Nom de l'article associé à l'Agent qui a déclenché l'alerte.|  
|**destination_object**|**sysname**|Nom de la table d'abonnement associée à l'alerte.|  
|**source_object**|**sysname**|Nom de la table publiée associée à l'alerte.|  
|**alert_error_text**|**ntext**|Texte de l'alerte.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
