---
title: sys.database_automatic_tuning_options (Transact-SQL) | Microsoft Docs
description: "Découvrez comment afficher les options de paramétrage automatique sur une base de données SQL"
ms.custom: 
ms.date: 07/20/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database
ms.service: 
ms.component: system-catalog-views
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- database_automatic_tuning_options_tsql
- database_automatic_tuning_options
- sys.database_automatic_tuning_options_tsql
- sys.database_automatic_tuning_options
dev_langs:
- TSQL
helpviewer_keywords:
- database_automatic_tuning_options catalog view
- sys.database_automatic_tuning_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
caps.latest.revision: 
author: jovanpop-msft
ms.author: jovanpop
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: a4208b9e294273444c24ac9e3a05e60b43d4274c
ms.sourcegitcommit: c556eaf60a49af7025db35b7aa14beb76a8158c5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="sysdatabaseautomatictuningoptions-transact-sql"></a>sys.database\_automatic\_tuning_options (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

  Renvoie les options de réglage automatique pour cette base de données.  

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**nvarchar(128)**|Le nom de l’option de réglage automatique. Reportez-vous à [ALTER AUTOMATIC_TUNING de jeu de base de données &#40; Transact-SQL &#41; ](../../t-sql/statements/alter-database-transact-sql-set-options.md) pour les options disponibles.|  
|**desired_state**|**smallint**|Indique le mode de fonctionnement souhaité pour l’option de réglage automatique, définie par l’utilisateur.<br />0 = désactivé<br />1 = ON|  
|**desired_state_desc**|**nvarchar(60)**|Description textuelle du mode de fonctionnement souhaité de l’option de réglage automatique.<br />OFF<br />ON|  
|**actual_state**|**smallint**|Indique le mode de fonctionnement de l’option de réglage automatique.<br />0 = désactivé<br />1 = ON|  
|**actual_state_desc**|**nvarchar(60)**|Description textuelle du mode d’opération réelle de l’option de réglage automatique.<br />OFF<br />ON|  
|**reason**|**smallint**|Indique pourquoi États souhaitées et réelles sont différentes.<br />2 = DÉSACTIVÉ<br />11 = QUERY_STORE_OFF<br />12 = QUERY_STORE_READ_ONLY<br />13 = NOT_SUPPORTED|   
|**reason_desc**|**nvarchar(60)**|Description textuelle de la raison pour laquelle les États souhaitées et réelles sont différents.<br />DÉSACTIVÉ = Option est désactivée par le système<br />QUERY_STORE_OFF = magasin de requêtes est désactivé.<br />QUERY_STORE_READ_ONLY = magasin de requêtes est en mode lecture seule<br />NOT_SUPPORTED = disponible uniquement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise edition| 
  
## <a name="permissions"></a>Autorisations  
 Nécessite le `VIEW DATABASE STATE` autorisation.  
  
## <a name="see-also"></a>Voir aussi  
 [Le paramétrage automatique](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 
