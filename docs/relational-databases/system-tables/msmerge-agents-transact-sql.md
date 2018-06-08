---
title: MSmerge_agents (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- MSmerge_agents
- MSmerge_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_agents system table
ms.assetid: 639d2ebb-2c37-4fe0-b14b-1637bc5fc221
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2199793fc6bc8d7822468d1fc0984347946277c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmergeagents-transact-sql"></a>MSmerge_agents (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_agents** table contient une ligne pour chaque Agent de fusion en cours d’exécution sur l’abonné. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'Agent de fusion|  
|**nom**|**nvarchar(100)**|Nom de l'Agent de fusion.|  
|**publisher_id**|**smallint**|L’ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**Publication**|**sysname**|Nom de la publication.|  
|**subscriber_id**|**smallint**|L’ID de l’abonné.|  
|**bd_abonné**|**sysname**|Le nom de la base de données d’abonnement.|  
|**local_job**|**bit**|Indique s'il existe un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de distribution local.|  
|**job_id**|**binary (16)**|Numéro d’identification du travail.|  
|**profile_id**|**int**|L’ID de configuration à partir de la **MSagent_profiles** table.|  
|**anonymous_subid**|**uniqueidentifier**|ID d'un agent anonyme.|  
|**subscriber_name**|**sysname**|Nom de l'Abonné.|  
|**date_création**|**datetime**|Date et heure de création de l'Agent de distribution ou de fusion.|  
|**offload_enabled**|**bit**|Indique si l'Agent peut être activé à distance.<br /><br /> **0** Spécifie l’agent ne peut pas être activé à distance.<br /><br /> **1** Spécifie l’agent sera activé à distance et sur l’ordinateur distant spécifié dans la propriété offload_server.|  
|**offload_server**|**sysname**|Indique le nom de réseau du serveur utilisé pour l'activation de l'Agent distant.|  
|**sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) de l'Agent de distribution ou de fusion lors de sa première exécution.|  
|**subscriber_security_mode**|**smallint**|Mode de sécurité utilisé par l'Agent lors de la connexion à l'Abonné. Les valeurs possibles sont les suivantes :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] l’authentification Windows.|  
|**subscriber_login**|**sysname**|Nom de connexion utilisé lors de la connexion à l'Abonné.|  
|**subscriber_password**|**nvarchar (524)**|Valeur chiffrée du mot de passe utilisé lors de la connexion à l'Abonné.|  
|**publisher_security_mode**|**smallint**|Mode de sécurité utilisé par l'agent lors de la connexion au serveur de publication et pouvant prendre la valeur suivante :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] l’authentification Windows.|  
|**publisher_login**|**sysname**|Nom de connexion utilisé lors de la connexion au serveur de publication.|  
|**publisher_password**|**nvarchar (524)**|Valeur chiffrée du mot de passe utilisée lors de la connexion au serveur de publication.|  
|**job_step_uid**|**uniqueidentifier**|ID unique de l'étape de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle l'Agent est démarré.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
