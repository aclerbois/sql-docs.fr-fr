---
title: Sys.DM_OS_SCHEDULERS (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_os_schedulers
- sys.dm_os_schedulers_TSQL
- sys.dm_os_schedulers
- dm_os_schedulers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_schedulers dynamic management view
ms.assetid: 3a09d81b-55d5-416f-9cda-1a3a5492abe0
caps.latest.revision: 55
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7ecf29c32131972d2802eae9cf735582252cf145
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosschedulers-transact-sql"></a>sys.dm_os_schedulers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne par planificateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où chaque planificateur est associé à un processeur. Vous pouvez utiliser cette vue pour surveiller la condition d'un planificateur ou pour identifier des tâches d'échappement.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_schedulers**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|scheduler_address|**varbinary(8)**|Adresse mémoire du planificateur. N'accepte pas la valeur NULL.|  
|parent_node_id|**int**|Identificateur du nœud auquel le planificateur appartient. On parle également de nœud parent. Il s'agit d'un nœud NUMA (Nonuniform Memory Access). N'accepte pas la valeur NULL.|  
|scheduler_id|**int**|ID du planificateur. Tous les planificateurs utilisés pour exécuter des requêtes régulières ont des numéros d'identificateur inférieurs à 1 048 576. Les planificateurs qui sont identifiés par un numéro supérieur ou égal à 1 048 576 sont utilisés en interne par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par exemple le planificateur de connexions administrateur dédiées. N'accepte pas la valeur NULL.|  
|cpu_id|**smallint**|ID de l'UC assigné au planificateur.<br /><br /> N'accepte pas la valeur NULL.<br /><br /> **Remarque :** 255 n’indique pas aucune affinité comme il le faisait [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Consultez [sys.dm_os_threads &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md) pour plus d’informations d’affinité supplémentaires.|  
|status|**nvarchar(60)**|Indique l'état du planificateur. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> -MASQUÉ EN LIGNE<br />-MASQUÉ EN MODE HORS CONNEXION<br />-VISIBLE EN LIGNE<br />-VISIBLE EN MODE HORS CONNEXION<br />-EN LIGNE VISIBLE (DAC)<br />-HOT_ADDED<br /><br /> N'accepte pas la valeur NULL.<br /><br /> Planificateurs masqués sont utilisés pour traiter les demandes qui sont internes à la [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Les planificateurs VISIBLE servent à traiter les requêtes des utilisateurs.<br /><br /> Les planificateurs OFFLINE se mappent avec des processeurs qui sont déconnectés dans le masque d'affinité et qui ne sont, par conséquent, pas utilisés pour traiter des requêtes. Les planificateurs ONLINE se mappent avec des processeurs qui sont connectés dans le masque d'affinité et qui sont disponibles pour traiter des threads.<br /><br /> DAC indique que le planificateur s'exécute sous une connexion administrateur dédiée (DAC, Dedicated Administrator Connection).<br /><br /> HOT ADDED indique que les planificateurs ont été ajoutés en réponse à un événement d'ajout d'un processeur à chaud.|  
|is_online|**bit**|Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré pour utiliser uniquement certains des processeurs disponibles sur le serveur, cette configuration peut indiquer que certains planificateurs sont associés à des processeurs non inclus dans le masque d'affinité. Dans ce cas, cette colonne retourne la valeur 0. Cette valeur signifie que le planificateur n'est pas utilisé pour traiter des requêtes ou des lots.<br /><br /> N'accepte pas la valeur NULL.|  
|is_idle|**bit**|1 = Le planificateur est inactif. Aucun processus de travail n'est actuellement en cours d'exécution. N'accepte pas la valeur NULL.|  
|preemptive_switches_count|**int**|Nombre de fois où les processus de travail opérant sur ce planificateur sont passés en mode préemptif.<br /><br /> Pour exécuter du code externe à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, des procédures stockées étendues et des requêtes distribuées), un thread doit s'exécuter en dehors du contrôle du planificateur non préemptif. Pour ce faire, un processus de travail passe en mode préemptif.|  
|context_switches_count|**int**|Nombre de changements de contexte ayant eu lieu sur ce planificateur. N'accepte pas la valeur NULL.<br /><br /> Pour permettre à d'autres processus de travail de s'exécuter, le processus de travail en cours doit abandonner le contrôle du planificateur ou changer de contexte.<br /><br /> **Remarque :** si un processus de travail abandonne le planificateur et les place dans la file d’attente exécutable et puis trouve aucun autre processus, le processus de travail sélectionne lui-même. Dans ce cas, context_switches_count n'est pas mis à jour, mais yield_count l'est.|  
|idle_switches_count|**int**|Nombre de fois où le planificateur a attendu un événement quand il était inactif. Cette colonne est similaire à context_switches_count. N'accepte pas la valeur NULL.|  
|current_tasks_count|**int**|Nombre de tâches actuellement associées au planificateur. Il s'agit des tâches suivantes :<br /><br /> -Les tâches qui sont en attente d’un processus de travail pour les exécuter.<br />-Les tâches qui sont actuellement en attente ou en cours d’exécution (en état SUSPENDED ou RUNNABLE).<br /><br /> Lorsqu'une tâche est terminée, ce nombre est décrémenté. N'accepte pas la valeur NULL.|  
|runnable_tasks_count|**int**|Nombre de processus de travail, auxquels des tâches sont affectées, qui attendent d'être planifiés sur la file d'attente exécutable. N'accepte pas la valeur NULL.|  
|current_workers_count|**int**|Nombre de processus de travail qui sont associés à ce planificateur. Il s'agit des processus de travail qui ne sont affectés à aucune tâche. N'accepte pas la valeur NULL.|  
|active_workers_count|**int**|Nombre de processus de travail actifs. Un processus de travail actif n'est jamais préemptif, doit être associé à une tâche et est soit en cours d'exécution, soit exécutable, soit suspendu. N'accepte pas la valeur NULL.|  
|work_queue_count|**bigint**|Nombre de tâches dans la file d'attente de travail. Ces tâches attendent d'être sélectionnées par un processus de travail. N'accepte pas la valeur NULL.|  
|pending_disk_io_count|**int**|Nombre d'E/S qui sont en attente. Chaque planificateur possède une liste d'E/S en attente, lesquelles sont vérifiées lors de chaque changement de contexte pour déterminer si elles ont été effectuées. Le nombre est incrémenté lorsque la demande est insérée. Le nombre est décrémenté lorsque la demande est terminée. Cette valeur n'indique pas l'état des E/S. N'accepte pas la valeur NULL.|  
|load_factor|**int**|Valeur interne qui indique la charge perçue sur le planificateur. Cette valeur est utilisée pour déterminer si une nouvelle tâche doit être placée sur ce planificateur ou sur un autre planificateur. Cette valeur est utile à des fins de débogage, lorsqu'il apparaît que les planificateurs ne sont pas chargés de façon uniforme. La décision de routage est prise en fonction de la charge placée sur le planificateur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise également un facteur de charge des nœuds et des planificateurs pour déterminer l'emplacement idéal pour l'acquisition des ressources. Lorsqu'une tâche est empilée, le facteur de charge est incrémenté. Lorsqu'une tâche est terminée, le facteur de charge est décrémenté. Les facteurs de charge permettent un meilleur équilibrage de la charge de travail par le système d'exploitation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. N'accepte pas la valeur NULL.|  
|yield_count|**int**|Valeur interne utilisée pour indiquer la progression du travail sur le planificateur. Cette valeur permet à la tâche système interne de déterminer si un processus de travail du planificateur ne transmet pas ses résultats aux autres processus de travail à temps. Elle n'indique pas que le processus de travail ou la tâche est passé à un nouveau processus de travail. N'accepte pas la valeur NULL.|  
|last_timer_activity|**bigint**|Dans les cycles de l'UC, cette valeur indique à quel moment a eu lieu la dernière vérification de la file d'attente du minuteur par le planificateur. N'accepte pas la valeur NULL.|  
|failed_to_create_worker|**bit**|Cette valeur est définie à 1 s'il a été impossible de créer un nouveau processus de travail sur le planificateur. Ce problème est souvent la conséquence de contraintes de mémoire. Autorise la valeur NULL.|  
|active_worker_address|**varbinary(8)**|Adresse mémoire du processus de travail actuellement actif. Autorise la valeur NULL. Pour plus d’informations, consultez [sys.dm_os_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|memory_object_address|**varbinary(8)**|Adresse mémoire de l'objet mémoire du planificateur. Cette colonne n'accepte pas la valeur NULL.|  
|task_memory_object_address|**varbinary(8)**|Adresse mémoire de l'objet mémoire de la tâche. N'accepte pas la valeur NULL. Pour plus d’informations, consultez [sys.dm_os_memory_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|quantum_length_us|**bigint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Expose le quantum de planificateur utilisé par SQLOS.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="examples"></a>Exemples  
  
### <a name="a-monitoring-hidden-and-nonhidden-schedulers"></a>A. Analyse des planificateurs masqués et non masqués  
 La requête suivante produit l'état des processus de travail et des tâches dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur tous les planificateurs. Cette requête a été exécutée sur un système informatique présentant la configuration suivante :  
  
-   Deux processeurs (UC)  
  
-   Deux nœuds (NUMA)  
  
-   Une UC par nœud NUMA  
  
-   Masque d'affinité défini à `0x03`.  
  
```  
SELECT  
    scheduler_id,  
    cpu_id,  
    parent_node_id,  
    current_tasks_count,  
    runnable_tasks_count,  
    current_workers_count,  
    active_workers_count,  
    work_queue_count  
  FROM sys.dm_os_schedulers;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
scheduler_id cpu_id parent_node_id current_tasks_count  
------------ ------ -------------- -------------------  
0            1      0              9                    
257          255    0              1                    
1            0      1              10                   
258          255    1              1                    
255          255    32             2                    
  
runnable_tasks_count current_workers_count  
-------------------- ---------------------  
0                    11                     
0                    1                      
0                    18                     
0                    1                      
0                    3                      
  
active_workers_count work_queue_count  
-------------------- --------------------  
6                    0  
1                    0  
8                    0  
1                    0  
1                    0  
```  
  
 Le résultat de la requête fournit les informations suivantes :  
  
-   Les planificateurs sont au nombre de cinq. Deux planificateurs possèdent une valeur d'ID inférieure à 1 048 576. Les planificateurs possédant une valeur d'ID supérieure ou égale à 1 048 576 sont qualifiés de planificateurs masqués. Le planificateur `255` représente la connexion administrateur dédiée (DAC). Il existe un planificateur DAC par instance. Les moniteurs de ressources qui coordonnent la sollicitation de la mémoire utilisent le planificateur `257` et le planificateur `258`, un par nœud NUMA.  
  
-   Le résultat présente 23 tâches actives. Ces tâches incluent les demandes utilisateur qui ont été démarrées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en plus des tâches de gestion des ressources. RESOURCE MONITOR (une par nœud NUMA), LAZY WRITER (une par nœud NUMA), LOCK MONITOR, CHECKPOINT et LOG WRITER sont des exemples de tâches [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Le nœud NUMA `0` est mappé à l'UC `1`, le nœud NUMA `1` à l'UC `0`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre généralement sur un nœud NUMA autre que le nœud 0.  
  
-   Lorsque `runnable_tasks_count` retourne `0`, aucune tâche n'est activement exécutée. Il peut cependant exister des sessions actives.  
  
-   Le planificateur `255` représentant la connexion administrateur dédiée (DAC) est associé à `3` processus de travail. Ces derniers sont affectés au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et ne changent pas. Ils sont utilisés pour traiter les requêtes à l'aide de la connexion administrateur dédiée uniquement. Les deux tâches sur ce planificateur représentent un gestionnaire de connexions et un processus de travail inactif.  
  
-   `active_workers_count` représente tous les traitements qui sont associés à des tâches et en cours d’exécution en mode non préemptif. Certaines tâches, comme les écouteurs de réseau, s'exécutent en mode de planification préemptive.  
  
-   Les planificateurs masqués ne traitent pas les demandes utilisateur standard. Le planificateur DAC constitue l'exception. Ce planificateur DAC possède un thread pour traiter les demandes.  
  
### <a name="b-monitoring-nonhidden-schedulers-in-a-busy-system"></a>B. Analyse des planificateurs non masqués dans un système occupé  
 La requête suivante indique l'état des planificateurs non masqués particulièrement chargés, lorsque la quantité de requêtes existante est supérieure à la quantité pouvant être gérée par les processus de travail disponibles. Dans cet exemple, 256 processus de travail sont affectés à des tâches. Certaines tâches sont en attente d'une affectation à un processus de travail. Un nombre exécutable faible implique que plusieurs tâches attendent une ressource.  
  
> [!NOTE]  
>  Vous pouvez interroger sys.dm_os_workers pour trouver l'état des processus de travail. Pour plus d’informations, consultez [sys.dm_os_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).  
  
 Voici la requête :  
  
```  
SELECT  
    scheduler_id,  
    cpu_id,  
    current_tasks_count,  
    runnable_tasks_count,  
    current_workers_count,  
    active_workers_count,  
    work_queue_count  
  FROM sys.dm_os_schedulers  
  WHERE scheduler_id < 255;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
scheduler_id current_tasks_count runnable_tasks_count  
------------ ------------------- --------------------  
0            144                 0                     
1            147                 1                     
  
current_workers_count active_workers_count work_queue_count  
--------------------- -------------------- --------------------  
128                   125                  16  
128                   126                  19  
```  
  
 En comparaison, le résultat suivant affiche plusieurs tâches exécutables où aucune tâche n'attend l'obtention d'un processus de travail. `work_queue_count` a la valeur `0` pour les deux planificateurs.  
  
```  
scheduler_id current_tasks_count runnable_tasks_count  
------------ ------------------- --------------------  
0            107                 98                    
1            110                 100                   
  
current_workers_count active_workers_count work_queue_count  
--------------------- -------------------- --------------------  
128                   104                  0  
128                   108                  0  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


