---
title: Date, Time et Timestamp littéraux | Documents Microsoft
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
- escape sequences [ODBC], literals
ms.assetid: 2b42a52a-6353-494c-a179-3a7533cd729f
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c41d78b848009083abef2595d8628bb8fa1c0b0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="date-time-and-timestamp-literals"></a>Date, Time et Timestamp littéraux
La séquence d’échappement pour les littéraux de date, time et timestamp est  
  
 **{***-type* **'** *valeur* **'}**  
  
 où *type de littéral* est l’une des valeurs répertoriées dans le tableau suivant.  
  
|*type de littéral*|Signification|Mettre en forme de *valeur*|  
|---------------------|-------------|-----------------------|  
|**d**|Date|*aaaa*-*mm*-*dd*|  
|**t**|Heure *|*hh*:*mm*:*ss*[1]|  
|**TS**|Horodateur|*aaaa*-*mm*-*jj* *hh*:*mm*:*ss*[.*f...*] [1]|  
  
 [1], le nombre de chiffres à droite de la virgule décimale dans un intervalle de temps ou timestamp littéral contenant un composant « secondes » dépend de la précision en secondes, comme contenue dans le champ de descripteur SQL_DESC_PRECISION. (Pour plus d’informations, consultez [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).)  
  
 Pour plus d’informations sur la date, heure et les séquences d’échappement timestamp, consultez [Date, Time et Timestamp les séquences d’échappement](../../../odbc/reference/appendixes/date-time-and-timestamp-escape-sequences.md) dans l’annexe c : SQL grammaire.  
  
 Par exemple, les deux instructions SQL suivantes à la date en cours de commande 1023 dans la table Orders. La première instruction utilise la syntaxe de la séquence d’échappement. La deuxième instruction utilise la syntaxe Oracle Rdb native pour la colonne de DATE et n’est pas interopérable.  
  
```  
UPDATE Orders SET OpenDate={d '1995-01-15'} WHERE OrderID=1023  
UPDATE Orders SET OpenDate='15-Jan-1995' WHERE OrderID=1023  
```  
  
 La séquence d’échappement pour une date, heure ou timestamp littéral peut également être placée dans une variable de caractère liée à une date, heure ou timestamp paramètre. Par exemple, le code suivant utilise un paramètre de date lié à une variable de caractère pour mettre à jour la date en cours de commande 1023 dans la table Orders :  
  
```  
SQLCHAR      OpenDate[56]; // The size of a date literal is 55.  
SQLINTEGER   OpenDateLenOrInd = SQL_NTS;  
  
// Bind the parameter.  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_TYPE_DATE, 0, 0,  
                  OpenDate, sizeof(OpenDate), &OpenDateLenOrInd);  
  
// Place the date in the OpenDate variable. In addition to the escape  
// sequence shown, it would also be possible to use either of the  
// strings "{d '1995-01-15'}" and "15-Jan-1995", although the latter  
// is data source-specific.  
strcpy_s( (char*) OpenDate, _countof(OpenDate), "{d '1995-01-15'}");  
  
// Execute the statement.  
SQLExecDirect(hstmt, "UPDATE Orders SET OpenDate=? WHERE OrderID = 1023", SQL_NTS);  
```  
  
 Toutefois, il est généralement plus efficace de lier le paramètre directement à une structure de date :  
  
```  
SQL_DATE_STRUCT   OpenDate;  
SQLINTEGER        OpenDateInd = 0;  
  
// Bind the parameter.  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 0, 0,  
                  &OpenDate, 0, &OpenDateLen);  
  
// Place the date in the dsOpenDate structure.  
OpenDate.year = 1995;  
OpenDate.month = 1;  
OpenDate.day = 15;  
  
// Execute the statement.  
SQLExecDirect(hstmt, "UPDATE Employee SET OpenDate=? WHERE OrderID = 1023", SQL_NTS);  
```  
  
 Pour déterminer si un pilote prend en charge les séquences d’échappement ODBC pour la date, heure ou timestamp littéraux, une application appelle **SQLGetTypeInfo**. Si la source de données prend en charge un type de données date, heure ou timestamp, il doit également prendre en charge la séquence d’échappement correspondante.  
  
 Sources de données peuvent prendre également en charge les littéraux de date/heure définis dans la spécification ANSI SQL-92, qui sont différents des séquences d’échappement ODBC pour la date, heure ou timestamp littéraux. Pour déterminer si une source de données prend en charge les littéraux ANSI, une application appelle **SQLGetInfo** avec l’option SQL_ANSI_SQL_DATETIME_LITERALS.  
  
 Pour déterminer si un pilote prend en charge les séquences d’échappement ODBC pour les littéraux de l’intervalle, une application appelle **SQLGetTypeInfo**. Si la source de données prend en charge un type de données d’intervalle datetime, il doit également prendre en charge la séquence d’échappement correspondante.  
  
 Sources de données peuvent prendre également en charge les littéraux de date/heure définis dans la spécification ANSI SQL-92, qui sont différents dans les séquences d’échappement ODBC pour les littéraux de l’intervalle datetime. Pour déterminer si une source de données prend en charge les littéraux ANSI, une application appelle **SQLGetInfo** avec l’option SQL_ANSI_SQL_DATETIME_LITERALS.
