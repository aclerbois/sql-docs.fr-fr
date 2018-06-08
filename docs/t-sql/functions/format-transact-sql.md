---
title: FORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- FORMAT_TSQL
- FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- FORMAT function
ms.assetid: dad6f24c-b8d9-4dbe-a561-9b167b8f20c8
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a0c157383e8e21d85c756d9fb64cd0b23bad6a33
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="format-transact-sql"></a>FORMAT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Retourne une valeur mise en forme avec la culture facultative et le format spécifiés dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Utilisez la fonction FORMAT pour la mise en forme comme chaînes de valeurs de date/heure et de valeurs numériques compatibles avec les paramètres régionaux. Pour les conversions de type de données général, continuez à utiliser CAST ou CONVERT.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
FORMAT ( value, format [, culture ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *value*  
 Expression d'un type de données pris en charge à mettre en forme. Pour obtenir la liste des types valides, consultez la table dans la section Notes ci-dessous.  
  
 *format*  
 Modèle de format **nvarchar**.  
  
 L’argument *format* doit contenir une chaîne de format .NET Framework valide, comme chaîne de format standard (par exemple, « C » ou « D ») ou comme modèle de caractères personnalisés pour les dates et les valeurs numériques (par exemple, « MMMM JJ, aaaa (jjjj) »). La mise en forme composite n'est pas prise en charge. Pour obtenir une explication complète de ces modèles de mise en forme, consultez la documentation du .NET Framework sur la mise en forme des chaînes en général, sur les formats de date et d’heure personnalisés et sur les formats de nombres personnalisés. La rubrique « [Types de mise en forme](http://go.microsoft.com/fwlink/?LinkId=211776) » constitue un bon point de départ.  
  
 *culture*  
 Argument **nvarchar** facultatif spécifiant une culture.  
  
 Si l’argument *culture* n’est pas fourni, la langue de la session en cours est utilisée. Cette langue est défini implicitement, ou explicitement à l'aide de l'instruction SET LANGAGE. *culture* accepte n’importe quelle culture prise en charge par le .NET Framework comme argument ; elle n’est pas limitée aux langues explicitement prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l’argument *culture* n’est pas valide, FORMAT génère une erreur.  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar** ou NULL  
  
 La longueur de la valeur renvoyée est déterminée par *format*.  
  
## <a name="remarks"></a>Notes   
 FORMAT retourne NULL pour des erreurs autres qu’un argument *culture* non *valide*. Par exemple, la valeur NULL est renvoyée si la valeur spécifiée dans *format* n’est pas valide.  
 
 La fonction FORMAT est non déterministe.   
  
 FORMAT repose sur la présence du CLR (Common Langage Runtime) .NET Framework.  
  
 Cette fonction ne peut pas être exécutée à distance car elle dépend de la présence du CLR. L’exécution à distance d’une fonction qui nécessite le CLR peut provoquer une erreur sur le serveur distant.  
  
 FORMAT repose sur les règles de mise en forme CLR, qui exigent l’échappement des deux-points et des points. Par conséquent, lorsque la chaîne format (second paramètre) contient un signe deux-points ou point, le signe deux-points ou point doit être précédé d’une barre oblique inverse lorsqu’une valeur d’entrée (premier paramètre) est du type de données **time**. Consultez [D. FORMAT avec types de données time](#ExampleD).  
  
 Le tableau suivant répertorie les types de données acceptables pour l’argument *value*, ainsi que leurs types équivalents de mappage .NET Framework.  
  
|Catégorie|Type|Type .NET|  
|--------------|----------|---------------|  
|Numérique|bigint|Int64|  
|Numérique|INT|Int32|  
|Numérique|SMALLINT|Int16|  
|Numérique|TINYINT|Byte|  
|Numérique|decimal|SqlDecimal|  
|Numérique|NUMERIC|SqlDecimal|  
|Numérique|FLOAT|Double|  
|Numérique|REAL|Unique|  
|Numérique|SMALLMONEY|Décimal|  
|Numérique|money|Décimal|  
|Date et heure|Date|DateTime|  
|Date et heure|time|TimeSpan|  
|Date et heure|DATETIME|DateTime|  
|Date et heure|smalldatetime|DateTime|  
|Date et heure|datetime2|DateTime|  
|Date et heure|datetimeoffset|DateTimeOffset|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-format-example"></a>A. Exemple FORMAT simple  
 L'exemple suivant retourne une date simple mise en forme pour différentes cultures.  
  
```sql  
DECLARE @d DATETIME = '10/01/2011';  
SELECT FORMAT ( @d, 'd', 'en-US' ) AS 'US English Result'  
      ,FORMAT ( @d, 'd', 'en-gb' ) AS 'Great Britain English Result'  
      ,FORMAT ( @d, 'd', 'de-de' ) AS 'German Result'  
      ,FORMAT ( @d, 'd', 'zh-cn' ) AS 'Simplified Chinese (PRC) Result';   
  
SELECT FORMAT ( @d, 'D', 'en-US' ) AS 'US English Result'  
      ,FORMAT ( @d, 'D', 'en-gb' ) AS 'Great Britain English Result'  
      ,FORMAT ( @d, 'D', 'de-de' ) AS 'German Result'  
      ,FORMAT ( @d, 'D', 'zh-cn' ) AS 'Chinese (Simplified PRC) Result';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
US English Result Great Britain English Result  German Result Simplified Chinese (PRC) Result  
----------------  ----------------------------- ------------- -------------------------------------  
10/1/2011         01/10/2011                    01.10.2011    2011/10/1  
  
(1 row(s) affected)  
  
US English Result            Great Britain English Result  German Result                    Chinese (Simplified PRC) Result  
---------------------------- ----------------------------- -----------------------------  ---------------------------------------  
Saturday, October 01, 2011   01 October 2011               Samstag, 1. Oktober 2011        2011年10月1日  
  
(1 row(s) affected)  
```  
  
### <a name="b-format-with-custom-formatting-strings"></a>B. FORMAT avec chaînes de format personnalisées  
 L'exemple suivant montre la mise en forme des valeurs numériques en spécifiant un format personnalisé. L’exemple suppose que la date actuelle est le 27 septembre 2012. Pour plus d’informations sur ces formats et d’autres formats personnalisés, consultez [Chaînes de format numérique personnalisées](http://msdn.microsoft.com/library/0c899ak8.aspx).  
  
```sql  
DECLARE @d DATETIME = GETDATE();  
SELECT FORMAT( @d, 'dd/MM/yyyy', 'en-US' ) AS 'DateTime Result'  
       ,FORMAT(123456789,'###-##-####') AS 'Custom Number Result';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
DateTime Result  Custom Number Result  
--------------   --------------------  
27/09/2012       123-45-6789  
  
(1 row(s) affected)  
```  
  
### <a name="c-format-with-numeric-types"></a>C. FORMAT avec types numériques  
 L’exemple suivant retourne 5 lignes de la table **Sales.CurrencyRate** dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La colonne **EndOfDateRate** est stockée comme type **money** dans la table. Dans cet exemple, la colonne est retournée dépourvue de mise en forme, puis mise en forme en spécifiant les types de formats Number, General et Currency .NET. Pour plus d’informations sur ces formats et d’autres formats numériques, consultez [Chaînes de format numérique standard](http://msdn.microsoft.com/library/dwhawy9k.aspx).  
  
```sql  
SELECT TOP(5)CurrencyRateID, EndOfDayRate  
            ,FORMAT(EndOfDayRate, 'N', 'en-us') AS 'Number Format'  
            ,FORMAT(EndOfDayRate, 'G', 'en-us') AS 'General Format'  
            ,FORMAT(EndOfDayRate, 'C', 'en-us') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1.00            1.0002          $1.00  
2              1.55          1.55            1.5500          $1.55  
3              1.9419        1.94            1.9419          $1.94  
4              1.4683        1.47            1.4683          $1.47  
5              8.2784        8.28            8.2784          $8.28  
  
(5 row(s) affected)  
  
```  
  
 Cet exemple spécifie la culture allemande (de-de).  
  
```sql  
SELECT TOP(5)CurrencyRateID, EndOfDayRate  
      ,FORMAT(EndOfDayRate, 'N', 'de-de') AS 'Numeric Format'  
      ,FORMAT(EndOfDayRate, 'G', 'de-de') AS 'General Format'  
      ,FORMAT(EndOfDayRate, 'C', 'de-de') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
```  
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1,00            1,0002          1,00 €  
2              1.55          1,55            1,5500          1,55 €  
3              1.9419        1,94            1,9419          1,94 €  
4              1.4683        1,47            1,4683          1,47 €  
5              8.2784        8,28            8,2784          8,28 €  
  
 (5 row(s) affected)  
```  
  
###  <a name="ExampleD"></a> D. FORMAT avec types de données time  
 FORMAT retourne la valeur NULL dans ces cas, car `.` et `:` ne sont pas échappés.  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh.mm');   --> returns NULL  
SELECT FORMAT(cast('07:35' as time), N'hh:mm');   --> returns NULL  
```  
  
 Format retourne une chaîne mise en forme, car `.` et `:` sont échappés.  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh\.mm');  --> returns 07.35  
SELECT FORMAT(cast('07:35' as time), N'hh\:mm');  --> returns 07:35  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
 [STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
  
  
