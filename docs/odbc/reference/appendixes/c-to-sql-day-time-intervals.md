---
title: 'C en SQL : intervalles de temps de jour | Documents Microsoft'
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
- day-time intervals [ODBC]
- data conversions from C to SQL types [ODBC], day-time intervals
- converting data from c to SQL types [ODBC], day-time intervals
- intervals [ODBC], converting
ms.assetid: f9ee1ddb-dec7-4f78-b6e2-5ba34e7d6f59
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 400bd47766d45564b8dbced6fec30732cfd02bcd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="c-to-sql-day-time-intervals"></a>C en SQL : intervalles de temps de jour
Les identificateurs pour les types de données ODBC C l’intervalle de temps jour sont :  
  
 SQL_C_INTERVAL_DAY  
  
 SQL_C_INTERVAL_HOUR  
  
 SQL_C_INTERVAL_MINUTE  
  
 SQL_C_INTERVAL_SECOND  
  
 SQL_C_INTERVAL_DAY_TO_HOUR  
  
 SQL_C_INTERVAL_DAY_TO_MINUTE  
  
 SQL_C_INTERVAL_DAY_TO_SECOND  
  
 SQL_C_INTERVAL_HOUR_TO_MINUTE  
  
 SQL_C_INTERVAL_HOUR_TO_SECOND  
  
 SQL_C_INTERVAL_MINUTE_TO_SECOND  
  
 Le tableau suivant présente les types de données à laquelle l’intervalle de données C peut être converti à ODBC SQL. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de C en Types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR [a]<br /><br /> SQL_VARCHAR, [a]<br /><br /> SQL_LONGVARCHAR [a]|Longueur d’octet de colonne > = longueur d’octet de caractère<br /><br /> Longueur d’octet colonne < caractères de longueur d’octet [a]<br /><br /> Valeur de données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22001<br /><br /> 22015|  
|SQL_WCHAR [a]<br /><br /> SQL_WVARCHAR [a]<br /><br /> SQL_WLONGVARCHAR [a]|Longueur de colonne > = longueur de caractères de données<br /><br /> Longueur de colonne caractère < caractères de longueur des données [a]<br /><br /> Valeur de données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22001<br /><br /> 22015|  
|SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b] SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b] SQL_NUMERIC [b]<br /><br /> SQL_DECIMAL [b]|Conversion d’un intervalle de champ unique n’a pas abouti à une troncation de chiffres entiers<br /><br /> La conversion a entraîné une troncation de chiffres entiers|n/a<br /><br /> 22003|  
|SQL_INTERVAL_DAY<br /><br /> SQL_INTERVAL_HOUR<br /><br /> SQL_INTERVAL_MINUTE<br /><br /> SQL_INTERVAL_SECOND<br /><br /> SQL_INTERVAL_DAY_TO_HOUR<br /><br /> SQL_INTERVAL_DAY_TO_MINUTE<br /><br /> SQL_INTERVAL_DAY_TO_SECOND<br /><br /> SQL_INTERVAL_HOUR_TO_MINUTE<br /><br /> SQL_INTERVAL_HOUR_TO_SECOND<br /><br /> SQL_INTERVAL_MINUTE_TO_SECOND|Valeur de données a été convertie sans troncation de tous les champs<br /><br /> Un ou plusieurs champs de valeur de données ont été tronquées lors de la conversion|n/a<br /><br /> 22015|  
  
 [a] C tous les types d’intervalle peuvent être converti en un type de données caractère.  
  
 [b] si le champ de type dans la structure de l’intervalle est tels que l’intervalle est un champ unique (SQL_DAY, SQL_HOUR, SQL_MINUTE ou SQL_SECOND), le type d’intervalle C peut être converti en n’importe quel numérique exact (SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_DECIMAL ou SQL_NUMERIC).  
  
 La conversion d’un type d’intervalle C par défaut est de l’intervalle de temps de jour correspondant type SQL.  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion des données à partir du type de données d’intervalle C et suppose que la taille du tampon de données est la taille du type de données d’intervalle C. La valeur de l’indicateur/longueur est passée dans le *StrLen_or_Ind* argument dans **SQLPutData** et dans la mémoire tampon spécifiée avec la *StrLen_or_IndPtr* argument dans **SQLBindParameter**. Le tampon de données est spécifié avec la *DataPtr* argument dans **SQLPutData** et *ParameterValuePtr* argument dans **SQLBindParameter**.  
  
 L’exemple suivant montre comment envoyer des données d’intervalle C stockées dans la structure SQL_INTERVAL_STRUCT dans une colonne de base de données. La structure de l’intervalle contient un intervalle de DAY_TO_SECOND ; Il sera être stocké dans une colonne de base de données de type SQL_INTERVAL_DAY_TO_MINUTE.  
  
```  
SQL_INTERVAL_STRUCT is;  
SQLINTEGER cbValue;  
  
// Initialize the interval struct to contain the DAY_TO_SECOND  
// interval "154 days, 22 hours, 44 minutes, and 10 seconds"  
is.intval.day_second.day      = 154;  
is.intval.day_second.hour     = 22;  
is.intval.day_second.minute   = 44;  
is.intval.day_second.second   = 10;  
is.interval_sign              = SQL_FALSE;  
  
// Bind the dynamic parameter  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_INTERVAL_DAY_TO_SECOND,  
                  SQL_INTERVAL_DAY_TO_MINUTE, 0, 0, &is,  
                  sizeof(SQL_INTERVAL_STRUCT), &cbValue);  
  
// Execute an insert statement; "interval_column" is a column  
// whose data type is SQL_INTERVAL_DAY_TO_MINUTE.  
SQLExecDirect(hstmt,"INSERT INTO table(interval_column) VALUES (?)",SQL_NTS);  
```
