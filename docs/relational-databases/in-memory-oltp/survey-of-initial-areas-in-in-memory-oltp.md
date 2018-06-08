---
title: 'Démarrage rapide 1 : technologies OLTP en mémoire pour accélérer les performances Transact-SQL | Microsoft Docs'
ms.custom: ''
ms.date: 09/05/2017"
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1c25a164-547d-43c4-8484-6b5ee3cbaf3a
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d29f3e4d1571d7c825bbf2eef01487a2e8da788b
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="survey-of-initial-areas-in-in-memory-oltp"></a>Inspection des zones initiales dans OLTP en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  
Cet article est destiné aux développeurs qui souhaitent se familiariser en quelques minutes avec les principes de base des fonctionnalités de performances OLTP en mémoire de Microsoft SQL Server et d’Azure SQL Database.  
  
Pour OLTP en mémoire, cet article fournit les éléments suivants :  
  
- Explications rapides des fonctionnalités  
- Exemples de code de base qui implémentent les fonctionnalités  
  
  
SQL Server et SQL Database ne varient que légèrement dans leur prise en charge des technologies en mémoire.  
  
  
Les blogueurs font parfois référence à l’OLTP en mémoire sous le terme *Hekaton*.  
  
  
<a name="benefits-of-in-memory-features-21a"></a>  
  
## <a name="benefits-of-in-memory-features"></a>Avantages des fonctionnalités en mémoire  
  
SQL Server fournit des fonctionnalités en mémoire qui peuvent améliorer considérablement les performances de nombreux systèmes d’applications. Les considérations les plus simples sont décrites dans cette section.  
  
  
### <a name="features-for-oltp-online-transactional-processing"></a>Fonctionnalités d’OLTP (traitement transactionnel en ligne)  
  
  
Les systèmes qui doivent traiter de nombreuses instructions SQL INSERT simultanément sont d’excellents candidats pour les fonctionnalités OLTP.  
  
- Nos tests d’évaluation montrent que des vitesses 5 à 20 fois supérieures peuvent être obtenues en adoptant les fonctionnalités en mémoire.  
  
  
Les systèmes qui traitent des calculs lourds dans Transact-SQL constituent d’excellents candidats.  
  
- Une procédure stockée dédiée aux calculs lourds peut s’exécuter jusqu’à 99 fois plus rapidement.  
  
  
Vous pourrez par la suite consulter les articles suivants qui offrent des démonstrations des gains de performances offerts par l’OLTP en mémoire :  
  
- La[Démonstration : optimisation des performances de l’OLTP en mémoire](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md) propose une démonstration à petite échelle des gains de performances potentiels.  
- La page [Sample Database for In-Memory OLTP](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md) (Exemple de base de données pour l’OLTP en mémoire) présente une démonstration à plus grande échelle.  
  
  
  
### <a name="features-for-operational-analytics"></a>Fonctionnalités d’analytique opérationnelle  
  
L’analytique en mémoire fait référence aux instructions SQL INSERT qui agrègent des données transactionnelles, généralement par l’inclusion d’une clause GROUP BY. Le type d’index appelé *columnstore* est central à l’analytique opérationnelle.  
  
Il existe deux scénarios principaux :  
  
- L’*analytique opérationnelle par lot* fait référence aux processus d’agrégation qui s’exécutent soit après les heures de bureau, soit sur du matériel secondaire qui comporte des copies des données transactionnelles.  
  - [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-overview-what-is/) est également lié à l’analytique opérationnelle par lot.  
- L’*analytique opérationnelle en temps réel* fait référence aux processus d’agrégation qui s’exécutent pendant les heures de bureau et sur le matériel principal utilisé pour les charges de travail transactionnelles.  
  
  
Cet article se concentre sur OLTP et non sur l’analyse. Pour plus d’informations sur la façon dont les index columnstore permettent à SQL de bénéficier de l’analytique, consultez :  
  
- [Prise en main de columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
- [Guide des index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)  
  
  
> [!NOTE]
> Une vidéo de deux minutes sur les fonctionnalités en mémoire est disponible à la page [Azure SQL Database - In-Memory Technologies](http://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-In-Memory-Technologies)(Azure SQL Database - Technologies en mémoire). La vidéo date de décembre 2015.  


### <a name="columnstore"></a>columnstore

Une série d’excellents billets de blog expliquent de manière élégante les index columnstore selon plusieurs perspectives. La majorité des billets décrivent en détail le concept d’analytique opérationnelle en temps réel, que columnstore prend en charge.  Ces billets ont été créés par Sunil Agarwal, responsable de programme chez Microsoft, en mars 2016.

#### <a name="real-time-operational-analytics"></a>analytique opérationnelle en temps réel

1. [Analytique opérationnelle en temps réel à l’aide de la technologie en mémoire](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)
2. [Analytique opérationnelle en temps réel - Vue d’ensemble d’un index columnstore non cluster](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/02/29/real-time-operational-analytics-using-nonclustered-columnstore-index/)
3. [Analytique opérationnelle en temps réel : Exemple simple utilisant un index columnstore non cluster dans SQL Server 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/02/29/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci/)
4. [Analytique opérationnelle en temps réel : Opérations DML et index columnstore non cluster dans SQL Server 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/04/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016/)
5. [Analytique opérationnelle en temps réel : Index columnstore non cluster filtré](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/06/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci/)
6. [Analytique opérationnelle en temps réel : Option de délai de compression pour l’index columnstore non cluster](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/06/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci/)
7. [Analytique opérationnelle en temps réel : Option de délai de compression avec index columnstore non cluster et performances](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/06/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance/)
8. [Analytique opérationnelle en temps réel : Tables optimisées en mémoire et index columnstore](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/07/real-time-operational-analytics-memory-optimized-table-and-columnstore-index/)

#### <a name="defragment-a-columnstore-index"></a>Défragmenter un index columnstore

1. [Défragmentation de l’index columnstore à l’aide de la commande REORGANIZE](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/07/columnstore-index-defragmentation-using-reorganize-command/)
2. [Stratégie de fusion de l’index columnstore pour REORGANIZE](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/08/columnstore-index-merge-policy-for-reorganize/)

#### <a name="bulk-importation-of-data"></a>Importation en bloc des données

1. [Cluster columnstore : Chargement en bloc](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2014/07/27/clustered-column-store-index-bulk-loading-the-data/)
2. [Index cluster columnstore : Optimisations du chargement des données - Journalisation minimale](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/01/10/clustered-columnstore-index-data-load-optimizations-minimal-logging/)
3. [Index cluster columnstore : Optimisations du chargement des données - Importation en bloc parallèle](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/02/28/clustered-columnstore-index-parallel-bulk-import/)





<a name="features-on-in-memory-oltp-13b"></a>  
  
## <a name="features-of-in-memory-oltp"></a>Fonctionnalités de l’OLTP en mémoire  
  
Examinons les fonctionnalités principales de l’OLTP en mémoire.  
  
  
#### <a name="memory-optimized-tables"></a>Tables optimisées en mémoire  
  
Le mot clé T-SQL MEMORY_OPTIMIZED, dans l’instruction CREATE TABLE, permet à la table créée d’exister dans la mémoire active, et non sur le disque.  
  
  
Une [table optimisée en mémoire](../../relational-databases/in-memory-oltp/memory-optimized-tables.md) a une représentation d’elle-même dans la mémoire active et une copie secondaire sur le disque.  
  
- La copie sur disque sert pour les opérations de récupération de routine après un redémarrage, puis arrêt, du serveur ou de la base de données. Cette dualité « disque plus mémoire » est totalement masquée pour l’utilisateur et pour votre code.  
  
  
#### <a name="natively-compiled-modules"></a>Modules compilés en mode natif  
  
Le mot clé T-SQL NATIVE_COMPILATION, dans l’instruction CREATE PROCEDURE, permet de créer une procédure stockée compilée en mode natif. Les instructions T-SQL sont compilées en code machine lors de la première utilisation de la procédure native chaque fois que la base de données bascule en ligne. Les instructions T-SQL ne subissent plus l’interprétation lente de chaque instruction.  
  
- La compilation native peut être cent fois plus rapide que le mode interprété.  
  
  
Un module natif ne peut référencer que des tables optimisées en mémoire. Il ne peut pas référencer de tables sur disque.  
  
Il existe trois types de modules compilés en mode natif :  
  
- [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)  
- Fonctions définies par l’utilisateur (UDF) compilées en mode natif, qui sont scalaires  
- Déclencheurs compilés en mode natif  
  
  
#### <a name="availability-in-azure-sql-database"></a>Disponibilité dans Azure SQL Database  
  
OLTP en mémoire et columnstore sont disponibles dans Azure SQL Database. Pour plus d’informations, consultez [Optimiser les performances à l’aide des technologies en mémoire dans SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory).
  
  
<a name="ensure-compatibility-level-gteq-130-99c"></a>  
  
## <a name="1-ensure-compatibility-level--130"></a>1. Assurer un niveau de compatibilité >= 130  
  
  
Cette section est la première d’une série de sections numérotées qui illustrent la syntaxe Transact-SQL que vous pouvez utiliser pour implémenter des fonctionnalités OLTP en mémoire.  
  
  
Tout d’abord, il est important que votre base de données soit définie sur un niveau de compatibilité d’au moins 130. Le code T-SQL suivant indique le niveau de compatibilité actuel sur lequel votre base de données actuelle est définie.  
  
  
  
  
  
    SELECT d.compatibility_level  
        FROM sys.databases as d  
        WHERE d.name = Db_Name();  
  
  
  
  
Le code T-SQL suivant met à jour le niveau, si nécessaire.  
  
  
  
  
    ALTER DATABASE CURRENT  
        SET COMPATIBILITY_LEVEL = 130;  
  
  
  
  
<a name="elevate-to-snapshot-26n"></a>  
  
## <a name="2-elevate-to-snapshot"></a>2. Élever au niveau capture instantanée (SNAPSHOT)  
  
  
Une transaction impliquant à la fois une table basée sur disque et une table optimisée en mémoire est une *transaction entre conteneurs*. Dans ce type de transaction, il est essentiel que la partie optimisation en mémoire de la transaction fonctionne au niveau d’isolation de la transaction nommé SNAPSHOT.  
  
Pour appliquer de manière fiable ce niveau aux tables optimisées en mémoire dans une transaction entre conteneurs, [modifiez le paramétrage de votre base de données](../../t-sql/statements/alter-database-transact-sql-set-options.md) en exécutant le code T-SQL suivant.  
  
  
  
  
    ALTER DATABASE CURRENT  
        SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;  
  
  
  
  
<a name="create-an-optimized-filegroup-24r"></a>  
  
## <a name="3-create-an-optimized-filegroup"></a>3. Créer un groupe de fichiers (FILEGROUP) optimisé  
  
  
Dans Microsoft SQL Server, avant de créer une table optimisée en mémoire, vous devez créer un groupe de fichiers en lui associant la déclaration CONTAINS MEMORY_OPTIMIZED_DATA. Le groupe de fichiers est attribué à votre base de données. Pour plus d’informations, consultez :  
  
- [Groupe de fichiers mémoire optimisé](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)  
  
  
Dans Azure SQL Database, vous ne devez et ne pouvez pas créer un groupe de fichiers de ce type.  

L’exemple de script T-SQL suivant active une base de données pour OLTP en mémoire et configure tous les paramètres recommandés. Il fonctionne avec SQL Server et Azure SQL Database : [enable-in-memory-oltp.sql](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/enable-in-memory-oltp.sql).

Notez que toutes les fonctionnalités SQL Server ne sont pas prises en charge pour les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA. Pour plus d’informations sur les limitations, consultez [Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire](unsupported-sql-server-features-for-in-memory-oltp.md).
  
<a name="create-a-memory-optimized-table-26y"></a>  
  
## <a name="4-create-a-memory-optimized-table"></a>4. Créer une table optimisée en mémoire  
  
Le mot-clé Transact-SQL essentiel est le mot-clé MEMORY_OPTIMIZED.  
  
  
  
  
    CREATE TABLE dbo.SalesOrder  
    (  
        SalesOrderId   integer        not null  IDENTITY  
            PRIMARY KEY NONCLUSTERED,  
        CustomerId     integer        not null,  
        OrderDate      datetime       not null  
    )  
        WITH  
            (MEMORY_OPTIMIZED = ON,  
            DURABILITY = SCHEMA_AND_DATA);  
  
  
  
  
Les instructions Transact-SQL INSERT et SELECT sur une table optimisée en mémoire sont les mêmes que pour une table normale.  
  
#### <a name="alter-table-for-memory-optimized-tables"></a>ALTER TABLE pour les tables optimisées en mémoire  
  
ALTER TABLE...ADD/DROP peut ajouter ou supprimer une colonne dans une table optimisée en mémoire ou un index.  
  
- CREATE INDEX et DROP INDEX ne peuvent pas être exécutés sur une table optimisée en mémoire, utilisez ALTER TABLE... ADD/DROP INDEX à la place.  
- Pour plus d’informations, consultez [Modification des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md).  
  
  
#### <a name="plan-your-memory-optimized-tables-and-indexes"></a>Planifier vos tables et index mémoire optimisés  
  
  
- [Index pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)  
- [Les constructions Transact-SQL ne sont pas prises en charge par l’OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
  
  
<a name="create-a-natively-compiled-stored-procedure-native-proc-29u"></a>  
  
## <a name="5-create-a-natively-compiled-stored-procedure-native-proc"></a>5. Créer une procédure stockée compilée en mode natif (procédure native)  
  
  
Le mot clé essentiel est NATIVE_COMPILATION.  
  
  
  
  
    CREATE PROCEDURE ncspRetrieveLatestSalesOrderIdForCustomerId  
        @_CustomerId   INT  
        WITH  
            NATIVE_COMPILATION,  
            SCHEMABINDING  
    AS  
    BEGIN ATOMIC  
        WITH  
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
            LANGUAGE = N'us_english')  
      
        DECLARE @SalesOrderId int, @OrderDate datetime;  
      
        SELECT TOP 1  
                @SalesOrderId = s.SalesOrderId,  
                @OrderDate    = s.OrderDate  
            FROM dbo.SalesOrder AS s  
            WHERE s.CustomerId = @_CustomerId  
            ORDER BY s.OrderDate DESC;  
      
        RETURN @SalesOrderId;  
    END;  
  
  
  
  
Le mot clé SCHEMABINDING signifie que les tables référencées dans la procédure native ne peuvent pas être supprimées, sauf si celle-ci est d’abord supprimée. Pour plus d’informations, consultez [Création de procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md).  
  
Notez que vous n’avez pas besoin de créer une procédure stockée compilée en mode natif pour accéder à une table à mémoire optimisée. Vous pouvez également référencer les tables à mémoire optimisée à partir de procédures stockées traditionnelles et de lots ad hoc.
  
<a name="execute-the-native-proc-31e"></a>  
  
## <a name="6-execute-the-native-proc"></a>6. Exécuter la procédure native  
  
  
Remplissez la table avec deux lignes de données.  
  
  
  
  
    INSERT into dbo.SalesOrder  
            ( CustomerId, OrderDate )  
        VALUES  
            ( 42, '2013-01-13 03:35:59' ),  
            ( 42, '2015-01-15 15:35:59' );  
  
  
  
  
Vient ensuite un appel EXECUTE à destination de la procédure stockée en mode natif.  
  
  
  
  
    DECLARE @LatestSalesOrderId int, @mesg nvarchar(128);  
      
    EXECUTE @LatestSalesOrderId =  
        ncspRetrieveLatestSalesOrderIdForCustomerId 42;  
      
    SET @mesg = CONCAT(@LatestSalesOrderId,  
        ' = Latest SalesOrderId, for CustomerId = ', 42);  
    PRINT @mesg;  
      
    -- Here is the actual PRINT output:  
    -- 2 = Latest SalesOrderId, for CustomerId = 42  
  
  
  
  
<a name="guide-to-the-documentation-and-next-steps-32w"></a>  
  
### <a name="guide-to-the-documentation-and-next-steps"></a>Guide de la documentation et étapes suivantes  
  
  
Les exemples simples précédents vous donnent les bases pour l’apprentissage des fonctionnalités plus avancées de l’OLTP en mémoire. Les sections suivantes présentent les considérations particulières que vous pouvez être amené à connaître et indiquent les sources d’informations qui détaillent chacune d’elles.  
  
  
  
<a name="how-do-in-memory-oltp-features-work-so-much-faster-33v"></a>  
  
## <a name="how-in-memory-oltp-features-work-so-much-faster"></a>Pourquoi les fonctionnalités de l’OLTP en mémoire fonctionnent-elles plus rapidement ?  
  
  
Les sous-sections suivantes décrivent brièvement comment fonctionne l’OLTP en mémoire en interne pour fournir de meilleures performances.  
  
  
<a name="how-do-memory-optimized-tables-perform-faster-34q"></a>  
  
### <a name="how-memory-optimized-tables-perform-faster"></a>Dans quelle mesure les performances des tables optimisées en mémoire sont-elles plus rapides ?  
  
  
**Double nature :** une table optimisée en mémoire présente une double nature : une représentation en mémoire active et une autre sur le disque dur. Chaque transaction est validée dans les deux représentations de la table. Les transactions s’exécutent par rapport à la représentation en mémoire active, qui est beaucoup plus rapide. Les tables optimisées en mémoire tirent parti de la vitesse supérieure qu’offre la mémoire active par rapport au disque. En outre, grâce à la souplesse supérieure de la mémoire active, vous pouvez facilement mettre en place une structure de table plus avancée qui est optimisée pour la vitesse. De plus, comme la structure avancée ne fait pas appel à la pagination, elle évite la surcharge et la contention liées aux verrous et aux verrouillages tournants.  
  
  
**Aucun verrou :** la table optimisée en mémoire s’appuie sur une approche *optimiste* des objectifs concurrents que sont, d’une part, l’intégrité des données et, d’autre part, la concurrence et le débit élevé. Pendant la transaction, la table ne place de verrous sur aucune version des lignes de données mises à jour. Cela peut réduire considérablement la contention dans certains systèmes à volumes élevés.  
  
  
**Versions de ligne :** au lieu de verrous, la table optimisée en mémoire ajoute une nouvelle version d’une ligne mise à jour à la table elle-même, et non dans tempdb. La ligne d’origine est conservée jusqu’à ce que la transaction soit validée. Pendant la transaction, les autres processus peuvent lire la version d’origine de la ligne.  
  
- Si plusieurs versions d’une ligne sont créées pour une table basée sur disque, les versions de ligne sont stockées temporairement dans tempdb.  
  
  
**Moins de journalisation :** les versions avant et après des lignes mises à jour sont conservées dans la table optimisée en mémoire. La paire de lignes fournit la plupart des informations qui sont traditionnellement écrites dans le fichier journal. Ainsi, le système écrit moins d’informations, et moins souvent, dans le journal. L’intégrité transactionnelle est néanmoins assurée.  
  
  
<a name="how-do-native-procs-perform-faster-35x"></a>  
  
### <a name="how-native-procs-perform-faster"></a>Dans quelle mesure les performances des procédures natives sont-elles plus rapides ?  
  
Convertir une procédure stockée interprétée normale en une procédure stockée compilée en mode natif réduit considérablement le nombre d’instructions à exécuter pendant l’exécution.  
  
  
<a name="trade-offs-of-in-memory-features-36j"></a>  
  
## <a name="trade-offs-of-in-memory-features"></a>Compromis des fonctionnalités en mémoire  
  
  
Comme cela est courant en informatique, les gains de performance procurés par les fonctionnalités en mémoire sont un compromis. Les meilleures fonctionnalités présentent des avantages qui compensent sensiblement les coûts supplémentaires qu’elles engendrent. Vous trouverez des instructions complètes sur les compromis dans l’article suivant :

- [Planifier votre adoption des fonctionnalités OLTP en mémoire dans SQL Server](../../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)

Le reste de cette section répertorie les principaux éléments à prendre en considération pour la planification et les compromis.
  
<a name="trade-offs-of-memory-optimized-tables-37d"></a>  
  
### <a name="trade-offs-of-memory-optimized-tables"></a>Compromis des tables optimisées en mémoire  
  
  
**Estimer la mémoire :** vous devez estimer la quantité de mémoire active que votre table optimisée en mémoire est appelée à consommer. Votre système informatique doit avoir une capacité de mémoire suffisante pour héberger une table optimisée en mémoire. Pour plus d’informations, consultez :  
  
- [Surveiller l’utilisation de la mémoire et résoudre les problèmes connexes](../../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)  
- [Estimer les besoins en mémoire des tables mémoire optimisées](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)  
- [Taille de la table et des lignes dans les tables mémoire optimisées](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)  
  
  
**Partitionner votre table volumineuse :** une façon de répondre à la demande d’une quantité de mémoire active élevée consiste à partitionner votre table volumineuse en parties en mémoire qui stockent les lignes de données *récentes à chaud* , tandis que les autres parties sur le disque comportent les lignes *héritées à froid* (telles que les commandes qui ont été entièrement livrées et terminées). Ce partitionnement est un processus manuel de conception et d’implémentation. Consultez :  
  
- [Partitionnement au niveau de l’application](../../relational-databases/in-memory-oltp/application-level-partitioning.md)  
- [Modèle d’application pour partitionner des tables mémoire optimisées](../../relational-databases/in-memory-oltp/application-pattern-for-partitioning-memory-optimized-tables.md)  
  
<a name="trade-offs-of-native-procs-38p"></a>  
  
### <a name="trade-offs-of-native-procs"></a>Compromis des procédures natives  
  
- Une procédure stockée compilée en mode natif ne peut pas accéder à une table sur disque. Une procédure native ne peut accéder qu’à des tables optimisées en mémoire.  
- Quand une procédure native s’exécute pour la première fois après la toute dernière remise en ligne du serveur ou de la base de données, elle doit être recompilée une fois. Cette opération retarde l’exécution de la procédure native.  
  
<a name="advanced-considerations-for-memory-optimized-tables-39n"></a>  
  
## <a name="advanced-considerations-for-memory-optimized-tables"></a>Observations importantes sur les tables optimisées en mémoire  
  
Les[index pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md) sont, à certains égards, différents des index sur les tables sur disque traditionnelles. Les index de hachage sont disponibles uniquement pour les tables à mémoire optimisée.
    
- [Index de hachage pour les tables mémoire optimisées](../../relational-databases/sql-server-index-design-guide.md#hash_index)
- [Index non-cluster pour les tables à mémoire optimisée](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index) 
  
Vous devez vous assurer qu’il y aura suffisamment de mémoire active pour votre table optimisée en mémoire planifié et ses index. Consultez :  
  
- [Création et gestion du stockage des objets mémoire optimisés](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
Vous pouvez déclarer une table optimisée en mémoire avec DURABILITY = SCHEMA_ONLY :  
  
- Cette syntaxe indique au système d’ignorer toutes les données de la table optimisée en mémoire quand la base de données est déconnectée. Seule la définition de table est conservée.  
- Quand la base de données est remise en ligne, la table optimisée en mémoire est rechargée, vide, en mémoire active.  
- Les tables SCHEMA_ONLY peuvent constituer une bonne [alternative aux tables #temporary](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md) dans tempdb, quand plusieurs milliers de lignes sont impliqués.  
  
Vous pouvez aussi déclarer des variables de table comme mémoire optimisée. Consultez :  
  
- [Table temporaire et variable de table plus rapides à l’aide de l’optimisation en mémoire](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)  
  
<a name="advanced-considerations-for-natively-compiled-modules-40k"></a>  
  
## <a name="advanced-considerations-for-natively-compiled-modules"></a>Observations importantes sur les modules compilés en mode natif  
  
Les types de modules compilés en mode natif disponibles par le biais de Transact-SQL sont les suivants :  
  
- Procédures stockées compilées en mode natif (procédures natives)  
- [Fonctions scalaires définies par l’utilisateur](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)compilées en mode natif  
- Déclencheurs compilés en mode natif (déclencheurs natifs)  
  - Seuls les déclencheurs compilés en mode natif sont autorisés sur les tables optimisées en mémoire.  
- [Fonctions table](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)compilées en mode natif  
  - [Improving temp table and table variable performance using memory optimization (Amélioration des performances des tables temporaires et des variables de table à l’aide de l’optimisation de la mémoire)](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/21/improving-temp-table-and-table-variable-performance-using-memory-optimization/)  
  
Une fonction définie par l’utilisateur compilée en mode natif s’exécute plus rapidement qu’une fonction définie par l’utilisateur interprétée. Voici quelques éléments à prendre en considération avec les fonctions définies par l’utilisateur :  
  
- Quand une instruction T-SQL SELECT utilise une fonction définie par l’utilisateur, celle-ci est toujours appelée une fois par ligne retournée.  
  - Les fonctions définies par l’utilisateur ne s’exécutent jamais en ligne et, à la place, sont toujours appelées.  
  - La distinction « interprété/compilé » est moins importante que la surcharge d’appels répétés inhérente à toutes les fonctions définies par l’utilisateur.  
  - La surcharge d’appels des fonctions définies par l’utilisateur reste souvent acceptable dans la pratique.  
  
Pour obtenir des données de test et des explications sur les performances des fonctions définies par l’utilisateur natives, consultez :  
  
  - [Soften the RBAR impact with Native Compiled UDFs in SQL Server 2016 (Atténuer l’impact du traitement RBAR avec les fonctions définies par l’utilisateur compilées en mode natif dans SQL Server 2016)](https://blogs.msdn.microsoft.com/sqlcat/2016/02/17/soften-the-rbar-impact-with-native-compiled-udfs-in-sql-server-2016/)  
  - Billet de blog [Natively Compiled User Defined Functions](http://sqlinthewild.co.za/index.php/2016/01/12/natively-compiled-user-defined-functions/) de Gail Shaw (janvier 2016).  
  
<a name="documentation-guide-for-memory-optimized-tables-41z"></a>  
  
## <a name="documentation-guide-for-memory-optimized-tables"></a>Guide de la documentation pour les tables optimisées en mémoire  
  
Consultez les autres articles suivants qui traitent de considérations spéciales sur les tables à mémoire optimisée :  
  
- [Migration vers OLTP en mémoire](../../relational-databases/in-memory-oltp/migrating-to-in-memory-oltp.md)  
  - [Déterminer si un tableau ou une procédure stockée doit être déplacée vers l’OLTP en mémoire](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)  
  - Le rapport d’analyse des performances de transaction dans SQL Server Management Studio vous aide à évaluer si l’OLTP en mémoire améliore les performances de votre application de base de données.  
  - Utilisez le [Conseiller d’optimisation de la mémoire](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) pour vous aider à migrer la table de base de données sur disque vers l’OLTP en mémoire.   
- [Sauvegarder, restaurer et récupérer des tables optimisées en mémoire](http://msdn.microsoft.com/library/3f083347-0fbb-4b19-a6fb-1818d545e281)  
  - Le stockage utilisé par les tables optimisées en mémoire peut être bien supérieur à sa taille en mémoire, et il affecte la taille de la sauvegarde de base de données.  
- [Transactions avec tables optimisées en mémoire](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)  
  - Fournit des informations sur la logique de nouvelle tentative dans T-SQL, pour les transactions sur les tables optimisées en mémoire.  
- [Prise en charge d’OLTP en mémoire par Transact-SQL](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)  
  - Instructions T-SQL et types de données pris en charge et non pris en charge, pour les procédures natives et les tables optimisées en mémoire  
- [Lier une base de données avec des tables mémoire optimisées à un pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md), qui présente une considération avancée optionnelle.  

<a name="documentation-guide-for-native-procs-42b"></a>  
  
## <a name="documentation-guide-for-native-procs"></a>Guide de la documentation pour les procédures natives  

L’article suivant, et ses articles enfants dans la table des matières, décrivent en détail les procédures stockées compilées en mode natif.

- [Procédures stockées compilées en mode natif](natively-compiled-stored-procedures.md)
  
<a name="related-links-43f"></a>  
  
## <a name="related-links"></a>Liens connexes  
  
- Article initial : [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
    
Voici des articles qui contiennent du code pour illustrer les gains de performance que vous pouvez obtenir à l’aide de l’OLTP en mémoire :  
  
- La[Démonstration : optimisation des performances de l’OLTP en mémoire](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md) propose une démonstration à petite échelle des gains de performances potentiels.  
- La page [Sample Database for In-Memory OLTP](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md) (Exemple de base de données pour l’OLTP en mémoire) présente une démonstration à plus grande échelle.  
