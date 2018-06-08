---
title: Guide d’architecture de traitement des requêtes | Microsoft Docs
ms.custom: ''
ms.date: 02/16/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: relational-databases-misc
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- guide, query processing architecture
- query processing architecture guide
ms.assetid: 44fadbee-b5fe-40c0-af8a-11a1eecf6cb5
caps.latest.revision: 5
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 15fd6269a2e879eba086af8d1d143cc0e0cffc1c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="query-processing-architecture-guide"></a>Guide d’architecture de traitement des requêtes
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] traite les requêtes sur diverses architectures de stockage des données, telles que des tables locales, des tables partitionnées et des tables distribuées sur plusieurs serveurs. Les rubriques suivantes expliquent comment [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] traite les requêtes et optimise leur réutilisation grâce à la mise en cache du plan d’exécution.

## <a name="sql-statement-processing"></a>Traitement des instructions SQL

Le traitement d'une instruction SQL unique est le cas le plus simple d'exécution par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Les étapes de traitement d’une instruction `SELECT` unique qui ne fait référence qu’à des tables de base locales (et non à des vues ou à des tables distantes) illustrent le processus de base.

#### <a name="logical-operator-precedence"></a>Priorité des opérateurs logiques

Quand une instruction contient plusieurs opérateurs logiques, `NOT` est traité en premier, ensuite `AND` et enfin `OR`. Les opérateurs arithmétiques, et au niveau du bit, sont traités avant les opérateurs logiques. Pour plus d’informations, consultez [Priorité des opérateurs](../t-sql/language-elements/operator-precedence-transact-sql.md).

Dans l'exemple suivant, la condition de couleur est associée au modèle de produit 21 et non au modèle de produit 20, car `AND` est prioritaire sur `OR`.

```sql
SELECT ProductID, ProductModelID
FROM Production.Product
WHERE ProductModelID = 20 OR ProductModelID = 21
  AND Color = 'Red';
GO
```

Vous pouvez modifier la signification de la requête en forçant le traitement de `OR` en premier lieu à l'aide de parenthèses. La requête suivante recherche uniquement les produits rouges dans les modèles 20 et 21.

```sql
SELECT ProductID, ProductModelID
FROM Production.Product
WHERE (ProductModelID = 20 OR ProductModelID = 21)
  AND Color = 'Red';
GO
```

L'utilisation de parenthèses, même quand elles ne sont pas nécessaires, peut améliorer la lisibilité des requêtes et limiter les risques d'erreurs dues à la priorité des opérateurs. L'utilisation de parenthèses ne diminue pas les performances du système. L'exemple suivant est plus lisible que le premier, bien qu'il soit identique sur le plan de la syntaxe.

```sql
SELECT ProductID, ProductModelID
FROM Production.Product
WHERE ProductModelID = 20 OR (ProductModelID = 21
  AND Color = 'Red');
GO
```

#### <a name="optimizing-select-statements"></a>Optimisation des instructions SELECT

Une instruction `SELECT` est non procédurale ; elle ne précise pas les étapes exactes à suivre par le serveur de base de données pour extraire les données demandées. Cela signifie que le serveur de base de données doit analyser l'instruction afin de déterminer la manière la plus efficace d'extraire les données demandées. Cette opération est nommée optimisation de l’instruction `SELECT` . Le composant qui s’en charge est l’optimiseur de requête. L’entrée de l’optimiseur de requête est composée de la requête, du schéma de base de données (définitions des tables et des index) et de ses statistiques de base de données. La sortie de l’optimiseur de requête est un plan d’exécution de la requête, parfois appelé plan de requête ou simplement plan. Le contenu d'un plan de requête est détaillé plus loin dans cette rubrique.

Les entrées et les sorties de l’optimiseur de requête pendant l’optimisation d’une instruction `SELECT` unique sont illustrées dans le diagramme suivant :

![query_processor_io](../relational-databases/media/query-processor-io.gif)

Une instruction `SELECT` ne définit que :  
* le format du jeu de résultats. Il est principalement spécifié dans la liste de sélection. Toutefois, d’autres clauses telles que `ORDER BY` et `GROUP BY` influencent également la syntaxe finale du jeu de résultats.
* les tables contenant les données source. Ceci est spécifié dans la clause `FROM` .
* la manière dont les tables sont reliées de façon logique pour les besoins de l’instruction `SELECT` . Elle est définie dans les spécifications de jointure, qui peuvent être présentes dans la clause `WHERE` ou dans une clause `ON` à la suite de `FROM`.
* Les conditions auxquelles doivent répondre les lignes des tables sources afin de correspondre à l’instruction `SELECT` . Elles sont spécifiées dans les clauses `WHERE` et `HAVING` .

Un plan d'exécution de requête permet de définir : 

* l'ordre d'accès aux tables source.  
  Pour créer le jeu de résultats, le serveur de bases de données peut accéder aux tables de base selon de nombreux ordres différents. Par exemple, si l’instruction `SELECT` fait référence à trois tables, le serveur de base de données accédera d’abord à `TableA`, utilisera les données de `TableA` pour extraire les lignes correspondantes de `TableB`, puis utilisera les données de `TableB` pour extraire les données de `TableC`. Les autres séquences dans lesquelles le serveur de bases de données peut accéder aux tables sont les suivantes :  
  `TableC`, `TableB`, `TableA`ou  
  `TableB`, `TableA`, `TableC`ou  
  `TableB`, `TableC`, `TableA`ou  
  `TableC`, `TableA`, `TableB`  

* les méthodes utilisées pour extraire les données de chaque table.  
  Il existe également différentes méthodes d'accès aux données dans chaque table. Si seules quelques lignes ayant des valeurs de clés spécifiques sont nécessaires, le serveur de base de données peut utiliser un index. Si toutes les lignes de la table sont nécessaires, le serveur de base de données peut ignorer les index et procéder à une analyse de la table. Si toutes les lignes de la table sont nécessaires mais qu’il existe un index dont les colonnes clés se trouvent dans une clause `ORDER BY`, l’analyse d’index plutôt que l’analyse de table peut éviter un tri séparé du jeu de résultats. Dans le cas d'une table très petite, les analyses de table peuvent s'avérer plus efficaces pour quasiment tous les accès à la table.

Le processus de sélection d'un plan d'exécution parmi plusieurs possibles est appelé optimisation. L'optimiseur de requêtes est un des composants les plus importants d'un système de base de données SQL. Bien que l'optimiseur de requête puisse créer une certaine surcharge pour analyser la requête et sélectionner un plan, celle-ci est en général largement compensée par l'adoption d'un plan d'exécution efficace. Prenons l'exemple de deux entrepreneurs en bâtiment à qui l'on commande la même maison. Si l'un d'eux commence par consacrer quelques jours à planifier la construction de cette maison alors que l'autre lance immédiatement la construction sans aucune planification, il est fort probable que celui qui a pris le temps de planifier son projet finira le premier.

L’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est un optimiseur de requête basé sur les coûts. À chaque plan d'exécution possible est associé un coût exprimé en termes de quantité de ressources informatiques utilisées. L'optimiseur de requêtes doit analyser les plans possibles et opter pour celui dont le coût estimé est le plus faible. Certaines instructions `SELECT` complexes disposent de milliers de plans d’exécution possibles. Dans ce cas, l'optimiseur de requêtes n'analyse pas toutes les combinaisons possibles. Il recourt alors à des algorithmes sophistiqués afin de trouver un plan d'exécution dont le coût se rapproche raisonnablement du minimum possible.

L’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] choisit non seulement le plan d’exécution dont le coût en ressources est le plus faible, mais également celui qui retourne le plus rapidement les résultats à l’utilisateur moyennant un coût en ressources raisonnable. Par exemple, le traitement d'une requête en parallèle monopolise généralement davantage de ressources qu'un traitement en série, mais il est plus rapide. L’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise un plan d’exécution en parallèle pour retourner les résultats si la charge du serveur n’en est pas affectée de façon rédhibitoire.

L’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se base sur les statistiques de distribution lors de l’estimation du coût en ressources des différentes méthodes d’extraction d’informations à partir d’une table ou d’un index. Les statistiques de distribution sont conservées pour les colonnes et les index. Elles indiquent la sélectivité des valeurs dans un index ou une colonne en particulier. Par exemple, dans une table représentant des voitures, plusieurs voitures proviennent du même constructeur mais chacune a un numéro d'identification unique. Un index sur le numéro d'identification du véhicule est plus sélectif qu'un index sur le constructeur. Si les statistiques d'index ne sont pas à jour, l'optimiseur de requêtes peut ne pas effectuer le meilleur choix pour l'état actuel de la table. Pour plus d'informations sur la tenue à jour des statistiques d'index, consultez [Statistiques](../relational-databases/statistics/statistics.md). 

L’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est important, car il permet l’ajustement dynamique du serveur de base de données au fur et à mesure que la base de données évolue sans recourir à l’intervention d’un programmeur ou d’un administrateur de bases de données. Cela permet aux programmeurs de se concentrer sur la description du résultat final de la requête. Ils peuvent faire confiance à l’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans son choix d’un plan d’exécution efficace pour l’état de la base de données à chaque exécution de l’instruction.

#### <a name="processing-a-select-statement"></a>Traitement d'une instruction SELECT

Les étapes permettant à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de traiter une instruction SELECT unique sont les suivantes : 

1. L’analyseur examine l’instruction `SELECT` et la décompose en unités logiques telles que mots clé, expressions, opérateurs et identificateurs.
2. Un arbre de requêtes, également appelé arbre de séquence, est créé pour décrire les étapes logiques nécessaires à la transformation des données source au format requis par le jeu de résultats.
3. L'optimiseur de requête analyse plusieurs méthodes d'accès aux tables source. Il choisit ensuite la série d'étapes qui retourne les résultats le plus rapidement tout en consommant moins de ressources. L'arbre de requêtes est mis à jour pour enregistrer cette série exacte d'étapes. La version optimisée finale de l'arbre de requêtes est nommée plan d'exécution.
4. Le moteur relationnel lance le plan d'exécution. Pendant le traitement des étapes qui requièrent des données issues des tables de base, le moteur relationnel demande que le moteur de stockage transmette les données des ensembles de lignes demandés à partir du moteur relationnel.
5. Le moteur relationnel traite les données retournées du moteur de stockage dans le format défini pour le jeu de résultats et retourne ce jeu au client.

#### <a name="processing-other-statements"></a>Traitement des autres instructions

Les étapes de base décrites pour le traitement d’une instruction `SELECT` s’appliquent également aux autres instructions SQL telles que `INSERT`, `UPDATE`et `DELETE`. Les instructions`UPDATE` et `DELETE` doivent toutes deux cibler l’ensemble de lignes à modifier ou à supprimer. Le processus d’identification de ces lignes est le même que celui utilisé pour identifier les lignes sources qui participent au jeu de résultats d’une instruction `SELECT` . Les instructions `UPDATE` et `INSERT` peuvent toutes deux contenir des instructions SELECT incorporées qui fournissent les valeurs de données à mettre à jour ou à insérer.

Même les instructions DDL telles que `CREATE PROCEDURE` ou `ALTER TABL`sont finalement réduites à une série d’opérations relationnelles sur les tables du catalogue système, voire (comme dans le cas de `ALTER TABLE ADD COLUMN`) sur les tables de données.

### <a name="worktables"></a>Tables de travail

Le moteur relationnel peut avoir besoin de créer une table de travail pour exécuter une opération logique spécifiée dans une instruction SQL. Les tables de travail sont des tables internes utilisées pour le stockage des résultats intermédiaires. Les tables de travail sont générées pour certaines requêtes `GROUP BY`, `ORDER BY`, ou `UNION` . Par exemple, si une clause `ORDER BY` fait référence à des colonnes qui ne sont couvertes par aucun index, le moteur relationnel peut être amené à générer une table de travail pour trier l’ensemble de résultats dans l’ordre demandé. En outre, les tables de travail sont parfois utilisées comme fichiers d'attente pour le stockage temporaire du résultat de l'exécution d'une partie d'un plan de requête. Les tables de travail sont créées dans `tempdb` et sont automatiquement supprimées lorsqu’elles ne sont plus nécessaires.

### <a name="view-resolution"></a>Résolution de vues

Le processeur de requêtes [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] traite différemment les vues indexées et les vues non indexées : 

* Les lignes des vues indexées sont stockées dans la base de données dans le même format qu'une table. Si l'optimiseur de requête décide d'utiliser une vue indexée dans un plan de requête, celle-ci est traitée de la même façon qu'une table de base.
* Seule la définition d'une vue non indexée est stockée, tandis que les lignes de la vue ne le sont pas. L’optimiseur de requête incorpore la logique de la définition de la vue dans le plan d’exécution qu’il construit pour l’instruction SQL référençant la vue non indexée. 

La logique utilisée par l’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour déterminer quand utiliser une vue indexée est similaire à la logique employée pour décider quand utiliser un index sur une table. Si les données de la vue indexée couvrent tout ou partie de l’instruction SQL et si l’optimiseur de requête détermine qu’un index sur la vue est le chemin le moins coûteux, l’optimiseur choisit l’index, que la vue soit référencée ou non par son nom dans la requête.

Lorsqu'une instruction SQL fait référence à une vue non indexée, l'analyseur et l'optimiseur de requête analysent la source de l'instruction SQL et de la vue, puis les résolvent dans un même plan d'exécution. Il n'y a pas de plans distincts pour l'instruction SQL et pour la vue.

Imaginons par exemple la vue suivante :

```sql
USE AdventureWorks2014;
GO
CREATE VIEW EmployeeName AS
SELECT h.BusinessEntityID, p.LastName, p.FirstName
FROM HumanResources.Employee AS h 
JOIN Person.Person AS p
ON h.BusinessEntityID = p.BusinessEntityID;
GO
```

Sur la base de cette vue, les deux instructions SQL exécutent les mêmes opérations sur les tables de base et produisent les mêmes résultats :

```sql
/* SELECT referencing the EmployeeName view. */
SELECT LastName AS EmployeeLastName, SalesOrderID, OrderDate
FROM AdventureWorks2014.Sales.SalesOrderHeader AS soh
JOIN AdventureWorks2014.dbo.EmployeeName AS EmpN
ON (soh.SalesPersonID = EmpN.BusinessEntityID)
WHERE OrderDate > '20020531';

/* SELECT referencing the Person and Employee tables directly. */
SELECT LastName AS EmployeeLastName, SalesOrderID, OrderDate
FROM AdventureWorks2014.HumanResources.Employee AS e 
JOIN AdventureWorks2014.Sales.SalesOrderHeader AS soh
ON soh.SalesPersonID = e.BusinessEntityID
JOIN AdventureWorks2014.Person.Person AS p
ON e.BusinessEntityID =p.BusinessEntityID
WHERE OrderDate > '20020531';
```

La fonctionnalité Showplan de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio montre que le moteur relationnel crée le même plan d’exécution pour ces deux instructions `SELECT`.

#### <a name="using-hints-with-views"></a>Utilisation d'indicateurs avec les vues

Les indicateurs placés sur une vue dans une requête peuvent être en conflit avec d'autres indicateurs découverts lors du développement de la vue pour l'accès à ses tables de base. Lorsque cela se produit, la requête retourne une erreur. Imaginons par exemple la vue suivante, dont la définition contient un indicateur de table :

```sql
USE AdventureWorks2014;
GO
CREATE VIEW Person.AddrState WITH SCHEMABINDING AS
SELECT a.AddressID, a.AddressLine1, 
    s.StateProvinceCode, s.CountryRegionCode
FROM Person.Address a WITH (NOLOCK), Person.StateProvince s
WHERE a.StateProvinceID = s.StateProvinceID;
```

Supposons à présent cette requête :

```sql
SELECT AddressID, AddressLine1, StateProvinceCode, CountryRegionCode
FROM Person.AddrState WITH (SERIALIZABLE)
WHERE StateProvinceCode = 'WA';
```

La requête échoue, car l’indicateur `SERIALIZABLE` appliqué à la vue `Person.AddrState` de la requête est propagé dans les tables `Person.Address` et `Person.StateProvince` de la vue lors du développement de cette dernière. Cependant, le développement de la vue révèle également l’indicateur `NOLOCK` sur `Person.Address`. La requête résultante est incorrecte parce que les indicateurs `SERIALIZABLE` et `NOLOCK` sont en conflit. 

Les indicateurs de table `PAGLOCK`, `NOLOCK`, `ROWLOCK`, `TABLOCK`ou `TABLOCKX` sont en conflit les uns avec les autres, tout comme les indicateurs de table `HOLDLOCK`, `NOLOCK`, `READCOMMITTED`, `REPEATABLEREAD`et `SERIALIZABLE` .

Les indicateurs peuvent se propager à différents niveaux des vues imbriquées. Imaginons par exemple une requête qui applique l’indicateur `HOLDLOCK` sur une vue `v1`. Lorsque `v1` est développé, il est établit que la vue `v2` fait partie de sa définition. La définition de`v2`inclut un indicateur `NOLOCK` sur l’une de ses tables de base. Cependant, cette table hérite également de l’indicateur `HOLDLOCK` de la requête sur la vue `v1`. La requête échoue parce que les indicateurs `NOLOCK` et `HOLDLOCK` sont en conflit.

Si l’indicateur `FORCE ORDER` est utilisé dans une requête contenant une vue, l’ordre de jointure des tables de la vue est déterminé par la position de la vue dans la construction ordonnée. Par exemple, la requête suivante effectue une sélection dans trois tables et une vue :

```sql
SELECT * FROM Table1, Table2, View1, Table3
WHERE Table1.Col1 = Table2.Col1 
    AND Table2.Col1 = View1.Col1
    AND View1.Col2 = Table3.Col2;
OPTION (FORCE ORDER);
```

`View1` est définie comme suit :

```sql
CREATE VIEW View1 AS
SELECT Colx, Coly FROM TableA, TableB
WHERE TableA.ColZ = TableB.Colz;
```

L’ordre de jointure dans le plan de requête est `Table1`, `Table2`, `TableA`, `TableB`, `Table3`.

### <a name="resolving-indexes-on-views"></a>Résolution d'index sur les vues

Comme avec tout index, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] choisit d’utiliser une vue indexée dans son plan de requête uniquement si l’optimiseur de requête le juge opportun.

Les vues indexées peuvent être créées dans n'importe quelle version de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Dans certaines éditions de certaines versions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], l’optimiseur de requête considère automatiquement la vue indexée. Dans certaines éditions de certaines versions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], l’indicateur de table `NOEXPAND` est nécessaire pour utiliser une vue indexée. Consultez la documentation de chaque version pour obtenir des précisions.

L’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise une vue indexée quand les conditions suivantes sont remplies : 

* Ces options de session sont activées ( `ON`) : 
  * `ANSI_NULLS`
  * `ANSI_PADDING`
  * `ANSI_WARNINGS`
  * `ARITHABORT`
  * `CONCAT_NULL_YIELDS_NULL`
  * `QUOTED_IDENTIFIER` 
  * L’option de session `NUMERIC_ROUNDABORT` est désactivée (OFF).
* L'optimiseur de requête trouve une correspondance entre les colonnes d'index des vues et les éléments de la requête, notamment : 
  * Prédicats de la condition de recherche dans la clause WHERE
  * Opérations de jointure
  * Fonctions d'agrégation
  * Clauses`GROUP BY` 
  * Références de table
* Le coût estimé de l'utilisation de l'index est le plus faible de tous les mécanismes d'accès envisagés par l'optimiseur de requête. 
* Dans la requête, vous devez appliquer le même ensemble d'indicateurs à chaque table que vous référencez, soit directement, soit en développant une vue afin d'accéder à ses tables sous-jacentes, et qui correspond à une référence de table dans la vue indexée.

> [!NOTE] 
> Les indicateurs `READCOMMITTED` et `READCOMMITTEDLOCK` sont toujours considérés comme des indicateurs différents dans ce contexte, indépendamment du niveau d’isolation de la transaction en cours.
 
En dehors des exigences relatives aux indicateurs de table et aux options `SET`, l’optimiseur de requête emploie ces mêmes règles pour déterminer si l’index d’une table couvre une requête. Vous n'avez pas besoin de spécifier autre chose dans la requête pour utiliser une vue indexée.

Une requête ne doit pas obligatoirement référencer explicitement une vue indexée dans la clause `FROM` pour que l’optimiseur de requête utilise la vue indexée. Si la requête contient des références à des colonnes dans des tables de base qui sont également présentes dans la vue indexée, et si l'optimiseur de requête estime que l'emploi de la vue indexée offre le mécanisme d'accès le moins coûteux, il choisit la vue indexée, un peu comme il choisit les index des tables de base lorsque ceux-ci ne sont pas directement référencés dans une requête. L'optimiseur de requête peut choisir la vue lorsqu'elle contient des colonnes qui ne sont pas référencées par la requête, à condition que cette dernière offre l'option la moins coûteuse pour couvrir une ou plusieurs colonnes spécifiées dans la requête.

L’optimiseur de requête traite une vue indexée référencée dans la clause `FROM` comme une vue standard. L'optimiseur de requête développe la définition de la vue dans la requête au début du processus d'optimisation. Ensuite, la mise en correspondance des éléments de la vue indexée est réalisée. La vue indexée peut être utilisée dans le plan d’exécution final sélectionné par l’optimiseur de requête ou, sinon, le plan peut matérialiser les données nécessaires à partir de la vue en accédant aux tables de base référencées par celle-ci. L’optimiseur de requête choisit la solution la plus économique.

#### <a name="using-hints-with-indexed-views"></a>Utilisation d'indicateurs avec les vues indexées

Vous pouvez empêcher l’utilisation d’index de vue pour une requête à l’aide de l’indicateur de requête `EXPAND VIEWS` ou recourir à l’indicateur de table `NOEXPAND` afin d’imposer l’utilisation d’un index pour une vue indexée spécifiée dans la clause `FROM` d’une requête. Toutefois, vous devez laisser l'optimiseur de requête déterminer dynamiquement les meilleures méthodes d'accès à utiliser pour chaque requête. Limitez l’utilisation des indicateurs `EXPAND` et `NOEXPAND` aux cas spécifiques où les tests ont démontré qu’ils améliorent les performances de façon significative.

L’option `EXPAND VIEWS` ordonne à l’optimiseur de requête de ne pas utiliser des index de vue pour toute la requête. 

Quand `NOEXPAND` est spécifié dans une vue, l’optimiseur de requête envisage l’utilisation de n’importe quel index défini sur la vue. `NOEXPAND` spécifié avec la clause `INDEX()` facultative force l’optimiseur de requête à utiliser les index spécifiés. `NOEXPAND` peut être spécifié uniquement pour une vue indexée et ne peut pas être spécifié pour une vue qui n’a pas été indexée.

Lorsque ni `NOEXPAND` ni `EXPAND VIEWS` ne sont spécifiés dans une requête qui contient une vue, celle-ci est développée de manière à permettre l’accès aux tables sous-jacentes. Si la requête qui compose la vue contient des indicateurs de table, ceux-ci sont propagés aux tables sous-jacentes. (Ce processus est expliqué en détail dans Résolution de vues.) Si les ensembles d'indicateurs existant sur les tables sous-jacentes de la vue sont identiques, la requête peut être mise en correspondance avec une vue indexée. La plupart du temps, ces indicateurs correspondent les uns aux autres car ils sont hérités directement de la vue. Toutefois, si la requête référence des tables au lieu de vues et que les indicateurs appliqués directement à ces tables ne sont pas identiques, cette requête ne peut pas être mise en correspondance avec une vue indexée. Si les indicateurs `INDEX`, `PAGLOCK`, `ROWLOCK`, `TABLOCKX`, `UPDLOCK`ou `XLOCK` s’appliquent aux tables référencées dans la requête une fois la vue développée, la requête ne peut pas être mise en correspondance avec la vue indexée.

Si un indicateur de table de la forme `INDEX (index_val[ ,...n] )` référence une vue dans une requête et que vous ne spécifiez pas l’indicateur `NOEXPAND` , l’indicateur d’index est ignoré. Pour spécifier l’utilisation d’un index particulier, utilisez `NOEXPAND`. 

En règle générale, quand l’optimiseur de requête fait correspondre une vue indexée avec une requête, tous les indicateurs spécifiés sur les tables ou vues dans la requête sont appliqués directement à la vue indexée. Si l'optimiseur de requête choisit de ne pas utiliser une vue indexée, tous les indicateurs sont propagés directement aux tables référencées dans la vue. Pour plus d’informations, consultez Résolution de vues. Cette propagation ne s'applique pas aux indicateurs de jointure. Ils ne sont appliqués qu'à leur emplacement initial dans la requête. Les indicateurs de jointure ne sont pas envisagés par l'optimiseur de requête lors de la mise en correspondance des requêtes avec les vues indexées. Si un plan de requête utilise une vue indexée qui correspond à une partie d'une requête contenant un indicateur de jointure, celui-ci n'est pas utilisé dans le plan.

L'utilisation d'indicateurs n'est pas autorisée dans les définitions de vues indexées. Dans les modes de compatibilité 80 et supérieurs, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ignore les indicateurs présents dans les définitions de vues indexées lorsqu'il gère ces définitions ou qu'il exécute des requêtes qui utilisent des vues indexées. Bien que l'utilisation d'indicateurs dans les définitions de vues indexées ne génère pas d'erreur de syntaxe dans le mode de compatibilité 80, ils sont ignorés.

### <a name="resolving-distributed-partitioned-views"></a>Résolution de vues distribuées partitionnées

Le processeur de requêtes [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] optimise les performances des vues partitionnées distribuées. L'aspect le plus important des performances d'une vue distribuée partitionnée est de minimiser la quantité de données à transférer entre des serveurs membres.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] construit des plans intelligents et dynamiques qui utilisent efficacement les requêtes distribuées pour accéder aux données à partir des tables membres distantes : 

* Le processeur de requêtes utilise d’abord OLE DB pour récupérer les définitions des contraintes de vérification de chaque table membre. Ceci permet au processeur de requêtes de mapper la distribution des valeurs de clés entre les tables membres.
* The Query Processor compares the key ranges specified in an SQL statement `WHERE` d’une instruction SQL au mappage qui représente la distribution des lignes dans les tables membres. Le processeur de requêtes construit alors un plan d'exécution des requêtes qui utilise les requêtes distribuées pour récupérer uniquement les lignes distantes requises pour exécuter l'instruction SQL. Le plan d'exécution est également construit de telle sorte que tout accès aux tables membres distantes pour les données ou les métadonnées est différé jusqu'à ce que les informations soient requises.

Par exemple, prenons un système où une table de clients est partitionnée entre Server1 (`CustomerID` de 1 à 3299999), Server2 (`CustomerID` de 3300000 à 6599999) et Server3 (`CustomerID` de 6600000 à 9999999).

Étudiez le plan d’exécution qui est construit pour chaque requête exécutée sur Server1 :

```sql
SELECT *
FROM CompanyData.dbo.Customers
WHERE CustomerID BETWEEN 3200000 AND 3400000;
```

Le plan d’exécution pour cette requête extrait les lignes avec des valeurs de clés `CustomerID` de 3200000 à 3299999 de la table membre locale et émet une requête distribuée pour récupérer les lignes dont les valeurs de clés sont comprises entre 3300000 et 3400000 de Server2.

Le processeur de requêtes [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut également créer une logique dynamique dans les plans d’exécution de requêtes pour les instructions SQL dont les valeurs de clés ne sont pas connues au moment de la construction du plan. Prenons par exemple cette procédure stockée :

```sql
CREATE PROCEDURE GetCustomer @CustomerIDParameter INT
AS
SELECT *
FROM CompanyData.dbo.Customers
WHERE CustomerID = @CustomerIDParameter;
```

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne peut pas prévoir quelle valeur de clé sera fournie par le paramètre `@CustomerIDParameter` à chaque exécution de la procédure. Puisque la valeur de la clé ne peut pas être prévue, le processeur de requêtes ne peut pas non plus prévoir quelle table membre devra faire l'objet d'un accès. Pour gérer ce cas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] construit un plan d'exécution comportant une logique conditionnelle, également appelée filtres dynamiques, pour contrôler quelle table membre fait l'objet d'un accès en fonction de la valeur du paramètre d'entrée. En partant du principe que la procédure stockée `GetCustomer` a été exécutée sur Server1, la logique du plan d’exécution peut être représentée sous la forme suivante :

```sql
IF @CustomerIDParameter BETWEEN 1 and 3299999
   Retrieve row from local table CustomerData.dbo.Customer_33
ELSE IF @CustomerIDParameter BETWEEN 3300000 and 6599999
   Retrieve row from linked table Server2.CustomerData.dbo.Customer_66
ELSE IF @CustomerIDParameter BETWEEN 6600000 and 9999999
   Retrieve row from linked table Server3.CustomerData.dbo.Customer_99
```

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] crée parfois ces types de plans d’exécution dynamique même pour des requêtes qui ne sont pas paramétrables. L’optimiseur peut paramétrer une requête pour que le plan d’exécution puisse être réutilisé. Si l’optimiseur de requête paramètre une requête faisant référence à une vue partitionnée, il ne peut plus supposer que les lignes requises proviendront d’une table de base spécifiée. Il devra alors utiliser des filtres dynamiques dans le plan d'exécution.

## <a name="stored-procedure-and-trigger-execution"></a>Exécution d'une procédure stockée et d'un déclencheur

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] stocke uniquement le code source des procédures stockées et des déclencheurs. Quand une procédure stockée ou un déclencheur est exécuté pour la première fois, la source est compilée dans un plan d'exécution. Si la procédure stockée ou le déclencheur doit à nouveau être exécuté alors que le plan d'exécution se trouve encore en mémoire, le moteur relationnel détecte le plan existant et le réutilise. Si le plan est trop ancien et a été évacué de la mémoire, le système crée un nouveau plan. Ce processus est similaire à celui suivi par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour toutes les instructions SQL. L'avantage principal en matière de performances dont bénéficient les procédures stockées et les déclencheurs dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] par rapport aux lots SQL dynamiques est que leurs instructions SQL sont toujours les mêmes. Par conséquent, le moteur relationnel les associe facilement à n'importe quel plan d'exécution existant. Les plans des procédures stockées et des déclencheurs sont faciles à réutiliser.

Le plan d'exécution des procédures stockées et des déclencheurs est exécuté séparément du plan d'exécution du traitement qui appelle la procédure stockée ou qui active le déclencheur. Cela permet une meilleure réutilisation des plans d'exécution des procédures stockées et des déclencheurs.

## <a name="execution-plan-caching-and-reuse"></a>Mise en mémoire cache et réutilisation du plan d'exécution

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dispose d'un pool de mémoire utilisé pour stocker les plans d'exécution et les tampons de données. Le pourcentage de ce pool alloué aux plans d'exécution ou aux tampons de données évolue de façon dynamique en fonction de l'état du système. La part du pool de mémoire utilisée pour stocker les plans d’exécution est appelée « cache du plan ».

Les plans d'exécution de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] comprennent les composants principaux suivants : 

* Plan d’exécution de requête Le corps du plan d’exécution est une structure de données réentrante et en lecture seule qui peut être utilisée par un nombre quelconque d’utilisateurs. Il constitue le plan de requête. Aucun contexte d'utilisateur n'est stocké dans le plan de requête. Il n'y a jamais plus d'une ou deux copies du plan de requête en mémoire : une copie pour toutes les exécutions en série et une autre pour toutes les exécutions en parallèle. La copie en parallèle couvre toutes les exécutions en parallèle, indépendamment de leur degré de parallélisme. 
* Contexte d’exécution Chaque utilisateur exécutant actuellement la requête dispose d’une structure de données qui contient les données spécifiques à son exécution, telles que la valeur des paramètres. Cette structure de données constitue le contexte d'exécution. Les structures de données du contexte d'exécution sont réutilisées. Si un utilisateur exécute une requête et qu'une des structures n'est pas en cours d'utilisation, elle est réinitialisée avec le contexte du nouvel utilisateur. 

![execution_context](../relational-databases/media/execution-context.gif)

Quand une instruction SQL est exécutée dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le moteur relationnel parcourt d’abord le cache de plan afin de voir s’il existe un plan d’exécution pour la même instruction SQL. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] réutilise le plan existant qu’il trouve, évitant ainsi la recompilation de l’instruction SQL. S'il n'existe aucun plan d'exécution, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en génère un nouveau pour la requête.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dispose d'un algorithme efficace qui permet de trouver un plan d'exécution existant pour toute instruction SQL spécifique. Dans la plupart des systèmes, les ressources minimales utilisées par cette analyse sont inférieures à celles économisées par la réutilisation de plans existants au lieu de la compilation de toutes les instructions SQL.

Les algorithmes qui permettent d'associer de nouvelles instructions SQL à des plans d'exécution inutilisés existants en mémoire cache imposent que toutes les références d'objets soient complètes. Par exemple, la première de ces instructions `SELECT` n'est pas associée à un plan existant, contrairement à la seconde :

```sql
SELECT * FROM Person;

SELECT * FROM Person.Person;
```

### <a name="removing-execution-plans-from-the-plan-cache"></a>Suppression de plans d’exécution du cache du plan

Les plans d’exécution restent dans le cache du plan tant qu’il y a suffisamment de mémoire pour les stocker. En cas de sollicitation élevée de la mémoire, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise une approche basée sur les coûts pour identifier les plans d’exécution à supprimer du cache du plan. Pour prendre une décision basée sur les coûts, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] augmente et diminue une variable de coût actuel pour chaque plan d’exécution en fonction des facteurs suivants.

Lorsqu'un processus utilisateur insère un plan d'exécution dans le cache, il définit le coût actuel de sorte qu'il soit égal au coût de compilation de la requête d'origine ; pour les plans d'exécution ad hoc, le processus utilisateur définit le coût actuel à zéro. Ensuite, chaque fois qu'un processus utilisateur fait référence à un plan d'exécution, il réinitialise le coût actuel au coût de compilation d'origine ; pour les plans d'exécution ad hoc, le processus utilisateur augmente le coût actuel. Pour tous les plans, la valeur maximale du coût actuel correspond au coût de compilation d'origine.

En cas de sollicitation élevée de la mémoire, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] répond en supprimant des plans d’exécution du cache du plan. Pour identifier les plans à supprimer, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] examine plusieurs fois l’état de chaque plan d’exécution et supprime des plans quand leur coût actuel est nul. Un plan d’exécution avec un coût nul n’est pas supprimé automatiquement en cas de sollicitation élevée de la mémoire ; il est supprimé uniquement quand le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] examine le plan et que son coût actuel est nul. Lors de l’examen d’un plan d’exécution, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] pousse le coût actuel vers la valeur zéro en réduisant le coût actuel si aucune requête n’utilise actuellement le plan.

Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] examine plusieurs fois les plans d’exécution jusqu’à ce que le nombre de plans supprimés soit suffisant pour répondre aux exigences mémoire. En cas de sollicitation élevée de la mémoire, un plan d'exécution peut voir son coût augmenter et diminuer à plusieurs reprises. Quand la mémoire n’est plus sollicitée, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] cesse de réduire le coût actuel des plans d’exécution inutilisés et tous les plans d’exécution demeurent dans le cache du plan, même si leur coût est égal à zéro.

Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise le moniteur de ressources et des threads de travail utilisateur pour libérer de la mémoire dans le cache du plan en réponse à une sollicitation élevée de la mémoire. Le moniteur de ressources et les threads de travail utilisateur peuvent examiner les plans exécutés simultanément afin de réduire le coût actuel de chaque plan d’exécution inutilisé. Le moniteur de ressources supprime des plans d’exécution du cache du plan en cas de sollicitation élevée globale de la mémoire. Il libère de la mémoire afin d'appliquer les stratégies en matière de mémoire système, de mémoire de processus, de mémoire du pool de ressources et de taille maximale pour tous les caches. 

La taille maximale de tous les caches dépend de la taille du pool de mémoires tampons et ne peut pas dépasser la quantité maximale de mémoire du serveur. Pour plus d’informations sur la configuration de la quantité maximale de mémoire du serveur, consultez le paramètre `max server memory` dans `sp_configure`. 

Les threads de travail utilisateur suppriment des plans d’exécution du cache du plan en cas de sollicitation élevée de la mémoire d’un cache unique. Ils appliquent les stratégies de taille maximale de cache unique et de nombre maximal d'entrées de cache unique. 

Les exemples suivants illustrent les plans d’exécution qui sont supprimés du cache du plan :

* Un plan d'exécution est fréquemment référencé de sorte que son coût n'est jamais égal à zéro. Le plan reste dans le cache du plan et n’est pas supprimé tant qu’il n’y a pas de sollicitation de la mémoire et que le coût actuel n’est pas égal à zéro.
* Un plan d'exécution ad hoc est inséré ; il n'est plus référencé tant que la mémoire n'est pas sollicitée de manière élevée. Dans la mesure où les plans d’exécution ad hoc sont initialisés avec un coût actuel égal à zéro, quand le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] examine le plan d’exécution, il constate que le coût actuel est égal à zéro et supprime le plan du cache du plan. Le plan d’exécution ad hoc reste dans le cache du plan avec un coût actuel égal à zéro en l’absence de sollicitation de la mémoire.

Pour supprimer manuellement un seul plan ou l’ensemble des plans du cache, utilisez [DBCC FREEPROCCACHE](../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md).

### <a name="recompiling-execution-plans"></a>Recompilation des plans d'exécution

Certaines modifications dans une base de données peuvent entraîner l'inefficacité ou la non-validité d'un plan d'exécution, selon le nouvel état de la base de données. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] détecte les modifications qui rendent un plan d’exécution non valide et marque ce plan comme tel. Il faut donc recompiler un nouveau plan pour la prochaine connexion qui exécute la requête. Les conditions qui provoquent l'invalidité d'un plan sont les suivantes : 

* Les modifications apportées à une table ou à une vue référencée par la requête (`ALTER TABLE` et `ALTER VIEW`).
* Les modifications apportées à une seule procédure, ce qui supprimerait tous les plans de cette procédure dans le cache (`ALTER PROCEDURE`).
* Les modifications apportées à des index utilisés par le plan d'exécution.
* Les mises à jour de statistiques utilisées par le plan d’exécution, générées explicitement à partir d’une instruction, telle que `UPDATE STATISTICS`, ou automatiquement.
* La suppression d'un index utilisé par le plan d'exécution.
* Un appel explicite à `sp_recompile`.
* Un nombre important de modifications de clés (générées par les instructions `INSERT` ou `DELETE` des autres utilisateurs qui modifient une table référencée par la requête).
* Pour les tables contenant des déclencheurs, si le nombre de lignes des tables inserted ou deleted augmente de manière significative.
* L’exécution d’une procédure stockée à l’aide de l’option `WITH RECOMPILE` .

La plupart des recompilations sont nécessaires pour que les instructions soient correctes ou pour obtenir des plans d'exécution de requête potentiellement plus rapides.

Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000, chaque fois qu’une instruction d’un lot entraîne une recompilation, la totalité du lot est recompilée, qu’il soit soumis par le biais d’une procédure stockée, d’un déclencheur, d’un lot ad hoc ou d’une instruction préparée. À compter de [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], seule l’instruction qui déclenche la recompilation dans le lot est recompilée. En raison de cette différence, les nombres de recompilations dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000 et versions ultérieures ne sont pas comparables. En outre, il existe davantage de types de recompilations dans [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] et dans les versions ultérieures en raison de son ensemble de fonctionnalités étendu.

La recompilation de niveau instruction améliore les performances car, dans la plupart des cas, un nombre réduit d'instructions est à l'origine des recompilations et de leurs effets secondaires, en termes de temps processeur et de verrous. Par conséquent, ces effets épargnent les autres instructions du traitement qui n'ont pas besoin d'être recompilées.

L’événement étendu `sql_statement_recompile` (xEvent) signale les recompilations au niveau de l’instruction. Ce xEvent se produit quand une recompilation au niveau de l’instruction est requise par n’importe quel type de lot. Cela comprend les procédures stockées, les déclencheurs, les lots ad hoc et les requêtes. Les lots peuvent être envoyés par l’intermédiaire de plusieurs interfaces, notamment sp_executesql, SQL dynamique, ou des méthodes Prepare ou Execute.
La colonne `recompile_cause` de `sql_statement_recompile` xEvent contient un code entier qui indique la raison de la recompilation. Le tableau suivant contient les raisons possibles :

|||
|----|----|  
|Schéma modifié|Statistiques modifiées|  
|Compilation différée|Option SET modifiée|  
|Table temporaire modifiée|Ensemble de lignes à distance modifié|  
|Autorisation`FOR BROWSE` modifiée|Environnement de notification de requête modifié|  
|Vue partitionnée modifiée|Options de curseur modifiées|  
|`OPTION (RECOMPILE)` requis|Plan paramétré vidé|  
|Plan affectant la version de base de données modifié|Stratégie de forçage de plan de Magasin des requêtes modifiée|  
|Échec de forçage de plan de Magasin des requêtes|Plan manquant dans le Magasin des requêtes|

> [!NOTE]
> Dans les versions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] où les événements étendus ne sont pas disponibles, l’événement de trace [SP:Recompile](../relational-databases/event-classes/sp-recompile-event-class.md) du profileur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut être utilisé dans le même but de signaler les recompilations au niveau de l’instruction.
> L’événement de trace [SQL:stmtrecompile](../relational-databases/event-classes/sql-stmtrecompile-event-class.md) signale également les recompilations au niveau de l’instruction, et vous pouvez aussi l’utiliser pour suivre et déboguer les recompilations. Tandis que SP:Recompile est généré uniquement pour les procédures stockées et les déclencheurs, SQL:StmtRecompile est généré pour les procédures stockées, les déclencheurs, les lots ad hoc, les lots exécutés à l’aide de `sp_executesql`, les requêtes préparées et le code SQL dynamique.
> La colonne *EventSubClass* de SP:Recompile et de SQL:StmtRecompile contient un code d’entier qui indique le motif de la recompilation. Les codes sont décrits [ici](../relational-databases/event-classes/sql-stmtrecompile-event-class.md).

> [!NOTE]
> Quand l’option de base de données `AUTO_UPDATE_STATISTICS` a pour valeur `ON`, les requêtes sont recompilées quand elles ciblent des tables ou des vues indexées dont les statistiques ont été mises à jour ou dont les cardinalités ont sensiblement évolué depuis la dernière exécution. Ce comportement s’applique aux tables temporaires, aux tables définies par l’utilisateur standard, ainsi qu’aux tables inserted et deleted créées par des déclencheurs DML. Si les performances des requêtes sont affectées par des recompilations excessives, vous pouvez attribuer à ce paramètre la valeur `OFF`. Quand l’option de base de données `AUTO_UPDATE_STATISTICS` a pour valeur `OFF`, aucune recompilation ne se produit en fonction des statistiques ou des modifications de cardinalité, à l’exception des tables inserted et deleted qui sont créées par des déclencheurs DML `INSTEAD OF`. Comme ces tables sont créées dans tempdb, la recompilation de requêtes qui accèdent à ces tables dépend du paramétrage de `AUTO_UPDATE_STATISTICS` dans tempdb. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000, la recompilation des requêtes se poursuit en fonction des modifications de cardinalité apportées aux tables inserted et deleted créées par des déclencheurs DML, même si ce paramètre a pour valeur `OFF`.

### <a name="PlanReuse"></a> Réutilisation des paramètres et des plans d'exécution

L'utilisation de paramètres, notamment de marqueurs de paramètres dans les applications ADO, OLE DB et ODBC, peut favoriser la réutilisation des plans d'exécution. 

> [!WARNING] 
> L’utilisation de paramètres ou de marqueurs de paramètres pour la conservation des valeurs entrées par les utilisateurs finaux est plus fiable que la concaténation des valeurs dans une chaîne qui sera exécutée à l’aide de la méthode API d’accès aux données, à savoir l’instruction `EXECUTE` , ou de la procédure stockée `sp_executesql` .
 
La seule différence entre les deux instructions `SELECT` suivantes porte sur les valeurs comparées dans la clause `WHERE` :

```sql
SELECT * 
FROM AdventureWorks2014.Production.Product 
WHERE ProductSubcategoryID = 1;
```

```sql
SELECT * 
FROM AdventureWorks2014.Production.Product 
WHERE ProductSubcategoryID = 4;
```

La seule différence entre les plans d’exécution de ces requêtes est la valeur stockée pour la comparaison avec la colonne `ProductSubcategoryID` . Bien que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] soit conçu pour toujours reconnaître que les instructions génèrent essentiellement le même plan et réutilisent les plans, il peut arriver que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne le détecte pas dans les instructions SQL complexes.

La séparation des constantes de l'instruction SQL à l'aide de paramètres permet au moteur relationnel de reconnaître plus facilement les plans en double. Vous pouvez utiliser les paramètres des manières suivantes : 

* Dans Transact-SQL, utilisez `sp_executesql`: 

   ```sql
   DECLARE @MyIntParm INT
   SET @MyIntParm = 1
   EXEC sp_executesql
     N'SELECT * 
     FROM AdventureWorks2014.Production.Product 
     WHERE ProductSubcategoryID = @Parm',
     N'@Parm INT',
     @MyIntParm
   ```

   Cette méthode est recommandée pour les scripts Transact-SQL, les procédures stockées ou les déclencheurs SQL qui génèrent dynamiquement des instructions SQL. 

* ADO, OLE DB et ODBC utilisent des marqueurs de paramètres. Les marqueurs de paramètres sont des points d'interrogation (?) qui remplacent une constante dans une instruction SQL et qui sont liés à une variable de programme. Dans une application ODBC, vous pourriez par exemple procéder comme suit : 

   * Utilisez `SQLBindParameter` pour lier une variable de type entier au premier marqueur de paramètres dans une instruction SQL.
   * Placez la valeur de type entier dans la variable.
   * Exécutez l'instruction en spécifiant le marqueur de paramètres (?) : 

   ```
   SQLExecDirect(hstmt, 
     "SELECT * 
     FROM AdventureWorks2014.Production.Product 
     WHERE ProductSubcategoryID = ?",
     SQL_NTS);
   ```

   Le fournisseur OLE DB Native Client [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et le pilote ODBC Native Client [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fournis avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilisent `sp_executesql` pour envoyer des instructions à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] quand les marqueurs de paramètres sont utilisés dans les applications. 

* Pour concevoir des procédures stockées qui utilisent les paramètres par définition.

Si vous ne construisez pas explicitement des paramètres dans la conception de vos applications, vous pouvez toujours vous fier à l’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui paramètre automatiquement certaines requêtes à l’aide du comportement par défaut de paramétrage simple. Vous pouvez également forcer l’optimiseur de requête à paramétrer l’ensemble des requêtes de la base de données en attribuant la valeur `FORCED` à l’option `PARAMETERIZATION` de l’instruction `ALTER DATABASE`.

En cas d’activation du paramétrage forcé, il est toujours possible d’utiliser le paramétrage simple. Par exemple, la requête suivante ne peut être paramétrée conformément aux règles de paramétrage forcé :

```sql
SELECT * FROM Person.Address
WHERE AddressID = 1 + 2;
```

Elle peut toutefois être paramétrée conformément aux règles de paramétrage simple. En cas de tentative infructueuse de paramétrage forcé, le paramétrage simple est activé.

### <a name="SimpleParam"></a> Paramétrage simple

Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], l’utilisation de paramètres ou de marqueurs de paramètres dans les instructions Transact-SQL augmente la capacité du moteur relationnel à associer les nouvelles instructions SQL aux plans d’exécution préalablement compilés existants.

> [!WARNING] 
> L’utilisation de paramètres ou de marqueurs de paramètres pour la conservation des valeurs entrées par les utilisateurs finaux est plus fiable que la concaténation des valeurs dans une chaîne qui sera exécutée à l’aide de la méthode API d’accès aux données, à savoir l’instruction `EXECUTE` , ou de la procédure stockée `sp_executesql` .

Si vous exécutez une instruction SQL sans paramètres, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] paramètre cette instruction en interne afin d’augmenter la possibilité de l’associer à un plan d’exécution existant. Ce processus est appelé « paramétrage simple ». Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000, le processus était désigné par le terme « autoparamétrage ».

Imaginons l'instruction suivante :

```sql
SELECT * FROM AdventureWorks2014.Production.Product 
WHERE ProductSubcategoryID = 1;
```

Vous pouvez spécifier comme paramètre la valeur 1 de la fin de l'instruction. Le moteur relationnel génère le plan d'exécution pour ce lot comme si un paramètre avait été spécifié au lieu de la valeur 1. En raison de ce paramétrage simple, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] reconnaît que les deux instructions suivantes génèrent essentiellement le même plan d’exécution et réutilise le premier plan pour la deuxième instruction :

```sql
SELECT * FROM AdventureWorks2014.Production.Product 
WHERE ProductSubcategoryID = 1;
```
```sql
SELECT * FROM AdventureWorks2014.Production.Product 
WHERE ProductSubcategoryID = 4;
```

Lors du traitement d'instructions SQL complexes, il est possible que le moteur relationnel rencontre certaines difficultés pour déterminer les expressions qui peuvent être paramétrables. Afin d’augmenter la capacité du moteur relationnel à associer des instructions SQL complexes à des plans d’exécution existants et inutilisés, spécifiez les paramètres de manière explicite à l’aide de sp_executesql ou de marqueurs de paramètres. 

> [!NOTE]
> Quand les opérateurs arithmétiques +, -, \*, / ou % sont utilisés pour réaliser une conversion implicite ou explicite de valeurs constantes int, smallint, tinyint ou bigint pour les types de données float, real, decimal ou numeric, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] applique des règles spécifiques pour calculer le type et la précision des résultats des expressions. Toutefois, ces règles diffèrent selon que la requête est paramétrable ou non. Par conséquent, dans certains cas, des expressions similaires dans les requêtes peuvent produire des résultats différents.

Avec le comportement par défaut du paramétrage simple, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] paramètre une classe relativement réduite de requêtes. Toutefois, vous pouvez attribuer la valeur `PARAMETERIZATION` à l’option `ALTER DATABASE` de la commande `FORCED`pour spécifier que toutes les requêtes d’une base de données soient paramétrables, sous réserve de certaines contraintes. Cette opération peut améliorer les performances des bases de données soumises à des volumes élevés de requêtes simultanées en réduisant la fréquence des compilations de requête.

Une autre solution consiste à spécifier que ne soient paramétrables qu'une requête et toutes autres requêtes dont la syntaxe ne se différencie que par les valeurs des paramètres. 

### <a name="ForcedParam"></a> Paramétrage forcé

Vous pouvez remplacer le comportement de paramétrage simple par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en spécifiant que toutes les instructions `SELECT`, `INSERT`, `UPDATE` et `DELETE` de la base de données soient paramétrables dans certaines limites. Le paramétrage forcé s’active en attribuant la valeur `PARAMETERIZATION` à l’option `FORCED` dans l’instruction `ALTER DATABASE` . Ce type de paramétrage permet d'améliorer les performances de certaines bases de données en réduisant la fréquence des compilations et des recompilations des requêtes. Les bases de données qui peuvent tirer profit du paramétrage forcé sont généralement des bases de données devant gérer un nombre important de requêtes simultanées émanant de sources telles que des applications de point de vente.

Lorsque l’option `PARAMETERIZATION` a la valeur `FORCED`, toute valeur littérale apparaissant dans une instruction `SELECT`, `INSERT`, `UPDATE`ou `DELETE` , dans n’importe quel format, est convertie en paramètre au moment de la compilation de la requête. Les littéraux apparaissant dans les constructions de requêtes suivantes font toutefois exception : 

* Instructions`INSERT...EXECUTE` .
* Les instructions internes au corps de procédures stockées, de déclencheurs ou de fonctions définies par l’utilisateur. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] réutilise les plans de requête pour ces routines.
* Les instructions préparées ayant déjà été paramétrées dans l'application cliente.
* Les instructions contenant des appels de méthode XQuery, où la méthode apparaît dans un contexte nécessitant généralement que ses arguments soient paramétrés (clause `WHERE` , par exemple). Si la méthode figure dans un contexte où le paramétrage de ses arguments n'est pas requis, le reste de l'instruction est paramétré.
* Instructions à l’intérieur d’un curseur Transact-SQL. (Les instructions`SELECT` à l’intérieur des curseurs API sont paramétrables.)
* Constructions de requêtes déconseillées.
* Toute instruction exécutée dans le contexte de `ANSI_PADDING` ou `ANSI_NULLS` ayant la valeur `OFF`.
* Les instructions contenant plus de 2 097 littéraux pouvant être paramétrables.
* Les instructions faisant référence à des variables comme `WHERE T.col2 >= @bb`.
* Les instructions contenant l’indicateur de requête `RECOMPILE` .
* Les instructions contenant une clause `COMPUTE` .
* Les instructions contenant une clause `WHERE CURRENT OF` .

En outre, les clauses de requête suivantes ne sont pas paramétrables. Notez que dans de tels cas, seules les clauses ne sont pas paramétrables. D'autres clauses au sein de la même requête peuvent être l'objet d'un paramétrage forcé.

* <select_list> d’une instruction `SELECT`. Notamment les listes `SELECT` des sous-requêtes et les listes `SELECT` des instructions `INSERT`.
* Les instructions `SELECT` de sous-requêtes apparaissant dans une instruction `IF` .
* Les clauses `TOP`, `TABLESAMPLE`, `HAVING`, `GROUP BY`, `ORDER BY`, `OUTPUT...INTO`et `FOR XM`d’une requête.
* Les arguments, qu’il s’agisse d’un argument direct ou d’une sous-expression, des opérateurs `OPENROWSET`, `OPENQUERY`, `OPENDATASOURCE`, `OPENXML`ou `FULLTEXT` .
* Les arguments pattern et escape_character d’une clause `LIKE` .
* L’argument style d’une clause `CONVERT` .
* Les constantes entières d’une clause `IDENTITY` .
* Les constantes spécifiées à l'aide de la syntaxe d'extension ODBC.
* Les expressions de constantes pouvant être évaluées lors de la compilation et qui sont des arguments des opérateurs +, -, \*, / et %. Lors de l'examen d'un paramétrage forcé éventuel, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] considère qu'une expression est constituée de constantes pouvant être évaluées lors de la compilation lorsque l'une des conditions suivantes est vraie :  
  * l'expression ne contient pas de colonnes, de variables ou de sous-requêtes ;  
  * l’expression contient une clause `CASE` .  
* Les arguments des clauses d'indicateur de requête. Notamment l’argument `number_of_rows` de l’indicateur de requête `FAST` , l’argument `number_of_processors` de l’indicateur de requête `MAXDOP` et l’argument number de l’indicateur de requête `MAXRECURSION` .

Le paramétrage est effectué au niveau des instructions Transact-SQL individuelles. En d'autres termes, les instructions individuelles d'un traitement sont paramétrables. Une fois la compilation terminée, la requête paramétrable est exécutée dans le contexte du traitement pour lequel elle a été initialement soumise. Dans le cas d’un plan d’exécution mis en cache, vous pouvez déterminer si la requête a été paramétrée en référençant la colonne sql de la vue de gestion dynamique sys.syscacheobjects. Si la requête est paramétrable, les noms et les types de données des paramètres sont spécifiés avant le texte de chaque lot soumis dans cette colonne, par exemple (@1 tinyint).

> [!NOTE]
> Les noms des paramètres sont arbitraires. Les utilisateurs et les applications ne doivent par conséquent pas se fier à un ordre particulier d'affectation des noms. En outre, les éléments suivants peuvent varier dans les versions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et les mises à niveau des Service Packs : les noms des paramètres, le choix des littéraux paramétrés et l’espacement dans le texte paramétré.

#### <a name="data-types-of-parameters"></a>Types de données des paramètres

Lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] paramètre des littéraux, les paramètres sont convertis dans les types de données suivants :

* Les littéraux entiers dont la taille correspondrait en d’autres circonstances au type de données int sont paramétrés sur int. Les littéraux de taille plus importante qui font partie d’un prédicat impliquant un opérateur de comparaison quelconque (notamment <, \<=, =, !=, >, >=, , !\<, !>, <>, `ALL`, `ANY`, `SOME`, `BETWEEN` et `IN`) sont paramétrés sur numeric(38,0). Les littéraux de taille plus importante qui ne font pas partie d’un prédicat impliquant un opérateur de comparaison sont paramétrés sur numeric dont la précision suffit à prendre en charge leur taille et dont l’échelle correspond à 0.
* Les littéraux numériques à virgule fixe qui font partie d’un prédicat impliquant un opérateur de comparaison sont paramétrés sur numeric dont la précision est de 38 et dont l’échelle suffit à prendre en charge leur taille. Les littéraux numériques à virgule fixe qui ne font pas partie d’un prédicat impliquant un opérateur de comparaison sont paramétrés sur numeric dont la précision et l’échelle suffisent à prendre en charge leur taille.
* Les littéraux numériques à virgule flottante sont paramétrés sur float(53).
* Les littéraux de chaîne non-Unicode sont paramétrés sur varchar(8000) lorsqu’ils comptent moins de 8 000 caractères et sur varchar(max) lorsqu’ils en comptent plus de 8 000.
* Les littéraux de chaîne Unicode sont paramétrés sur nvarchar(4000) lorsqu’ils comptent moins de 4 000 caractères Unicode et sur nvarchar(max) lorsqu’ils en comptent plus de 4 000.
* Les littéraux binaires sont paramètres en varbinary(8000) si le littéral n’excède pas 8 000 octets. S’il dépasse 8 000 octets, il est converti en varbinary(max).
* Les littéraux de type monétaire sont paramétrés sur money.

#### <a name="ForcedParamGuide"></a> Principes d'utilisation du paramétrage forcé

Lorsque vous affectez à l’option `PARAMETERIZATION` la valeur FORCED, tenez compte des points suivants :

* Le paramétrage forcé convertit les constantes des littéraux d'une requête en paramètres lors de la compilation d'une requête. Par conséquent, l'optimiseur de requête peut opter pour des plans d'exécution de requêtes non optimisés. Plus spécifiquement, il est moins probable que l'optimiseur de requête établisse une correspondance avec une vue indexée ou un index d'une colonne calculée. Il peut également opter pour des plans non optimisés dans le cas de requêtes soumises pour des tables partitionnées ou des vues partitionnées et distribuées. Il n'est pas recommandé d'utiliser le paramétrage forcé dans des environnements reposant principalement sur des vues indexées ou des index de colonnes calculées. Dans l’ensemble, l’option `PARAMETERIZATION FORCED` devrait être utilisée exclusivement par des administrateurs de bases de données expérimentés qui se seront assurés que cela n’affectera pas les performances.
* Les requêtes distribuées qui font référence à plusieurs bases de données sont éligibles pour le paramétrage forcé pour autant que l’option `PARAMETERIZATION` ait la valeur `FORCED` dans la base de données dont le contexte fait l’objet de la requête.
* L’affectation de la valeur `PARAMETERIZATION` à l’option `FORCED` vide tous les plans de requêtes du cache de plans d’une base de données, à l’exception de ceux qui sont en cours de compilation, de recompilation ou d’exécution. Tout plan d'une requête en cours de compilation ou d'exécution lors de la modification des paramètres est paramétré lors de la prochaine exécution de la requête.
* La définition de l’option `PARAMETERIZATION` est une opération en ligne qui ne requiert aucun verrou exclusif au niveau de la base de données.
* Le paramètre actuel de l’option `PARAMETERIZATION` est conservé en cas de rattachement ou de restauration d’une base de données.

Pour remplacer le comportement de paramétrage forcé, il suffit de spécifier qu'un paramétrage simple soit tenté sur une requête unique, à l'instar des autres requêtes de syntaxe équivalente mais présentant des valeurs de paramètres différentes. À l'inverse, vous pouvez spécifier que le paramétrage forcé soit tenté sur seul jeu de requêtes de syntaxe équivalente, même en cas de désactivation de l'option de paramétrage forcé dans la base de données. Vous pouvez utiliser les[repères de plan](../relational-databases/performance/plan-guides.md) à cet effet.

> [!NOTE]
> Quand l’option `PARAMETERIZATION` est définie avec la valeur `FORCED`, le rapport des messages d’erreur peut ne pas être le même que quand l’option `PARAMETERIZATION` est définie avec la valeur `SIMPLE` : davantage de messages d’erreur peuvent être signalés avec le paramétrage forcé qu’avec le paramétrage simple et les numéros de ligne où interviennent les erreurs peuvent ne pas être corrects.

### <a name="preparing-sql-statements"></a>Préparation des instructions SQL

Le moteur relationnel de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permet la prise en charge intégrale de la préparation des instructions SQL avant leur exécution. Si une application doit exécuter une instruction SQL plusieurs fois, elle peut recourir à l'API de base de données pour effectuer les opérations suivantes : 

* Préparer l'instruction en une seule fois. L'instruction SQL est compilée dans un plan d'exécution.
* Exécuter le plan d'exécution précompilé chaque fois qu'elle doit exécuter l'instruction. Cela évite de recompiler l'instruction SQL après chaque exécution suivant la première.   
  La préparation et l'exécution des instructions sont contrôlées par les fonctions et les méthodes API. Ceci ne fait pas partie du langage Transact-SQL. Le modèle de préparation et d'exécution des instructions SQL est pris en charge par le fournisseur OLE DB Native Client [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et par le pilote ODBC Native Client [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Lors d’une demande de préparation, le fournisseur ou le pilote envoie l’instruction à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec une demande de préparation de l’instruction. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] compile un plan d’exécution et retourne un descripteur de ce plan au fournisseur ou au pilote. Pour toute requête d'exécution, le fournisseur ou le pilote envoie au serveur une requête d'exécution du plan associé au descripteur. 

Les instructions préparées ne peuvent pas être utilisées pour la création d'objets temporaires dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Elles ne peuvent pas faire référence à des procédures stockées du système qui créent des objets temporaires, tels que des tables temporaires. Ces procédures doivent être exécutées directement.

L'utilisation excessive du modèle de préparation et d'exécution peut nuire aux performances. Si une instruction n'est exécutée qu'une seule fois, son exécution directe ne requiert qu'un seul aller-retour au serveur. La préparation suivie de l'exécution unique d'une instruction SQL nécessite un aller-retour supplémentaire : un pour préparer l'instruction et un pour l'exécuter.

La préparation d'une instruction est plus efficace si vous utilisez les marqueurs de paramètres. Par exemple, supposons que vous demandez occasionnellement à une application d’extraire des informations sur un produit à partir de l’exemple de base de données `AdventureWorks` . Il existe deux moyens pour y arriver : 

Premièrement, l'application peut exécuter une requête différente pour chaque produit demandé :

```sql
SELECT * FROM AdventureWorks2014.Production.Product
WHERE ProductID = 63;
```

Deuxièmement, l'application peut procéder comme suit : 

1. Préparer une instruction contenant un marqueur de paramètres (?) :  
   ```sql
   SELECT * FROM AdventureWorks2014.Production.Product  
   WHERE ProductID = ?;
   ```
2. Lier une variable de programme au marqueur de paramètres.
3. Chaque fois que vous avez besoin d'informations sur un produit, l'application remplit la variable liée avec la valeur de la clé et exécute l'instruction.

La seconde méthode est plus efficace lorsque l'instruction est exécutée plus de trois fois.

Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le modèle de préparation et d’exécution présente très peu d’avantages en terme de performances par rapport à l’exécution directe en raison du mode de réutilisation des plans d’exécution par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] intègre des algorithmes efficaces associant les instructions SQL en cours aux plans d’exécution générés pour des exécutions antérieures de la même instruction SQL. Si une application exécute plusieurs fois une instruction SQL avec des marqueurs de paramètres, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] réutilisera le plan d’exécution de la première exécution pour les exécutions suivantes (sauf si le plan a été supprimé du cache du plan suite à son expiration). Le modèle de préparation et d'exécution offre encore d'autres avantages : 

* La recherche d'un plan d'exécution à l'aide d'un descripteur d'identification est plus efficace que les algorithmes utilisés pour associer une instruction SQL à un plan d'exécution existant.
* L'application peut contrôler le moment où le plan d'exécution est créé et réutilisé.
* Le modèle de préparation et d'exécution peut être transféré à d'autres bases de données, y compris des versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

 
### <a name="ParamSniffing"></a> Détection des paramètres
La « détection de paramètres » fait référence à un processus par lequel [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] « espionne » les valeurs de paramètres actuelles pendant la compilation ou la recompilation, et les transmet à l’optimiseur de requête afin qu’elles puissent servir à générer des plans d’exécution de requête potentiellement plus efficaces.

Les valeurs de paramètres sont détectées pendant la compilation ou la recompilation pour les types de lots suivants :

-  Procédures stockées
-  Requêtes soumises par l’intermédiaire de sp_executesql 
-  Requêtes préparées

> [!NOTE]
> Pour les requêtes utilisant l’indicateur `RECOMPILE`, les valeurs de paramètres et les valeurs actuelles des variables locales sont détectées. Les valeurs détectées (des paramètres et variables locales) sont celles présentes dans le lot juste avant l’instruction avec l’indicateur `RECOMPILE`. Pour les paramètres en particulier, les valeurs fournies avec l’appel du lot ne sont pas détectées.

## <a name="parallel-query-processing"></a>Traitement de requêtes en parallèle

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permet les requêtes parallèles afin d'optimiser leur exécution et les opérations d'index sur les ordinateurs dotés de plusieurs processeurs (ou unités centrales). Comme [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut exécuter une requête ou une opération d’index en parallèle à l’aide de plusieurs threads de travail du système d’exploitation, l’opération peut être exécutée rapidement et efficacement.

Durant l'optimisation, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] recherche les requêtes ou les opérations d'index qui pourraient tirer profit d'une exécution en parallèle. Pour ces requêtes, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] insère des opérateurs d'échange dans le plan d'exécution de la requête afin de la préparer à l'exécution en parallèle. Un opérateur d'échange est un opérateur dans un plan d'exécution de requêtes qui assure la gestion du processus, la redistribution des données et le contrôle de flux. L’opérateur d’échange inclut les opérateurs logiques `Distribute Streams`, `Repartition Streams`et `Gather Streams` comme sous-types, qui peuvent apparaître dans la sortie Showplan du plan de requête d’une requête parallèle. 

Une fois les opérateurs d'échange insérés, vous obtenez un plan d'exécution de requêtes en parallèle. Un plan d’exécution de requêtes en parallèle peut utiliser plusieurs threads de travail. En revanche, un plan d’exécution en série, qui porte sur une requête non parallèle, n’utilise qu’un seul thread de travail pour son exécution. Le nombre réel de threads de travail utilisés par une requête parallèle est déterminé au moment de l’initialisation de l’exécution du plan de requête et dépend de la complexité et du degré de parallélisme du plan. Le degré de parallélisme détermine le nombre maximal d’unités centrales utilisées ; il n’indique pas le nombre de threads de travail employés. La valeur du degré de parallélisme est définie au niveau du serveur et peut être modifiée par le biais de la procédure stockée système sp_configure. Cette valeur peut être remplacée pour les instructions de requête ou d’index individuelles en spécifiant l’indicateur de requête `MAXDOP` ou l’option d’index `MAXDOP` . 

L’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’utilise pas de plan d’exécution parallèle pour une requête si l’une des conditions suivantes est remplie :

* Le coût d'une exécution en série n'est pas assez élevé pour envisager à la place un plan d'exécution parallèle. 
* Un plan d'exécution en série est considéré comme plus rapide que n'importe quel plan d'exécution parallèle envisageable pour la requête particulière.
* La requête contient des opérateurs scalaires ou relationnels qui ne peuvent être exécutés en parallèle. Certains opérateurs peuvent entraîner l'exécution d'une section du plan de requête ou de la totalité du plan en mode série.

### <a name="DOP"></a> Degré de parallélisme

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] détecte automatiquement le meilleur degré de parallélisme pour chaque instance d'une exécution de requête en parallèle ou d'une opération DDL (Data Definition Language) d'index. Cette détection se fait sur la base des critères suivants : 

1. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fonctionne sur un ordinateur doté de plusieurs microprocesseurs ou UC, tel qu'un ordinateur à multitraitement symétrique (SMP, symmetric multiprocessing).  
  Seuls les ordinateurs dotés de plusieurs UC peuvent utiliser des requêtes en parallèle. 

2. Le nombre de threads de travail disponibles.  
  Chaque requête ou opération d’index nécessite un certain nombre de threads de travail. Pour être exécuté, un plan parallèle nécessite plus de threads de travail qu’un plan série, le nombre de threads de travail nécessaires allant de pair avec le degré de parallélisme. Quand les threads de travail disponibles sont insuffisants pour un certain degré de parallélisme, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] diminue automatiquement le degré de parallélisme ou abandonne complètement le plan parallèle dans le contexte de charge de travail spécifié. Ensuite, il exécute le plan série (un thread de travail). 

3. Le type de requête ou d'opération d'index exécutée.  
  Les requêtes qui utilisent fortement les cycles microprocesseur et les opérations d'index qui créent ou reconstruisent un index, ou qui suppriment un index cluster, sont les candidates idéales pour un plan parallèle. Par exemple, les jointures de grandes tables, les agrégations importantes et le tri d'ensembles de résultats volumineux s'y prêtent bien. Pour les requêtes simples, typiques des applications de traitement de transactions, il s'avère que la coordination supplémentaire nécessaire à l'exécution d'une requête en parallèle n'est pas rentabilisée par l'augmentation potentielle des performances. Pour faire la distinction entre les requêtes qui tirent profit du parallélisme et les autres, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] compare le coût estimé de l’exécution de la requête ou de l’opération d’index à la valeur [cost threshold for parallelism](../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md). L’utilisateur peut changer la valeur par défaut (5) à l’aide de [sp_configure](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) si un test approprié a révélé qu’une valeur différente est mieux adaptée pour la charge de travail en cours d’exécution. 

4. Le nombre de lignes à traiter.  
  Si l'optimiseur de requête détermine que le nombre de lignes est trop faible, il n'introduit pas les opérateurs d'échange qui servent à distribuer les lignes. Par conséquent, ces opérateurs sont exécutés en série. L'exécution des opérateurs dans un plan série permet d'éviter que les coûts de démarrage, de distribution et de coordination dépassent les bénéfices d'une exécution en parallèle.

5. Disponibilité des statistiques de distribution actuelles.  
  Si ce niveau ne peut être atteint, le moteur de base de données envisage de passer à un degré inférieur avant d'abandonner totalement le plan parallèle.  
  Par exemple, lorsque vous créez un index cluster sur une vue, les statistiques de distribution ne peuvent pas être prises en compte étant donné que l'index en question n'existe pas encore. Dans ce cas, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] est incapable de garantir le degré de parallélisme le plus élevé pour l'opération d'index. Toutefois, certains opérateurs, tels que le tri et l'analyse, peuvent malgré tout bénéficier de l'exécution en parallèle.

> [!NOTE]
> Les opérations d’index parallèles ne sont disponibles que dans les éditions Enterprise, Developer et Evaluation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].
 
Lors de l'exécution, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] détermine si la charge de travail actuelle du système et les informations de configuration décrites ci-dessus permettent une exécution parallèle. Si c’est le cas, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] détermine le nombre optimal de threads de travail et répartit l’exécution du plan parallèle parmi ces derniers. Quand une requête ou une opération d’index commence à s’exécuter en parallèle sur plusieurs threads de travail, un nombre identique de threads de travail est utilisé jusqu’à ce que l’opération soit terminée. Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] réexamine le nombre optimal de threads de travail chaque fois qu’un plan d’exécution est récupéré à partir du cache du plan. Par exemple, la première exécution d’une requête peut nécessiter l’utilisation d’un plan série, la deuxième d’un plan parallèle et de trois threads de travail, et la troisième exécution d’un plan parallèle et de quatre threads.

Dans un plan d'exécution parallèle de requêtes, les opérateurs insert, update et delete sont exécutés en série. Cependant, la clause WHERE d'une instruction UPDATE ou DELETE, ou la partie SELECT d'une instruction INSERT peut être exécutée en parallèle. Les modifications réelles des données sont ensuite appliquées en série à la base de données.

Les curseurs statiques et les curseurs pilotés par jeux de clés peuvent être complétés par des plans d'exécution parallèle. Cependant, le comportement des curseurs dynamiques ne peut être fourni que par une exécution en série. L'optimiseur de requête génère toujours un plan d'exécution en série pour une requête qui fait partie d'un curseur dynamique.

#### <a name="overriding-degrees-of-parallelism"></a>Remplacement des degrés de parallélisme

Vous pouvez utiliser l’option de configuration de serveur [max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) (MAXDOP) ([ALTER DATABASE SCOPED CONFIGURATION](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) sur [!INCLUDE[ssSDS_md](../includes/sssds-md.md)]) pour limiter le nombre de processeurs à utiliser dans une exécution de plan parallèle. L’option max degree of parallelism peut être remplacée par des instructions d’exécution de requêtes ou d’opérations d’index individuelles grâce à la spécification de l’indicateur MAXDOP ou de l’option d’index MAXDOP. MAXDOP offre un meilleur contrôle sur les requêtes et les opérations d'index individuelles. Par exemple, vous pouvez utiliser l’option MAXDOP pour contrôler (à savoir augmenter ou réduire) le nombre de processeurs alloués à une opération d’index en ligne. Ceci vous permet d'équilibrer les ressources utilisées par une opération d'index et celles des utilisateurs simultanés. 

L’attribution de la valeur 0 (valeur par défaut) à l’option Degré maximal de parallélisme permet à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] d’utiliser tous les processeurs disponibles, jusqu’à 64, dans une exécution de plan parallèle. Bien que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] définisse une cible d’exécution de 64 processeurs logiques quand l’option MAXDOP a la valeur 0, une autre valeur peut être définie manuellement si nécessaire. Attribuer la valeur 0 à MAXDOP pour les requêtes et les index permet à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] d’utiliser tous les processeurs disponibles, 64 au maximum, pour les requêtes ou les index donnés dans une exécution de plan parallèle. MAXDOP n’est pas une valeur appliquée pour toutes les requêtes parallèles, mais plutôt une cible provisoire pour toutes les requêtes éligibles pour le parallélisme. Cela signifie que si le nombre de threads de travail disponibles n’est pas suffisant au moment de l’exécution, une requête peut s’exécuter avec un degré de parallélisme inférieur à l’option MAXDOP.

Pour obtenir des recommandations sur la configuration de MAXDOP, consultez cet [Article du support technique Microsoft](http://support.microsoft.com/help/2806535/recommendations-and-guidelines-for-the-max-degree-of-parallelism-configuration-option-in-sql-server).

### <a name="parallel-query-example"></a>Exemple de requête en parallèle

La requête suivante compte le nombre de commandes passées dans le courant du trimestre débutant le 1er avril 2000, dont au moins un poste a été livré au client à une date postérieure à la date prévue. Cette requête affiche le nombre de ce type de commandes groupées par priorité de commande et triées en ordre de priorité croissant. 

Cet exemple utilise des noms de tables et de colonnes théoriques.

```sql
SELECT o_orderpriority, COUNT(*) AS Order_Count
FROM orders
WHERE o_orderdate >= '2000/04/01'
   AND o_orderdate < DATEADD (mm, 3, '2000/04/01')
   AND EXISTS
         (
          SELECT *
            FROM    lineitem
            WHERE l_orderkey = o_orderkey
               AND l_commitdate < l_receiptdate
         )
   GROUP BY o_orderpriority
   ORDER BY o_orderpriority
```

Supposons que les index suivants soient définis dans les tables `lineitem` et `orders` :

```sql
CREATE INDEX l_order_dates_idx 
   ON lineitem
      (l_orderkey, l_receiptdate, l_commitdate, l_shipdate)

CREATE UNIQUE INDEX o_datkeyopr_idx
   ON ORDERS
      (o_orderdate, o_orderkey, o_custkey, o_orderpriority)
```

Voici un plan d'exécution en parallèle possible, généré pour la requête illustrée précédemment :

```
|--Stream Aggregate(GROUP BY:([ORDERS].[o_orderpriority])
                  DEFINE:([Expr1005]=COUNT(*)))
    |--Parallelism(Gather Streams, ORDER BY:
                  ([ORDERS].[o_orderpriority] ASC))
         |--Stream Aggregate(GROUP BY:
                  ([ORDERS].[o_orderpriority])
                  DEFINE:([Expr1005]=Count(*)))
              |--Sort(ORDER BY:([ORDERS].[o_orderpriority] ASC))
                   |--Merge Join(Left Semi Join, MERGE:
                  ([ORDERS].[o_orderkey])=
                        ([LINEITEM].[l_orderkey]),
                  RESIDUAL:([ORDERS].[o_orderkey]=
                        [LINEITEM].[l_orderkey]))
                        |--Sort(ORDER BY:([ORDERS].[o_orderkey] ASC))
                        |    |--Parallelism(Repartition Streams,
                           PARTITION COLUMNS:
                           ([ORDERS].[o_orderkey]))
                        |         |--Index Seek(OBJECT:
                     ([tpcd1G].[dbo].[ORDERS].[O_DATKEYOPR_IDX]),
                     SEEK:([ORDERS].[o_orderdate] >=
                           Apr  1 2000 12:00AM AND
                           [ORDERS].[o_orderdate] <
                           Jul  1 2000 12:00AM) ORDERED)
                        |--Parallelism(Repartition Streams,
                     PARTITION COLUMNS:
                     ([LINEITEM].[l_orderkey]),
                     ORDER BY:([LINEITEM].[l_orderkey] ASC))
                             |--Filter(WHERE:
                           ([LINEITEM].[l_commitdate]<
                           [LINEITEM].[l_receiptdate]))
                                  |--Index Scan(OBJECT:
         ([tpcd1G].[dbo].[LINEITEM].[L_ORDER_DATES_IDX]), ORDERED)
```

L’illustration ci-après montre un plan de requête exécuté avec un degré de parallélisme de 4 et comprenant une jointure entre deux tables.

![plan en parallèle](../relational-databases/media/parallel-plan.gif)

Le plan en parallèle comprend trois opérateurs de parallélisme. L’opérateur Index Seek de l’index `o_datkey_ptr` et l’opérateur Index Scan de l’index `l_order_dates_idx` sont exécutés en parallèle, ce qui produit plusieurs flux exclusifs. Cela peut être déterminé à partir des opérateurs Parallelism les plus proches des opérateurs Index Scan et Index Seek, respectivement. Dans les deux cas, le type d'échange est repartitionné. En d'autres termes, les données sont tout simplement distribuées aux flux en produisant le même nombre de flux en sortie qu'en entrée. Ce nombre de flux est égal au degré de parallélisme.

L’opérateur de parallélisme qui se trouve au-dessus de l’opérateur `L_ORDERKEY` Index Scan repartitionne ses flux d’entrée en utilisant la valeur de `l_order_dates_idx` comme clé. De cette façon, toutes les valeurs identiques de `L_ORDERKEY` se retrouvent dans un même flux de sortie. Au même moment, les flux de sortie gèrent l’ordre de la colonne `L_ORDERKEY` afin qu’elle réponde aux conditions d’entrée de l’opérateur Merge Join.

L’opérateur de parallélisme qui se trouve au-dessus de l’opérateur Index Seek repartitionne ses flux d’entrée en utilisant la valeur de `O_ORDERKEY`. Étant donné que son entrée n’est pas triée dans les valeurs de la colonne `O_ORDERKEY` et qu’il s’agit de la colonne de jointure de l’opérateur `Merge Join`, l’opérateur Sort qui se trouve entre les opérateurs de parallélisme et Merge Join s’assure que l’entrée est triée pour l’opérateur `Merge Join` dans les colonnes de jointure. L’opérateur `Sort`, tout comme l’opérateur Merge Join, est exécuté en parallèle.

Le premier opérateur de parallélisme rassemble les résultats de plusieurs flux en un seul flux. Les agrégations partielles effectuées par l’opérateur Stream Aggregate situé sous l’opérateur de parallélisme sont ensuite accumulées dans une seule valeur `SUM` pour chaque valeur différente de `O_ORDERPRIORITY` dans l’opérateur Stream Aggregate qui se trouve au-dessus de l’opérateur de parallélisme. Étant donné que le plan comporte deux segments d’échange avec un degré de parallélisme de 4, il utilise huit threads de travail.

Pour plus d’informations sur les opérateurs utilisés dans cet exemple, consultez le [Guide de référence des opérateurs Showplan logiques et physiques](../relational-databases/showplan-logical-and-physical-operators-reference.md).

### <a name="parallel-index-operations"></a>Opérations d’index parallèles

Les plans de requête générés pour les opérations d’index qui créent ou régénèrent un index, ou suppriment un index cluster, autorisent les opérations multithread parallèles sur des ordinateurs dotés de plusieurs microprocesseurs.

> [!NOTE]
> Les opérations d’index parallèles sont uniquement disponibles dans Enterprise Edition, à compter de [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)].
 
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise les mêmes algorithmes pour déterminer le degré de parallélisme (le nombre total de threads de travail séparés à exécuter) des opérations d’index, comme il le fait pour les autres instructions. Le degré maximal de parallélisme pour une opération d’index est fonction de l’option de configuration de serveur [max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) . Vous pouvez remplacer la valeur de max degree of parallelism pour des opérations d’index particulières en définissant l’option d’index MAXDOP dans les instructions CREATE INDEX, ALTER INDEX, DROP INDEX et ALTER TABLE.

Quand le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] génère un plan d’exécution d’index, le nombre d’opérations parallèles est défini sur la valeur la plus faible parmi les suivantes : 

* Nombre d'UC dans l'ordinateur
* Nombre spécifié dans l’option de configuration de serveur max degree of parallelism
* Nombre de processeurs (UC) ne dépassant pas déjà un seuil de travail effectué pour les threads de travail de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Par exemple, sur un ordinateur doté de huit UC, mais où max degree of parallelism a la valeur 6, pas plus de six threads de travail parallèles sont générés pour une création d’index. Si cinq des UC de l’ordinateur ont déjà dépassé le seuil de travail [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] lors de la création d’un plan d’exécution d’index, ce dernier spécifie uniquement trois threads de travail parallèles.

Les phases principales d'une opération d'index parallèle sont les suivantes : 

* Un thread de travail de coordination analyse rapidement et de façon aléatoire la table pour évaluer la distribution des clés d’index. Le threadde travail de coordination établit les limites des clés qui créeront un nombre de plages de clés égal au degré d’opérations parallèles, où chaque plage de clés est évaluée pour couvrir des nombres de lignes similaires. Par exemple, si la table comporte quatre millions de lignes et que le degré de parallélisme est de 4, le thread de travail de coordination détermine les valeurs de clé qui délimitent quatre ensembles de lignes avec un million de lignes dans chaque ensemble. Si un nombre suffisant de plages de clés ne peut être établi pour l'utilisation de toutes les UC, le degré de parallélisme est réduit en conséquence.  
* Le thread de travail de coordination répartit un nombre de threads de travail égal au degré d’opérations parallèles, et attend que ces threads de travail terminent leur travail. Chaque thread de travail analyse la table de base en utilisant un filtre qui extrait uniquement les lignes avec des valeurs de clé situées dans la plage affectée au thread de travail. Chaque thread de travail construit une structure d’index pour les lignes contenues dans sa plage de clés. Dans le cas d’un index partitionné, chaque thread de travail génère un nombre spécifié de partitions. Les partitions ne sont pas partagées entre les threads de travail.  
* Une fois que tous les threads de travail parallèles ont été exécutés, le thread de travail de coordination connecte les sous-unités d’index dans un index unique. Cette phase s'applique uniquement aux opérations d'index hors ligne.

Les instructions `CREATE TABLE` ou `ALTER TABLE` individuelles peuvent avoir plusieurs contraintes imposant la création d’un index. Ces opérations de création d'index multiples sont exécutées en série, bien que chaque opération de création d'index individuelle puisse être une opération parallèle sur un ordinateur doté de plusieurs UC.

## <a name="distributed-query-architecture"></a>Architecture des requêtes distribuées

Microsoft [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge deux méthodes distinctes pour référencer des sources de données OLE DB hétérogènes dans les instructions Transact-SQL :

* Noms de serveurs liés  
  Les procédures stockées système `sp_addlinkedserver` et `sp_addlinkedsrvlogin` servent à donner un nom de serveur à une source de données OLE DB. Les objets inclus dans ces serveurs liés peuvent être référencés dans des instructions Transact-SQL en utilisant un nom en quatre parties. Par exemple, si le nom d’un serveur lié `DeptSQLSrvr` est défini par rapport à une autre instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], l’instruction suivante fait référence à une table de ce serveur : 
  
  ```sql
  SELECT JobTitle, HireDate 
  FROM DeptSQLSrvr.AdventureWorks2014.HumanResources.Employee;
  ```

   Le nom de serveur lié peut également être spécifié dans une instruction `OPENQUERY` afin d’ouvrir un ensemble de lignes à partir d’une source de données OLE DB. Cet ensemble de lignes peut ensuite être référencé en tant que table dans les instructions Transact-SQL. 

* Noms de connecteurs appropriés  
  Dans le cas de références rares à une source de données, la fonction `OPENROWSET` ou `OPENDATASOURCE` est spécifiée avec les informations nécessaires à la connexion au serveur lié. Il est donc possible de faire référence à l’ensemble de lignes comme à une table dans les instructions Transact-SQL : 
  
  ```sql
  SELECT *
  FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
        'c:\MSOffice\Access\Samples\Northwind.mdb';'Admin';'';
        Employees);
  ```

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise OLE DB pour la communication entre le moteur relationnel et le moteur de stockage. Le moteur relationnel décompose chaque instruction Transact-SQL en une série d’opérations sur des ensembles de lignes OLE DB simples ouverts par le moteur de stockage à partir des tables de base. En d'autres termes, le moteur relationnel peut également ouvrir des ensembles de lignes OLE DB simples dans n'importe quelle source de données OLE DB.  
![oledb_storage](../relational-databases/media/oledb-storage.gif)  
Le moteur relationnel utilise l'API OLE DB pour ouvrir les ensembles de lignes sur les serveurs liés, pour extraire les lignes et pour gérer les transactions.

Pour chaque source de données OLE DB à laquelle vous accédez en tant que serveur lié, un fournisseur OLE DB doit être présent sur le serveur exécutant [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L’ensemble d’opérations Transact-SQL qui peut être utilisé sur une source de données OLE DB spécifique dépend des capacités du fournisseur OLE DB.

Pour chaque instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], les membres du rôle serveur fixe `sysadmin` peuvent activer ou désactiver l’utilisation de noms de connecteurs ad hoc pour un fournisseur OLE DB à l’aide de la propriété [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] `DisallowAdhocAccess`. Si l'accès approprié est activé, tout utilisateur connecté à l'instance peut exécuter des instructions SQL contenant des noms de connecteurs appropriés, en faisant référence à n'importe quelle source de données sur le réseau accessible par le biais du fournisseur OLE DB. Afin de contrôler l’accès aux sources de données, les membres du rôle `sysadmin` peuvent désactiver l’accès ad hoc pour ce fournisseur OLE DB, limitant ainsi l’accès des utilisateurs aux sources de données référencées par les noms de serveurs liés définis par les administrateurs. Par défaut, ce type d'accès approprié est activé pour le fournisseur OLE DB pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], mais désactivé pour tous les autres fournisseurs OLE DB.

Les requêtes distribuées permettent aux utilisateurs d’accéder à une autre source de données (par exemple, des fichiers, des sources de données non relationnelles telles que Active Directory, etc.) par le biais du contexte de sécurité du compte Microsoft Windows sous lequel le service [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s’exécute. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] emprunte l’identité de la connexion appropriée aux connexions Windows ; cet emprunt ne peut cependant pas être effectué pour les connexions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Ceci permet d'autoriser éventuellement l'utilisateur d'une requête distribuée à accéder à une autre source de données pour laquelle il ne bénéficie pas de droits d'accès, mais où le compte sous lequel le service [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'exécute dispose des autorisations appropriées. Utilisez la procédure stockée `sp_addlinkedsrvlogin` afin de définir les connexions spécifiques autorisées à accéder au serveur lié correspondant. Ce type de contrôle n'est pas ouvert aux noms appropriés. Il est donc impératif d'activer un fournisseur OLE DB pour un accès approprié avec précaution.

Quand c’est possible, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] envoie les opérations relationnelles telles que les jointures, les restrictions, les projections, les tris ou les regroupements à la source de données OLE DB. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’analyse pas par défaut la table de base dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et n’assure pas les opérations relationnelles de son propre chef. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] interroge le fournisseur OLE DB afin de déterminer le niveau de grammaire SQL pris en charge et, en fonction de ces informations, envoie autant d’opérations relationnelles que possible au fournisseur. 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] spécifie un mécanisme permettant à un fournisseur OLE DB de renvoyer des statistiques indiquant comment les valeurs clés sont distribuées dans la source de données OLE DB. Cela permet à l’optimiseur de requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de mieux analyser le modèle de données dans la source de données par rapport aux exigences spécifiques à chaque instruction SQL, augmentant ainsi sa capacité à générer des plans d’exécution optimaux. 

## <a name="query-processing-enhancements-on-partitioned-tables-and-indexes"></a>Améliorations du traitement des requêtes sur les tables et les index partitionnés

[!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] a amélioré les performances du traitement des requêtes sur les tables partitionnées pour de nombreux plans parallèles, changé la façon dont les plans parallèles et en série sont représentés, et amélioré les informations de partitionnement fournies dans les plans d’exécution au moment de la compilation et au moment de l’exécution. Cette rubrique décrit ces améliorations, fournit des indications sur la façon d'interpréter les plans d'exécution de requêtes de tables et d'index partitionnés, et fournit des recommandations pour améliorer les performances des requêtes sur les objets partitionnés. 

> [!NOTE]
> Les tables et les index partitionnés sont uniquement pris en charge dans les éditions Enterprise, Developer et Evaluation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

### <a name="new-partition-aware-seek-operation"></a>Nouvelle opération de recherche sensible aux partitions

Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la représentation interne d’une table partitionnée est modifiée afin que la table apparaisse au processeur de requêtes sous forme d’un index multicolonne avec `PartitionID` comme colonne principale. `PartitionID` est une colonne calculée cachée utilisée en interne pour représenter l’ `ID` de la partition contenant une ligne spécifique. Supposons, par exemple, que la table T définie comme `T(a, b, c)`est partitionnée sur la colonne a et possède un index cluster sur la colonne b. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], cette table partitionnée est traitée en interne comme une table non partitionnée, avec le schéma `T(PartitionID, a, b, c)` et un index cluster sur la clé composite `(PartitionID, b)`. Cela permet à l’optimiseur de requête d’effectuer des opérations de recherche basées sur `PartitionID` sur n’importe quel table ou index partitionné. 

L'élimination de partition est maintenant réalisée dans cette opération de recherche.

In addition, the Query Optimizer is extended so that a seek or scan operation with one condition can be done on `PartitionID` (comme colonne principale logique) et éventuellement d'autres colonnes clés d'index, puis une recherche de second niveau, avec une condition différente, peut être réalisée sur une ou plusieurs colonnes supplémentaires, pour chaque valeur distincte répondant à la qualification de l'opération de recherche de premier niveau. Autrement dit, cette opération, appelée analyse par saut, permet à l’optimiseur de requête d’effectuer une opération de recherche ou d’analyse basée sur une condition pour déterminer à quelles partitions accéder et une opération de recherche d’index de second niveau au sein de cet opérateur pour retourner les lignes de ces partitions qui répondent à une condition différente. Examinez, par exemple, la requête suivante.

```sql
SELECT * FROM T WHERE a < 10 and b = 2;
```

Dans cet exemple, supposons que la table T définie comme `T(a, b, c)`est partitionnée sur la colonne a et possède un index cluster sur la colonne b. Les limites de partition pour la table T sont définies par la fonction de partition suivante :

```sql
CREATE PARTITION FUNCTION myRangePF1 (int) AS RANGE LEFT FOR VALUES (3, 7, 10);
```

Pour résoudre la requête, le processeur de requêtes effectue une opération de recherche de premier niveau pour rechercher chaque partition contenant des lignes répondant à la condition `T.a < 10`. Cela identifie les partitions à accéder. Dans chaque partition identifiée, le processeur effectue ensuite une recherche de second niveau dans l’index cluster sur la colonne b pour rechercher les lignes qui répondent aux conditions `T.b = 2` et `T.a < 10`. 

L'illustration suivante est une représentation logique de l'opération d'analyse par saut. Elle montre la table `T` avec des données dans les colonnes `a` et `b`. Les partitions sont numérotées de 1 à 4 et les limites de partition sont indiquées par des lignes verticales en pointillé. Une opération de recherche de premier niveau dans les partitions (non représentée dans l'illustration) a déterminé que les partitions 1, 2 et 3 répondent à la condition de recherche impliquée par le partitionnement défini pour la table et le prédicat sur la colonne `a`. À savoir, `T.a < 10`. Le chemin d'accès parcouru par la partie de la recherche de second niveau de l'opération d'analyse par saut est illustré par la ligne courbée. Fondamentalement, l'opération d'analyse par saut recherche dans chacune de ces partitions les lignes qui répondent à la condition `b = 2`. Le coût total de l'opération d'analyse par saut est le même que celui de trois recherches d'index séparées.   

![skip_scan](../relational-databases/media/skip-scan.gif)

### <a name="displaying-partitioning-information-in-query-execution-plans"></a>Affichage d'informations de partitionnement dans les plans d'exécution de requêtes

Vous pouvez examiner les plans d’exécution de requêtes sur les tables et les index partitionnés en utilisant les instructions `SET` Transact-SQL `SET SHOWPLAN_XML` ou `SET STATISTICS XML`, ou en utilisant la sortie du plan d’exécution graphique dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio. Par exemple, vous pouvez afficher le plan d’exécution de compilation en cliquant sur *Afficher le plan d’exécution estimé* dans la barre d’outils de l’éditeur de requête et le plan au moment de l’exécution en cliquant sur *Inclure le plan d’exécution réel*. 

À l'aide de ces outils, vous pouvez déterminer les informations suivantes :

* les opérations telles que `scans`, `seeks`, `inserts`, `updates`, `merges`et `deletes` qui accèdent aux tables ou aux index partitionnés ;
* les partitions auxquelles accède la requête (par exemple, le nombre total de partitions ayant fait l'objet d'un accès et les plages de partitions contiguës qui font l'objet d'un accès sont disponibles dans les plans au moment de l'exécution) ;
* lorsque l'opération d'analyse par saut est utilisée dans une opération de recherche ou d'analyse pour récupérer les données d'une ou de plusieurs partitions.

#### <a name="partition-information-enhancements"></a>Améliorations apportées aux informations de partition

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fournit des informations de partitionnement améliorées pour les plans d'exécution de compilation et au moment de l'exécution. Les plans d'exécution fournissent désormais les informations suivantes :

* Un attribut `Partitioned` facultatif qui indique qu’un opérateur, tel que `seek`, `scan`, `insert`, `update`, `merge`ou `delete`, est effectué sur une table partitionnée.  
* Un nouvel élément `SeekPredicateNew` avec un sous-élément `SeekKeys` qui inclut `PartitionID` comme la colonne clé d’index principale et des conditions de filtrage qui spécifient les recherches de plage sur `PartitionID`. La présence de deux sous-éléments `SeekKeys` indique qu’une opération d’analyse par saut sur `PartitionID` est utilisée.   
* Des informations de résumé qui indiquent le nombre total de partitions ayant fait l'objet d'un accès. Ces informations sont uniquement disponibles dans les plans au moment de l'exécution. 

Pour démontrer comment ces informations sont affichées dans la sortie du plan d’exécution graphique et dans la sortie du plan d’exécution de requêtes XML, considérez la requête suivante sur la table partitionnée `fact_sales`. Cette requête met à jour les données dans deux partitions. 

```sql
UPDATE fact_sales
SET quantity = quantity * 2
WHERE date_id BETWEEN 20080802 AND 20080902;
```

L’illustration suivante montre les propriétés de l’opérateur `Clustered Index Seek` dans le plan d’exécution de compilation pour cette requête. Pour examiner la définition de la table `fact_sales` et la définition de la partition, consultez la section « Exemple » dans cette rubrique.  

![clustered_index_seek](../relational-databases/media/clustered-index-seek.gif)

#### <a name="partitioned-attribute"></a>Attributs partitionnés

Lorsqu’un opérateur tel que `Index Seek` est exécuté sur une table ou un index partitionné, l’attribut `Partitioned` apparaît dans le plan de compilation et au moment de l’exécution et a pour valeur `True` (1). L'attribut ne s'affiche pas lorsqu'il a pour valeur `False` (0).

L’attribut `Partitioned` peut apparaître dans les opérateurs physiques et logiques suivants :  
* `Table Scan`  
* `Index Scan`  
* `Index Seek`  
* `Insert`  
* `Update`  
* `Delete`  
* `Merge`  

Comme indiqué dans l'illustration précédente, cet attribut est affiché dans les propriétés de l'opérateur dans lequel il est défini. Dans la sortie du plan d’exécution XML, cet attribut apparaît comme `Partitioned="1"` dans le nœud `RelOp` de l’opérateur dans lequel il est défini.

#### <a name="new-seek-predicate"></a>Nouveau prédicat de recherche

Dans la sortie du plan d’exécution XML, l’élément `SeekPredicateNew` apparaît dans l’opérateur dans lequel il est défini. Il peut contenir jusqu’à deux occurrences du sous-élément `SeekKeys` . Le premier élément `SeekKeys` spécifie l’opération de recherche de premier niveau au niveau de l’ID de partition de l’index logique. Autrement dit, cette recherche détermine les partitions qui doivent être faire l'objet d'un accès pour satisfaire aux conditions de la requête. Le deuxième élément `SeekKeys` spécifie la partie de la recherche de second niveau de l’opération d’analyse par saut qui se produit dans chaque partition identifiée dans la recherche de premier niveau. 

#### <a name="partition-summary-information"></a>Informations de résumé sur les partitions

Dans les plans au moment de l'exécution, les informations de résumé sur les partitions fournissent le nombre des partitions ayant fait l'objet d'un accès et l'identité des partitions ayant réellement fait l'objet d'un accès. Vous pouvez utiliser ces informations pour vérifier que la requête accède aux bonnes partitions et que toutes les autres partitions sont ignorées.

Les informations suivantes sont fournies : `Actual Partition Count`et `Partitions Accessed`. 

`Actual Partition Count` est le nombre total de partitions auxquelles la requête a accédé.

`Partitions Accessed`, dans la sortie du plan d’exécution XML, correspond aux informations de résumé sur les partitions qui apparaissent dans le nouvel élément `RuntimePartitionSummary` dans le nœud `RelOp` de l’opérateur dans lequel il est défini. L’exemple suivant affiche le contenu de l’élément `RuntimePartitionSummary` , indiquant que deux partitions au total font l’objet d’un accès (partitions 2 et 3).
```
<RunTimePartitionSummary>

    <PartitionsAccessed PartitionCount="2" >

        <PartitionRange Start="2" End="3" />

    </PartitionsAccessed>

</RunTimePartitionSummary>
```

#### <a name="displaying-partition-information-by-using-other-showplan-methods"></a>Affichage d'informations sur les partitions en utilisant d'autres méthodes de plan d'exécution de requêtes

Les méthodes de plan d’exécution de requêtes `SHOWPLAN_ALL`, `SHOWPLAN_TEXT`et `STATISTICS PROFILE` ne signalent pas les informations sur les partitions décrites dans cette rubrique, avec toutefois l’exception suivante. Dans le cadre du prédicat `SEEK` , les partitions devant faire l’objet d’un accès sont identifiées par un prédicat de plage sur la colonne calculée qui représente l’ID de partition. L’exemple suivant affiche le prédicat `SEEK` pour un opérateur `Clustered Index Seek` . Les partitions 2 et 3 font l'objet d'un accès et l'opérateur de recherche filtre les lignes qui répondent à la condition `date_id BETWEEN 20080802 AND 20080902`.
```
|--Clustered Index Seek(OBJECT:([db_sales_test].[dbo].[fact_sales].[ci]), 

        SEEK:([PtnId1000] >= (2) AND [PtnId1000] \<= (3) 

                AND [db_sales_test].[dbo].[fact_sales].[date_id] >= (20080802) 

                AND [db_sales_test].[dbo].[fact_sales].[date_id] <= (20080902)) 

                ORDERED FORWARD)
```

#### <a name="interpreting-execution-plans-for-partitioned-heaps"></a>Interprétation des plans d'exécution pour les segments de mémoire partitionnés

Un segment de mémoire partitionné est traité comme un index logique sur l’ID de partition. Une élimination de partition sur un segment de mémoire partitionné est représentée dans un plan d’exécution en tant qu’opérateur `Table Scan` avec un prédicat `SEEK` sur l’ID de partition. L’exemple suivant illustre les informations du plan d’exécution de requêtes fournies :
```
|-- Table Scan (OBJECT: ([db].[dbo].[T]), SEEK: ([PtnId1001]=[Expr1011]) ORDERED FORWARD)
```

#### <a name="interpreting-execution-plans-for-collocated-joins"></a>Interprétation des plans d'exécution pour les jointures en colocation

La colocation de jointure peut se produire lorsque deux tables sont partitionnées à l'aide de la même fonction de partitionnement ou d'une fonction de partitionnement équivalente et que les colonnes de partitionnement des deux côtés de la jointure sont spécifiées dans la condition de jointure de la requête. L'optimiseur de requête peut générer un plan où les partitions de chaque table avec des ID de partition identiques sont jointes séparément. Les jointures en colocation peuvent être plus rapides que les autres jointures car elles peuvent nécessiter moins de mémoire et occasionner un temps de traitement inférieur. L’optimiseur de requête choisit un plan en colocation ou non en fonction des estimations de coût.

Dans un plan en colocation, la jointure `Nested Loops` lit une ou plusieurs partitions de table ou d’index jointes en interne. Les nombres dans les opérateurs `Constant Scan` représentent les numéros de partition. 

Lorsque des plans parallèles pour des jointures en colocation sont générés pour des tables ou des index partitionnés, un opérateur Parallelism apparaît entre les opérateurs de jointure `Constant Scan` et `Nested Loops` . Dans ce cas, chacun des threads de travail du côté extérieur de la jointure lit et opère sur une partition distincte. 

L'illustration suivante montre un plan de requête parallèle pour une jointure en colocation.   
![colocated_join](../relational-databases/media/colocated-join.gif)


#### <a name="parallel-query-execution-strategy-for-partitioned-objects"></a>Stratégie d'exécution de requête parallèle pour les objets partitionnés

Le processeur de requêtes utilise une stratégie d'exécution parallèle pour les requêtes qui sélectionnent parmi des objets partitionnés. Dans le cadre de la stratégie d’exécution, le processeur de requêtes détermine les partitions de table requises pour la requête et la proportion de threads de travail à allouer à chaque partition. Dans la plupart des cas, le processeur de requêtes alloue un nombre de threads de travail égal ou presque égal à chaque partition, puis il exécute la requête en parallèle sur les partitions. Les paragraphes suivants expliquent l’allocation des threads de travail de manière détaillée.  

![worker thread1](../relational-databases/media/thread1.gif)

Si le nombre de threads de travail est inférieur au nombre de partitions, le processeur de requêtes affecte chaque thread de travail à une partition différente, laissant initialement une ou plusieurs partitions sans thread de travail affecté. Quand un thread de travail a fini de s’exécuter sur une partition, le processeur de requêtes l’assigne à la partition suivante jusqu’à ce qu’un seul thread de travail ait été assigné à chaque partition. Il s’agit du seul cas dans lequel le processeur de requêtes réaffecte des threads de travail à d’autres partitions.  
Montre le thread de travail réaffecté une fois terminé. Si le nombre de threads de travail est égal au nombre de partitions, le processeur de requêtes assigne un thread de travail à chaque partition. Quand un thread de travail se termine, il n’est pas réaffecté à une autre partition.  

![worker thread2](../relational-databases/media/thread2.gif)  

Si le nombre de threads de travail est supérieur au nombre de partitions, le processeur de requêtes alloue une quantité égale de threads de travail à chaque partition. Si le nombre de threads de travail n’est pas un multiple exact du nombre de partitions, le processeur de requêtes alloue un thread de travail supplémentaire à certaines partitions afin d’utiliser tous les threads de travail disponibles. Notez que s’il n’y a qu’une seule partition, tous les threads de travail sont assignés à cette partition. Dans le schéma ci-dessous, il y a quatre partitions et 14 threads de travail. Trois threads de travail sont assignés à chaque partition et deux partitions ont un thread de travail supplémentaire, pour un total de 14 affectations de threads de travail. Quand un thread de travail se termine, il n’est pas réassigné à une autre partition.  

![worker thread3](../relational-databases/media/thread3.gif)  

Bien que les exemples ci-dessus suggèrent une méthode simple pour allouer des threads de travail, la stratégie réelle est plus complexe et prend en considération d’autres variables durant l’exécution de la requête. Par exemple, si la table est partitionnée et a un index cluster sur la colonne A et qu’une requête a la clause de prédicat `WHERE A IN (13, 17, 25)`, le processeur de requêtes alloue un ou plusieurs threads de travail à chacune de ces trois valeurs de recherche (A=13, A=17 et A=25) au lieu de chaque partition de table. Il est nécessaire d’exécuter la requête uniquement dans les partitions qui contiennent ces valeurs, et si tous ces prédicats de recherche se trouvent être dans la même partition de table, tous les threads de travail sont assignés à la même partition de table.

Pour prendre un autre exemple, supposons que la table a quatre partitions sur la colonne A avec des points de limite (10, 20, 30), un index sur la colonne B, et que la requête a une clause de prédicat `WHERE B IN (50, 100, 150)`. Les partitions de table étant basées sur les valeurs de A, les valeurs de B peuvent se produire dans chacune des partitions de table. Par conséquent, le processeur de requêtes recherche chacune des trois valeurs de B (50, 100, 150) dans chacune des quatre partitions de table. Le processeur de requêtes assignera des threads de travail proportionnellement afin de pouvoir exécuter chacune de ces 12 analyses de requête en parallèle.

|Partitions de table basées sur la colonne A |Recherches pour la colonne B dans chaque partition de table |
|----|----|
|Partition de table 1 : A < 10   |B=50, B=100, B=150 |
|Partition de table 2 : A >= 10 AND A < 20   |B=50, B=100, B=150 |
|Partition de table 3 : A >= 20 AND A < 30   |B=50, B=100, B=150 |
|Partition de table 4 : A >= 30  |B=50, B=100, B=150 |

### <a name="best-practices"></a>Bonnes pratiques

Pour améliorer les performances des requêtes qui accèdent à une grande quantité de données à partir de tables et d'index partitionnés volumineux, nous vous recommandons d'appliquer les méthodes conseillées suivantes :

* Agrégez par bandes chaque partition sur plusieurs disques. Cette opération est particulièrement appropriée quand vous utilisez des disques de rotation.
* Dans la mesure du possible, utilisez un serveur avec suffisamment de mémoire principale pour prendre en charge les partitions faisant l'objet d'un accès fréquent ou toutes les partitions afin de réduire le coût des E/S.
* Si les données que vous interrogez ne tiennent pas en mémoire, compressez les tables et les index. Cela réduira le coût des E/S.
* Utilisez un serveur avec des processeurs rapides et autant de noyaux de processeur que^possible selon vos moyens pour tirer parti des capacités de traitement de requête parallèle.
* Assurez-vous que le serveur possède une bande passante de contrôleur d'E/S suffisante. 
* Créez un index cluster sur chaque grande table partitionnée pour tirer parti des optimisations d'analyse d'arbre B (B-tree).
* Appliquez les recommandations mentionnées dans le livre blanc « [The Data Loading Performance Guide](http://msdn.microsoft.com/en-us/library/dd425070.aspx)» lors du chargement en masse des données dans des tables partitionnées.

### <a name="example"></a> Exemple

L'exemple suivant crée une base de données de test contenant une table unique avec sept partitions. Utilisez les outils décrits précédemment lors de l'exécution des requêtes dans cet exemple pour afficher des informations de partitionnement pour le plan de compilation et le plan au moment de l'exécution. 

> [!NOTE]
> Cet exemple insère plus d'un million de lignes dans la table. En fonction de votre matériel, l'exécution de cet exemple peut prendre plusieurs minutes. Avant d'exécuter cet exemple, vérifiez que l'espace disque dont vous disposez est supérieur à 1,5 Go. 
 
```sql
USE master;
GO
IF DB_ID (N'db_sales_test') IS NOT NULL
    DROP DATABASE db_sales_test;
GO
CREATE DATABASE db_sales_test;
GO
USE db_sales_test;
GO
CREATE PARTITION FUNCTION [pf_range_fact](int) AS RANGE RIGHT FOR VALUES 
(20080801, 20080901, 20081001, 20081101, 20081201, 20090101);
GO
CREATE PARTITION SCHEME [ps_fact_sales] AS PARTITION [pf_range_fact] 
ALL TO ([PRIMARY]);
GO
CREATE TABLE fact_sales(date_id int, product_id int, store_id int, 
    quantity int, unit_price numeric(7,2), other_data char(1000))
ON ps_fact_sales(date_id);
GO
CREATE CLUSTERED INDEX ci ON fact_sales(date_id);
GO
PRINT 'Loading...';
SET NOCOUNT ON;
DECLARE @i int;
SET @i = 1;
WHILE (@i<1000000)
BEGIN
    INSERT INTO fact_sales VALUES(20080800 + (@i%30) + 1, @i%10000, @i%200, RAND() * 25, (@i%3) + 1, '');
    SET @i += 1;
END;
GO
DECLARE @i int;
SET @i = 1;
WHILE (@i<10000)
BEGIN
    INSERT INTO fact_sales VALUES(20080900 + (@i%30) + 1, @i%10000, @i%200, RAND() * 25, (@i%3) + 1, '');
    SET @i += 1;
END;
PRINT 'Done.';
GO
-- Two-partition query.
SET STATISTICS XML ON;
GO
SELECT date_id, SUM(quantity*unit_price) AS total_price
FROM fact_sales
WHERE date_id BETWEEN 20080802 AND 20080902
GROUP BY date_id ;
GO
SET STATISTICS XML OFF;
GO
-- Single-partition query.
SET STATISTICS XML ON;
GO
SELECT date_id, SUM(quantity*unit_price) AS total_price
FROM fact_sales
WHERE date_id BETWEEN 20080801 AND 20080831
GROUP BY date_id;
GO
SET STATISTICS XML OFF;
GO
```

##  <a name="Additional_Reading"></a> Lecture supplémentaire  
 [Guide de référence des opérateurs Showplan logiques et physiques](../relational-databases/showplan-logical-and-physical-operators-reference.md)  
 [Événements étendus](../relational-databases/extended-events/extended-events.md)  
 [Bonnes pratiques relatives au Magasin des requêtes](../relational-databases/performance/best-practice-with-the-query-store.md)  
 [Estimation de la cardinalité](../relational-databases/performance/cardinality-estimation-sql-server.md)  
 [Traitement des requêtes adaptatives](../relational-databases/performance/adaptive-query-processing.md)   
 [Priorité des opérateurs](../t-sql/language-elements/operator-precedence-transact-sql.md)
