---
title: SELECT et JOIN à partir de vues système pour les événements étendus dans SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: xevents
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 04521d7f-588c-4259-abc2-1a2857eb05ec
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 356cfffea1189b3d7efb042fcec7e26eedc9b747
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="selects-and-joins-from-system-views-for-extended-events-in-sql-server"></a>SELECT et JOIN à partir de vues système pour les événements étendus dans SQL Server
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]


Cet article explique les deux ensembles de vues système qui se rapportent aux événements étendus dans Microsoft SQL Server et dans le service cloud Azure SQL Database. L’article explique :

- Comment joindre (JOIN) différentes vues système
- Comment sélectionner (SELECT) des types d’informations particuliers à partir de la vue système
- Comment les mêmes informations de session d’événements sont représentées de différentes perspectives technologiques, ce qui permet de mieux comprendre chaque perspective


La plupart des exemples sont écrits pour SQL Server, mais avec de petites modifications ils devraient s’exécuter sur SQL Database.



## <a name="a-foundational-information"></a>A. Informations fondamentales


Il existe deux ensembles de vues système pour les événements étendus :


#### <a name="catalog-views"></a>Affichages catalogue :

- Ces vues stockent des informations sur la *définition* de chaque session d’événements créée par [CREATE EVENT SESSION](../../t-sql/statements/create-event-session-transact-sql.md), ou par un équivalent dans l’interface utilisateur de SSMS. Ces vues ignorent si une session a démarré.
    - Par exemple, si l’ **Explorateur d’objets** de SSMS montre qu’aucune session d’événements n’est définie, une instruction SELECT dans la vue *sys.server_event_session_targets* retourne zéro ligne.


- Le préfixe de nom est :
    - *sys.server\_événement\_session\** est le préfixe de nom sur SQL Server.
    - *sys.database\_événement\_session\** est le préfixe de nom sur SQL Database.


#### <a name="dynamic-management-views-dmvs"></a>Vues de gestion dynamiques (DMV) :

- Elles stockent des informations sur l’ *activité en cours* des sessions d’événements en cours d’exécution. Ces DMV en savent peu sur la définition des sessions.
    - Même si toutes les sessions d’événements sont actuellement arrêtées, une instruction SELECT à partir de la vue *sys.dm_xe_packages* retournera toujours des lignes, car différents packages sont chargés dans la mémoire active au démarrage du serveur.
    - Pour la même raison, *sys.dm_xe_objects* *sys.dm_xe_object_columns* would also still return rows.


- Le préfixe de nom pour les DMV des événements étendus est le suivant :
    - *sys.dm\_xe\_\** est le préfixe de nom sur SQL Server.
    - *sys.dm\_xe\_database\_\** est généralement le préfixe de nom sur SQL Database.


#### <a name="permissions"></a>Autorisations :


Pour sélectionner (avec SELECT) à partir des vues système, l’autorisation suivante est nécessaire :

- VIEW SERVER STATE si vous utilisez Microsoft SQL Server.
- VIEW DATABASE STATE si vous utilisez Azure SQL Database.


<a name="section_B_catalog_views"></a>

## <a name="b-catalog-views"></a>B. Affichages catalogue


Cette section met en correspondance et en corrélation trois différentes perspectives technologiques sur la même session d’événements définie. La session a été définie et est visible dans l’ **Explorateur d’objets** de SQL Server Management Studio (SSMS.exe), mais elle n’est pas en cours d’exécution.

Chaque mois, il est préférable d’ [installer la dernière mise à jour de SSMS](http://msdn.microsoft.com/library/mt238290.aspx), afin d’éviter toute erreur inattendue.


Vous trouverez de la documentation de référence sur les affichages catalogue pour les événements étendus dans [Affichages catalogue des événements étendus (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md).


&nbsp;



#### <a name="the-sequence-in-this-section-b"></a>Voici la séquence de cette section B :


- [B.1 Perspective de l’interface utilisateur de SSMS](#section_B_1_SSMS_UI_perspective)
    - Créez la définition de la session d’événements à l’aide de l’interface utilisateur de SSMS. Des captures d’écran étape par étape sont fournies.


- [B.2 Perspective de Transact-SQL](#section_B_2_TSQL_perspective)
    - Utilisez le menu contextuel de SSMS pour rétroconcevoir la session d’événements définie dans l’instruction Transact-SQL équivalente **CREATE EVENT SESSION** . Le code T-SQL montre une correspondance parfaite avec les choix dans les captures d’écran SSMS.


- [B.3 Perspective de SELECT JOIN UNION dans l’affichage catalogue](#section_B_3_Catalog_view_S_J_UNION)
    - Exécutez une instruction T-SQL SELECT à partir des affichages catalogue système pour notre session d’événements. Les résultats correspondent aux spécifications de l’instruction **CREATE EVENT SESSION** .


&nbsp;



<a name="section_B_1_SSMS_UI_perspective"></a>

### <a name="b1-ssms-ui-perspective"></a>B.1 Perspective de l’interface utilisateur de SSMS


Dans SSMS, dans l’ **Explorateur d’objets**, vous pouvez démarrer la boîte de dialogue **Nouvelle session** en développant **Gestion** > **Événements étendus**, puis en cliquant avec le bouton droit sur **Sessions** > **Nouvelle session**.

Dans la grande boîte de dialogue **Nouvelle session** , dans la première section intitulée **Général**, nous constatons que l’option **Démarrer la session d’événements au démarrage du serveur**a été sélectionnée.

![Nouvelle session > Général, Démarrer la session d’événements au démarrage du serveur.](../../relational-databases/extended-events/media/xevents-ssms-ac105-eventname-startup.png)


Ensuite, dans la section **Événements**, nous constatons que l’événement **lock_deadlock** a été choisi. Pour cet événement, nous voyons que trois **Actions** ont été sélectionnées. Cela signifie que le bouton **Configurer** a été enfoncé. C’est pourquoi il est maintenant grisé.

![Nouvelle session > Événements, Champs globaux (actions)](../../relational-databases/extended-events/media/xevents-ssms-ac110-actions-global.png)


<a name="resource_type_PAGE_cat_view"></a>

Ensuite, toujours dans la section **Événements** > **Configurer**, nous constatons que [**resource_type** a la valeur **PAGE**](#resource_type_dmv_actual_row). Cela signifie que les données d’événement ne seront pas envoyées du moteur d’événements à la cible si la valeur de **resource_type** est autre que **PAGE**.

Nous constatons la présence de filtres de prédicat supplémentaires pour le nom de base de données et pour un compteur.

![Nouvelle session > Événements, Filtre (prédicat)](../../relational-databases/extended-events/media/xevents-ssms-ac115-predicate-db.png)


Ensuite, dans la section **Stockage de données**, nous constatons qu’**event_file** a été choisi comme cible. En outre, nous constatons que l’option **Activer la substitution de fichier** a été sélectionnée.

![Nouvelle session > Stockage de données, eventfile_enablefileroleover](../../relational-databases/extended-events/media/xevents-ssms-ac120-target-eventfile.png)


Pour finir, dans la section **Avancé**, nous constatons que la valeur de **Latence maximale de répartition** a été réduite à quatre secondes.

![Nouvelle session > Avancé, Latence maximale de répartition](../../relational-databases/extended-events/media/xevents-ssms-ac125-latency4.png)


Cette étape termine la perspective de l’interface utilisateur de SSMS sur une définition de session d’événement.


<a name="section_B_2_TSQL_perspective"></a>

### <a name="b2-transact-sql-perspective"></a>B.2 Perspective de Transact-SQL


Quelle que soit la façon dont une définition de session d’événement est créée, à partir de l’interface utilisateur de SSMS la session peut être rétroconçue dans un script Transact-SQL correspondant parfaitement. Vous pouvez examiner les captures d’écran de Nouvelle session précédentes et comparer leurs spécifications visibles aux clauses dans le script T-SQL **CREATE EVENT SESSION** suivant généré.

Pour rétroconcevoir une session d’événements, dans l’ **Explorateur d’objets** vous pouvez cliquez avec le bouton droit sur le nœud de votre session, puis choisir **Générer un script de la session en tant que** > **Créer dans** > **Presse-papiers**.

Le script T-SQL suivant a été créé par rétroconception avec SSMS. Il a ensuite été amélioré manuellement par manipulation stratégique des espaces uniquement.


```sql
CREATE EVENT SESSION [event_session_test3]
    ON SERVER  -- Or, if on Azure SQL Database, ON DATABASE.

    ADD EVENT sqlserver.lock_deadlock
    (
        SET
            collect_database_name = (1)
        ACTION
        (
            package0  .collect_system_time,
            package0  .event_sequence,
            sqlserver .client_hostname
        )
        WHERE
        (
            [database_name]           = N'InMemTest2'
            AND [package0].[counter] <= (16)
            AND [resource_type]       = (6)
        )
    )

    ADD TARGET package0.event_file
    (
        SET
            filename           = N'C:\Junk\event_session_test3_EF.xel',
            max_file_size      = (20),
            max_rollover_files = (2)
    )

    WITH
    (
        MAX_MEMORY            = 4096 KB,
        EVENT_RETENTION_MODE  = ALLOW_SINGLE_EVENT_LOSS,
        MAX_DISPATCH_LATENCY  = 4 SECONDS,
        MAX_EVENT_SIZE        = 0 KB,
        MEMORY_PARTITION_MODE = NONE,
        TRACK_CAUSALITY       = OFF,
        STARTUP_STATE         = ON
    );
```


Cette étape termine la perspective de T-SQL.


<a name="section_B_3_Catalog_view_S_J_UNION"></a>

### <a name="b3-catalog-view-select-join-union-perspective"></a>B.3 Perspective de SELECT JOIN UNION dans l’affichage catalogue


N’ayez pas peur. L’instruction T-SQL SELECT suivante est longue uniquement car elle joint (avec des instructions UNION) plusieurs petites instructions SELECT. Chaque petite instruction SELECT peut être exécutée individuellement. Les petites instructions SELECT montrent comment les différents affichages catalogue système doivent être joints.


```sql
SELECT
        s.name        AS [Session-Name],
        '1_EVENT'     AS [Clause-Type],
        'Event-Name'  AS [Parameter-Name],
        e.name        AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name         AS [Session-Name],
        '2_EVENT_SET'  AS [Clause-Type],
        f.name         AS [Parameter-Name],
        f.value        AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_fields   As f

            ON  f.event_session_id = s.event_session_id
            AND f.object_id        = e.event_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name              AS [Session-Name],
        '3_EVENT_ACTION'    AS [Clause-Type],

        e.package + '.' + a.name
                            AS [Parameter-Name],

        '(Not_Applicable)'  AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_actions  As a

            ON  a.event_session_id = s.event_session_id
            AND a.event_id         = e.event_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name                AS [Session-Name],
        '4_EVENT_PREDICATES'  AS [Clause-Type],
        e.predicate           AS [Parameter-Name],
        '(Not_Applicable)'    AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name              AS [Session-Name],
        '5_TARGET'          AS [Clause-Type],
        t.name              AS [Parameter-Name],
        '(Not_Applicable)'  AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_targets  AS t

            ON  t.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name          AS [Session-Name],
        '6_TARGET_SET'  AS [Clause-Type],
        f.name          AS [Parameter-Name],
        f.value         AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_targets  AS t

            ON  t.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_fields   As f

            ON  f.event_session_id = s.event_session_id
            AND f.object_id        = t.target_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name               AS [Session-Name],
        '7_WITH_MAX_MEMORY'  AS [Clause-Type],
        'max_memory'         AS [Parameter-Name],
        s.max_memory         AS [Parameter-Value]
    FROM
              sys.server_event_sessions  AS s
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name                  AS [Session-Name],
        '7_WITH_STARTUP_STATE'  AS [Clause-Type],
        'startup_state'         AS [Parameter-Name],
        s.startup_state         AS [Parameter-Value]
    FROM
              sys.server_event_sessions  AS s
    WHERE
        s.name = 'event_session_test3'

ORDER BY
    [Session-Name],
    [Clause-Type],
    [Parameter-Name]
;
```


#### <a name="output"></a>Sortie


Voici la sortie de l’exécution de l’instruction SELECT JOIN UNION précédente. Les noms et les valeurs des paramètres de sortie sont mappés à ce qui est clairement visible dans l’instruction CREATE EVENT SESSION précédente.


```
Session-Name          Clause-Type            Parameter-Name                  Parameter-Value
------------          -----------            --------------                  ---------------
event_session_test3   1_EVENT                Event-Name                      lock_deadlock
event_session_test3   2_EVENT_SET            collect_database_name           1
event_session_test3   3_EVENT_ACTION         sqlserver.client_hostname       (Not_Applicable)
event_session_test3   3_EVENT_ACTION         sqlserver.collect_system_time   (Not_Applicable)
event_session_test3   3_EVENT_ACTION         sqlserver.event_sequence        (Not_Applicable)
event_session_test3   4_EVENT_PREDICATES     ([sqlserver].[equal_i_sql_unicode_string]([database_name],N'InMemTest2') AND [package0].[counter]<=(16))   (Not_Applicable)
event_session_test3   5_TARGET               event_file                      (Not_Applicable)
event_session_test3   6_TARGET_SET           filename                        C:\Junk\event_session_test3_EF.xel
event_session_test3   6_TARGET_SET           max_file_size                   20
event_session_test3   6_TARGET_SET           max_rollover_files              2
event_session_test3   7_WITH_MAX_MEMORY      max_memory                      4096
event_session_test3   7_WITH_STARTUP_STATE   startup_state                   1
```


Cette étape termine la section sur les affichages catalogue.



<a name="section_C_DMVs"></a>

## <a name="c-dynamic-management-views-dmvs"></a>C. Vues de gestion dynamique


Passons maintenant aux vues de gestion dynamique (DMV). Cette section fournit plusieurs instructions Transact-SQL SELECT, chacune ayant une fonction métier spécifique. Ces instructions SELECT montrent aussi comment joindre les DMV pour toute nouvelle utilisation souhaitée.


Vous trouverez de la documentation de référence sur les vues de gestion dynamique dans [Vues de gestion dynamique des Événements étendus](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md).


Dans cet article, toutes les lignes de sortie des instructions SELECT suivantes proviennent de SQL Server 2016, sauf indication contraire.


Voici la liste des instructions SELECT dans cette section C sur les vues de gestion dynamique :

- [C.1 Liste de tous les packages](#section_C_1_list_packages)
- [C.2 Quantité de chaque type d’objet](#section_C_2_count_object_type)
- [C.3 Sélectionner tous les éléments disponibles triés par type](#section_C_3_select_all_available_objects)
- [C.4 Champs de données disponibles pour votre événement](#section_C_4_data_fields)
- [C.5 *sys.dm_xe_map_values* et champs d’événements](#section_C_5_map_values_fields)
- [C.6 Paramètres pour les cibles](#section_C_6_parameters_targets)
- [C.7 Transtypage SELECT DMV de colonne target_data en XML](#section_C_7_dmv_select_target_data_column)
- [C.8 Sélectionner à partir d’une fonction pour extraire des données event_file à partir du lecteur de disque](#section_C_8_select_function_disk)



<a name="section_C_1_list_packages"></a>

### <a name="c1-list-of-all-packages"></a>C.1 Liste de tous les packages


Tous les objets que vous pouvez utiliser dans le domaine des événements étendus proviennent de packages qui sont chargés dans le système. Cette section répertorie tous les packages et leurs descriptions.


```sql
SELECT  --C.1
        p.name         AS [Package],
        p.description  AS [Package-Description]
    FROM
        sys.dm_xe_packages  AS p
    ORDER BY
        p.name;
```


#### <a name="output"></a>Sortie

Voici la liste des packages.


```
/***  (The unique p.guid values are not shown.)
Package        Package-Description
-------        -------------------
filestream     Extended events for SQL Server FILESTREAM and FileTable
package0       Default package. Contains all standard types, maps, compare operators, actions and targets
qds            Extended events for Query Store
SecAudit       Security Audit Events
sqlclr         Extended events for SQL CLR
sqlos          Extended events for SQL Operating System
SQLSatellite   Extended events for SQL Satellite
sqlserver      Extended events for Microsoft SQL Server
sqlserver      Extended events for Microsoft SQL Server
sqlserver      Extended events for Microsoft SQL Server
sqlsni         Extended events for Microsoft SQL Server
ucs            Extended events for Unified Communications Stack
XtpCompile     Extended events for the XTP Compile
XtpEngine      Extended events for the XTP Engine
XtpRuntime     Extended events for the XTP Runtime
***/
```


*Définitions des abréviations précédentes :*

- clr = Common Language Runtime de .NET
- qds = Query Data Store (magasin de données de requête)
- sni = Server Network Interface (interface réseau du serveur)
- ucs = Unified Communications Stack (pile de communications unifiées)
- xtp = traitement transactionnel extrême


<a name="section_C_2_count_object_type"></a>

### <a name="c2-count-of-every-object-type"></a>C.2 Quantité de chaque type d’objet


Cette section indique les types d’objets que contiennent les packages d’événements. Une liste complète de tous les types d’objets qui se trouvent dans *sys.dm\_xe\_objects* est affichée, ainsi que la quantité de chaque type.


```sql
SELECT  --C.2
        Count(*)  AS [Count-of-Type],
        o.object_type
    FROM
        sys.dm_xe_objects  AS o
    GROUP BY
        o.object_type
    ORDER BY
        1  DESC;
```


#### <a name="output"></a>Sortie

Voici le nombre d’objets par type d’objet. Il existe environ 1915 objets.


```
/***  Actual output, sum is about 1915:

Count-of-Type   object_type
-------------   -----------
1303            event
351             map
84              message
77              pred_compare
53              action
46              pred_source
28              type
17              target
***/
```


<a name="section_C_3_select_all_available_objects"></a>

### <a name="c3-select-all-available-items-sorted-by-type"></a>C.3 Sélectionner tous les éléments disponibles triés par type


L’instruction SELECT suivante retourne environ 1915 lignes, une par objet.



```sql
SELECT  --C.3
        o.object_type  AS [Type-of-Item],
        p.name         AS [Package],
        o.name         AS [Item],
        o.description  AS [Item-Description]
    FROM
             sys.dm_xe_objects  AS o
        JOIN sys.dm_xe_packages AS p  ON o.package_guid = p.guid
    WHERE
        o.object_type IN ('action' , 'target' , 'pred_source')
        AND
        (
            (o.capabilities & 1) = 0
            OR
            o.capabilities IS NULL
        )
    ORDER BY
        [Type-of-Item],
        [Package],
        [Item];
```


#### <a name="output"></a>Sortie

Pour vous mettre en appétit, voici un échantillonnage arbitraire des objets retournés par l’instruction SELECT précédente.


```
/***
Type-of-Item   Package        Item                          Item-Description
------------   -------        ----                          ----------------
action         package0       callstack                     Collect the current call stack
action         package0       debug_break                   Break the process in the default debugger
action         sqlos          task_time                     Collect current task execution time
action         sqlserver      sql_text                      Collect SQL text
event          qds            query_store_aprc_regression   Fired when Query Store detects regression in query plan performance
event          SQLSatellite   connection_accept             Occurs when a new connection is accepted. This event serves to log all connection attempts.
event          XtpCompile     cgen                          Occurs at start of C code generation.
map            qds            aprc_state                    Query Store Automatic Plan Regression Correction state
message        package0       histogram_event_required      A value is required for the parameter 'filtering_event_name' when source type is 0.
pred_compare   package0       equal_ansi_string             Equality operator between two ANSI string values
pred_compare   sqlserver      equal_i_sql_ansi_string       Equality operator between two SQL ANSI string values
pred_source    sqlos          task_execution_time           Get current task execution time
pred_source    sqlserver      client_app_name               Get the current client application name
target         package0       etw_classic_sync_target       Event Tracing for Windows (ETW) Synchronous Target
target         package0       event_counter                 Use the event_counter target to count the number of occurrences of each event in the event session.
target         package0       event_file                    Use the event_file target to save the event data to an XEL file, which can be archived and used for later analysis and review. You can merge multiple XEL files to view the combined data from separate event sessions.
target         package0       histogram                     Use the histogram target to aggregate event data based on a specific event data field or action associated with the event. The histogram allows you to analyze distribution of the event data over the period of the event session.
target         package0       pair_matching                 Pairing target
target         package0       ring_buffer                   Asynchronous ring buffer target.
type           package0       xml                           Well formed XML fragment
***/
```



<a name="section_C_4_data_fields"></a>

### <a name="c4-data-fields-available-for-your-event"></a>C.4 Champs de données disponibles pour votre événement


L’instruction SELECT suivante retourne tous les champs de données propres à votre type d’événement.

- Notez l’élément de clause WHERE : *column_type = 'data'*.
- Vous devez aussi modifier la valeur de la clause WHERE pour *o.name =*.


```sql
SELECT  -- C.4
        p.name         AS [Package],
        c.object_name  AS [Event],
        c.name         AS [Column-for-Predicate-Data],
        c.description  AS [Column-Description]
    FROM
              sys.dm_xe_object_columns  AS c
        JOIN  sys.dm_xe_objects         AS o

            ON  o.name = c.object_name

        JOIN  sys.dm_xe_packages        AS p

            ON  p.guid = o.package_guid
    WHERE
        c.column_type = 'data'
        AND
        o.object_type = 'event'
        AND
        o.name        = '\<EVENT-NAME-HERE!>'  --'lock_deadlock'
    ORDER BY
        [Package],
        [Event],
        [Column-for-Predicate-Data];
```


#### <a name="output"></a>Sortie

Les lignes suivantes ont été retournées par l’instruction précédente SELECT, WHERE `o.name = 'lock_deadlock'`:

- Chaque ligne représente un filtre facultatif pour l’événement *sqlserver.lock_deadlock* .
- La colonne *\[Column-Description\]* est omise de l’affichage suivant. Sa valeur est souvent NULL.


```
/***
Actual output, except for the omitted Description column which is often NULL.
These rows are where object_type = 'lock_deadlock'.

Package     Event           Column-for-Predicate-Data
-------     -----           -------------------------
sqlserver   lock_deadlock   associated_object_id
sqlserver   lock_deadlock   database_id
sqlserver   lock_deadlock   database_name
sqlserver   lock_deadlock   deadlock_id
sqlserver   lock_deadlock   duration
sqlserver   lock_deadlock   lockspace_nest_id
sqlserver   lock_deadlock   lockspace_sub_id
sqlserver   lock_deadlock   lockspace_workspace_id
sqlserver   lock_deadlock   mode
sqlserver   lock_deadlock   object_id
sqlserver   lock_deadlock   owner_type
sqlserver   lock_deadlock   resource_0
sqlserver   lock_deadlock   resource_1
sqlserver   lock_deadlock   resource_2
sqlserver   lock_deadlock   resource_description
sqlserver   lock_deadlock   resource_type
sqlserver   lock_deadlock   transaction_id
***/
```



<a name="section_C_5_map_values_fields"></a>

### <a name="c5-sysdmxemapvalues-and-event-fields"></a>C.5 *sys.dm_xe_map_values* et champs d’événements


L’instruction SELECT suivante inclut une jointure à la vue délicate nommée *sys.dm_xe_map_values*.

L’instruction SELECT affiche les nombreux champs parmi lesquels vous pouvez choisir pour votre session d’événements. Vous pouvez utiliser les champs d’événements de deux manières :

- Pour choisir les valeurs de champs qui seront écrites dans votre cible pour chaque occurrence d’événement.
- Pour filtrer les occurrences des événements qui seront envoyées à la cible plutôt que conservées.


```sql
SELECT  --C.5
        dp.name         AS [Package],
        do.name         AS [Object],
        do.object_type  AS [Object-Type],
        'o--c'     AS [O--C],
        dc.name         AS [Column],
        dc.type_name    AS [Column-Type-Name],
        dc.column_type  AS [Column-Type],
        dc.column_value AS [Column-Value],
        'c--m'     AS [C--M],
        dm.map_value    AS [Map-Value],
        dm.map_key      AS [Map-Key]
    FROM
              sys.dm_xe_objects         AS do
        JOIN  sys.dm_xe_object_columns  AS dc

            ON  dc.object_name = do.name

        JOIN  sys.dm_xe_map_values      AS dm

            ON  dm.name = dc.type_name

        JOIN  sys.dm_xe_packages        AS dp

            ON  dp.guid = do.package_guid
    WHERE
        do.object_type = 'event'
        AND
        do.name        = '\<YOUR-EVENT-NAME-HERE!>'  --'lock_deadlock'
    ORDER BY
        [Package],
        [Object],
        [Column],
        [Map-Value];
```


#### <a name="output"></a>Sortie

<a name="resource_type_dmv_actual_row"></a>

Voici un échantillonnage des 153 lignes de sortie de l’instruction T-SQL SELECT précédente. La ligne pour **resource_type** est [pertinente](#resource_type_PAGE_cat_view) au filtrage de prédicat utilisé dans l’exemple **event_session_test3** ailleurs dans cet article.


```
/***  5 sampled rows from the actual 153 rows returned.
    NOTE:  'resource_type' under 'Column'.

Package     Object          Object-Type   O--C   Column          Column-Type-Name     Column-Type   Column-Value   C--M   Map-Value        Map-Key
-------     ------          -----------   ----   ------          ----------------     -----------   ------------   ----   ---------        -------
sqlserver   lock_deadlock   event         o--c   CHANNEL         etw_channel          readonly      2              c--m   Operational      4
sqlserver   lock_deadlock   event         o--c   KEYWORD         keyword_map          readonly      16             c--m   access_methods   1024
sqlserver   lock_deadlock   event         o--c   mode            lock_mode            data          NULL           c--m   IX               8
sqlserver   lock_deadlock   event         o--c   owner_type      lock_owner_type      data          NULL           c--m   Cursor           2
sqlserver   lock_deadlock   event         o--c   resource_type   lock_resource_type   data          NULL           c--m   PAGE             6

Therefore, on your CREATE EVENT SESSION statement, in its ADD EVENT WHERE clause,
you could put:
    WHERE( ... resource_type = 6 ...)  -- Meaning:  6 = PAGE.
***/
```


<a name="section_C_6_parameters_targets"></a>

### <a name="c6-parameters-for-targets"></a>C.6 Paramètres pour les cibles


L’instruction SELECT suivante retourne chaque paramètre pour votre cible. Chaque paramètre est balisé pour indiquer s’il est obligatoire. Les valeurs que vous assignez aux paramètres affectent le comportement de la cible.

- Notez l’élément de clause WHERE : *object_type = 'customizable'*.
- Vous devez aussi modifier la valeur de la clause WHERE pour *o.name =*.


```sql
SELECT  --C.6
        p.name        AS [Package],
        o.name        AS [Target],
        c.name        AS [Parameter],
        c.type_name   AS [Parameter-Type],

        CASE c.capabilities_desc
            WHEN 'mandatory' THEN 'YES_Mandatory'
            ELSE 'Not_mandatory'
        END  AS [IsMandatoryYN],

        c.description AS [Parameter-Description]
    FROM
              sys.dm_xe_objects   AS o
        JOIN  sys.dm_xe_packages  AS p

            ON  o.package_guid = p.guid

        LEFT OUTER JOIN  sys.dm_xe_object_columns  AS c

            ON  o.name        = c.object_name
            AND c.column_type = 'customizable'  -- !
    WHERE
        o.object_type = 'target'
        AND
        o.name     LIKE '%'    -- Or '\<YOUR-TARGET-NAME-HERE!>'.
    ORDER BY
        [Package],
        [Target],
        [IsMandatoryYN]  DESC,
        [Parameter];
```


#### <a name="output"></a>Sortie

Les lignes de paramètres suivantes sont un sous-ensemble des éléments retournés par l’instruction SELECT précédente, dans SQL Server 2016.


```
/***  Actual output, all rows, where target name = 'event_file'.
Package    Target       Parameter            Parameter-Type       IsMandatoryYN   Parameter-Description
-------    ------       ---------            --------------       -------------   ---------------------
package0   event_file   filename             unicode_string_ptr   YES_Mandatory   Specifies the location and file name of the log
package0   event_file   increment            uint64               Not_mandatory   Size in MB to grow the file
package0   event_file   lazy_create_blob     boolean              Not_mandatory   Create blob upon publishing of first event buffer, not before.
package0   event_file   max_file_size        uint64               Not_mandatory   Maximum file size in MB
package0   event_file   max_rollover_files   uint32               Not_mandatory   Maximum number of files to retain
package0   event_file   metadatafile         unicode_string_ptr   Not_mandatory   Not used
***/
```


<a name="section_C_7_dmv_select_target_data_column"></a>

### <a name="c7-dmv-select-casting-targetdata-column-to-xml"></a>C.7 Transtypage SELECT DMV de colonne target_data en XML


Cette instruction SELECT de DMV retourne les lignes de données à partir de la cible de votre session d’événements active. Les données sont converties au format XML, ce qui rend la cellule retournée interactive pour faciliter l’affichage dans SSMS.

- Si votre session d’événements est arrêtée, cette instruction SELECT retourne zéro ligne.
- Vous devez modifier la valeur de la clause WHERE pour *s.name =*.


```sql
SELECT  --C.7
        s.name,
        t.target_name,
        CAST(t.target_data AS XML)  AS [XML-Cast]
    FROM
              sys.dm_xe_session_targets  AS t
        JOIN  sys.dm_xe_sessions         AS s

            ON s.address = t.event_session_address
    WHERE
        s.name = '\<Your-Session-Name-Here!>';
```


#### <a name="output-the-only-row-including-its-xml-cell"></a>Sortie : une seule ligne, y compris sa cellule XML

Voici la seule ligne générée à partir de l’instruction SELECT précédente. La colonne *XML-Cast* contient une chaîne XML que SSMS comprend comme étant du XML. Ainsi, SSMS comprend qu’il doit rendre la cellule XML-Cast interactive.


Pour cette séquence :

- *s.name =* a pris comme valeur une session d’événements pour l’événement *checkpoint_begin* .
- La cible était un *ring_buffer*.


```XML
name                              target_name   XML-Cast
----                              -----------   --------
checkpoint_session_ring_buffer2   ring_buffer   <RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="104"><event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:23.508Z"><data name="database_id"><type name="uint32" package="package0" /><value>5</value></data></event><event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:26.975Z"><data name="database_id"><type name="uint32" package="package0" /><value>5</value></data></event></RingBufferTarget>
```


#### <a name="output-xml-displayed-pretty-when-cell-is-clicked"></a>Sortie : XML affiché correctement en cas de clic sur la cellule


Quand vous cliquez sur la cellule XML-Cast, vous obtenez l’affichage suivant.


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="104">
  <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:23.508Z">
    <data name="database_id">
      <type name="uint32" package="package0" />
      <value>5</value>
    </data>
  </event>
  <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:26.975Z">
    <data name="database_id">
      <type name="uint32" package="package0" />
      <value>5</value>
    </data>
  </event>
</RingBufferTarget>
```


<a name="section_C_8_select_function_disk"></a>

### <a name="c8-select-from-a-function-to-retrieve-eventfile-data-from-disk-drive"></a>C.8 Sélectionner à partir d’une fonction pour extraire des données event_file à partir du lecteur de disque


Supposez que votre session d’événements a collecté des données puis a été arrêtée. Si votre session a été définie pour utiliser la cible event_file, vous pouvez toujours récupérer les données en appelant la fonction *sys.fn_xe_target_read_file*.

- Vous devez modifier votre chemin et votre nom de fichier dans le paramètre de l’appel de fonction avant d’exécuter cette instruction SELECT.
    - Ne faites pas attention aux chiffres supplémentaires incorporés par le système SQL dans vos noms de fichiers .XEL chaque fois que vous redémarrez votre session. Affectez simplement le nom racine et l’extension ordinaires.


```sql
SELECT  --C.8
        f.module_guid,
        f.package_guid,
        f.object_name,
        f.file_name,
        f.file_offset,
        CAST(f.event_data AS XML)  AS [Event-Data-As-XML]
    FROM
        sys.fn_xe_file_target_read_file(

            '\<YOUR-PATH-FILE-NAME-ROOT-HERE!>*.xel',
            --'C:\Junk\Checkpoint_Begins_ES*.xel',  -- Example.

            NULL, NULL, NULL
        )  AS f;
```


#### <a name="output-rows-returned-by-select-from-the-function"></a>Sortie : lignes retournées par l’instruction SELECT


Voici les lignes retournées par l’instruction SELECT précédente. La colonne XML de droite contient les données qui sont propres à l’occurrence d’événement.


```
module_guid                            package_guid                           object_name        file_name                                                           file_offset   Event-Data-As-XML
-----------                            ------------                           -----------        ---------                                                           -----------   -----------------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_begin   C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5120          <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:14.023Z"><data name="database_id"><value>5</value></data><action name="session_id" package="sqlserver"><value>60</value></action><action name="database_id" package="sqlserver"><value>5</value></action></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_end     C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5120          <event name="checkpoint_end" package="sqlserver" timestamp="2016-07-09T03:30:14.025Z"><data name="database_id"><value>5</value></data></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_begin   C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5632          <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:17.704Z"><data name="database_id"><value>5</value></data><action name="session_id" package="sqlserver"><value>60</value></action><action name="database_id" package="sqlserver"><value>5</value></action></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_end     C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5632          <event name="checkpoint_end" package="sqlserver" timestamp="2016-07-09T03:30:17.709Z"><data name="database_id"><value>5</value></data></event>
```


#### <a name="output-one-xml-cell"></a>Sortie : une cellule XML


Voici le contenu de la première cellule XML, tirée de l’ensemble de lignes retourné précédent.


```xml
<event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:14.023Z">
  <data name="database_id">
    <value>5</value>
  </data>
  <action name="session_id" package="sqlserver">
    <value>60</value>
  </action>
  <action name="database_id" package="sqlserver">
    <value>5</value>
  </action>
</event>
```


