---
title: Table temporaire et variable de table plus rapides à l’aide de l’optimisation en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 10/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 38512a22-7e63-436f-9c13-dde7cf5c2202
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: bce01856dd7652b824e8ef38e06fb75c5cc63be4
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="faster-temp-table-and-table-variable-by-using-memory-optimization"></a>Table temporaire et variable de table plus rapides à l’aide de l’optimisation en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  
Si vous utilisez des tables temporaires, des variables de table ou des paramètres table, envisagez de les convertir pour tirer parti des tables optimisées en mémoire et des variables de table afin d’améliorer les performances. Les modifications de code sont généralement minimes.  
  
Cet article décrit :  
  
- Scénarios en faveur d’une conversion en mémoire.  
- Étapes techniques pour implémenter les conversions en mémoire.  
- Configuration requise avant la conversion en mémoire.  
- Exemple de code qui met en évidence les avantages en matière de performances de l’optimisation en mémoire
  
  
## <a name="a-basics-of-memory-optimized-table-variables"></a>A. Principes de base des variables de table optimisées en mémoire  
  
Une variable de table optimisée en mémoire offre une grande efficacité en utilisant les mêmes algorithme et structures de données optimisés en mémoire que ceux utilisés par les tables optimisées en mémoire. L’efficacité est optimale quand la variable de table est accessible à partir d’un module compilé en mode natif.  
  
  
Une variable de table optimisée en mémoire :  
  
- Est stockée uniquement en mémoire et n’a aucun composant sur le disque.  
- N’implique aucune activité d’E/S.  
- N’implique aucune contention ni utilisation de tempdb.  
- Peut être passée dans une procédure stockée comme un paramètre table.  
- Doit avoir au moins un index, de hachage ou non cluster.  
  - Pour un index de hachage, le nombre de compartiments doit idéalement être égal à 1 à 2 fois le nombre de clés d’index uniques attendu, mais une surestimation du nombre de compartiments convient habituellement (jusqu’à 10 fois). Pour plus d’informations, consultez [Index pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).  

  
  
#### <a name="object-types"></a>Types d'objets  
  
OLTP en mémoire fournit les objets suivants qui peuvent être utilisés pour l’optimisation en mémoire des tables temporaires et des variables de table :  
  
- Tables optimisées en mémoire  
  - Durabilité = SCHEMA_ONLY  
- Variables de table optimisées en mémoire  
  - Déclaration en deux étapes (plutôt qu’inline) :  
    - `CREATE TYPE my_type AS TABLE ...;` , puis  
    - `DECLARE @mytablevariable my_type;`.  
  
  
## <a name="b-scenario-replace-global-tempdb-x23x23table"></a>B. Scénario : Remplacer la table temporaire globale &#x23;&#x23;tempGlobalB  
  
Le remplacement d’une table temporaire globale par une table SCHEMA_ONLY à mémoire optimisée est assez simple. La plus grande différence est de créer la table au moment du déploiement, et non de l’exécution. La création de tables à mémoire optimisée est plus longue que la création de tables traditionnelles en raison des optimisations au moment de la compilation. La création et la suppression de tables à mémoire optimisée dans le cadre de la charge de travail en ligne impactent les performances de la charge de travail, ainsi que les performances de restauration par progression sur les bases de données secondaires AlwaysOn et la récupération des bases de données.

Supposons que vous disposez de la table temporaire globale suivante.  
  
  
  
  
    CREATE TABLE ##tempGlobalB  
    (  
        Column1   INT   NOT NULL ,  
        Column2   NVARCHAR(4000)  
    );  
  
  
  
  
Envisagez de remplacer la table temporaire globale par la table optimisée en mémoire suivante qui affiche DURABILITY = SCHEMA_ONLY.  
  
  
  
  
    CREATE TABLE dbo.soGlobalB  
    (  
        Column1   INT   NOT NULL   INDEX ix1 NONCLUSTERED,  
        Column2   NVARCHAR(4000)  
    )  
        WITH  
            (MEMORY_OPTIMIZED = ON,  
            DURABILITY        = SCHEMA_ONLY);  
  
  
  
  
#### <a name="b1-steps"></a>B.1 Étapes  
  
La conversion de la table temporaire globale en SCHEMA_ONLY s’effectue comme suit :  
  
  
1. Créez la table **dbo.soGlobalB** unique comme n’importe quelle table sur disque classique.  
2. Dans votre code Transact-SQL, supprimez la création de la table **&#x23;&#x23;tempGlobalB**.  Il est important de créer la table à mémoire optimisée au moment du déploiement, et non de l’exécution, pour éviter la surcharge de compilation qui accompagne la création de la table.
3. Dans votre code T-SQL, remplacez toutes les mentions de **&#x23;&#x23;tempGlobalB** par **dbo.soGlobalB**.  
  
  
## <a name="c-scenario-replace-session-tempdb-x23table"></a>C. Scénario : Remplacer la table temporaire de session &#x23;tempSessionC  
  
Les tâches de préparation pour remplacer une table temporaire de session impliquent plus de code T-SQL que pour le scénario de table temporaire globale précédent. Heureusement, le code T-SQL supplémentaire n’implique pas plus de travail pour effectuer la conversion.  

Comme avec le scénario de la table temporaire globale, la plus grande différence consiste à créer la table au moment du déploiement et non de l’exécution, afin d’éviter la surcharge de la compilation.
  
Supposons que vous disposez de la table temporaire de session suivante.  
  
  
  
  
    CREATE TABLE #tempSessionC  
    (  
        Column1   INT   NOT NULL ,  
        Column2   NVARCHAR(4000)  
    );  
  
  
  
  
Tout d’abord, créez la fonction table suivante pour filtrer sur **@@spid**. La fonction sera utilisable par toutes les tables SCHEMA_ONLY que vous convertissez à partir des tables temporaires de session.  
  
  
  
  
    CREATE FUNCTION dbo.fn_SpidFilter(@SpidFilter smallint)  
        RETURNS TABLE  
        WITH SCHEMABINDING , NATIVE_COMPILATION  
    AS  
        RETURN  
            SELECT 1 AS fn_SpidFilter  
                WHERE @SpidFilter = @@spid;  
  
  
  
  
Ensuite, créez la table SCHEMA_ONLY, ainsi qu’une stratégie de sécurité sur la table.  
  
  
Notez que chaque table optimisée en mémoire doit avoir au moins un index.  
  
- Pour la table dbo.soSessionC, un index de hachage peut être préférable, si nous calculons la valeur BUCKET_COUNT appropriée. Toutefois, pour cet exemple, nous simplifions l’opération avec un index non cluster.  
  
  
  
  
    CREATE TABLE dbo.soSessionC  
    (  
        Column1     INT         NOT NULL,  
        Column2     NVARCHAR(4000)  NULL,  
  
        SpidFilter  SMALLINT    NOT NULL   DEFAULT (@@spid),  
  
        INDEX ix_SpidFiler NONCLUSTERED (SpidFilter),  
        --INDEX ix_SpidFilter HASH  
        --    (SpidFilter) WITH (BUCKET_COUNT = 64),  
          
        CONSTRAINT CHK_soSessionC_SpidFilter  
            CHECK ( SpidFilter = @@spid ),  
    )  
        par  
            (MEMORY_OPTIMIZED = ON,  
             DURABILITY = SCHEMA_ONLY);  
    GO  
  
  
    CREATE SECURITY POLICY dbo.soSessionC_SpidFilter_Policy  
        ADD FILTER PREDICATE dbo.fn_SpidFilter(SpidFilter)  
        ON dbo.soSessionC  
        WITH (STATE = ON);  
    GO  
  
  
  
  
Troisièmement, dans votre code T-SQL général :  
  
1. Modifiez toutes les références à la table temporaire dans vos instructions Transact-SQL en spécifiant la nouvelle table optimisée en mémoire :
    - _Ancien :_ &#x23;tempSessionC  
    - _Nouveau :_ dbo.soSessionC  
2. Remplacez les instructions `CREATE TABLE #tempSessionC` dans votre code par `DELETE FROM dbo.soSessionC`, pour garantir qu’une session n’est pas exposée au contenu de table inséré par une session précédente avec la même valeur session_id. Il est important de créer la table à mémoire optimisée au moment du déploiement, et non de l’exécution, pour éviter la surcharge de compilation qui accompagne la création de la table.
3. Supprimez les instructions `DROP TABLE #tempSessionC` de votre code. Vous pouvez éventuellement insérer une instruction `DELETE FROM dbo.soSessionC` au cas où la taille de la mémoire serait un problème potentiel.
  
  
  
  
## <a name="d-scenario-table-variable-can-be-memoryoptimizedon"></a>D. Scénario : Une variable de table peut afficher MEMORY_OPTIMIZED=ON  
  
  
Une variable de table classique représente une table dans la base de données tempdb. Pour des performances beaucoup plus rapides, vous pouvez optimiser votre variable de table en mémoire.  
  
Voici le code T-SQL pour une variable de table classique. Son étendue s’arrête quand le lot ou la session se termine.  
  
  
  
  
    DECLARE @tvTableD TABLE  
        ( Column1   INT   NOT NULL ,  
          Column2   CHAR(10) );  
  
  
  
  
#### <a name="d1-convert-inline-to-explicit"></a>D.1 Convertir inline en explicite  
  
La syntaxe précédente permet de créer la variable de table *inline*. La syntaxe inline ne prend pas en charge l’optimisation en mémoire. C’est pourquoi nous allons convertir la syntaxe inline en syntaxe explicite pour TYPE.  
  
*Étendue :* la définition TYPE créée par le premier lot délimité par une commande go est conservée même après l’arrêt et le redémarrage du serveur. Toutefois, après le premier délimiteur go, la table déclarée @tvTableC est conservée uniquement jusqu’à ce que le délimiteur go suivant soit atteint et que le lot se termine.  
  
  
  
  
    CREATE TYPE dbo.typeTableD  
        AS TABLE  
        (  
            Column1  INT   NOT NULL ,  
            Column2  CHAR(10)  
        );  
    go  
          
    SET NoCount ON;  
    DECLARE @tvTableD dbo.typeTableD  
    ;  
    INSERT INTO @tvTableD (Column1) values (1), (2)  
    ;  
    SELECT * from @tvTableD;  
    go  
  
  
  
  
#### <a name="d2-convert-explicit-on-disk-to-memory-optimized"></a>D.2 Convertir une table sur disque explicite en table optimisée en mémoire  
  
Une variable de table optimisée en mémoire ne se trouve pas dans tempdb. L’optimisation en mémoire entraîne une augmentation de la vitesse, qui est souvent 10 fois supérieure ou plus.  
  
La conversion en table optimisée en mémoire est effectuée en une seule étape. Améliorez la création TYPE explicite pour obtenir le résultat suivant, qui ajoute :  
  
- Un index. Là encore, chaque table optimisée en mémoire doit avoir au moins un index.  
- MEMORY_OPTIMIZED = ON.  
  
  
  
  
    CREATE TYPE dbo.typeTableD  
        AS TABLE  
        (  
            Column1  INT   NOT NULL   INDEX ix1,  
            Column2  CHAR(10)  
        )  
        par  
            (MEMORY_OPTIMIZED = ON);  
  
  
  
  
Opération terminée.  
  
  
## <a name="e-prerequisite-filegroup-for-sql-server"></a>E. Groupe de fichiers requis pour SQL Server  
  
Dans Microsoft SQL Server, pour utiliser les fonctionnalités optimisées en mémoire, votre base de données doit avoir un groupe de fichiers qui est déclaré avec **MEMORY_OPTIMIZED_DATA**.  
  
- La base de données SQL Azure ne nécessite pas la création de ce groupe de fichiers.  
  
  
*Condition préalable :* le code Transact-SQL suivant pour un groupe de fichiers est requis pour les exemples de code T-SQL longs dans les sections ultérieures de cet article.  
  
1. Vous devez utiliser SSMS.exe ou un autre outil qui peut envoyer du code T-SQL.  
2. Collez l’exemple de code T-SQL de groupe de fichiers dans SSMS.  
3. Modifiez le code T-SQL pour changer ses noms et chemins d’accès aux répertoires spécifiques à votre convenance.  
  - Tous les répertoires dans la valeur de nom de fichier doivent exister au préalable, à l’exception du répertoire final.  
4. Exécutez votre code T-SQL modifié.  
  - Il est inutile d’exécuter le code T-SQL de groupe de fichiers plusieurs fois, même si vous ajustez et réexécutez à plusieurs reprises le code T-SQL de comparaison de vitesse dans la sous-section suivante.  
  
  
  
 
  
    ALTER DATABASE InMemTest2  
        ADD FILEGROUP FgMemOptim3  
            CONTAINS MEMORY_OPTIMIZED_DATA;  
    GO  
    ALTER DATABASE InMemTest2  
        ADD FILE  
        (  
            NAME = N'FileMemOptim3a',  
            FILENAME = N'C:\DATA\FileMemOptim3a'  
                     --  C:\DATA\    preexisted.  
        )  
        TO FILEGROUP FgMemOptim3;  
    GO  
  
  
Le script suivant crée le groupe de fichiers pour vous et configure les paramètres de base de données recommandés : [enable-in-memory-oltp.sql](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/enable-in-memory-oltp.sql)
  
Pour plus d’informations sur `ALTER DATABASE ... ADD` pour les fichiers et groupes de fichiers, consultez :  
  
- [Options de fichiers et de groupes de fichiers ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
- [Groupe de fichiers optimisé en mémoire](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)    
  
  
## <a name="f-quick-test-to-prove-speed-improvement"></a>F. Test rapide pour prouver l’amélioration de la vitesse  
  
  
Cette section fournit le code Transact-SQL que vous pouvez exécuter pour tester et comparer le gain de vitesse pour INSERT-DELETE à partir de l’utilisation d’une variable de table optimisée en mémoire. Le code est composé de deux parties presque identiques sauf que, dans la première partie, le type de table est optimisé en mémoire.  
  
Le test de comparaison dure environ 7 secondes. Pour exécuter l’exemple :  
  
1. *Condition préalable :* vous devez déjà avoir exécuté le code T-SQL de groupe de fichiers de la section précédente.  
2. Exécutez le script T-SQL INSERT-DELETE suivant.  
  - Notez l’instruction « GO 5001 », qui renvoie le code T-SQL 5001 fois. Vous pouvez ajuster le nombre et l’exécuter à nouveau.  
  
Quand vous exécutez le script dans une base de données SQL Azure, veillez à effectuer l’opération à partir d’une machine virtuelle dans la même région.
  
  
    PRINT ' ';  
    PRINT '---- Next, memory-optimized, faster. ----';  
  
    DROP TYPE IF EXISTS dbo.typeTableC_mem;  
    go  
    CREATE TYPE dbo.typeTableC_mem  -- !!  Memory-optimized.  
         AS TABLE  
         (  
              Column1  INT NOT NULL INDEX ix1,  
              Column2  CHAR(10)  
         )  
         WITH  
              (MEMORY_OPTIMIZED = ON);  
    go  
    DECLARE @dateString_Begin nvarchar(64) =  
        Convert(nvarchar(64), GetUtcDate(), 121);  
    PRINT Concat(@dateString_Begin, '  = Begin time, _mem.');  
    go  
    SET NoCount ON;  
    DECLARE @tvTableC dbo.typeTableC_mem;  -- !!  
  
    INSERT INTO @tvTableC (Column1) values (1), (2);  
    INSERT INTO @tvTableC (Column1) values (3), (4);  
    DELETE @tvTableC;  
  
    GO 5001  
  
    DECLARE @dateString_End nvarchar(64) =  
        Convert(nvarchar(64), GetUtcDate(), 121);  
    PRINT Concat(@dateString_End, '  = End time, _mem.');  
    go  
    DROP TYPE IF EXISTS dbo.typeTableC_mem;  
    go  
  
    ---- End memory-optimized.  
    -------------------------------------------------  
    ---- Start traditional on-disk.  
  
    PRINT ' ';  
    PRINT '---- Next, tempdb based, slower. ----';  
  
    DROP TYPE IF EXISTS dbo.typeTableC_tempdb;  
    go  
    CREATE TYPE dbo.typeTableC_tempdb  -- !!  Traditional tempdb.  
        AS TABLE  
        (  
            Column1  INT NOT NULL ,  
            Column2  CHAR(10)  
        );  
    go  
    DECLARE @dateString_Begin nvarchar(64) =  
        Convert(nvarchar(64), GetUtcDate(), 121);  
    PRINT Concat(@dateString_Begin, '  = Begin time, _tempdb.');  
    go  
    SET NoCount ON;  
    DECLARE @tvTableC dbo.typeTableC_tempdb;  -- !!  
  
    INSERT INTO @tvTableC (Column1) values (1), (2);  
    INSERT INTO @tvTableC (Column1) values (3), (4);  
    DELETE @tvTableC;  
  
    GO 5001  
  
    DECLARE @dateString_End nvarchar(64) =  
        Convert(nvarchar(64), GetUtcDate(), 121);  
    PRINT Concat(@dateString_End, '  = End time, _tempdb.');  
    go  
    DROP TYPE IF EXISTS dbo.typeTableC_tempdb;  
    go  
    ----  
  
    PRINT '---- Tests done. ----';  
  
    go  
  
    /*** Actual output, SQL Server 2016:  
  
    ---- Next, memory-optimized, faster. ----  
    2016-04-20 00:26:58.033  = Begin time, _mem.  
    Beginning execution loop  
    Batch execution completed 5001 times.  
    2016-04-20 00:26:58.733  = End time, _mem.  
  
    ---- Next, tempdb based, slower. ----  
    2016-04-20 00:26:58.750  = Begin time, _tempdb.  
    Beginning execution loop  
    Batch execution completed 5001 times.  
    2016-04-20 00:27:05.440  = End time, _tempdb.  
    ---- Tests done. ----  
    ***/  
  

  
  
  
## <a name="g-predict-active-memory-consumption"></a>G. Prédire la consommation de mémoire active  
  
Vous pouvez apprendre à prévoir les besoins en mémoire active de vos tables optimisées en mémoire avec les ressources suivantes :  
  
- [Estimer les besoins en mémoire des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)  
- [Taille de la table et des lignes dans les tables optimisées en mémoire : exemple de calcul](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)  
  
Pour les variables de table plus importantes, les index non cluster utilisent plus de mémoire que pour les *tables*optimisées en mémoire. Plus le nombre de lignes et la clé d’index sont importants, plus la différence augmente.  
  
Si la variable de table optimisée en mémoire est accessible uniquement avec une valeur de clé exacte par accès, un index de hachage peut être un meilleur choix qu’un index non cluster. Toutefois, si vous ne pouvez pas estimer la valeur BUCKET_COUNT appropriée, un index non cluster représente une bonne alternative.  
  
## <a name="h-see-also"></a>H. Voir aussi  
  
- [Memory-Optimized Tables](../../relational-databases/in-memory-oltp/memory-optimized-tables.md)
- [Définition de la durabilité des objets mémoire optimisés](../../relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects.md)  
  
