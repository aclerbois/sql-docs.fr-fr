---
title: Jointures (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 02/18/2018
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology:
- joins
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- HASH join
- NESTED LOOPS join
- MERGE join
- joins [SQL Server], about joins
- join hints [SQL Server]
ms.assetid: bfc97632-c14c-4768-9dc5-a9c512f4b2bd
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1c078a3ab0b1899267705673ed149689bcdc5f05
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="joins-sql-server"></a>Jointures (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue les opérations de tri, d’intersection, d’union et de différentiation au moyen des technologies de jointure de hachage et de tri en mémoire. Grâce à ce type de plan de requête, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le partitionnement vertical de tables, parfois nommé stockage en colonnes.   

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise trois types d'opérations de jointure :    
-   Jointures de boucles imbriquées     
-   Jointures de fusion   
-   Jointures de hachage   

## <a name="fundamentals"></a> Principes de base des jointures
Les jointures permettent d'extraire des données de deux ou de plusieurs tables en fonction des relations logiques existant entre ces tables. Les jointures indiquent comment Microsoft SQL Server doit utiliser les données d’une table pour sélectionner les lignes d’une autre table.    

Une condition de jointure définit la manière dont deux tables sont liées dans une requête :    
-   en spécifiant la colonne de chaque table à utiliser pour la jointure. Une condition de jointure standard spécifie une clé étrangère d'une table et la clé qui lui est associée dans l'autre table ;    
-   en spécifiant un opérateur logique (par exemple = ou <>) à utiliser pour comparer les valeurs des colonnes.    

Vous pouvez spécifier des jointures internes dans les clauses `FROM` ou `WHERE`. Vous ne pouvez spécifier des jointures externes que dans la clause `FROM`. Les conditions de jointure peuvent être combinées aux conditions de recherche `WHERE` et `HAVING` afin de contrôler les lignes qui sont sélectionnées parmi les tables de base référencées dans la clause `FROM`.    

Nous vous recommandons de spécifier les conditions de jointure dans la clause `FROM`, car cela vous permet de les séparer des autres conditions de recherche susceptibles d’être spécifiées dans une clause `WHERE`. Voici une syntaxe de jointure simplifiée d'une clause FROM ISO :

```
FROM first_table join_type second_table [ON (join_condition)]
```

*join_type* spécifie le type de jointure effectué : interne, externe ou croisée. *join_condition* définit le prédicat à évaluer pour chaque paire de lignes jointes. L'exemple suivant spécifie une jointure dans une clause FROM :

```sql
FROM Purchasing.ProductVendor JOIN Purchasing.Vendor
     ON (ProductVendor.BusinessEntityID = Vendor.BusinessEntityID)
```

L'instruction SELECT suivante utilise cette jointure :

```sql
SELECT ProductID, Purchasing.Vendor.BusinessEntityID, Name
FROM Purchasing.ProductVendor JOIN Purchasing.Vendor
    ON (Purchasing.ProductVendor.BusinessEntityID = Purchasing.Vendor.BusinessEntityID)
WHERE StandardPrice > $10
  AND Name LIKE N'F%'
GO
```

La sélection retourne les informations relatives au produit et au fournisseur pour toutes les combinaisons de pièces fournies par une société dont le nom commence par F et dont le prix du produit est supérieur à 10 $.   

Lorsque plusieurs tables sont référencées dans une seule requête, aucune référence de colonne ne doit être ambiguë. Dans l’exemple précédent, les tables ProductVendor et Vendor contiennent toutes deux une colonne nommée BusinessEntityID. Tout nom de colonne dupliqué dans deux ou plusieurs tables référencées dans la requête doit être qualifié par le nom de la table. Toutes les références aux colonnes Vendor de l’exemple sont qualifiées.   

Lorsqu'un nom de colonne n'est pas dupliqué dans d'autres tables utilisées dans la requête, les références à ce nom ne doivent pas être qualifiées par le nom de la table. Cette caractéristique apparaît dans l'exemple précédent. Une telle instruction SELECT est parfois difficile à comprendre car rien n'indique de quelle table provient chaque colonne. Vous pouvez donc améliorer la lisibilité de la requête si vous qualifiez toutes les colonnes par leur nom de table. La lisibilité est encore améliorée si vous utilisez des alias de table, surtout lorsque les noms de table eux-mêmes doivent être qualifiés par les noms de base de données et de propriétaire. Il s'agit du même exemple, à ceci près que des alias de table ont été affectés et que les colonnes ont été qualifiées par les alias de table afin d'améliorer la lisibilité :

```sql
SELECT pv.ProductID, v.BusinessEntityID, v.Name
FROM Purchasing.ProductVendor AS pv 
JOIN Purchasing.Vendor AS v
    ON (pv.BusinessEntityID = v.BusinessEntityID)
WHERE StandardPrice > $10
    AND Name LIKE N'F%';
```

Dans les exemples précédents, les conditions de jointure sont spécifiées dans la clause FROM, ce qui correspond à la méthode recommandée. La requête suivante contient la même condition de jointure spécifiée dans la clause WHERE :

```sql
SELECT pv.ProductID, v.BusinessEntityID, v.Name
FROM Purchasing.ProductVendor AS pv, Purchasing.Vendor AS v
WHERE pv.BusinessEntityID=v.BusinessEntityID
    AND StandardPrice > $10
    AND Name LIKE N'F%';
```

La liste de sélection d'une jointure peut faire référence à toutes les colonnes des tables jointes ou à un sous-ensemble de colonnes. Il n'est pas nécessaire que la liste de sélection contienne des colonnes de toutes les tables de la jointure. Dans une jointure sur trois tables, par exemple, vous pouvez n'utiliser qu'une seule table pour rapprocher l'une des autres tables de la troisième, sans qu'il soit nécessaire de référencer dans la liste de sélection une des colonnes de la table du milieu.   

Bien que les conditions de jointure disposent généralement d'opérateurs de comparaison d'égalité (=), vous pouvez spécifier d'autres opérateurs de comparaison ou relationnels ainsi que d'autres prédicats. Pour plus d’informations, consultez [Opérateurs de comparaison &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md) et [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md).  

Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procède au traitement des jointures, le moteur de requête choisit (parmi plusieurs possibilités) la méthode de traitement la plus efficace. L'exécution physique de différentes jointures peut utiliser de nombreuses optimisations différentes et ne peut par conséquent pas être prédite de manière fiable.   

Les colonnes utilisées dans une condition de jointure ne doivent pas forcément porter le même nom ou être du même type de données. Si les types de données ne sont pas identiques, ils doivent cependant être compatibles ou pouvoir être convertis de manière implicite par SQL Server. Si les types de données ne peuvent pas être convertis implicitement, la condition de jointure doit le faire explicitement à l’aide de la fonction `CAST`. Pour plus d’informations sur les conversions implicites et explicites, consultez [Conversion de types de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).    

La plupart des requêtes utilisant une jointure peuvent être réécrites à l'aide d'une sous-requête (une requête imbriquée dans une autre), et la plupart des sous-requêtes peuvent être réécrites sous forme de jointures. Pour plus d’informations sur les sous-requêtes, consultez [Sous-requêtes](../../relational-databases/performance/subqueries.md).   

> [!NOTE]
> Les tables ne peuvent pas être jointes directement sur des colonnes ntext, text ou image. Elles peuvent cependant l’être indirectement sur des colonnes ntext, text ou image à l’aide de `SUBSTRING`.    
> Par exemple, `SELECT * FROM t1 JOIN t2 ON SUBSTRING(t1.textcolumn, 1, 20) = SUBSTRING(t2.textcolumn, 1, 20)` exécute une jointure interne sur deux tables en prenant en compte les 20 premiers caractères de chaque colonne de type text dans les tables t1 et t2.   
> En outre, il existe une autre possibilité de comparaison des colonnes ntext ou text de deux tables ; elle consiste à comparer les longueurs des colonnes à l’aide d’une clause `WHERE`, par exemple `WHERE DATALENGTH(p1.pr_info) = DATALENGTH(p2.pr_info)`

## <a name="nested_loops"></a> Présentation des jointures de boucles imbriquées
Si une entrée de jointure est petite (moins de 10 lignes) et que l'autre entrée de jointure est assez importante et indexée sur ses colonnes de jointure, une jointure de type boucles imbriquées d'index représente l'opération de jointure la plus rapide, car elle nécessite le moins d'E/S et de comparaisons. 

La jointure de boucles imbriquées, également nommée *itération imbriquée*, utilise une entrée de jointure comme table d’entrée externe (représentée comme entrée la plus haute dans le plan d’exécution graphique) et une autre comme table d’entrée interne (la plus basse). La boucle externe mobilise la table d'entrée externe ligne par ligne. La boucle interne, exécutée pour chaque ligne externe, recherche les lignes en correspondance dans la table d'entrée interne.   

Dans le cas le plus simple, la recherche analyse intégralement une table ou un index, opération désignée tout simplement sous le nom de *jointure de boucles imbriquées*. Si la recherche exploite un index, il s’agit d’une *jointure de boucles imbriquées d’index*. Si l’index est généré dans le cadre du plan de requête (et détruit à l’achèvement de la requête), il s’agit d’une *jointure de boucles imbriquées d’index temporaire*. Toutes ces variantes sont prises en compte par l’optimiseur de requête.   

Une jointure de boucles imbriquées est particulièrement efficace si l'entrée externe est petite alors que l'entrée interne préindexée est volumineuse. Dans bon nombre de transactions réduites, comme celles concernant un petit nombre de lignes seulement, les jointures de boucles imbriquées d'index sont supérieures aux jointures de fusion et de hachage. À l'inverse, pour les requêtes de grande ampleur, les jointures de boucles imbriquées ne constituent pas le meilleur choix.    

## <a name="merge"></a> Présentation des jointures de fusion
Si les deux entrées de jointure ne sont pas réduites, mais triées sur leur colonne de jointure (si elles ont été obtenues par analyse des index triés, par exemple), une jointure de fusion constitue l'opération de jointure la plus rapide. Si les deux entrées de jointure sont étendues et de taille similaire, une jointure de fusion avec tri préalable et une jointure de hachage offrent des performances équivalentes. Cependant, les opérations de jointure de hachage sont souvent beaucoup plus rapides si la taille des deux entrées diffère de façon significative.       

La jointure de fusion impose le tri des deux entrées sur les colonnes de fusion, qui sont définies par les clauses d'égalité (ON) du prédicat de jointure. L'optimiseur de requête analyse généralement un index, s'il en existe un sur l'ensemble approprié de colonnes, ou place un opérateur de tri sous la jointure de fusion. Dans de rares cas, il peut y avoir plusieurs clauses d'égalité, mais les colonnes de fusion ne sont prises en compte que par certaines des clauses d'égalité disponibles.    

Étant donné que chaque entrée est triée, l’opérateur **Merge Join** reçoit une ligne de chaque entrée et les compare. Par exemple, pour les opérations de jointure interne, les lignes sont retournées si elles sont égales. Si elles ne le sont pas, la ligne ayant la valeur la plus faible est ignorée et une autre ligne est prélevée dans cette entrée. Ce processus se répète jusqu'au traitement intégral de toutes les lignes.    

L'opération de jointure de fusion peut être de type régulier ou de type plusieurs-à-plusieurs. Une jointure de fusion de type plusieurs-à-plusieurs utilise une table temporaire pour stocker des lignes. S'il existe des valeurs en double provenant de chaque entrée, l'une d'elles devra être replacée au début de ces valeurs en double au fur et à mesure du traitement de chaque doublon provenant de l'autre entrée.    

Si un prédicat résiduel existe, toutes les lignes répondant au prédicat de fusion évaluent le prédicat résiduel, et seules celles y répondant sont retournées.   

La jointure de fusion est très rapide en elle-même mais elle peut constituer un choix onéreux si des opérations de tri sont nécessaires. Toutefois, si le volume de données est important et si les données souhaitées peuvent être obtenues prétriées à partir d'index d'arbre B (B-tree) existants, la jointure de fusion constitue souvent l'algorithme de jointure le plus rapide.    

## <a name="hash"></a> Présentation des jointures de hachage
Les jointures de hachage peuvent traiter efficacement des entrées importantes, non indexées et non triées. Elles sont utiles pour obtenir des résultats intermédiaires dans des requêtes complexes, car :
-   Les résultats intermédiaires ne sont pas indexés (sauf s'ils sont volontairement enregistrés sur disque puis indexés) et souvent ne sont pas triés de façon adéquate pour l'opération suivante du plan de requête.
-   Les optimiseurs de requêtes n'évaluent que les tailles de résultats intermédiaires. Comme ces estimations risquent d'être imprécises pour les requêtes complexes, les algorithmes qui traitent les résultats intermédiaires doivent bien évidemment être efficaces, mais ils doivent également offrir un traitement réduit si un résultat intermédiaire s'avère bien plus volumineux que prévu.   

La jointure de hachage permet le déroulement de réductions dans le cadre de la dénormalisation. Cette dernière est généralement utilisée pour obtenir de meilleures performances par la réduction des opérations de jointure, malgré les risques de redondance, tels que les mises à jour incohérentes. Les jointures de hachage réduisent le besoin de dénormalisation. Elles autorisent le partitionnement vertical (en représentant des groupes de colonnes d'une table unique dans des index ou des fichiers séparés) comme option valable pour la structure des bases de données physiques.     

La jointure de hachage dispose de deux entrées : l’entrée de construction (**build**) et l’entrée d’exploration (**probe**). L'optimiseur de requête attribue ces rôles de sorte que la plus petite des deux entrées soit l'entrée de construction.    

Les jointures de hachage sont utilisées pour de nombreux types d'opérations de correspondance d'ensembles : jointure interne ; jointure externe gauche, droite et complète ; semi-jointure gauche et droite ; intersection ; union ; différentiation. En outre, le regroupement et la suppression des doublons, tels que `SUM(salary) GROUP BY department`, peuvent être effectués par une variante de la jointure de hachage. Ces modifications n'utilisent qu'une seule entrée pour les rôles de construction et d'exploration.   

Les sections suivantes décrivent différents types de jointures de hachage : jointure de hachage en mémoire, jointure de hachage progressive et jointure de hachage récursive.    

### <a name="inmem_hash"></a> Jointure de hachage en mémoire

La jointure de hachage analyse ou calcule l'intégralité de l'entrée de construction puis génère une table de hachage en mémoire. Chaque ligne est insérée dans un compartiment de hachage en fonction de la valeur de hachage calculée pour la clé de hachage. Si la totalité de l'entrée de construction est inférieure à la mémoire disponible, toutes les lignes peuvent être insérées dans la table de hachage. La phase de construction est suivie de la phase d'exploration. La totalité de l'entrée d'exploration est analysée ou calculée ligne par ligne puis, pour chaque ligne d'exploration, la valeur de clé de hachage est calculée, le compartiment de hachage correspondant analysé et les correspondances établies.    

### <a name="grace_hash"></a> Jointure de hachage progressive

Si l'entrée de construction ne tient pas en mémoire, une jointure de hachage intervient en plusieurs étapes. Cette opération est qualifiée de jointure de hachage progressive. Chaque étape comprend une phase de construction et une phase d'exploration. Initialement, l'intégralité des entrées de construction et d'exploration est mobilisée et partitionnée (à l'aide d'une fonction de hachage appliquée aux clés de hachage) en plusieurs fichiers. L'utilisation de la fonction de hachage sur les clés de hachage garantit la présence des deux enregistrements de jointure dans la même paire de fichiers. Ainsi, la tâche consistant à joindre deux entrées volumineuses a été réduite à plusieurs instances moins importantes des mêmes tâches. La jointure de hachage est ensuite appliquée à chaque paire de fichiers partitionnés.    

### <a name="recursive_hash"></a> Jointure de hachage récursive

Si l'entrée de construction est volumineuse au point que les entrées pour une fusion externe standard nécessitent plusieurs niveaux de fusion, plusieurs étapes et plusieurs niveaux de partitionnement sont nécessaires. Si seules certaines partitions sont volumineuses, des étapes de partitionnement supplémentaires ne sont utilisées que pour celles-ci. Des opérations d'E/S asynchrones sont utilisées de sorte qu'un thread unique puisse occuper plusieurs unités de disque, dans le but d'accélérer au maximum les étapes du partitionnement.    

> [!NOTE]
> Si l'entrée de construction est à peine plus volumineuse que la mémoire disponible, des éléments de jointure de hachage en mémoire et de jointure de hachage progressive sont associés en une étape unique, créant ainsi une jointure de hachage hybride.   

Il n'est pas toujours possible, lors de l'optimisation, de déterminer quelle jointure de hachage doit être utilisée. C’est pourquoi SQL Server commence par utiliser une jointure de hachage en mémoire, puis transite graduellement vers la jointure de hachage progressive et la jointure de hachage récursive en fonction de la taille de l’entrée de construction.    

Si l’optimiseur de requête anticipe mal laquelle des deux entrées est moins volumineuse et devrait donc être l’entrée de construction, les rôles de construction et d’exploration sont inversés de façon dynamique. La jointure de hachage garantit que l'optimiseur utilise bien le fichier de débordement le moins volumineux comme entrée de construction. Cette technique s’appelle l’inversion des rôles. L'inversion des rôles intervient à l'intérieur de la jointure de hachage après au moins un débordement sur le disque.     

> [!NOTE]
> L'inversion des rôles se produit indépendamment de tous les indicateurs ou de toute structure de requête. L'inversion des rôles ne s'affiche pas dans votre plan de requête ; quand elle se produit, elle est transparente pour l'utilisateur.

### <a name="hash_bailout"></a> Interruption de hachage

Le terme interruption de hachage est parfois utilisé pour décrire des jointures de hachage progressives ou des jointures de hachage récursives.    

> [!NOTE]
> Les jointures de hachage récursives ou les interruptions de hachage entraînent une diminution des performances de votre serveur. Si vous voyez de nombreux événements d’avertissements de hachage dans une trace, mettez à jour les statistiques sur les colonnes qui sont jointes.    

Pour plus d’informations sur l’interruption de hachage, consultez [Hash Warning (classe d’événements)](../../relational-databases/event-classes/hash-warning-event-class.md).    
  
## <a name="nulls_joins"></a> Valeurs Null et jointures
Si certaines colonnes de tables jointes contiennent des valeurs NULL, ces valeurs NULL ne correspondent pas les unes aux autres. La présence de valeurs Null dans une colonne d’une des tables jointes ne peut être retournée que si vous utilisez une jointure externe (sauf si la clause `WHERE` exclut les valeurs Null).     

Les deux tables suivantes contiennent chacune des valeurs NULL dans les colonnes participant à la jointure :     

```
table1                          table2
a           b                   c            d
-------     ------              -------      ------
      1        one                 NULL         two
   NULL      three                    4        four
      4      join4
```    

Une jointure qui compare les valeurs de la colonne a à celles de la colonne c ne trouve pas de correspondance dans les colonnes qui comportent des valeurs Null :

```sql
SELECT *
FROM table1 t1 JOIN table2 t2
   ON t1.a = t2.c
ORDER BY t1.a;
GO
```  

Seule une ligne comportant la valeur 4 dans les colonnes a et c est retournée :

```
a           b      c           d      
----------- ------ ----------- ------ 
4           join4  4           four   

(1 row(s) affected)
```   

Les valeurs NULL retournées d'une table de base sont également difficiles à distinguer des valeurs NULL retournées d'une jointure externe. Par exemple, l'instruction `SELECT` suivante effectue une jointure externe gauche sur ces deux tables :   

```sql
SELECT *
FROM table1 t1 LEFT OUTER JOIN table2 t2
   ON t1.a = t2.c
ORDER BY t1.a;
GO
```   

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]   

```
a           b      c           d      
----------- ------ ----------- ------ 
NULL        three  NULL        NULL 
1           one    NULL        NULL 
4           join4  4           four   

(3 row(s) affected)
```   

Dans les résultats, il est difficile d’établir la différence entre une valeur Null provenant des données et une valeur Null représentant un échec de jointure. Lorsque des valeurs NULL figurent dans des données à joindre, il est préférable de les retirer des résultats en employant une jointure normale.    

## <a name="see-also"></a> Voir aussi  
[Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
[Opérateurs de comparaison &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)    
[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
[Sous-requêtes](../../relational-databases/performance/subqueries.md)      
[Jointures adaptatives](../../relational-databases/performance/adaptive-query-processing.md#batch-mode-adaptive-joins)    


  
  
