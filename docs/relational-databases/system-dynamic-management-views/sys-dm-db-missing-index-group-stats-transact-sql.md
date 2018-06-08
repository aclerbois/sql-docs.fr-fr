---
title: Sys.dm_db_missing_index_group_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_TSQL
- sys.dm_db_missing_index_group_stats
- dm_db_missing_index_group_stats_TSQL
- dm_db_missing_index_group_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats dynamic management view
ms.assetid: c2886986-9e07-44ea-a350-feeac05ee4f4
caps.latest.revision: 43
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 65b019f8e7b1f9e82e288c9c53732db4e2c7a253
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbmissingindexgroupstats-transact-sql"></a>sys.dm_db_missing_index_group_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations de résumé sur les groupes d'index manquants, à l'exclusion des index spatiaux.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d'exposer ces informations, chaque ligne contenant des données qui n'appartient pas au locataire connecté est filtrée.  
    
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Identifie un groupe d'index manquant. Cet identificateur est unique sur le serveur.<br /><br /> Les autres colonnes fournissent des informations sur toutes les requêtes pour lesquelles l'index du groupe est considéré comme manquant.<br /><br /> Un groupe d'index ne contient qu'un seul index.|  
|**unique_compiles**|**bigint**|Nombre de compilations et de recompilations qui pourraient tirer parti de ce groupe d'index manquants. Les compilations et les recompilations de nombreuses requêtes peuvent contribuer à la valeur de cette colonne.|  
|**user_seeks**|**bigint**|Nombre de recherches résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**user_scans**|**bigint**|Nombre d'analyses résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_user_seek**|**datetime**|Date et heure de la dernière recherche résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_user_scan**|**datetime**|Date et heure de la dernière analyse résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**avg_total_user_cost**|**float**|Coût moyen des requêtes utilisateur qui pourrait être réduit grâce à l'index du groupe.|  
|**avg_user_impact**|**float**|Bénéfice moyen (en pourcentage) dont les requêtes utilisateur pourraient tirer parti si ce groupe d'index manquants était implémenté. Cela signifie que le coût des requêtes diminuerait, en moyenne, de la valeur de ce pourcentage si ce groupe d'index manquants était implémenté.|  
|**system_seeks**|**bigint**|Nombre de recherches résultant de requêtes système (telles que les requêtes de statistiques automatiques) pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé. Pour plus d’informations, consultez [classe d’événements Auto Stats](../../relational-databases/event-classes/auto-stats-event-class.md).|  
|**system_scans**|**bigint**|Nombre d'analyses résultant de requêtes système pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_system_seek**|**datetime**|Date et heure de la dernière recherche système résultant de requêtes système pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_system_scan**|**datetime**|Date et heure de la dernière analyse système résultant de requêtes système pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**avg_total_system_cost**|**float**|Coût moyen des requêtes système qui pourrait être réduit grâce à l'index du groupe.|  
|**avg_system_impact**|**float**|Bénéfice moyen (en pourcentage) dont les requêtes système pourraient tirer parti si ce groupe d'index manquants était implémenté. Cela signifie que le coût des requêtes diminuerait, en moyenne, de la valeur de ce pourcentage si ce groupe d'index manquants était implémenté.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par **sys.dm_db_missing_index_group_stats** est mis à jour à chaque exécution de requête, et non par chaque compilation de la requête ou la recompilation. Les statistiques d'utilisation ne sont pas conservées de manière permanente ; elles sont simplement conservées jusqu'au redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les administrateurs de base de données doivent effectuer régulièrement des copies de sauvegarde des informations sur les index manquants s'ils souhaitent conserver les statistiques d'utilisation après le recyclage du serveur.  
  
## <a name="permissions"></a>Autorisations  
 Pour interroger cette vue de gestion dynamique, les utilisateurs doivent bénéficier de l'autorisation VIEW SERVER STATE ou de tout privilège qui implique l'autorisation VIEW SERVER STATE.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent comment utiliser le **sys.dm_db_missing_index_group_stats** vue de gestion dynamique.  
  
### <a name="a-find-the-10-missing-indexes-with-the-highest-anticipated-improvement-for-user-queries"></a>A. Trouvez les 10 index manquants qui devraient générer l'amélioration la plus importante pour les requêtes utilisateur  
 La requête suivante détermine les 10 index manquants qui génèreraient l'amélioration cumulée la plus importante, par ordre décroissant, pour les requêtes utilisateur.  
  
```  
SELECT TOP 10 *  
FROM sys.dm_db_missing_index_group_stats  
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans)DESC;  
```  
  
### <a name="b-find-the-individual-missing-indexes-and-their-column-details-for-a-particular-missing-index-group"></a>B. Trouvez les différents index manquants et les détails de leurs colonnes pour un groupe d'index manquants spécifique  
 La requête suivante détermine quels index manquants constituent un groupe d'index manquants spécifique et affiche les détails de leurs colonnes. Dans le cadre de cet exemple, le descripteur du groupe d'index manquants est 24.  
  
```  
SELECT migs.group_handle, mid.*  
FROM sys.dm_db_missing_index_group_stats AS migs  
INNER JOIN sys.dm_db_missing_index_groups AS mig  
    ON (migs.group_handle = mig.index_group_handle)  
INNER JOIN sys.dm_db_missing_index_details AS mid  
    ON (mig.index_handle = mid.index_handle)  
WHERE migs.group_handle = 24;  
```  
  
 Cette requête fournit le nom de la base de données, du schéma et de la table où un index est manquant. Elle fournit également le nom des colonnes devant être utilisées pour la clé d'index. Lors de l’écriture de l’instruction DDL CREATE INDEX pour implémenter les index manquants, indiquez tout d’abord les colonnes d’égalité, puis les colonnes d’inégalité dans le \< *table_name*> clause de l’instruction CREATE INDEX. Les colonnes incluses doivent être indiquées dans la clause INCLUDE de l'instruction CREATE INDEX. Pour déterminer un ordre efficace pour les colonnes d'égalité, organisez ces colonnes en fonction de leur sélectivité, en répertoriant d'abord les colonnes les plus sélectives (les colonnes de gauche dans la liste des colonnes).  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
  
  
