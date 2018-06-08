---
title: Convertir un script Trace SQL existant en session d’événements étendus | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: xevents
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Trace, convert script to extended events
- extended events [SQL Server], convert SQL Trace script
ms.assetid: 4c8f29e6-0a37-490f-88b3-33493871b3f9
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 894818a0311d9a7f85c8991702bb924e12a67276
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="convert-an-existing-sql-trace-script-to-an-extended-events-session"></a>Convertir un script Trace SQL existant en session d'événements étendus
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Si vous disposez d'un script Trace SQL existant que vous souhaitez convertir en session d'événements étendus, vous pouvez utiliser les procédures décrites dans cette rubrique pour créer une session d'événements étendus équivalente. En utilisant les informations contenues dans les tables système trace_xe_action_map et trace_xe_event_map, vous pouvez recueillir les informations nécessaires pour effectuer la conversion.  
  
 Ces étapes sont les suivantes :  
  
1.  Exécutez le script existant pour créer une session Trace SQL, puis obtenez l'ID de la trace.  
  
2.  Exécutez une requête qui utilise la fonction fn_trace_geteventinfo pour rechercher les événements et actions d’événements étendus équivalents pour chaque classe d’événements Trace SQL et ses colonnes associées.  
  
3.  Utilisez la fonction fn_trace_getfilterinfo pour répertorier les filtres et les actions d’événements étendus équivalentes à utiliser.  
  
4.  Créez manuellement une session d'événements étendus, à l'aide des événements, des actions et des prédicats (filtres) d'événements étendus équivalents.  
  
## <a name="to-obtain-the-trace-id"></a>Pour obtenir l'ID de trace  
  
1.  Ouvrez le script Trace SQL dans l'Éditeur de requête, puis exécutez le script pour créer la session de trace. Notez qu'il n'est pas nécessaire que la session de trace soit en cours d'exécution pour exécuter cette procédure.  
  
2.  Obtenez l'ID de la trace. Pour cela, utilisez la requête suivante :  
  
    ```  
    SELECT * FROM sys.traces;  
    GO  
    ```  
  
    > [!NOTE]  
    >  L'ID de trace 1 indique en général la trace par défaut.  
  
## <a name="to-determine-the-extended-events-equivalents"></a>Pour déterminer les équivalents des événements étendus  
  
1.  Pour déterminer les événements et actions d’événements étendus équivalents, exécutez la requête suivante, où la valeur de l’ID de trace obtenue au cours de l’étape précédente est affectée à *trace_id* .  
  
    > [!NOTE]  
    >  Dans cet exemple, l'ID de trace pour la trace par défaut (1) est utilisé.  
  
    ```  
    USE MASTER;  
    GO  
    DECLARE @trace_id int;  
    SET @trace_id = 1;  
    SELECT DISTINCT el.eventid, em.package_name, em.xe_event_name AS 'event'  
       , el.columnid, ec.xe_action_name AS 'action'  
    FROM (sys.fn_trace_geteventinfo(@trace_id) AS el  
       LEFT OUTER JOIN sys.trace_xe_event_map AS em  
          ON el.eventid = em.trace_event_id)  
    LEFT OUTER JOIN sys.trace_xe_action_map AS ec  
       ON el.columnid = ec.trace_column_id  
    WHERE em.xe_event_name IS NOT NULL AND ec.xe_action_name IS NOT NULL;  
    ```  
  
     L'ID d'événement, le nom du package, le nom de l'événement, l'ID de colonne et le nom de l'action des événements étendus équivalents sont retournés. Vous utiliserez cette sortie dans la procédure « Pour créer une session d'événements étendus » plus loin dans cette rubrique.  
  
     Dans certains cas, la colonne filtrée est mappée à un champ de données d'événement inclus par défaut dans l'événement Événements étendus. Par conséquent, la colonne « Extended_Events_action_name » sera NULL. Si cela se produit, vous devez effectuer les opérations suivantes pour déterminer le champ de données qui est équivalent à la colonne filtrée :  
  
    1.  Pour les actions qui retournent NULL, identifiez quelles sont les classes d'événements Trace SQL dans le script qui contiennent la colonne filtrée.  
  
         Par exemple, vous avez peut-être utilisé la classe d’événements SP:StmtCompleted et spécifié un filtre sur le nom de colonne de trace Duration (ID 45 de classe d’événements Trace SQL, et ID 13 de colonne Trace SQL). Dans ce cas, le nom de l'action s'affichera comme NULL dans les résultats de la requête.  
  
    2.  Pour chaque classe d'événements Trace SQL que vous avez identifiée à l'étape précédente, recherchez le nom d'événement Événements étendus équivalent. (Si vous n’êtes pas sûr du nom de l’événement équivalent, utilisez la requête dans la rubrique [Consulter les Événements étendus équivalents aux classes d’événements Trace SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md).)  
  
    3.  Utilisez la requête suivante pour identifier les champs de données corrects à utiliser pour les événements que vous avez identifiés à l'étape précédente. La requête affiche les champs de données d'événements étendus dans la colonne « event_field ». Dans la requête, remplacez *<event_name>* par le nom d’un événement que vous avez spécifié à l’étape précédente.  
  
        ```  
        SELECT xp.name package_name, xe.name event_name  
           ,xc.name event_field, xc.description  
        FROM sys.trace_xe_event_map AS em  
        INNER JOIN sys.dm_xe_objects AS xe  
           ON em.xe_event_name = xe.name  
        INNER JOIN sys.dm_xe_packages AS xp  
           ON xe.package_guid = xp.guid AND em.package_name = xp.name  
        INNER JOIN sys.dm_xe_object_columns AS xc  
           ON xe.name = xc.object_name  
        WHERE xe.object_type = 'event' AND xc.column_type <> 'readonly'  
           AND em.xe_event_name = '<event_name>';  
        ```  
  
         Par exemple, la classe d’événements SP:StmtCompleted correspond à l’événement d’événements étendus sp_statement_completed. Si vous spécifiez sp_statement_completed comme nom d’événement dans la requête, la colonne « event_field » affiche les champs inclus par défaut avec l’événement. Si vous observez les champs, vous pouvez voir qu'il y a un champ de « durée ». Pour créer le filtre dans la session d'événements étendus équivalente, vous devez ajouter un prédicat tel que « WHERE duration > 0 ». Pour obtenir un exemple, consultez la procédure « Pour créer la session d'événements étendus » dans cette rubrique.  
  
## <a name="to-create-the-extended-events-session"></a>Pour créer la session d'événements étendus  
 Utilisez l'Éditeur de requête pour créer la session d'événements étendus, et écrire la sortie dans une cible de fichier. Les étapes suivantes décrivent une requête unique, avec les explications indiquant comment générer la requête. Pour obtenir un exemple de requête complet, consultez la section Exemple de cette rubrique.  
  
1.  Ajoutez les instructions pour créer la session d’événements, en remplaçant*session_name* par le nom à utiliser pour la session d’événements étendus.  
  
    ```  
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [Session_Name] ON SERVER;  
    CREATE EVENT SESSION [Session_Name]  
    ON SERVER;  
    ```  
  
2.  Ajoutez les événements d'événements étendus et les actions retournés comme sortie dans la procédure « Déterminer les équivalents d'événements étendus », puis ajoutez les prédicats (filtres) que vous avez identifiés dans la procédure « Pour déterminer les filtres utilisés dans le script ».  
  
     L’exemple suivant utilise un script Trace SQL qui inclut les classes d’événements SQL:StmtStarting et SP:StmtCompleted, avec les filtres pour l’ID et la durée de session. L'exemple de sortie pour la requête dans la procédure « Déterminer les équivalents d'événements étendus » a retourné le jeu de résultats suivant :  
  
    ```  
    Eventid  package_name  event                   columnid  action  
    44       sqlserver     sp_statement_starting   6         nt_username  
    44       sqlserver     sp_statement_starting   9         client_pid  
    44       sqlserver     sp_statement_starting   10        client_app_name  
    44       sqlserver     sp_statement_starting   11        server_principal_name  
    44       sqlserver     sp_statement_starting   12        session_id  
    45       sqlserver     sp_statement_completed  6         nt_username  
    45       sqlserver     sp_statement_completed  9         client_pid  
    45       sqlserver     sp_statement_completed  10        client_app_name  
    45       sqlserver     sp_statement_completed  11        server_principal_name  
    45       sqlserver     sp_statement_completed  12        session_id  
    ```  
  
     Pour convertir ceci en équivalents d’événements étendus, les événements sqlserver.sp_statement_starting et sqlserver.sp_statement_completed sont ajoutés, avec une liste d’actions. Les instructions de prédicat sont incluses comme clauses WHERE.  
  
    ```  
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       )  
    ```  
  
3.  Ajoutez la cible de fichier asynchrone, en remplaçant les chemins d'accès de fichier par l'emplacement où vous souhaitez enregistrer la sortie. Lorsque vous spécifiez la cible de fichier, vous devez inclure un fichier journal et un fichier du chemin d'accès du fichier de métadonnées.  
  
    ```  
    ADD TARGET package0.asynchronous_file_target(  
       SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="to-view-the-results"></a>Pour afficher les résultats  
  
1.  Vous pouvez utiliser la fonction sys.fn_xe_file_target_read_file pour afficher la sortie. Pour cela, exécutez la requête suivante, en remplaçant les chemins d'accès aux fichiers par les chemins d'accès que vous avez spécifiés :  
  
    ```  
    SELECT *, CAST(event_data as XML) AS 'event_data_XML'  
    FROM sys.fn_xe_file_target_read_file('c:\temp\ExtendedEventsStoredProcs*.xel', 'c:\temp\ExtendedEventsStoredProcs*.xem', NULL, NULL);  
  
    ```  
  
    > [!NOTE]  
    >  La conversion des données d'événement en tant que XML est facultative.  
  
     Pour plus d’informations sur la fonction sys.fn_xe_file_target_read_file, consultez [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
    ```  
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [session_name] ON SERVER;  
    CREATE EVENT SESSION [session_name]  
    ON SERVER  
  
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       );  
  
    ADD TARGET package0.asynchronous_file_target  
       (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="example"></a> Exemple  
  
```  
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
   DROP EVENT SESSION [session_name] ON SERVER;  
CREATE EVENT SESSION [session_name]  
ON SERVER  
  
ADD EVENT sqlserver.sp_statement_starting  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59   
   ),  
  
ADD EVENT sqlserver.sp_statement_completed  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59 AND duration > 0  
   )  
  
ADD TARGET package0.asynchronous_file_target  
   (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Consulter les Événements étendus équivalents aux classes d’événements Trace SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)  
  
  
