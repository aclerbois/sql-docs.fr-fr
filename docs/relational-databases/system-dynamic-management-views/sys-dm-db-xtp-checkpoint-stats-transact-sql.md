---
title: Sys.dm_db_xtp_checkpoint_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine-imoltp
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_stats
- dm_db_xtp_checkpoint_stats_TSQL
- sys.dm_db_xtp_checkpoint_stats
- sys.dm_db_xtp_checkpoint_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_stats dynamic management view
ms.assetid: 8d0b18ca-db4d-4376-9905-3e4457727c46
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 183970c09d23304553167b20366e0751d5f35207
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbxtpcheckpointstats-transact-sql"></a>sys.dm_db_xtp_checkpoint_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

  Retourne des statistiques sur les opérations de point de contrôle de l'OLTP en mémoire dans la base de données active. Si la base de données n'a pas d'objets OLTP en mémoire, retourne un ensemble de résultats vide.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
```  
SELECT * FROM db.sys.dm_db_xtp_checkpoint_stats;  
```  
  
**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] diffère sensiblement de versions plus récentes et est inférieur de la rubrique à [SQL Server 2014](#bkmk_2014).**
  
## <a name="includesssql15includessssql15-mdmd-and-later"></a>[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures  
 Le tableau suivant décrit les colonnes de `sys.dm_db_xtp_checkpoint_stats`, en commençant par **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]**.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|last_lsn_processed|**bigint**|Dernier NSE détecté par le contrôleur.|  
|end_of_log_lsn|**numeric(38)**|Le LSN de la fin du journal.|  
|bytes_to_end_of_log|**bigint**|Octets non traitées par le contrôleur, correspondant à des octets entre les journal `last_lsn_processed` et `end_of_log_lsn`.|  
|log_consumption_rate|**bigint**|Taux de consommation de journal des transactions par le contrôleur (en Ko/s).|  
|active_scan_time_in_ms|**bigint**|Temps passé par le contrôleur d’analyser activement le journal des transactions.|  
|total_wait_time_in_ms|**bigint**|Temps d’attente cumulé pour le contrôleur lors de l’analyse de ne pas le journal.|  
|waits_for_io|**bigint**|Nombre d’attentes pour le journal des e/s générée par le thread de contrôleur.|  
|io_wait_time_in_ms|**bigint**|Durée cumulative d’attente de journal des e/s par le thread de contrôleur.|  
|waits_for_new_log_count|**bigint**|Nombre d’attentes induite par le thread de contrôleur d’un nouveau journal doit être créé.|  
|new_log_wait_time_in_ms|**bigint**|Durée cumulative d’attente sur un nouveau journal par le thread de contrôleur.|  
|idle_attempts_count|**bigint**|Nombre de fois que le contrôleur est passé à un état inactif.|  
|tx_segments_dispatched|**bigint**|Nombre de segments détecté par le contrôleur et d’expédition les sérialiseurs. Segment est une partie contiguë de journal constituant une unité de sérialisation. Il est actuellement une taille de 1 Mo, mais peut changer dans les futures.|  
|segment_bytes_dispatched|**bigint**|Nombre total d’octets d’octets envoyés par le contrôleur de sérialiseurs, depuis le redémarrage de la base de données.|  
|bytes_serialized|**bigint**|Nombre total d’octets puisque le redémarrage de la base de données.|  
|serializer_user_time_in_ms|**bigint**|Temps passé par les sérialiseurs en mode utilisateur.|  
|serializer_kernel_time_in_ms|**bigint**|Temps passé par les sérialiseurs en mode noyau.|  
|xtp_log_bytes_consumed|**bigint**|Nombre total d’octets du journal utilisé depuis le redémarrage de la base de données.|  
|checkpoints_closed|**bigint**|Nombre de points de contrôle fermés depuis le redémarrage de la base de données.|  
|last_closed_checkpoint_ts|**bigint**|Horodatage du dernier point de contrôle fermé.|  
|hardened_recovery_lsn|**numeric(38)**|Récupération démarre à partir de LSN.|  
|hardened_root_file_guid|**uniqueidentifier**|GUID du fichier racine qui renforcé à la suite du dernier point de contrôle terminé.|  
|hardened_root_file_watermark|**bigint**|**Interne uniquement**. Combien il est valide pour lire le fichier racine jusqu'à (c’est un type pertinente en interne uniquement-appelée BSN).|  
|hardened_truncation_lsn|**numeric(38)**|LSN du point de troncature.|  
|log_bytes_since_last_close|**bigint**|Octets à partir du dernier fermer à la fin actuelle du journal.|  
|time_since_last_close_in_ms|**bigint**|Heure depuis la dernière fermeture du point de contrôle.|  
|current_checkpoint_id|**bigint**|Actuellement des segments ont été affectés à ce point de contrôle. Le système de point de contrôle est un pipeline. Le point de contrôle actuel est celui qui segments à partir du journal sont assignées à. Une fois qu’il a atteint une limite, le point de contrôle est libéré par le contrôleur et une création en cours.|  
|current_checkpoint_segment_count|**bigint**|Nombre de segments dans le point de contrôle actuel.|  
|recovery_lsn_candidate|**bigint**|**En interne uniquement**. Candidat pour être sélectionnées en tant que recoverylsn lorsque current_checkpoint_id se ferme.|  
|outstanding_checkpoint_count|**bigint**|Nombre de points de contrôle dans le pipeline en attente d’être fermé.|  
|closing_checkpoint_id|**bigint**|ID du point de contrôle de fermeture.<br /><br /> Utilisation de sérialiseurs en parallèle, par conséquent, une fois qu’ils ont fini le point de contrôle est un candidat à être fermée par le thread de fermeture. Mais le thread de fermeture peut fermer uniquement à la fois et il doit être dans l’ordre, de sorte que le point de contrôle de fermeture est celui qui travaille sur le thread de fermeture.|  
|recovery_checkpoint_id|**bigint**|ID du point de contrôle à utiliser en mode de récupération.|  
|recovery_checkpoint_ts|**bigint**|Horodatage du point de contrôle de la récupération.|  
|bootstrap_recovery_lsn|**numeric(38)**|LSN de récupération pour le programme d’amorçage.|  
|bootstrap_root_file_guid|**uniqueidentifier**|GUID du fichier racine pour le programme d’amorçage.|  
|internal_error_code|**bigint**|Erreur lors de la vue par le contrôleur, le sérialiseur, fermez, et la fusion threads.|
|bytes_of_large_data_serialized|**bigint**|La quantité de données qui a été sérialisées. |  
  
##  <a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 Le tableau suivant décrit les colonnes de `sys.dm_db_xtp_checkpoint_stats`, pour **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|log_to_process_in_bytes|**bigint**|Nombre d'octets du journal entre le numéro séquentiel dans le journal (LSN) actuel du thread et la fin du journal.|  
|total_log_blocks_processed|**bigint**|Nombre total de blocs de journal traités depuis le démarrage du serveur.|  
|total_log_records_processed|**bigint**|Nombre total d'enregistrements de journal traités depuis le démarrage du serveur.|  
|xtp_log_records_processed|**bigint**|Nombre total d'enregistrements de journal de l'OLTP en mémoire traités depuis le démarrage du serveur.|  
|total_wait_time_in_ms|**bigint**|Durée cumulative d'attente en ms.|  
|waits_for_io|**bigint**|Nombre d'attentes pour les E/S du journal.|  
|io_wait_time_in_ms|**bigint**|Durée cumulative d'attente des E/S du journal.|  
|waits_for_new_log|**bigint**|Nombre d'attentes pour le nouveau journal à générer.|  
|new_log_wait_time_in_ms|**bigint**|Durée cumulative d'attente du nouveau journal.|  
|log_generated_since_last_checkpoint_in_bytes|**bigint**|Nombre d'entrées de journal générées depuis le dernier point de contrôle de l'OLTP en mémoire.|  
|ms_since_last_checkpoint|**bigint**|Durée en millisecondes depuis le dernier point de contrôle de l'OLTP en mémoire.|  
|checkpoint_lsn|**numérique (38)**|Numéro séquentiel dans le journal (LSN) de récupération associé au dernier point de vérification de l'OLTP en mémoire terminé.|  
|current_lsn|**numérique (38)**|Numéro séquentiel dans le journal de l'enregistrement de journal en cours de traitement.|  
|end_of_log_lsn|**numérique (38)**|Numéro séquentiel dans le journal à la fin du journal.|  
|task_address|**varbinary(8)**|Adresse de SOS_Task. Jointure à sys.dm_os_tasks pour rechercher des informations supplémentaires.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW DATABASE STATE` sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables optimisées en mémoire &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
