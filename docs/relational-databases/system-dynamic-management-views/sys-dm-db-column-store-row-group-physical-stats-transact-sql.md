---
title: Sys.dm_db_column_store_row_group_physical_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 05/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_column_store_row_group_physical_stats_TSQL
- sys.dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_column_store_row_group_physical_stats
- sys.dm_db_column_store_row_group_physical_stats dynamic management view
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 9f48268c800345ed66f065e444a8d7cfe09a6a30
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbcolumnstorerowgroupphysicalstats-transact-sql"></a>Sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Fournit des informations au niveau du groupe de lignes en cours sur tous les index dans la base de données actuelle.  
  
 Cela permet d’étendre l’affichage catalogue [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table sous-jacente.|  
|**index_id**|**int**|ID de cet index columnstore sur *object_id* table.|  
|**partition_number**|**int**|ID de la partition de table qui contient *row_group_id*. Utilisez partition_number pour joindre cette vue DMV à sys.partitions.|  
|**row_group_id**|**int**|ID de ce groupe de lignes. Pour les tables partitionnées, il est unique au sein de la partition.<br /><br /> -1 pour une fin en mémoire.|  
|**delta_store_hobt_id**|**bigint**|Hobt_id pour un groupe de lignes dans la banque delta.<br /><br /> NULL si le groupe de lignes n’est pas dans la banque delta.<br /><br /> Valeur NULL de fin d’une table en mémoire.|  
|**state**|**tinyint**|Numéro d’identification associé *state_description*.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED<br /><br /> 4 = DÉSACTIVÉ (TOMBSTONE)<br /><br /> COMPRESSÉ est le seul état qui s’applique aux tables en mémoire.|  
|**state_desc**|**nvarchar(60)**|Description de l’état du groupe de lignes :<br /><br /> INVISIBLE – un groupe de lignes en cours de construction. Par exemple : <br />Un groupe de lignes dans le columnstore est INVISIBLE, tandis que les données compressées. Une fois que la compression est une modification des métadonnées commutateur l’état de la ligne de columnstore groupe entre INVISIBLE compressé et l’état du groupe de lignes deltastore fermé à désactivé (tombstone).<br /><br /> Ouvrez – un groupe de lignes du deltastore qui accepte les nouvelles lignes. Un groupe de lignes ouvert est toujours au format rowstore et n'a pas été compressé au format columnstore.<br /><br /> CLOSED – un groupe de lignes dans la banque delta qui contient le nombre maximal de lignes et est en attente pour le processus de déplacement de tuple de les compresser dans le columnstore.<br /><br /> COMPRESSED – un groupe de lignes qui est la compression et stocké dans le columnstore.<br /><br /> TOMBSTONE – un groupe de lignes qui était auparavant dans le deltastore et n’est plus utilisé.|  
|**total_rows**|**bigint**|Nombre de lignes physiques stocké dans le groupe de lignes. Pour les groupes de lignes compressés, cela inclut les lignes qui sont marqués à supprimer.|  
|**deleted_rows**|**bigint**|Nombre de lignes stockées physiquement dans un groupe de lignes compressés qui sont marqués pour suppression.<br /><br /> 0 pour les groupes de lignes qui se trouvent dans la banque delta.|  
|**size_in_bytes**|**bigint**|Taille combinée, en octets, de toutes les pages de ce groupe de lignes. Cette taille n’inclut pas la taille requise pour stocker les métadonnées ou les dictionnaires partagés.|  
|**trim_reason**|**tinyint**|Raison qui a déclenché le groupe de lignes compressés à inférieur le nombre maximal de lignes.<br /><br /> 0 – UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION<br /><br /> 1 - NO_TRIM<br /><br /> 2 - CHARGEMENT EN MASSE<br /><br /> 3 – REORG<br /><br /> 4 – DICTIONARY_SIZE<br /><br /> 5 – MEMORY_LIMITATION<br /><br /> 6 – RESIDUAL_ROW_GROUP<br /><br /> 7 - STATS_MISMATCH<br /><br /> 8 - EXCÉDENTAIRE|  
|**trim_reason_desc**|**nvarchar(60)**|Description de *trim_reason*.<br /><br /> 0 – UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION : s’est produite lors de la mise à niveau à partir de la version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 - NO_TRIM : Le groupe de lignes n’a pas été tronqué. Le groupe de lignes a été compressé avec le nombre maximal de 1,048,476 lignes.  Le nombre de lignes peut être inférieure si un subsset de lignes a été supprimé après la fermeture de rowgroup delta<br /><br /> 2 – BULKLOAD : La taille de lot de chargement en masse limité le nombre de lignes.<br /><br /> 3 – REORG : Forcé de la compression dans le cadre de la commande REORG.<br /><br /> 4 – DICTIONARY_SIZE : Taille du dictionnaire a augmenté de trop grande pour compresser toutes les lignes ensemble.<br /><br /> 5 – MEMORY_LIMITATION : Mémoire insuffisante pour compresser toutes les lignes ensemble.<br /><br /> 6 – RESIDUAL_ROW_GROUP : Fermé dans le cadre du dernier groupe de lignes avec des millions de lignes < 1 pendant l’opération de création d’index<br /><br /> STATS_MISMATCH : uniquement pour columnstore sur la table en mémoire. Si les statistiques incorrectement indiqué > = 1 million de lignes complet dans la fin, mais nous avons trouvé moins, le rowgroup compressé a < 1 millions de lignes<br /><br /> EXCÉDENTAIRE : uniquement pour columnstore sur la table en mémoire. Si la fin du a > 1 millions de lignes complet, les dernières lignes restantes du lot sont compressés si le nombre est compris entre 100 Ko et 1 million|  
|**transition_to_compressed_state**|tinyint|Montre comment ce groupe de lignes déplacée deltastore vers un état compressé dans le columnstore.<br /><br /> 1 - NOT_APPLICABLE<br /><br /> 2 – INDEX_BUILD<br /><br /> 3 – TUPLE_MOVER<br /><br /> 4 – REORG_NORMAL<br /><br /> 5 – REORG_FORCED<br /><br /> 6 - CHARGEMENT EN MASSE<br /><br /> 7 - FUSION|  
|**transition_to_compressed_state_desc**|nvarchar(60)|NOT_APPLICABLE – l’opération ne s’applique pas au deltastore. Ou, le rowgroup a été compressé avant la mise à niveau vers [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] dans ce cas l’historique n’est pas conservé.<br /><br /> Création d’un index INDEX_BUILD – ou reconstruction d’index compressés du rowgroup.<br /><br /> TUPLE_MOVER : le moteur de tuple en cours d’exécution en arrière-plan compressé rowgroup. Cela se produit une fois que le rowgroup modifie l’état d’ouvert ou fermé.<br /><br /> REORG_NORMAL : l’opération de réorganisation, ALTER INDEX... RÉORGANISATION déplacé rowgroup fermé du deltastore dans le columnstore. Cela s’est produite avant que le moteur de tuple eu le temps pour déplacer le groupe de lignes.<br /><br /> REORG_FORCED – cette rowgroup a été ouvert dans le deltastore et a été forcé dans le columnstore avant qu’elle avait un nombre total de lignes.<br /><br /> Chargement en masse : une opération de chargement en bloc compressé rowgroup directement sans utiliser le deltastore.<br /><br /> FUSION : une opération de fusion consolidées d’un ou plusieurs groupes de lignes dans ce groupe de lignes et ensuite effectué la compression columnstore.|  
|**has_vertipaq_optimization**|bit|L’optimisation Vertipaq améliore la compression columnstore en réorganisant l’ordre des lignes dans le groupe de lignes à un taux de compression plus élevée. Cette optimisation se produit automatiquement dans la plupart des cas. Il existe deux cas Vertipaq optimisation n’est pas utilisée :<br/>  A. Dans ce cas lorsqu’un rowgroup delta se déplace dans le columnstore et qu’il existe un ou plusieurs index non cluster sur l’index columnstore - Vertipaq optimisation est ignorée pour réduit des modifications apportées à l’index de mappage ;<br/> B. pour les index columnstore sur les tables optimisées en mémoire. <br /><br /> 0 = Non<br /><br /> 1 = Oui|  
|**génération**|bigint|Génération du groupe de lignes associée à ce groupe de lignes.|  
|**created_time**|datetime2|Heure de création de ce groupe de lignes.<br /><br /> NULL : pour un index columnstore sur une table en mémoire.|  
|**closed_time**|datetime2|Heure de fermeture de ce groupe de lignes.<br /><br /> NULL : pour un index columnstore sur une table en mémoire.|  
  
## <a name="results"></a>Résultats  
 Retourne une ligne pour chaque groupe de lignes dans la base de données actuelle.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite ces autorisations :  
  
-   Autorisation CONTROL sur la table.  
  
-   Autorisation VIEW DATABASE STATE sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calculate-fragmentaton-to-decide-when-to-reorganize-or-rebuild-a-columnstore-index"></a>A. Calculer fragmentaton pour décider du moment réorganiser ou reconstruire un index columnstore.  
 Pour les index columnstore, le pourcentage de lignes supprimées est une bonne mesure de la fragmentation dans un groupe de lignes. Lorsque la fragmentation est de 20 % ou plus nous vous recommandons de supprimer les lignes supprimées.  Pour obtenir des exemples, consultez [défragmentation des index Columnstore](~/relational-databases/indexes/columnstore-indexes-defragmentation.md).  
  
 Cet exemple joint **sys.dm_db_column_store_row_group_physical_stats** avec un autre système des tables, puis calcule la `Fragmentation` colonne sous la forme d’une estimation de l’efficacité de chaque groupe de lignes dans la base de données actuelle.     Pour rechercher des informations sur une seule table, supprimez le commentaire des traits d’union devant le **où** clause et fournir un nom de table.  
  
```sql  
SELECT i.object_id,   
    object_name(i.object_id) AS TableName,   
    i.name AS IndexName,   
    i.index_id,   
    i.type_desc,   
    CSRowGroups.*,  
    100*(ISNULL(deleted_rows,0))/total_rows AS 'Fragmentation'  
FROM sys.indexes AS i  
JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id AND i.index_id = CSRowGroups.index_id   
-- WHERE object_name(i.object_id) = 'table_name'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation des catalogues système SQL Server FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Guide des index columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)   
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
