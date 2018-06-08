---
title: Sys.dm_db_stats_properties (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_stats_properties_TSQL
- sys.dm_db_stats_properties
- dm_db_stats_properties_TSQL
- dm_db_stats_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_stats_properties
ms.assetid: 8a54889d-e263-4881-9fcb-b1db410a9453
caps.latest.revision: 13
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a586933111d5bc08a36d7c0818d33e74975e7a4d
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbstatsproperties-transact-sql"></a>sys.dm_db_stats_properties (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne les propriétés de statistiques de l'objet de base de données spécifié (table ou vue indexée) dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] active. Pour les tables partitionnées, consultez similaire [sys.dm_db_incremental_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md). 
 
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_db_stats_properties (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Arguments  
 *object_id*  
 ID de l'objet dans la base de données active dont les propriétés d'une de ses statistiques sont demandées. *l’object_id* est **int**.  
  
 *stats_id*  
 ID des statistiques pour *l’object_id*. spécifié. L’ID des statistiques peut être obtenu à partir de la vue de gestion dynamique [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* correspond à **int**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID de l'objet (table ou vue indexée) pour lequel retourner les propriétés de l'objet de statistiques.|  
|stats_id|**int**|ID de l'objet de statistiques. Unique dans la table ou la vue indexée. Pour plus d’informations, consultez [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|  
|last_updated|**datetime2**|Date et heure de la dernière mise à jour de l'objet de statistiques. Pour plus d’informations, consultez la section [Notes](#Remarks) dans cette page.|  
|lignes|**bigint**|Nombre total de lignes dans la table ou la vue indexée au moment de la dernière mise à jour des statistiques. Si les statistiques sont filtrées ou correspondent à un index filtré, le nombre de lignes peut être inférieur à celui de la table.|  
|rows_sampled|**bigint**|Nombre total de lignes échantillonnées pour le calcul des statistiques.|  
|étapes|**int**|Nombre d'étapes dans l'histogramme. Pour plus d’informations, consultez [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).|  
|unfiltered_rows|**bigint**|Nombre total de lignes dans la table avant l'application de l'expression de filtre (pour les statistiques filtrées). Si les statistiques ne sont pas filtrées, unfiltered_rows est égal à la valeur retournée dans la colonne rows.|  
|modification_counter|**bigint**|Nombre total de modifications de la première colonne de statistiques (la colonne sur laquelle l'histogramme est construit) depuis la dernière mise à jour des statistiques.<br /><br /> Tables optimisées en mémoire : démarrage [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] cette colonne contient : nombre total de modifications pour la table étant donné que les statistiques de dernière mise à jour ou de la base de données a été redémarré.|  
|persisted_sample_percent|**float**|Pourcentage d’échantillon persistant utilisé pour les mises à jour des statistiques qui ne spécifient pas explicitement un pourcentage d’échantillonnage. Si la valeur est zéro, aucun pourcentage d’échantillon persistant n’est défini pour cette statistique.<br /><br /> **S’applique à :** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1, mise à jour cumulative 4|  
  
## <a name="Remarks"></a> Notes  
 **Sys.dm_db_stats_properties** renvoie un ensemble de lignes vide si l’une des conditions suivantes :  
  
-   **object_id** ou **stats_id** a la valeur NULL.    
-   L'objet spécifié est introuvable ou ne correspond à aucune table ou vue indexée.    
-   L'ID de statistiques spécifié ne correspond pas à des statistiques existantes pour l'ID d'objet spécifié.    
-   L'utilisateur actuel n'est pas autorisé à afficher l'objet de statistiques.  
  
 Ce comportement permet à l’utilisation sécurisée de **sys.dm_db_stats_properties** lorsque croisée appliqué aux lignes dans les vues comme **sys.objects** et **sys.stats**.  
 
La date de mise à jour des statistiques est stockée dans l’[objet blob de statistiques](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) avec l’[histogramme](../../relational-databases/statistics/statistics.md#histogram) et le [vecteur de densité](../../relational-databases/statistics/statistics.md#density), et non dans les métadonnées. Lorsqu’aucune donnée n’est en lecture pour générer des données de statistiques, l’objet blob de statistiques n’est pas créé, la date n’est pas disponible et le *last_updated* colonne est NULL. C’est le cas pour les statistiques filtrées pour lesquelles le prédicat ne renvoie aucune ligne, ou pour les nouvelles tables vides.
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit avoir sélectionné des autorisations sur les colonnes de statistiques, ou bien il doit être le propriétaire de la table, ou encore il doit être membre du rôle serveur fixe `sysadmin`, du rôle de base de données fixe `db_owner` ou du rôle de base de données fixe `db_ddladmin`.  
  
## <a name="examples"></a>Exemples  

### <a name="a-simple-example"></a>A. Exemple simple
L’exemple suivant retourne les statistiques pour le `Person.Person` table dans la base de données AdventureWorks.

```sql
SELECT * FROM sys.dm_db_stats_properties (object_id('Person.Person'), 1);
``` 
  
### <a name="b-returning-all-statistics-properties-for-a-table"></a>B. Renvoi de toutes les propriétés de statistiques pour une table  
 L'exemple suivant retourne les propriétés de toutes les statistiques existantes pour la table TEST.  
  
```sql  
SELECT sp.stats_id, name, filter_definition, last_updated, rows, rows_sampled, steps, unfiltered_rows, modification_counter   
FROM sys.stats AS stat   
CROSS APPLY sys.dm_db_stats_properties(stat.object_id, stat.stats_id) AS sp  
WHERE stat.object_id = object_id('TEST');  
```  
  
### <a name="c-returning-statistics-properties-for-frequently-modified-objects"></a>C. Renvoi de toutes les propriétés de statistiques pour les objets modifiés fréquemment  
 L'exemple suivant retourne toutes les tables, les vues indexées et les statistiques de la base de données active, dans lesquelles la première colonne a été modifiée plus de 1000 fois depuis la dernière mise à jour des statistiques.  
  
```sql  
SELECT obj.name, obj.object_id, stat.name, stat.stats_id, last_updated, modification_counter  
FROM sys.objects AS obj   
INNER JOIN sys.stats AS stat ON stat.object_id = obj.object_id  
CROSS APPLY sys.dm_db_stats_properties(stat.object_id, stat.stats_id) AS sp  
WHERE modification_counter > 1000;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [Fonctions et vues de gestion dynamique relatives à l’objet &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
 [Sys.dm_db_incremental_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)  
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 
  

