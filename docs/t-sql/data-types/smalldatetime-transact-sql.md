---
title: smalldatetime (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 7/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- smalldatetime_TSQL
- smalldatetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- smalldatetime data type [SQL Server]
- dates [SQL Server], data types
- date and time [SQL Server], smalldatetime
- data types [SQL Server], date and time
ms.assetid: 68b74610-d54c-4c8e-b4b2-7e3747546ee0
caps.latest.revision: 50
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a9fff2a0066840b8adcb6afd80cb901fceb8b95f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="smalldatetime-transact-sql"></a>smalldatetime (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Définit une date qui est associée à une heure de la journée. L'heure est basée sur une journée de 24 heures, les secondes ayant toujours la valeur zéro (:00) et sans fractions de seconde.
  
> [!NOTE]  
>  Utilisez les types de données **time**, **date**, **datetime2** et **datetimeoffset** pour une nouvelle tâche. Ces types s'alignent sur la norme SQL. Ils sont plus portables. **time**, **datetime2** et **datetimeoffset** offrent une meilleure précision à la seconde. **datetimeoffset** fournit la prise en charge des fuseaux horaires pour les applications globalement déployées.  
  
## <a name="smalldatetime-description"></a>Description de smalldatetime
  
|||  
|-|-|  
|Syntaxe|**smalldatetime**|  
|Utilisation|DECLARE @MySmalldatetime **smalldatetime**<br /><br /> CREATE TABLE Table1 ( Column1 **smalldatetime** )|  
|Formats de littéraux de chaîne par défaut<br /><br /> (utilisé pour le client de bas niveau)|Non applicable|  
|Plage de dates|1900-01-01 à 2079-06-06<br /><br /> Du 1er janvier 1900 au 6 juin 2079|  
|Plage temporelle|00:00:00 à 23:59:59<br /><br /> 2007-05-09 23:59:59 sera arrondi à<br /><br /> 2007-05-10 00:00:00|  
|Plages d'éléments|AAAA comprend quatre chiffres, entre 1900 et 2079, qui représentent une année.<br /><br /> MM comprend deux chiffres, entre 01 et 12, qui représentent un mois de l’année spécifiée.<br /><br /> DD comprend deux chiffres, entre 01 et 31 selon le mois, qui représentent un jour du mois spécifié.<br /><br /> hh comprend deux chiffres, entre 00 et 23, qui représentent l'heure.<br /><br /> mm comprend deux chiffres, entre 00 et 59, qui représentent la minute.<br /><br /> ss comprend deux chiffres, entre 00 et 59, qui représentent la seconde. Les valeurs inférieures ou égales à 29,998 sont arrondies à la minute inférieure ; les valeurs supérieures ou égales à 29,999 sont arrondies à la minute supérieure.|  
|Longueur de caractère|19 positions au maximum|  
|Taille de stockage|4 octets, fixe.|  
|Précision|Une minute|  
|Valeur par défaut|1900-01-01 00:00:00|  
|Calendrier|Grégorien<br /><br /> (N'inclut pas la plage complète des années.)|  
|Précision à la fraction de seconde définie par l'utilisateur|non|  
|Prise en charge et conservation du décalage de fuseau horaire|non|  
|Prise en charge de l'heure d'été|non|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformité ANSI et ISO 8601  
**smalldatetime** n’est pas conforme à ANSI ni ISO 8601.
  
## <a name="converting-date-and-time-data"></a>Conversion de données date et time
Lorsque vous effectuez une conversion vers des types de données date et heure, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette toutes les valeurs qu'il ne peut identifier comme dates ou heures. Pour plus d’informations sur l’utilisation des fonctions CAST et CONVERT avec des données date et time, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-smalldatetime-to-other-date-and-time-types"></a>Conversion de smalldatetime en d’autres types de données date et time
Cette section décrit ce qui se produit quand un type de données **smalldatetime** est converti en d’autres types de données date et time.
  
Dans le cas d’une conversion en **date**, l’année, le mois et le jour sont copiés. Le code suivant montre les résultats de la conversion d'une valeur `smalldatetime` en valeur `date`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @date date = @smalldatetime  
  
SELECT @smalldatetime AS '@smalldatetime', @date AS 'date';  
  
--Result  
--@smalldatetime          date  
------------------------- ----------  
--1955-12-13 12:43:00     1955-12-13  
--  
--(1 row(s) affected)  
```  
  
Dans le cas d’une conversion en **time(n)**, les heures, minutes et secondes sont copiées. Les fractions de seconde sont définies sur 0. Le code suivant montre les résultats de la conversion d'une valeur `smalldatetime` en valeur `time(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @time time(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @time AS 'time';  
  
--Result  
--@smalldatetime          time  
------------------------- -------------  
--1955-12-13 12:43:00     12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
Dans le cas d’une conversion en **datetime**, la valeur **smalldatetime** est copiée dans la valeur **datetime**. Les fractions de seconde sont définies sur 0. Le code suivant montre les résultats de la conversion d'une valeur `smalldatetime` en valeur `datetime`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime AS 'datetime';  
  
--Result  
--@smalldatetime          datetime  
------------------------- -----------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.000  
--  
--(1 row(s) affected)  
```  
  
Dans le cas d’une conversion en **datetimeoffset(n)**, la valeur **smalldatetime** est copiée dans la valeur **datetimeoffset(n)**. Les fractions de seconde sont définies sur 0 et le décalage de fuseau horaire est défini sur +00:0. Le code suivant montre les résultats de la conversion d'une valeur `smalldatetime` en valeur `datetimeoffset(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetimeoffset datetimeoffset(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetimeoffset AS 'datetimeoffset(4)';  
  
--Result  
--@smalldatetime          datetimeoffset(4)  
------------------------- ------------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000 +00:0  
--  
--(1 row(s) affected)  
```  
  
Dans le cas d’une conversion en **datetime2(n)**, la valeur **smalldatetime** est copiée dans la valeur **datetime2(n)**. Les fractions de seconde sont définies sur 0. Le code suivant montre les résultats de la conversion d'une valeur `smalldatetime` en valeur `datetime2(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime2 datetime2(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime2 AS ' datetime2(4)';  
  
--Result  
--@smalldatetime           datetime2(4)  
------------------------- ------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-casting-string-literals-with-seconds-to-smalldatetime"></a>A. Conversion de littéraux de chaîne avec secondes en smalldatetime  
L'exemple suivant compare la conversion de secondes dans les littéraux de chaîne en `smalldatetime`.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29'     AS smalldatetime)  
    ,CAST('2007-05-08 12:35:30'     AS smalldatetime)  
    ,CAST('2007-05-08 12:59:59.998' AS smalldatetime);  
```  
  
|Entrée|Sortie|  
|---|---|
|2007-05-08 12:35:29|2007-05-08 12:35:00|  
|2007-05-08 12:35:30|2007-05-08 12:36:00|  
|2007-05-08 12:59:59.998|2007-05-08 13:00:00|  
  
### <a name="b-comparing-date-and-time-data-types"></a>B. Comparaison de types de données date et time  
L'exemple suivant compare les résultats de la conversion d'une chaîne en chaque type de données de date et d'heure.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
|Type de données|Sortie|  
|---|---|
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
