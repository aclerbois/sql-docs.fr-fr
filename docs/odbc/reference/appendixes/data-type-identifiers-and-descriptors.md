---
title: Identificateurs et descripteurs de Type de données | Documents Microsoft
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
- data types [ODBC], identifiers
- identifiers [ODBC], data types
- descriptors [ODBC], data types
- verbose data types [ODBC]
- data types [ODBC], descriptors
- concise data types [ODBC]
ms.assetid: f0077c9b-8eb2-4b5f-8c4c-7436fdef37ab
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 690df82a803df4bb4b987026753c448f90734600
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-identifiers-and-descriptors"></a>Les identificateurs de Type de données et les descripteurs de
Les types de données répertoriés dans le [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) et [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md) sections plus haut dans cette annexe sont des types de données « concis » : chaque identificateur fait référence à un type de données unique. Il existe une correspondance univoque entre l’identificateur et le type de données. Descripteurs, toutefois, ne faire pas dans tous les cas utilisent une valeur unique pour identifier les types de données. Dans certains cas, ils utilisent un type de données « commentaires » et un sous-code de type. Pour tous les types de données à l’exception des types de données datetime et d’intervalle, l’identificateur de type détaillé est le même que l’identificateur de type concis, et la valeur SQL_DESC_DATETIME_INTERVAL_CODE est égale à 0. Pour les types de données datetime et interval, toutefois, un type verbose (SQL_DATETIME ou SQL_INTERVAL) est stocké dans SQL_DESC_TYPE, un type concis est stocké dans SQL_DESC_CONCISE_TYPE, et un sous-code pour chaque type concis est stocké dans SQL_DESC_DATETIME_INTERVAL_CODE. L’un de ces champs affecte les autres. Pour plus d’informations sur ces champs, consultez la [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) description de fonction.  
  
 Lorsque le champ SQL_DESC_TYPE ou SQL_DESC_CONCISE_TYPE est défini pour certains types de données, les champs SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_LENGTH, SQL_DESC_PRECISION et SQL_DESC_SCALE sont définies automatiquement les valeurs par défaut, le cas échéant pour le type de données. Pour plus d’informations, consultez la description du champ SQL_DESC_TYPE dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Si une de l’ensemble de valeurs par défaut n’est pas approprié, l’application doit définir explicitement le champ de descripteur via un appel à **SQLSetDescField**.  
  
 Le tableau suivant affiche l’identificateur de type concis, verbose identificateur de type et sous-code de type pour chaque date/heure et un intervalle SQL et un identificateur de type C. Comme indiqué dans cette table, pour les types de données datetime et interval, la SQL_DESC_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE ont les mêmes constantes de manifeste pour les types de données SQL (dans les descripteurs d’implémentation) et les types de données C (dans les descripteurs de l’application).  
  
|Concis type SQL|Type de C concis|Type de commentaires|DATETIME_INTERVAL_CODE|  
|----------------------|--------------------|------------------|------------------------------|  
|SQL_TYPE_DATE|SQL_C_TYPE_DATE|SQL_DATETIME|SQL_CODE_DATE|  
|SQL_TYPE_TIME|SQL_C_TYPE_TIME|SQL_DATETIME|SQL_CODE_TIME|  
|SQL_TYPE_TIMESTAMP|SQL_C_TYPE_TIMESTAMP|SQL_DATETIME|SQL_CODE_TIMESTAMP|  
|SQL_INTERVAL_MONTH|SQL_C_INTERVAL_MONTH|SQL_INTERVAL|SQL_CODE_MONTH|  
|SQL_INTERVAL_YEAR|SQL_C_INTERVAL_YEAR|SQL_INTERVAL|SQL_CODE_YEAR|  
|SQL_INTERVAL_YEAR_TO_MONTH|SQL_C_INTERVAL_YEAR_TO_MONTH|SQL_INTERVAL|SQL_CODE_YEAR_TO_MONTH|  
QL_INTERVAL_DAY|SQL_C_INTERVAL_DAY|SQL_INTERVAL|SQL_CODE_DAY|  
|SQL_INTERVAL_HOUR|SQL_C_INTERVAL_HOUR|SQL_INTERVAL|SQL_CODE_HOUR|  
|SQL_INTERVAL_MINUTE|SQL_C_INTERVAL_MINUTE|SQL_INTERVAL|SQL_CODE_MINUTE|  
|SQL_INTERVAL_SECOND|SQL_C_INTERVAL_SECOND|SQL_INTERVAL|SQL_CODE_SECOND|  
|SQL_INTERVAL_DAY_TO_HOUR|SQL_C_INTERVAL_DAY_TO_HOUR|SQL_INTERVAL|SQL_CODE_DAY_TO_HOUR|  
|SQL_INTERVAL_DAY_TO_MINUTE|SQL_C_INTERVAL_DAY_TO_MINUTE|SQL_INTERVAL|SQL_CODE_DAY_TO_MINUTE|  
QL_INTERVAL_DAY_TO_SECOND|SQL_C_INTERVAL_DAY_TO_SECOND|SQL_INTERVAL|SQL_CODE_DAY_TO_SECOND|  
|SQL_INTERVAL_HOUR_TO_MINUTE|SQL_C_INTERVAL_HOUR_TO_MINUTE|SQL_INTERVAL|SQL_CODE_HOUR_TO_MINUTE|  
|SQL_INTERVAL_HOUR_TO_SECOND|SQL_C_INTERVAL_HOUR_TO_SECOND|SQL_INTERVAL|SQL_CODE_HOUR_TO_SECOND|  
|SQL_INTERVAL_MINUTE_TO_SECOND|SQL_C_INTERVAL_MINUTE_TO_SECOND|SQL_INTERVAL|SQL_CODE_MINUTE_TO_SECOND|
