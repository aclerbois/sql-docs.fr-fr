---
title: Indexer des données JSON | Microsoft Docs
ms.custom: ''
ms.date: 06/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: json
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- dbe-json
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- JSON, indexing JSON data
- indexing JSON data
ms.assetid: ced241e1-ff09-4d6e-9f04-a594a9d2f25e
caps.latest.revision: 9
author: jovanpop-msft
ms.author: jovanpop
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 22a22adda08dffc2b0afeadd2f672f034b958826
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="index-json-data"></a>Indexer des données JSON
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Dans SQL Server et SQL Database, JSON n’est pas un type de données intégré et SQL Server n’a pas d’index JSON personnalisés. Toutefois, vous pouvez optimiser vos requêtes sur des documents JSON à l’aide d’index standard. 

Les index de base de données permettent d’améliorer les performances des opérations de filtrage et de tri. Sans index, SQL Server doit effectuer une analyse de table complète chaque fois que vous interrogez des données.  
  
## <a name="index-json-properties-by-using-computed-columns"></a>Indexer les propriétés JSON à l’aide des colonnes calculées  
Quand vous stockez des données JSON dans SQL Server, cela signifie généralement que vous souhaitez filtrer ou trier les résultats de la requête en fonction d’une ou de plusieurs *propriétés* des documents JSON.  

### <a name="example"></a> Exemple 
Dans cet exemple, supposons que la table `SalesOrderHeader` d’AdventureWorks comporte une colonne `Info` contenant plusieurs informations au format JSON sur des commandes client. Par exemple, elle contient des informations sur le client, le vendeur, les adresses de livraison et de facturation, etc. Vous souhaitez utiliser les valeurs de la colonne `Info` pour filtrer les commandes d’un client particulier.

### <a name="query-to-optimize"></a>Requête à optimiser
Voici un exemple de type de requête à optimiser à l’aide d’un index.  
  
```sql  
SELECT SalesOrderNumber,
    OrderDate,
    JSON_VALUE(Info, '$.Customer.Name') AS CustomerName
FROM Sales.SalesOrderHeader
WHERE JSON_VALUE(Info, '$.Customer.Name') = N'Aaron Campbell' 
```  

### <a name="example-index"></a>Exemple d’index
Pour accélérer vos filtres ou vos clauses `ORDER BY` sur une propriété d’un document JSON, vous pouvez utiliser les mêmes index que ceux que vous utilisez déjà sur d’autres colonnes. Toutefois, vous ne pouvez pas référencer *directement* des propriétés dans les documents JSON.
    
1.  Vous devez d’abord créer une « colonne virtuelle » qui retourne les valeurs à utiliser pour le filtrage.
2.  Vous devez ensuite créer un index sur cette colonne virtuelle.  
  
L’exemple suivant crée une colonne calculée qui peut être utilisée pour l’indexation. Il crée ensuite un index sur la nouvelle colonne calculée. Cet exemple crée une colonne qui affiche le nom du client, stocké dans le chemin `$.Customer.Name` des données JSON. 
  
```sql  
ALTER TABLE Sales.SalesOrderHeader
ADD vCustomerName AS JSON_VALUE(Info,'$.Customer.Name')

CREATE INDEX idx_soh_json_CustomerName
ON Sales.SalesOrderHeader(vCustomerName)  
```  
### <a name="more-info-about-the-computed-column"></a>Plus d’informations sur la colonne calculée 
La colonne calculée n’est pas persistante. Elle est calculée uniquement lorsque l’index doit être recréé. Elle n’occupe pas d’espace supplémentaire dans la table.   
  
Il est important de créer la colonne calculée avec la même expression que celle que vous comptez utiliser dans vos requêtes. Ici, par exemple, il s’agit de l’expression `JSON_VALUE(Info, '$.Customer.Name')`.  
  
Vous n’avez pas besoin de réécrire vos requêtes. Si vous utilisez des expressions avec la fonction `JSON_VALUE`, comme indiqué dans l’exemple de requête précédent, SQL Server détecte qu’il existe une colonne calculée équivalente avec la même expression et applique un index si possible.

### <a name="execution-plan-for-this-example"></a>Plan d’exécution pour cet exemple
Voici le plan d’exécution de la requête utilisée dans cet exemple.  
  
![Plan d’exécution](../../relational-databases/json/media/jsonindexblog1.png "Plan d’exécution")  
  
Au lieu d’une analyse de table complète, SQL Server utilise une recherche d’index dans l’index non cluster et recherche les lignes qui répondent aux conditions spécifiées. Il utilise ensuite une recherche de clé dans la table `SalesOrderHeader` pour extraire les autres colonnes référencées dans la requête. Dans cet exemple, il s’agit de `SalesOrderNumber` et `OrderDate`.  
 
### <a name="optimize-the-index-further-with-included-columns"></a>Optimiser davantage l’index avec les colonnes incluses
Si vous ajoutez les colonnes nécessaires dans l’index, vous pouvez éviter cette recherche supplémentaire dans la table. Vous pouvez ajouter ces colonnes en tant que colonnes incluses standard, comme indiqué dans l’exemple suivant, ce qui étend l’exemple `CREATE INDEX` précédent.  
  
```sql  
CREATE INDEX idx_soh_json_CustomerName
ON Sales.SalesOrderHeader(vCustomerName)
INCLUDE(SalesOrderNumber,OrderDate)
```  
  
Dans ce cas, SQL Server n’a pas à lire les données supplémentaires de la table `SalesOrderHeader`, car tout ce dont il a besoin est inclus dans l’index JSON non cluster. Ce type d’index est un bon moyen de combiner des données JSON et des données de colonne dans les requêtes, et de créer des index optimaux pour votre charge de travail.  
  
## <a name="json-indexes-are-collation-aware-indexes"></a>Les index JSON prennent en charge le classement  
Les index des données JSON présentent une caractéristique importante : ils prennent en charge le classement. Le résultat de la fonction `JSON_VALUE` que vous utilisez quand vous créez la colonne calculée est une valeur texte qui hérite son classement de l’expression d’entrée. Ainsi, les valeurs de l’index sont triées à l’aide des règles de classement définies dans les colonnes sources.  
  
Pour démontrer que les index prennent en charge le classement, l’exemple suivant crée une table de collection simple avec une clé primaire et un contenu JSON.  
  
```sql  
CREATE TABLE JsonCollection
 (
  id INT IDENTITY CONSTRAINT PK_JSON_ID PRIMARY KEY,
  json NVARCHAR(MAX) COLLATE SERBIAN_CYRILLIC_100_CI_AI
  CONSTRAINT [Content should be formatted as JSON]
  CHECK(ISJSON(json)>0)
 ) 
```  
  
La commande précédente spécifie le classement serbe cyrillique pour la colonne JSON. L’exemple suivant remplit la table et crée un index sur la propriété de nom.  
  
```sql  
INSERT INTO JsonCollection
VALUES
(N'{"name":"Иво","surname":"Андрић"}'),
(N'{"name":"Андрија","surname":"Герић"}'),
(N'{"name":"Владе","surname":"Дивац"}'),
(N'{"name":"Новак","surname":"Ђоковић"}'),
(N'{"name":"Предраг","surname":"Стојаковић"}'),
(N'{"name":"Михајло","surname":"Пупин"}'),
(N'{"name":"Борислав","surname":"Станковић"}'),
(N'{"name":"Владимир","surname":"Грбић"}'),
(N'{"name":"Жарко","surname":"Паспаљ"}'),
(N'{"name":"Дејан","surname":"Бодирога"}'),
(N'{"name":"Ђорђе","surname":"Вајферт"}'),
(N'{"name":"Горан","surname":"Бреговић"}'),
(N'{"name":"Милутин","surname":"Миланковић"}'),
(N'{"name":"Никола","surname":"Тесла"}')
GO
  
ALTER TABLE JsonCollection
ADD vName AS JSON_VALUE(json,'$.name')

CREATE INDEX idx_name
ON JsonCollection(vName)
```  
  
Les commandes précédentes créent un index standard sur la colonne calculée `vName`, qui représente la valeur de la propriété JSON `$.name`. Dans la page de codes cyrilliques serbes, l’ordre des lettres est « А », « Б », « В », « Г », « Д », « Ђ », « Е », etc. L’ordre des éléments de l’index est conforme aux règles cyrilliques serbes, car le résultat de la fonction `JSON_VALUE` hérite son classement de la colonne source. L’exemple suivant interroge cette collection et trie les résultats par nom.  
  
```sql  
SELECT JSON_VALUE(json,'$.name'),*
FROM JsonCollection
ORDER BY JSON_VALUE(json,'$.name')
```  
  
 Si vous examinez le plan d’exécution, vous pouvez constater qu’il utilise les valeurs triées de l’index non cluster.  
  
 ![Plan d’exécution](../../relational-databases/json/media/jsonindexblog2.png "Plan d’exécution")  
  
 Bien que la requête comporte une clause `ORDER BY`, le plan d’exécution n’utilise pas d’opérateur de tri. L’index JSON est déjà trié selon les règles cyrilliques serbes. Par conséquent, SQL Server peut utiliser l’index non cluster dans lequel les résultats sont déjà triés.  
  
 Toutefois, si vous changez le classement de l’expression `ORDER BY`, par exemple en ajoutant `COLLATE French_100_CI_AS_SC` après la fonction `JSON_VALUE`, vous obtenez un autre plan d’exécution de requête.  
  
 ![Plan d’exécution](../../relational-databases/json/media/jsonindexblog3.png "Plan d’exécution")  
  
 Étant donné que l’ordre des valeurs de l’index n’est pas compatible avec les règles de classement françaises, SQL Server ne peut pas utiliser l’index pour trier les résultats. Par conséquent, il ajoute un opérateur de tri qui trie les résultats à l’aide des règles de classement françaises.  
 
## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>En savoir plus sur JSON dans SQL Server et Azure SQL Database  
  
### <a name="microsoft-blog-posts"></a>Billets de blog Microsoft  
  
Pour accéder à un grand nombre de solutions spécifiques, de cas d’usage et de recommandations, consultez ces [billets de blog](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/) sur la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database.  

### <a name="microsoft-videos"></a>Vidéos Microsoft

Pour obtenir une présentation visuelle de la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database, consultez les vidéos suivantes :

-   [SQL Server 2016 et prise en charge de JSON](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Utilisation de JSON dans SQL Server 2016 et Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON : un pont entre les mondes relationnel et NoSQL](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
