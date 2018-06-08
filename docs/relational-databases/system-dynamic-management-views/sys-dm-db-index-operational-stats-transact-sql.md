---
title: Sys.dm_db_index_operational_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_db_index_operational_stats
- sys.dm_db_index_operational_stats_TSQL
- sys.dm_db_index_operational_stats
- dm_db_index_operational_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_operational_stats dynamic management function
ms.assetid: 13adf2e5-2150-40a6-b346-e74a33ce29c6
caps.latest.revision: 61
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: fb0db9ea7c4d58fdecf8ef4973e4d8f971ebb3d3
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34553799"
---
# <a name="sysdmdbindexoperationalstats-transact-sql"></a>sys.dm_db_index_operational_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

  Retourne e/s de bas niveau en cours, verrouillage, activité d’accès et méthode pour chaque partition d’une table ou un index dans la base de données.    
    
 Les index optimisés en mémoire n'apparaissent pas dans cette vue DMV.    
    
> [!NOTE]    
>  **Sys.dm_db_index_operational_stats** ne retourne pas d’informations sur les index optimisés en mémoire. Pour plus d’informations sur l’utilisation d’index optimisé en mémoire, consultez [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).    
        
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Syntaxe    
    
```    
    
sys.dm_db_index_operational_stats (    
    { database_id | NULL | 0 | DEFAULT }    
  , { object_id | NULL | 0 | DEFAULT }    
  , { index_id | 0 | NULL | -1 | DEFAULT }    
  , { partition_number | NULL | 0 | DEFAULT }    
)    
```    
    
## <a name="arguments"></a>Arguments    
 *database_id* | NULL | 0 | PAR DÉFAUT    
 ID de la base de données. *database_id* est **smallint**. Les entrées autorisées sont l'ID d'une base de données ou la valeur NULL, 0 ou DEFAULT. La valeur par défaut est 0. Les valeurs NULL, 0 et DEFAULT sont des valeurs équivalentes dans ce contexte.    
    
 Spécifiez NULL pour retourner des informations concernant toutes les bases de données de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous spécifiez NULL pour *database_id*, vous devez également spécifier NULL pour *object_id*, *index_id*, et *partition_number*.    
    
 La fonction intégrée [DB_ID](../../t-sql/functions/db-id-transact-sql.md) peut être spécifié.    
    
 *object_id* | NULL | 0 | PAR DÉFAUT    
 Identificateur d'objet de la table ou de la vue sur laquelle l'index est défini. *l’object_id* est **int**.    
    
 Les entrées autorisées sont l'ID d'une table et d'une vue ou la valeur NULL, 0 ou DEFAULT. La valeur par défaut est 0. Les valeurs NULL, 0 et DEFAULT sont des valeurs équivalentes dans ce contexte.    
    
 Spécifiez la valeur NULL pour retourner des informations mises en cache pour toutes les tables et les vues de la base de données spécifiée. Si vous spécifiez NULL pour *object_id*, vous devez également spécifier NULL pour *index_id* et *partition_number*.    
    
 *index_id* | 0 | NULL | -1 | PAR DÉFAUT    
 Identificateur de l'index. *index_id* est **int**. Les entrées autorisées sont l’ID d’un index, 0 si *object_id* est un segment, NULL, -1 ou DEFAULT. La valeur par défaut est -1. Les valeurs NULL, -1 et DEFAULT sont des valeurs équivalentes dans ce contexte.    
    
 Spécifiez la valeur NULL pour retourner des informations mises en cache pour tous les index d'une table de base ou d'une vue. Si vous spécifiez NULL pour *index_id*, vous devez également spécifier NULL pour *partition_number*.    
    
 *partition_number* | NULL | 0 | PAR DÉFAUT    
 Numéro de partition dans l’objet. *partition_number* est **int**. Les entrées valides sont les *partion_number* d’un index ou le segment, NULL, 0 ou DEFAULT. La valeur par défaut est 0. Les valeurs NULL, 0 et DEFAULT sont des valeurs équivalentes dans ce contexte.    
    
 Spécifiez la valeur NULL pour retourner des informations mises en cache pour toutes les partitions de l'index ou du segment de mémoire.    
    
 *partition_number* est basé sur 1. Un index non partitionné ou le segment de mémoire a *partition_number* défini sur 1.    
    
## <a name="table-returned"></a>Table retournée    
    
|Nom de colonne|Type de données|Description|    
|-----------------|---------------|-----------------|    
|**database_id**|**smallint**|ID de la base de données.|    
|**object_id**|**Int**|ID de la table ou de la vue.|    
|**index_id**|**Int**|ID de l'index ou du segment de mémoire.<br /><br /> 0 = Segment de mémoire|    
|**hobt_id**|**bigint**|**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> ID du segment de données ou de B-tree de lignes qui effectue le suivi des données internes pour un index columnstore.<br /><br /> NULL : Ceci n’est pas un ensemble de lignes columnstore interne.<br /><br /> Pour plus d’informations, consultez [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)|    
|**partition_number**|**Int**|Numéro de partition (basé sur la valeur 1) au sein de l'index ou du segment de mémoire.|    
|**leaf_insert_count**|**bigint**|Nombre cumulatif d'insertions de niveau feuille.|    
|**leaf_delete_count**|**bigint**|Nombre cumulatif de suppressions de niveau feuille. leaf_delete_count est incrémenté uniquement pour les enregistrements supprimés ne sont pas marqués en tant que fantôme tout d’abord. Pour les enregistrements supprimés sont tout d’abord, un fichier fantôme **leaf_ghost_count** est incrémenté à la place.|    
|**leaf_update_count**|**bigint**|Nombre cumulatif de mises à jour de niveau feuille.|    
|**leaf_ghost_count**|**bigint**|Nombre cumulatif de lignes de niveau feuille marquées pour la suppression qui ne sont pas encore supprimées. Ce nombre n’inclut pas les enregistrements qui sont immédiatement supprimées sans être marquées en tant que fantôme. Ces lignes sont supprimées par un thread de nettoyage à intervalles définis. Cette valeur ne comprend pas les lignes qui sont conservées à cause d'une transaction d'isolement d'instantané en attente.|    
|**nonleaf_insert_count**|**bigint**|Nombre cumulatif d'insertions au-dessus du niveau feuille.<br /><br /> 0 = Segment de mémoire ou columnstore|    
|**nonleaf_delete_count**|**bigint**|Nombre cumulatif de suppressions au-dessus du niveau feuille.<br /><br /> 0 = Segment de mémoire ou columnstore|    
|**nonleaf_update_count**|**bigint**|Nombre cumulatif de mises à jour au-dessus du niveau feuille.<br /><br /> 0 = Segment de mémoire ou columnstore|    
|**leaf_allocation_count**|**bigint**|Nombre cumulatif d'allocations de page de niveau feuille dans l'index ou le segment de mémoire.<br /><br /> Pour un index, une allocation de page correspond à un fractionnement de page.|    
|**nonleaf_allocation_count**|**bigint**|Nombre cumulatif d'allocations de page causées par des fractionnements de page au-dessus du niveau feuille.<br /><br /> 0 = Segment de mémoire ou columnstore|    
|**leaf_page_merge_count**|**bigint**|Nombre cumulatif de fusions de pages de niveau feuille. Toujours 0 pour l’index columnstore.|    
|**nonleaf_page_merge_count**|**bigint**|Nombre cumulatif de fusions de pages au-dessus du niveau feuille.<br /><br /> 0 = Segment de mémoire ou columnstore|    
|**range_scan_count**|**bigint**|Nombre cumulatif d'analyses de plage et de table commencées sur l'index ou le segment de mémoire.|    
|**singleton_lookup_count**|**bigint**|Nombre cumulatif d'extractions de ligne unique à partir de l'index ou du segment de mémoire.|    
|**forwarded_fetch_count**|**bigint**|Nombre de lignes extraites via un enregistrement de transfert.<br /><br /> 0 = Index|    
|**lob_fetch_in_pages**|**bigint**|Nombre cumulatif de pages d'objets volumineux (LOB) extraites de l'unité d'allocation LOB_DATA. Ces pages contiennent des données stockées dans des colonnes de type **texte**, **ntext**, **image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, et **xml**. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|    
|**lob_fetch_in_bytes**|**bigint**|Nombre cumulatif d'octets de données LOB extraits.|    
|**lob_orphan_create_count**|**bigint**|Nombre cumulatif de valeurs LOB orphelines créées pour des opérations en bloc.<br /><br /> 0 = Index non cluster|    
|**lob_orphan_insert_count**|**bigint**|Nombre cumulatif de valeurs LOB orphelines insérées au cours d'opérations en bloc.<br /><br /> 0 = Index non cluster|    
|**row_overflow_fetch_in_pages**|**bigint**|Nombre cumulatif de pages de données de dépassement de ligne qui ont été extraites de l'unité d'allocation ROW_OVERFLOW_DATA.<br /><br /> Ces pages contiennent des données stockées dans des colonnes de type **varchar (n)**, **nvarchar (n)**, **varbinary (n)**, et **sql_variant** qui ont été envoyées hors ligne.|    
|**row_overflow_fetch_in_bytes**|**bigint**|Nombre cumulatif d'octets de données de dépassement de ligne extraits.|    
|**column_value_push_off_row_count**|**bigint**|Nombre cumulatif de valeurs de colonne correspondant aux données LOB et aux données de dépassement de ligne qui sont envoyées hors ligne pour qu'une ligne insérée ou mise à jour puisse figurer dans une page.|    
|**column_value_pull_in_row_count**|**bigint**|Nombre cumulatif de valeurs de colonne correspondant aux données LOB et aux données de dépassement de ligne qui sont extraites dans la ligne. Cette situation se produit lorsqu'une opération de mise à jour libère de l'espace dans un enregistrement et permet ainsi d'extraire une ou plusieurs valeurs hors ligne des unités d'allocation LOB_DATA ou ROW_OVERFLOW_DATA dans l'unité d'allocation IN_ROW_DATA.|    
|**row_lock_count**|**bigint**|Nombre cumulatif de verrous de ligne demandés.|    
|**row_lock_wait_count**|**bigint**|Nombre cumulatif de fois où le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu sur un verrou de ligne.|    
|**row_lock_wait_in_ms**|**bigint**|Durée totale en millisecondes pendant laquelle le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu sur un verrou de ligne.|    
|**page_lock_count**|**bigint**|Nombre cumulatif de verrous de page demandés.|    
|**page_lock_wait_count**|**bigint**|Nombre cumulatif de fois où le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu sur un verrou de page.|    
|**page_lock_wait_in_ms**|**bigint**|Durée totale en millisecondes pendant laquelle le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu sur un verrou de page.|    
|**index_lock_promotion_attempt_count**|**bigint**|Nombre cumulatif de fois où le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a essayé de promouvoir des verrous.|    
|**index_lock_promotion_count**|**bigint**|Nombre cumulatif de fois où le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a promu des verrous.|    
|**page_latch_wait_count**|**bigint**|Nombre cumulatif de fois où le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu à cause d'une contention de verrous internes.|    
|**page_latch_wait_in_ms**|**bigint**|Durée cumulée en millisecondes pendant laquelle le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu en raison d'une contention de verrous internes.|    
|**page_io_latch_wait_count**|**bigint**|Nombre cumulatif de fois où le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu sur un verrou d'E/S de page.|    
|**page_io_latch_wait_in_ms**|**bigint**|Durée cumulée en millisecondes pendant laquelle le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a attendu sur un verrou d'E/S de page.|    
|**tree_page_latch_wait_count**|**bigint**|Sous-ensemble de **page_latch_wait_count** qui inclut uniquement les pages de B-tree de niveau supérieur. Toujours 0 pour un segment de mémoire ou un index columnstore.|    
|**tree_page_latch_wait_in_ms**|**bigint**|Sous-ensemble de **page_latch_wait_in_ms** qui inclut uniquement les pages de B-tree de niveau supérieur. Toujours 0 pour un segment de mémoire ou un index columnstore.|    
|**tree_page_io_latch_wait_count**|**bigint**|Sous-ensemble de **page_io_latch_wait_count** qui inclut uniquement les pages de B-tree de niveau supérieur. Toujours 0 pour un segment de mémoire ou un index columnstore.|    
|**tree_page_io_latch_wait_in_ms**|**bigint**|Sous-ensemble de **page_io_latch_wait_in_ms** qui inclut uniquement les pages de B-tree de niveau supérieur. Toujours 0 pour un segment de mémoire ou un index columnstore.|    
|**page_compression_attempt_count**|**bigint**|Nombre de pages évaluées pour la compression de niveau PAGE pour des partitions spécifiques d'une table, d'un index ou d'une vue indexée. Inclut des pages qui n'ont pas été compressées car des économies significatives n'ont pas pu être obtenues. Toujours 0 pour l’index columnstore.|    
|**page_compression_success_count**|**bigint**|Nombre de pages de données compressées à l'aide de la compression PAGE pour des partitions spécifiques d'une table, d'un index ou d'une vue indexée. Toujours 0 pour l’index columnstore.|    
    
## <a name="remarks"></a>Notes    
 Cet objet de gestion dynamique n'accepte pas les paramètres corrélés de CROSS APPLY et OUTER APPLY.    
    
 Vous pouvez utiliser **sys.dm_db_index_operational_stats** pour effectuer le suivi de la durée pendant laquelle les utilisateurs doivent attendre pour lire ou écrire dans une table, un index ou une partition et identifier les tables ou les index qui connaissent une activité d’e/s importante ou des zones réactives.    
    
 Utilisez les colonnes suivantes pour identifier les zones de contention.    
    
 **Pour analyser un modèle commun d’accès à la partition de table ou un index**, utilisez ces colonnes :    
    
-   **leaf_insert_count**    
    
-   **leaf_delete_count**    
    
-   **leaf_update_count**    
    
-   **leaf_ghost_count**    
    
-   **range_scan_count**    
    
-   **singleton_lookup_count**    
    
 Pour identifier les contentions de verrous (internes et externes), utilisez les colonnes suivantes :    
    
-   **page_latch_wait_count** et **page_latch_wait_in_ms**    
    
     Ces colonnes indiquent s'il y a contention de verrous internes sur l'index ou le segment de mémoire et précisent l'importance de ce conflit.    
    
-   **row_lock_count** et **page_lock_count**    
    
     Ces colonnes indiquent combien de fois le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a essayé d'acquérir des verrous externes de ligne et de page.    
    
-   **row_lock_wait_in_ms** et **page_lock_wait_in_ms**    
    
     Ces colonnes indiquent s'il y a contention de verrous externes sur l'index ou le segment de mémoire et précisent l'importance de cette contention.    
    
 **Pour analyser les statistiques d’e/s physiques sur une partition d’index ou segment de mémoire**    
    
-   **page_io_latch_wait_count** et **page_io_latch_wait_in_ms**    
    
     Ces colonnes indiquent si des E/S physiques ont été envoyées pour placer en mémoire les pages de l'index ou du segment et précisent le nombre d'E/S envoyées.    
    
## <a name="column-remarks"></a>Remarques sur les colonnes    
 Les valeurs de **lob_orphan_create_count** et **lob_orphan_insert_count** doivent toujours être égales.    
    
 La valeur dans les colonnes **lob_fetch_in_pages** et **lob_fetch_in_bytes** peut être supérieur à zéro pour les index non ordonnés en clusters qui contiennent une ou plusieurs colonnes LOB en tant que colonnes incluses. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md). De même, la valeur dans les colonnes **row_overflow_fetch_in_pages** et **row_overflow_fetch_in_bytes** peut être supérieur à 0 pour les index non cluster si l’index contient des colonnes qui peuvent être envoyées hors ligne.    
    
## <a name="how-the-counters-in-the-metadata-cache-are-reset"></a>Mode de réinitialisation des compteurs dans le cache des métadonnées    
 Les données retournées par **sys.dm_db_index_operational_stats** existent uniquement tant que l’objet de cache de métadonnées qui représente le segment de mémoire ou index n’est disponible. Ces données ne sont ni persistantes, ni cohérentes d'un point de vue transactionnel. Autrement dit, vous ne pouvez pas utiliser ces compteurs pour déterminer si un index a été utilisé ou pas, ni pour savoir quand il a été utilisé pour la dernière fois. Pour plus d’informations, consultez [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md).    
    
 Les valeurs de chaque colonne sont remises à zéro chaque fois que les métadonnées associées au segment de mémoire ou à l'index sont envoyées dans le cache de métadonnées et les statistiques s'accumulent jusqu'à ce que l'objet cache soit supprimé du cache de métadonnées. Par conséquent, un segment de mémoire actif ou un index aura probablement toujours ses métadonnées dans le cache et les nombres cumulatifs peuvent refléter l'activité depuis le dernier redémarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les métadonnées d'un segment de mémoire ou d'un index moins actif entrent dans le cache et en sortent à mesure qu'elles sont utilisées. Le cache ne contient donc pas forcément des valeurs. La suppression d'un index entraîne l'effacement des statistiques correspondantes en mémoire, de sorte que la fonction n'en fera plus état. D'autres opérations DDL par rapport à l'index peuvent provoquer la remise à zéro de la valeur des statistiques.    
    
## <a name="using-system-functions-to-specify-parameter-values"></a>Utilisation de fonctions système pour spécifier des valeurs de paramètres    
 Vous pouvez utiliser la [!INCLUDE[tsql](../../includes/tsql-md.md)] fonctions [DB_ID](../../t-sql/functions/db-id-transact-sql.md) et [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md) pour spécifier une valeur pour le *database_id* et *object_id* paramètres. Toutefois, la transmission de valeurs non valides à ces fonctions peut entraîner des résultats imprévisibles. Vérifiez systématiquement qu'un ID valide est retourné lorsque vous utilisez DB_ID ou OBJECT_ID. Pour plus d’informations, consultez la section Remarques dans [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).    
    
## <a name="permissions"></a>Autorisations    
 Les autorisations suivantes sont nécessaires :    
    
-   Autorisation CONTROL sur l'objet spécifié dans la base de données    
    
-   Autorisation VIEW DATABASE STATE pour retourner des informations sur tous les objets dans la base de données spécifiée, à l’aide de l’objet générique @*object_id* = NULL    
    
-   Autorisation VIEW SERVER STATE pour retourner des informations sur toutes les bases de données à l’aide de la base de données générique @*database_id* = NULL    
    
 L'octroi de l'autorisation VIEW DATABASE STATE autorise le renvoi de tous les objets de la base de données, quelles que soient les autorisations CONTROL refusées sur des objets spécifiques.    
    
 Le refus de l'autorisation VIEW DATABASE STATE interdit le retour de tous les objets de la base de données, quelles que soient les autorisations CONTROL accordées sur des objets spécifiques. Également, lorsque la base de données générique @*database_id*= NULL est spécifiée, la base de données est omis.    
    
 Pour plus d’informations, consultez [fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).    
    
## <a name="examples"></a>Exemples    
    
### <a name="a-returning-information-for-a-specified-table"></a>A. Retour d'informations sur une table spécifique    
 L'exemple suivant retourne des informations concernant tous les index et toutes les partitions de la table `Person.Address` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] : L'exécution de cette requête nécessite au minimum l'autorisation CONTROL sur la table `Person.Address`.    
    
> [!IMPORTANT]    
>  Lorsque vous utilisez les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] DB_ID et OBJECT_ID pour obtenir la valeur d'un paramètre, vérifiez toujours que l'ID retourné est valide. Si le nom de la base de données ou de l'objet est introuvable, par exemple s'il n'existe pas ou n'est pas correctement orthographié, les deux fonctions retournent la valeur NULL. La fonction **sys.dm_db_index_operational_stats** interprète la valeur NULL comme une valeur générique qui désigne toutes les bases de données ou tous les objets. Comme il peut s'agir d'une opération non intentionnelle, les exemples fournis dans cette section présentent une méthode sûre pour déterminer les ID de base de données et d'objet.    
    
```    
DECLARE @db_id int;    
DECLARE @object_id int;    
SET @db_id = DB_ID(N'AdventureWorks2012');    
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');    
IF @db_id IS NULL     
  BEGIN;    
    PRINT N'Invalid database';    
  END;    
ELSE IF @object_id IS NULL    
  BEGIN;    
    PRINT N'Invalid object';    
  END;    
ELSE    
  BEGIN;    
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);    
  END;    
GO    
    
```    
    
### <a name="b-returning-information-for-all-tables-and-indexes"></a>B. Retour d'informations sur toutes les tables et tous les index    
 L'exemple suivant retourne des informations concernant toutes les tables et tous les index compris dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’exécution de cette requête nécessite l’autorisation VIEW SERVER STATE.    
    
```    
SELECT * FROM sys.dm_db_index_operational_stats( NULL, NULL, NULL, NULL);    
GO    
    
```    
    
## <a name="see-also"></a>Voir aussi    
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
 [Fonctions et vues de gestion dynamique relatives aux index &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)     
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)     
 [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)     
 [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)     
 [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)     
 [Sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)    
    
  

