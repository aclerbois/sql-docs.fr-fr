---
title: sp_server_diagnostics (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_server_diagnostics
- sp_server_diagnostics_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_server_diagnostics
ms.assetid: 62658017-d089-459c-9492-c51e28f60efe
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 181b5ba51d87db43392af280d8fc4178da54dcc4
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spserverdiagnostics-transact-sql"></a>sp_server_diagnostics (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

Capture des données de diagnostics et des informations d'intégrité à propos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour détecter des échecs potentiels. La procédure fonctionne en mode de répétition et envoie régulièrement des résultats. Elle peut être appelée depuis une connexion DAC ou ordinaire.  
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).  
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_server_diagnostics [@repeat_interval =] 'repeat_interval_in_seconds'   
```  
  
## <a name="arguments"></a>Arguments  
 [ **@repeat_interval** =] **'***repeat_interval_in_seconds***'**  
 Indique l'intervalle de temps auquel la procédure stockée s'exécutera de façon répétée pour envoyer les informations d'intégrité.  
  
 *repeat_interval_in_seconds* est **int** avec la valeur par défaut 0. Les valeurs de paramètre valides sont 0, ou toute valeur égale à ou supérieure à 5. La procédure stockée doit s'exécuter au moins 5 secondes pour retourner des données complètes. La valeur minimale pour que la procédure stockée s'exécute en mode de répétition est de 5 secondes.  
  
 Si ce paramètre n'est pas spécifié, ou si la valeur spécifiée est 0, la procédure stockée retournera des données une fois puis s'arrêtera.  
  
 Si la valeur spécifiée est inférieure à la valeur minimale, une erreur sera générée et rien ne sera retourné.  
  
 Si la valeur spécifiée est supérieure ou égale à 5, la procédure stockée s'exécute à plusieurs reprises pour retourner l'état d'intégrité jusqu'à ce qu'elle soit annulée manuellement.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
**sp_server_diagnostics** renvoie les informations suivantes  
  
|Colonne|Data type| Description|  
|------------|---------------|-----------------|  
|**creation_time**|**datetime**|Indique l'horodateur de la création de ligne. Chaque ligne dans un ensemble de lignes unique a le même horodateur.|  
|**component_type**|**sysname**|Indique si la ligne contient des informations pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au niveau du composant ou d’un groupe de disponibilité Always On de l’instance :<br /><br /> instance<br /><br /> Always On : groupe de disponibilité|  
|**nom du composant**|**sysname**|Indique le nom du composant ou le nom du groupe de disponibilité :<br /><br /> système<br /><br /> ressource<br /><br /> query_processing<br /><br /> io_subsystem<br /><br /> événements<br /><br /> *\<nom du groupe de disponibilité >*|  
|**state**|**int**|Indique l'état d'intégrité du composant :<br /><br /> 0<br /><br /> 1<br /><br /> 2<br /><br /> 3|  
|**state_desc**|**sysname**|Décrit la colonne d'état. Les descriptions qui correspondent aux valeurs dans la colonne d'état sont :<br /><br /> 0 : inconnu<br /><br /> 1 : nettoyer<br /><br /> 2 : avertissement<br /><br /> 3 : erreur|  
|**data**|**varchar (max)**|Spécifie des données spécifiques au composant.|  
  
 Voici les descriptions des cinq composants :  
  
-   **système**: recueille des données à partir d’un point de vue du système sur les verrouillages spinlock, conditions de traitement sévères, tâches improductives, défauts de page et l’utilisation du processeur. Ces informations permettent d'obtenir une recommandation de l'état d'intégrité global.  
  
-   **ressource**: collecte des données à partir d’un point de vue des ressources sur les pages de mémoire physique et virtuelle, les pools de mémoires tampons, le cache et autres objets en mémoire. Ces informations produit une recommandation d’état d’intégrité global.  
  
-   **query_processing**: attente de collecte des données à partir d’un point de vue du traitement des requêtes sur les threads de travail, tâches, types, les sessions sollicitant beaucoup l’UC et les tâches de blocage. Ces informations produit une recommandation d’état d’intégrité global.  
  
-   **io_subsystem**: collecte des données sur les e/s. En plus des données de diagnostics, ce composant produit un état d'intégrité sain ou un état d'intégrité d'avertissement uniquement pour un sous-système d'E/S.  
  
-   **événements**: collecte des données et des surfaces via la procédure stockée sur les erreurs et les événements d’intérêt enregistrés par le serveur, notamment des détails sur les exceptions de mémoire tampon en anneau, en anneau des événements de mémoire tampon sur l’allocation de mémoire, la mémoire, le Moniteur du planificateur pool de mémoires tampons, verrouillages spinlock, sécurité et la connectivité. Les événements afficheront toujours 0 comme état.  
  
-   **\<nom du groupe de disponibilité >**: collecte des données pour le groupe de disponibilité spécifié (si component_type = « toujours sur : AvailabilityGroup »).  
  
## <a name="remarks"></a>Notes  
Du point de vue d'un échec, les composant system, resource et query_processing seront exploités pour la détection de pannes, tandis que les composants io_subsystem et events le seront uniquement à des fins de diagnostics.  
  
Le tableau suivant mappe les composants à leurs états d'intégrité associés.  
  
|Components|Bon état (1)|Avertissement (2)|Erreur (3)|Inconnu (0)|  
|----------------|-----------------|-------------------|-----------------|--------------------|  
|système|x|x|x||  
|ressource|x|x|x||  
|query_processing|x|x|x||  
|io_subsystem|x|x|||  
|événements||||x|  
  
Le (x) dans chaque ligne représente des états d'intégrité valides pour le composant. Par exemple, io_subsystem indiquera un bon état ou un avertissement. Il n'affichera pas les états d'erreur.  
 
> [!NOTE]
> L’exécution de la procédure interne sp_server_diagnostics est implémentée sur un thread préemptif à priorité élevée.
  
## <a name="permissions"></a>Autorisations  
requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
Il est recommandé d'utiliser les sessions étendues pour capturer les informations d'intégrité et de les enregistrer dans un fichier situé en dehors de SQL Server. Par conséquent, vous pouvez toujours y accéder en cas d'échec. L'exemple suivant enregistre la sortie d'une session d'événement dans un fichier :  
```sql  
CREATE EVENT SESSION [diag]  
ON SERVER  
           ADD EVENT [sp_server_diagnostics_component_result] (set collect_data=1)  
           ADD TARGET [asynchronous_file_target] (set filename='c:\temp\diag.xel');  
GO  
ALTER EVENT SESSION [diag]  
      ON SERVER STATE = start;  
GO  
```  
  
La requête d'exemple ci-dessous lit le fichier journal de session étendue :  
```sql  
SELECT  
    xml_data.value('(/event/@name)[1]','varchar(max)') AS Name  
  , xml_data.value('(/event/@package)[1]', 'varchar(max)') AS Package  
  , xml_data.value('(/event/@timestamp)[1]', 'datetime') AS 'Time'  
  , xml_data.value('(/event/data[@name=''component_type'']/value)[1]','sysname') AS Sysname  
  , xml_data.value('(/event/data[@name=''component_name'']/value)[1]','sysname') AS Component  
  , xml_data.value('(/event/data[@name=''state'']/value)[1]','int') AS State  
  , xml_data.value('(/event/data[@name=''state_desc'']/value)[1]','sysname') AS State_desc  
  , xml_data.query('(/event/data[@name="data"]/value/*)') AS Data  
FROM   
(  
      SELECT  
                        object_name as event  
                        ,CONVERT(xml, event_data) as xml_data  
       FROM    
      sys.fn_xe_file_target_read_file('C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log\*.xel', NULL, NULL, NULL)  
)   
AS XEventData  
ORDER BY time;  
```  
  
L'exemple suivant capture la sortie de sp_server_diagnostics dans une table dans un mode de non-répétition :  
```sql  
CREATE TABLE SpServerDiagnosticsResult  
(  
      create_time DateTime,  
      component_type sysname,  
      component_name sysname,  
      state int,  
      state_desc sysname,  
      data xml  
);  
INSERT INTO SpServerDiagnosticsResult  
EXEC sp_server_diagnostics; 
```  

L’exemple de requête ci-dessous lit la sortie sous forme de résumé de la table :  
```sql  
SELECT create_time,
       component_name,
       state_desc 
FROM SpServerDiagnosticsResult;  
``` 

L’exemple de requête ci-dessous lit une partie de la sortie détaillée de chaque composant dans la table :  
```sql  
-- system
select data.value('(/system/@systemCpuUtilization)[1]','bigint') as 'System_CPU',
   data.value('(/system/@sqlCpuUtilization)[1]','bigint') as 'SQL_CPU',
   data.value('(/system/@nonYieldingTasksReported)[1]','bigint') as 'NonYielding_Tasks',
   data.value('(/system/@pageFaults)[1]','bigint') as 'Page_Faults',
   data.value('(/system/@latchWarnings)[1]','bigint') as 'Latch_Warnings',
   data.value('(/system/@BadPagesDetected)[1]','bigint') as 'BadPages_Detected',
   data.value('(/system/@BadPagesFixed)[1]','bigint') as 'BadPages_Fixed'
from SpServerDiagnosticsResult 
where component_name like 'system'
go

-- Resource Monitor
select data.value('(./Record/ResourceMonitor/Notification)[1]', 'VARCHAR(max)') AS [Notification],
    data.value('(/resource/memoryReport/entry[@description=''Working Set'']/@value)[1]', 'bigint')/1024 AS [SQL_Mem_in_use_MB],
    data.value('(/resource/memoryReport/entry[@description=''Available Paging File'']/@value)[1]', 'bigint')/1024 AS [Avail_Pagefile_MB],
    data.value('(/resource/memoryReport/entry[@description=''Available Physical Memory'']/@value)[1]', 'bigint')/1024 AS [Avail_Physical_Mem_MB],
    data.value('(/resource/memoryReport/entry[@description=''Available Virtual Memory'']/@value)[1]', 'bigint')/1024 AS [Avail_VAS_MB],
    data.value('(/resource/@lastNotification)[1]','varchar(100)') as 'LastNotification',
    data.value('(/resource/@outOfMemoryExceptions)[1]','bigint') as 'OOM_Exceptions'
from SpServerDiagnosticsResult 
where component_name like 'resource'
go

-- Nonpreemptive waits
select waits.evt.value('(@waitType)','varchar(100)') as 'Wait_Type',
   waits.evt.value('(@waits)','bigint') as 'Waits',
   waits.evt.value('(@averageWaitTime)','bigint') as 'Avg_Wait_Time',
   waits.evt.value('(@maxWaitTime)','bigint') as 'Max_Wait_Time'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/topWaits/nonPreemptive/byDuration/wait') AS waits(evt)
where component_name like 'query_processing'
go

-- Preemptive waits
select waits.evt.value('(@waitType)','varchar(100)') as 'Wait_Type',
   waits.evt.value('(@waits)','bigint') as 'Waits',
   waits.evt.value('(@averageWaitTime)','bigint') as 'Avg_Wait_Time',
   waits.evt.value('(@maxWaitTime)','bigint') as 'Max_Wait_Time'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/topWaits/preemptive/byDuration/wait') AS waits(evt)
where component_name like 'query_processing'
go

-- CPU intensive requests
select cpureq.evt.value('(@sessionId)','bigint') as 'SessionID',
   cpureq.evt.value('(@command)','varchar(100)') as 'Command',
   cpureq.evt.value('(@cpuUtilization)','bigint') as 'CPU_Utilization',
   cpureq.evt.value('(@cpuTimeMs)','bigint') as 'CPU_Time_ms'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/cpuIntensiveRequests/request') AS cpureq(evt)
where component_name like 'query_processing'
go

-- Blocked Process Report
select blk.evt.query('.') as 'Blocked_Process_Report_XML'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/queryProcessing/blockingTasks/blocked-process-report') AS blk(evt)
where component_name like 'query_processing'
go

-- IO
select data.value('(/ioSubsystem/@ioLatchTimeouts)[1]','bigint') as 'Latch_Timeouts',
   data.value('(/ioSubsystem/@totalLongIos)[1]','bigint') as 'Total_Long_IOs'
from SpServerDiagnosticsResult 
where component_name like 'io_subsystem'
go

-- Event information
select xevts.evt.value('(@name)','varchar(100)') as 'xEvent_Name',
   xevts.evt.value('(@package)','varchar(100)') as 'Package',
   xevts.evt.value('(@timestamp)','datetime') as 'xEvent_Time',
   xevts.evt.query('.') as 'Event Data'
from SpServerDiagnosticsResult 
    CROSS APPLY data.nodes('/events/session/RingBufferTarget/event') AS xevts(evt)
where component_name like 'events'
go  
``` 
  
## <a name="see-also"></a>Voir aussi  
 [Stratégie de basculement pour les instances de cluster de basculement](../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
  
