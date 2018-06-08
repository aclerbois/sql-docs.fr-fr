---
title: Surveillance des performances des procédures stockées compilées en mode natif | Microsoft Docs
ms.custom: ''
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 55548cb2-77a8-4953-8b5a-f2778a4f13cf
caps.latest.revision: 11
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d62096acba1d44cfa15af8ae6028ec2b756a117c
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="monitoring-performance-of-natively-compiled-stored-procedures"></a>Surveillance des performances des procédures stockées compilées en mode natif
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cet article explique comment surveiller les performances des procédures stockées compilées en mode natif et celles d’autres modules T-SQL compilés en mode natif.  
  
## <a name="using-extended-events"></a>Utilisation des événements étendus  
 Utilisez l’événement étendu **sp_statement_completed** pour tracer l’exécution d’une requête. Créez une session d’événements étendus avec cet événement, éventuellement avec un filtre sur object_id pour une procédure stockée compilée en mode natif. L’événement étendu est déclenché après l’exécution de chaque requête. Le temps processeur et la durée signalés par l'événement étendu indiquent l'UC utilisée par la requête et le temps d'exécution. Une procédure stockée compilée en mode natif qui utilise beaucoup de temps processeur peut rencontrer des problèmes de performances.  
  
 Vous pouvez utiliser**line_number**et **object_id** dans l’événement étendu pour analyser la requête. La requête suivante peut être utilisée pour extraire la définition de procédure. Le numéro de ligne peut être utilisé pour identifier la requête dans la définition :  
  
```sql  
select [definition] from sys.sql_modules where object_id=object_id  
```  
  
 Pour plus d’informations sur l’événement étendu **sp_statement_completed** , consultez [Procédure : extraire l’instruction qui a provoqué un événement](http://blogs.msdn.com/b/extended_events/archive/2010/05/07/making-a-statement-how-to-retrieve-the-t-sql-statement-that-caused-an-event.aspx).  
  
## <a name="using-data-management-views-and-query-store"></a>Utilisation des vues de gestion des données et du magasin des requêtes
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)] prennent en charge la collecte de statistiques d’exécution des procédures stockées compilées en mode natif, au niveau de la procédure et au niveau de la requête. Le collecte des statistiques d'exécution n'est pas activée par défaut en raison de l'impact sur les performances.  

Les statistiques d’exécution sont présentées dans les vues système [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) et [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md), ainsi que dans le [magasin des requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

### <a name="enabling-procedure-level-execution-statistics-collection"></a>Activation de la collecte des statistiques d’exécution au niveau de la procédure

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : pour activer ou désactiver la collecte des statistiques sur les procédures stockées compilées en mode natif au niveau de la procédure, utilisez [sys.sp_xtp_control_proc_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).  L’instruction suivante active la collecte des statistiques d’exécution au niveau de la procédure pour tous les modules T-SQL compilés en mode natif sur l’instance actuelle :
```sql
EXEC sys.sp_xtp_control_proc_exec_stats 1
```

**[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]** : pour activer ou désactiver la collecte des statistiques sur les procédures stockées compilées en mode natif au niveau de la procédure, utilisez l’option de [configuration délimitée à la base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) `XTP_PROCEDURE_EXECUTION_STATISTICS`. L’instruction suivante active la collecte des statistiques d’exécution au niveau de la procédure pour tous les modules T-SQL compilés en mode natif dans la base de données actuelle :
```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_PROCEDURE_EXECUTION_STATISTICS = ON
```

### <a name="enabling-query-level-execution-statistics-collection"></a>Activation de la collecte des statistiques d’exécution au niveau de la requête

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : pour activer ou désactiver la collecte des statistiques sur les procédures stockées compilées en mode natif au niveau de la requête, utilisez [sys.sp_xtp_control_query_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).  L’instruction suivante active la collecte des statistiques d’exécution au niveau de la requête pour tous les modules T-SQL compilés en mode natif sur l’instance actuelle :
```sql
EXEC sys.sp_xtp_control_query_exec_stats 1
```

**[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]** : pour activer ou désactiver la collecte des statistiques sur les procédures stockées compilées en mode natif au niveau de l’instruction, utilisez l’option de [configuration délimitée à la base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) `XTP_QUERY_EXECUTION_STATISTICS`. L’instruction suivante active la collecte des statistiques d’exécution au niveau de la requête pour tous les modules T-SQL compilés en mode natif dans la base de données actuelle :
```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_QUERY_EXECUTION_STATISTICS = ON
```

## <a name="sample-queries"></a>Exemples de requêtes

 Une fois recueillies, les statistiques d’exécution des procédures stockées compilées en mode natif peuvent être interrogées pour une procédure à l’aide de [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) et pour les requêtes à l’aide de [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md).  
 
  
 La requête suivante retourne les noms de procédure et les statistiques d'exécution des procédures stockées compilées en mode natif dans la base de données active, après collection de statistiques :  
  
```sql  
select object_id,  
       object_name(object_id) as 'object name',  
       cached_time,  
       last_execution_time,  
       execution_count,  
       total_worker_time,  
       last_worker_time,  
       min_worker_time,  
       max_worker_time,  
       total_elapsed_time,  
       last_elapsed_time,  
       min_elapsed_time,  
       max_elapsed_time   
from sys.dm_exec_procedure_stats  
where database_id=db_id() and object_id in (select object_id   
from sys.sql_modules where uses_native_compilation=1)  
order by total_worker_time desc  
```  
  
 La requête suivante retourne le texte des requêtes ainsi que les statistiques d'exécution de toutes les requêtes dans les procédures stockées compilées en mode natif dans la base de données active pour laquelle les statistiques ont été collectées, triées par temps total de travail, dans l'ordre décroissant :  
  
```sql  
select st.objectid,   
       object_name(st.objectid) as 'object name',   
       SUBSTRING(st.text, (qs.statement_start_offset/2) + 1, ((qs.statement_end_offset-qs.statement_start_offset)/2) + 1) as 'query text',   
       qs.creation_time,  
       qs.last_execution_time,  
       qs.execution_count,  
       qs.total_worker_time,  
       qs.last_worker_time,  
       qs.min_worker_time,  
       qs.max_worker_time,  
       qs.total_elapsed_time,  
       qs.last_elapsed_time,  
       qs.min_elapsed_time,  
       qs.max_elapsed_time  
from sys.dm_exec_query_stats qs cross apply sys.dm_exec_sql_text(sql_handle) st  
where  st.dbid=db_id() and st.objectid in (select object_id   
from sys.sql_modules where uses_native_compilation=1)  
order by qs.total_worker_time desc  
```  

## <a name="query-execution-plans"></a>Plans d’exécution de requête

 Les procédures stockées compilées en mode natif prennent en charge SHOWPLAN_XML (plan d'exécution de requêtes). Le plan d'exécution estimé permet d'inspecter le plan de requête, afin de trouver des problèmes de plan erroné. Causes courantes de plans incorrects :  
  
-   Les statistiques n'ont pas été mises à jour avant de créer la procédure.  
  
-   Index manquants  
  
 Le XML du plan d'exécution de requêtes est obtenu en exécutant l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]suivante :  
  
```sql  
SET SHOWPLAN_XML ON  
GO  
EXEC my_proc   
GO  
SET SHOWPLAN_XML OFF  
GO  
```  
  
 Ou, dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez le nom de la procédure et cliquez sur **Afficher le plan d'exécution estimé**.  
  
 Le plan d'exécution estimé pour les procédures stockées compilées en mode natif affiche les opérateurs de requête et les expressions des requêtes dans la procédure. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ne prend pas en charge tous les attributs SHOWPLAN_XML des procédures stockées compilées en mode natif. Par exemple, les attributs liés au coût de l'optimiseur de requête ne font pas partie de SHOWPLAN_XML pour la procédure.  
  
## <a name="see-also"></a> Voir aussi  
 [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)  
  
  
