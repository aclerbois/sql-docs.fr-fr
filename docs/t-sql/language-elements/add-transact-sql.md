---
title: + (Addition) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- add
- +
- +_TSQL
- + (Add)
dev_langs:
- TSQL
helpviewer_keywords:
- addition (+)
- adding numbers
- + (add)
- plus sign (+)
- add operator (+)
ms.assetid: 4ba8baac-5f07-432c-87c5-d23e7011da55
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 44f41f19766120e741e23753ea1b919cf8e25df0
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34582051"
---
# <a name="-addition-transact-sql"></a>+ (Addition) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all_md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Additionne deux nombres. Cet opérateur arithmétique d'addition peut aussi ajouter un nombre de jours à une date.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
expression + expression  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de l’un des types de données de la catégorie numérique, à l’exception du type de données **bit**. Non utilisable avec les types de données **date**, **time**, **datetime2** ou **datetimeoffset**.  
  
## <a name="result-types"></a>Types des résultats  
 Retourne le type de données de l'argument ayant la priorité la plus élevée. Pour plus d’informations, consultez [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-addition-operator-to-calculate-the-total-number-of-hours-away-from-work-for-each-employee"></a>A. Utilisation de l'opérateur d'addition pour calculer le nombre total d'heures d'absence du travail de chaque employé.  
 L’exemple calcule le nombre total d’heures d’absence au travail de chaque employé en additionnant le nombre d’heures de congés et celui des arrêts maladie.  
  
```  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, VacationHours, SickLeaveHours,   
    VacationHours + SickLeaveHours AS 'Total Hours Away'  
FROM HumanResources.Employee AS e  
    JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
ORDER BY 'Total Hours Away' ASC;  
GO  
```  
  
### <a name="b-using-the-addition-operator-to-add-days-to-date-and-time-values"></a>B. Utilisation de l'opérateur d'addition pour ajouter des jours à des valeurs de date et d'heure  
 Cet exemple additionne un nombre de jours à une date de type `datetime`.  
  
```  
  
SET NOCOUNT ON  
DECLARE @startdate datetime, @adddays int;  
SET @startdate = 'January 10, 1900 12:00 AM';  
SET @adddays = 5;  
SET NOCOUNT OFF;  
SELECT @startdate + 1.25 AS 'Start Date',   
   @startdate + @adddays AS 'Add Date';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Start Date                  Add Date
--------------------------- ---------------------------
1900-01-11 06:00:00.000     1900-01-15 00:00:00.000
  
(1 row(s) affected)
 ```  
  
### <a name="c-adding-character-and-integer-data-types"></a>C. Addition de types de données caractère et entier  
 L’exemple suivant additionne une valeur de type **int** et une valeur de caractère en convertissant le type de données caractères en valeur **int**. Si la chaîne **char** contient un caractère non valide, [!INCLUDE[tsql](../../includes/tsql-md.md)] retourne une erreur.  
  
```  
DECLARE @addvalue int;  
SET @addvalue = 15;  
SELECT '125127' + @addvalue;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
-----------------------
125142
  
(1 row(s) affected)
 ```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-the-addition-operator-to-calculate-the-total-number-of-hours-away-from-work-for-each-employee"></a>D : Utilisation de l’opérateur d’addition pour calculer le nombre total d’heures d’absence au travail de chaque employé  
 L’exemple ci-après calcule le nombre total d’heures d’absence au travail de chaque employé en additionnant le nombre d’heures de congés et celui des arrêts maladie, puis trie les résultats dans l’ordre croissant.  
  
```  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, VacationHours, SickLeaveHours,   
    VacationHours + SickLeaveHours AS TotalHoursAway  
FROM DimEmployee  
ORDER BY TotalHoursAway ASC;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [+= &#40;Affectation après addition&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/add-equals-transact-sql.md)   
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  


