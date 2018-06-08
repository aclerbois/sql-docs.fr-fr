---
title: KILL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- KILL_TSQL
- KILL
dev_langs:
- TSQL
helpviewer_keywords:
- WITH STATUSONLY option
- terminating distributed transactions
- distributed transactions [SQL Server], terminating
- in-doubt transactions
- IDs [SQL Server], user processes
- ending distributed transactions [SQL Server]
- stopping distributed transactions
- session IDs [SQL Server]
- system process termination [SQL Server]
- stopping processes
- ending processes [SQL Server]
- UOW [SQL Server]
- orphaned transactions [SQL Server]
- unit of work [SQL Server]
- process termination [SQL Server]
- KILL statement
- terminating process
ms.assetid: 071cf260-c794-4b45-adc0-0e64097938c0
caps.latest.revision: 61
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d2cf23ffc6cceaf92f80eb75dbe9da1179e9e4a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="kill-transact-sql"></a>KILL (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Arrête un processus utilisateur basé sur l'ID de session ou sur l'unité de travail (UOW). Si l’ID de session ou l’UOW spécifié implique l’annulation de nombreuses tâches, l’instruction KILL peut mettre du temps s’exécuter, notamment en cas de restauration d’une longue transaction.  
  
 KILL peut être utilisé pour mettre fin à une connexion normale, ce qui met fin en interne aux transactions associées à l'ID de session spécifié. L'instruction peut également servir à mettre fin à des transactions distribuées orphelines et incertaines lorsque le Coordinateur de transactions distribuées [!INCLUDE[msCoName](../../includes/msconame-md.md)] est utilisé.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server  
  
KILL { session ID | UOW } [ WITH STATUSONLY ]   
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
KILL 'session_id'  
[;]   
```  
  
## <a name="arguments"></a>Arguments  
 *session ID*  
 ID de session du processus à interrompre. *session ID* est un entier unique (**int**) affecté à chaque connexion utilisateur lors de l’établissement de la connexion. La valeur de l'ID de session est liée à la connexion pendant la durée de la connexion. Lorsque la connexion se termine, la valeur entière est libérée et peut être réaffectée à une nouvelle connexion.  
La requête suivante peut vous aider à identifier quel `session_id` tuer :  
 ```sql  
 SELECT conn.session_id, host_name, program_name,
     nt_domain, login_name, connect_time, last_request_end_time 
FROM sys.dm_exec_sessions AS sess
JOIN sys.dm_exec_connections AS conn
    ON sess.session_id = conn.session_id;
```  
  
*UOW*  
**S’applique à** : ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
 Identifie l'ID de l'UOW (Unit of Work) des transactions distribuées. *UOW* est un GUID qui peut être obtenu à partir de la colonne request_owner_guid de la vue de gestion dynamique sys.dm_tran_locks. *UOW* peut également être obtenu à partir du journal des erreurs ou par le biais du moniteur MS DTC. Pour plus d'informations sur la surveillance des transactions distribuées, reportez-vous à la documentation de MS DTC.  
  
 Utilisez KILL *UOW* pour arrêter les transactions distribuées orphelines. Ces transactions ne sont associées à aucun ID de session réel mais artificiellement associées à l'ID de session = « -2 ». Cet ID de session facilite l'identification des transactions orphelines par l'interrogation de la colonne d'ID de session dans les vues de gestion dynamique sys.dm_tran_locks, sys.dm_exec_sessions ou sys.dm_exec_requests.  
  
 WITH STATUSONLY  
 Génère un rapport de progression sur un *ID de session* ou *UOW* spécifié qui est restauré en raison d’une instruction KILL préalable. KILL WITH STATUSONLY ne termine pas ou ne restaure pas *session ID* ou *UOW* ; la commande affiche uniquement la progression actuelle de la restauration.  
  
## <a name="remarks"></a>Notes   
 KILL est habituellement utilisé pour arrêter un processus qui bloque d'autres processus importants ayant des verrous ou un processus exécutant une requête qui utilise des ressources système nécessaires. Il n'est pas possible de mettre fin aux processus système et aux processus exécutant une procédure stockée étendue.  
  
 Utilisez l’instruction KILL avec précaution, particulièrement lorsque des processus critiques sont en cours d’exécution. Vous ne pouvez pas arrêter votre propre processus. Les autres processus à ne pas tuer sont les suivants :  
  
-   AWAITING COMMAND  
-   CHECKPOINT SLEEP  
-   LAZY WRITER  
-   LOCK MONITOR  
-   SIGNAL HANDLER  
  
Utilisez @@SPID pour afficher la valeur de l’ID de la session en cours.  
  
 Pour obtenir un rapport des valeurs d'ID de session active, vous pouvez interroger la colonne session_id des vues de gestion dynamique sys.dm_tran_locks, sys.dm_exec_sessions et sys.dm_exec_requests. Vous pouvez également visualiser la colonne SPID renvoyée par la procédure stockée système sp_who. Si une restauration est en cours pour un SPID spécifique, la colonne cmd associée à celui-ci dans le jeu de résultats sp_who indique « KILLED/ROLLBACK ».  
  
 Lorsqu'une connexion particulière possède un verrou sur une ressource de base de données et qu'elle bloque le déroulement d'une autre connexion, l'ID de session de la connexion bloquante apparaît dans la colonne `blocking_session_id` de `sys.dm_exec_requests` ou dans la colonne `blk` retournée par `sp_who`.  
  
 La commande KILL permet de résoudre des transactions distribuées incertaines. Ce sont des transactions distribuées non résolues dues à des démarrages non planifiés du serveur de base de données ou du coordinateur MS DTC. Pour plus d’informations sur les transactions incertaines, consultez la section « Validation en deux phases » dans [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="using-with-statusonly"></a>Utilisation de WITH STATUSONLY  
 KILL WITH STATUSONLY ne génère un rapport que si l’ID de session ou l’UOW est en cours de restauration en raison d’une instruction KILL *session ID*|*UOW* antérieure. Le rapport de progression indique la proportion d'annulation réalisée (en pourcentage) et le temps restant estimé (en secondes), sous la forme suivante :  
  
 `Spid|UOW <xxx>: Transaction rollback in progress. Estimated rollback completion: <yy>% Estimated time left: <zz> seconds`  
  
 Si la restauration de l’ID de session ou de l’UOW était achevée au moment de l’exécution de l’instruction KILL *session ID*|*UOW* WITH STATUSONLY, ou qu’aucun ID de session ni aucune UOW n’est en cours de restauration, KILL *session ID*|*UOW* WITH STATUSONLY renvoie l’erreur suivante :  
  
 ```
"Msg 6120, Level 16, State 1, Line 1"  
"Status report cannot be obtained. Rollback operation for Process ID <session ID> is not in progress."
```  
  
 Le même rapport d’état peut être obtenu en répétant la même instruction KILL *session ID*|*UOW* sans utiliser l’option WITH STATUSONLY ; toutefois, ceci n’est pas recommandé. La répétition d’une instruction KILL *session ID* peut terminer un nouveau processus si la restauration a été achevée et si l’ID de session a été réaffecté à une nouvelle tâche avant l’exécution de la nouvelle instruction KILL. Ceci peut être évité en spécifiant WITH STATUSONLY.  
  
## <a name="permissions"></a>Autorisations  
 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :** Nécessite l’autorisation ALTER ANY CONNECTION. ALTER ANY CONNECTION est incluse avec appartenance au rôle de serveur fixe sysadmin ou processadmin.  
  
 **[!INCLUDE[ssSDS](../../includes/sssds-md.md)] :** Nécessite l’autorisation KILL DATABASE CONNECTION. La connexion du principal au niveau du serveur a l’autorisation KILL DATABASE CONNECTION.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-kill-to-terminate-a-session"></a>A. Utilisation de KILL pour arrêter une session  
 L'exemple suivant indique comment arrêter l'ID de session `53`.  
  
```sql  
KILL 53;  
GO  
```  
  
### <a name="b-using-kill-session-id-with-statusonly-to-obtain-a-progress-report"></a>B. Utilisation de l'instruction KILL ID de session WITH STATUSONLY pour obtenir un rapport de progression  
 L'exemple suivant génère un état sur le processus de restauration d'un ID de session spécifique.  
  
```sql  
KILL 54;  
KILL 54 WITH STATUSONLY;  
GO  
  
--This is the progress report.  
spid 54: Transaction rollback in progress. Estimated rollback completion: 80% Estimated time left: 10 seconds.  
```  
  
### <a name="c-using-kill-to-terminate-an-orphaned-distributed-transaction"></a>C. Utilisation de KILL pour arrêter une transaction distribuée orpheline  
 L’exemple suivant indique comment terminer une transaction distribuée orpheline (ID de session = -2) avec un *UOW* de valeur `D5499C66-E398-45CA-BF7E-DC9C194B48CF`.  
  
```sql  
KILL 'D5499C66-E398-45CA-BF7E-DC9C194B48CF';  
```  

  
## <a name="see-also"></a> Voir aussi  
 [KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md)   
 [@@SPID &#40;Transact-SQL&#41;](../../t-sql/functions/spid-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
  
