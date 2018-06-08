---
title: Sys.dm_db_missing_index_details (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_missing_index_details
- dm_db_missing_index_details
- sys.dm_db_missing_index_details_TSQL
- dm_db_missing_index_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- missing indexes feature [SQL Server], sys.dm_db_missing_index_details dynamic management view
- sys.dm_db_missing_index_details dynamic management view
ms.assetid: ced484ae-7c17-4613-a3f9-6d8aba65a110
caps.latest.revision: 37
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7d3b07692b4e12ab4bdd0be15566cf115ad71b76
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbmissingindexdetails-transact-sql"></a>sys.dm_db_missing_index_details (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations détaillées sur les index manquants, à l'exclusion des index spatiaux.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d'exposer ces informations, chaque ligne contenant des données qui n'appartient pas au locataire connecté est filtrée.  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**index_handle**|**int**|Identifie un index manquant. L'identificateur est unique sur le serveur. **index_handle** est la clé de cette table.|  
|**database_id**|**smallint**|Identifie la base de données dans laquelle réside la table comportant les index manquants.|  
|**object_id**|**int**|Identifie la table dans laquelle est situé l'index manquant.|  
|**equality_columns**|**nvarchar(4000)**|Liste de colonnes, séparées par des virgules, qui contribuent aux prédicats d'égalité au format :<br /><br /> *table.column* =*constant_value*|  
|**inequality_columns**|**nvarchar(4000)**|Liste de colonnes, séparées par des virgules, qui contribuent aux prédicats d'inégalité, par exemple, les prédicats au format :<br /><br /> *table.column* > *constant_value*<br /><br /> Tout opérateur de comparaison autre que "=" exprime l'inégalité.|  
|**included_columns**|**nvarchar(4000)**|Liste de colonnes, séparées par des virgules, requises comme colonnes de couverture pour la requête. Pour plus d’informations sur la couverture ou les colonnes incluses, consultez [créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md).<br /><br /> Pour les index optimisés en mémoire (hachage et optimisées en mémoire non ordonné en clusters), ignorer **included_columns**. Toutes les commandes de la table sont incluses dans chaque index optimisé en mémoire.|  
|**instruction**|**nvarchar(4000)**|Nom de la table dans laquelle est situé l'index manquant.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par **sys.dm_db_missing_index_details** est mis à jour lorsqu’une requête est optimisée par l’optimiseur de requête et n’est pas persistant. Les informations sur les index manquants sont simplement conservées jusqu'au redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les administrateurs de base de données doivent effectuer régulièrement des copies de sauvegarde des informations sur les index manquants s'ils souhaitent les conserver après le recyclage du serveur.  
  
 Pour déterminer quels index manquant regroupe un index manquant fait partie, vous pouvez interroger la **sys.dm_db_missing_index_groups** vue de gestion dynamique en établissant une équijointure avec **sys.dm_db_missing_index_details** selon la **index_handle** colonne.  
  
## <a name="using-missing-index-information-in-create-index-statements"></a>Utilisation des informations sur les index manquants dans les instructions CREATE INDEX  
 Pour convertir les informations retournées par **sys.dm_db_missing_index_details** dans une instruction CREATE INDEX pour les index sur disque et optimisées en mémoire, les colonnes d’égalité doivent être placées avant les colonnes d’inégalité, et ensemble qu’ils doivent constituer la clé de l’index. Les colonnes incluses doivent être ajoutées à l'instruction CREATE INDEX à l'aide de la clause INCLUDE. Pour déterminer un ordre efficace pour les colonnes d'égalité, vous devez les organiser en fonction de leur sélectivité : répertoriez les colonnes les plus sélectives (les colonnes de gauche dans la liste des colonnes).  
  
 Pour plus d’informations sur les index optimisés en mémoire, consultez [index des Tables mémoire optimisées](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).  
  
## <a name="transaction-consistency"></a>Cohérence transactionnelle  
 Si une transaction crée ou supprime une table, les lignes qui contiennent les informations d'index manquants concernant les objets supprimés sont retirées de cet objet de gestion dynamique, ce qui permet de préserver la cohérence des transactions.  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="see-also"></a>Voir aussi  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
  
  
