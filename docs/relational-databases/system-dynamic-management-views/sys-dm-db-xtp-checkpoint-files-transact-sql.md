---
title: Sys.dm_db_xtp_checkpoint_files (Transact-SQL) | Documents Microsoft
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- database-engine-imoltp
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_files
- sys.dm_db_xtp_checkpoint_files_TSQL
- dm_db_xtp_checkpoint_files_TSQL
- sys.dm_db_xtp_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_files dynamic management view
ms.assetid: ac8e6333-7a9f-478a-b446-5602283e81c9
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c4ad13459024604d748c1dac8a6649c09a53f10f
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbxtpcheckpointfiles-transact-sql"></a>sys.dm_db_xtp_checkpoint_files (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

  Affiche des informations sur les fichiers de point de contrôle, y compris la taille de fichier, l'emplacement physique et l'ID de la transaction.  
  
> **Remarque :** pour le point de contrôle actuel n’est pas fermée, la colonne d’état de s`ys.dm_db_xtp_checkpoint_files` sera UNDER CONSTRUCTION pour les nouveaux fichiers. Un point de contrôle se ferme automatiquement lorsqu’il existe suffisamment croissance du journal des transactions depuis le dernier point de contrôle, ou si vous exécutez le `CHECKPOINT` commande ([point de contrôle &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)).  
  
 Un groupe de fichiers mémoire optimisé utilise en interne des fichiers en mode append-only pour stocker les lignes insérées et supprimées pour les tables en mémoire. Il existe deux types de fichiers : Un fichier de données contient les lignes insérées un fichier delta contient des références à des lignes supprimées. 
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] diffère sensiblement de versions plus récentes et est inférieur de la rubrique à [SQL Server 2014](#bkmk_2014).  
  
 Pour plus d’informations, consultez [création et gestion du stockage des objets mémoire optimisés](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
##  <a name="bkmk_2016"></a> [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures  
 Le tableau suivant décrit les colonnes de `sys.dm_db_xtp_checkpoint_files`, en commençant par **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]**.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|container_id|**int**|ID du conteneur (représenté en tant que fichier de type FILESTREAM dans sys.database_files) dont les données ou le fichier delta font partie. Jointures avec file_id dans [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID du conteneur, le fichier racine, de données ou delta fait partie. Jointures avec file_guid dans la table sys.database_files.|  
|checkpoint_file_id|**uniqueidentifier**|GUID du fichier de point de contrôle.|  
|relative_file_path|**nvarchar (256)**|Chemin d’accès du fichier relatif au conteneur, à qu'il est mappé.|  
|file_type|**smallint**|-1 pour gratuit<br /><br /> 0 pour le fichier de données.<br /><br /> 1 pour le fichier DELTA.<br /><br /> 2 pour le fichier racine<br /><br /> 3 pour le fichier de données volumineux|  
|file_type_desc|**nvarchar(60)**|Fichiers de libre - All conservées comme étant libres sont disponibles pour l’allocation. Fichiers libres peuvent varier en taille en fonction des besoins anticipés par le système. La taille maximale est de 1 Go.<br /><br /> DONNÉES - fichiers de données contiennent des lignes qui ont été insérées dans des tables optimisées en mémoire.<br /><br /> DELTA - les fichiers Delta contiennent des références aux lignes dans les fichiers de données qui ont été supprimés.<br /><br /> RACINE - fichiers racine contient les métadonnées système pour les objets mémoire optimisés et compilées en mode natif.<br /><br /> DONNÉES volumineuses - gros fichiers de données contiennent des valeurs insérées dans (colonnes varchar et varbinary (max), ainsi que les segments de colonne qui font partie de l’index columnstore sur des tables optimisées en mémoire.|  
|internal_storage_slot|**int**|Index du fichier dans le tableau de stockage interne. NULL pour la racine ou pour un état autre que 1.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Les données correspondantes ou le fichier DELTA. NULL pour la racine.|  
|file_size_in_bytes|**bigint**|Taille du fichier sur le disque.|  
|file_size_used_in_bytes|**bigint**|Pour les paires de fichiers de point de contrôle dont le remplissage est toujours en cours, cette colonne est mise à jour après le point de contrôle suivant.|  
|logical_row_count|**bigint**|Pour les données, le nombre de lignes insérées.<br /><br /> Delta, le nombre de lignes supprimés après prise en compte pour supprimer une table.<br /><br /> Pour la racine, la valeur NULL.|  
|state|**smallint**|0 – PRECREATED<br /><br /> 1 - EN COURS D’ÉLABORATION<br /><br /> 2 - ACTIVE<br /><br /> 3 – MERGE TARGET<br /><br /> 8 – EN ATTENTE DE TRONCATION DU JOURNAL|  
|state_desc|**nvarchar(60)**|PRECREATED – un nombre de fichiers de point de contrôle est alloué au préalable pour réduire ou éliminer les attentes d’allocation de nouveaux fichiers lorsque des transactions sont en cours d’exécution. Ces précréés fichiers peuvent varier en taille, en fonction des besoins estimées de la charge de travail, mais ils contiennent pas de données. Il s’agit d’une surcharge de stockage de bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.<br /><br /> En cours d’élaboration - ces fichiers de point de contrôle sont en cours d’élaboration, c'est-à-dire qu’ils sont remplis basés sur les enregistrements de journal générés par la base de données et ne sont pas encore partie d’un point de contrôle.<br /><br /> ACTIVE - contient les lignes insérées/supprimées à partir de points de contrôle fermés précédents. Elles contiennent le contenu des tables zone lues en mémoire avant d’appliquer la partie active du journal des transactions au redémarrage de la base de données. Nous pensons que cette taille de ces fichiers de point de contrôle pour être d’environ 2 x de la taille en mémoire des tables optimisées en mémoire, en supposant que l’opération de fusion reste au niveau de la charge de travail transactionnelle.<br /><br /> MERGE TARGET – la cible des opérations de fusion - ces fichiers de point de contrôle stockent les lignes de données consolidées à partir des fichiers source qui ont été identifiées par la stratégie de fusion. Une fois que la fusion est installée, l'état MERGE TARGET passe à ACTIVE.<br /><br /> ATTENTE de la troncation du journal – une fois que la fusion a été installée et que la CFP de cible de fusion fait partie du point de contrôle durable, la transition de fichiers de point de contrôle de source fusion dans cet état. Dans cet état, les fichiers sont nécessaires pour le fonctionnement de la base de données avec une table mémoire optimisée.  Par exemple, pour la récupération à partir d'un point de contrôle durable afin de remonter dans le temps.|  
|lower_bound_tsn|**bigint**|Limite inférieure de la transaction dans le fichier. null si l’état ne figurant pas dans (1, 3).|  
|upper_bound_tsn|**bigint**|Limite supérieure de la transaction dans le fichier. null si l’état ne figurant pas dans (1, 3).|  
|begin_checkpoint_id|**bigint**|ID du point de contrôle de début.|  
|end_checkpoint_id|**bigint**|ID du point de contrôle final.|  
|last_updated_checkpoint_id|**bigint**|ID du dernier point de contrôle dans ce fichier de mise à jour.|  
|encryption_status|**smallint**|0, 1, 2|  
|encryption_status_desc|**nvarchar(60)**|0 = &GT; UNENCRTPTED<br /><br /> 1 = &GT; CHIFFRÉE AVEC LA CLÉ 1<br /><br /> 2 = &GT; CHIFFRÉE AVEC LA CLÉ 2. Valide uniquement pour les fichiers actifs.|  
  
##  <a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 Le tableau suivant décrit les colonnes de `sys.dm_db_xtp_checkpoint_files`, pour **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|container_id|**int**|ID du conteneur (représenté en tant que fichier de type FILESTREAM dans sys.database_files) dont les données ou le fichier delta font partie. Jointures avec file_id dans [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID du conteneur dont les données ou le fichier delta font partie.|  
|checkpoint_file_id|**GUID**|ID du fichier de données ou delta.|  
|relative_file_path|**nvarchar (256)**|Chemin d'accès au fichier de données ou delta, relatif à l'emplacement du conteneur.|  
|file_type|**tinyint**|0 pour le fichier de données.<br /><br /> 1 pour le fichier delta.<br /><br /> NULL si la colonne d'état a la valeur 7.|  
|file_type_desc|**nvarchar(60)**|Le type de fichier : DATA_FILE, DELTA_FILE ou NULL si la colonne d’état est définie à 7.|  
|internal_storage_slot|**int**|Index du fichier dans le tableau de stockage interne. NULL si la colonne d'état n'a pas la valeur 2 ni 3.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Fichier de données ou delta correspondant.|  
|file_size_in_bytes|**bigint**|Taille du fichier utilisée. NULL si la colonne d'état a la valeur 5, 6, ou 7.|  
|file_size_used_in_bytes|**bigint**|Taille utilisée du fichier. NULL si la colonne d'état a la valeur 5, 6, ou 7.<br /><br /> Pour les paires de fichiers de point de contrôle dont le remplissage est toujours en cours, cette colonne est mise à jour après le point de contrôle suivant.|  
|inserted_row_count|**bigint**|Nombre de lignes dans le fichier de données.|  
|deleted_row_count|**bigint**|Nombre de lignes supprimées dans le fichier delta.|  
|drop_table_deleted_row_count|**bigint**|Nombre de lignes dans les fichiers de données affectées par une table de suppression. S'applique aux fichiers de données lorsque la colonne d'état est égale à 1.<br /><br /> Affiche les lignes supprimées des tables supprimées. Les statistiques drop_table_deleted_row_count sont compilées après que l'opération de garbage collection pour libérer la mémoire des lignes des tables supprimées est terminée et un point de contrôle est effectué. Si vous redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant que les statistiques des tables supprimées soient reflétées dans cette colonne, les statistiques seront mises à jour dans le cadre de la récupération. Le processus de récupération ne charge pas de lignes des tables supprimées. Les statistiques des tables supprimées sont compilées pendant la phase de chargement et reportées dans cette colonne lorsque la récupération est terminée.|  
|state|**int**|0 – PRECREATED<br /><br /> 1 – UNDER CONSTRUCTION<br /><br /> 2 - ACTIVE<br /><br /> 3 – MERGE TARGET<br /><br /> 4 – MERGED SOURCE<br /><br /> 5 – REQUIRED FOR BACKUP/HA<br /><br /> 6 – IN TRANSITION TO TOMBSTONE<br /><br /> 7 – TOMBSTONE|  
|state_desc|**nvarchar(60)**|PRECREATED – Un petit ensemble de paires de fichiers de données et delta, également appelées « paires de fichiers de point de contrôle » (CFP), est préaffecté pour éliminer ou réduire les attentes d'allocation de nouveaux fichiers lorsque des transactions sont exécutées. Elles ont une taille complète avec un fichier de données de 128 Mo et un fichier delta de 8 Mo, mais ne contenant aucune donnée. Le nombre de paires de fichiers de point de contrôle est calculé en fonction du nombre de processeurs logiques ou de planificateurs (un par cœur, sans limitation) avec un minimum de 8. Il s'agit d'une charge de stockage fixe dans les bases de données avec des tables mémoire optimisées.<br /><br /> UNDER CONSTRUCTION – Ensemble de paires de fichiers de point de contrôle qui stockent les lignes de données nouvellement insérées et éventuellement supprimées depuis le dernier point de contrôle.<br /><br /> ACTIVE - Contient les lignes insérées et les lignes supprimées provenant des points de contrôle fermés précédents. Ces paires contiennent toutes les lignes insérées et supprimées requises avant d'appliquer la partie actuelle du journal des transactions au redémarrage de la base de données. La taille de ces paires de fichiers de point de contrôle sera approximativement 2 fois la taille en mémoire des tables mémoire optimisées, en supposant que l'opération de fusion est actualisée avec la charge de travail transactionnelle.<br /><br /> MERGE TARGET – Les paires de fichiers de point de contrôle stockent des lignes de données consolidées depuis les paires de fichiers de point de contrôle identifiées par la stratégie de fusion. Une fois que la fusion est installée, l'état MERGE TARGET passe à ACTIVE.<br /><br /> MERGED SOURCE – Une fois que l'opération de fusion est installée, les paires de fichiers de point de contrôle source sont marqués comme MERGED SOURCE. Notez que l'évaluateur de la stratégie de fusion peut identifier plusieurs fusions mais une paire de fichiers de point de contrôle ne peut participer qu'à une seule opération de fusion.<br /><br /> REQUIRED FOR BACKUP/HA – Une fois que la fusion a été installée et que la paire de fichiers de point de contrôle MERGE TARGET fait partie d'un point de contrôle durable, les paires de fichiers de point de contrôle de la source de fusion passent à cet état. Des paires de fichiers de point de contrôle ayant cet état sont nécessaires pour le bon fonctionnement de la base de données contenant la table mémoire optimisée.  Par exemple, pour la récupération à partir d'un point de contrôle durable afin de remonter dans le temps. Une paire de fichiers de point de contrôle peut être marquée pour l'opération de garbage collection une fois que le point de troncation de journal dépasse sa plage de transactions.<br /><br /> IN TRANSITION TO TOMBSTONE – Ces paires de fichiers de point de contrôle ne sont pas nécessaires par le moteur en mémoire OLTP et peuvent être récupérées par le garbage collector. Cet état indique que ces paires de fichiers de point de contrôle attendent que le thread d'arrière-plan les passe à l'état suivant, qui est TOMBSTONE.<br /><br /> TOMBSTONE – Ces paires de fichiers de point de contrôle attendent d'être récupérées par le garbage collector du flux de fichier. ([sp_filestream_force_garbage_collection &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md))|  
|lower_bound_tsn|**bigint**|Limite inférieure des transactions contenues dans le fichier. Null si la colonne d'état a une valeur autre que 2, 3 ou 4.|  
|upper_bound_tsn|**bigint**|Limite supérieure des transactions contenues dans le fichier. Null si la colonne d'état a une valeur autre que 2, 3 ou 4.|  
|last_backup_page_count|**int**|Nombre de pages logiques déterminé lors de la dernière sauvegarde. S'applique quand la colonne d'état a la valeur 2, 3, 4 ou 5. NULL si le nombre de pages est inconnu.|  
|delta_watermark_tsn|**int**|Transaction du dernier point de contrôle qui a écrit dans ce fichier delta. Il s'agit de la filigrane du fichier delta.|  
|last_checkpoint_recovery_lsn|**nvarchar(23)**|Numéro séquentiel dans le journal de récupération du dernier point de contrôle qui requiert toujours le fichier.|  
|tombstone_operation_lsn|**nvarchar(23)**|Le fichier sera supprimé une fois que le tombstone_operation_lsn passe derrière le numéro séquentiel de troncation du journal.|  
|logical_deletion_log_block_id|**bigint**|S'applique uniquement à l'état 5.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW DATABASE STATE` sur le serveur.  
  
## <a name="use-cases"></a>Cas d'usage  
 Vous pouvez estimer le stockage utilisé par l’OLTP en mémoire comme suit :  
  
```  
-- total storage used by In-Memory OLTP  
SELECT SUM (file_size_in_bytes)/(1024*1024) as file_size_in_MB  
FROM sys.dm_db_xtp_checkpoint_files  
```  
  
  
Pour afficher une répartition de l’utilisation du stockage par type état et le fichier, exécutez la requête suivante :
  
```  
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(file_size_in_bytes) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  

  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables optimisées en mémoire &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
