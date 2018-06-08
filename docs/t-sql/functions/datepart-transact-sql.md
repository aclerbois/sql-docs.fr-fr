---
title: DATEPART (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DATEPART_TSQL
- DATEPART
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], DATEPART
- dateparts [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dateparts [SQL Server], datepart
- comparing dates times [SQL Server]
- DATEPART function [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 15f1a5bc-4c0c-4c48-848d-8ec03473e6c1
caps.latest.revision: 57
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: eee173d268af8e18343c286bda29384b2a327860
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datepart-transact-sql"></a>DATEPART (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Retourne un entier qui représente la valeur *datepart* précisée de la *date* spécifiée.
  
Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DATEPART ( datepart , date )  
```  
  
## <a name="arguments"></a>Arguments  
*datepart*  
Partie de *date* (une valeur de date ou d’heure) pour laquelle un **entier** est retourné. Le tableau suivant répertorie tous les arguments *datepart* valides. Les équivalents de variables définis par l'utilisateur ne sont pas valides.
  
|*datepart*|Abréviations|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**isowk**, **isoww**|  
  
*date*  
Expression qui peut être résolue en valeur **time**, **date**, **smalldatetime**, **datetime**, **datetime2** ou **datetimeoffset**. *date* peut être une expression, une expression de colonne, une variable définie par l’utilisateur ou un littéral de chaîne.  
Pour éviter toute ambiguïté, représentez les années à l'aide de quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration de serveur Année de coupure à deux chiffres](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Type de retour  
 **Int**  
  
## <a name="return-value"></a>Valeur retournée  
Chaque *datepart* et ses abréviations retournent la même valeur.
  
La valeur renvoyée dépend de l’environnement de langage défini en utilisant [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) et l’[option de configuration de serveur Configurer la langue par défaut](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) de la connexion. Si *date* est un littéral de chaîne pour certains formats, la valeur renvoyée dépend du format spécifié à l’aide de [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md). SET DATEFORMAT n'affecte pas la valeur de retour lorsque la date est une expression de colonne d'un type de données de date ou d'heure.
  
Le tableau suivant répertorie tous les arguments *datepart* avec les valeurs renvoyées correspondantes pour l’instruction `SELECT DATEPART(datepart,'2007-10-30 12:15:32.1234567 +05:10')`. Le type de données de l’argument *date* est **datetimeoffset(7)**. La valeur retournée **nanosecond***datepart* a une échelle de 9 (0,123456700) et les deux dernières positions sont toujours 00.
  
|*datepart*|Valeur retournée|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|10|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|45|  
|**weekday, dw**| 1|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|310|  
  
## <a name="week-and-weekday-datepart-arguments"></a>Arguments des parties de date semaine et jour ouvrable
Quand *datepart* a la valeur **week** (**wk**, **ww**) ou **weekday** (**dw**), la valeur renvoyée dépend de la valeur définie à l’aide de [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
Le 1er janvier d’une année définit le numéro de départ de **week***datepart*, par exemple, DATEPART (** wk**, 'Jan 1, *xxx*x') = 1, où *xxxx* représente une année.
  
Le tableau suivant répertorie la valeur retournée pour **week** et **weekday***datepart*, pour la valeur '2007-04-21' de chaque argument SET DATEFIRST. Le 1er janvier est un lundi dans l'année 2007. Le 21 avril est un samedi dans l'année 2007. SET DATEFIRST 7, Sunday, est la valeur par défaut pour l'anglais des États-Unis Anglais.
  
|SET DATEFIRST<br /><br /> argument|week<br /><br /> retourné|weekday<br /><br /> retourné|  
|---|---|---|
| 1|16|6|  
|2|17|5|  
|3|17|4|  
|4|17|3|  
|5|17|2|  
|6|17| 1|  
|7|16|7|  
  
## <a name="year-month-and-day-datepart-arguments"></a>Arguments des parties de date année, mois et jour  
Les valeurs retournées pour DATEPART (**year**, *date*), DATEPART (**month**, *date*) et DATEPART (**day**, *date*) sont les mêmes que celles retournées par les fonctions [YEAR](../../t-sql/functions/year-transact-sql.md), [MONTH](../../t-sql/functions/month-transact-sql.md) et [DAY](../../t-sql/functions/day-transact-sql.md), respectivement.
  
## <a name="isoweek-datepart"></a>Partie de date ISO_WEEK  
ISO 8601 inclut le système semaine-date ISO, un système de numérotation pour les semaines. Chaque semaine est associée à l'année dans laquelle jeudi se produit. Par exemple, la semaine 1 de 2004 (2004W01) a eu lieu du lundi 29 décembre 2003 au dimanche 4 janvier 2004. Le numéro de semaine le plus élevé dans une année peut être 52 ou 53. Ce style de numérotation est utilisé en général dans les pays européens, plus rarement ailleurs.
  
Le système de numérotation des différents pays peut ne pas se conformer à la norme ISO. Il existe au moins six possibilités comme illustré dans le tableau suivant
  
|Premier jour de la semaine|La première semaine de l'année contient|Semaines assignées deux fois|Utilisé par/dans|  
|---|---|---|---|
|Dimanche|1er janvier,<br /><br /> Premier samedi,<br /><br /> 1 à 7 jours de l'année|Oui|United States|  
|Lundi|1er janvier,<br /><br /> Premier dimanche,<br /><br /> 1 à 7 jours de l'année|Oui|Majorité de l'Europe et Royaume-Uni|  
|Lundi|4 janvier<br /><br /> Premier jeudi,<br /><br /> 4 à 7 jours de l’année|non|ISO 8601, Norvège et Suède|  
|Lundi|7 janvier,<br /><br /> Premier lundi,<br /><br /> 7 jours de l'année|non||  
|Mercredi|1er janvier,<br /><br /> Premier mardi,<br /><br /> 1 à 7 jours de l'année|Oui||  
|Samedi|1er janvier,<br /><br /> Premier vendredi,<br /><br /> 1 à 7 jours de l'année|Oui||  
  
## <a name="tzoffset"></a>TZoffset  
**TZoffset** (**tz**) est retourné en tant que nombre de minutes (signé). L'instruction suivante retourne un décalage de fuseau horaire de 310 minutes.
  
```sql
SELECT DATEPART (TZoffset, '2007-05-10  00:00:01.1234567 +05:10');  
```  
La valeur TZoffset est restituée comme suit :
- Pour datetimeoffset et datetime2, TZoffset retourne le décalage horaire en minutes, où le décalage pour datetime2 s’élève toujours à 0 minutes.
- Pour les types de données qui peuvent être convertis implicitement en datetimeoffset ou datetime2, à l’exception des autres types de données de date/d’heure, le décalage horaire en minutes est retourné.
- Les paramètres de tous les autres types génèrent une erreur.
  
  
## <a name="smalldatetime-date-argument"></a>Argument date smalldatetime  
Quand *date* a la valeur [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), les secondes sont renvoyées sous la forme 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-a-date-argument"></a>Valeur par défaut retournée pour une partie de date qui ne figure pas dans un argument date  
Si le type de données de l’argument *date* n’a pas l’argument *datepart* spécifié, la valeur par défaut pour ce *datepart* sera renvoyée uniquement lors de la spécification d’un littéral pour *date*.
  
Par exemple, la combinaison année-mois-jour par défaut pour tout type de données **date** est 1900-01-01. L’instruction suivante a des arguments de partie de date pour *datepart*, un argument d’heure pour *date* et retourne `1900, 1, 1, 1, 2`.
  
```sql
SELECT DATEPART(year, '12:10:30.123')  
    ,DATEPART(month, '12:10:30.123')  
    ,DATEPART(day, '12:10:30.123')  
    ,DATEPART(dayofyear, '12:10:30.123')  
    ,DATEPART(weekday, '12:10:30.123');  
```  
  
Si *date* est spécifié comme variable ou colonne de table et que l’argument *datepart* n’est pas spécifié pour le type de données de cette variable ou colonne, l’erreur 9810 est renvoyée. L’exemple de code suivant échoue car la partie année de la date n’est pas valide pour le type de données **time** déclaré pour la variable *@t*.
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATEPART(year, @t);  
```  
  
## <a name="fractional-seconds"></a>Fractions de seconde
Les fractions de seconde sont retournées comme illustré dans les instructions suivantes :
  
```sql
SELECT DATEPART(millisecond, '00:00:01.1234567'); -- Returns 123  
SELECT DATEPART(microsecond, '00:00:01.1234567'); -- Returns 123456  
SELECT DATEPART(nanosecond,  '00:00:01.1234567'); -- Returns 123456700  
```  
  
## <a name="remarks"></a>Notes   
DATEPART peut être utilisé dans la liste de sélection et les clauses WHERE, HAVING, GROUP BY et ORDER BY.
  
Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], DATEPART caste des littéraux de chaîne implicitement en type **datetime2**. Cela signifie que DATEPART ne prend pas en charge le format YDM lorsque la date est transmise en tant que chaîne. Vous devez caster explicitement la chaîne en type **datetime** ou **smalldatetime** pour utiliser le format AJM.
  
## <a name="examples"></a>Exemples  
L'exemple suivant retourne l'année de base. L'année de base est utile pour les calculs de date. Dans l'exemple, la date est spécifiée comme un nombre. Notez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interprète 0 comme représentant le 1er janvier 1900.
  
```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0);  
-- Returns: 1900    1    1 */  
```  
  
L’exemple suivant renvoie la partie jour de la date `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (day,'12/20/1974') FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
--------
20
```  
  
L’exemple suivant renvoie la partie année de la date `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (year,'12/20/1974') FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
--------
1974
```  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
