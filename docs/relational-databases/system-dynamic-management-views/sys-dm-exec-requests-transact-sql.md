---
title: Sys.dm_exec_requests (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_requests_TSQL
- sys.dm_exec_requests
- dm_exec_requests_TSQL
- dm_exec_requests
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_requests dynamic management view
ms.assetid: 4161dc57-f3e7-4492-8972-8cfb77b29643
caps.latest.revision: 67
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1b8e51c1078ebe9b1fd2784a14e8c8e1575eae27
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecrequests-transact-sql"></a>sys.dm_exec_requests (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations sur chaque demande qui s'exécute dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour exécuter du code externe à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, des procédures stockées étendues et des requêtes distribuées), un thread doit s'exécuter en dehors du contrôle du planificateur non préemptif. Pour ce faire, un processus de travail passe en mode préemptif. Les valeurs temporelles retournées par cette vue de gestion dynamique n'incluent pas le temps passé en mode préemptif.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|ID de la session à laquelle cette demande est liée. N'accepte pas la valeur NULL.|  
|request_id|**int**|ID de la demande. Unique dans le contexte de la session. N'accepte pas la valeur NULL.|  
|start_time|**datetime**|Horodateur lors la réception de la demande. N'accepte pas la valeur NULL.|  
|status|**nvarchar(30)**|Statut de la demande. Il peut s'agir de l'un des états suivants :<br /><br /> Arrière-plan<br />Exécution en cours<br />Exécutable<br />En état de veille<br />Suspendu<br /><br /> N'accepte pas la valeur NULL.|  
|commande|**nvarchar(32)**|Identifie le type de commande en cours de traitement. Les types de commandes courants comprennent notamment :<br /><br /> SELECT<br />INSERT<br />UPDATE<br />DELETE<br />BACKUP LOG<br />BACKUP DATABASE<br />DBCC<br />FOR<br /><br /> Le texte de la demande peut être extrait à l'aide de sys.dm_exec_sql_text avec le paramètre sql_handle correspondant pour la demande. Des processus système internes définissent la commande en fonction du type de tâche à exécuter. Il peut s'agir des tâches suivantes :<br /><br /> LOCK MONITOR<br />CHECKPOINTLAZY<br />WRITER<br /><br /> N'accepte pas la valeur NULL.|  
|sql_handle|**varbinary(64)**|Table de hachage du texte SQL de la demande. Autorise la valeur NULL.|  
|statement_start_offset|**int**|Nombre de caractères dans le traitement ou la procédure stockée en cours d'exécution où les instructions en cours d'exécution commencent. Cette valeur peut être utilisée avec les fonctions de gestion dynamique sql_handle, statement_end_offset et sys.dm_exec_sql_text pour extraire l'instruction en cours d'exécution de la demande. Autorise la valeur NULL.|  
|statement_end_offset|**int**|Nombre de caractères dans le traitement ou la procédure stockée en cours d'exécution où les instructions en cours d'exécution se terminent. Cette valeur peut être utilisée avec les fonctions de gestion dynamique sql_handle, statement_end_offset et sys.dm_exec_sql_text pour extraire l'instruction en cours d'exécution de la demande. Autorise la valeur NULL.|  
|plan_handle|**varbinary(64)**|Table de hachage du plan pour l'exécution SQL. Autorise la valeur NULL.|  
|database_id|**smallint**|ID de la base de données dans laquelle la requête s'exécute. N'accepte pas la valeur NULL.|  
|user_id|**int**|ID de l'utilisateur qui a envoyé la demande. N'accepte pas la valeur NULL.|  
|connection_id|**uniqueidentifier**|ID de la connexion à laquelle la demande est parvenue. Autorise la valeur NULL.|  
|blocking_session_id|**smallint**|ID de la session qui bloque la demande. Si cette colonne est NULL, la demande n'est pas bloquée, ou les informations de session de la session bloquant la demande ne sont pas disponibles (ou ne peuvent pas être identifiées).<br /><br /> -2 = La ressource qui bloque la demande appartient à une transaction distribuée orpheline.<br /><br /> -3 = La ressource qui bloque la demande appartient à une transaction de récupération différée.<br /><br /> -4 = L'ID de session du propriétaire du verrou qui bloque la demande n'a pas pu être déterminé pour le moment en raison de transitions d'état de verrou interne.|  
|wait_type|**nvarchar(60)**|Si la demande est actuellement bloquée, cette colonne retourne le type d'attente. Autorise la valeur NULL.<br /><br /> Pour plus d’informations sur les types d’attentes, consultez [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|wait_time|**int**|Si la demande est actuellement bloquée, cette colonne retourne la durée de l'attente, en millisecondes. N'accepte pas la valeur NULL.|  
|last_wait_type|**nvarchar(60)**|Si la demande a été bloquée précédemment, cette colonne indique le type de la dernière attente. N'accepte pas la valeur NULL.|  
|wait_resource|**nvarchar (256)**|Si la demande est actuellement bloquée, cette colonne retourne la ressource attendue par la demande. N'accepte pas la valeur NULL.|  
|open_transaction_count|**int**|Nombre de transactions ouvertes pour cette demande. N'accepte pas la valeur NULL.|  
|open_resultset_count|**int**|Nombre de jeux de résultats ouverts pour cette demande. N'accepte pas la valeur NULL.|  
|transaction_id|**bigint**|ID de la transaction dans laquelle cette demande s'exécute. N'accepte pas la valeur NULL.|  
|context_info|**varbinary(128)**|Valeur CONTEXT_INFO de la session. Autorise la valeur NULL.|  
|percent_complete|**real**|Le pourcentage de travail terminé pour les commandes suivantes :<br /><br /> ALTER INDEX REORGANIZE<br />Option AUTO_SHRINK avec ALTER DATABASE<br />BACKUP DATABASE<br />DBCC CHECKDB<br />DBCC CHECKFILEGROUP<br />DBCC CHECKTABLE<br />DBCC INDEXDEFRAG<br />DBCC SHRINKDATABASE<br />DBCC SHRINKFILE<br />RECOVERY<br />RESTORE DATABASE<br />ROLLBACK<br />TDE ENCRYPTION<br /><br /> N'accepte pas la valeur NULL.|  
|estimated_completion_time|**bigint**|Interne uniquement. N'accepte pas la valeur NULL.|  
|cpu_time|**int**|Quantité de temps UC (en millisecondes) utilisée par la demande. N'accepte pas la valeur NULL.|  
|total_elapsed_time|**int**|Temps total écoulé en millisecondes depuis l'arrivée de la demande. N'accepte pas la valeur NULL.|  
|scheduler_id|**int**|ID du planificateur qui planifie cette demande. N'accepte pas la valeur NULL.|  
|task_address|**varbinary(8)**|Adresse mémoire de la tâche associée à la demande. Autorise la valeur NULL.|  
|reads|**bigint**|Nombre de lectures effectuées par la demande. N'accepte pas la valeur NULL.|  
|writes|**bigint**|Nombre d'écritures effectuées par la demande. N'accepte pas la valeur NULL.|  
|logical_reads|**bigint**|Nombre de lectures logiques effectuées par la demande. N'accepte pas la valeur NULL.|  
|text_size|**int**|Valeur du paramètre TEXTSIZE pour la demande. N'accepte pas la valeur NULL.|  
|langue|**nvarchar(128)**|Valeur du paramètre de langue pour la demande. Autorise la valeur NULL.|  
|date_format|**nvarchar(3)**|Valeur du paramètre DATEFORMAT pour la demande. Autorise la valeur NULL.|  
|date_first|**smallint**|Valeur du paramètre DATEFIRST pour la demande. N'accepte pas la valeur NULL.|  
|quoted_identifier|**bit**|1 = QUOTED_IDENTIFIER est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|arithabort|**bit**|1 = ARITHABORT est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_null_dflt_on|**bit**|1 = ANSI_NULL_DFLT_ON est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_defaults|**bit**|1 = ANSI_DEFAULTS est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_warnings|**bit**|1 = ANSI_WARNINGS est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_padding|**bit**|1 = ANSI_PADDING est activé (ON) pour la demande.<br /><br /> Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_nulls|**bit**|1 = ANSI_NULLS est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|concat_null_yields_null|**bit**|1 = CONCAT_NULL_YIELDS_NULL est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|transaction_isolation_level|**smallint**|Niveau d'isolation avec lequel la transaction pour cette demande est créée. N'accepte pas la valeur NULL.<br /><br /> 0 = Non spécifié<br /><br /> 1 = Lecture non validée<br /><br /> 2 = Lecture validée<br /><br /> 3 = Répétable<br /><br /> 4 = Sérialisable<br /><br /> 5 = Instantané|  
|lock_timeout|**int**|Délai d'attente de verrou externe pour la demande, en millisecondes. N'accepte pas la valeur NULL.|  
|deadlock_priority|**int**|Paramètre DEADLOCK_PRIORITY de la demande. N'accepte pas la valeur NULL.|  
|row_count|**bigint**|Nombre de lignes retournées au client par cette demande. N'accepte pas la valeur NULL.|  
|prev_error|**int**|Dernière erreur générée pendant l'exécution de la demande. N'accepte pas la valeur NULL.|  
|nest_level|**int**|Niveau d'imbrication actuel du code en cours d'exécution sur la demande. N'accepte pas la valeur NULL.|  
|granted_query_memory|**int**|Nombre de pages allouées à l'exécution d'une requête dans la demande. N'accepte pas la valeur NULL.|  
|executing_managed_code|**bit**|Indique si une demande spécifique est en train d'exécuter des objets CLR (Common Language Runtime) tels que des routines, des types et des déclencheurs. Cette valeur est définie pour toute la durée pendant laquelle un objet CLR réside dans la pile, même lorsqu'elle exécute [!INCLUDE[tsql](../../includes/tsql-md.md)] à partir du CLR. N'accepte pas la valeur NULL.|  
|group_id|**int**|ID du groupe de charge de travail auquel cette requête appartient. N'accepte pas la valeur NULL.|  
|query_hash|**binary(8)**|La valeur de hachage binaire calculée sur la requête et utilisée pour identifier des requêtes avec une logique similaire. Vous pouvez utiliser le hachage de requête pour déterminer l'utilisation des ressources globale pour les requêtes qui diffèrent uniquement par les valeurs littérales.|  
|query_plan_hash|**binary(8)**|Valeur de hachage binaire calculée sur le plan d'exécution de requête et utilisée pour identifier des plans d'exécution de requête semblables. Vous pouvez utiliser le hachage de plan de requête pour rechercher le coût cumulatif de requêtes avec les plans d'exécution semblables.|  
|statement_sql_handle|**varbinary(64)**|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Descripteur SQL de la requête individuelle. |  
|statement_context_id|**bigint**|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> La clé étrangère facultatif sys.query_context_settings. |  
|dop |**int** |**S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Le degré de parallélisme de la requête. |  
|parallel_worker_count |**int** |**S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Le nombre de travailleurs parallèles réservés, s’il s’agit d’une requête parallèle.  |  
|external_script_request_id |**uniqueidentifier** |**S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> L’ID de demande de script externe associé à la demande en cours. |  
|is_resumable |**bit** |**S'applique à**: [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Indique si la demande est une opération d’index peut être repris. |  
  
## <a name="permissions"></a>Autorisations  
 Si l’utilisateur a `VIEW SERVER STATE` autorisation sur le serveur, l’utilisateur voit en cours d’exécution toutes les sessions sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; sinon, l’utilisateur voit uniquement la session active. `VIEW SERVER STATE` ne peut pas être accordées dans [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] donc `sys.dm_exec_requests` est toujours limitée à la connexion actuelle. 
  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-the-query-text-for-a-running-batch"></a>A. Recherche du texte de la requête pour un lot en cours d'exécution  
 L'exemple suivant interroge `sys.dm_exec_requests` afin de rechercher la requête qui vous intéresse et de copier son `sql_handle` de la sortie.  
  
```  
SELECT * FROM sys.dm_exec_requests;  
GO  
```  
  
 Puis, pour obtenir le texte d'instruction, utilisez le `sql_handle` copié avec la fonction système `sys.dm_exec_sql_text(sql_handle)`.  
  
```  
SELECT * FROM sys.dm_exec_sql_text(< copied sql_handle >);  
GO  
```  
  
### <a name="b-finding-all-locks-that-a-running-batch-is-holding"></a>B. Recherche de tous les verrous maintenus par un lot en cours d'exécution  
 L’exemple suivant interroge **sys.dm_exec_requests** pour rechercher le lot vous intéresse et copier son `transaction_id` à partir de la sortie.  
  
```  
SELECT * FROM sys.dm_exec_requests;  
GO  
```  
  
 Ensuite, pour rechercher des informations sur les verrous, utilisez copié `transaction_id` avec la fonction système **sys.dm_tran_locks**.  
  
```  
SELECT * FROM sys.dm_tran_locks   
WHERE request_owner_type = N'TRANSACTION'   
    AND request_owner_id = < copied transaction_id >;  
GO  
```  
  
### <a name="c-finding-all-currently-blocked-requests"></a>C. Recherche de toutes les demandes bloquées actuellement  
 L’exemple suivant interroge **sys.dm_exec_requests** pour trouver des informations sur les demandes bloquées.  
  
```  
SELECT session_id ,status ,blocking_session_id  
    ,wait_type ,wait_time ,wait_resource   
    ,transaction_id   
FROM sys.dm_exec_requests   
WHERE status = N'suspended';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
