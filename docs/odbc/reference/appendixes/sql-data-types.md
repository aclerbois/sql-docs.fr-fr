---
title: Types de données SQL | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL data types [ODBC]
- SQL data types [ODBC], about SQL data types
- data types [ODBC], SQL data types
ms.assetid: 1b22f985-f5e4-4779-87eb-e43329a442b1
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ba122ccbc873603b434a8541fe44aee10a5104e0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-data-types"></a>Types de données SQL
Chaque SGBD définit ses propres types SQL. Chaque pilote ODBC expose uniquement ces types de données SQL qui définit des SGBD associé. Plus d’informations sur la façon dont un pilote mappe les types DBMS SQL pour les identificateurs de type défini par ODBC de SQL et la façon dont un pilote mappe les types de DBMS SQL à ses propres identificateurs de type spécifiques au pilote SQL est retourné via un appel à **SQLGetTypeInfo**. Un pilote retourne également les types de données SQL lors de la description des types de données des colonnes et des paramètres via des appels de **SQLColAttribute**, **SQLColumns**, **SQLDescribeCol**, **SQLDescribeParam**, **SQLProcedureColumns**, et **SQLSpecialColumns**.  
  
> [!NOTE]  
>  Les types de données SQL sont contenus dans les champs SQL_DESC_ CONCISE_TYPE, SQL_DESC_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE des descripteurs de mise en œuvre. Caractéristiques des types de données SQL sont contenues dans les champs SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH et SQL_DESC_OCTET_LENGTH des descripteurs de mise en œuvre. Pour plus d’informations, consultez [les identificateurs de Type de données et les descripteurs de](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) plus loin dans cette annexe.  
  
 Une source de données et le pilote donnée ne gèrent pas nécessairement tous les types de données SQL qui sont définis dans cette annexe. Prise en charge d’un pilote pour les types de données SQL varie selon le niveau de SQL-92 le pilote est conforme à. Pour déterminer le niveau de prise en charge par le pilote de la grammaire SQL-92, une application appelle **SQLGetInfo** avec le type d’informations SQL_SQL_CONFORMANCE. En outre, une source de données et le pilote donnée peut-être prendre en charge les types de données SQL supplémentaires, spécifiques au pilote. Pour déterminer quelles données types un pilote prend en charge, une application appelle **SQLGetTypeInfo**. Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote. Pour plus d’informations sur les types de données dans une source de données spécifique, consultez la documentation pour cette source de données.  
  
> [!IMPORTANT]  
>  Les tables dans l’ensemble de cette annexe sont uniquement des recommandations et afficher utilisé en général, les noms, plages et les limites des types de données SQL. Une source de données peut prendre en charge uniquement certains des types de données répertoriés, et les caractéristiques des types de données pris en charge peuvent différer de ceux répertoriés.  
  
 Le tableau suivant répertorie les identificateurs de type SQL valides pour tous les types de données SQL. Le tableau répertorie également le nom et la description du type de données correspondant à partir de SQL-92 (le cas échéant).  
  
|Identificateur de type SQL [1]|Type de données SQL<br /><br /> Type [2]|Description de type standard|  
|------------------------------|------------------------------------|------------------------------|  
|SQL_CHAR|CHAR (*n*)|Chaîne de longueur fixe de caractères *n*.|  
|SQL_VARCHAR|VARCHAR (*n*)|Chaîne de caractères de longueur variable avec une longueur maximale de la chaîne *n*.|  
|SQL_LONGVARCHAR|LONG VARCHAR|Données de caractères de longueur variable. Longueur maximale est la source de données. [9]|  
|SQL_WCHAR|WCHAR (*n*)|Chaîne de caractères Unicode de longueur fixe *n*|  
|SQL_WVARCHAR|VARWCHAR (*n*)|Chaîne de caractères de longueur variable Unicode avec une longueur de chaîne maximale *n*|  
|SQL_WLONGVARCHAR|LONGWVARCHAR|Données Unicode de longueur variable. La longueur maximale est la source de données|  
|SQL_DECIMAL|DECIMAL (*p*,*s*)|Signé, valeur numérique exacte avec une précision d’au moins *p* et l’échelle *s.* (La précision maximale est définie par le pilote). (1 < = *p* < = 15 ; *s* <= *p*). [ 4]|  
|SQL_NUMERIC|NUMÉRIQUE (*p*,*s*)|Signé, valeur numérique exacte avec une précision *p* et l’échelle *s* (1 < = *p* < = 15 ; *s* <= *p*). [ 4]|  
|SQL_SMALLINT|SMALLINT|Valeur numérique exacte avec une précision de 5 et une échelle 0 (signée : – 32 768 et < = *n* < = 32 767, non signé : 0 < = *n* < = 65 535) [3].|  
_INTEGER|INTEGER|Valeur numérique exacte avec une précision de 10 et une échelle 0 (signée : – 2 [31] < = *n* < = 2 [31] – 1, non signé : 0 < = *n* < = 2 [32] – 1) [3].|  
|SQL_REAL|REAL|Signé, valeur numérique approximative avec une précision binaire de 24 (zéro ou valeur absolue 10 [–38] à 10[38]).|  
|SQL_FLOAT|FLOAT (*p*)|Signé, valeur numérique approximative avec une précision binaire d’au moins *p*. (La précision maximale est définie par le pilote). [5]|  
|SQL_DOUBLE|DOUBLE PRECISION|Signé, valeur numérique approximative avec une précision binaire de 53 (zéro ou valeur absolue 10 [–308] à 10[308]).|  
|SQL_BIT|BIT|Données binaires de transmission unique. [8]|  
|SQL_TINYINT|TINYINT|Valeur numérique exacte avec une précision de 3 et une échelle 0 (signée : – 128 < = *n* < = 127, non signée : 0 < = *n* < = 255) [3].|  
_BIGINT|bigint|Valeur numérique exacte avec une précision 19 (si signée) ou 20 (si non signée) et une échelle 0 (signée : – 2 [63] < = *n* < = 2 [63] – 1, non signé : 0 < = *n* < = 2 [64] – 1) [3], [9].|  
|SQL_BINARY|BINAIRE (*n*)|Données binaires de longueur fixe *n*. [ 9]|  
|SQL_VARBINARY|VARBINARY (*n*)|Données binaires de longueur variable de la longueur maximale de *n*. La valeur maximale est définie par l’utilisateur. [9]|  
|SQL_LONGVARBINARY|VARBINARY LONG|Données binaires de longueur variable. Longueur maximale est la source de données. [9]|  
|SQL_TYPE_DATE [6]|DATE|Année, mois et champs jour conformes aux règles du calendrier grégorien. (Consultez [contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md), plus loin dans cette annexe.)|  
|SQL_TYPE_TIME [6]|HEURE (*p*)|Heure, minute et seconde champs, avec les valeurs valides pour les heures de 00 à 23, les valeurs valides de 00 à 59 minutes et les valeurs valides pour les secondes de 00 à 61. Précision *p* indique la précision en secondes.|  
|SQL_TYPE_TIMESTAMP [6]|HORODATEUR (*p*)|Year, month, jour, heure, minute et seconde champs, avec les valeurs valides sont définis pour les types de données DATE et d’heure.|  
_TYPE_UTCDATETIME|UTCDATETIME|Champs année, mois, jour, heure, minute, seconde, utchour et utcminute. Les champs utchour et utcminute ont la précision de 1/10 microsecondes.|  
|SQL_TYPE_UTCTIME|UTCTIME|Champs heure, minute, seconde, utchour et utcminute. Les champs utchour et utcminute ont la précision de 1/10 microsecondes...|  
|SQL_INTERVAL_MONTH [7]|MOIS de l’intervalle (*p*)|Nombre de mois entre deux dates ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_YEAR [7]|ANNÉE de l’intervalle (*p*)|Nombre d’années entre deux dates ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_YEAR_TO_MONTH [7]|ANNÉE de l’intervalle (*p*) mois|Nombre d’années et les mois entre deux dates ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_DAY [7]|JOURS d’intervalle (*p*)|Nombre de jours entre deux dates ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_HOUR [7]|HEURE de l’intervalle (*p*)|Nombre d’heures entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_MINUTE [7]|MINUTES d’intervalle (*p*)|Nombre de minutes entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_SECOND [7]|INTERVALLE de deuxième (*p*,*q*)|Nombre de secondes entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête et *q* correspond à la précision de secondes d’intervalle.|  
_INTERVAL_DAY_TO_HOUR [7]|JOURS d’intervalle (*p*) à l’heure|Nombre de jours/heures entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_DAY_TO_MINUTE [7]|JOURS d’intervalle (*p*) à la MINUTE|Nombre de jours/heures/minutes entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_DAY_TO_SECOND [7]|JOURS d’intervalle (*p*) à la seconde (*q*)|Nombre de jours/heures/minutes/secondes entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête et *q* correspond à la précision de secondes d’intervalle.|  
|SQL_INTERVAL_HOUR_TO_MINUTE [7]|HEURE de l’intervalle (*p*) à la MINUTE|Nombre d’heures/minutes entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête.|  
|SQL_INTERVAL_HOUR_TO_SECOND [7]|HEURE de l’intervalle (*p*) à la seconde (*q*)|Nombre d’heures/minutes/secondes entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête et *q* correspond à la précision de secondes d’intervalle.|  
_INTERVAL_MINUTE_TO_SECOND [7]|MINUTES d’intervalle (*p*) à la seconde (*q*)|Nombre de minutes/secondes entre deux dates et des heures ; *p* est l’intervalle de précision d’en-tête et *q* correspond à la précision de secondes d’intervalle.|  
|SQL_GUID|GUID|GUID de longueur fixe.|  
  
 [1] Ceci est la valeur retournée dans la colonne DATA_TYPE par un appel à **SQLGetTypeInfo**.  
  
 [2] Ceci est la valeur retournée dans la colonne nom et créer des paramètres par un appel à **SQLGetTypeInfo**. La colonne de nom renvoie la désignation — par exemple, CHAR, tandis que la colonne de créer un PARAMS retourne une liste séparée par des virgules des paramètres de création telles que la précision, échelle et longueur.  
  
 [3], une application utilise **SQLGetTypeInfo** ou **SQLColAttribute** pour déterminer si un type de données particulière ou une colonne particulière dans un jeu de résultats n’est pas signée.  
  
 [4] types de données SQL_DECIMAL et SQL_NUMERIC diffèrent uniquement par leur précision. La précision d’une valeur décimale (*p*,*s*) est une implémentation définie après la virgule qui est non inférieure à *p*, alors que la précision d’une valeur numérique (*p*,*s*) est exactement égal à *p*.  
  
 [5] en fonction de l’implémentation, la précision de SQL_FLOAT peut être 24 ou 53 : s’il s’agit de 24, le type de données SQL_FLOAT est le même que SQL_REAL ; s’il est 53, le type de données SQL_FLOAT est le même que SQL_DOUBLE.  
  
 [6] dans ODBC 3 *.x*, les types de données SQL date, time et timestamp sont SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP, respectivement ; dans ODBC 2. *x*, les types de données sont SQL_DATE, SQL_TIME et SQL_TIMESTAMP.  
  
 [7] pour plus d’informations sur les types de données SQL intervalle, consultez le [Types de données Interval](../../../odbc/reference/appendixes/interval-data-types.md) section, plus loin dans cette annexe.  
  
 [8], le type de données SQL_BIT possède des caractéristiques différentes du type BIT dans SQL-92.  
  
 [9] ce type de données n’a aucun type de données correspondant dans SQL-92.  
  
 Cette section fournit de l’exemple suivant.  
  
-   [Exemple de jeu de résultats de SQLGetTypeInfo](../../../odbc/reference/appendixes/example-sqlgettypeinfo-result-set.md)
