---
title: sp_check_dynamic_filters (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- dynamic_filters_TSQL
- sp_check_TSQL
- check
- sp_check_dynamic filter
- check_TSQL
- filters_TSQL
- check_dynamic_filters_TSQL
- dynamic filters
- filters
- check dynamic filters
- sp_check_dynamic filter_TSQL
- sp_check_for_sync_trigger_TSQL
- sp_check
helpviewer_keywords:
- sp_check_dynamic_filters
ms.assetid: dd7760db-a3a5-460f-bd97-b8d436015e19
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 248a5234a0620ce18122723db7bce88c61201758
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spcheckdynamicfilters-transact-sql"></a>sp_check_dynamic_filters (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche des informations sur les propriétés du filtre de lignes paramétrable d'une publication, notamment les fonctions utilisées pour générer une partition de données filtrées pour une publication, et indique si la publication peut utiliser des partitions précalculées. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_check_dynamic_filters [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publication**=] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Est si la publication peut utiliser des partitions précalculées ; où **1** signifie que les partitions précalculées peuvent être utilisées, et **0** signifie qu’ils ne peuvent pas être utilisés.|  
|**has_dynamic_filters**|**bit**|Est si le filtre de lignes paramétrable au moins a été défini dans la publication. où **1** signifie qu’un ou plusieurs filtres de lignes paramétrable existent, et **0** signifie qu’aucun filtre dynamique n’existe.|  
|**dynamic_filters_function_list**|**nvarchar(500)**|Indique les fonctions utilisées pour filtrer les articles dans une publication, chaque fonction étant séparée par un point-virgule.|  
|**validate_subscriber_info**|**nvarchar(500)**|Indique les fonctions utilisées pour filtrer les articles dans une publication, chaque fonction étant séparée par un signe plus (+).|  
|**uses_host_name**|**bit**|Si le [HOST_NAME()](../../t-sql/functions/host-name-transact-sql.md) fonction est utilisée dans les filtres de lignes paramétrables, où **1** signifie que cette fonction est utilisée pour le filtrage dynamique.|  
|**uses_suser_sname**|**bit**|Si le [SUSER_SNAME()](../../t-sql/functions/suser-sname-transact-sql.md) fonction est utilisée dans les filtres de lignes paramétrables, où **1** signifie que cette fonction est utilisée pour le filtrage dynamique.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_check_dynamic_filters** est utilisé dans la réplication de fusion.  
  
 Si une publication a été définie pour utiliser des partitions précalculées, **sp_check_dynamic_filters** recherche les violations des restrictions de partitions précalculées. S'il en existe, une erreur est renvoyée. Pour plus d’informations, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 Si une publication est définie comme ayant des filtres de lignes paramétrables et qu'aucun filtrage n'est trouvé, une erreur est renvoyée.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_check_dynamic_filters**.  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer les Partitions d’une Publication de fusion avec des filtres paramétrés](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [sp_check_join_filter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-check-join-filter-transact-sql.md)   
 [sp_check_subset_filter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-check-subset-filter-transact-sql.md)  
  
  
