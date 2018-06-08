---
title: WHILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- WHILE_TSQL
- WHILE
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], repeated executions
- statement blocks [SQL Server]
- repeated statement executions
- nested WHILE loops
- WHILE keyword
ms.assetid: 52dd29ab-25d7-4fd3-a960-ac55c30c9ea9
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7d264357616121eeb25d4265ffeceadcdbc1b275
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="while-transact-sql"></a>WHILE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Définit la condition qui fera se répéter l'exécution d'une instruction SQL ou d'un bloc d'instructions. L'exécution des instructions se répète aussi longtemps que la condition spécifiée demeure vraie. L'exécution des instructions de la boucle WHILE peut être contrôlée de l'intérieur de la boucle avec les mots clés BREAK et CONTINUE.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
WHILE Boolean_expression   
     { sql_statement | statement_block | BREAK | CONTINUE }  
  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
WHILE Boolean_expression   
     { sql_statement | statement_block | BREAK }  
  
```  
  
## <a name="arguments"></a>Arguments  
 *Boolean_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) qui renvoie **TRUE** ou **FALSE**. Si l'expression booléenne contient une instruction SELECT, cette dernière doit être mise entre parenthèses.  
  
 {*sql_statement* | *statement_block*}  
 Toute instruction ou tout groupe d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] tels que définis dans un bloc d'instructions. Pour définir un bloc d'instructions, utilisez les mots clés de contrôle de flux BEGIN et END.  
  
 BREAK  
 Provoque la sortie de la boucle WHILE la plus récente. Toutes les instructions situées après le mot clé END, marquant la fin de cette boucle, sont alors exécutées.  
  
 CONTINUE  
 Provoque le redémarrage de la boucle WHILE, en ignorant toutes les instructions qui suivent le mot clé CONTINUE.  
  
## <a name="remarks"></a>Notes   
 Si plusieurs boucles WHILE sont imbriquées, le BREAK à l'intérieur de la boucle provoque le retour à la boucle précédente. Avant que la boucle précédente ne prenne le relais, toutes les instructions situées après la fin de la boucle intérieure sont d'abord exécutées.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-break-and-continue-with-nested-ifelse-and-while"></a>A. Utilisation de BREAK et CONTINUE avec des IF…ELSE et des WHILE imbriqués  
 Dans l'exemple suivant, si le prix moyen d'un produit est inférieur à `$300`, la boucle `WHILE` double les prix puis sélectionne le prix maximum. Si le prix maximum est inférieur ou égal à `$500`, la boucle `WHILE` redémarre et double de nouveau les prix. Cette boucle continue à doubler les prix jusqu'à ce que le prix maximum soit supérieur à `$500`, puis le programme sort de la boucle `WHILE` et affiche un message.  
  
```  
USE AdventureWorks2012;  
GO  
WHILE (SELECT AVG(ListPrice) FROM Production.Product) < $300  
BEGIN  
   UPDATE Production.Product  
      SET ListPrice = ListPrice * 2  
   SELECT MAX(ListPrice) FROM Production.Product  
   IF (SELECT MAX(ListPrice) FROM Production.Product) > $500  
      BREAK  
   ELSE  
      CONTINUE  
END  
PRINT 'Too much for the market to bear';  
```  
  
### <a name="b-using-while-in-a-cursor"></a>B. Utilisation de WHILE dans un curseur  
 L'exemple suivant utilise `@@FETCH_STATUS` pour contrôler les activités d'un curseur dans une boucle `WHILE`.  
  
```  
DECLARE Employee_Cursor CURSOR FOR  
SELECT EmployeeID, Title   
FROM AdventureWorks2012.HumanResources.Employee  
WHERE JobTitle = 'Marketing Specialist';  
OPEN Employee_Cursor;  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
   BEGIN  
      FETCH NEXT FROM Employee_Cursor;  
   END;  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-simple-while-loop"></a>C. Boucle While simple  
 Dans l'exemple suivant, si le prix moyen d'un produit est inférieur à `$300`, la boucle `WHILE` double les prix puis sélectionne le prix maximum. Si le prix maximum est inférieur ou égal à `$500`, la boucle `WHILE` redémarre et double de nouveau les prix. Cette boucle continue à doubler les prix jusqu’à ce que le prix maximal soit supérieur à `$500`, puis le programme sort de la boucle `WHILE`.  
  
```  
-- Uses AdventureWorks  
  
WHILE ( SELECT AVG(ListPrice) FROM dbo.DimProduct) < $300  
BEGIN  
    UPDATE dbo.DimProduct  
        SET ListPrice = ListPrice * 2;  
    SELECT MAX ( ListPrice) FROM dbo.DimProduct  
    IF ( SELECT MAX (ListPrice) FROM dbo.DimProduct) > $500  
        BREAK;  
END  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Langage de contrôle de flux &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Curseurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  


