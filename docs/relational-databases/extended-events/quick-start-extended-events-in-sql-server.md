---
title: 'Démarrage rapide : événements étendus dans SQL Server | Microsoft Docs'
ms.custom: ''
ms.date: 09/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: xevents
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7bb78b25-3433-4edb-a2ec-c8b2fa58dea1
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 28dad124a7c4552418f103dc03d6893d5718632b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="quick-start-extended-events-in-sql-server"></a>Démarrage rapide : Événements étendus dans SQL Server
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]


Cet article vise à aider le développeur SQL qui ne connaît pas encore les événements étendus et qui veut créer une session d’événements en quelques minutes seulement. Grâce aux événements étendus, vous pouvez afficher des détails sur les opérations internes du système SQL et votre application. Quand vous créez une session d’événements étendus, vous indiquez au système :

- les occurrences qui vous intéresse,
- la manière dont vous voulez que le système vous indique les données.


Cet article :

- utilise des captures d’écran pour illustrer les clics dans SSMS.exe qui créent une session d’événements ;
- met en corrélation les captures d’écran avec les instructions Transact-SQL équivalentes ;
- explique en détail les termes et concepts liés aux clics et à T-SQL pour les sessions d’événements ;
- montre comment tester votre session d’événements ;
- décrit les alternatives autour des résultats :
  - capture du stockage des résultats,
  - résultats traités par rapport aux résultats bruts ;
  - présente les outils permettant d’afficher les résultats de différentes façons et sur différentes échelles de temps ;
- montre comment vous pouvez rechercher et découvrir tous les événements disponibles ;
- indique les relations de clé primaire et de clé étrangère implicites entre les vues de gestion dynamique (DMV) des événements étendus ;
- décrit ce que vous pouvez apprendre de plus dans les articles connexes.


Les blogs et autres fils de conversation informelle font parfois référence aux événements étendus par l’abréviation *xevents*.


> [!NOTE]
> Pour plus d’informations sur les différences des événements étendus entre Microsoft SQL Server et la base de données SQL Azure, consultez [Événements étendus dans la base de données SQL](http://azure.microsoft.com/documentation/articles/sql-database-xevent-db-diff-from-svr/).


## <a name="preparations-before-demo"></a>Préparations avant la démonstration


Les prérequis suivants sont incontournables pour effectuer réellement la démonstration qui suit.

1. [Télécharger SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx)
  - Chaque mois, vous devez installer la dernière mise à jour mensuelle de SSMS.
2. Connectez-vous à Microsoft SQL Server 2014 ou version ultérieure, ou à une base de données SQL Azure où `SELECT @@version` retourne une valeur dont le premier nœud est supérieur ou égal à 12.
3. Assurez-vous que votre compte dispose de l’ [autorisation de serveur](../../t-sql/statements/grant-server-permissions-transact-sql.md) **ALTER ANY EVENT SESSION**.
  - Si cela vous intéresse, vous trouverez d’autres informations sur la sécurité et les autorisations relatives aux événements étendus à la fin de cet article en [annexe](#appendix1).




## <a name="demo-of-ssms-integration"></a>Démonstration de l’intégration de SSMS


SSMS.exe offre une excellente interface utilisateur (IU) pour les événements étendus. L’interface utilisateur est tellement bien que de nombreux utilisateurs n’ont pas besoin d’aborder les événements étendus en utilisant Transact-SQL ou les vues de gestion dynamique (DMV) qui ciblent les événements étendus.

Dans cette section, vous pouvez voir les étapes d’interface utilisateur qui permettent de créer un événement étendu et d’afficher les données qu’il signale. Après les étapes, vous pouvez vous informer sur les concepts qu’elles impliquent afin d’en approfondir votre compréhension.


### <a name="steps-of-demo"></a>Étapes de démonstration


Vous pouvez comprendre les étapes même si vous décidez de ne pas les exécuter. La démonstration lance la boîte de dialogue **Nouvelle session** . Nous traitons ses quatre pages nommées :

- Général
- Événements
- Stockage de données
- Avancé


Le texte et les captures d’écran qui l’accompagnent peuvent perdre en précision à mesure que l’interface utilisateur de SSMS est modifiée au fil des mois ou années. Néanmoins, les captures d’écran restent efficaces à titre d’explication si les différences sont seulement mineures.


1. Connectez-vous à SSMS.

2. Dans l’Explorateur d’objets, cliquez sur **Gestion** > **Événements étendus** > **Nouvelle session**. La boîte de dialogue **nouvelle Session** est préférable à l’ **Assistant Nouvelle Session**, bien que les deux soient similaires.

3. Dans l’angle supérieur gauche, cliquez sur la page **Général** . Ensuite, tapez *YourSession*ou tout autre nom de votre choix, dans la zone de texte **Nom de session** . Ne cliquez *pas* encore sur le bouton **OK**, vous le ferez uniquement à la fin de la démonstration.

    ![Nouvelle Session > Général > Nom de session](../../relational-databases/extended-events/media/xevents-session-newsessions-10-general-ssms-yoursessionnode.png)

4. Dans l’angle supérieur gauche, cliquez sur la page **Événements** , puis cliquez sur le bouton **Sélectionner** .

    ![Nouvelle Session > Événements > Sélectionner > Bibliothèque d’événements, Événements sélectionnés](../../relational-databases/extended-events/media/xevents-session-newsessions-14-events-ssms-rightclick-not-wizard.png)

5. Dans la **Bibliothèque d’événements**, dans la liste déroulante, choisissez **Noms d’événements uniquement**.
    - Dans la zone de texte, tapez **sql**, ce qui filtre et réduit la longue liste des événements disponibles en utilisant un opérateur *contient* .
    - Faites défiler et cliquez sur l’événement nommé **sql_statement_completed**.
    - Cliquez sur le bouton représentant une flèche vers la droite **>** pour déplacer l’événement vers la zone **Événements sélectionnés** .

6. Toujours dans la page **Événements** , cliquez sur le bouton **Configurer** situé à l’extrémité droite.
    - Le côté gauche étant coupé pour améliorer la présentation, vous pouvez voir, dans la capture d’écran ci-après, la zone **Options de configuration d’événement**.

    ![Nouvelle session > Événements > Configurer > Filtre (prédicat) > Champ](../../relational-databases/extended-events/media/xevents-session-newsessions-20b-events-ssms-yoursessionnode.png)

7. Cliquez sur l’onglet **Filtre (prédicat)**. Ensuite, cliquez sur **Cliquez ici pour ajouter une clause**, dans le but de capturer toutes les instructions SQL SELECT qui ont une clause HAVING.

8. Dans la liste déroulante **Champ** , choisissez **sqlserver.sql_text**.
   - Pour **Opérateur** , choisissez un opérateur LIKE.
   - Pour **Valeur** , tapez **%SELECT%HAVING%**.

    > [!NOTE]
    > Dans ce nom en deux parties, *sqlserver* correspond au nom du package et *sql_text* au nom du champ. L’événement que nous avons choisi précédemment, *sql_statement_completed* doit être dans le même package que le champ que nous choisissons.

9. Dans l’angle supérieur gauche, cliquez sur la page **Stockage de données** .

10. Dans la zone **Cibles** , cliquez sur **Cliquez ici pour ajouter une cible**.
    - Dans la liste déroulante **Type** , choisissez **event_file**.
    - Cela signifie que les données d’événement seront stockées dans un fichier consultable.

    ![Nouvelle Session > Stockage de données > Cibles > Type > event_file](../../relational-databases/extended-events/media/xevents-session-newsessions-30-datastorage-ssms-yoursessionnode.png)

11. Dans la zone **Propriétés** , tapez un chemin et un nom de fichier dans la zone de texte **Nom de fichier sur le serveur** .
    - L’extension de nom de fichier doit être *.xel*.
    - Notre test requiert une taille de fichier inférieure à 1 Mo.

    ![Nouvelle session > Avancé > Latence maximale de répartition > OK](../../relational-databases/extended-events/media/xevents-session-newsessions-40-advanced-ssms-yoursessionnode.png)

12. Dans l’angle supérieur gauche, cliquez sur la page **Avancé**.
    - Réduisez la valeur **Latence maximale de répartition** à 3 secondes.
    - Enfin, cliquez sur le bouton **OK** situé en bas.

13. De retour dans l’**Explorateur d’objets**, développez **Gestion** > **Sessions**, puis observez la présence du nouveau nœud **YourSession**.

    ![Nœud de votre nouvelle *session d’événements* nommé YourSession, dans l’Explorateur d’objets, sous Gestion > Événements étendus > Sessions](../../relational-databases/extended-events/media/xevents-session-newsessions-50-objectexplorer-ssms-yoursessionnode.png)


#### <a name="edit-your-event-session"></a>Modifier votre session d’événements


Dans l’ **Explorateur d’objets**de SSMS, vous pouvez modifier votre session d’événements en double-cliquant sur son nœud, puis en cliquant sur **Propriétés**. La même boîte de dialogue constituée de plusieurs pages s’affiche.


### <a name="corresponding-t-sql-for-your-event-session"></a>T-SQL correspondant de votre session d’événements


Vous avez utilisé l’interface utilisateur de SSMS pour générer un script T-SQL qui a créé votre session d’événements. Vous pouvez voir le script généré comme suit :

- Cliquez avec le bouton droit sur le nœud de votre session, cliquez sur **Générer un script de la session en tant que** > **CREATE to** > **Presse-papiers**.
- Collez-le dans un éditeur de texte.


Ensuite vient l’instruction T-SQL CREATE EVENT SESSION pour *YourSession*, qui a été générée par vos clics dans l’interface utilisateur :


```sql
CREATE EVENT SESSION [YourSession]
    ON SERVER 
    ADD EVENT sqlserver.sql_statement_completed
    (
        ACTION(sqlserver.sql_text)
        WHERE
        ( [sqlserver].[like_i_sql_unicode_string]([sqlserver].[sql_text], N'%SELECT%HAVING%')
        )
    )
    ADD TARGET package0.event_file
    (SET
        filename = N'C:\Junk\YourSession_Target.xel',
        max_file_size = (2),
        max_rollover_files = (2)
    )
    WITH (
        MAX_MEMORY = 2048 KB,
        EVENT_RETENTION_MODE = ALLOW_MULTIPLE_EVENT_LOSS,
        MAX_DISPATCH_LATENCY = 3 SECONDS,
        MAX_EVENT_SIZE = 0 KB,
        MEMORY_PARTITION_MODE = NONE,
        TRACK_CAUSALITY = OFF,
        STARTUP_STATE = OFF
    );
GO
```


> [!NOTE]
> Pour la base de données SQL Azure, dans l’instruction CREATE EVENT SESSION précédente, la clause ON SERVER serait plutôt ON DATABASE.
> 
> Pour plus d’informations sur les différences des événements étendus entre Microsoft SQL Server et la base de données SQL Azure, consultez [Événements étendus dans la base de données SQL](http://azure.microsoft.com/documentation/articles/sql-database-xevent-db-diff-from-svr/).


#### <a name="pre-drop-of-the-event-session"></a>Instruction DROP préalable de la session d’événements


Avant l’instruction CREATE EVENT SESSION, vous pouvez émettre une instruction DROP EVENT SESSION à certaines conditions au cas où le nom existerait déjà.


```sql
IF EXISTS (SELECT *
      FROM sys.server_event_sessions    -- If Microsoft SQL Server.
    --FROM sys.database_event_sessions  -- If Azure SQL Database in the cloud.
      WHERE name = 'YourSession')
BEGIN
    DROP EVENT SESSION YourSession
          ON SERVER;    -- If Microsoft SQL Server.
        --ON DATABASE;  -- If Azure SQL Database.
END
go
```


#### <a name="alter-to-start-and-stop-the-event-session"></a>ALTER pour démarrer et arrêter la session d’événements


Quand vous créez une session d’événements, par défaut, elle ne démarre pas automatiquement. Vous pouvez démarrer ou arrêter votre session d’événements à tout moment à l’aide de l’instruction T-SQL ALTER EVENT SESSION suivante.


```sql
ALTER EVENT SESSION [YourSession]
      ON SERVER
    --ON DATABASE
    STATE = START;   -- STOP;
```


Vous avez la possibilité d’indiquer à la session d’événements de démarrer automatiquement au démarrage de l’instance de SQL Server. Consultez le mot clé **STARTUP STATE = ON** dans CREATE EVENT SESSION.

- L’interface utilisateur de SSMS propose une case à cocher correspondante dans la page **Nouvelle Session** > **Général** .


## <a name="test-your-event-session"></a>Tester votre session d’événements


Testez votre session d’événements à l’aide de ces quelques étapes simples :

1. Dans l’ **Explorateur d’objets**de SSMS, cliquez avec le bouton droit sur le nœud de votre session d’événements, puis cliquez sur **Démarrer la Session**.
2. Exécutez l’instruction `SELECT...HAVING` suivante à deux reprises.
    - Idéalement, vous pouvez modifier la valeur `HAVING Count` entre les deux exécutions, en la faisant passer de 2 à 3. Cela vous permet de voir les différences dans les résultats.
3. Cliquez avec le bouton droit sur le nœud de votre session, puis cliquez sur **Arrêter la session**.
4. Lisez la sous-section suivante sur la [manière de sélectionner et afficher les résultats](#select-the-full-results-xml-37).



```sql
SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o
    
            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) >= 3   --2     -- Try both values during session.
    ORDER BY
        c.name;
```


Par souci d’exhaustivité, voici la sortie approximative de l’instruction SELECT...HAVING précédente.


```
/*** Approximate output, 6 rows, all HAVING Count >= 3:
name                   Count-Per-Column-Repeated-Name
---------------------  ------------------------------
event_group_type       4
event_group_type_desc  4
event_session_address  5
event_session_id       5
is_trigger_event       4
trace_event_id         3
***/
```



<a name="select-the-full-results-xml-37"/>

### <a name="select-the-full-results-as-xml"></a>Sélectionner les résultats complets au format XML


Dans SSMS, exécutez l’instruction T-SQL SELECT suivante pour retourner les résultats où chaque ligne fournit les données sur une seule occurrence d’événement. L’instruction CAST AS XML facilite l’affichage des résultats.


> [!NOTE]
> Le système d’événements ajoute toujours un long nombre au nom de fichier event_file *.xel* que vous avez spécifié. Avant d’exécuter l’instruction SELECT suivante à partir du fichier, vous devez copier le nom complet fourni par le système et le coller dans l’instruction SELECT.


```sql
SELECT
        object_name,
        file_name,
        file_offset,
        event_data,
        'CLICK_NEXT_CELL_TO_BROWSE_XML RESULTS!'
                AS [CLICK_NEXT_CELL_TO_BROWSE_XML_RESULTS],
    
        CAST(event_data AS XML) AS [event_data_XML]
                -- TODO: In ssms.exe results grid, double-click this xml cell!
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\Junk\YourSession_Target_0_131085363367310000.xel',
            null, null, null
        );
```


L’instruction SELECT précédente vous offre deux manières d’afficher les résultats complets de toute ligne d’événement :

- Exécutez l’instruction SELECT dans SSMS, puis cliquez sur une cellule dans la colonne **event_data_XML** . Cette méthode est très pratique.
- Copiez la longue chaîne XML à partir d’une cellule dans la colonne **event_data** . Collez-la dans un éditeur de texte simple comme Bloc-notes.exe et enregistrez-la dans un fichier portant l’extension .XML. Ensuite, ouvrez le fichier .XML avec un navigateur.


#### <a name="display-of-results-for-one-event"></a>Affichage des résultats d’un seul événement


Ensuite, nous observons qu’une partie des résultats est au format XML. Ce code XML ici est modifié pour qu’il soit plus court à afficher. Notez que `<data name="row_count">` affiche une valeur `6`, qui correspond à nos 6 lignes de résultat précédemment affichées. Et nous pouvons voir l’instruction SELECT entière.


```xml
<event name="sql_statement_completed" package="sqlserver" timestamp="2016-05-24T04:06:08.997Z">
  <data name="duration">
    <value>111021</value>
  </data>
  <data name="cpu_time">
    <value>109000</value>
  </data>
  <data name="physical_reads">
    <value>0</value>
  </data>
  <data name="last_row_count">
    <value>6</value>
  </data>
  <data name="offset">
    <value>0</value>
  </data>
  <data name="offset_end">
    <value>584</value>
  </data>
  <data name="statement">
    <value>SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o

            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) &gt;= 3   --2     -- Try both values during session.
    ORDER BY
        c.name</value>
  </data>
</event>
```


## <a name="ssms-to-display-results"></a>SSMS pour afficher les résultats


Il existe plusieurs fonctionnalités avancées dans l’interface utilisateur de SSMS qui vous permettent d’afficher les données capturées à partir d’un événement étendu. Des informations détaillées sont données ici :

- [Affichage avancé des données cibles à partir d’événements étendus dans SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)


Pour commencer, deux options de menu contextuel intitulées **Afficher les données cibles** et **Surveiller les données actives**sont proposées.


### <a name="view-target-data"></a>Afficher les données cibles


Dans l’ **Explorateur d’objets**de SSMS, vous pouvez cliquer avec le bouton droit sur le nœud cible qui se trouve sous le nœud de votre session d’événements. Dans le menu contextuel, cliquez sur **Afficher les données cibles**. SSMS affiche les données.

L’affichage n’est pas mis à jour si de nouvelles données sont signalées par l’événement. Mais vous pouvez cliquer de nouveau sur **Afficher les données cibles** .


![Afficher les données cibles, dans SSMS, Gestion > Événements étendus > Sessions > YourSession > package0.event_file, clic avec le bouton droit](../../relational-databases/extended-events/media/xevents-viewtargetdata-ssms-targetnode-61.png)


### <a name="watch-live-data"></a>Surveiller les données actives


Dans l’ **Explorateur d’objets**de SSMS, vous pouvez cliquer avec le bouton droit sur le nœud de votre session d’événements. Dans le menu contextuel, cliquez sur **Surveiller les données actives**. SSMS affiche les données entrantes au fur et à mesure qu’elles arrivent en temps réel.


![Surveiller les données actives, dans SSMS, Gestion > Événements étendus > Sessions > YourSession, clic avec le bouton droit](../../relational-databases/extended-events/media/xevents-watchlivedata-ssms-yoursessionnode-63.png)


## <a name="scenarios"></a>Scénarios


Il existe d’innombrables scénarios d’utilisation efficace des événements étendus. Les articles suivants donnent des exemples de scénarios qui impliquent les verrous utilisés pendant les requêtes.


Les scénarios spécifiques des sessions d’événements dont le but est d’évaluer des verrous sont décrits dans les articles suivants. Les articles présentent également certaines techniques avancées, telles que l’utilisation de **@dbid**et de l’instruction `EXECUTE (@YourSqlString)`dynamique :

- [Trouver les objets comportant le plus de verrous](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)
  - Ce scénario utilise l’objet package0.histogram cible, qui traite les données d’événement brutes avant de vous les afficher.
- [Déterminer quelles requêtes détiennent des verrous](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)
  - Ce scénario utilise l’objet [package0.pair_matching cible](http://msdn.microsoft.com/library/3c87dcfb-543a-4bd8-a73d-1390bdf4ffa3), où la paire d’événements comporte sqlserver.lock_acquire et lock_release.


## <a name="terms-and-concepts-in-extended-events"></a>Termes et concepts liés aux événements étendus


Le tableau suivant répertorie les termes utilisés pour les événements étendus et en donne la signification.


| Terme | Description |
| :--- | :---------- |
| session d'événements | Construction centrée autour d’un ou plusieurs événements, associées à des éléments comme des actions et des cibles. L’instruction CREATE EVENT SESSION construit chaque session d’événements. Vous pouvez utiliser l’instruction ALTER sur une session d’événements pour la démarrer et l’arrêter à votre gré. <br/> <br/> Une session d’événements est parfois simplement appelée *session*. Quand le contexte le précise, il s’agit d’une *session d’événements*. <br/> <br/> D’autres détails sur les sessions d’événements sont donnés dans : [Sessions d’événements étendus SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md). |
| événement | Occurrence spécifique dans le système qui est surveillée par une session d’événements active. <br/> <br/> Par exemple, l’événement *sql_statement_completed* représente le moment auquel une instruction T-SQL donnée se termine. L’événement peut signaler sa durée et d’autres données. |
| target | Élément qui reçoit les données de sortie d’un événement capturé. La cible vous affiche les données. <br/> <br/> Exemples : *event_file*et la mémoire *ring_buffer*. La cible *histogram* traite vos données avant de les afficher. <br/> <br/> Toute cible peut être utilisée pendant une session d’événements. Pour plus d’informations, consultez [Cibles des Événements étendus SQL Server](../../relational-databases/extended-events/targets-for-extended-events-in-sql-server.md). |
| action | Champ connu de l’événement. Les données issues du champ sont envoyées à la cible. Le champ d’action est étroitement lié au *filtre de prédicat*. |
| filtre de prédicat | Test de données d’un champ d’événement, utilisé pour que seul un sous-ensemble intéressant d’occurrences d’événements soit envoyé à la cible. <br/> <br/> Par exemple, un filtre peut inclure uniquement les occurrences d’événements *sql_statement_completed* où l’instruction T-SQL contient la chaîne *HAVING*. |
| package | Qualificateur de nom associé à chaque élément dans un ensemble d’éléments qui tournent autour d’un cœur d’événements. <br/> <br/> Par exemple, un package peut comporter des événements relatifs à du texte T-SQL. Un événement peut concerner toute l’instruction T-SQL dans un lot délimité par une commande GO. Dans le même temps, un autre événement plus précis concerne des instructions T-SQL individuelles. De plus, pour toute instruction T-SQL, il existe des événements de début et de fin. <br/> <br/> Les champs appropriés aux événements sont également inclus dans le package avec les événements. La plupart des cibles sont dans *package0* et sont utilisées avec les événements de nombreux autres packages. |


## <a name="how-to-discover-the-available-events-in-packages"></a>Comment découvrir les événements disponibles dans des packages


L’instruction T-SQL SELECT suivante retourne une ligne pour chaque événement disponible dont le nom contient la chaîne de trois caractères 'sql'. Bien sûr, vous pouvez modifier la valeur LIKE pour rechercher d’autres noms d’événements. Les lignes nomment également le package qui contient l’événement.


```sql
SELECT   -- Find an event you want.
        p.name         AS [Package-Name],
        o.object_type,
        o.name         AS [Object-Name],
        o.description  AS [Object-Descr],
        p.guid         AS [Package-Guid]
    FROM
              sys.dm_xe_packages  AS p
        JOIN  sys.dm_xe_objects   AS o
    
                ON  p.guid = o.package_guid
    WHERE
        o.object_type = 'event'   --'action'  --'target'
        AND
        p.name LIKE '%'
        AND
        o.name LIKE '%sql%'
    ORDER BY
        p.name, o.object_type, o.name;
```


L’affichage suivant montre la ligne retournée, modifiée ici au format column name = value. Les données sont issues de l’événement *sql-statement_completed* qui était utilisé dans les exemples d’étapes précédents. La phrase de la colonne Object-Descr s’avère particulièrement utile.


```  
Package-Name = sqlserver
object_type  = event
Object-Name  = sql_statement_completed
Object-Descr = Occurs when a Transact-SQL statement has completed.
Package-Guid = 655FD93F-3364-40D5-B2BA-330F7FFB6491
```


#### <a name="ssms-ui-for-search"></a>Interface utilisateur de SSMS pour la recherche


Une autre option de recherche consiste à utiliser l’interface utilisateur de SSMS et sa boîte de dialogue **Nouvelle session** > **Événements** > **Bibliothèque d’événements** illustrée dans une capture d’écran précédente.



#### <a name="sql-trace-event-classes-with-extended-events"></a>Classes d’événements de trace SQL, avec des événements étendus


Vous trouverez la description de l’utilisation des événements étendus avec les classes d’événements de trace SQL et les colonnes ici : [Consulter les événements étendus équivalents aux classes d’événements de trace SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)



#### <a name="event-tracing-for-windows-etw-with-extended-events"></a>Suivi d’événements pour Windows (ETW), avec des événements étendus


Vous trouverez des descriptions de l’utilisation des événements étendus avec le suivi d’événements pour Windows (ETW) ici :

- [Cible du suivi d'événements pour Windows](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [Surveiller l’activité système à l’aide d’événements étendus](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)



Les événements ETW ne sont pas disponibles dans les événements étendus dans la base de données SQL Azure.



## <a name="additional-items"></a>Éléments supplémentaires


Cette section mentionne brièvement quelques éléments divers.


### <a name="event-sessions-installed-with-sql-server"></a>Sessions d’événements installées avec SQL Server


SQL Server est fourni avec quelques événements étendus déjà créés. Tous sont configurés pour démarrer dès que le système SQL démarre. Ces sessions d’événements rassemblent des données qui peuvent s’avérer utiles en cas d’erreur système. Comme tous les événements étendus, ils utilisent uniquement une minuscule quantité de ressources et Microsoft recommande de les laisser s’exécuter tout seuls.

Vous pouvez voir ces sessions d’événements dans l’ **Explorateur d’objets** de SSMS sous **Gestion** > **Événements étendus** > **Sessions**.  Depuis juin 2016, la liste de ces sessions d’événements installées est la suivante :

- AlwaysOn_health
- system_health
- telemetry_events



### <a name="powershell-provider-for-extended-events"></a>Fournisseur PowerShell pour les événements étendus


Vous pouvez gérer les événements étendus SQL Server à l’aide du fournisseur PowerShell SQL Server. Pour plus d’informations : [Utiliser le fournisseur PowerShell pour les événements étendus](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)


### <a name="system-views-for-extended-events"></a>Vues système pour les événements étendus


Les vues système pour les événements étendus sont les suivantes :

- *Affichages catalogue :* pour plus d’informations sur les sessions d’événements définies par CREATE EVENT SESSION.

- *Vues de gestion dynamique (DMV) :* pour plus d’informations sur les sessions d’événements activement en cours d’exécution.


[Instructions SELECT et JOIN des vues système pour les événements étendus dans SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) - fournit des informations sur :


- La manière de regrouper les vues ensemble.


- Plusieurs instructions SELECT utiles à partir des vues.


- La corrélation entre :
    - Les colonnes des vues.
    - Les clauses CREATE EVENT SESSION.
    - Les contrôles d’interface utilisateur de SSMS.


<a name="appendix1"></a>
## <a name="appendix-selects-to-ascertain-permission-owner-in-advance"></a>Annexe : instructions SELECT pour vérifier le propriétaire de l’autorisation à l’avance


Les autorisations mentionnées dans cet article sont :

- ALTER ANY EVENT SESSION
- VIEW SERVER STATE
- CONTROL SERVER

Les instructions Transact-SQL SELECT suivantes peuvent identifier les détenteurs de ces autorisations.


#### <a name="union-direct-permissions-plus-role-derived-permissions"></a>Autorisations directes UNION et autorisations dérivées des rôles


L’instruction SELECT...UNION ALL suivante retourne des lignes qui identifient les détenteurs des autorisations nécessaires pour créer des sessions d’événements et interroger les affichages catalogue système sur les événements étendus.


```sql
-- Ascertain who has the permissions listed in the ON clause.
-- 'CONTROL SERVER' permission includes the permissions
-- 'ALTER ANY EVENT SESSION' and 'VIEW SERVER STATE'.
SELECT
        'Owner-is-Principal'  AS [Type-That-Owns-Permission],
        NULL                  AS [Role-Name],
        prin.name             AS [Owner-Name],

        perm.permission_name
            COLLATE Latin1_General_CI_AS_KS_WS
            AS [Permission-Name]
    FROM
             sys.server_permissions  AS perm
        JOIN sys.server_principals   AS prin

            ON prin.principal_id = perm.grantee_principal_id
    WHERE
        perm.permission_name IN
            ('ALTER ANY EVENT SESSION',
            'VIEW SERVER STATE',
            'CONTROL SERVER')
UNION ALL

-- Plus check for members of the 'sysadmin' fixed server role,
-- because 'sysadmin' includes the 'CONTROL SERVER' permission.
SELECT
        'Owner-is-Role',
        prin.name,  -- [Role-Name]

        CAST( (IsNull(pri2.name, N'No members'))
            AS nvarchar(128)),

        NULL
    FROM
                         sys.server_role_members  AS rolm
        RIGHT OUTER JOIN sys.server_principals    AS prin

            ON prin.principal_id = rolm.role_principal_id

        LEFT OUTER JOIN sys.server_principals     AS pri2

            ON rolm.member_principal_id = pri2.principal_id
    WHERE
        prin.name = 'sysadmin'
    ORDER BY
        1,2,3,4;
```


#### <a name="haspermsbyname-function"></a>HAS_PERMS_BY_NAME, fonction


L’instruction SELECT suivante indique vos autorisations. Elle s’appuie sur la fonction intégrée [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md).

De plus, si vous êtes autorisé à temporairement *emprunter l’identité* d’autres comptes, vous pouvez ne pas commenter les instructions [EXECUTE AS LOGIN](../../t-sql/statements/execute-as-transact-sql.md) et REVERT, pour obtenir des informations sur d’autres comptes.


```sql
--EXECUTE AS LOGIN = 'AccountNameHere';
SELECT HAS_PERMS_BY_NAME(
    null, null,
    'ALTER ANY EVENT SESSION'
    );
--REVERT;
```


#### <a name="security-links"></a>Liens de sécurité

Voici des liens vers la documentation relative à ces instructions SELECT et les autorisations :

- Détails de la fonction intégrée [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)
- [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)
- [GRANT – octroi d'autorisations de serveur (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)
- [sys.server_principals (Transact-SQL)](http://msdn.microsoft.com/library/ms188786.aspx)
- Pour la base de données SQL Azure en particulier, [sys.database_principals (Transact-SQL)](http://msdn.microsoft.com/library/ms187328.aspx)
- Blog : [Autorisations de moteur de base de données en vigueur](http://social.technet.microsoft.com/wiki/contents/articles/15180.effective-database-engine-permissions.aspx)
- [Poster](https://aka.ms/sql-permissions-poster)zoomable, au format PDF, qui présente la hiérarchie de toutes les autorisations SQL Server.



## <a name="links-to-supporting-information"></a>Liens vers des informations connexes


- [sys.fn_xe_file_target_read_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)


