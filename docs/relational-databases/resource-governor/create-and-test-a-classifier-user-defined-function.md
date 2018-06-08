---
title: Créer et tester une fonction classifieur définie par l’utilisateur | Microsoft Docs
ms.custom: ''
ms.date: 07/11/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, classifier function create
- classifier function [SQL Server], test
- classifier function [SQL Server], create
- Resource Governor, classifier function test
ms.assetid: 7866b3c9-385b-40c6-aca5-32d3337032be
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2b65e9f0d7706520362281d6da456a6748e3a12e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="create-and-test-a-classifier-user-defined-function"></a>Créer et tester une fonction classifieur définie par l'utilisateur
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique indique comment créer et tester une fonction définie par l'utilisateur classifieur (UDF). Les étapes impliquent l’exécution d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans l’Éditeur de requêtes [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
 L'exemple de la procédure suivante illustre les possibilités de création d'une fonction définie par l'utilisateur classifieur assez complexe.  
  
 Dans notre exemple :  
  
-   Un pool de ressources (pProductionProcessing) et un groupe de charges de travail (gProductionProcessing) sont créés pour le traitement de la production pendant une plage temporelle spécifiée.  
  
-   Un pool de ressources (pOffHoursProcessing) et un groupe de charges de travail (gOffHoursProcessing) sont créés pour gérer les connexions qui ne répondent pas aux besoins du traitement de production.  
  
-   Une table (TblClassificationTimeTable) est créée dans la base de données master afin de contenir les heures de début et de fin qui peuvent être évaluées par rapport à une heure de connexion. Cette table doit être créée dans la base de données master car Resource Governor utilise la liaison de schéma pour les fonctions classifieur.  
  
    > [!NOTE]  
    >  Il est recommandé de ne pas stocker de grandes tables fréquemment mises à jour dans la base de données master.  
  
 La fonction classifieur étend le temps de connexion. Une fonction trop complexe peut provoquer l'expiration des délais d'attente de connexion ou ralentir les connexions rapides.  
  
## <a name="to-create-the-classifier-user-defined-function"></a>Pour créer la fonction classifieur définie par l'utilisateur  
  
1.  Créez et configurez les nouveaux pools de ressources et groupes de charges de travail. Affectez chaque groupe de charge de travail au pool de ressources approprié.  
  
    ```  
    --- Create a resource pool for production processing  
    --- and set limits.  
    USE master;  
    GO  
    CREATE RESOURCE POOL pProductionProcessing  
    WITH  
    (  
         MAX_CPU_PERCENT = 100,  
         MIN_CPU_PERCENT = 50  
    );  
    GO  
    --- Create a workload group for production processing  
    --- and configure the relative importance.  
    CREATE WORKLOAD GROUP gProductionProcessing  
    WITH  
    (  
         IMPORTANCE = MEDIUM  
    );  
    --- Assign the workload group to the production processing  
    --- resource pool.  
    USING pProductionProcessing  
    GO  
    --- Create a resource pool for off-hours processing  
    --- and set limits.  
  
    CREATE RESOURCE POOL pOffHoursProcessing  
    WITH  
    (  
         MAX_CPU_PERCENT = 50,  
         MIN_CPU_PERCENT = 0  
    );  
    GO  
    --- Create a workload group for off-hours processing  
    --- and configure the relative importance.  
    CREATE WORKLOAD GROUP gOffHoursProcessing  
    WITH  
    (  
         IMPORTANCE = LOW  
    )  
    --- Assign the workload group to the off-hours processing  
    --- resource pool.  
    USING pOffHoursProcessing;  
    GO  
    ```  
  
2.  Mettez à jour la configuration en mémoire.  
  
    ```  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    GO  
    ```  
  
3.  Créez une table et définissez les heures de début et de fin pour la plage temporelle de traitement de production.  
  
    ```  
    USE master;  
    GO  
    CREATE TABLE tblClassificationTimeTable  
    (  
         strGroupName     sysname          not null,  
         tStartTime       time              not null,  
         tEndTime         time              not null  
    );  
    GO  
    --- Add time values that the classifier will use to  
    --- determine the workload group for a session.  
    INSERT into tblClassificationTimeTable VALUES('gProductionProcessing', '6:35 AM', '6:15 PM');  
    go  
    ```  
  
4.  Créez la fonction classifieur qui utilise des fonctions d'heure et des valeurs qui peuvent être évaluées par rapport aux heures figurant dans la table de recherche. Pour plus d'informations sur l'utilisation des tables de recherche dans une fonction classifieur, consultez la section « Meilleures pratiques recommandées pour l'utilisation de tables de recherche dans une fonction classifieur » dans cette rubrique.  
  
    > [!NOTE]  
    >  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] introduit un jeu étendu de types de données et de fonctions de date et d’heure. Pour plus d’informations, consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
    ```  
    CREATE FUNCTION fnTimeClassifier()  
    RETURNS sysname  
    WITH SCHEMABINDING  
    AS  
    BEGIN  
    /* We recommend running the classifier function code under 
    snapshot isolation level OR using NOLOCK hint to avoid blocking on 
    lookup table. In this example, we are using NOLOCK hint. */
         DECLARE @strGroup sysname  
         DECLARE @loginTime time  
         SET @loginTime = CONVERT(time,GETDATE())  
         SELECT TOP 1 @strGroup = strGroupName  
              FROM dbo.tblClassificationTimeTable WITH(NOLOCK)
              WHERE tStartTime <= @loginTime and tEndTime >= @loginTime  
         IF(@strGroup is not null)  
         BEGIN  
              RETURN @strGroup  
         END  
    --- Use the default workload group if there is no match  
    --- on the lookup.  
         RETURN N'gOffHoursProcessing'  
    END;  
    GO  
    ```  
  
5.  Inscrivez la fonction classifieur et mettez à jour la configuration en mémoire.  
  
    ```  
    ALTER RESOURCE GOVERNOR with (CLASSIFIER_FUNCTION = dbo.fnTimeClassifier);  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    GO  
    ```  
  
## <a name="to-verify-the-resource-pools-workload-groups-and-the-classifier-user-defined-function"></a>Pour vérifier les pools de ressources, les groupes de charges de travail et la fonction définie par l'utilisateur classifieur  
  
1.  Obtenez la configuration de pool de ressources et de groupe de charges de travail à l'aide de la requête suivante.  
  
    ```  
    USE master;  
    SELECT * FROM sys.resource_governor_resource_pools;  
    SELECT * FROM sys.resource_governor_workload_groups;  
    GO  
    ```  
  
2.  Vérifiez que la fonction classifieur existe et qu'elle est activée en utilisant les requêtes suivantes.  
  
    ```  
    --- Get the classifier function Id and state (enabled).  
    SELECT * FROM sys.resource_governor_configuration;  
    GO  
    --- Get the classifer function name and the name of the schema  
    --- that it is bound to.  
    SELECT   
          object_schema_name(classifier_function_id) AS [schema_name],  
          object_name(classifier_function_id) AS [function_name]  
    FROM sys.dm_resource_governor_configuration;  
    ```  
  
3.  Obtenez les données d'exécution actuelles pour les pools de ressources et groupes de charges de travail en utilisant la requête suivante.  
  
    ```  
    SELECT * FROM sys.dm_resource_governor_resource_pools;  
    SELECT * FROM sys.dm_resource_governor_workload_groups;  
    GO  
    ```  
  
4.  Déterminez quelles sessions se trouvent dans chaque de groupe en utilisant la requête suivante.  
  
    ```  
    SELECT s.group_id, CAST(g.name as nvarchar(20)), s.session_id, s.login_time, 
        CAST(s.host_name as nvarchar(20)), CAST(s.program_name AS nvarchar(20))  
    FROM sys.dm_exec_sessions AS s  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = s.group_id  
    ORDER BY g.name;  
    GO  
    ```  
  
5.  Déterminez quelles demandes se trouvent dans chaque groupe en utilisant la requête suivante.  
  
    ```  
    SELECT r.group_id, g.name, r.status, r.session_id, r.request_id, 
        r.start_time, r.command, r.sql_handle, t.text   
    FROM sys.dm_exec_requests AS r  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = r.group_id  
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t  
    ORDER BY g.name;  
    GO  
    ```  
  
6.  Déterminez quelles demandes s'exécutent dans la fonction classifieur en utilisant la requête suivante.  
  
    ```  
    SELECT s.group_id, g.name, s.session_id, s.login_time, s.host_name, s.program_name   
    FROM sys.dm_exec_sessions AS s  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = s.group_id  
           AND 'preconnect' = s.status  
    ORDER BY g.name;  
    GO  
  
    SELECT r.group_id, g.name, r.status, r.session_id, r.request_id, r.start_time, 
        r.command, r.sql_handle, t.text   
    FROM sys.dm_exec_requests AS r  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = r.group_id  
           AND 'preconnect' = r.status  
     CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t  
    ORDER BY g.name;  
    GO  
    ```  
  
## <a name="best-practices-for-using-lookup-tables-in-a-classifier-function"></a>Meilleures pratiques recommandées pour l'utilisation de tables de recherche dans une fonction classifieur  
  
1.  N'utilisez pas de table de recherche sauf en cas d'absolue nécessité. Si vous devez utiliser une table de recherche, vous pouvez la coder de façon irréversible dans la fonction elle-même ; toutefois, cette action doit être équilibrée avec la complexité et les modifications dynamiques de la fonction classifieur.  
  
2.  Limitez les E/S effectuées pour les tables de recherche.  
  
    1.  Utilisez TOP 1 pour ne retourner qu'une seule ligne.  
  
    2.  Réduisez le nombre de lignes présentes dans la table.  
  
    3.  Faites en sorte que toutes les lignes de la table s'affichent sur une seule page ou sur un petit nombre de pages.  
  
    4.  Vérifiez que les lignes trouvées à l'aide des opérations de recherche d'index utilisent autant de colonnes de recherche que possible.  
  
    5.  Dénormalisez une seule table si vous envisagez d'utiliser plusieurs tables comportant des jointures.  
  
3.  Empêchez tout blocage sur la table de recherche.  
  
    1.  Utilisez l'indicateur `NOLOCK` pour empêcher tout blocage ou utilisez `SET LOCK_TIMEOUT` dans la fonction avec une valeur maximale de 1 000 millisecondes.  
  
    2.  Les tables doivent exister dans la base de données master. (La base de données master est la seule base de données dont la récupération est garantie lorsque les ordinateurs clients essaient de se connecter.)  
  
    3.  Qualifiez toujours entièrement le nom de la table avec le schéma. Le nom de la base de données n'est pas nécessaire dans la mesure où il doit s'agir de la base de données master.  
  
    4.  Pas de déclencheurs sur la table.  
  
    5.  Si vous mettez à jour le contenu de la table, veillez à utiliser une transaction de niveau d’isolement de capture instantanée dans la fonction classifieur afin d’éviter que l’enregistreur ne bloque les lecteurs. Notez que l'utilisation de l'indicateur `NOLOCK` doit également réduire cet effet.  
  
    6.  Si possible, désactivez la fonction classifieur lorsque vous modifiez le contenu de la table.  
  
        > [!WARNING]  
        >  Nous vous recommandons vivement d'appliquer ces meilleures pratiques. Si des problèmes vous empêchent de les mettre en œuvre, nous vous recommandons de contacter le suport technique de Microsoft afin de d'éviter d'éventuels problèmes futurs.  
  
## <a name="see-also"></a> Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Groupe de charge de travail de Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Configurer le gouverneur de ressources à l'aide d'un modèle](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [Afficher les propriétés de Resource Governor](../../relational-databases/resource-governor/view-resource-governor-properties.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
