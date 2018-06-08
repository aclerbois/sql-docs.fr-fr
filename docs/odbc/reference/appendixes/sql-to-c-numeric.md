---
title: 'SQL en c : numérique | Documents Microsoft'
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
- data conversions from SQL to C types [ODBC], numeric
- numeric data type [ODBC], converting
- converting data from SQL to C types [ODBC], numeric
ms.assetid: 76f8b5d5-4bd0-4dcb-a90a-698340e0d36e
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 72714e2bf41a820bec154487d23fb0fcf0c66ac0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-to-c-numeric"></a>SQL en c : numérique
Les identificateurs pour les types de données SQL ODBC numériques sont :  
  
 SQL_DECIMAL  
  
 SQL_BIGINT  
  
 SQL_NUMERIC  
  
 SQL_REAL  
  
 SQL_TINYINT  
  
 SQL_FLOAT  
  
 SQL_SMALLINT  
  
 SQL_DOUBLE SQL_INTEGER  
  
 Le tableau suivant présente les types de données à laquelle les données SQL numériques peuvent être converties à ODBC C. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|Longueur en octets de caractères < *BufferLength*<br /><br /> Nombre de chiffres de l’ensemble (par opposition aux fractions de seconde) < *BufferLength*<br /><br /> Nombre de chiffres de l’ensemble (par opposition aux fractions de seconde) > = *BufferLength*|data<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Longueur des caractères < *BufferLength*<br /><br /> Nombre de chiffres de l’ensemble (par opposition aux fractions de seconde) < *BufferLength*<br /><br /> Nombre de chiffres de l’ensemble (par opposition aux fractions de seconde) > = *BufferLength*|data<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données de caractères<br /><br /> Longueur des données de caractères<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_STINYINT<br /><br /> SQL_C_UTINYINT<br /><br /> SQL_C_TINYINT<br /><br /> SQL_C_SBIGINT<br /><br /> SQL_C_UBIGINT<br /><br /> SQL_C_SSHORT<br /><br /> SQL_C_USHORT<br /><br /> SQL_C_SHORT<br /><br /> SQL_C_SLONG<br /><br /> SQL_C_ULONG<br /><br /> SQL_C_LONG<br /><br /> SQL_C_NUMERIC|Les données converties sans troncation [a]<br /><br /> Les données converties avec la troncation des fractions [a]<br /><br /> La conversion de données entraînerait une perte de chiffres entiers (par opposition aux fractions de seconde) [a]|data<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 01 S 07<br /><br /> 22003|  
_C_FLOAT<br /><br /> SQL_C_DOUBLE|Les données sont dans la plage du type de données à laquelle le nombre est en cours de conversion [a]<br /><br /> Données sont trouve en dehors de la plage du type de données à laquelle le nombre est en cours de conversion [a]|data<br /><br /> Indéfini|Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_BIT|Les données sont 0 ou 1, [a]<br /><br /> Données sont supérieures à 0, inférieure à 2 et non égal à 1, [a]<br /><br /> Données sont inférieur à 0 ou supérieur ou égal à 2, [a]|data<br /><br /> Données tronquées<br /><br /> Indéfini|1 [b]<br /><br /> 1 [b]<br /><br /> Indéfini|n/a<br /><br /> 01 S 07<br /><br /> 22003|  
|SQL_C_BINARY|Longueur en octets de données < = *BufferLength*<br /><br /> Longueur en octets de données > *BufferLength*|data<br /><br /> Indéfini|Longueur des données<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_INTERVAL_MONTH (c) SQL_C_INTERVAL_YEAR (c) SQL_C_INTERVAL_DAY (c) SQL_C_INTERVAL_HOUR (c) SQL_C_INTERVAL_MINUTE (c) SQL_C_INTERVAL_SECOND (c)|Données ne tronquées pas<br /><br /> Partie fractions tronqué<br /><br /> Partie entière du nombre tronqué|data<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01 S 07<br /><br /> 22015|  
_C_INTERVAL_YEAR_TO_MONTH SQL_C_INTERVAL_DAY_TO_HOUR SQL_C_INTERVAL_DAY_TO_MINUTE SQL_C_INTERVAL_DAY_TO_SECOND SQL_C_INTERVAL_HOUR_TO_MINUTE SQL_C_INTERVAL_HOUR_TO_SECOND|Partie entière du nombre tronqué|Indéfini|Indéfini|22015|  
  
 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote part du principe que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] Il s’agit de la taille du type de données C correspondante.  
  
 [c] cette conversion est prise en charge uniquement pour les types de données numériques exactes (SQL_DECIMAL, SQL_NUMERIC, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER et SQL_BIGINT). Il n’est pas pris en charge pour les types de données numérique approximatif (SQL_FLOAT, SQL_REAL ou SQL_DOUBLE).  
  
## <a name="sqlcnumeric-and-sqlsetdescfield"></a>SQL_C_NUMERIC et SQLSetDescField  
 Le [SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md) est nécessaire pour effectuer une liaison avec des valeurs SQL_C_NUMERIC manuelle. (Notez que SQLSetDescField a été ajoutée dans ODBC 3.0.) Pour effectuer une liaison manuelle, vous devez d’abord obtenir le handle du descripteur.  
  
```  
if (fCType == SQL_C_NUMERIC) {   
   // special processing required for NUMERIC to get right scale & precision  
   // Modify the fields in the implicit application parameter descriptor  
   SQLHDESC hdesc=NULL;  
  
   // Use SQL_ATTR_APP_ROW_DESC for calls to SQLBindCol()  
   // Use SQL_ATTR_APP_PARAM_DESC for calls to SQLBindParameter()  
   //  
   // retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_ROW_DESC, &hdesc, 0, NULL);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);  
   if (!ODBC_CALL_SUCCESS(retcode)) {  
      printf ("\nSQLGetStmtAttr failed");  
      i = 1;  
      sqlstate[7] = '\0';  
      while (SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, i, sqlstate, &NativeError, wrkbuf, sizeof(wrkbuf), &len) != SQL_NO_DATA) {  
         printf("\niTestCase = %d Failed...Precision = %d, Scale = %d\nNativeError=%d, State=%s, \n  Message=%s",   
            iTestCase, Precision, Scale, NativeError, sqlstate, wrkbuf);  
         i++;  
      }  
      continue;  
   }  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_TYPE, (SQLPOINTER) SQL_C_NUMERIC, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_PRECISION, (SQLPOINTER)num.precision, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_SCALE, (SQLPOINTER)num.scale, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_DATA_PTR, (SQLPOINTER) &(num), sizeof(num));  
```
