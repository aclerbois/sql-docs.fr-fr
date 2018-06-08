---
title: DATENAME (Transact-SQL) | Microsoft Docs
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
- DATENAME_TSQL
- DATENAME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATENAME function [SQL Server
- functions [SQL Server], time
- functions [SQL Server], date and time
- dateparts [SQL Server], datename
- date and time [SQL Server], DATENAME
- comparing dates times [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 11855b56-c554-495d-aad4-ba446990153b
caps.latest.revision: 59
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8cf093318d49fcf6d28777cf4381dead7110bcab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datename-transact-sql"></a>DATENAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Retourne une chaîne de caractères qui représente la partie *datepart* de la *date* spécifiée
  
Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DATENAME ( datepart , date )  
```  
  
## <a name="arguments"></a>Arguments  
*datepart*  
Partie de la *date* à renvoyer. Le tableau suivant répertorie tous les arguments *datepart* valides. Les équivalents de variables définis par l'utilisateur ne sont pas valides.
  
|*datepart*|Abréviations|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**weekday**|**dw, w**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**ISOWK, ISOWW**|  
  
*date*  
Expression qui peut être résolue en valeur **time**, **date**, **smalldatetime**, **datetime**, **datetime2** ou **datetimeoffset**. *date* peut être une expression, une expression de colonne, une variable définie par l’utilisateur ou un littéral de chaîne.  
Pour éviter toute ambiguïté, représentez les années à l'aide de quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration de serveur d’année de coupure à deux chiffres](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Type de retour  
**nvarchar**
  
## <a name="return-value"></a>Valeur retournée  
  
-   Chaque *datepart* et ses abréviations retournent la même valeur.  
  
La valeur renvoyée dépend de l’environnement de langage défini en utilisant [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) et l’[option de configuration de serveur Configurer la langue par défaut](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) de la connexion. La valeur renvoyée dépend de [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) si *date* est un littéral de chaîne de certains formats. SET DATEFORMAT n'affecte pas la valeur de retour lorsque la date est une expression de colonne d'un type de données de date ou d'heure.
  
Lorsque le paramètre *date* a un argument de type de données **date**, la valeur renvoyée dépend du paramètre spécifié en utilisant [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
## <a name="tzoffset-datepart-argument"></a>Argument de la partie de date TZoffset  
Si l’argument *datepart* est de type **TZoffset** (**tz**) et que l’argument *date* n’a aucun décalage de fuseau horaire, la valeur 0 est renvoyée.
  
## <a name="smalldatetime-date-argument"></a>Argument date smalldatetime  
Quand *date* a la valeur [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), les secondes sont renvoyées sous la forme 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-the-date-argument"></a>Valeur par défaut retournée pour une partie de date qui ne figure pas dans l'argument date  
Si le type de données de l’argument *date* n’a pas l’argument *datepart* spécifié, la valeur par défaut pour ce *datepart* sera renvoyée uniquement lors de la spécification d’un littéral pour *date*.
  
Par exemple, la combinaison année-mois-jour par défaut pour tout type de données **date** est 1900-01-01. L’instruction suivante a des arguments de partie de date pour *datepart*, un argument d’heure pour *date* et retourne `1900, January, 1, 1, Monday`.
  
```sql
SELECT DATENAME(year, '12:10:30.123')  
    ,DATENAME(month, '12:10:30.123')  
    ,DATENAME(day, '12:10:30.123')  
    ,DATENAME(dayofyear, '12:10:30.123')  
    ,DATENAME(weekday, '12:10:30.123');  
```  
  
Si *date* est spécifié comme variable ou colonne de table et que l’argument *datepart* n’est pas spécifié pour le type de données de cette variable ou colonne, l’erreur 9810 est renvoyée. L’exemple de code suivant échoue car la partie année de la date n’est pas valide pour le type de données **time** déclaré pour la variable *@t*.
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATENAME(year, @t);  
```  
  
## <a name="remarks"></a>Notes   
DATENAME peut être utilisé dans la liste de sélection, WHERE, HAVING, GROUP BY et les clauses ORDER BY.
  
Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], DATENAME convertit des littéraux de chaîne implicitement en type **datetime2**. Cela signifie que DATENAME ne prend pas en charge le format YDM lorsque la date est transmise en tant que chaîne. Vous devez caster explicitement la chaîne en type **datetime** ou **smalldatetime** pour utiliser le format AJM.
  
## <a name="examples"></a>Exemples  
L'exemple suivant retourne les parties de la date spécifiée.
  
`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valeur retournée|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Octobre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Mardi|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|310|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

L'exemple suivant retourne les parties de la date spécifiée.
  
```sql
SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valeur retournée|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Octobre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Mardi|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|310|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

