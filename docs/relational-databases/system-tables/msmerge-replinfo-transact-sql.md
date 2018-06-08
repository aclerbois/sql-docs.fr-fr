---
title: MSmerge_replinfo (Transact-SQL) | Documents Microsoft
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
- MSmerge_replinfo_TSQL
- MSmerge_replinfo
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_replinfo system table
ms.assetid: b0924094-c0cc-49c1-869a-65be0d0465a0
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 44bd1de5c31f43c56e2fe7cb90bfe8e6585ad45d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmergereplinfo-transact-sql"></a>MSmerge_replinfo (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_replinfo** table contient une ligne pour chaque abonnement. Cette table effectue le suivi des informations relatives aux abonnements. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**repid**|**uniqueidentifier**|ID unique du réplica.|  
|**use_interactive_resolver**|**bit**|Indique si le résolveur interactif est utilisé au cours de la réconciliation.<br /><br /> **0** = ne pas utiliser le résolveur interactif.<br /><br /> **1** = utiliser le résolveur interactif.|  
|**validation_level**|**int**|Type de validation à réaliser sur l'abonnement. Le niveau de validation spécifié peut prendre l'une des valeurs suivantes :<br /><br /> **0** = pas de validation.<br /><br /> **1** = validation du nombre de lignes uniquement.<br /><br /> **2** = validation du nombre de lignes et somme de contrôle.<br /><br /> **3** = nombre de lignes et de validation de somme de contrôle binaire.|  
|**resync_gen**|**bigint**|Numéro de génération utilisé pour la resynchronisation de l'abonnement. La valeur **-1** indique que l’abonnement n’est pas marqué pour la resynchronisation.|  
|**login_name**|**sysname**|Nom de l'utilisateur qui a créé l'abonnement.|  
|**Nom d’hôte**|**sysname**|Valeur utilisée par le filtre de lignes paramétré lors de la génération de la partition de l'abonnement.|  
|**merge_jobid**|**binary (16)**|Identificateur du travail de fusion pour cet abonnement.|  
|**sync_info**|**int**|Interne-usage.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
