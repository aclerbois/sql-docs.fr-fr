---
title: IHextendedSubscriptionView (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- IHextendedSubscriptionView_TSQL
- IHextendedSubscriptionView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedSubscriptionView view
ms.assetid: 124756a4-463a-4a81-bf5b-de7e8ffc7a62
caps.latest.revision: 11
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 49ef47fe35d430dcbe2499b9ef5e8d946db3e56b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ihextendedsubscriptionview-transact-sql"></a>IHextendedSubscriptionView (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **IHextendedSubscriptionView** vue expose des informations sur l’abonnement à une publication non SQL Server. Cette vue est stockée dans le **distribution** base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|Identificateur unique d'un article|  
|**dest_db**|**sysname**|Nom de la base de données de destination|  
|**srvid**|**smallint**|Identificateur unique pour un Abonné|  
|**login_name**|**sysname**|Nom de connexion utilisé pour se connecter à un Abonné.|  
|**distribution_jobid**|**binaire**|Identifie le travail de l'Agent de distribution.|  
|**publisher_database_id**|**int**|Identifie la base de données de publication.|  
|**subscription_type**|**int**|Le type d’abonnement :<br /><br /> **0** = par envoi de données - l’agent de distribution s’exécute sur l’abonné.<br /><br /> **1** = par extraction de données - l’agent de distribution s’exécute sur le serveur de distribution.|  
|**sync_type**|**tinyint**|Type de synchronisation initiale :<br /><br /> **1** = automatique<br /><br /> **2** = none|  
|**status**|**tinyint**|L’état de l’abonnement :<br /><br /> **0** = inactif<br /><br /> **1** = abonné<br /><br /> **2** = actif|  
|**snapshot_seqno_flag**|**bit**|Indique si un numéro de séquence d'instantané est utilisé.|  
|**independent_agent**|**bit**|Spécifie s’il existe un Agent de Distribution autonome pour cette publication.<br /><br /> **0** = la publication utilise un Agent de Distribution partagé, et chaque paire de base de données de serveur de publication/abonné de base de données a un seul Agent partagé.<br /><br /> **1** = il existe un Agent de Distribution autonome pour cette publication.|  
|**subscription_time**|**datetime**|À usage interne uniquement|  
|**loopback_detection**|**bit**|S'applique aux abonnements qui font partie d'une topologie de réplication transactionnelle bidirectionnelle. La détection de boucle détermine si l'Agent de distribution renvoie à l'Abonné les transactions émanant de ce dernier :<br /><br /> **1** = ne pas renvoyer.<br /><br /> **0** = renvoie les transactions.|  
|**agent_id**|**int**|Identificateur unique de l'Agent de distribution.|  
|**update_mode**|**tinyint**|Indique le type du mode de mise à jour, qui peut être l'un des suivants :<br /><br /> **0** = lecture seule.<br /><br /> **1** = mise à jour immédiate.<br /><br /> **2** = mise à jour en file d’attente à l’aide de Message Queuing.<br /><br /> **3** = exécution mettre à jour avec la mise à jour en file d’attente sous forme de basculement à l’aide de Message Queuing.<br /><br /> **4** = mise à jour en file d’attente à l’aide de la file d’attente de SQL Server.<br /><br /> **5** = mise à jour immédiate avec basculement de mise à jour en file d’attente, à l’aide de la file d’attente de SQL Server.|  
|**publisher_seqno**|**varbinary(16)**|Numéro de séquence de la transaction au niveau du serveur de publication pour cet abonnement.|  
|**ss_cplt_seqno**|**varbinary(16)**|Numéro de séquence utilisé pour indiquer la fin du traitement de l'instantané concurrent.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
