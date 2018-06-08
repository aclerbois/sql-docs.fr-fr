---
title: Sys.dm_os_wait_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 04/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_os_wait_stats_TSQL
- dm_os_wait_stats
- sys.dm_os_wait_stats
- sys.dm_os_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_wait_stats dynamic management view
ms.assetid: 568d89ed-2c96-4795-8a0c-2f3e375081da
caps.latest.revision: 111
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e2ae34c5ffd67712e925d8dda26dbc79680e3520
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmoswaitstats-transact-sql"></a>sys.dm_os_wait_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Retourne des informations sur toutes les attentes subies par les threads qui se sont exécutés. Vous pouvez utiliser cette vue agrégée pour diagnostiquer les problèmes de performance liés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et également liés à des requêtes et des traitements spécifiques. [Sys.dm_exec_session_wait_stats &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md) fournit des informations similaires par session.  
  
> [!NOTE] 
> Pour appeler cette de **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]**, utilisez le nom **sys.dm_pdw_nodes_os_wait_stats**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|Nom du type d'attente. Pour plus d’informations, consultez [Types d’attentes](#WaitTypes), plus loin dans cette rubrique.|  
|waiting_tasks_count|**bigint**|Nombre d'attentes sur ce type d'attente. Ce compteur est incrémenté au début de chaque attente.|  
|wait_time_ms|**bigint**|Temps d'attente total en millisecondes pour ce type d'attente. Ce temps comprend signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Temps d'attente maximal sur ce type d'attente.|  
|signal_wait_time_ms|**bigint**|Différence entre le moment où le thread qui attend a été signalé et le moment où il a commencé à s'exécuter.|  
|pdw_node_id|**int**|L’identificateur du nœud qui se trouve sur cette distribution. <br/> **S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

##  <a name="WaitTypes"></a> Types d’attentes  
 **Les attentes de ressource** attentes se produisent lorsqu’un processus de travail demande l’accès à une ressource qui n’est pas disponible, car la ressource est utilisée par un autre travail ou qu’il n’est pas encore disponible. Ces attentes sont par exemple des attentes de verrous, de verrous internes, de réseau et d'E/S de disque. Les attentes de verrous et de verrous internes sont des attentes sur des objets de synchronisation.  
  
**Attentes de file d’attente**  
 Les attentes de file d'attente se produisent lorsqu'un thread de travail est inactif, en attente d'une affectation de travail. Ces attentes se produisent le plus souvent avec des tâches système en arrière-plan, telles que la surveillance des interblocages et le nettoyage des enregistrements supprimés. Ces tâches attendent que les demandes de travail soient placées dans une file d'attente. Les attentes de file d'attente peuvent aussi devenir actives même si aucun nouveau paquet n'a été placé dans la file d'attente.  
  
 **Attentes externes**  
 Les attentes externes se produisent lorsqu'un thread de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend un événement externe, par exemple un appel de procédure stockée étendue ou une requête de serveur lié, pour se terminer. Lorsque vous diagnostiquez des problèmes de blocage, souvenez-vous que les attentes externes n'impliquent pas forcément que le thread de travail est inactif, parce qu'il est peut être en train d'exécuter du code externe.  
  
 `sys.dm_os_wait_stats` indique la durée des attentes qui se sont achevées. Cette vue de gestion dynamique n'affiche pas les attentes actuelles.  
  
 Un thread de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas considéré comme étant en train d'attendre si l'une de ces conditions est vraie :  
  
-   Une ressource devient disponible.  
  
-   Une file d'attente n'est pas vide.  
  
-   Un processus externe se termine.  
  
 Bien que le thread ne soit plus en train d'attendre, il n'a pas à redémarrer immédiatement. En effet, ce type de thread est d'abord placé dans la file d'attente des travaux pouvant s'exécuter et doit attendre qu'un quantum s'exécute sur le planificateur.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les compteurs de temps d’attente sont **bigint** les valeurs et par conséquent, ne sont pas sujets à la substitution de compteur en tant que les compteurs équivalents des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Des types spécifiques de temps d'attente pendant l'exécution des requêtes peuvent indiquer des goulots d'étranglement ou des points de blocage dans la requête. De la même façon, des temps d'attente élevés, ou des nombres d'attente à l'échelle du serveur peuvent indiquer des goulots d'étranglement ou des zones réactives en interaction avec l'instance du serveur. Par exemple, des attentes de verrou indiquent une contention de données par les requêtes ; des attentes de verrou interne d'E/S de page indiquent des temps de réponse E/S lents ; des attentes de mise à jour de verrous internes de page indiquent une mise en page de fichier incorrecte.  
  
 Le contenu de cette vue de gestion dynamique peut être réinitialisé en exécutant la commande suivante :  
  
```sql  
DBCC SQLPERF ('sys.dm_os_wait_stats', CLEAR);  
GO  
```  
  
Cette commande remet tous les compteurs à 0.  
  
> [!NOTE]
> Ces statistiques ne sont pas préservées d'un redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'autre et toutes les données s'accumulent depuis la dernière réinitialisation des statistiques ou le dernier démarrage du serveur.  
  
 Le tableau suivant récapitule les types d'attente que rencontrent les tâches.  

|Type | Description| 
|-------------------------- |--------------------------| 
|ABR |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| | 
|AM_INDBUILD_ALLOCATION |TBD <br />**S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|AM_SCHEMAMGR_UNSHARED_CACHE |TBD <br />**S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|ASSEMBLY_FILTER_HASHTABLE |TBD <br />**S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|ASSEMBLY_LOAD |Se produit pendant l'accès exclusif au chargement d'un assembly.| 
|ASYNC_DISKPOOL_LOCK |Se produit lors d'une tentative de synchronisation de threads parallèles qui effectuent des tâches telles que la création ou l'initialisation d'un fichier.| 
|ASYNC_IO_COMPLETION |Se produit lorsqu'une tâche attend la fin d'E/S.| 
|ASYNC_NETWORK_IO |Se produit sur des écritures réseau lorsque la tâche est bloquée derrière le réseau. Vérifiez que le client traite les données du serveur.| 
|ASYNC_OP_COMPLETION |TBD <br />**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|ASYNC_OP_CONTEXT_READ |TBD <br />**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|ASYNC_OP_CONTEXT_WRITE |TBD <br />**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|ASYNC_SOCKETDUP_IO |TBD <br />**S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|AUDIT_GROUPCACHE_LOCK |Se produit lorsqu'il y a une attente sur un verrou qui contrôle l'accès à un cache spécial. Le cache contient des informations sur les audits utilisés pour auditer chaque groupe d'actions d'audit.| 
|AUDIT_LOGINCACHE_LOCK |Se produit lorsqu'il y a une attente sur un verrou qui contrôle l'accès à un cache spécial. Le cache contient des informations sur les audits utilisés pour auditer des groupes d'actions d'audit de connexion.| 
|AUDIT_ON_DEMAND_TARGET_LOCK |Se produit lorsqu'il y a une attente sur un verrou utilisé pour garantir une initialisation unique des cibles d'événements étendus liées à l'audit.| 
|AUDIT_XE_SESSION_MGR |Se produit lorsqu'il y a une attente sur un verrou utilisé pour synchroniser le démarrage et l'arrêt des sessions d'événements étendus liées à l'audit.| 
|BACKUP |Se produit lorsqu'une tâche est bloquée dans un traitement de sauvegarde.| 
|BACKUP_OPERATOR |Se produit lorsqu'une tâche attend le montage d'une bande. Pour afficher l’état de la bande, la requête sys.dm_io_backup_tapes. S'il n'y a pas d'opération de montage en cours, ce type d'attente peut indiquer un problème matériel au niveau du lecteur de bande.| 
|BACKUPBUFFER |Se produit lorsqu'une tâche de sauvegarde attend des données ou une mémoire tampon pour y stocker les données. Ce type d'attente n'est pas typique, sauf lorsqu'une tâche attend qu'une bande monte.| 
|BACKUPIO |Se produit lorsqu'une tâche de sauvegarde attend des données ou une mémoire tampon pour y stocker les données. Ce type d'attente n'est pas typique, sauf lorsqu'une tâche attend qu'une bande monte.| 
|BACKUPTHREAD |Se produit lorsqu'une tâche attend la fin d'une tâche de sauvegarde. Les temps d'attente peuvent être longs, de plusieurs minutes à plusieurs heures. Si la tâche concernée est un processus d'E/S, ce type n'indique pas de problème.| 
|BAD_PAGE_PROCESS |Se produit lorsque le journal de pages suspectes en arrière-plan tente d'éviter une exécution plus que toutes les cinq secondes. Un nombre excessif de pages suspectes entraîne une exécution fréquente du journal.| 
|BLOB_METADATA |TBD <br />**S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BMPALLOCATION |TBD <br />**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BMPBUILD |TBD <br />**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BMPREPARTITION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BMPREPLICATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BPSORT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_CONNECTION_RECEIVE_TASK |Se produit lorsque vous attendez l'autorisation de recevoir un message sur un point de terminaison de connexion. L'accès au point de terminaison accordé est sérialisé.| 
|BROKER_DISPATCHER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_ENDPOINT_STATE_MUTEX |Se produit en cas de contention pour accéder à l’état d’un point de terminaison de connexion de Service Broker. L'accès à l'état pour procéder à des modifications est sérialisé.| 
|BROKER_EVENTHANDLER |Se produit lorsqu’une tâche est en attente dans le Gestionnaire d’événements principal de Service Broker. Ce doit être très bref.| 
|BROKER_FORWARDER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_INIT |Se produit lors de l’initialisation du Service Broker dans chaque base de données active. Cela ne doit pas arriver souvent.| 
|BROKER_MASTERSTART |Se produit lorsqu’une tâche est en attente pour le Gestionnaire d’événements principal de Service Broker pour démarrer. Ce doit être très bref.| 
|BROKER_RECEIVE_WAITFOR |Se produit lorsque RECEIVE WAITFOR attend. Cela peut signifier qu’aucun message n’est prêt à être reçu dans la file d’attente ou une contention de verrouillage empêche de recevoir des messages de la file d’attente.| 
|BROKER_REGISTERALLENDPOINTS |Se produit pendant l’initialisation d’un point de terminaison de connexion de Service Broker. Ce doit être très bref.| 
|BROKER_SERVICE |Se produit lorsque la liste de destination Service Broker qui est associée à un service cible est mis à jour ou retriée.| 
|BROKER_SHUTDOWN |Se produit lors d’un arrêt programmé de Service Broker. Ce doit être très bref, voire inexistant.| 
|BROKER_START |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_TASK_SHUTDOWN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_TASK_STOP |Se produit lorsque le Gestionnaire de tâches de file d’attente Service Broker tente d’arrêter la tâche. Le contrôle d'état est sérialisé et doit être au préalable dans un état d'exécution.| 
|BROKER_TASK_SUBMIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_TO_FLUSH |Se produit lors de la transmission de vidages de mémoire différée dispositif de vidage du Service Broker d’objets pour une table de travail.| 
|BROKER_TRANSMISSION_OBJECT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_TRANSMISSION_TABLE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_TRANSMISSION_WORK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|BROKER_TRANSMITTER |Se produit lorsque la transmission Service Broker est en attente de travail.| 
|BUILTIN_HASHKEY_MUTEX |Peut se produire après le démarrage d'une instance, pendant l'initialisation des structures de données internes. Ne se reproduira plus lorsque les structures de données auront été initialisées.| 
|CHANGE_TRACKING_WAITFORCHANGES |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CHECK_PRINT_RECORD |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|CHECK_SCANNER_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CHECK_TABLES_INITIALIZATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CHECK_TABLES_SINGLE_SCAN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CHECK_TABLES_THREAD_BARRIER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CHECKPOINT_QUEUE |Se produit lorsque la tâche de point de vérification attend la demande de point de vérification suivante.| 
|CHKPT |Se produit au démarrage du serveur pour signaler au thread du point de contrôle qu'il peut démarrer.| 
|CLEAR_DB |Se produit lors d'opérations qui modifient l'état d'une base de données, telles que l'ouverture ou la fermeture d'une base de données.| 
|CLR_AUTO_EVENT |Se produit lorsqu'une tâche est en train d'exécuter du CLR (Common Language Runtime) et qu'elle attend le début d'un événement automatique particulier. De longues attentes sont classiques et elles n'indiquent pas la présence d'un problème.| 
|CLR_CRST |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend avant de passer à une phase essentielle qui est actuellement utilisée par une autre tâche.| 
|CLR_JOIN |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend la fin d'une autre tâche. Cet état d'attente se produit lorsqu'il y a une jointure entre des tâches.| 
|CLR_MANUAL_EVENT |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend le début d'un événement manuel particulier.| 
|CLR_MEMORY_SPY |Se produit pendant une attente sur une acquisition de verrou pour une structure de données utilisée pour enregistrer toutes les allocations de mémoire virtuelle qui viennent du CLR. La structure de données est verrouillée pour maintenir son intégrité en cas d'accès parallèle.| 
|CLR_MONITOR |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend d'avoir obtenu un verrou sur le moniteur.| 
|CLR_RWLOCK_READER |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un verrou de lecteur.| 
|CLR_RWLOCK_WRITER |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un verrou d'écriture.| 
|CLR_SEMAPHORE |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un sémaphore.| 
|CLR_TASK_START |Se produit pendant l'attente du démarrage d'une tâche CLR.| 
|CLRHOST_STATE_ACCESS |Se produit lorsqu'il y a une attente pour l'acquisition d'un accès exclusif aux structures de données d'hébergement CLR. Ce type d'attente se produit lors de la configuration ou de la destruction du runtime CLR.| 
|CMEMPARTITIONED |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CMEMTHREAD |Se produit lorsqu'une tâche attend un objet mémoire thread-safe. Le temps d'attente peut augmenter en cas de contention liée au fait que plusieurs tâches essaient d'allouer de la mémoire à partir du même objet mémoire.| 
|COLUMNSTORE_BUILD_THROTTLE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|COLUMNSTORE_COLUMNDATASET_SESSION_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|COMMIT_TABLE |TBD| 
|CONNECTION_ENDPOINT_LOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|COUNTRECOVERYMGR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CREATE_DATINISERVICE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|CXCONSUMER |Se produit avec les plans de requête parallèles lorsqu’un thread consommateur attend qu’un thread producteur envoyer des lignes. Il s’agit d’un composant normal de l’exécution des requêtes parallèles. <br /> **S’applique aux**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (en commençant par [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3), [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|
|CXPACKET |Se produit avec les plans de requête parallèles, lors de la synchronisation de l’itérateur exchange de processeur de requêtes et lors de la production et la consommation des lignes. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme.<br /> **Remarque :** compter [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3, et [!INCLUDE[ssSDS](../../includes/sssds-md.md)], CXPACKET fait référence uniquement à la synchronisation de l’itérateur exchange de processeur de requêtes et à produire des lignes pour les threads de consommateur. Threads de consommateur sont suivis séparément dans le type d’attente CXCONSUMER.| 
|CXROWSET_SYNC |Se produit pendant une analyse de plage parallèle.| 
|DAC_INIT |Se produit alors que la connexion administrateur dédiée est en cours d'initialisation.| 
|DBCC_SCALE_OUT_EXPR_CACHE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DBMIRROR_DBM_EVENT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|DBMIRROR_DBM_MUTEX |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|DBMIRROR_EVENTS_QUEUE |Se produit lorsque la mise en miroir de base de données attend le traitement des événements.| 
|DBMIRROR_SEND |Se produit lorsqu'une tâche attend que le Backlog des communications au niveau de la couche réseau soit vidé pour pouvoir envoyer des messages. Indique que la couche des communications est proche de la saturation et que cela affecte le débit des données pour la mise en miroir de bases de données.| 
|DBMIRROR_WORKER_QUEUE |Indique que la tâche de travail de mise en miroir de base de données attend plus de travail.| 
|DBMIRRORING_CMD |Se produit lorsqu'une tâche attend que les enregistrements de journal soient vidés sur le disque. Cet état d'attente dure généralement assez longtemps.| 
|DBSEEDING_FLOWCONTROL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DBSEEDING_OPERATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DEADLOCK_ENUM_MUTEX |Se produit lorsque le moniteur de blocage et sys.dm_os_waiting_tasks pour vous assurer que SQL Server ne fonctionne pas plusieurs recherches d’interblocage en même temps.| 
|DEADLOCK_TASK_SEARCH |Une durée d'attente importante pour cette ressource indique que le serveur exécute des requêtes par-dessus sys.dm_os_waiting_tasks, et que ces dernières empêchent l'analyseur de blocages d'exécuter une recherche de blocage. Ce type d'attente est utilisé uniquement par l'analyseur d'interblocages. Les requêtes exécutées par-dessus sys.dm_os_waiting_tasks utilisent DEADLOCK_ENUM_MUTEX.| 
|DEBUG |Se produit au cours de Transact-SQL et le débogage CLR pour la synchronisation interne.| 
|DIRECTLOGCONSUMER_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DIRTY_PAGE_POLL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DIRTY_PAGE_SYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DIRTY_PAGE_TABLE_LOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DISABLE_VERSIONING |Se produit lorsque SQL Server interroge le Gestionnaire de transactions de version pour voir si l’horodatage de la plus ancienne transaction active est postérieur à l’horodatage de l’état de démarrage de la modification. Si c'est le cas, toutes les transactions d'instantané qui avaient démarré avant l'exécution de l'instruction ALTER DATABASE sont terminées. Cet état d’attente est utilisé lorsque SQL Server désactive le contrôle de version à l’aide de l’instruction ALTER DATABASE.| 
|DISKIO_SUSPEND |Se produit lorsqu'une tâche attend pour accéder à un fichier lorsqu'une sauvegarde externe est en cours. Ce type d'attente est signalé pour chaque processus utilisateur en attente. Un nombre supérieur à cinq par processus utilisateur peut indiquer que la sauvegarde externe prend trop de temps.| 
|DISPATCHER_PRIORITY_QUEUE_SEMAPHORE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DISPATCHER_QUEUE_SEMAPHORE |Se produit lorsqu'un thread du pool de répartiteurs attend de traiter d'autres travaux. Le temps d'attente pour ce type d'attente est supposé augmenter lorsque le répartiteur est inactif.| 
|DLL_LOADING_MUTEX |Se produit une fois pendant l'attente du chargement de la DLL de l'analyseur XML.| 
|DPT_ENTRY_LOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DROP_DATABASE_TIMER_TASK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DROPTEMP |Se produit entre les tentatives de suppression d'un objet temporaire si la tentative précédente a échoué. La durée d'attente augmente de manière exponentielle au fur et à mesure que les différentes tentatives de suppression échouent.| 
|DTC |Se produit lorsqu'une tâche s'occupe d'un événement qui est utilisé pour gérer une transition d'état. Contrôles de cet état lors de la récupération de transactions Microsoft Distributed Transaction Coordinator (MS DTC) se produit une fois que SQL Server reçoit la notification que le service MS DTC n’est plus disponible.| 
|DTC_ABORT_REQUEST |Se produit dans une session de travail MS DTC lorsque la session attend de prendre possession d'une transaction MS DTC. Une fois que MS DTC est propriétaire de la transaction, la session peut la restaurer. En général, la session attendra une autre session qui utilise la transaction.| 
|DTC_RESOLVE |Se produit lorsqu'une tâche de récupération attend la base de données master dans une transaction entre bases de données, afin de pouvoir interroger le résultat de la transaction.| 
|DTC_STATE |Se produit lorsqu'une tâche s'occupe d'un événement qui protège les modifications effectuées sur l'objet d'état global MS DTC interne. Cet état doit être maintenu pendant des périodes très courtes.| 
|DTC_TMDOWN_REQUEST |Se produit dans une session de travail MS DTC lorsque SQL Server reçoit la notification que le service MS DTC n’est pas disponible. Tout d'abord, le thread de travail attend que le processus de récupération MS DTC commence. Ensuite, il attend d'avoir obtenu le résultat de la transaction distribuée sur laquelle il travaille. Cela peut continuer jusqu'à ce que la connexion avec le service MS DTC ait été rétablie.| 
|DTC_WAITFOR_OUTCOME |Se produit lorsque des tâches de récupération attendent que MS DTC s'active pour permettre la résolution des transactions préparées.| 
|DTCNEW_ENLIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DTCNEW_PREPARE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DTCNEW_RECOVERY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DTCNEW_TM |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DTCNEW_TRANSACTION_ENLISTMENT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DTCPNTSYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|DUMP_LOG_COORDINATOR |Se produit lorsqu'une tâche principale attend qu'une tâche secondaire ait produit des données. En principe cet état n'arrive pas. Une longue attente indique un blocage inattendu. Il faut examiner la tâche secondaire.| 
|DUMP_LOG_COORDINATOR_QUEUE |TBD| 
|DUMPTRIGGER |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|EC |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|EE_PMOLOCK |Se produit durant la synchronisation de certains types d'allocations de mémoire durant l'exécution d'une instruction.| 
|EE_SPECPROC_MAP_INIT |Se produit durant la synchronisation de la création de la table de hachage de procédure interne. Cette attente se produit uniquement lors de l’initiale de l’accès à la table de hachage après le démarrage de l’instance de SQL Server.| 
|ENABLE_EMPTY_VERSIONING |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|ENABLE_VERSIONING |Se produit lorsque SQL Server attend que toutes les transactions de mise à jour de cette base de données soit terminée avant de déclarer que la base de données est prête à passer à l’état autorisé l’isolement d’instantané. Cet état est utilisé lorsque SQL Server permet l’isolement d’instantané à l’aide de l’instruction ALTER DATABASE.| 
|ERROR_REPORTING_MANAGER |Se produit durant la synchronisation de plusieurs initialisations simultanées de journaux d'erreurs.| 
|EXCHANGE |Se produit durant la synchronisation dans l'itérateur d'échange du processeur de requêtes au cours de requêtes parallèles.| 
|EXECSYNC |Se produit au cours de requêtes parallèles, lors de la synchronisation dans le processeur de requêtes, dans des zones qui ne sont pas liées à l'itérateur d'échange. Ces zones sont par exemple des images bitmap, des objets binaires volumineux et l'itérateur de spouleurs. Les objets LOB peuvent utiliser souvent cet état d'attente.| 
|EXECUTION_PIPE_EVENT_INTERNAL |Se produit durant la synchronisation entre les parties producteur et consommateur d'exécution de lot soumises via le contexte de connexion.| 
|EXTERNAL_RG_UPDATE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|EXTERNAL_SCRIPT_NETWORK_IO |TBD <br /> **S’applique aux**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à la version actuelle.| 
|EXTERNAL_SCRIPT_PREPARE_SERVICE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|EXTERNAL_SCRIPT_SHUTDOWN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|EXTERNAL_WAIT_ON_LAUNCHER, |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FABRIC_HADR_TRANSPORT_CONNECTION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FABRIC_REPLICA_CONTROLLER_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FABRIC_REPLICA_CONTROLLER_STATE_AND_CONFIG |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FABRIC_REPLICA_PUBLISHER_EVENT_PUBLISH |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FABRIC_REPLICA_PUBLISHER_SUBSCRIBER_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FABRIC_WAIT_FOR_BUILD_REPLICA_EVENT_PROCESSING |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FAILPOINT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FCB_REPLICA_READ |Se produit lorsque les lectures du fichier partiellement alloué d'un instantané (ou d'un instantané temporaire créée par DBCC) sont synchronisées.| 
|FCB_REPLICA_WRITE |Se produit lorsque des émissions ou extractions d'une page dans un fichier partiellement alloué d'instantané (ou un instantané temporaire créé par DBCC) sont synchronisées.| 
|FEATURE_SWITCHES_UPDATE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_DB_KILL_FLAG |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_DB_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_FCB |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_FCB_FIND |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_FCB_PARENT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_FCB_RELEASE_CACHED_ENTRIES |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_FCB_STATE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_FILEOBJECT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NSO_TABLE_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_NTFS_STORE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_RECOVERY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_RSFX_COMM |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_RSFX_WAIT_FOR_MEMORY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_STARTUP_SHUTDOWN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_STORE_DB |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_STORE_ROWSET_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FFT_STORE_TABLE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILE_VALIDATION_THREADS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILESTREAM_CACHE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILESTREAM_CHUNKER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILESTREAM_CHUNKER_INIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILESTREAM_FCB |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILESTREAM_FILE_OBJECT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILESTREAM_WORKITEM_QUEUE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FILETABLE_SHUTDOWN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FOREIGN_REDO |TBD <br /> **S’applique aux**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à la version actuelle.| 
|FORWARDER_TRANSITION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FS_FC_RWLOCK |Se produit lorsqu'il y a une attente par le garbage collector FILESTREAM pour effectuer l'une des opérations suivantes :| 
|FS_GARBAGE_COLLECTOR_SHUTDOWN |Se produit lorsque le garbage collector FILESTREAM attend l'exécution de tâches de nettoyage.| 
|FS_HEADER_RWLOCK |Se produit lorsqu'il y a une attente d'obtention de l'accès à l'en-tête FILESTREAM d'un conteneur de données FILESTREAM pour la lecture ou la mise à jour du contenu du fichier d'en-tête FILESTREAM (Filestream.hdr).| 
|FS_LOGTRUNC_RWLOCK |Se produit lorsqu'il y a une attente d'obtention de l'accès à la troncation de journaux FILESTREAM pour effectuer l'une des opérations suivantes :| 
|FSA_FORCE_OWN_XACT |Se produit lorsqu'une opération d'E/S de fichier FILESTREAM doit se lier à la transaction associée, alors que la transaction appartient actuellement à une autre session.| 
|FSAGENT |Se produit lorsqu'une opération d'E/S de fichier FILESTREAM attend une ressource de l'agent FILESTREAM utilisée par une autre opération d'E/S de fichier.| 
|FSTR_CONFIG_MUTEX |Se produit lorsqu'il y a une attente d'une autre reconfiguration de fonctionnalité FILESTREAM.| 
|FSTR_CONFIG_RWLOCK |Se produit lorsqu'il y a une attente de sérialisation de l'accès aux paramètres de configuration FILESTREAM.| 
|FT_COMPROWSET_RWLOCK |Le texte intégral attend une opération des métadonnées du fragment. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_IFTS_RWLOCK |Le texte intégral attend une synchronisation interne. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_IFTS_SCHEDULER_IDLE_WAIT |Type d'attente en état de veille du planificateur de texte intégral. Le planificateur est inactif.| 
|FT_IFTSHC_MUTEX |Le texte intégral attend une opération de contrôle fdhost. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_IFTSISM_MUTEX |Le texte intégral attend une opération de communication. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_MASTER_MERGE |Le texte intégral attend une opération de fusion principale. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_MASTER_MERGE_COORDINATOR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FT_METADATA_MUTEX |Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_PROPERTYLIST_CACHE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|FT_RESTART_CRAWL |Se produit lorsqu'une analyse de texte intégral doit redémarrer à partir du dernier point de référence connu et fiable pour une récupération faisant suite à une panne transitoire. Grâce à cette attente, les tâches travaillant actuellement sur ce remplissage peuvent se terminer ou quitter l'étape en cours.| 
|FULLTEXT GATHERER |Se produit durant la synchronisation d'opérations de texte intégral.| 
|GDMA_GET_RESOURCE_OWNER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GHOSTCLEANUP_UPDATE_STATS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GHOSTCLEANUPSYNCMGR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GLOBAL_QUERY_CANCEL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GLOBAL_QUERY_CLOSE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GLOBAL_QUERY_CONSUMER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GLOBAL_QUERY_PRODUCER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GLOBAL_TRAN_CREATE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GLOBAL_TRAN_UCS_SESSION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|GUARDIAN |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|HADR_AG_MUTEX |Se produit lorsqu’une instruction de DDL Always On ou de la commande de Clustering de basculement Windows Server est en attente pour l’accès exclusif en lecture/écriture à la configuration d’un groupe de disponibilité., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_AR_CRITICAL_SECTION_ENTRY |Se produit lorsqu’une instruction de DDL Always On ou de la commande de Clustering de basculement Windows Server est en attente pour l’accès exclusif en lecture/écriture à l’état d’exécution du réplica local du groupe de disponibilité associé., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_AR_MANAGER_MUTEX |Se produit lorsque un réplica de disponibilité à l'arrêt attend d'être redémarré ou qu'un réplica de disponibilité en cours de démarrage attend d'être arrêté. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_AR_UNLOAD_COMPLETED |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_ARCONTROLLER_NOTIFICATIONS_SUBSCRIBER_LIST |Le serveur de publication d'un événement de réplica de disponibilité (comme un changement d'état ou de configuration) attend l'accès exclusif en lecture/écriture à la liste des abonnés à un événement. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_BACKUP_BULK_LOCK |Toujours sur base de données primaire a reçu une demande de sauvegarde à partir d’une base de données secondaire et qu’il est en attente pour l’arrière-plan du thread pour terminer le traitement de la demande sur l’acquisition ou de libération du verrou BulkOp., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_BACKUP_QUEUE |Le thread d’arrière-plan de sauvegarde de la base primaire Always On est en attente pour une nouvelle demande de travail à partir de la base de données secondaire. (en règle générale, cela se produit lorsque la base de données primaire détient le journal BulkOp et attend que la base de données secondaire indiquer que la base de données primaire peut libérer le verrou)., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_CLUSAPI_CALL |Un thread de SQL Server est en attente pour basculer du mode non préemptif (planifié par SQL Server) en mode préemptif (planifié par le système d’exploitation) pour appeler l’API de Clustering de basculement Windows Server., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_COMPRESSED_CACHE_SYNC |Attente de l’accès au cache des blocs de journal compressés qui est utilisé pour éviter une compression redondante des blocs de journal envoyé à plusieurs bases de données secondaires., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_CONNECTIVITY_INFO |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DATABASE_FLOW_CONTROL |En attente de messages à envoyer au partenaire lorsque le nombre maximal de messages en file d'attente a été atteint. Indique que les analyses de journal s'exécutent plus rapidement que la vitesse d'envoi par le réseau. Ceci est un problème uniquement si le réseau envoie les plus lentes que prévu., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DATABASE_VERSIONING_STATE |Se produit sur le changement d’état de la base secondaire Always On de contrôle de version. Cette attente pour les structures de données internes et est généralement très courte durée sans aucune incidence directe sur l’accès aux données., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DATABASE_WAIT_FOR_RECOVERY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DATABASE_WAIT_FOR_RESTART |En attente de la base de données à redémarrer sous contrôle de groupes de disponibilité AlwaysOn. Dans des conditions normales, il n’est pas un problème du client, car les attentes sont attendus ici., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING |Une requête sur un ou plusieurs objets dans une base de données secondaire accessible en lecture d’une toujours sur le groupe de disponibilité est bloqué sur le contrôle de version de ligne pendant l’attente de validation ou de restauration de toutes les transactions qui étaient en cours lorsque le réplica secondaire a été activé pour les charges de travail en lecture. Ce type d’attente garantit que les versions de ligne sont disponibles avant l’exécution d’une requête en isolement d’instantané., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DB_COMMAND |En attente de réponses aux messages de conversation (qui nécessitent une réponse explicite de l’autre côté, à l’aide de l’infrastructure de message de conversation Always On). Un nombre de différents types de messages utilisent ce type d’attente., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DB_OP_COMPLETION_SYNC |En attente de réponses aux messages de conversation (qui nécessitent une réponse explicite de l’autre côté, à l’aide de l’infrastructure de message de conversation Always On). Un nombre de différents types de messages utilisent ce type d’attente., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DB_OP_START_SYNC |Une instruction DDL Always On ou une commande de Clustering de basculement Windows Server est en attente pour l’accès sérialisé à une base de données de disponibilité et son état d’exécution., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DBR_SUBSCRIBER |Le serveur de publication d'un événement de réplica de disponibilité (comme un changement d'état ou de configuration) attend l'accès exclusif en lecture/écriture à l'état d'exécution d'un abonné à un événement qui correspond à une base de données de disponibilité. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DBR_SUBSCRIBER_FILTER_LIST |Le serveur de publication d'un événement de réplica de disponibilité (comme un changement d'état ou de configuration) attend l'accès exclusif en lecture/écriture à la liste des abonnés à un événement qui correspondent à des bases de données de disponibilité. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DBSEEDING |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DBSEEDING_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_DBSTATECHANGE_SYNC |Attente de contrôle d’accès concurrentiel pour mettre à jour l’état interne de la base de données réplica., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FABRIC_CALLBACK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FILESTREAM_BLOCK_FLUSH |Le Gestionnaire de transport FILESTREAM Always On est en attente jusqu'à ce que le traitement d’un bloc de journal est terminé., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FILESTREAM_FILE_CLOSE |Le Gestionnaire de transport FILESTREAM Always On est en attente jusqu'à ce que le prochain fichier FILESTREAM soit traité et que son descripteur se ferme., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FILESTREAM_FILE_REQUEST |Une toujours sur réplica secondaire est en attente pour le réplica principal envoyer FILESTREAM demandée tous les fichiers durant l’annulation., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FILESTREAM_IOMGR |Le Gestionnaire de transport FILESTREAM Always On est en attente de verrou en lecture/écriture qui protège le gestionnaire FILESTREAM toujours sur les e/s lors du démarrage ou arrêt., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FILESTREAM_IOMGR_IOCOMPLETION |Le gestionnaire FILESTREAM toujours sur les e/s est en attente d’achèvement d’e/s., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FILESTREAM_MANAGER |Le Gestionnaire de transport FILESTREAM Always On attend le verrou en lecture/écriture qui protège le Gestionnaire de transport FILESTREAM Always On pendant le démarrage ou l’arrêt., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_FILESTREAM_PREPROC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_GROUP_COMMIT |Le traitement de validation de la transaction attend d'autoriser la validation d'un groupe afin que plusieurs enregistrements de journal de validation puissent être placés dans un bloc de journal unique. Cette attente est une condition prévue qui optimise le journal des e/s, de capturer et d’envoyer des opérations., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_LOGCAPTURE_SYNC |Contrôle de concurrence autour de la capture du journal ou de l'objet d'application lors de la création ou la destruction d'analyses. Il s’agit d’une attente prévue lorsque les serveurs partenaires changent d’état état ou de la connexion., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_LOGCAPTURE_WAIT |En attente de la mise à disposition d'enregistrements de journal. Peut se produire en cas d'attente de la génération de nouveaux enregistrements de journal par des connexions ou de la réalisation d'opérations d'E/S lors de la lecture du journal ne figurant pas dans le cache. Cela a une attente prévue si l’analyse du journal est au niveau de la fin du journal ou la lecture à partir du disque., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_LOGPROGRESS_SYNC |Attente de contrôle d’accès concurrentiel lors de la mise à jour de l’état d’avancement des réplicas de base de données de journal., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_NOTIFICATION_DEQUEUE |Une tâche en arrière-plan qui traite les notifications de clustering de basculement Windows Server attend la notification suivante. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_NOTIFICATION_WORKER_EXCLUSIVE_ACCESS |Le Gestionnaire de réplicas de disponibilité Always On est en attente pour l’accès sérialisé à l’état d’exécution d’une tâche en arrière-plan qui traite les notifications de Clustering de basculement Windows Server. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_NOTIFICATION_WORKER_STARTUP_SYNC |Une tâche en arrière-plan attend la fin du démarrage d'une tâche en arrière-plan qui traite les notifications de clustering de basculement Windows Server. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_NOTIFICATION_WORKER_TERMINATION_SYNC |Une tâche en arrière-plan attend la fin d'une tâche en arrière-plan qui traite les notifications de clustering de basculement Windows Server. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_PARTNER_SYNC |Attente de contrôle d’accès concurrentiel sur la liste des partenaires., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_READ_ALL_NETWORKS |En attente de l'obtention de l'accès en lecture ou en écriture à la liste des réseaux WSFC. À usage interne uniquement Remarque : Le moteur conserve une liste des réseaux WSFC qui est utilisée dans les vues de gestion dynamique (par exemple, sys.dm_hadr_cluster_networks) ou pour valider toujours sur Transact-SQL instructions faisant référence à WSFC les informations de réseau. Rapport avec WSFC de cette liste est mise à jour lors du démarrage du moteur, les notifications et Always On interne redémarrage (par exemple, perdre et regagnant le quorum WSFC). Les tâches sont généralement bloquées lorsqu'une mise à jour est en cours dans cette liste. , <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_RECOVERY_WAIT_FOR_CONNECTION |En attente de la connexion de la base de données secondaire à la base de données primaire avant d'effectuer la récupération. Il s’agit d’une attente prévue, ce qui peut allonger si la connexion vers le réplica principal est lente à établir., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_RECOVERY_WAIT_FOR_UNDO |La récupération de base de données attend que la base de données secondaire termine la phase de rétablissement et d'initialisation afin de la ramener au point de journal commun avec la base de données primaire. Il s’agit une attente prévue après les basculements. Annuler progression peut être suivie par le biais du Moniteur système de Windows (perfmon.exe) et les vues de gestion dynamique., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_REPLICAINFO_SYNC |En attente de contrôle d’accès concurrentiel mettre à jour l’état du réplica en cours., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SEEDING_CANCELLATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SEEDING_FILE_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SEEDING_LIMIT_BACKUPS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SEEDING_SYNC_COMPLETION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SEEDING_TIMEOUT_TASK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SEEDING_WAIT_FOR_COMPLETION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SYNC_COMMIT |En attente du traitement de validation de la transaction pour que les bases de données secondaires synchronisées renforcent le journal. Cette attente est également reflétée par le compteur de performances Délai de transaction. Ce type d’attente est attendu pour la synchronisation de groupes de disponibilité et indique le temps d’envoyer, écrire et accepter le journal dans les bases de données secondaire., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_SYNCHRONIZING_THROTTLE |En attente du traitement de validation de la transaction pour permettre à une base de données secondaire en cours de synchronisation de se mettre au niveau de la fin du journal de la base de données primaire afin d'assurer la transition vers l'état synchronisé. Il s’agit d’une attente prévue lors de la base de données secondaire rattrape., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_TDS_LISTENER_SYNC |Le système AlwaysOn interne ou sur le cluster WSFC demande que les écouteurs sont démarrés ou arrêtés. Le traitement de cette demande est toujours asynchrone et il existe un mécanisme pour supprimer les demandes redondantes. Parfois, ce processus est interrompu en raison de modifications de la configuration. Toutes les attentes en rapport avec ce mécanisme de synchronisation des écouteurs font appel à ce type d'attente. Usage interne uniquement., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_TDS_LISTENER_SYNC_PROCESSING |Utilisé à la fin d’une instruction toujours sur Transact-SQL qui nécessite le démarrage et/ou l’arrêt d’écouteur de groupe anavailability. Étant donné que l’opération de démarrage/arrêt est terminée de façon asynchrone, bloque le thread d’utilisateur à l’aide de ce type d’attente jusqu'à ce que la situation de l’écouteur est connue., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_THROTTLE_LOG_RATE_GOVERNOR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_THROTTLE_LOG_RATE_LOG_SIZE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_THROTTLE_LOG_RATE_SEEDING |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_TIMER_TASK |En attente de l'obtention du verrou sur l'objet de tâche du minuteur ; également utilisé pour les attentes réelles entre l'exécution de travaux. Par exemple, pour une tâche qui s’exécute toutes les 10 secondes, après une exécution, les groupes de disponibilité AlwaysOn patientent environ 10 secondes de replanifier la tâche, et l’attente est incluse ici., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_TRANSPORT_DBRLIST |En attente de l'accès à la liste des réplicas de bases de données de la couche de transport. Utilisé pour le verrouillage spinlock qui accorde l’accès à ce dernier., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_TRANSPORT_FLOW_CONTROL |En attente lorsque le nombre de messages en attente sans accusé de réception Always On est sur le hors seuil de contrôle de flux. Il s’agit d’une base de réplica de disponibilité (et non sur une base de données à base de données)., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_TRANSPORT_SESSION |Groupes de disponibilité AlwaysOn attendent lors de la modification ou l’accès à l’état du transport sous-jacent., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_WORK_POOL |Attente de contrôle d’accès concurrentiel sur l’objet de tâche de travail en arrière-plan groupes de disponibilité AlwaysOn., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_WORK_QUEUE |Groupes de disponibilité AlwaysOn en arrière-plan de thread de travail en attente pour les nouvelles tâches à affecter. Il s’agit d’une attente prévue lorsque des travailleurs prêt en attente de travail nouveaux, qui est un état normal., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HADR_XRF_STACK_ACCESS |L’accès aux (Rechercher, ajouter et supprimer) la pile de branchements de récupération étendus pour une base de données Always On disponibilité., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HCCO_CACHE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HK_RESTORE_FILEMAP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HKCS_PARALLEL_MIGRATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HKCS_PARALLEL_RECOVERY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HTBUILD |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HTDELETE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HTMEMO |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HTREINIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HTREPARTITION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|HTTP_ENUMERATION |Se produit au démarrage pour énumérer les points de terminaison HTTP pour lancer HTTP.| 
|HTTP_START |Se produit lorsqu'une connexion attend que le protocole HTTP termine l'initialisation.| 
|HTTP_STORAGE_CONNECTION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|IMPPROV_IOWAIT |Se produit lorsque SQL Server attend le chargement en bloc d’e/s se termine.| 
|INSTANCE_LOG_RATE_GOVERNOR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|INTERNAL_TESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|IO_AUDIT_MUTEX |Se produit durant la synchronisation des mémoires tampons d'événements de trace.| 
|IO_COMPLETION |Se produit durant l'attente de l'exécution des opérations d'E/S. Ce type d'attente représente en général des entrées/sorties de page qui ne sont pas des données. Données d’e/s de page apparaissent en tant que PAGEIOLATCH\_ \* attend.| 
|IO_QUEUE_LIMIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|IO_RETRY |Se produit lorsqu'une opération d'E/S telle qu'une lecture ou une écriture sur disque échoue en raison de ressources insuffisantes, puis est retentée.| 
|IOAFF_RANGE_QUEUE |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|KSOURCE_WAKEUP |Utilisé par la tâche de contrôle de service pour les demandes émanant du Gestionnaire de contrôle des services. De longues attentes sont prévisibles, et elles n'indiquent pas la présence d'un problème.| 
|KTM_ENLISTMENT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|KTM_RECOVERY_MANAGER |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|KTM_RECOVERY_RESOLUTION |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LATCH_DT |Se produit pendant l'attente d'un verrou en mode de destruction. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. Une liste des verrous\_ \* attentes est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_EX |Se produit pendant l'attente d'un verrou exclusif. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. Une liste des verrous\_ \* attentes est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_KP |Se produit pendant l'attente d'un verrou de maintien. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. Une liste des verrous\_ \* attentes est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LATCH_SH |Se produit pendant l'attente d'un verrou de partage. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. Une liste des verrous\_ \* attentes est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_UP |Se produit pendant l'attente d'un verrou de mise à jour. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. Une liste des verrous\_ \* attentes est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LAZYWRITER_SLEEP |Se produit lorsque des tâches d'écriture différée sont suspendues. Il s'agit d'une mesure de la durée consacrée aux tâches en arrière-plan qui attendent. Ne considérez pas cet état lorsque vous cherchez des blocages d'utilisateur.| 
|LCK_M_BU |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour en bloc.| 
|LCK_M_BU_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour en bloc avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_BU_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour en bloc avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_IS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel partagé.| 
|LCK_M_IS_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel partagé (IS) avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_IS_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel partagé (IS) avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_IU |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour.| 
|LCK_M_IU_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour (IU) avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_IU_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour (IU) avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_IX |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif.| 
|LCK_M_IX_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif (IX) avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_IX_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif (IX) avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_NL |Se produit lorsqu'une tâche attend pour acquérir un verrou NULL sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente. Un verrou NULL sur la clé est un verrou de libération instantanée.| 
|LCK_M_RIn_NL_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou NULL avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. Un verrou NULL sur la clé est un verrou de libération instantanée. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_NL_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou NULL avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. Un verrou NULL sur la clé est un verrou de libération instantanée. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente.| 
|LCK_M_RIn_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_U |La tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente.| 
|LCK_M_RIn_U_ABORT_BLOCKERS |La tâche attend d'acquérir un verrou de mise à jour avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_U_LOW_PRIORITY |La tâche attend d'acquérir un verrou de mise à jour avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_X |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente.| 
|LCK_M_RIn_X_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RIn_X_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RS_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes partagés entre la clé actuelle et la clé précédente.| 
|LCK_M_RS_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage partagée avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RS_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité base sur la valeur de clé actuelle, et un verrou de plage partagée avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RS_U |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes de mises à jour entre la clé actuelle et la clé précédente.| 
|LCK_M_RS_U_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage de mise à jour avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RS_U_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec une priorité base sur la valeur de clé actuelle, et un verrou de plage de mise à jour avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RX_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente.| 
|LCK_M_RX_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage exclusive avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RX_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité base sur la valeur de clé actuelle, et un verrou de plage exclusive avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RX_U |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente.| 
|LCK_M_RX_U_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage exclusive avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RX_U_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec une priorité base sur la valeur de clé actuelle, et un verrou de plage exclusive avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RX_X |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente.| 
|LCK_M_RX_X_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage exclusive avec des blocages d'abandon entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_RX_X_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec une priorité base sur la valeur de clé actuelle, et un verrou de plage exclusive avec une priorité basse entre la clé actuelle et la clé précédente. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé.| 
|LCK_M_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SCH_M |Se produit lorsqu'une tâche attend pour acquérir un verrou de modification de schéma.| 
|LCK_M_SCH_M_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de modification de schéma avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SCH_M_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de modification de schéma avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SCH_S |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage de schéma.| 
|LCK_M_SCH_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage de schéma avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SCH_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage de schéma avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SIU |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour partagé.| 
|LCK_M_SIU_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour partagé avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SIU_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour partagé avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SIX |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif partagé.| 
|LCK_M_SIX_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif partagé avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_SIX_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif partagé avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_U |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour.| 
|LCK_M_U_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_U_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_UIX |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif de mise à jour.| 
|LCK_M_UIX_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif de mise à jour avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_UIX_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif de mise à jour avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_X |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif.| 
|LCK_M_X_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec des blocages d'abandon. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LCK_M_X_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec une priorité basse. (Lié à l’option d’attente de priorité basse de ALTER TABLE et ALTER INDEX.), <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOG_POOL_SCAN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOG_RATE_GOVERNOR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGBUFFER |Se produit lorsqu'une tâche attend de l'espace dans le tampon de journal pour stocker un enregistrement de journal. Des valeurs élevées fréquentes peuvent indiquer que les unités de journaux ne parviennent pas à gérer la quantité d'entrées de journaux produites par le serveur.| 
|LOGCAPTURE_LOGPOOLTRUNCPOINT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGGENERATION |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LOGMGR |Se produit lorsqu'une tâche attend la fin des E/S de journal en cours avant d'arrêter le journal lors de la fermeture de la base de données.| 
|LOGMGR_FLUSH |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LOGMGR_PMM_LOG |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGMGR_QUEUE |Se produit lorsque la tâche d'écriture du journal attend des demandes de travail.| 
|LOGMGR_RESERVE_APPEND |Se produit lorsqu'une tâche attend de voir si la troncature du journal libère de l'espace pour lui permettre d'écrire un nouvel enregistrement dans le journal. Vous pouvez éventuellement augmenter la taille des fichiers journaux correspondant à la base de données concernée pour réduire cette attente.| 
|LOGPOOL_CACHESIZE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGPOOL_CONSUMER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGPOOL_CONSUMERSET |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGPOOL_FREEPOOLS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGPOOL_MGRSET |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGPOOL_REPLACEMENTSET |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOGPOOLREFCOUNTEDOBJECT_REFDONE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|LOWFAIL_MEMMGR_QUEUE |Se produit lorsque vous attendez que la mémoire soit disponible afin d'être utilisée.| 
|MD_AGENT_YIELD |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|MD_LAZYCACHE_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|MEMORY_ALLOCATION_EXT |Se produit lors de l’allocation de mémoire à partir du pool de mémoire interne SQL Server ou le système d’exploitation., <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|MEMORY_GRANT_UPDATE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|METADATA_LAZYCACHE_RWLOCK |TBD <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|MIGRATIONBUFFER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|MISCELLANEOUS |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|MISCELLANEOUS |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|MSQL_DQ |Se produit lorsqu'une tâche attend la fin d'une opération de requête distribuée. Permet de détecter d'éventuels interblocages d'application MARS (Multiple Active Result Set). L'attente se termine à la fin de l'appel de requête distribuée.| 
|MSQL_XACT_MGR_MUTEX |Se produit lorsqu'une tâche attend d'avoir obtenu la propriété du gestionnaire de transactions de la session pour effectuer une opération de transaction au niveau de la session.| 
|MSQL_XACT_MUTEX |Se produit durant la synchronisation de l'utilisation de la transaction. Une demande doit d'abord obtenir l'exclusion mutuelle pour pouvoir utiliser la transaction.| 
|MSQL_XP |Se produit lorsqu'une tâche attend la fin d'une procédure stockée étendue. SQL Server utilise cet état d’attente pour détecter d’éventuels blocages d’application MARS. L'attente se termine à la fin de l'appel de procédure stockée étendue.| 
|MSSEARCH |Se produit durant des appels de recherche en texte intégral. Cette attente se termine lorsque l'opération de texte intégral prend fin. Ces informations n'indiquent pas des contentions, mais plutôt la durée des opérations de texte intégral.| 
|NET_WAITFOR_PACKET |Se produit lorsqu'une connexion attend un paquet réseau durant une lecture sur le réseau.| 
|NETWORKSXMLMGRLOAD |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|NODE_CACHE_MUTEX |TBD| 
|OLEDB |Se produit lorsque SQL Server appelle le fournisseur SQL Server Native Client OLE DB. Ce type d'attente n'est pas utilisé pour la synchronisation. Par contre, il indique la durée des appels émis vers le fournisseur OLE DB.| 
|ONDEMAND_TASK_QUEUE |Se produit lorsqu'une tâche en arrière-plan attend des demandes de tâches système à priorité élevée. De longues attentes indiquent qu'il n'existe aucune demande à priorité élevée à traiter, et elles ne révèlent pas l'existence d'un problème.| 
|PAGEIOLATCH_DT |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de destruction. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_EX |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode exclusif. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_KP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de conservation. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PAGEIOLATCH_SH |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode partagé. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_UP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de mise à jour. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGELATCH_DT |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de destruction.| 
|PAGELATCH_EX |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode exclusif.| 
|PAGELATCH_KP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de conservation.| 
|PAGELATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PAGELATCH_SH |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode partagé.| 
|PAGELATCH_UP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de mise à jour.| 
|PARALLEL_BACKUP_QUEUE |Se produit lors de la sérialisation de la sortie générée par RESTORE HEADERONLY, RESTORE FILELISTONLY ou RESTORE LABELONLY.| 
|PARALLEL_REDO_DRAIN_WORKER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PARALLEL_REDO_FLOW_CONTROL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PARALLEL_REDO_LOG_CACHE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PARALLEL_REDO_TRAN_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PARALLEL_REDO_TRAN_TURN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PARALLEL_REDO_WORKER_SYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PARALLEL_REDO_WORKER_WAIT_WORK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PERFORMANCE_COUNTERS_RWLOCK |TBD| 
|PHYSICAL_SEEDING_DMV |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|POOL_LOG_RATE_GOVERNOR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_ABR |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG |Se produit lorsque le planificateur du système d'exploitation [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] (SQLOS) bascule en mode préemptif pour écrire un événement d'audit dans le journal des événements Windows. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour écrire un événement d'audit dans le journal de sécurité Windows. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|PREEMPTIVE_CLOSEBACKUPMEDIA |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer le support de sauvegarde.| 
|PREEMPTIVE_CLOSEBACKUPTAPE |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer une unité de sauvegarde sur bande.| 
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer une unité de sauvegarde virtuelle.| 
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour effectuer des opérations de cluster de basculement Windows.| 
|PREEMPTIVE_COM_COCREATEINSTANCE |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour créer un objet COM.| 
|PREEMPTIVE_COM_COGETCLASSOBJECT |TBD| 
|PREEMPTIVE_COM_CREATEACCESSOR |TBD| 
|PREEMPTIVE_COM_DELETEROWS |TBD| 
|PREEMPTIVE_COM_GETCOMMANDTEXT |TBD| 
|PREEMPTIVE_COM_GETDATA |TBD| 
|PREEMPTIVE_COM_GETNEXTROWS |TBD| 
|PREEMPTIVE_COM_GETRESULT |TBD| 
|PREEMPTIVE_COM_GETROWSBYBOOKMARK |TBD| 
|PREEMPTIVE_COM_LBFLUSH |TBD| 
|PREEMPTIVE_COM_LBLOCKREGION |TBD| 
|PREEMPTIVE_COM_LBREADAT |TBD| 
|PREEMPTIVE_COM_LBSETSIZE |TBD| 
|PREEMPTIVE_COM_LBSTAT |TBD| 
|PREEMPTIVE_COM_LBUNLOCKREGION |TBD| 
|PREEMPTIVE_COM_LBWRITEAT |TBD| 
|PREEMPTIVE_COM_QUERYINTERFACE |TBD| 
|PREEMPTIVE_COM_RELEASE |TBD| 
|PREEMPTIVE_COM_RELEASEACCESSOR |TBD| 
|PREEMPTIVE_COM_RELEASEROWS |TBD| 
|PREEMPTIVE_COM_RELEASESESSION |TBD| 
|PREEMPTIVE_COM_RESTARTPOSITION |TBD| 
|PREEMPTIVE_COM_SEQSTRMREAD |TBD| 
|PREEMPTIVE_COM_SEQSTRMREADANDWRITE |TBD| 
|PREEMPTIVE_COM_SETDATAFAILURE |TBD| 
|PREEMPTIVE_COM_SETPARAMETERINFO |TBD| 
|PREEMPTIVE_COM_SETPARAMETERPROPERTIES |TBD| 
|PREEMPTIVE_COM_STRMLOCKREGION |TBD| 
|PREEMPTIVE_COM_STRMSEEKANDREAD |TBD| 
|PREEMPTIVE_COM_STRMSEEKANDWRITE |TBD| 
|PREEMPTIVE_COM_STRMSETSIZE |TBD| 
|PREEMPTIVE_COM_STRMSTAT |TBD| 
|PREEMPTIVE_COM_STRMUNLOCKREGION |TBD| 
|PREEMPTIVE_CONSOLEWRITE |TBD| 
|PREEMPTIVE_CREATEPARAM |TBD| 
|PREEMPTIVE_DEBUG |TBD| 
|PREEMPTIVE_DFSADDLINK |TBD| 
|PREEMPTIVE_DFSLINKEXISTCHECK |TBD| 
|PREEMPTIVE_DFSLINKHEALTHCHECK |TBD| 
|PREEMPTIVE_DFSREMOVELINK |TBD| 
|PREEMPTIVE_DFSREMOVEROOT |TBD| 
|PREEMPTIVE_DFSROOTFOLDERCHECK |TBD| 
|PREEMPTIVE_DFSROOTINIT |TBD| 
|PREEMPTIVE_DFSROOTSHARECHECK |TBD| 
|PREEMPTIVE_DTC_ABORT |TBD| 
|PREEMPTIVE_DTC_ABORTREQUESTDONE |TBD| 
|PREEMPTIVE_DTC_BEGINTRANSACTION |TBD| 
|PREEMPTIVE_DTC_COMMITREQUESTDONE |TBD| 
|PREEMPTIVE_DTC_ENLIST |TBD| 
|PREEMPTIVE_DTC_PREPAREREQUESTDONE |TBD| 
|PREEMPTIVE_FILESIZEGET |TBD| 
|PREEMPTIVE_FSAOLEDB_ABORTTRANSACTION |TBD| 
|PREEMPTIVE_FSAOLEDB_COMMITTRANSACTION |TBD| 
|PREEMPTIVE_FSAOLEDB_STARTTRANSACTION |TBD| 
|PREEMPTIVE_FSRECOVER_UNCONDITIONALUNDO |TBD| 
|PREEMPTIVE_GETRMINFO |TBD| 
|PREEMPTIVE_HADR_LEASE_MECHANISM |Planification pour les diagnostics CSS de gestionnaire de bail de groupes de disponibilité AlwaysOn., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_HTTP_EVENT_WAIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_HTTP_REQUEST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_LOCKMONITOR |TBD| 
|PREEMPTIVE_MSS_RELEASE |TBD| 
|PREEMPTIVE_ODBCOPS |TBD| 
|PREEMPTIVE_OLE_UNINIT |TBD| 
|PREEMPTIVE_OLEDB_ABORTORCOMMITTRAN |TBD| 
|PREEMPTIVE_OLEDB_ABORTTRAN |TBD| 
|PREEMPTIVE_OLEDB_GETDATASOURCE |TBD| 
|PREEMPTIVE_OLEDB_GETLITERALINFO |TBD| 
|PREEMPTIVE_OLEDB_GETPROPERTIES |TBD| 
|PREEMPTIVE_OLEDB_GETPROPERTYINFO |TBD| 
|PREEMPTIVE_OLEDB_GETSCHEMALOCK |TBD| 
|PREEMPTIVE_OLEDB_JOINTRANSACTION |TBD| 
|PREEMPTIVE_OLEDB_RELEASE |TBD| 
|PREEMPTIVE_OLEDB_SETPROPERTIES |TBD| 
|PREEMPTIVE_OLEDBOPS |TBD| 
|PREEMPTIVE_OS_ACCEPTSECURITYCONTEXT |TBD| 
|PREEMPTIVE_OS_ACQUIRECREDENTIALSHANDLE |TBD| 
|PREEMPTIVE_OS_AUTHENTICATIONOPS |TBD| 
|PREEMPTIVE_OS_AUTHORIZATIONOPS |TBD| 
|PREEMPTIVE_OS_AUTHZGETINFORMATIONFROMCONTEXT |TBD| 
|PREEMPTIVE_OS_AUTHZINITIALIZECONTEXTFROMSID |TBD| 
|PREEMPTIVE_OS_AUTHZINITIALIZERESOURCEMANAGER |TBD| 
|PREEMPTIVE_OS_BACKUPREAD |TBD| 
|PREEMPTIVE_OS_CLOSEHANDLE |TBD| 
|PREEMPTIVE_OS_CLUSTEROPS |TBD| 
|PREEMPTIVE_OS_COMOPS |TBD| 
|PREEMPTIVE_OS_COMPLETEAUTHTOKEN |TBD| 
|PREEMPTIVE_OS_COPYFILE |TBD| 
|PREEMPTIVE_OS_CREATEDIRECTORY |TBD| 
|PREEMPTIVE_OS_CREATEFILE |TBD| 
|PREEMPTIVE_OS_CRYPTACQUIRECONTEXT |TBD| 
|PREEMPTIVE_OS_CRYPTIMPORTKEY |TBD| 
|PREEMPTIVE_OS_CRYPTOPS |TBD| 
|PREEMPTIVE_OS_DECRYPTMESSAGE |TBD| 
|PREEMPTIVE_OS_DELETEFILE |TBD| 
|PREEMPTIVE_OS_DELETESECURITYCONTEXT |TBD| 
|PREEMPTIVE_OS_DEVICEIOCONTROL |TBD| 
|PREEMPTIVE_OS_DEVICEOPS |TBD| 
|PREEMPTIVE_OS_DIRSVC_NETWORKOPS |TBD| 
|PREEMPTIVE_OS_DISCONNECTNAMEDPIPE |TBD| 
|PREEMPTIVE_OS_DOMAINSERVICESOPS |TBD| 
|PREEMPTIVE_OS_DSGETDCNAME |TBD| 
|PREEMPTIVE_OS_DTCOPS |TBD| 
|PREEMPTIVE_OS_ENCRYPTMESSAGE |TBD| 
|PREEMPTIVE_OS_FILEOPS |TBD| 
|PREEMPTIVE_OS_FINDFILE |TBD| 
|PREEMPTIVE_OS_FLUSHFILEBUFFERS |TBD| 
|PREEMPTIVE_OS_FORMATMESSAGE |TBD| 
|PREEMPTIVE_OS_FREECREDENTIALSHANDLE |TBD| 
|PREEMPTIVE_OS_FREELIBRARY |TBD| 
|PREEMPTIVE_OS_GENERICOPS |TBD| 
|PREEMPTIVE_OS_GETADDRINFO |TBD| 
|PREEMPTIVE_OS_GETCOMPRESSEDFILESIZE |TBD| 
|PREEMPTIVE_OS_GETDISKFREESPACE |TBD| 
|PREEMPTIVE_OS_GETFILEATTRIBUTES |TBD| 
|PREEMPTIVE_OS_GETFILESIZE |TBD| 
|PREEMPTIVE_OS_GETFINALFILEPATHBYHANDLE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_OS_GETLONGPATHNAME |TBD| 
|PREEMPTIVE_OS_GETPROCADDRESS |TBD| 
|PREEMPTIVE_OS_GETVOLUMENAMEFORVOLUMEMOUNTPOINT |TBD| 
|PREEMPTIVE_OS_GETVOLUMEPATHNAME |TBD| 
|PREEMPTIVE_OS_INITIALIZESECURITYCONTEXT |TBD| 
|PREEMPTIVE_OS_LIBRARYOPS |TBD| 
|PREEMPTIVE_OS_LOADLIBRARY |TBD| 
|PREEMPTIVE_OS_LOGONUSER |TBD| 
|PREEMPTIVE_OS_LOOKUPACCOUNTSID |TBD| 
|PREEMPTIVE_OS_MESSAGEQUEUEOPS |TBD| 
|PREEMPTIVE_OS_MOVEFILE |TBD| 
|PREEMPTIVE_OS_NETGROUPGETUSERS |TBD| 
|PREEMPTIVE_OS_NETLOCALGROUPGETMEMBERS |TBD| 
|PREEMPTIVE_OS_NETUSERGETGROUPS |TBD| 
|PREEMPTIVE_OS_NETUSERGETLOCALGROUPS |TBD| 
|PREEMPTIVE_OS_NETUSERMODALSGET |TBD| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICY |TBD| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICYFREE |TBD| 
|PREEMPTIVE_OS_OPENDIRECTORY |TBD| 
|PREEMPTIVE_OS_PDH_WMI_INIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_OS_PIPEOPS |TBD| 
|PREEMPTIVE_OS_PROCESSOPS |TBD| 
|PREEMPTIVE_OS_QUERYCONTEXTATTRIBUTES |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_OS_QUERYREGISTRY |TBD| 
|PREEMPTIVE_OS_QUERYSECURITYCONTEXTTOKEN |TBD| 
|PREEMPTIVE_OS_REMOVEDIRECTORY |TBD| 
|PREEMPTIVE_OS_REPORTEVENT |TBD| 
|PREEMPTIVE_OS_REVERTTOSELF |TBD| 
|PREEMPTIVE_OS_RSFXDEVICEOPS |TBD| 
|PREEMPTIVE_OS_SECURITYOPS |TBD| 
|PREEMPTIVE_OS_SERVICEOPS |TBD| 
|PREEMPTIVE_OS_SETENDOFFILE |TBD| 
|PREEMPTIVE_OS_SETFILEPOINTER |TBD| 
|PREEMPTIVE_OS_SETFILEVALIDDATA |TBD| 
|PREEMPTIVE_OS_SETNAMEDSECURITYINFO |TBD| 
|PREEMPTIVE_OS_SQLCLROPS |TBD| 
|PREEMPTIVE_OS_SQMLAUNCH |TBD <br /> **S'applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] jusqu'à [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)]. |  
|PREEMPTIVE_OS_VERIFYSIGNATURE |TBD| 
|PREEMPTIVE_OS_VERIFYTRUST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_OS_VSSOPS |TBD| 
|PREEMPTIVE_OS_WAITFORSINGLEOBJECT |TBD| 
|PREEMPTIVE_OS_WINSOCKOPS |TBD| 
|PREEMPTIVE_OS_WRITEFILE |TBD| 
|PREEMPTIVE_OS_WRITEFILEGATHER |TBD| 
|PREEMPTIVE_OS_WSASETLASTERROR |TBD| 
|PREEMPTIVE_REENLIST |TBD| 
|PREEMPTIVE_RESIZELOG |TBD| 
|PREEMPTIVE_ROLLFORWARDREDO |TBD| 
|PREEMPTIVE_ROLLFORWARDUNDO |TBD| 
|PREEMPTIVE_SB_STOPENDPOINT |TBD| 
|PREEMPTIVE_SERVER_STARTUP |TBD| 
|PREEMPTIVE_SETRMINFO |TBD| 
|PREEMPTIVE_SHAREDMEM_GETDATA |TBD| 
|PREEMPTIVE_SNIOPEN |TBD| 
|PREEMPTIVE_SOSHOST |TBD| 
|PREEMPTIVE_SOSTESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_SP_SERVER_DIAGNOSTICS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_STARTRM |TBD| 
|PREEMPTIVE_STREAMFCB_CHECKPOINT |TBD| 
|PREEMPTIVE_STREAMFCB_RECOVER |TBD| 
|PREEMPTIVE_STRESSDRIVER |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_TESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_TRANSIMPORT |TBD| 
|PREEMPTIVE_UNMARSHALPROPAGATIONTOKEN |TBD| 
|PREEMPTIVE_VSS_CREATESNAPSHOT |TBD| 
|PREEMPTIVE_VSS_CREATEVOLUMESNAPSHOT |TBD| 
|PREEMPTIVE_XE_CALLBACKEXECUTE |TBD| 
|PREEMPTIVE_XE_CX_FILE_OPEN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_XE_CX_HTTP_CALL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PREEMPTIVE_XE_DISPATCHER |TBD| 
|PREEMPTIVE_XE_ENGINEINIT |TBD| 
|PREEMPTIVE_XE_GETTARGETSTATE |TBD| 
|PREEMPTIVE_XE_SESSIONCOMMIT |TBD| 
|PREEMPTIVE_XE_TARGETFINALIZE |TBD| 
|PREEMPTIVE_XE_TARGETINIT |TBD| 
|PREEMPTIVE_XE_TIMERRUN |TBD| 
|PREEMPTIVE_XETESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PRINT_ROLLBACK_PROGRESS |Utilisé pour attendre que des processus utilisateur se terminent dans une base de données qui a subi un changement d'état suite à l'utilisation de la clause de terminaison ALTER DATABASE. Pour plus d’informations, voir ALTER DATABASE (Transact-SQL).| 
|PRU_ROLLBACK_DEFERRED |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_ALL_COMPONENTS_INITIALIZED |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_COOP_SCAN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_DIRECTLOGCONSUMER_GETNEXT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_EVENT_SESSION_INIT_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_FABRIC_REPLICA_CONTROLLER_DATA_LOSS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_ACTION_COMPLETED |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC |Se produit lorsqu’une tâche en arrière-plan attend la fin de la tâche en arrière-plan qui reçoit (par interrogation) des notifications de Clustering de basculement Windows Server., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_CLUSTER_INTEGRATION |Ajout, de remplacer et/ou de supprimer une opération est en attente de saisir un verrou d’écriture sur une liste interne Always On (par exemple, une liste de réseaux, les adresses réseau ou les écouteurs de groupe de disponibilité). Usage interne uniquement, <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_FAILOVER_COMPLETED |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_JOIN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_OFFLINE_COMPLETED |Une Always On disponibilité groupe opération de suppression est en attente pour le groupe de disponibilité cible passe en mode hors connexion avant de détruire des objets de Clustering de basculement Windows Server., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_ONLINE_COMPLETED |Un Always On créer ou opération de groupe de disponibilité de basculement est en attente pour le groupe de disponibilité cible passe en ligne., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_POST_ONLINE_COMPLETED |Une Always On disponibilité groupe opération de suppression est en attente de l’arrêt d’une tâche en arrière-plan qui était planifiée dans le cadre d’une commande précédente. Par exemple, une tâche en arrière-plan peut effectuer la transition de bases de données de disponibilité vers le rôle principal. La DLL DROP AVAILABILITY GROUP doit attendre pour cette tâche en arrière-plan mettre fin à afin d’éviter des conditions de concurrence., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_SERVER_READY_CONNECTIONS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADR_WORKITEM_COMPLETED |Attente interne par un thread attendant la fin d'une tâche de travail asynchrone. Il s’agit d’une attente prévue et concerne l’utilisation de CSS., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_HADRSIM |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_LOG_CONSOLIDATION_IO |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_LOG_CONSOLIDATION_POLL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_MD_LOGIN_STATS |Se produit durant la synchronisation interne dans les métadonnées des statistiques de connexion., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_MD_RELATION_CACHE |Se produit durant la synchronisation interne dans les métadonnées de table ou un index., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_MD_SERVER_CACHE |Se produit durant la synchronisation interne dans les métadonnées sur les serveurs liés., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_MD_UPGRADE_CONFIG |Se produit durant la synchronisation interne dans la mise à niveau des configurations à l’échelle du serveur., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_PREEMPTIVE_APP_USAGE_TIMER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_PREEMPTIVE_AUDIT_ACCESS_WINDOWSLOG |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_QRY_BPMEMORY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_REPLICA_ONLINE_INIT_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_SBS_FILE_OPERATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_XTP_FSSTORAGE_MAINTENANCE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|PWAIT_XTP_HOST_STORAGE_WAIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_ASYNC_CHECK_CONSISTENCY_TASK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_ASYNC_PERSIST_TASK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_ASYNC_PERSIST_TASK_START |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_ASYNC_QUEUE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_BCKG_TASK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_BLOOM_FILTER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_CLEANUP_STALE_QUERIES_TASK_MAIN_LOOP_SLEEP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_CTXS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_DB_DISK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_DYN_VECTOR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_EXCLUSIVE_ACCESS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_HOST_INIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_LOADDB |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_PERSIST_TASK_MAIN_LOOP_SLEEP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_QDS_CAPTURE_INIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_SHUTDOWN_QUEUE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_STMT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_STMT_DISK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_TASK_SHUTDOWN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QDS_TASK_START |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QE_WARN_LIST_SYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QPJOB_KILL |Indique qu'une mise à jour asynchrone des statistiques automatiques a été annulée par un appel à KILL alors que la mise à jour commençait. Le thread de terminaison est suspendu, en attendant qu'il commence à écouter les commandes KILL. Une valeur idéale est inférieure à une seconde.| 
|QPJOB_WAITFOR_ABORT |Indique qu'une mise à jour asynchrone des statistiques automatiques a été annulée par un appel à KILL lors de son exécution. La mise à jour est maintenant terminée, mais elle est suspendue jusqu'à ce que la coordination du message de fin de thread soit achevée. Il s'agit d'un état ordinaire, mais rare, qui doit être très bref. Une valeur idéale est inférieure à une seconde.| 
|QRY_MEM_GRANT_INFO_MUTEX |Se produit lorsque la gestion de la mémoire pour l'exécution des requêtes essaie de contrôler l'accès à la liste d'informations d'octroi statique. Cet état énumère les informations sur les demandes de mémoire actuellement accordées et en attente. Il s'agit d'un simple état de contrôle d'accès. Il ne devrait jamais y avoir de longue attente sur cet état. Si ce mutex n'est pas libéré, toutes les nouvelles requêtes d'utilisation de mémoire cesseront de répondre.| 
|QRY_PARALLEL_THREAD_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QRY_PROFILE_LIST_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QUERY_ERRHDL_SERVICE_DONE |Identifié à titre d'information uniquement. Non pris en charge. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|QUERY_WAIT_ERRHDL_SERVICE |Identifié à titre d'information uniquement.  Non pris en charge. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement.  |  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN |Se produit dans certains cas lorsqu'une construction d'index hors connexion est exécutée en parallèle, et les différents threads de travail qui effectuent le tri synchronisent l'accès aux fichiers de tri.| 
|QUERY_NOTIFICATION_MGR_MUTEX |Se produit durant la synchronisation de la file d'attente de nettoyage de la mémoire dans le gestionnaire de notification de requête.| 
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX |Se produit durant la synchronisation de l'état des transactions dans les notifications de requêtes.| 
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX |Se produit durant la synchronisation interne dans le gestionnaire de notification de requête.| 
|QUERY_NOTIFICATION_UNITTEST_MUTEX |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|QUERY_OPTIMIZER_PRINT_MUTEX |Se produit durant la synchronisation de la production de la sortie de diagnostics de l'optimiseur de requête. Ce type d’attente se produit uniquement si les paramètres de diagnostic ont été activés sous la direction du Support technique Microsoft.| 
|QUERY_TASK_ENQUEUE_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|QUERY_TRACEOUT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|RBIO_WAIT_VLF |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|RECOVER_CHANGEDB |Se produit durant la synchronisation de l'état de la base de données dans une base de données en mode secours semi-automatique.| 
|RECOVERY_MGR_LOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REDO_THREAD_PENDING_WORK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REDO_THREAD_SYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REMOTE_BLOCK_IO |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REMOTE_DATA_ARCHIVE_MIGRATION_DMV |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REMOTE_DATA_ARCHIVE_SCHEMA_DMV |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REMOTE_DATA_ARCHIVE_SCHEMA_TASK_QUEUE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REPL_CACHE_ACCESS |Se produit durant la synchronisation sur un cache des articles de réplication. Lors de ces attentes, l'utilitaire de lecture du journal des réplications se bloque et les instructions de langage de définition de données (DDL - Data Definition Language) sur une table publiée sont bloquées.| 
|REPL_HISTORYCACHE_ACCESS |TBD| 
|REPL_SCHEMA_ACCESS |Se produit durant la synchronisation des informations de version du schéma de réplication. Cet état existe lorsque des instructions DDL sont exécutées sur l'objet répliqué et lorsque l'utilitaire de lecture du journal crée ou exploite le schéma avec version reposant sur une occurrence de DDL. Si vous avez plusieurs bases de données publiées sur un serveur de publication avec la réplication transactionnelle et de bases de données publiées sont très actifs, conflits peuvent être consultés sur ce type d’attente.| 
|REPL_TRANFSINFO_ACCESS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REPL_TRANHASHTABLE_ACCESS |TBD| 
|REPL_TRANTEXTINFO_ACCESS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|REPLICA_WRITES |Se produit lorsqu'une tâche attend la fin des écritures de page dans des instantanés de base de données ou des réplicas DBCC.| 
|REQUEST_DISPENSER_PAUSE |Se produit lorsqu'une tâche attend la fin de toutes les E/S en suspens, afin que les E/S puissent être figées dans un fichier pour une sauvegarde des instantanés.| 
|REQUEST_FOR_DEADLOCK_SEARCH |Se produit lorsque l'analyseur de blocages attend pour démarrer la recherche de blocage suivante. Cette attente est normale entre les détections de blocages, et une importante durée d'attente totale sur cette ressource n'indique pas la présence d'un problème.| 
|RESERVED_MEMORY_ALLOCATION_EXT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|RESMGR_THROTTLED |Se produit lorsqu'une nouvelle demande arrive et qu'elle est accélérée en fonction du paramètre GROUP_MAX_REQUESTS.| 
|RESOURCE_GOVERNOR_IDLE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|RESOURCE_QUEUE |Se produit durant la synchronisation de diverses files d'attente de ressources internes.| 
|RESOURCE_SEMAPHORE |Se produit lorsqu'une demande de mémoire de requête ne peut pas être accordée immédiatement en raison d'autres requêtes simultanées. Des temps d'attente élevés peuvent indiquer un trop grand nombre de requêtes simultanées ou des quantités de demande de mémoire trop importantes.| 
|RESOURCE_SEMAPHORE_MUTEX |Se produit lorsqu'une requête attend que sa demande de réservation de thread soit exécutée. Se produit également lors de la synchronisation des demandes d'allocation de mémoire et de compilation de requête.| 
|RESOURCE_SEMAPHORE_QUERY_COMPILE |Se produit lorsque le nombre de compilations de requêtes simultanées atteint une limite. Des temps d'attente élevés peuvent indiquer des compilations et des recompilations excessives, ou encore des plans ne pouvant pas être mis en cache.| 
|RESOURCE_SEMAPHORE_SMALL_QUERY |Se produit lorsqu'une demande de mémoire émanant d'une petite requête ne peut pas être accordée immédiatement en raison d'autres requêtes simultanées. Le temps d'attente ne doit pas excéder quelques secondes, car le serveur transfère la demande au pool de mémoire de requête principal s'il ne parvient pas à accorder la mémoire demandée dans les secondes qui suivent. Des temps d'attente élevés peuvent indiquer un nombre excessif de petites requêtes simultanées alors que le pool de mémoire principal est bloqué par les requêtes en attente. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|RESTORE_FILEHANDLECACHE_ENTRYLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|RESTORE_FILEHANDLECACHE_LOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|RG_RECONFIG |TBD| 
|ROWGROUP_OP_STATS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|ROWGROUP_VERSION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|RTDATA_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SATELLITE_CARGO |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SATELLITE_SERVICE_SETUP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SATELLITE_TASK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SBS_DISPATCH |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SBS_RECEIVE_TRANSPORT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SBS_TRANSPORT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SCAN_CHAR_HASH_ARRAY_INITIALIZATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SEC_DROP_TEMP_KEY |Se produit après l'échec d'une tentative de suppression d'une clé de sécurité temporaire avant une nouvelle tentative.| 
|SECURITY_CNG_PROVIDER_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SECURITY_CRYPTO_CONTEXT_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SECURITY_DBE_STATE_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SECURITY_KEYRING_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SECURITY_MUTEX |Se produit lorsqu'il y a une attente de mutex qui contrôlent l'accès à la liste globale de fournisseurs de services de chiffrement EKM (Gestion de clés extensible) et la liste au niveau de la session des sessions EKM.| 
|SECURITY_RULETABLE_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SEMPLAT_DSI_BUILD |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SEQUENCE_GENERATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SEQUENTIAL_GUID |Se produit lorsqu'un nouveau GUID séquentiel est obtenu.| 
|SERVER_IDLE_CHECK |Se produit durant la synchronisation de l’état inactif de l’instance SQL Server lorsqu’un moniteur de ressource est essaie de déclarer une instance de SQL Server comme étant inactive ou lors de la tentative sortir de veille.| 
|SERVER_RECONFIGURE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SESSION_WAIT_STATS_CHILDREN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SHARED_DELTASTORE_CREATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SHUTDOWN |Se produit lorsqu'une instruction d'arrêt attend que les connexions actives soient coupées.| 
|SLEEP_BPOOL_FLUSH |Se produit lorsqu'un point de vérification limite l'émission des nouvelles E/S afin de ne pas saturer le sous-système de disque.| 
|SLEEP_BUFFERPOOL_HELPLW |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SLEEP_DBSTARTUP |Se produit au démarrage de la base de données lors de l'attente de la récupération de toutes les bases de données.| 
|SLEEP_DCOMSTARTUP |Se produit une fois au maximum durant le démarrage de l’instance SQL Server en attendant la fin de l’initialisation de DCOM.| 
|SLEEP_MASTERDBREADY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SLEEP_MASTERMDREADY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SLEEP_MASTERUPGRADED |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SLEEP_MEMORYPOOL_ALLOCATEPAGES |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SLEEP_MSDBSTARTUP |Se produit lorsque Trace SQL attend la fin du démarrage de la base de données msdb.| 
|SLEEP_RETRY_VIRTUALALLOC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SLEEP_SYSTEMTASK |Se produit lors du démarrage d'une tâche en arrière-plan pendant l'attente du démarrage de tempdb.| 
|SLEEP_TASK |Se produit lorsqu'une tâche est en état de veille en attendant qu'un événement générique survienne.| 
|SLEEP_TEMPDBSTARTUP |Se produit lorsqu'une tâche attend la fin du démarrage de tempdb.| 
|SLEEP_WORKSPACE_ALLOCATEPAGE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SLO_UPDATE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SMSYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SNI_CONN_DUP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SNI_CRITICAL_SECTION |Se produit durant la synchronisation interne au sein des composants de mise en réseau de SQL Server.| 
|SNI_HTTP_WAITFOR_0_DISCON |Se produit lors de l’arrêt de SQL Server, en attendant que les connexions HTTP en suspens.| 
|SNI_LISTENER_ACCESS |Se produit pendant l'attente de mise à jour de la modification d'état des nœuds NUMA (Non-Uniform Memory Access). L'accès à la modification d'état est sérialisé.| 
|SNI_TASK_COMPLETION |Se produit lorsqu'il y a une attente de fin de toutes les tâches pendant la modification d'état d'un nœud NUMA.| 
|SNI_WRITE_ASYNC |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SOAP_READ |Se produit durant l'attente de l'exécution d'une lecture sur le réseau HTTP.| 
|SOAP_WRITE |Se produit durant l'attente de l'exécution d'une écriture sur le réseau HTTP.| 
|SOCKETDUPLICATEQUEUE_CLEANUP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SOS_CALLBACK_REMOVAL |Se produit lors de la synchronisation sur une liste de rappels afin de supprimer un rappel. Ce compteur ne change pas à la fin de l'initialisation du serveur.| 
|SOS_DISPATCHER_MUTEX |Se produit durant la synchronisation interne du pool de répartiteurs. Cela inclut l'ajustement du pool.| 
|SOS_LOCALALLOCATORLIST |Se produit durant la synchronisation interne dans le gestionnaire de mémoire [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SOS_MEMORY_TOPLEVELBLOCKALLOCATOR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SOS_MEMORY_USAGE_ADJUSTMENT |Se produit lorsque l'utilisation de la mémoire est répartie entre des pools.| 
|SOS_OBJECT_STORE_DESTROY_MUTEX |Se produit durant la synchronisation interne dans les pools de mémoire lorsque des objets du pool sont détruits.| 
|SOS_PHYS_PAGE_CACHE |Tient compte de la durée d'attente d'un thread pour acquérir le mutex qu'il doit obtenir avant de pouvoir allouer des pages physiques ou avant de retourner ces pages au système d'exploitation. Attentes sur ce type s’affichent uniquement si l’instance de SQL Server utilise la mémoire AWE., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SOS_PROCESS_AFFINITY_MUTEX |Se produit durant la synchronisation de l'accès pour traiter les paramètres d'affinité.| 
|SOS_RESERVEDMEMBLOCKLIST |Se produit durant la synchronisation interne dans le Gestionnaire de mémoire de SQL Server. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SOS_SCHEDULER_YIELD |Se produit lorsqu'une tâche abandonne volontairement le planificateur pour d'autres tâches à exécuter. Durant cette attente, la tâche attend le renouvellement de son quantum.| 
|SOS_SMALL_PAGE_ALLOC |Se produit pendant l'allocation et la libération de la mémoire gérée par quelques objets mémoire.| 
|SOS_STACKSTORE_INIT_MUTEX |Se produit durant la synchronisation de l'initialisation de stockage interne.| 
|SOS_SYNC_TASK_ENQUEUE_EVENT |Se produit lorsqu'une tâche est démarrée de manière synchrone. La plupart des tâches dans SQL Server sont démarrés de manière asynchrone, dans lequel contrôle retourne à l’élément initial dès que la tâche a été placée sur la file d’attente de travail.| 
|SOS_VIRTUALMEMORY_LOW |Se produit lorsqu'une allocation de mémoire attend qu'un gestionnaire de ressources libère de la mémoire virtuelle.| 
|SOSHOST_EVENT |Se produit lorsqu’un composant hébergé, tel que CLR, attend sur un objet de synchronisation d’événement SQL Server.| 
|SOSHOST_INTERNAL |Se produit durant la synchronisation des rappels du gestionnaire de mémoire utilisés par des composants hébergés, tels que CLR.| 
|SOSHOST_MUTEX |Se produit lorsqu’un composant hébergé, tel que CLR, attend sur un objet de synchronisation mutex SQL Server.| 
|SOSHOST_RWLOCK |Se produit lorsqu’un composant hébergé, tel que CLR, attend sur un objet de synchronisation de lecture / écriture SQL Server.| 
|SOSHOST_SEMAPHORE |Se produit lorsqu’un composant hébergé, tel que CLR, attend sur un objet de synchronisation de sémaphore de SQL Server.| 
|SOSHOST_SLEEP |Se produit lorsqu'une tâche hébergée est en veille en attendant qu'un événement générique survienne. Les tâches hébergées sont utilisées par les composants hébergés tels que CLR.| 
|SOSHOST_TRACELOCK |Se produit durant la synchronisation de l'accès aux flux de trace.| 
|SOSHOST_WAITFORDONE |Se produit lorsqu'un composant hébergé, tel que CLR, attend la fin d'une tâche.| 
|SP_PREEMPTIVE_SERVER_DIAGNOSTICS_SLEEP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SP_SERVER_DIAGNOSTICS_BUFFER_ACCESS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SP_SERVER_DIAGNOSTICS_INIT_MUTEX |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SP_SERVER_DIAGNOSTICS_SLEEP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SQLCLR_APPDOMAIN |Se produit lorsque CLR attend la fin du démarrage d'un domaine d'application.| 
|SQLCLR_ASSEMBLY |Se produit durant l'attente de l'accès à la liste des assembly chargés dans le domaine d'application.| 
|SQLCLR_DEADLOCK_DETECTION |Se produit lorsque CLR attend la fin d'une détection d'interblocage.| 
|SQLCLR_QUANTUM_PUNISHMENT |Se produit lorsqu'une tâche CLR est accélérée car elle a dépassé son quantum d'exécution. Cette accélération est opérée afin de limiter l'incidence de cette tâche consommant une grande quantité de ressources sur les autres tâches.| 
|SQLSORT_NORMMUTEX |Se produit durant la synchronisation interne, lors de l'initialisation des structures de tri internes.| 
|SQLSORT_SORTMUTEX |Se produit durant la synchronisation interne, lors de l'initialisation des structures de tri internes.| 
|SQLTRACE_BUFFER_FLUSH |Se produit lorsqu'une tâche attend qu'une tâche en arrière-plan vide les tampons de traçage sur le disque toutes les quatre secondes. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SQLTRACE_FILE_BUFFER |Se produit durant la synchronisation de mémoires tampons de trace lors d’un fichier de trace., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SQLTRACE_FILE_READ_IO_COMPLETION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SQLTRACE_FILE_WRITE_IO_COMPLETION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SQLTRACE_INCREMENTAL_FLUSH_SLEEP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SQLTRACE_LOCK |TBD <br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SQLTRACE_PENDING_BUFFER_WRITERS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|SQLTRACE_SHUTDOWN |Se produit pendant que l'arrêt de la trace attend la fin des événements de trace en suspens.| 
|SQLTRACE_WAIT_ENTRIES |Se produit lorsqu'une file d'attente d'événements Trace SQL attend l'arrivée de paquets dans la file d'attente.| 
|SRVPROC_SHUTDOWN |Se produit lorsque le processus d'arrêt attend la libération des ressources internes pour que l'arrêt s'effectue correctement.| 
|STARTUP_DEPENDENCY_MANAGER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|TDS_BANDWIDTH_STATE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|TDS_INIT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|TDS_PROXY_CONTAINER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|TEMPOBJ |Se produit lorsque des suppressions d'objets temporaires sont synchronisées. Cette attente est rare ; elle survient uniquement si une tâche a demandé un accès exclusif pour les suppressions de tables temp.| 
|TEMPORAL_BACKGROUND_PROCEED_CLEANUP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|TERMINATE_LISTENER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|THREADPOOL |Se produit lorsqu'une tâche attend un thread de travail pour l'exécution. Ceci peut indiquer qu'un paramètre de thread de travail maximal est trop bas ou que les exécutions de traitement sont exceptionnellement longues, ce qui réduit le nombre de threads de travail disponibles pour répondre aux autres lots.| 
|TIMEPRIV_TIMEPERIOD |Se produit durant la synchronisation interne du minuteur d'événements étendus.| 
|TRACE_EVTNOTIF |TBD| 
|TRACEWRITE |Se produit lorsque le fournisseur de traces d'ensemble de lignes Trace SQL attend le traitement d'un tampon libre ou d'un tampon avec des événements.| 
|TRAN_MARKLATCH_DT |Se produit lors de l'attente d'un verrou en mode de destruction sur un verrou interne de marque de transaction. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_EX |Se produit lors de l'attente d'un verrou en mode exclusif sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_KP |Se produit lors de l'attente d'un verrou en mode de conservation sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|TRAN_MARKLATCH_SH |Se produit lors de l'attente d'un verrou en mode partagé sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_UP |Se produit lors de l'attente d'un verrou en mode de mise à jour sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRANSACTION_MUTEX |Se produit durant la synchronisation de l'accès à une transaction par plusieurs traitements.| 
|UCS_ENDPOINT_CHANGE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|UCS_MANAGER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|UCS_MEMORY_NOTIFICATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|UCS_SESSION_REGISTRATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|UCS_TRANSPORT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|UCS_TRANSPORT_STREAM_CHANGE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|UTIL_PAGE_ALLOC |Se produit lorsque les analyses des journaux des transactions attendent que de la mémoire soit disponible lors de la sollicitation de la mémoire.| 
|VDI_CLIENT_COMPLETECOMMAND |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|VDI_CLIENT_GETCOMMAND |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|VDI_CLIENT_OPERATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|VDI_CLIENT_OTHER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|VERSIONING_COMMITTING |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|VIA_ACCEPT |Se produit lorsqu'une connexion du fournisseur VIA (Virtual Interface Adapter) est terminée au cours du démarrage.| 
|VIEW_DEFINITION_MUTEX |Se produit durant la synchronisation d'accès aux définitions des vues mises en cache.| 
|WAIT_FOR_RESULTS |Se produit durant l'attente du déclenchement d'une notification de requête.| 
|WAIT_SCRIPTDEPLOYMENT_REQUEST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_SCRIPTDEPLOYMENT_WORKER |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XLOGREAD_SIGNAL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_ASYNC_TX_COMPLETION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_CKPT_AGENT_WAKEUP |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_CKPT_CLOSE |Se produit pendant l’attente d’un point de contrôle., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_CKPT_ENABLED |Se produit lorsque les points de contrôle est désactivé et en attente pour les points de contrôle doit être activé., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_CKPT_STATE_LOCK |Se produit lors de la synchronisation de la vérification de l’état du point de contrôle., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_COMPILE_WAIT |TBD <br /> **S’APPLIQUE À** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_GUEST |Se produit lorsque l’allocateur de mémoire de base de données doit cesser de recevoir des notifications de mémoire insuffisante., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_HOST_WAIT |Se produit lorsque les attentes sont déclenchées par le moteur de base de données et implémentées par l’hôte., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_OFFLINE_CKPT_BEFORE_REDO |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_OFFLINE_CKPT_LOG_IO |Se produit lorsque le point de contrôle hors connexion est en attente pour un journal de lecture e/s pour terminer., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_OFFLINE_CKPT_NEW_LOG |Se produit lorsque le point de contrôle hors connexion est en attente pour les nouveaux enregistrements de journal à analyser., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_PROCEDURE_ENTRY |Se produit lorsqu’une procédure de suppression est en attente pour toutes les exécutions en cours de cette procédure pour terminer., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_RECOVERY |Se produit lors de la récupération de la base de données est en attente pour la récupération des objets optimisés en mémoire à la fin., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_SERIAL_RECOVERY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_SWITCH_TO_INACTIVE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_TASK_SHUTDOWN |Se produit lors de l’attente d’un thread OLTP en mémoire terminer., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAIT_XTP_TRAN_DEPENDENCY |Se produit lors de l’attente des dépendances de transaction., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAITFOR |Se produit suite à une instruction WAITFOR Transact-SQL. La durée de l'attente est déterminée par les paramètres de l'instruction. Il s'agit d'une attente initialisée par l'utilisateur.| 
|WAITFOR_PER_QUEUE |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WAITFOR_TASKSHUTDOWN |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|WAITSTAT_MUTEX |Se produit durant la synchronisation d'accès à la collection de statistiques utilisées pour remplir sys.dm_os_wait_stats.| 
|WCC |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|WINDOW_AGGREGATES_MULTIPASS |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WINFAB_API_CALL |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WINFAB_REPLICA_BUILD_OPERATION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WINFAB_REPORT_FAULT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|WORKTBL_DROP |Se produit lors d'une suspension avant une nouvelle tentative, suite à l'échec de la suppression d'une table de travail.| 
|WRITE_COMPLETION |Se produit lorsqu'une opération d'écriture est en cours.| 
|WRITELOG |Se produit lors de l'attente d'un vidage du journal. Les opérations courantes qui provoquent des vidages du journal sont les points de vérification et les validations des transactions.| 
|XACT_OWN_TRANSACTION |Se produit pendant l'attente de l'obtention de la propriété d'une transaction.| 
|XACT_RECLAIM_SESSION |Se produit lorsque vous attendez que le propriétaire actuel d'une session libère la propriété de la session.| 
|XACTLOCKINFO |Se produit durant la synchronisation de l'accès à la liste des verrous d'une transaction. Outre la transaction proprement dite, la liste des verrous est accessible par le biais d'opérations telles que la détection d'interblocages et la migration des verrous durant les fractionnements de pages.| 
|XACTWORKSPACE_MUTEX |Se produit durant la synchronisation des désinscriptions à partir d'une transaction et du nombre de verrous de base de données entre les membres d'inscription d'une transaction.| 
|XDB_CONN_DUP_HASH |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XDES_HISTORY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XDES_OUT_OF_ORDER_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XDES_SNAPSHOT |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XDESTSVERMGR |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XE_BUFFERMGR_ALLPROCESSED_EVENT |Se produit lorsque les mémoires tampons de session d'Événements étendus sont vidées sur les cibles. Cette attente se produit dans un thread d'arrière-plan.| 
|XE_BUFFERMGR_FREEBUF_EVENT |Se produit lorsqu'une des conditions suivantes est remplie :| 
|XE_CALLBACK_LIST |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XE_CX_FILE_READ |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XE_DISPATCHER_CONFIG_SESSION_LIST |Se produit lorsqu'une session Événements étendus qui utilise des cibles asynchrones est démarrée ou arrêtée. Cette attente signifie que :| 
|XE_DISPATCHER_JOIN |Se produit lorsqu'un thread d'arrière-plan utilisé pour les sessions Événements étendus est arrêté.| 
|XE_DISPATCHER_WAIT |Se produit lorsqu'un thread d'arrière-plan utilisé pour les sessions Événements étendus attend le traitement des tampons d'événements.| 
|XE_FILE_TARGET_TVF |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XE_LIVE_TARGET_TVF |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XE_MODULEMGR_SYNC |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|XE_OLS_LOCK |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|XE_PACKAGE_LOCK_BACKOFF |Identifié à titre d'information uniquement. Non pris en charge. <br /> **S’applique aux**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|XE_SERVICES_EVENTMANUAL |TBD| 
|XE_SERVICES_MUTEX |TBD| 
|XE_SERVICES_RWLOCK |TBD| 
|XE_SESSION_CREATE_SYNC |TBD| 
|XE_SESSION_FLUSH |TBD| 
|XE_SESSION_SYNC |TBD| 
|XE_STM_CREATE |TBD| 
|XE_TIMER_EVENT |TBD| 
|XE_TIMER_MUTEX |TBD| 
|XE_TIMER_TASK_DONE |TBD| 
|XIO_CREDENTIAL_MGR_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XIO_CREDENTIAL_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XIO_EDS_MGR_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XIO_EDS_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XIO_IOSTATS_BLOBLIST_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XIO_IOSTATS_FCBLIST_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XIO_LEASE_RENEW_MGR_RWLOCK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XTP_HOST_DB_COLLECTION |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XTP_HOST_LOG_ACTIVITY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XTP_HOST_PARALLEL_RECOVERY |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XTP_PREEMPTIVE_TASK |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XTP_TRUNCATION_LSN |TBD <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XTPPROC_CACHE_ACCESS |Se produit lors de l’accès à tous les objets du cache de procédure stockée compilée en mode natif., <br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
|XTPPROC_PARTITIONED_STACK_CREATE |Se produit lors de l’allocation par nœud NUMA en mode natif compilé des structures de cache de procédure stockée (doit être effectué un thread unique) pour une procédure donnée., <br /> **S'applique à**: [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|

  
 Les événements XEvent suivants sont liés pour partitionner **commutateur** et de reconstruction de l’index en ligne. Pour plus d’informations sur la syntaxe, consultez [ALTER TABLE &#40;Transact-SQL&#41; ](../../t-sql/statements/alter-table-transact-sql.md) et [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 Pour une matrice de compatibilité de verrouillage, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
    
 [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_session_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md)   
 [Sys.dm_db_wait_stats &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database.md)  
  
  


