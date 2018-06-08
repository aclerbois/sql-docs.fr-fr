---
title: Créer des vues indexées | Microsoft Docs
ms.custom: ''
ms.date: 01/22/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- indexed views [SQL Server], creating
- clustered indexes, views
- CREATE INDEX statement
- large_value_types_out_of_row option
- indexed views [SQL Server]
- views [SQL Server], indexed views
ms.assetid: f86dd29f-52dd-44a9-91ac-1eb305c1ca8d
caps.latest.revision: 79
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 556665c8b89dd8b3c2e1b608bdede9bf226c2a8f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-indexed-views"></a>Créer des vues indexées
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Cette rubrique décrit comment créer des index sur une vue. Le premier index créé sur une vue doit être un index cluster unique. Après avoir créé l'index cluster unique, vous pouvez créer davantage d'index non cluster. La création d'un index cluster unique sur une vue améliore les performances des requêtes, car la vue est stockée dans la base de données au même titre qu'une table avec un index cluster. L'optimiseur de requête peut utiliser des vues indexées pour accélérer l'exécution des requêtes. Il n'est pas nécessaire de référencer la vue dans la requête pour que l'optimiseur envisage d'utiliser cette vue.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 Les étapes suivantes de création d'une vue indexée sont essentielles à la réussite de l'implémentation de la vue indexée :  
  
1.  Vérifiez que les options SET sont correctes pour toutes les tables existantes qui seront référencées dans la vue.    
2.  Vérifiez que les options SET de la session sont définies correctement avant de créer des tables et la vue.   
3.  Vérifiez que la définition de la vue est déterministe.   
4.  Créez la vue en utilisant l’option `WITH SCHEMABINDING`.   
5.  Créez l'index cluster unique sur la vue.   

> [!IMPORTANT]
> Lors de l’exécution de DML<sup>1</sup> sur une table référencée par un grand nombre de vues indexées, ou par moins de vues mais très complexes, ces vues indexées référencées doivent également être mises à jour. Par conséquent, les performances des requêtes DML peuvent se dégrader considérablement ou, dans certains cas, un plan de requête ne peut même pas être produit.   
> Dans de tels scénarios, testez vos requêtes DML avant une utilisation en production, analysez le plan de requête et optimisez/simplifiez l’instruction DML.   
>   
> <sup>1</sup> Comme des opérations UPDATE, DELETE ou INSERT.   
  
###  <a name="Restrictions"></a> Options SET requises pour les vues indexées  
L'évaluation de la même expression peut produire des résultats différents dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] si des options SET différentes sont actives lors de l'exécution de la requête. Par exemple, si l’option SET `CONCAT_NULL_YIELDS_NULL` est définie sur ON, l’expression `'abc' + NULL` retourne la valeur `NULL`. Cependant, si l’option `CONCAT_NULL_YIEDS_NULL` est définie sur OFF, la même expression produit `'abc'`.  
  
Pour pouvoir gérer correctement les vues et retourner des résultats cohérents, les vues indexées nécessitent des valeurs fixes pour plusieurs options SET. Les options SET répertoriées dans le tableau ci-dessous doivent être définies avec les valeurs indiquées dans la colonne **Valeur requise** chaque fois que les conditions suivantes sont réunies :  
  
-   La vue et les index suivants sur la vue sont créés.    
  
-   Tables de base référencées dans la vue lorsque la table est créée.    
  
-   Une insertion, une mise à jour ou une suppression est exécutée sur une table qui participe à la vue indexée. Cette exigence inclut des opérations telles que la copie en bloc, la réplication et les requêtes distribuées.    
  
-   L'optimiseur de requête utilise la vue indexée pour générer le plan de requête.   
  
|Options SET|Valeur requise|Valeur de serveur par défaut|Valeur par défaut<br /><br /> Valeur OLE DB et ODBC|Valeur par défaut<br /><br /> Valeur DB-Library|  
|-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|  
|ANSI_NULLS|ON|ON|ON|OFF|  
|ANSI_PADDING|ON|ON|ON|OFF|  
|ANSI_WARNINGS<sup>1</sup>|ON|ON|ON|OFF|  
|ARITHABORT|ON|ON|OFF|OFF|  
|CONCAT_NULL_YIELDS_NULL|ON|ON|ON|OFF|  
|NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|  
|QUOTED_IDENTIFIER|ON|ON|ON|OFF|  
  
<sup>1</sup> Définir `ANSI_WARNINGS` sur ON définit implicitement `ARITHABORT` sur ON.  
  
Si vous utilisez une connexion serveur OLE DB ou ODBC, la seule valeur qui doit être modifiée est le paramètre `ARITHABORT`. Toutes les valeurs DB-Library doivent être définies correctement au niveau du serveur à l’aide de **sp_configure** , ou dans l’application à l’aide de la commande SET.  
  
> [!IMPORTANT]  
> Il est fortement recommandé de définir l’option utilisateur `ARITHABORT` sur ON sur le serveur dès la création de la première vue indexée ou du premier index sur une colonne calculée dans une base de données sur le serveur.  
  
### <a name="deterministic-views"></a>Vues déterministes  
La définition d'une vue indexée doit être déterministe. Une vue est déterministe si toutes les expressions de la liste de sélection, ainsi que les clauses `WHERE` et `GROUP BY` sont déterministes. Les expressions déterministes retournent toujours le même résultat chaque fois qu'elles sont évaluées avec un groupe de valeurs d'entrée spécifiques. Seules les fonctions déterministes peuvent participer à des expressions déterministes. Par exemple, la fonction `DATEADD` est déterministe, car elle retourne toujours le même résultat pour un groupe donné de valeurs d’arguments pour ses trois paramètres. `GETDATE` n’est pas déterministe, car elle est toujours appelée avec le même argument, mais la valeur qu’elle retourne change chaque fois qu’elle est exécutée.  
  
Pour déterminer si une colonne de vue est déterministe, utilisez la propriété **IsDeterministic** de la fonction [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) . Pour déterminer si une colonne déterministe d’une vue avec une liaison de schéma est précise, utilisez la propriété **IsPrecise** de la fonction `COLUMNPROPERTY`. `COLUMNPROPERTY` retourne 1 pour TRUE, 0 pour FALSE et NULL pour une entrée non valide. Cela signifie que la colonne n'est pas déterministe ou pas précise.  
  
Même si une expression est déterministe, si elle contient des expressions flottantes, le résultat exact dépend de l'architecture du processeur ou de la version du microcode. Pour garantir l'intégrité des données, ces expressions peuvent participer seulement sous forme de colonnes non clés de vues indexées. Les expressions déterministes qui ne contiennent pas d'expressions flottantes s'appellent des expressions précises. Seules les expressions déterministes précises peuvent participer à des colonnes clés et dans les clauses `WHERE` et `GROUP BY` des vues indexées.  

### <a name="additional-requirements"></a>Autres conditions requises  
Outre les options SET et les conditions requises pour les fonctions déterministes, les conditions suivantes doivent être satisfaites :  
  
-   L’utilisateur qui exécute `CREATE INDEX` doit être le propriétaire de la vue.    
  
-   Quand vous créez l’index, l’option `IGNORE_DUP_KEY` doit être définie sur OFF (valeur par défaut).    
  
-   Les tables doivent être référencées par des noms en deux parties, *schéma ***.*** nom_table*, dans la définition de la vue.    
  
-   Les fonctions définies par l’utilisateur référencées dans la vue doivent avoir été créées avec l’option `WITH SCHEMABINDING`.    
  
-   Toutes les fonctions définies par l’utilisateur référencées dans la vue doivent être référencées par des noms en deux parties, *\<schéma>***.***\<fonction>*.   
  
-   La propriété d’accès aux données d’une fonction définie par l’utilisateur doit avoir la valeur `NO SQL`, et la propriété d’accès externe doit avoir la valeur `NO`.   
  
-   Les fonctions CLR (Common Language Runtime) peuvent s'afficher dans la liste SELECT de la vue mais ne peuvent pas faire partie de la définition de la clé d'index cluster. Ces fonctions ne peuvent pas apparaître dans la clause WHERE de la vue ou dans la clause ON d'une opération JOIN au sein de la vue.   
  
-   Les propriétés des méthodes et fonctions CLR des types CLR définis par l'utilisateur employés dans la définition de vue doivent être définies de la manière illustrée dans le tableau suivant.   
  
    |Propriété|Remarque|  
    |--------------|----------|  
    |DETERMINISTIC = TRUE|Doit être déclarée explicitement comme attribut de la méthode Microsoft .NET Framework.|  
    |PRECISE = TRUE|Doit être déclarée explicitement comme attribut de la méthode .NET Framework.|  
    |DATA ACCESS = NO SQL|Déterminée en affectant à l'attribut DataAccess la valeur DataAccessKind.None et à l'attribut SystemDataAccess la valeur SystemDataAccessKind.None.|  
    |EXTERNAL ACCESS = NO|Cette propriété a la valeur NO par défaut pour les routines CLR.|  
  
-   La vue doit être créée avec l’option `WITH SCHEMABINDING`.  
  
-   La vue doit référencer seulement des tables de base qui sont dans la même base de données que la vue. La vue ne peut pas faire référence à d'autres vues.  
  
-   L'instruction SELECT de la définition de la vue ne doit pas contenir les éléments Transact-SQL suivants :  
  
    ||||  
    |-|-|-|  
    |`COUNT`|Fonctions de ROWSET (`OPENDATASOURCE`, `OPENQUERY`, `OPENROWSET` ET `OPENXML`)|Jointures `OUTER` (`LEFT`, `RIGHT` ou `FULL`)|  
    |Table dérivée (définie en spécifiant une instruction `SELECT` dans la clause `FROM`)|Jointures réflexives|Spécification de colonnes avec `SELECT *` ou `SELECT <table_name>.*`|  
    |`DISTINCT`|`STDEV`, `STDEVP`, `VAR`, `VARP` ou`AVG`|Expression de table commune (CTE)|  
    |**float**<sup>1</sup>, colonnes **text**, **ntext**, **image**, **XML** ou **filestream**|Sous-requête|Clause `OVER`, qui inclut des fonctions de classement ou d’agrégation de fenêtre|  
    |Prédicats de texte intégral (`CONTAINS`, `FREETEXT`)|Fonction `SUM` qui référence une expression acceptant les valeurs Null|`ORDER BY`|  
    |Fonction d'agrégation CLR définie par l'utilisateur|`TOP`|Opérateurs `CUBE`, `ROLLUP` ou `GROUPING SETS`|  
    |`MIN`, `MAX`|Opérateurs `UNION`, `EXCEPT` ou `INTERSECT`|`TABLESAMPLE`|  
    |les variables de tables ;|`OUTER APPLY` ou `CROSS APPLY`|`PIVOT`, `UNPIVOT`|  
    |Jeux de colonnes éparses|Fonctions table incluse ou fonctions table à instructions multiples|`OFFSET`|  
    |`CHECKSUM_AGG`|||  
  
     <sup>1</sup> La vue indexée peut contenir des colonnes **float**, mais ces colonnes ne peuvent pas être incluses dans la clé d’index cluster.  
  
-   Si la clause `GROUP BY` est présente, la définition VIEW doit contenir `COUNT_BIG(*)`, mais pas `HAVING`. Ces restrictions de `GROUP BY` sont applicables seulement à la définition de la vue indexée. Une requête peut utiliser une vue indexée dans son plan d’exécution, même si elle ne répond pas à ces restrictions de `GROUP BY`.  
  
-   Si la définition de la vue contient une clause `GROUP BY`, la clé de l’index cluster unique peut référencer seulement les colonnes définies dans la clause `GROUP BY`.  
  
> [!IMPORTANT]  
> Les vues indexées ne sont pas prises en charge en plus des requêtes temporelles (qui utilisent la clause `FOR SYSTEM_TIME`).  

###  <a name="Recommendations"></a> Recommandations  
 Si vous faites référence aux littéraux de chaîne **datetime** et **smalldatetime** au sein de vues indexées, il est recommandé de convertir explicitement le littéral en type date souhaité à l’aide d’un style de format de date déterministe. Pour obtenir la liste des styles de formats de date qui sont déterministes, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md). Pour plus d’informations sur les expressions déterministes et non déterministes, consultez la section [Considérations](#nondeterministic) de cette page.

Lors de l’exécution d’instructions DML (comme `UPDATE`, `DELETE` ou `INSERT`) sur une table référencée par un grand nombre de vues indexées, ou par moins de vues mais très complexes, ces vues indexées référencées doivent également être mises à jour pendant l’exécution d’instructions DML. Par conséquent, les performances des requêtes DML peuvent se dégrader considérablement ou, dans certains cas, un plan de requête ne peut même pas être produit. Dans de tels scénarios, testez vos requêtes DML avant une utilisation en production, analysez le plan de requête et optimisez/simplifiez l’instruction DML.
  
###  <a name="Considerations"></a> Observations  
 La valeur de l’option **large_value_types_out_of_row** des colonnes contenues dans une vue indexée est héritée de la valeur de la colonne correspondante dans la table de base. Cette valeur est définie à l’aide de [sp_tableoption](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). La valeur par défaut des colonnes constituées à partir d'expressions est 0. Cela signifie que les types de valeurs élevées sont stockés dans la ligne.  
  
 Des vues indexées peuvent être créées sur une table partitionnée, et elles peuvent elles-mêmes être partitionnées.  
  
 Pour empêcher [!INCLUDE[ssDE](../../includes/ssde-md.md)] d’utiliser des vues indexées, incluez l’indicateur `OPTION (EXPAND VIEWS)` dans la requête. En outre, si des options sont définies incorrectement, l'optimiseur ne peut pas utiliser les index des vues. Pour plus d’informations sur l’indicateur `OPTION (EXPAND VIEWS)`, consultez [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
 Si une vue est supprimée, tous ses index le sont également. Tous les index non cluster et les caractéristiques créées automatiquement d'une vue sont supprimés lorsque son index cluster l'est. Les statistiques créées par l'utilisateur sur la vue sont conservées. Les index non cluster peuvent toutefois être supprimés individuellement. Lorsque l'index cluster de la vue est supprimé, le jeu de résultats stocké est aussi supprimé, et l'optimiseur traite de nouveau la vue comme une vue standard.  
  
 Les index sur les tables et les vues peuvent être désactivés. Lorsqu'un index cluster sur une table est désactivé, les index sur les vues associées à la table le sont également.  
 
<a name="nondeterministic"></a> Les expressions qui impliquent une conversion implicite de chaînes de caractères en **datetime** ou **smalldatetime** sont considérées comme non déterministes. Cela est dû au fait que les résultats dépendent des paramètres LANGUAGE et DATEFORMAT de la session serveur. Par exemple, les résultats de l'expression `CONVERT (datetime, '30 listopad 1996', 113)` dépendent du paramètre LANGUAGE, car la chaîne '`listopad`' désigne des mois différents selon la langue. De même, dans l'expression `DATEADD(mm,3,'2000-12-01')`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interprète la chaîne `'2000-12-01'` en fonction du paramètre DATEFORMAT. La conversion implicite de données caractères non-Unicode entre les classements est également considérée comme non déterministe.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l’autorisation **CREATE VIEW** dans la base de données et l’autorisation **ALTER** sur le schéma dans lequel la vue est créée.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-an-indexed-view"></a>Pour créer une vue indexée  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple suivant crée une vue et un index sur cette vue. Deux requêtes sont incluses, lesquelles utilisent la vue indexée.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    --Set the options to support indexed views.  
    SET NUMERIC_ROUNDABORT OFF;  
    SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,  
        QUOTED_IDENTIFIER, ANSI_NULLS ON;  
    GO  
    --Create view with schemabinding.  
    IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL  
    DROP VIEW Sales.vOrders ;  
    GO  
    CREATE VIEW Sales.vOrders  
    WITH SCHEMABINDING  
    AS  
        SELECT SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Revenue,  
            OrderDate, ProductID, COUNT_BIG(*) AS COUNT  
        FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o  
        WHERE od.SalesOrderID = o.SalesOrderID  
        GROUP BY OrderDate, ProductID;  
    GO  
    --Create an index on the view.  
    CREATE UNIQUE CLUSTERED INDEX IDX_V1   
        ON Sales.vOrders (OrderDate, ProductID);  
    GO  
    --This query can use the indexed view even though the view is   
    --not specified in the FROM clause.  
    SELECT SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Rev,   
        OrderDate, ProductID  
    FROM Sales.SalesOrderDetail AS od  
        JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID=o.SalesOrderID  
            AND ProductID BETWEEN 700 and 800  
            AND OrderDate >= CONVERT(datetime,'05/01/2002',101)  
    GROUP BY OrderDate, ProductID  
    ORDER BY Rev DESC;  
    GO  
    --This query can use the above indexed view.  
    SELECT  OrderDate, SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Rev  
    FROM Sales.SalesOrderDetail AS od  
        JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID=o.SalesOrderID  
            AND DATEPART(mm,OrderDate)= 3  
            AND DATEPART(yy,OrderDate) = 2002  
    GROUP BY OrderDate  
    ORDER BY OrderDate ASC;  
    GO  
    ```  
  
Pour plus d’informations, consultez [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md)   
 [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)   
 [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)   
 [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
  
  
