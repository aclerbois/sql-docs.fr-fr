---
title: Sys.dm_xe_sessions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_xe_sessions_TSQL
- dm_xe_sessions
- sys.dm_xe_sessions_TSQL
- sys.dm_xe_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_sessions dynamic management view
- extended events [SQL Server], views
ms.assetid: defd6efb-9507-4247-a91f-dc6ff5841e17
caps.latest.revision: 17
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5e4e4e6655aeffd54d06cc1cf23aa60208076d18
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmxesessions-transact-sql"></a>sys.dm_xe_sessions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur une session Événements étendus active. Cette session est une collection d'événements, d'actions et de cibles.  
    
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|address|**varbinary(8)**|Adresse mémoire de la session. adresse est unique sur le système local. N'accepte pas la valeur NULL.|  
|name|**nvarchar (256)**|Le nom de la session. nom est unique dans le système local. N'accepte pas la valeur NULL.|  
|pending_buffers|**int**|Nombre de mémoires tampons saturées en attente de traitement. N'accepte pas la valeur NULL.|  
|total_regular_buffers|**int**|Nombre total de mémoires tampons standard associées à la session. N'accepte pas la valeur NULL.<br /><br /> Remarque : Les mémoires tampons standard sont utilisées la plupart du temps. La taille de ces mémoires tampons est suffisante pour contenir de nombreux événements. En général, il y a au moins trois mémoires tampons par session. Le nombre de mémoires tampons standard est déterminé automatiquement par le serveur, selon le partitionnement de la mémoire défini à travers l'option MEMORY_PARTITION_MODE. La taille des mémoires tampons standard est égale à la valeur de l'option MAX_MEMORY (la valeur par défaut est de 4 Mo) divisée par le nombre de mémoires tampons. Pour plus d’informations sur le MEMORY_PARTITION_MODE et les options de MAX_MEMORY, consultez [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md).|  
|regular_buffer_size|**bigint**|Taille de la mémoire tampon standard, en octets. N'accepte pas la valeur NULL.|  
|total_large_buffers|**int**|Nombre total de mémoires tampons de grande taille. N'accepte pas la valeur NULL.<br /><br /> Remarque : Les tampons de grande taille sont utilisées lorsqu’un événement est supérieur à une mémoire tampon standard. Elles sont explicitement réservées à cet effet. Les mémoires tampons de grande taille sont allouées lorsque la session d'événements démarre et sont dimensionnées en fonction de l'option MAX_EVENT_SIZE. Pour plus d’informations sur l’option MAX_EVENT_SIZE, consultez [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md).|  
|large_buffer_size|**bigint**|Taille de la mémoire tampon de grande taille, en octets. N'accepte pas la valeur NULL.|  
|total_buffer_size|**bigint**|Taille totale de la mémoire tampon utilisée pour stocker des événements de la session, en octets. N'accepte pas la valeur NULL.|  
|buffer_policy_flags|**int**|Bitmap qui indique comment les mémoires tampons d'événements de session se comportent lorsque toutes les mémoires tampons sont saturées et qu'un nouvel élément est déclenché. N'accepte pas la valeur NULL.|  
|buffer_policy_desc|**nvarchar (256)**|Description qui indique comment les mémoires tampons d'événements de session se comportent lorsque toutes les mémoires tampons sont saturées et qu'un nouvel élément est déclenché.  N'accepte pas la valeur NULL. buffer_policy_desc peut prendre la valeur de l’une des opérations suivantes :<br /><br /> Supprimer l'événement<br /><br /> Ne pas supprimer d'événements<br /><br /> Supprimer la mémoire tampon saturée<br /><br /> Allouer une nouvelle mémoire tampon|  
|flags|**int**|Bitmap qui indique les indicateurs définis sur la session. N'accepte pas la valeur NULL.|  
|flag_desc|**nvarchar (256)**|Description des indicateurs définis sur la session.  N'accepte pas la valeur NULL. flag_desc peut être n’importe quelle combinaison des éléments suivants :<br /><br /> Vider les mémoires tampons à la fermeture<br /><br /> Répartiteur dédié<br /><br /> Autoriser les événements récursifs|  
|dropped_event_count|**int**|Nombre d'événements supprimés lorsque les mémoires tampons étaient saturées. Cette valeur est **0** si la stratégie de la mémoire tampon est « Supprimer la mémoire tampon complète » ou « Ne pas supprimer les événements ». N'accepte pas la valeur NULL.|  
|dropped_buffer_count|**int**|Nombre de mémoires tampons supprimées lorsque les mémoires tampons étaient saturées. Cette valeur est **0** si la stratégie de tampon a la valeur « Supprimer l’événement » ou « Ne pas supprimer les événements ». N'accepte pas la valeur NULL.|  
|blocked_event_fire_time|**int**|Durée pendant laquelle les déclenchements d'événements ont été bloqués car les mémoires tampons étaient saturées. Cette valeur est **0** si la stratégie de la mémoire tampon est « Supprimer la mémoire tampon complète » ou « Supprimer l’événement ». N'accepte pas la valeur NULL.|  
|create_time|**datetime**|Heure de création de la session. N'accepte pas la valeur NULL.|  
|largest_event_dropped_size|**int**|Taille de l'événement le plus grand parmi ceux pour lesquels la mémoire tampon de session n'a pas suffi. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="change-history"></a>Historique des modifications  
  
|Mise à jour du contenu|  
|---------------------|  
|Correction du type de données pour les colonnes name et blocked_event_fire_time.|  
|Suppression des colonnes buffer_size et total_buffers.|  
|Ajouter les colonnes de colonnes total_regular_buffers, regular_buffer_size, total_large_buffers, large_buffer_size et total_buffer_size.|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

