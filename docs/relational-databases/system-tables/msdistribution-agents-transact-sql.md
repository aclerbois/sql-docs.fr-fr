---
title: MSdistribution_agents (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/28/2015
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
- MSdistribution_agents_TSQL
- MSdistribution_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_agents system table
ms.assetid: 0e8f0653-1351-41d1-95d2-40f6d5a050ca
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 63d202dceb9f5b8c96edf47e2105d21b478969d6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msdistributionagents-transact-sql"></a>MSdistribution_agents (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSdistribution_agents** table contient une ligne pour chaque Agent de Distribution en cours d’exécution sur le serveur de distribution local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'Agent de distribution.|  
|**nom**|**nvarchar(100)**|Nom de l'Agent de distribution.|  
|**publisher_database_id**|**int**|Identificateur de la base de données du serveur de publication.|  
|**publisher_id**|**smallint**|L’ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**Publication**|**sysname**|Nom de la publication.|  
|**subscriber_id**|**smallint**|ID de l'Abonné, utilisé uniquement par les agents reconnus. Pour les agents anonymes, cette colonne est réservée.|  
|**bd_abonné**|**sysname**|Nom de la base de données d'abonnement|  
|**subscription_type**|**int**|Le type d’abonnement :<br /><br /> **0** = par envoi de données.<br /><br /> **1** = par extraction de données.<br /><br /> **2** = anonyme.|  
|**local_job**|**bit**|Indique s’il existe un travail de l’Agent SQL Server sur le serveur de distribution local.|  
|**job_id**|**binary (16)**|Numéro d’identification du travail.|  
|**subscription_guid**|**binary (16)**|ID des abonnements de cet agent|  
|**profile_id**|**int**|L’ID de configuration à partir de la [MSagent_profiles &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) table.|  
|**anonymous_subid**|**uniqueidentifier**|ID d'un agent anonyme.|  
|**subscriber_name**|**sysname**|Nom de l'Abonné, utilisé par des agents anonymes uniquement|  
|**virtual_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**anonymous_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**date_création**|**datetime**|Date et heure de création de l'Agent de distribution ou de l'Agent de fusion|  
|**queue_id**|**sysname**|Identificateur permettant de localiser la file d'attente pour les abonnements de mise à jour en attente. Pour les abonnements qui ne sont pas en attente, la valeur est NULL. Pour les publications [!INCLUDE[msCoName](../../includes/msconame-md.md)] basées sur Message Queuing, la valeur est un GUID qui identifie de manière unique la file d'attente à utiliser pour l'abonnement. Pour les publications de la file d’attente SQL Server, la colonne contient la valeur **SQL**.<br /><br /> Remarque : L’utilisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing a été déconseillée et n’est plus pris en charge.|  
|**queue_status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offload_enabled**|**bit**|Indique si l’agent peut être activé à distance.<br /><br /> **0** indique que l’agent ne peut pas être activé à distance.<br /><br /> **1** indique que l’agent sera être activé à distance et sur l’ordinateur distant spécifié dans le *offload_server* propriété.|  
|**offload_server**|**sysname**|Nom de réseau du serveur à utiliser pour l'activation de l'agent à distance|  
|**argument dts_package_name**|**sysname**|Nom du package DTS. Par exemple, pour un package nommé **DTSPub_Package**, spécifiez `@dts_package_name = N'DTSPub_Package'`.|  
|**dts_package_password**|**nvarchar (524)**|Mot de passe du package.|  
|**dts_package_location**|**int**|L’emplacement du package. L’emplacement du package peut être **distributeur** ou **abonné**.|  
|**sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) de l'Agent de distribution ou de fusion lors de sa première exécution.|  
|**queue_server**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**subscriber_security_mode**|**smallint**|Mode de sécurité utilisé par l'Agent lors de la connexion à l'Abonné. Les valeurs possibles sont les suivantes :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] l’authentification SQL Server<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] l’authentification Windows.|  
|**subscriber_login**|**sysname**|Nom de connexion utilisé lors de la connexion à l'Abonné.|  
|**subscriber_password**|**nvarchar (524)**|Indique la valeur chiffrée du mot de passe qui est utilisé lors de la connexion à l'Abonné.|  
|**reset_partial_snapshot_progress**|**bit**|Indique si un instantané partiellement téléchargée sera annulée pour que la totalité du processus d'instantané puisse recommencer.|  
|**job_step_uid**|**uniqueidentifier**|L’ID unique du travail de l’Agent SQL Server de l’étape dans laquelle l’agent est démarré.|  
|**flux d’abonnements**|**tinyint**|Définit le nombre de connexions autorisées par l'Agent de distribution pour appliquer des traitements de modifications en parallèle à un Abonné. Une plage de valeurs allant de 1 à 64 est prise en charge.|  
|**optimisées en mémoire**|**bit**|1 indique que l’abonné peut être utilisé pour les tables optimisées en mémoire.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
