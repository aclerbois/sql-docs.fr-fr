---
title: sp_check_subset_filter (Transact-SQL) | Documents Microsoft
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
- sp_check_TSQL
- sp_check_subset_filter
- filter
- subset
- subset_TSQL
- sp_check
- sp_check_subset_filter_TSQL
helpviewer_keywords:
- sp_check_subset_filter
ms.assetid: 525cfcfc-f317-478d-ba84-72e62285f160
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 29eb4ae1b96c8f9a116b221282ea4b293059b2c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchecksubsetfilter-transact-sql"></a>sp_check_subset_filter (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Est utilisée pour vérifier une clause de filtre en fonction d'une table, afin de déterminer si cette clause est valide pour la table. Cette procédure stockée renvoie des informations sur le filtre fourni, y compris si le filtre peut être utilisé avec des partitions précalculées. Cette procédure stockée est exécutée au niveau du serveur de publication dans la base de données contenant la publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_check_subset_filter [ @filtered_table = ] 'filtered_table'  
        , [ @subset_filterclause = ] 'subset_filterclause'  
    [ , [ @has_dynamic_filters = ] has_dynamic_filters OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@filtered_table**=] **'***filtered_table***'**  
 Nom d'une table filtrée. *filtered_table* est **nvarchar (400)**, sans valeur par défaut.  
  
 [ **@subset_filterclause** =] **'***subset_filterclause***'**  
 Clause de filtre testée. *subset_filterclause* est **nvarchar (1000)**, sans valeur par défaut.  
  
 [ **@has_dynamic_filters**=] *has_dynamic_filters*  
 Indique si la clause de filtre correspond à un filtre de lignes paramétrable. *has_dynamic_filters* est **bits**, avec NULL comme valeur par défaut et est un paramètre de sortie. Retourne une valeur de **1** lorsque la clause de filtre est un filtre de lignes paramétrable.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Est si la publication peut utiliser des partitions précalculées ; où **1** signifie que les partitions précalculées peuvent être utilisées, et **0** signifie qu’ils ne peuvent pas être utilisés.|  
|**has_dynamic_filters**|**bit**|Si la clause de filtre fournie inclut au moins un filtre de lignes paramétrable ; où **1** signifie qu’un filtre de lignes paramétrable est utilisé, et **0** signifie que cette fonction n’est pas utilisée.|  
|**dynamic_filters_function_list**|**nvarchar(500)**|Listes des fonctions de la clause de filtre qui filtrent dynamiquement un article, où chaque fonction est séparée par un point-virgule.|  
|**uses_host_name**|**bit**|Si le [HOST_NAME()](../../t-sql/functions/host-name-transact-sql.md) fonction est utilisée dans la clause de filtre, où **1** signifie que cette fonction est présente.|  
|**uses_suser_sname**|**bit**|Si le [SUSER_SNAME()](../../t-sql/functions/suser-sname-transact-sql.md) fonction est utilisée dans la clause de filtre, où **1** signifie que cette fonction est présente.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_check_subset_filter** est utilisé dans la réplication de fusion.  
  
 **sp_check_subset_filter** peut être exécutée sur une table, même si la table n’est pas publiée. Cette procédure stockée peut être utilisée pour vérifier une clause de filtre avant de définir un article filtré.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_check_subset_filter**.  
  
## <a name="see-also"></a>Voir aussi  
 [Optimiser les performances des filtres paramétrés avec des Partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)  
  
  
