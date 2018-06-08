---
title: Planifier votre adoption des fonctionnalités OLTP en mémoire dans SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 041b428f-781d-4628-9f34-4d697894e61e
caps.latest.revision: 4
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 97480ae8c4b390250ae2007b7d1d03d0efccf472
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="plan-your-adoption-of-in-memory-oltp-features-in-sql-server"></a>Planifier votre adoption des fonctionnalités OLTP en mémoire dans SQL Server
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]


Cet article décrit la manière dont l’adoption des fonctionnalités de mémoire affecte les autres aspects de votre système métier.



## <a name="a-adoption-of-in-memory-oltp-features"></a>A. Adoption des fonctionnalités OLTP en mémoire


Les sous-sections suivantes décrivent les facteurs que vous devez prendre en compte lorsque vous envisagez d’adopter et d’implémenter des fonctionnalités en mémoire. De nombreuses explications sont disponibles dans l’article suivant :

- [Utiliser OLTP en mémoire pour améliorer les performances de votre application dans Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-in-memory-oltp-migration/)



### <a name="a1-prerequisites"></a>A.1 Prérequis

L’un des prérequis pour utiliser les fonctionnalités en mémoire peut concerner l’édition ou le niveau de service du produit SQL. Pour plus d’informations sur les prérequis, consultez :

- [Conditions requises pour l'utilisation des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)
    - [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)
    - [Recommandations concernant le niveau de tarification de SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-service-tier-advisor/)


### <a name="a2-forecast-the-amount-of-active-memory"></a>A.2 Prévoir la quantité de mémoire active

Votre système dispose-t-il d’assez de mémoire active pour prendre en charge une nouvelle table optimisée en mémoire ?

#### <a name="microsoft-sql-server"></a>Microsoft SQL Server

Une table optimisée en mémoire qui contient 200 Go de données nécessite plus de 200 Go de mémoire active dédiée pour sa prise en charge. Avant d’implémenter une table optimisée en mémoire contenant une grande quantité de données, vous devez prévoir la quantité de mémoire active supplémentaire que vous devrez peut-être ajouter à votre serveur. Pour obtenir des conseils sur l’estimation, consultez :

- [Estimer les besoins en mémoire des tables mémoire optimisées](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)

#### <a name="azure-sql-database"></a>Azure SQL Database

Dans le cas d’une base de données hébergée dans le service cloud Azure SQL Database, le niveau de service que vous choisissez a un impact sur la quantité de mémoire active que votre base de données est autorisée à consommer. Vous devez prévoir de surveiller l’utilisation de la mémoire de votre base de données à l’aide d’une alerte. Pour plus d’informations, consultez :

- Passer en revue les limites de stockage OLTP en mémoire pour votre [niveau tarifaire](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-service-tiers#single-database-service-tiers-and-performance-levels)
- [Surveiller le stockage OLTP en mémoire](https://azure.microsoft.com/documentation/articles/sql-database-in-memory-oltp-monitoring/)

#### <a name="memory-optimized-table-variables"></a>Variables de table optimisées en mémoire

Une variable de table déclarée comme étant à mémoire optimisée est parfois préférable à une #TempTable traditionnelle située dans la base de données **tempdb** . Ces variables de table peuvent permettre une amélioration significative des performances, sans utiliser d’importantes quantités de mémoire active.

### <a name="a3-table-must-be-offline-to-convert-to-memory-optimized"></a>A.3 La table doit être mise hors connexion pour être convertie en table optimisée en mémoire

Certaines fonctionnalités ALTER TABLE sont disponibles pour les tables optimisées en mémoire. Toutefois, vous ne pouvez pas émettre une instruction ALTER TABLE pour convertir une table basée sur disque en une table optimisée en mémoire. Au lieu de cela, vous devez effectuer manuellement une série d’étapes. Voici différentes méthodes permettant de convertir une table basée sur disque en table optimisée en mémoire.

#### <a name="manual-scripting"></a>Scripts manuels

L’une des méthodes de conversion d’une table basée sur disque en table optimisée en mémoire consiste à coder vous-même les étapes nécessaires de Transact-SQL.


1. Suspendez l’activité d’application.

2. Effectuez une sauvegarde complète.

3. Renommez la table basée sur disque.

4. Émettez une instruction CREATE TABLE pour créer votre table optimisée en mémoire.

5. Utilisez INSERT INTO dans votre table optimisée en mémoire avec un sous-SELECT de la table basée sur disque.

6. Supprimez (DROP) votre table basée sur disque.

7. Effectuez une autre sauvegarde complète.

8. Reprenez l’activité d’application.


#### <a name="memory-optimization-advisor"></a>Conseiller d'optimisation de la mémoire

L’outil Conseiller d’optimisation de la mémoire peut générer un script permettant d’implémenter la conversion d’une table basée sur disque en une table optimisée en mémoire. Cet outil est installé avec SQL Server Data Tools (SSDT).

- [Conseiller d'optimisation de la mémoire](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md)
- [Télécharger SSDT (SQL Server Data Tools)](../../ssdt/download-sql-server-data-tools-ssdt.md)


#### <a name="dacpac-file"></a>Fichier .dacpac

Vous pouvez mettre à jour votre base de données sur place à l’aide d’un fichier .dacpac géré par SSDT. Dans SSDT, vous pouvez spécifier les modifications apportées au schéma qui est encodé dans le fichier .dacpac.

Vous utilisez des fichiers .dacpac dans le contexte d’un projet Visual Studio de type *Database*.

- [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md) et fichiers .dacpac



### <a name="a4-guidance-for-whether-in-memory-oltp-features-are-right-for-your-application"></a>A.4 Déterminer si les fonctionnalités OLTP en mémoire sont adaptées à votre application

Pour savoir si les fonctionnalités OLTP en mémoire peuvent améliorer les performances de votre application, consultez :

- [OLTP en mémoire (optimisation en mémoire)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)



## <a name="b-unsupported-features"></a>B. Fonctionnalités non prises en charge

Les fonctionnalités qui ne sont pas prises en charge dans certains scénarios OLTP en mémoire sont décrites dans :

- [Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)


Les sous-sections suivantes abordent certaines des fonctionnalités non prises en charge les plus importantes.


### <a name="b1-snapshot-of-a-database"></a>B.1 Capture instantanée d’une base de données

Lorsqu’une table optimisée en mémoire ou un module est créé pour la première fois dans une base de données, aucun [SNAPSHOT](../../relational-databases/databases/database-snapshots-sql-server.md) (capture instantanée) de la base de données ne pourra plus être créé. Voici pourquoi :

- Le premier élément optimisé en mémoire rend impossible la suppression du dernier fichier du FILEGROUP optimisé en mémoire.
- Aucune base de données qui dispose d’un fichier dans un FILEGROUP optimisé en mémoire ne peut prendre en charge un SNAPSHOT (capture instantanée).

En général, une capture instantanée est utile pour les itérations de test rapides.


### <a name="b2-cross-database-queries"></a>B.2 Requêtes de bases de données croisées

Les tables optimisées en mémoire ne prennent pas en charge les transactions [entre bases de données](../../relational-databases/in-memory-oltp/cross-database-queries.md) . Vous ne pouvez pas accéder à une autre base de données à partir de la même transaction ou de la même requête qui accède également à une table mémoire optimisée.

Les variables de table ne sont pas transactionnelles. Par conséquent, les [variables de tables optimisées en mémoire](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md) peuvent être utilisées dans les requêtes de bases de données croisées.


### <a name="b3-readpast-table-hint"></a>B.3 Indicateur de table READPAST

Aucune requête ne peut appliquer [l’indicateur de table](../../t-sql/queries/hints-transact-sql-table.md) READPAST à toutes les tables optimisées en mémoire.

L’indicateur READPAST est utile lorsque plusieurs sessions accèdent à un même ensemble de lignes et le modifient, comme dans une file d’attente de traitement.


### <a name="b4-rowversion-sequence"></a>B.4 RowVersion, Sequence

- Aucune colonne ne peut être marquée pour [RowVersion](../../t-sql/data-types/rowversion-transact-sql.md) dans une table optimisée en mémoire.


- [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md) ne peut pas être utilisé avec une contrainte dans une table à mémoire optimisée. Par exemple, vous ne pouvez pas créer une contrainte DEFAULT avec une clause NEXT VALUE FOR. Vous pouvez utiliser plusieurs SEQUENCE avec des instructions INSERT et UPDATE.


## <a name="c-administrative-maintenance"></a>C. Maintenance administrative


Cette section décrit les différences qui existent au niveau de l’administration de base de données lorsque des tables optimisées en mémoire sont utilisées.


### <a name="c1-identity-seed-reset-increment--1"></a>C.1 Réinitialisation de la valeur initiale de la propriété Identity, incrément > 1

Pour réattribuer une valeur à la colonne IDENTITY,[DBCC CHECKIDENT](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)ne peut pas être utilisé dans une table optimisée en mémoire.

La valeur d’incrément est limitée à exactement 1 pour une colonne IDENTITY dans une table optimisée en mémoire.


### <a name="c2-dbcc-checkdb-cannot-validate-memory-optimized-tables"></a>C.2 DBCC CHECKDB ne peut pas valider les tables optimisées en mémoire

La commande [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) n’a aucun effet lorsque la cible est une table optimisée en mémoire. Les étapes suivantes permettent de contourner ce problème :


1. [Sauvegardez le journal des transactions](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).

2. Sauvegardez les fichiers dans le FILEGROUP optimisé en mémoire pour un système Null. Le processus de sauvegarde appelle une validation de somme de contrôle.

    Si l’altération est identifiée, passez aux étapes suivantes.

3. Pour le stockage temporaire, copiez les données de vos tables optimisées en mémoire dans des tables basées sur disque.

4. Restaurez les fichiers du FILEGROUP optimisé en mémoire.

5. Dans les tables optimisées en mémoire, utilisez INSERT INTO pour les données que vous avez temporairement stockées dans les tables basées sur disque.

6. Supprimez (DROP) les tables basées sur disque qui ont contenu temporairement les données.



## <a name="d-performance"></a>D. Performances

Cette section décrit les situations dans lesquelles les excellentes performances des tables optimisées en mémoire peuvent être freinées.


### <a name="d1-index-considerations"></a>D.1 Observations relatives aux index

Tous les index d’une table optimisée en mémoire sont créés et gérés par les instructions de table CREATE TABLE et ALTER TABLE. Vous ne pouvez pas cibler une table optimisée en mémoire avec une instruction CREATE INDEX.

L’index non-cluster traditionnel b-tree est souvent le choix le plus simple et le plus logique lorsque vous implémentez une table optimisée en mémoire pour la première fois. Plus tard, après avoir vu comment votre application fonctionne, vous pourrez envisager l’utilisation d’un autre type d’index.

Deux types d’index doivent être abordés dans le contexte d’une table optimisée en mémoire : les index de hachage et les index Columnstore.

Pour une présentation des index de tables optimisées en mémoire, consultez :

- [Index pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)


#### <a name="hash-indexes"></a>Index de hachage

Les index de hachage peuvent être les plus rapides pour accéder à une ligne spécifique par le biais de la valeur exacte de sa clé primaire, à l’aide de l’opérateur '**=**'.

- Les opérateurs tels que '**!=**', '**>**' ou '**BETWEEN**' nuiront aux performances si vous les utilisez avec un index de hachage.

- Un index de hachage peut ne pas constituer le meilleur choix si la vitesse de duplication de la valeur de clé est trop élevée.

- Ne sous-estimez pas le nombre de *compartiments* dont peut avoir besoin votre index de hachage, afin d’éviter que les compartiments ne contiennent de longues chaînes. Pour plus d’informations, consultez :
    - [Index de hachage pour les tables mémoire optimisées](../../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md)


#### <a name="nonclustered-columnstore-indexes"></a>Index columnstore non cluster

Les tables optimisées en mémoire fournissent un débit élevé de données transactionnelles d’entreprise dans le paradigme appelé *traitement transactionnel en ligne* ou *OLTP*. Les index ColumnStore fournissent un débit élevé d’agrégations et un traitement similaire appelé *Analyse*. Ces dernières années, la meilleure approche disponible pour répondre aux besoins en matière d’OLTP et d’analyse était d’avoir des tables séparées, ce qui impliquait un déplacement important de données et une certaine duplication de données. Aujourd’hui, une **solution hybride** plus simple est disponible. Elle consiste à utiliser un index columnstore dans une table optimisée en mémoire.


- Un [index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md) peut être généré dans une table basée sur disque, même comme index cluster. Toutefois, dans une table optimisée en mémoire, l’index columnstore ne peut pas être mis en cluster.


- Les colonnes LOB (Large Object) et hors ligne d’une table optimisée en mémoire empêchent la création d’un index columnstore dans la table.


- Aucune instruction ALTER TABLE ne peut être exécutée dans une table optimisée en mémoire si celle-ci a un index columnstore.
    - Depuis août 2016, Microsoft envisage, à court terme, d’améliorer les performances de recréation d’index columnstore.



### <a name="d2-lob-and-off-row-columns"></a>D.2 Colonnes LOB et colonnes hors ligne

Les Large Objects (LOB) sont des colonnes de types tels que varchar(**max**). Le fait d’avoir deux colonnes LOB dans une table optimisée en mémoire ne nuit pas suffisamment aux performances pour poser problème. Toutefois, évitez d’avoir plus de colonnes LOB que le nécessitent vos données. Le même conseil s’applique pour les colonnes hors ligne. Ne définissez pas une colonne comme nvarchar(3072) si varchar(512) suffit.


Pour plus d’informations sur les colonnes LOB et hors ligne, consultez :

- [Taille de la table et des lignes dans les tables mémoire optimisées](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)
- [Types de données pris en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)



## <a name="e-limitations-of-native-procs"></a>E. Limitations des procédures natives


Certains éléments de Transact-SQL ne sont pas pris en charge dans les modules T-SQL compilés en mode natif, y compris les procédures stockées. Pour plus d’informations sur les fonctionnalités prises en charge, consultez :

- [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)

Pour plus d’informations sur les problèmes liés à la migration de modules Transact-SQL utilisant des fonctionnalités non prises en charge vers des modules compilés en mode natif, consultez :

- [Problèmes de migration pour les procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/migration-issues-for-natively-compiled-stored-procedures.md)

Outre les restrictions relatives à certains éléments de Transact-SQL, il existe également des restrictions quant aux opérateurs de requête pris en charge dans les modules T-SQL compilés en mode natif. En raison de ces limitations, les procédures stockées compilées en mode natif ne conviennent pas aux requêtes analytiques qui traitent de grands jeux de données.

#### <a name="no-parallel-processing-in-a-native-proc"></a>Aucun traitement parallèle dans une procédure native

Le traitement parallèle ne peut pas faire partie d’un plan de requête d’une procédure native. Les procédures natives sont toujours monothread.


#### <a name="join-types"></a>Types de jointure


Ni la jointure de fusion ni la jointure de hachage ne peuvent faire partie d’un plan de requête d’une procédure native. Des jointures de boucles imbriquées sont utilisées.


#### <a name="no-hash-aggregation"></a>Aucune agrégation de hachage

Lorsque le plan de requête d’une procédure native nécessite une phase d’agrégation, seule l’agrégation de flux est disponible. L’agrégation de hachage n’est pas prise en charge dans un plan de requête de procédure native.

- L’agrégation de hachage est préférable lorsque les données provenant d’un grand nombre de lignes doivent être regroupées.



## <a name="f-application-design-transactions-and-retry-logic"></a>F. Conception de l’application : transactions et logique des nouvelles tentatives

Une transaction impliquant une table optimisée en mémoire peut devenir dépendante d’une autre transaction qui implique la même table. Si le nombre de transactions dépendantes dépasse la valeur maximale autorisée, toutes les opérations dépendantes échouent.

Dans SQL Server 2016 :

- La valeur maximale autorisée est de 8 transactions dépendantes. 8 correspond également au nombre maximal de transactions dont une transaction peut dépendre.
- Le numéro de l’erreur est 41839. (Dans SQL Server 2014, le numéro de l’erreur est 41301).


Vous pouvez renforcer vos scripts Transact-SQL par rapport à une possible erreur de transaction en leur ajoutant une *logique de nouvelle tentative* . La logique de nouvelle tentative peut aider en cas d’appels UPDATE et DELETE fréquents, ou si la table optimisée en mémoire est référencée par une clé étrangère d’une autre table. Pour plus d’informations, consultez :

- [Transactions avec tables optimisées en mémoire](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)
- [Limites des dépendances de transaction avec des tables optimisées en mémoire – Erreur 41839](https://blogs.msdn.microsoft.com/sqlcat/2016/07/11/transaction-dependency-limits-with-memory-optimized-tables-error-41839/)



## <a name="related-links"></a>Liens connexes

- [OLTP en mémoire (optimisation en mémoire)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)


