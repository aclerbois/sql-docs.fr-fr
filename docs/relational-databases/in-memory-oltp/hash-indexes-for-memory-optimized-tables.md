---
title: Résolution des problèmes des index de hachage pour les tables à mémoire optimisée | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.component: in-memory-oltp
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e922cc3a-3d6e-453b-8d32-f4b176e98488
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: caff40911c06bcf57ae1eb39f042de12b8157405
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="troubleshooting-hash-indexes-for-memory-optimized-tables"></a>Résolution des problèmes des index de hachage pour les tables à mémoire optimisée
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

## <a name="prerequisite"></a>Condition préalable  
  
Des informations de contexte importantes pour comprendre cet article sont disponibles dans l’article :  
  
- [Index pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)
- [Index de hachage pour les tables mémoire optimisées](../../relational-databases/sql-server-index-design-guide.md#hash_index) 
 
## <a name="practical-numbers"></a>Nombres pratiques  
  
Lors de la création d’un index de hachage pour une table à mémoire optimisée, le nombre de compartiments doit être spécifié au moment de la création. Dans la plupart des cas, le nombre de compartiments doit être compris entre 1 et 2 fois le nombre de valeurs distinctes dans la clé d’index. 

Toutefois, même si la valeur **BUCKET_COUNT** se situe modérément en dessous ou au-dessus de la plage par défaut, les performances de l’index de hachage sont susceptibles d’être tolérables ou acceptables. Pensez au moins à affecter à l’index de hachage une valeur **BUCKET_COUNT** à peu près égale au nombre de lignes que vous pensez que votre table optimisée en mémoire finira par atteindre.  
Supposons que votre table en développement comprend 2 000 000 de lignes, mais vous prévoyez que cette quantité sera multipliée par 10 pour atteindre 20 000 000 de lignes. Démarrez avec un nombre de compartiments qui représente 10 fois le nombre de lignes dans la table. Vous avez ainsi la place pour une quantité accrue de lignes.  
  
- Dans l’idéal, vous augmentez le nombre de compartiments quand la quantité de lignes atteint le nombre de compartiments initial.  
- Même si la quantité de lignes atteint un nombre 5 fois plus élevé que le nombre de compartiments, les performances restent satisfaisantes dans la plupart des cas.  
  
Supposons qu’un index de hachage possède 10 000 000 de valeurs de clés distinctes.  
  
- Un nombre de compartiments de 2 000 000 représente le seuil minimal que vous pouvez accepter. Le degré de détérioration des performances peut être tolérable.  
  
## <a name="too-many-duplicate-values-in-the-index"></a>Trop de valeurs en double dans l’index ?  
  
Si les valeurs indexées de hachage ont un taux élevé de doublons, les compartiments de hachage ont des chaînes plus longues.  
  
Supposons que vous utilisez la même table SupportEvent que pour le bloc de code de la syntaxe T-SQL plus haut. Le code T-SQL suivant montre comment vous pouvez rechercher et afficher le rapport entre *toutes* les valeurs et les valeurs *uniques* :  
  
```sql
-- Calculate ratio of:  Rows / Unique_Values.  
DECLARE @allValues float(8) = 0.0, @uniqueVals float(8) = 0.0;  
  
SELECT @allValues = Count(*) FROM SupportEvent;  
  
SELECT @uniqueVals = Count(*) FROM  
  (SELECT DISTINCT SupportEngineerName  
      FROM SupportEvent) as d;  
  
    -- If (All / Unique) >= 10.0, use a nonclustered index, not a hash.   
SELECT Cast((@allValues / @uniqueVals) as float) as [All_divby_Unique];  
go  
```
  
- Un résultat de rapport supérieur ou égal à 10,0 signifie qu’un hachage constitue un type d’index médiocre. Envisagez plutôt d’utiliser un index non-cluster.   
  
## <a name="troubleshooting-hash-index-bucket-count"></a>Résolution de problèmes liés au nombre de compartiments d’index de hachage  
  
Cette section décrit comment résoudre les problèmes liés au nombre de compartiments pour votre index de hachage.  
  
### <a name="monitor-statistics-for-chains-and-empty-buckets"></a>Analyser les statistiques relatives aux chaînes et compartiments vides  
  
Vous pouvez analyser les statistiques de l’intégrité de vos index de hachage en exécutant l’instruction T-SQL SELECT suivante. L’instruction SELECT utilise la vue de gestion de données nommée **sys.dm_db_xtp_hash_index_stats**.  
  
```sql
SELECT  
  QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [table],   
  i.name                   as [index],   
  h.total_bucket_count,  
  h.empty_bucket_count,  
    
  FLOOR((  
    CAST(h.empty_bucket_count as float) /  
      h.total_bucket_count) * 100)  
                            as [empty_bucket_percent],  
  h.avg_chain_length,   
  h.max_chain_length  
FROM  
        sys.dm_db_xtp_hash_index_stats  as h   
  JOIN sys.indexes                     as i  
          ON h.object_id = i.object_id  
          AND h.index_id  = i.index_id  
JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
JOIN sys.tables t on h.object_id=t.object_id
WHERE ia.type=1
ORDER BY [table], [index];  
```
  
Comparez les résultats de l’instruction SELECT aux règles statistiques suivantes :  
  
- Compartiments vides :  
  - 33 % est une valeur cible correcte, mais un pourcentage plus important (même 90 %) est généralement accepté.  
  - Quand le nombre de compartiments est égal au nombre de valeurs de clés distinctes, environ 33 % des compartiments sont vides.  
  - Une valeur inférieure à 10 % est trop faible.  
- Chaînes dans les compartiments :  
  - Une longueur de chaîne moyenne de 1 est idéale en l’absence de valeurs de clé d’index en double. Les longueurs de chaîne jusqu’à 10 sont généralement acceptables.  
  - Si la longueur de chaîne moyenne est supérieure à 10 et que le pourcentage de compartiments vides est supérieur à 10 %, les données comportent tant de doublons qu’il est possible qu’un index de hachage ne représente pas le type le plus approprié.  
  
### <a name="demonstration-of-chains-and-empty-buckets"></a>Démonstration de chaînes et de compartiments vides  
  
Le bloc de code T-SQL suivant vous permet de tester aisément un `SELECT * FROM sys.dm_db_xtp_hash_index_stats;`. Le bloc de code se termine en 1 minute. Voici les phases du bloc de code suivant :  
  
1. Crée une table optimisée en mémoire avec quelques index de hachage.  
2. Remplit la table avec des milliers de lignes.  
    A. Un opérateur modulo est utilisé pour configurer le taux de valeurs en double dans la colonne StatusCode.  
    B. La boucle insère 262 144 lignes en une minute environ.  
3. PRINT imprime un message vous demandant d’exécuter l’instruction SELECT précédente à partir de **sys.dm_db_xtp_hash_index_stats**.  
  
```sql
DROP TABLE IF EXISTS SalesOrder_Mem;  
go  
  
  
CREATE TABLE SalesOrder_Mem  
(  
  SalesOrderId   uniqueidentifier  NOT NULL  DEFAULT newid(),  
  OrderSequence  int               NOT NULL,  
  OrderDate      datetime2(3)      NOT NULL,  
  StatusCode     tinyint           NOT NULL,  
  
  PRIMARY KEY NONCLUSTERED  
      HASH (SalesOrderId)  WITH (BUCKET_COUNT = 262144),  
  
  INDEX ix_OrderSequence  
      HASH (OrderSequence) WITH (BUCKET_COUNT = 20000),  
  
  INDEX ix_StatusCode  
      HASH (StatusCode)    WITH (BUCKET_COUNT = 8),  
  
  INDEX ix_OrderDate       NONCLUSTERED (OrderDate DESC)  
)  
  WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
--------------------  
  
SET NOCOUNT ON;  
  
-- Same as PK bucket_count.  68 seconds to complete.  
DECLARE @i int = 262144;  
  
BEGIN TRANSACTION;  
  
WHILE @i > 0  
BEGIN  
  
  INSERT SalesOrder_Mem  
      (OrderSequence, OrderDate, StatusCode)  
    Values  
      (@i, GetUtcDate(), @i % 8);  -- Modulo technique.  
  
  SET @i -= 1;  
END  
COMMIT TRANSACTION;  
  
PRINT 'Next, you should query:  sys.dm_db_xtp_hash_index_stats .';  
go  
```  
  
La boucle `INSERT` précédente effectue les opérations suivantes :  
  
- Elle insère des valeurs uniques pour l’index de clé primaire et pour *ix_OrderSequence*.  
- Elle insère deux cent mille lignes qui représentent uniquement 8 valeurs distinctes pour `StatusCode`. Par conséquent, le taux de duplication de valeurs est élevé dans l’index *ix_StatusCode*.  
  
Pour obtenir des informations de dépannage quand le nombre de compartiments n’est pas optimal, examinez la sortie suivante de l’instruction SELECT dans **sys.dm_db_xtp_hash_index_stats**. Pour ces résultats, nous avons ajouté `WHERE Object_Name(h.object_id) = 'SalesOrder_Mem'` à l’instruction SELECT copiée à partir de la section D.1.  
  
Nos résultats `SELECT` sont affichés après le code, artificiellement divisés en deux tables de résultats plus étroites pour un meilleur affichage.  
  
- Voici les résultats pour le *nombre de compartiments*.  
  
| IndexName | total_bucket_count | empty_bucket_count | EmptyBucketPercent |  
| :-------- | -----------------: | -----------------: | -----------------: |  
| ix_OrderSequence | 32768 | 13 | 0 |  
| ix_StatusCode | 8 | 4 | 50 |  
| PK_SalesOrd_B14003... | 262144 | 96525 | 36 |  
  
- Ensuite, voici les résultats pour la *longueur de chaîne*.  
  
| IndexName | avg_chain_length | max_chain_length |  
| :-------- | ---------------: | ---------------: |  
| ix_OrderSequence | 8 | 26 |  
| ix_StatusCode | 65536 | 65536 |  
| PK_SalesOrd_B14003... |  1 | 8 |  
  
Interprétons les tables de résultats précédentes pour les trois index de hachage :  
  
*ix_StatusCode :*  
  
- 50 % des compartiments sont vides, ce qui est correct.  
- Cependant, la longueur de chaîne moyenne est très élevée (65536).  
  - Cela indique un taux élevé de valeurs en double.  
  - Par conséquent, l’utilisation d’un index de hachage n’est pas appropriée dans ce cas. Il convient d'utiliser un index non cluster.  
  
*ix_OrderSequence :*  
  
- 0 % des compartiments sont vides, ce qui est trop faible.  
- La longueur de chaîne moyenne est égale à 8, même si toutes les valeurs de cet index sont uniques.  
  - Par conséquent, le nombre de compartiments doit être augmenté pour ramener la longueur de chaîne moyenne plus près de 2 ou 3.  
- Étant donné que la clé d’index a 262 144 valeurs uniques, le nombre de compartiments doit être supérieur ou égal à 262 144.  
  - Si une croissance future est attendue, le nombre de compartiments doit être supérieur.  
  
*Index de clé primaire (PK_SalesOrd_...) :*  
  
- 36 % des compartiments sont vides, ce qui est correct.  
- La longueur de chaîne moyenne est égale à 1, ce qui est également correct. Aucun changement n’est nécessaire.  
  
### <a name="balancing-the-trade-off"></a>Équilibre du compromis  
  
Les charges de travail OLTP se concentrent sur des lignes individuelles. Les analyses de tables complètes ne sont généralement pas un problème critique pour les performances pour les charges de travail OLTP. Donc, le compromis à équilibrer se situe entre la **quantité d’utilisation de la mémoire** et la **performance des tests d’égalité et des opérations d’ajout**.  
  
**Si l’utilisation de la mémoire représente le principal critère :**  
  
- Choisissez un nombre de compartiments proche du nombre d’enregistrements de clés d’index.  
- Le nombre de compartiments ne doit pas être considérablement inférieur au nombre de valeurs de clés d'index, car cela affecte la plupart des opérations DML tout comme le temps de récupération de la base de données après le redémarrage du serveur.  
  
**Si les performances des tests d’égalité représentent le principal critère :**  
  
- Un nombre plus élevé de compartiments, égal à 2 ou 3 fois le nombre de valeurs d’index uniques, semble approprié. Un nombre plus élevé signifie :  
  - des récupérations plus rapides lors de la recherche d’une valeur spécifique ;  
  - une utilisation accrue de la mémoire ;  
  - une augmentation du temps requis pour une analyse complète de l’index de hachage.  
  

##  <a name="Additional_Reading"></a> Lecture supplémentaire  
 [Index de hachage pour les tables mémoire optimisées](../../relational-databases/sql-server-index-design-guide.md#hash_index)   
 [Index non cluster pour les tables à mémoire optimisée](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)  
