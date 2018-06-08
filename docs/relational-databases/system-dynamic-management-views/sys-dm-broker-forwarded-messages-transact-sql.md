---
title: Sys.dm_broker_forwarded_messages (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_broker_forwarded_messages
- dm_broker_forwarded_messages
- sys.dm_broker_forwarded_messages_TSQL
- dm_broker_forwarded_messages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_forwarded_messages dynamic management view
ms.assetid: 5576376d-6364-417a-8475-aa770e060845
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a4a772419ba49436cefd1a135bed3859c5905ae2
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmbrokerforwardedmessages-transact-sql"></a>sys.dm_broker_forwarded_messages (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque message de Service Broker qu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en train de retransmettre.  
  

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**conversation_id**|**uniqueidentifier**|Identificateur de la conversation à laquelle appartient le message. Accepte la valeur NULL.|  
|**is_initiator**|**bit**|Indique si le message provient de l'initiateur de la conversation.  Accepte la valeur NULL.<br /><br /> 0 = Non activé par l'initiateur<br /><br /> 1 = Activé par l'initiateur|  
|**to_service_name**|**nvarchar(512)**|Nom du service auquel ce message est envoyé. Accepte la valeur NULL.|  
|**to_broker_instance**|**nvarchar(512)**|Identificateur du broker qui héberge le service auquel le message est envoyé. Accepte la valeur NULL.|  
|**from_service_name**|**nvarchar(512)**|Nom du service dont ce message provient. Accepte la valeur NULL.|  
|**from_broker_instance**|**nvarchar(512)**|Identificateur du broker qui héberge le service dont provient le message. Accepte la valeur NULL.|  
|**adjacent_broker_address**|**nvarchar(512)**|Adresse réseau à laquelle le message est envoyé. Accepte la valeur NULL.|  
|**message_sequence_number**|**bigint**|Numéro de séquence du message dans la boîte de dialogue. Accepte la valeur NULL.|  
|**message_fragment_number**|**int**|Si le message est fragmenté, numéro du fragment transporté par le message. Accepte la valeur NULL.|  
|**hops_remaining**|**tinyint**|Nombre de tentatives de retransmission du message jusqu'à sa destination finale. Cette valeur est décrémentée de 1 à chaque transfert du message. Accepte la valeur NULL.|  
|**time_to_live**|**int**|Durée maximale d'activité du message. Lorsque cette valeur atteint 0, le message est supprimé. Accepte la valeur NULL.|  
|**time_consumed**|**int**|Durée totale d'activité du message. Chaque fois que le message est transféré, cette valeur est augmentée du temps de transfert correspondant. Cette colonne n'accepte pas la valeur NULL.|  
|**message_id**|**uniqueidentifier**|ID du message. Accepte la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique & #40 ; liées à Service Broker Transact-SQL & #41 ;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

