---
title: Optimiser le traitement JSON avec OLTP en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: json
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- dbe-json
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d9c5adb1-3209-4186-bc10-8e41a26f5e57
caps.latest.revision: 3
author: jovanpop-msft
ms.author: jovanpop
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b969dac20c3869deb74d70e530d5b97e5fcc30dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="optimize-json-processing-with-in-memory-oltp"></a>Optimiser le traitement JSON avec OLTP en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

SQL Server et Azure SQL Database vous permettent de travailler avec du texte au format JSON. Pour améliorer les performances des requêtes qui traitent des données JSON, vous pouvez stocker des documents JSON dans des tables à mémoire optimisée en utilisant des colonnes de chaîne standard (type NVARCHAR). Le stockage des données JSON dans des tables optimisées en mémoire améliore les performances des requêtes en tirant parti de l’accès aux données en mémoire sans verrou.

## <a name="store-json-in-memory-optimized-tables"></a>Stocker des données JSON dans des tables optimisées en mémoire
L’exemple suivant montre une table optimisée en mémoire `Product` avec deux colonnes JSON, `Tags` et `Data` :

```sql
CREATE SCHEMA xtp;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED, --standard column
    Name nvarchar(400) NOT NULL, --standard column
    Price float, --standard column

    Tags nvarchar(400),--json stored in string column
    Data nvarchar(4000) --json stored in string column

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="optimize-json-processing-with-additional-in-memory-features"></a>Optimiser le traitement JSON avec des fonctionnalités en mémoire supplémentaires
Les fonctionnalités disponibles dans SQL Server et Azure SQL Database vous permettent d’intégrer les fonctionnalités JSON avec les technologies OLTP en mémoire existantes. Vous pouvez par exemple effectuer les opérations suivantes :
 - [Valider la structure des documents JSON](#validate) stockés dans des tables à mémoire optimisée à l’aide des contraintes CHECK compilées en mode natif.
 - [Exposer et typer fortement les valeurs](#computedcol) stockées dans les documents JSON à l’aide de colonnes calculées.
 - [Indexer les valeurs](#index) des documents JSON à l’aide d’index à mémoire optimisée.
 - [Compiler en mode natif les requêtes SQL](#compile) qui utilisent des valeurs de documents JSON ou mettent en forme les résultats au format de texte JSON.

## <a name="validate"></a> Valider des colonnes JSON
SQL Server et Azure SQL Database vous permettent d’ajouter des contraintes CHECK compilées en mode natif qui valident le contenu de documents JSON stockés dans une colonne de chaîne. Avec les contraintes JSON CHECK compilées en mode natif, vous pouvez faire en sorte que le texte JSON stocké dans vos tables à mémoire optimisée soit correctement mis en forme.

L’exemple suivant crée une table `Product` avec une colonne JSON `Tags`. La colonne `Tags` a une contrainte CHECK qui utilise la fonction `ISJSON` pour valider le texte JSON dans la colonne.

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Tags nvarchar(400)
            CONSTRAINT [Tags should be formatted as JSON]
                CHECK (ISJSON(Tags)=1),
    Data nvarchar(4000)

) WITH (MEMORY_OPTIMIZED=ON);
```

Vous pouvez également ajouter la contrainte CHECK compilée en mode natif à une table existante qui contient des colonnes JSON.

```sql
ALTER TABLE xtp.Product
    ADD CONSTRAINT [Data should be JSON]
        CHECK (ISJSON(Data)=1)
```

## <a name="computedcol"></a> Exposer les valeurs JSON à l’aide de colonnes calculées
Les colonnes calculées vous permettent d’exposer les valeurs d’un texte JSON et d’accéder à ces valeurs sans avoir à récupérer à nouveau la valeur du texte JSON ni à réanalyser la structure JSON. Les valeurs exposées de cette manière sont fortement typées et conservées physiquement dans les colonnes calculées. L’accès aux valeurs JSON à l’aide de colonnes calculées persistantes est plus rapide que l’accès aux valeurs dans le document JSON directement.

L’exemple suivant montre comment exposer les deux valeurs suivantes à partir de la colonne JSON `Data` :
-   Le pays où un produit est fabriqué.
-   Le coût de fabrication du produit.

Dans cet exemple, les colonnes calculées `MadeIn` et `Cost` sont mises à jour chaque fois que le document JSON stocké dans la colonne `Data` est changé.

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float)

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="index"></a> Valeurs d’index dans les colonnes JSON
SQL Server et Azure SQL Database vous permettent d’indexer les valeurs des colonnes JSON à l’aide des index à mémoire optimisée. Les valeurs JSON qui sont indexées doivent être exposées et fortement typées à l’aide de colonnes calculées, comme illustré dans l’exemple suivant.

Les valeurs des colonnes JSON peuvent être indexées à l’aide des index standard NONCLUSTERED et HASH.
-   Les index NONCLUSTERED optimisent les requêtes qui sélectionnent des plages de lignes selon une valeur JSON ou trient les résultats par valeurs JSON.
-   Les index HASH optimisent les requêtes qui sélectionnent une seule ligne ou quelques lignes en spécifiant une valeur exacte à rechercher.

L’exemple suivant crée une table qui expose les valeurs JSON à l’aide de deux colonnes calculées. L’exemple crée un index NONCLUSTERED sur une valeur JSON et un index HASH sur l’autre.

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float) PERSISTED,

    INDEX [idx_Product_MadeIn] NONCLUSTERED (MadeIn)

) WITH (MEMORY_OPTIMIZED=ON)

ALTER TABLE Product
    ADD INDEX [idx_Product_Cost] NONCLUSTERED HASH(Cost)
        WITH (BUCKET_COUNT=20000)
```

## <a name="compile"></a> Compilation native des requêtes JSON
Si vos procédures, fonctions et déclencheurs contiennent des requêtes qui utilisent les fonctions JSON intégrées, la compilation native améliore les performances de ces requêtes et réduit les cycles processeur nécessaires pour les exécuter.

L’exemple suivant montre une procédure compilée en mode natif qui utilise plusieurs fonctions JSON : **JSON_VALUE**, **OPENJSON** et **JSON_MODIFY**.

```sql
CREATE PROCEDURE xtp.ProductList(@ProductIds nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    SELECT ProductID,Name,Price,Data,Tags, JSON_VALUE(data,'$.MadeIn') AS MadeIn
    FROM xtp.Product
        JOIN OPENJSON(@ProductIds)
            ON ProductID = value

END;

CREATE PROCEDURE xtp.UpdateProductData(@ProductId int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE xtp.Product
    SET Data = JSON_MODIFY(Data, @Property, @Value)
    WHERE ProductID = @ProductId;

END
```

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>En savoir plus sur JSON dans SQL Server et Azure SQL Database  
  
### <a name="microsoft-blog-posts"></a>Billets de blog Microsoft  
  
Pour accéder à un grand nombre de solutions spécifiques, de cas d’usage et de recommandations, consultez ces [billets de blog](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/) sur la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database.  

### <a name="microsoft-videos"></a>Vidéos Microsoft

Pour obtenir une présentation visuelle de la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database, consultez les vidéos suivantes :

-   [SQL Server 2016 et prise en charge de JSON](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Utilisation de JSON dans SQL Server 2016 et Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON : un pont entre les mondes relationnel et NoSQL](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
