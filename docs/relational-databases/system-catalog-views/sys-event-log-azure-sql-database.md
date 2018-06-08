---
title: Sys.event_log (base de données de SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-catalog-views
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- event_log
- sys.event_log_TSQL
- event_log_TSQL
- sys.event_log
dev_langs:
- TSQL
helpviewer_keywords:
- event_log
- sys.event_log
ms.assetid: ad5496b5-e5c7-4a18-b5a0-3f985d7c4758
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: 5f871501b63bd9823c5a44872224439e8df07a9c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syseventlog-azure-sql-database"></a>sys.event_log (Azure SQL Database)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Retourne la réussite [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] connexions, les échecs de connexion et les blocages de base de données. Utilisez ces informations pour suivre ou dépanner l'activité de base de données avec [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
> [!CAUTION]  
>  Pour les installations comportant un grand nombre de bases de données ou un nombre élevé de connexions, activité dans sys.event_log peut entraîner des limitations des performances, l’utilisation élevée du processeur et entraîner des échecs de connexion. Les requêtes de sys.event_log peuvent contribuer au problème. Microsoft s’emploie pour résoudre ce problème. En attendant, pour réduire l’impact de ce problème, limiter les requêtes de sys.event_log. Les utilisateurs du plug-in NewRelic SQL Server doivent visiter [ajustements de performances et de réglage de plug-in Microsoft Azure SQL Database](https://discuss.newrelic.com/t/microsoft-azure-sql-database-plugin-tuning-performance-tweaks/30729) des informations de configuration supplémentaires.  
  
 La vue `sys.event_log` contient les colonnes suivantes.  
  
|Nom de la colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nom de la base de données. Si la connexion échoue et l'utilisateur n'a pas spécifié de nom de la base de données, cette colonne est vide.|  
|**start_time**|**datetime2**|Date et heure UTC indiquant le début de l'intervalle d'agrégation. Pour les événements agrégés, l'heure est toujours un multiple de 5 minutes. Par exemple :<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|Date et heure UTC indiquant la fin de l'intervalle d'agrégation. Pour les événements agrégés, **End_time** correspond toujours exactement 5 minutes plus tard correspondant **heure_début** dans la même ligne. Pour les événements qui ne sont pas agrégés, **heure_début** et **end_time** correspondent à la date UTC réelles et l’heure de l’événement.|  
|**event_category**|**nvarchar(64)**|Composant de niveau supérieur qui a généré cet événement.<br /><br /> Consultez [Types d’événements](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) pour obtenir la liste des valeurs possibles.|  
|**event_type**|**nvarchar(64)**|Type de l'événement.<br /><br /> Consultez [Types d’événements](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) pour obtenir la liste des valeurs possibles.|  
|**event_subtype**|**int**|Sous-type de l'événement.<br /><br /> Consultez [Types d’événements](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) pour obtenir la liste des valeurs possibles.|  
|**event_subtype_desc**|**nvarchar(64)**|Description du sous-type d'événement.<br /><br /> Consultez [Types d’événements](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) pour obtenir la liste des valeurs possibles.|  
|**severity**|**int**|Gravité de l'erreur. Les valeurs possibles sont :<br /><br /> 0 = Information<br />1 = Avertissement<br />2 = Erreur|  
|**event_count**|**int**|Le nombre de fois que cet événement s’est produit pour la base de données spécifié dans l’intervalle de temps spécifié (**heure_début** et **end_time**).|  
|**description**|**nvarchar(max)**|Description détaillée de l'événement.<br /><br /> Consultez [Types d’événements](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) pour obtenir la liste des valeurs possibles.|  
|**additional_data**|**XML**|*Remarque : Cette valeur est toujours NULL pour Azure SQL Database V12. Consultez [exemples](#Deadlock) section pour savoir comment récupérer les événements de blocage pour V12.*<br /><br /> Pour **blocage** événements, cette colonne contient le graphique de blocage. Pour les autres types d'événements, cette colonne renvoie la valeur NULL. |  
  
##  <a name="EventTypes"></a> Types d’événements  
 Les événements enregistrés par chaque ligne de cette vue sont identifiés par catégorie (**event_category**), type d’événement (**event_type**) et un sous-type (**event_subtype**). Le tableau suivant répertorie les types d'événements regroupés dans cette vue.  
  
 Pour les événements dans le **connectivité** catégorie, les informations de résumé sont disponibles dans la vue sys.database_connection_stats.  
  
> [!NOTE]  
>  Cette vue ne contient pas tous les événements possibles de base de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)] qui peuvent se produire, uniquement ceux qui sont répertoriés ici. Des catégories, types d'événements et sous-types supplémentaires peuvent être ajoutés dans les versions ultérieures de [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**description**|  
|-------------------------|---------------------|------------------------|------------------------------|------------------|---------------------|  
|**Connectivité**|**connection_successful**|0|**connection_successful**|0|Connexion à la base de données réussie.|  
|**Connectivité**|**connection_failed**|0|**invalid_login_name**|2|Le nom de connexion n'est pas valide dans cette version de SQL Server.|  
|**Connectivité**|**connection_failed**|1|**windows_auth_not_supported**|2|Les identifiants de connexion Windows ne sont pas pris en charge dans cette version de SQL Server.|  
|**Connectivité**|**connection_failed**|2|**attach_db_not_supported**|2|L'utilisateur a tenté de joindre un fichier de base de données non pris en charge.|  
|**Connectivité**|**connection_failed**|3|**change_password_not_supported**|2|L'utilisateur a demandé la modification du passe de connexion qui n'est pas prise en charge.|  
|**Connectivité**|**connection_failed**|4|**login_failed_for_user**|2|Échec de la connexion pour l'utilisateur.|  
|**Connectivité**|**connection_failed**|5|**login_disabled**|2|Connexion désactivée.|  
|**Connectivité**|**connection_failed**|6|**failed_to_open_db**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> Impossible d'ouvrir la base de données. Peut être dû au fait que la base de données n'existe pas ou à l'absence d'authentification pour ouvrir la base de données.|  
|**Connectivité**|**connection_failed**|7|**blocked_by_firewall**|2|L'adresse IP du client n'est pas autorisée à accéder au serveur.|  
|**Connectivité**|**connection_failed**|8|**client_close**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> Le délai de connexion du client a peut-être expiré. Essayez d'augmenter le délai d'expiration de la connexion.|  
|**Connectivité**|**connection_failed**|9|**Reconfiguration**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La connexion a échoué car la base de données était en cours de reconfiguration.|  
|**Connectivité**|**connection_terminated**|0|**idle_connection_timeout**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La connexion est restée inactive plus longtemps que ne l'autorise le seuil défini pour le système.|  
|**Connectivité**|**connection_terminated**|1|**Reconfiguration**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La session a été interrompue en raison d'une reconfiguration de la base de données.|  
|**Connectivité**|**La limitation**|*\<code motif >*|**reason_code**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La demande est limitée.  Code de raison de la limitation :  *\<code motif >*. Pour plus d’informations, consultez [la limitation du moteur](http://msdn.microsoft.com/library/windowsazure/dn338079.aspx).|  
|**Connectivité**|**throttling_long_transaction**|40549|**long_transaction**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La session a pris fin, car elle contient une transaction à long terme. Essayez de diminuer la durée de la transaction. Pour plus d’informations, consultez [les limites de ressources](http://msdn.microsoft.com/library/windowsazure/dn338081.aspx).|  
|**Connectivité**|**throttling_long_transaction**|40550|**excessive_lock_usage**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La session a pris fin car elle a acquis trop de verrous. Essayez de lire ou de modifier moins de lignes au cours d'une transaction. Pour plus d’informations, consultez [les limites de ressources](http://msdn.microsoft.com/library/windowsazure/dn338081.aspx).|  
|**Connectivité**|**throttling_long_transaction**|40551|**excessive_tempdb_usage**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La session a pris fin en raison d'une utilisation excessive de TEMPDB. Essayez de modifier votre requête afin de réduire l'utilisation de l'espace de table temporaire. Pour plus d’informations, consultez [les limites de ressources](http://msdn.microsoft.com/library/windowsazure/dn338081.aspx).|  
|**Connectivité**|**throttling_long_transaction**|40552|**excessive_log_space_usage**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La session a pris fin en raison d'une utilisation de l'espace pour le journal de transactions excessive. Essayez de modifier moins de lignes au cours d'une transaction. Pour plus d’informations, consultez [les limites de ressources](http://msdn.microsoft.com/library/windowsazure/dn338081.aspx).|  
|**Connectivité**|**throttling_long_transaction**|40553|**excessive_memory_usage**|2|*Remarque : S’applique uniquement aux V11 de base de données SQL Azure.*<br /><br /> La session a pris fin en raison d'une utilisation de mémoire excessive. Essayez de modifier votre requête afin que le nombre de lignes à traiter soit moins important. Pour plus d’informations, consultez [les limites de ressources](http://msdn.microsoft.com/library/windowsazure/dn338081.aspx).|  
|**Moteur**|**blocage**|0|**blocage**|2|Un blocage s'est produit.|  
  
## <a name="permissions"></a>Autorisations  
 Les utilisateurs autorisés à accéder à la **master** base de données ont un accès en lecture seule à cette vue.  
  
## <a name="remarks"></a>Notes  
  
### <a name="event-aggregation"></a>Agrégation d'événements  
 Les informations relatives aux événements de cette vue sont collectées et agrégées par intervalles de 5 minutes. Le **event_count** colonne représente le nombre de fois qu’un particulier **event_type** et **event_subtype** s’est produite pour une base de données spécifique dans un intervalle de temps donné.  
  
> [!NOTE]  
>  Certains événements, comme les blocages, ne sont pas agrégés. Pour ces événements, **event_count** est égal à 1 et **heure_début** et **end_time** est égale à la véritable date et heure UTC lorsque l’événement s’est produite.  
  
 Par exemple, si un utilisateur n'arrive pas à se connecter à la base de données Database1, en raison d'un nom de connexion non valide, sept fois entre 11h00 et 11h05 le 5/2/2012 (UTC), ces informations sont disponibles dans une seule ligne de cette vue :  
  
|**database_name**|**start_time**|**end_time**|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**event_count**|**description**|**additional_data**|  
|------------------------|---------------------|-------------------|-------------------------|---------------------|------------------------|------------------------------|------------------|----------------------|---------------------|--------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`connectivity`|`connection_failed`|`4`|`login_failed_for_user`|`2`|`7`|`Login failed for user.`|`NULL`|  
  
### <a name="interval-starttime-and-endtime"></a>Heure de début (start_time) et heure de fin (end_time) de l'intervalle  
 Un événement est inclus dans un intervalle d’agrégation lorsque l’événement se produit *sur* ou *après *** heure_début** et *avant *** heure_fin** pour cet intervalle. Par exemple, un événement se produisant exactement à `2012-10-30 19:25:00.0000000` est inclus uniquement dans le deuxième intervalle indiqué ci-dessous :  
  
```  
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```  
  
### <a name="data-updates"></a>Mises à jour des données  
 Les données de cette vue sont cumulées au fil du temps. Généralement, les données sont cumulées pendant une heure à compter du début de l'intervalle d'agrégation, mais cela peut prendre jusqu'à 24 heures avant que toutes les données apparaissent dans la vue. Pendant ce temps, les informations d'une seule ligne peuvent être mises à jour périodiquement.  
  
### <a name="data-retention"></a>Rétention des données  
 Les données de cette vue sont conservées pendant 30 jours au maximum et même moins encore selon le nombre de bases de données dans le serveur logique et le nombre d'événements uniques générés par chaque base de données. Pour conserver ces informations plus longtemps, copiez les données dans une base de données distincte. Une fois que vous avez effectué une copie initiale de la vue, les lignes de la vue peuvent être mises à jour au fur et à mesure que les données sont cumulées. Pour tenir à jour votre copie des données, effectuez périodiquement une analyse des lignes de la table pour détecter une augmentation du nombre d'événements dans les lignes existantes et pour identifier les nouvelles lignes (vous pouvez identifier les lignes qui sont uniques à l'aide des heures de début et de fin), puis mettez à jour votre copie des données en fonction de ces modifications.  
  
### <a name="errors-not-included"></a>Erreurs non incluses.  
 Cette vue peut ne pas inclure toutes les informations de connexion et d'erreur :  
  
-   Cette vue n’inclut pas tous les [!INCLUDE[ssSDS](../../includes/sssds-md.md)] erreurs qui peuvent se produire, uniquement celles spécifiées dans la base de données [Types d’événements](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) dans cette rubrique.  
  
-   En cas de défaillance d'un ordinateur dans le centre de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)], un faible volume de données du serveur logique peuvent être manquantes dans la table d'événement.  
  
-   Si une adresse IP a été bloquée par DoSGuard, les événements de tentative de connexion à partir de cette adresse IP ne peuvent pas être collectés et n'apparaitront pas dans cette vue.  
  
## <a name="examples"></a>Exemples  
  
### <a name="simple-examples"></a>Exemples simples  
 La requête suivante retourne tous les événements qui se sont produits entre le 25/9/2011 à midi et le 28/9/2011 (UTC) à midi. Par défaut, les résultats de la requête sont triés par **heure_début** (ordre croissant).  
  
```  
SELECT * FROM sys.event_log   
WHERE start_time >= '2011-09-25:12:00:00'   
    AND end_time <= '2011-09-28 12:00:00';  
```  
  
 La requête suivante renvoie tous les événements de blocage pour la base de données Database1 (s’applique uniquement aux V11 de base de données SQL Azure).  
  
```  
SELECT * FROM sys.event_log   
WHERE event_type = 'deadlock'   
    AND database_name = 'Database1';  
```  
<a name="Deadlock"></a> La requête suivante renvoie tous les événements de blocage pour la base de données Database1 (s’applique uniquement à Azure SQL Database V12).  
  
```  
WITH CTE AS (  
       SELECT CAST(event_data AS XML)  AS [target_data_XML]   
   FROM sys.fn_xe_telemetry_blob_target_read_file('dl', null, null, null)  
)  
SELECT target_data_XML.value('(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
target_data_XML.query('/event/data[@name=''xml_report'']/value/deadlock') AS deadlock_xml,  
target_data_XML.query('/event/data[@name=''database_name'']/value').value('(/value)[1]', 'nvarchar(100)') AS db_name  
FROM CTE   
  
```  
  
  
 La requête suivante retourne une limitation dure sur les événements Threads de travail de SQL qui se sont produits entre 10h00 et 11h00 le 25/9/2011 (UTC).  
  
```  
SELECT * FROM sys.event_log   
WHERE event_type = 'throttling'   
    AND event_subtype = 4194307   
    AND start_time >= '2011-09-25 10:00:00'   
    AND end_time <= '2011-09-25 11:00:00';  
```  
  
### <a name="db-scoped-extended-event"></a>Étendue de base de données des événements étendus  
 L’exemple de code suivant permet de configurer la session d’événements étendus (XEvent) étendue de base de données :  
  
```sql  
IF EXISTS  
    (SELECT * from sys.database_event_sessions  
        WHERE name = 'azure_monitor_deadlock_session')  
BEGIN  
    ALTER EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE  
        DROP TARGET package0.ring_buffer;  
  
    DROP EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE;  
END  
  
CREATE EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    ADD EVENT sqlserver.database_xml_deadlock_report  
    ADD TARGET package0.ring_buffer  
    (  
        SET max_memory = 2048, max_events_limit = 10  
    )  
    WITH (STARTUP_STATE = ON,  
          EVENT_RETENTION_MODE = ALLOW_SINGLE_EVENT_LOSS);  
  
ALTER EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    STATE = START;  
```  
  
### <a name="check-for-deadlock"></a>Vérification de blocage

Utilisez la requête suivante pour vérifier s’il existe un blocage.  
  
```sql  
WITH CTE AS (  
    SELECT CAST(xet.target_data AS XML)  AS [target_data_XML]  
        FROM            sys.dm_xe_database_session_targets AS xet  
             INNER JOIN sys.dm_xe_database_sessions        AS xe  
                 ON (xe.address = xet.event_session_address)  
        WHERE xe.name = 'azure_monitor_deadlock_session'  
)  
, CTE2 AS (  
    SELECT  
            T2.EventData.query('.').value(  
                '(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
            T2.EventData.query('.').query(  
                '(/event/data/value/deadlock)[1]')     AS deadlock_xml  
        FROM CTE  
            CROSS Apply [target_data_XML].nodes(  
                '/RingBufferTarget/event') AS T2(EventData)  
)  
SELECT * FROM CTE2;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus dans la base de données SQL Azure](http://azure.microsoft.com/documentation/articles/sql-database-xevent-db-diff-from-svr/)  
  
  
