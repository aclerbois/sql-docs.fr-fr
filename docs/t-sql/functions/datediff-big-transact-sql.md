---
title: DATEDIFF_BIG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DATEDIFF_BIG
- DATEDIFF_BIG_TSQL
helpviewer_keywords:
- DATEDIFF_BIG function [SQL Server]
- dates [SQL Server]. functions
- date and time [SQL Server], DATEDIFF_BIG
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 19ac1693-3cfa-400d-bf83-20a9cb46599a
caps.latest.revision: 7
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6c0cfefde5609455f53d2afcb33659cd5c1d99bb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datediffbig-transact-sql"></a>DATEDIFF_BIG (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

Retourne le nombre (entier très grand signé) des limites *datepart* spécifiées, traversées entre les valeurs *startdate* et *enddate* spécifiées.
  
Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
  
DATEDIFF_BIG ( datepart , startdate , enddate )  
```  
  
## <a name="arguments"></a>Arguments  
*datepart*  
Partie de *startdate* et *enddate* qui spécifie le type de limite traversée. Le tableau suivant répertorie tous les arguments *datepart* valides. Les équivalents de variables définis par l'utilisateur ne sont pas valides.
  
|*datepart*|Abréviations|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
  
*startdate*  
Expression qui peut être résolue en valeur **time**, **date**, **smalldatetime**, **datetime**, **datetime2** ou **datetimeoffset**. *date* peut être une expression, une expression de colonne, une variable définie par l’utilisateur ou un littéral de chaîne. La valeur *startdate* est soustraite de *enddate*.  
Pour éviter toute ambiguïté, représentez les années à l'aide de quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration de serveur Année de coupure à deux chiffres](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
*enddate*  
Consultez *startdate*.
  
## <a name="return-type"></a>Type de retour  
 Signé   
        **bigint**  
  
## <a name="return-value"></a>Valeur retournée  
Retourne le nombre (bigint signé) des limites datepart spécifiées, traversées entre les valeurs startdate et enddate spécifiées.
-   Chaque *datepart* et ses abréviations retournent la même valeur.  
  
Si la valeur retournée est hors limites pour **bigint** (-9 223 372 036 854 775 808 à 9 223 372 036 854 775 807), une erreur est retournée. Pour **millisecond**, la différence maximale entre *startdate* et *enddate* est de 24 jours, 20 heures, 31 minutes et 23,647 secondes. Pour **second**, la différence maximale est de 68 ans.
  
Si *startdate* et *enddate* se voient tous les deux assigner uniquement une valeur d’heure et que *datepart* n’est pas un argument *datepart* d’heure, 0 est renvoyé.
  
Un composant de décalage de fuseau horaire de *startdate* ou *endate* n’est pas utilisé pour calculer la valeur renvoyée.
  
Dans la mesure où [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) n’offre qu’une précision à la minute, lorsque vous utilisez une valeur **smalldatetime** pour *startdate* ou *enddate*, les secondes et millisecondes ont toujours la valeur 0 dans la valeur de retour.
  
Si une valeur d'heure uniquement est assignée à une variable d'un type de données date, la valeur de la partie date manquante est égale à la valeur par défaut : 1900-01-01. Si une valeur de date uniquement est assignée à une variable d'un type de données date ou heure, la valeur de la partie heure manquante est égale à la valeur par défaut : 00-00-00. Si *startdate* ou *enddate* a uniquement une partie heure et que l’autre a uniquement une partie date, les parties heure et date manquantes sont égales aux valeurs par défaut.
  
Si *startdate* et *enddate* sont de types de données date différents et que l’un a plus de parties heure ou une meilleure précision en fractions de seconde que l’autre, les parties manquantes de l’autre prennent la valeur 0.
  
## <a name="datepart-boundaries"></a>Limites de datepart
Les instructions suivantes ont les mêmes *startdate* et *endate*. Ces dates sont adjacentes et ont une différence horaire de .0000001 seconde. La différence entre les *startdate* et *endate* dans chaque instruction traverse une limite d’heure ou de calendrier de son *datepart*. Chaque instruction retourne 1. Si des années différentes sont utilisées pour cet exemple et que les paramètres *startdate* et *endate* figurent tous les deux dans la même semaine de calendrier, la valeur de retour pour **week** est 0.

```sql
SELECT DATEDIFF_BIG(year, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(quarter, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(month, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(dayofyear, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(day, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(week, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(hour, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(minute, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(second, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```
  
## <a name="remarks"></a>Notes   
DATEDIFF_BIG peut être utilisé dans la liste de sélection et les clauses WHERE, HAVING, GROUP BY et ORDER BY.
  
DATEDIFF_BIG convertit des littéraux de chaîne implicitement en type **datetime2**. Cela signifie que DATEDIFF_BIG ne prend pas en charge le format YDM lorsque la date est transmise en tant que chaîne. Vous devez convertir explicitement la chaîne en type **datetime** ou **smalldatetime** pour utiliser le format YDM.
  
La spécification de SET DATEFIRST n’a aucun effet sur DATEDIFF_BIG. DATEDIFF_BIG utilise toujours le dimanche comme premier jour de la semaine pour que la fonction soit déterministe.
  
## <a name="examples"></a>Exemples  
Les exemples suivants utilisent différents types d’expressions comme arguments pour les paramètres *startdate* et *enddate*.
  
### <a name="specifying-columns-for-startdate-and-enddate"></a>Spécification de colonnes pour les dates de début et de fin  
L'exemple suivant calcule le nombre de limites de jour qui sont traversées entre des dates de deux colonnes dans une table.
  
```sql
CREATE TABLE dbo.Duration  
    (  
    startDate datetime2  
    ,endDate datetime2  
    );  
INSERT INTO dbo.Duration(startDate,endDate)  
    VALUES('2007-05-06 12:10:09','2007-05-07 12:10:09');  
SELECT DATEDIFF_BIG(day,startDate,endDate) AS 'Duration'  
FROM dbo.Duration;  
-- Returns: 1  
```  
  
Pour de nombreux exemples supplémentaires, consultez les exemples étroitement liés dans [DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md).
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md)
  
  
