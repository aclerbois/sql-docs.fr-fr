---
title: Sys.dm_exec_query_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_query_stats_TSQL
- dm_exec_query_stats
- sys.dm_exec_query_stats
- sys.dm_exec_query_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_stats dynamic management view
ms.assetid: eb7b58b8-3508-4114-97c2-d877bcb12964
caps.latest.revision: 64
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f758b902012ecbc3f13921fccd0326ec94fc918a
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecquerystats-transact-sql"></a>sys.dm_exec_query_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne les statistiques de performances agrégées pour les plans de requête mis en cache dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La vue contient une ligne par instruction de requête dans le plan en cache et la durée de vie des lignes est liée au plan lui-même. Lorsqu'un plan est supprimé du cache, les lignes correspondantes sont éliminées de cette vue.  
  
> [!NOTE]
> Une requête initiale de **sys.dm_exec_query_stats** peut produire des résultats inexacts s’il existe une charge de travail en cours d’exécution sur le serveur. Des résultats plus précis peuvent être déterminés en réexécutant la requête.  
  
> [!NOTE]
> Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_exec_query_stats**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**sql_handle**|**varbinary(64)**  |Jeton qui fait référence au traitement ou à la procédure stockée dont fait partie la requête.<br /><br /> **sql_handle**, avec **statement_start_offset** et **statement_end_offset**, peut être utilisé pour extraire le texte SQL de la requête en appelant le **sys.dm_exec_sql_ texte** fonction de gestion dynamique.|  
|**statement_start_offset**|**int**|Indique, en octets, la position de début (à partir de 0) de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant.|  
|**statement_end_offset**|**int**|Indique, en octets, la position de fin (à partir de 0) de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant. Pour les versions antérieures [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], la valeur -1 indique la fin du lot. Les commentaires de fin sont n’incluent plus les.|  
|**plan_generation_num**|**bigint**|Numéro de séquence permettant de distinguer les instances de plans après une recompilation.|  
|**plan_handle**|**varbinary(64)**|Jeton qui fait référence au plan compilé dont fait partie la requête. Cette valeur peut être passée à la [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) fonction de gestion dynamique pour obtenir le plan de requête.<br /><br /> Sa valeur est toujours 0x000 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**creation_time**|**datetime**|Heure de compilation du plan.|  
|**last_execution_time**|**datetime**|Heure de début de la dernière exécution du plan.|  
|**execution_count**|**bigint**|Nombre d'exécutions du plan depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|Temps processeur total, indiqué en microsecondes (mais précis uniquement en millisecondes), utilisé par les exécutions de ce plan depuis sa compilation.<br /><br /> Pour les procédures stockées compilées en mode natif, **total_worker_time** peut être inexact si plusieurs exécutions sont réalisées en moins d’une milliseconde.|  
|**last_worker_time**|**bigint**|Temps processeur, indiqué en microsecondes (mais précis uniquement en millisecondes), utilisé lors de la dernière exécution du plan. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Temps processeur minimum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan en une seule exécution. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Temps processeur maximum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan en une seule exécution. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de ce plan depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées lors de la dernière exécution du plan.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_physical_reads**|**bigint**|Nombre minimal de lectures physiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_physical_reads**|**bigint**|Nombre maximal de lectures physiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_writes**|**bigint**|Nombre total d'écritures logiques effectuées par les exécutions de ce plan depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_writes**|**bigint**|Numéro du nombre de pages du pool de mémoires tampons modifiées lors de la dernière exécution du plan. Si une page est déjà modifiée, aucune écriture n'est comptée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_writes**|**bigint**|Nombre minimal d'écritures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_writes**|**bigint**|Nombre maximal d'écritures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de ce plan depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées lors de la dernière exécution du plan.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_reads**|**bigint**|Nombre minimal de lectures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_reads**|**bigint**|Nombre maximal de lectures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_clr_time**|**bigint**|Temps, indiqué en microsecondes (mais précis uniquement en millisecondes), utilisé dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] objets de common language runtime (CLR) par les exécutions de ce plan depuis sa compilation. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**last_clr_time**|**bigint**|Temps, indiqué en microsecondes (mais précis uniquement en millisecondes) utilisé par l’exécution dans [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] objets CLR pendant la dernière exécution de ce plan. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**min_clr_time**|**bigint**|Temps minimum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan dans les objets CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] en une seule exécution. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**max_clr_time**|**bigint**|Temps maximum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan dans les objets CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] en une seule exécution. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**total_elapsed_time**|**bigint**|Temps total écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour les exécutions de ce plan.|  
|**last_elapsed_time**|**bigint**|Temps écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour la dernière exécution de ce plan.|  
|**min_elapsed_time**|**bigint**|Temps minimum écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour les différentes exécutions de ce plan.|  
|**max_elapsed_time**|**bigint**|Temps maximum écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour les différentes exécutions de ce plan.|  
|**query_hash**|**Binary(8)**|La valeur de hachage binaire calculée sur la requête et utilisée pour identifier des requêtes avec une logique similaire. Vous pouvez utiliser le hachage de requête pour déterminer l'utilisation des ressources globale pour les requêtes qui diffèrent uniquement par les valeurs littérales.|  
|**query_plan_hash**|**binary(8)**|Valeur de hachage binaire calculée sur le plan d'exécution de requête et utilisée pour identifier des plans d'exécution de requête semblables. Vous pouvez utiliser le hachage de plan de requête pour rechercher le coût cumulatif de requêtes avec les plans d'exécution semblables.<br /><br /> Sa valeur est toujours 0x000 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**total_rows**|**bigint**|Nombre total de lignes renvoyées par la requête. Ne peut pas être NULL.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**last_rows**|**bigint**|Nombre de lignes renvoyées par la dernière exécution de la requête. Ne peut pas avoir la valeur null.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**min_rows**|**bigint**|Nombre minimal de lignes jamais retourné par la requête pendant une exécution. Ne peut pas avoir la valeur null.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**max_rows**|**bigint**|Nombre maximal de lignes jamais retourné par la requête pendant une exécution. Ne peut pas avoir la valeur null.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**statement_sql_handle**|**varbinary(64)**|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Rempli avec des valeurs non NULL uniquement si le magasin de requêtes est activé et collecte les statistiques de cette requête particulière.|  
|**statement_context_id**|**bigint**|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Rempli avec des valeurs non NULL uniquement si le magasin de requêtes est activé et collecte les statistiques de cette requête particulière.|  
|**total_dop**|**bigint**|La somme totale de degré de parallélisme ce plan utilisé depuis sa compilation. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**last_dop**|**bigint**|Le degré de parallélisme lors de la dernière exécution de ce plan. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**min_dop**|**bigint**|Le degré de parallélisme minimal ce plan jamais utilisé pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**max_dop**|**bigint**|Le degré maximal de parallélisme ce plan jamais utilisé pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**total_grant_kb**|**bigint**|La quantité totale de mémoire réservée accorder, en Ko, ce plan reçu depuis sa compilation. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**last_grant_kb**|**bigint**|La quantité de mémoire réservée accorder en Ko lors de la dernière exécution de ce plan. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**min_grant_kb**|**bigint**|La quantité minimale de mémoire réservée accorder, en Ko, ce plan jamais reçu au cours d’une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**max_grant_kb**|**bigint**|La quantité maximale de mémoire réservée accorder, en Ko, ce plan jamais reçu au cours d’une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**total_used_grant_kb**|**bigint**|La quantité totale de mémoire réservée allocation, en Ko, ce plan utilisé, car il a été compilé. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**last_used_grant_kb**|**bigint**|La quantité de la quantité de mémoire utilisée en Ko lors de la dernière exécution de ce plan. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**min_used_grant_kb**|**bigint**|Allocation de la quantité minimale de mémoire utilisée en Ko ce plan jamais utilisé pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**max_used_grant_kb**|**bigint**|Allocation de la quantité maximale de mémoire utilisée en Ko ce plan jamais utilisé pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**total_ideal_grant_kb**|**bigint**|La quantité totale de la quantité de mémoire idéale en Ko estimé de ce plan depuis sa compilation. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**last_ideal_grant_kb**|**bigint**|La quantité de mémoire idéale accorder en Ko lors de la dernière exécution de ce plan. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**min_ideal_grant_kb**|**bigint**|Accorder de la quantité minimale de mémoire idéale en Ko, ce plan estimé jamais pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**max_ideal_grant_kb**|**bigint**|Accorder de la quantité maximale de mémoire idéale en Ko, ce plan estimé jamais pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**total_reserved_threads**|**bigint**|La somme totale des réservé parallèle de threads de ce plan déjà utilisé, car il a été compilé. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**last_reserved_threads**|**bigint**|Le nombre de threads parallèles réservés, lors de la dernière exécution de ce plan. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**min_reserved_threads**|**bigint**|Le nombre minimal de réservé parallèle de threads de ce plan jamais utilisé pendant une exécution.  Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**max_reserved_threads**|**bigint**|Le nombre maximal de réservé parallèle de threads de ce plan jamais utilisé pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**total_used_threads**|**bigint**|La somme totale des utilisé threads parallèles de ce plan déjà utilisé, car il a été compilé. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**last_used_threads**|**bigint**|Le nombre de threads en parallèle utilisés lors de la dernière exécution de ce plan. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**min_used_threads**|**bigint**|Le nombre minimal de threads de parallèles utilisés ce plan jamais utilisé pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**max_used_threads**|**bigint**|Le nombre maximal de threads de parallèles utilisés ce plan jamais utilisé pendant une exécution. Il sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**total_columnstore_segment_reads**|**bigint**|La somme totale des segments columnstore lues par la requête. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_reads**|**bigint**|Le nombre de segments de columnstore par la dernière exécution de la requête de lecture. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_reads**|**bigint**|Le nombre minimal de segments columnstore jamais lues par la requête pendant une exécution. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_reads**|**bigint**|Le nombre maximal de segments columnstore jamais lues par la requête pendant une exécution. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**total_columnstore_segment_skips**|**bigint**|La somme totale des segments columnstore ignoré par la requête. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_skips**|**bigint**|Le nombre de segments de columnstore ignoré par la dernière exécution de la requête. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_skips**|**bigint**|Le nombre minimal de segments columnstore jamais ignoré par la requête pendant une exécution. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_skips**|**bigint**|Le nombre maximal de segments columnstore jamais ignoré par la requête pendant une exécution. Ne peut pas avoir la valeur null.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|
|**total_spills**|**bigint**|Le nombre total de pages répandues par l’exécution de cette requête, car il a été compilé.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Le nombre de pages répandues la dernière exécution de la requête.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Le nombre minimal de pages de cette requête ont été répandues lors d’une exécution unique.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Le nombre maximal de pages que cette requête ont été répandues lors d’une exécution unique.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|L’identificateur du nœud qui se trouve sur cette distribution.<br /><br /> **S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]| 

> [!NOTE]
> <sup>1</sup> des procédures stockées compilées en mode natif lors de la collecte de statistiques est activée, des temps de travail est collecté en millisecondes. Si la requête s’exécute en moins d’une milliseconde, la valeur sera 0.  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
   
## <a name="remarks"></a>Notes  
 Les statistiques de la vue sont actualisées lorsqu'une requête est terminée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-the-top-n-queries"></a>A. Recherche des N premières requêtes (TOP N)  
 L'exemple suivant retourne des informations sur les cinq premières requêtes classées d'après le temps processeur moyen. Cet exemple regroupe les requêtes d'après leur hachage de requête afin que les requêtes logiquement équivalentes soient groupées par leur consommation de ressources cumulative.  
  
```sql  
SELECT TOP 5 query_stats.query_hash AS "Query Hash",   
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",  
    MIN(query_stats.statement_text) AS "Statement Text"  
FROM   
    (SELECT QS.*,   
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,  
    ((CASE statement_end_offset   
        WHEN -1 THEN DATALENGTH(ST.text)  
        ELSE QS.statement_end_offset END   
            - QS.statement_start_offset)/2) + 1) AS statement_text  
     FROM sys.dm_exec_query_stats AS QS  
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats  
GROUP BY query_stats.query_hash  
ORDER BY 2 DESC;  
```  
  
### <a name="b-returning-row-count-aggregates-for-a-query"></a>B. Retour des agrégats de nombre de lignes à une requête  
 L'exemple suivant retourne les informations d'agrégation du nombre de lignes (nombre total de lignes, nombre minimal de lignes, nombre maximal de lignes et dernières lignes) pour les requêtes.  
  
```sql  
SELECT qs.execution_count,  
    SUBSTRING(qt.text,qs.statement_start_offset/2 +1,   
                 (CASE WHEN qs.statement_end_offset = -1   
                       THEN LEN(CONVERT(nvarchar(max), qt.text)) * 2   
                       ELSE qs.statement_end_offset end -  
                            qs.statement_start_offset  
                 )/2  
             ) AS query_text,   
     qt.dbid, dbname= DB_NAME (qt.dbid), qt.objectid,   
     qs.total_rows, qs.last_rows, qs.min_rows, qs.max_rows  
FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS qt   
WHERE qt.text like '%SELECT%'   
ORDER BY qs.execution_count DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)    
[sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[Sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)     
[sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)     
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  


