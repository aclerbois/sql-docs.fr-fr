---
title: SQLGetInfo (pilote Access) | Documents Microsoft
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
- SQLGetInfo function [ODBC], Access Driver
- Access driver [ODBC], SQLGetInfo
ms.assetid: c226aba7-a2f4-4b32-b640-92654b40e5a7
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0a761fe38b7d162d7e43ae2f9148626dadff57e6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetinfo-access-driver"></a>SQLGetInfo (pilote Access)
> [!NOTE]  
>  Cette rubrique fournit des informations spécifiques au pilote Access. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 **SQLGetInfo** prend en charge le type d’informations SQL_FILE_USAGE. La valeur retournée est un entier 16 bits qui indique comment le pilote traite directement les fichiers dans une source de données :  
  
-   SQL_FILE_NOT_SUPPORTED : Le pilote n’est pas un pilote de niveau unique.  
  
-   SQL_FILE_TABLE : Un pilote de niveau unique traite les fichiers dans une source de données en tant que tables.  
  
-   SQL_FILE_QUALIFIER : Un pilote de niveau unique traite les fichiers dans une source de données en tant que qualificateur.  
  
 Le pilote ODBC retourne SQL_FILE_QUALIFIER, car chaque fichier est une base de données complète.  
  
## <a name="sqlbookmarkpersistence"></a>SQL_BOOKMARK_PERSISTENCE  
 SQL_BP_SCROLL &AMP;#124; SQL_BP_UPDATE [1]  
  
 [1] signets sont conservés après une validation, mais ne sont pas conservés après une restauration.  
  
## <a name="sqlconvertbinary"></a>SQL_CONVERT_BINARY  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertchar"></a>SQL_CONVERT_CHAR  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertdate"></a>SQL_CONVERT_DATE  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertdouble"></a>SQL_CONVERT_DOUBLE  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertfloat"></a>SQL_CONVERT_FLOAT  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertinteger"></a>SQL_CONVERT_INTEGER  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertlongvarbinary"></a>SQL_CONVERT_LONGVARBINARY  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertlongvarchar"></a>SQL_CONVERT_LONGVARCHAR  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertnumeric"></a>SQL_CONVERT_NUMERIC  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertreal"></a>SQL_CONVERT_REAL  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertsmallint"></a>SQL_CONVERT_SMALLINT  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconverttime"></a>SQL_CONVERT_TIME  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconverttimestamp"></a>SQL_CONVERT_TIMESTAMP  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconverttinyint"></a>SQL_CONVERT_TINYINT  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertvarbinary"></a>SQL_CONVERT_VARBINARY  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlconvertvarchar"></a>SQL_CONVERT_VARCHAR  
 SQL_CVT_DOUBLE &AMP;#124; SQL_CVT_FLOAT &AMP;#124; SQL_CVT_INTEGER &AMP;#124; SQL_CVT_NUMERIC &AMP;#124; SQL_CVT_REAL &AMP;#124; SQL_CVT_SMALLINT &AMP;#124; SQL_CVT_VARCHAR &AMP;#124; SQL_CVT_WVARCHAR  
  
## <a name="sqlunion"></a>SQL_UNION  
 SQL_U_UNION_ALL &AMP;#124; SQL_U_UNION  
  
## <a name="sqldbmsver"></a>SQL_DBMS_VER  
  
|ISAM|Version|Format des numéros de version|  
|----------|-------------|-------------------------------|  
|Microsoft Access|2|02.00.0000|  
||3|03.00.0000|  
||3.5|03.50.0000|  
||4.0|04.00.0000|  
  
> [!NOTE]  
>  Les versions 1.0 et 1.1 ne sont pas pris en charge. En outre, il n’existe aucune différence dans le format de données dans les versions 3.0, 7.0 et 97 de Microsoft Access.  
  
## <a name="sqlddlindex"></a>SQL_DDL_INDEX  
 SQL_DL_CREATE_INDEX  
  
 SQL_DL_DROP_INDEX  
  
## <a name="sqlgetdataextensions"></a>SQL_GETDATA_EXTENSIONS  
 SQL_GD_ANY_ORDER &AMP;#124; SQL_GD_ANY_COLUMN &AMP;#124; SQL_GD_BLOCK &AMP;#124; SQL_GD_BOUND  
  
## <a name="sqlkeywords"></a>SQL_KEYWORDS  
 ALPHANUMÉRIQUE  
  
 AUTOINCREMENT  
  
 BINARY  
  
 BOOLEAN  
  
 BYTE  
  
 COMPTEUR  
  
 Monétaire (Currency)  
  
 DATABASE  
  
 DATABASENAME  
  
 DATETIME  
  
 NE PAS AUTORISER  
  
 DISTINCTROW  
  
 DOUBLEFLOAT  
  
 FLOAT4  
  
 FLOAT8  
  
 GENERAL  
  
 IEEEDOUBLE  
  
 IEEESINGLE  
  
 IGNORE  
  
 IMAGE  
  
 INTEGER1  
  
 INTEGER2  
  
 INTEGER4  
  
 LOGIQUE  
  
 VALEUR_LOGIQUE1  
  
 LONG  
  
 LONGBINARY  
  
 LONGCHAR  
  
 LONGTEXT  
  
 MÉMO  
  
 MONEY  
  
 REMARQUE  
  
 NUMBER  
  
 CLASSES OLEOBJECT  
  
 OWNERACCESS  
  
 PARAMETERS  
  
 PERCENT  
  
 PIVOT  
  
 COURTE  
  
 UNIQUE  
  
 SINGLEFLOAT  
  
 STDEV  
  
 STDEVP  
  
 CHAÎNE  
  
 TABLEID  
  
 TEXT  
  
 Haut de la page  
  
 TRANSFORMATION  
  
 UNSIGNEDBYTE  
  
 VAR  
  
 VARBINARY  
  
 VARP  
  
 YESNO  
  
## <a name="sqlnumericfunctions"></a>SQL_NUMERIC_FUNCTIONS  
 SQL_FN_NUM_ABS &AMP;#124; SQL_FN_NUM_ATAN &AMP;#124; SQL_FN_NUM_CEILING &AMP;#124; SQL_FN_NUM_COS &AMP;#124; SQL_FN_NUM_EXP &AMP;#124; SQL_FN_NUM_FLOOR &AMP;#124; SQL_FN_NUM_LOG &AMP;#124; SQL_FN_NUM_MOD &AMP;#124; SQL_FN_NUM_POWER &AMP;#124; SQL_FN_ NUM_RAND &AMP;#124; SQL_FN_NUM_SIGN &AMP;#124; SQL_FN_NUM_SIN &AMP;#124; SQL_FN_NUM_SQRT &AMP;#124; SQL_FN_NUM_TAN  
  
## <a name="sqlojcapabilities"></a>SQL_OJ_CAPABILITIES  
 SQL_OJ_LEFT SQL_OJ_RIGHT SQL_OJ_NOT_ORDERED SQL_OJ_INNER SQL_OJ_ALL_COMPARISON_OPS  
  
## <a name="sqlcatalogusage"></a>SQL_CATALOG_USAGE  
 SQL_QU_DML_STATEMENTS &AMP;#124; SQL_QU_TABLE_DEFINITION &AMP;#124; SQL_QU_INDEX_DEFINITION &AMP;#124; SQL_QU_PROCEDURE_INVOCATION  
  
## <a name="sqlscrolloptions"></a>SQL_SCROLL_OPTIONS  
 SQL_SO_FORWARD_ONLY &AMP;#124; SQL_SO_STATIC &AMP;#124; SQL_SO_KEYSET_DRIVEN  
  
## <a name="sqlstringfunctions"></a>SQL_STRING_FUNCTIONS  
 SQL_FN_STR_ASCII &AMP;#124; SQL_FN_STR_CHAR &AMP;#124; SQL_FN_STR_CONCAT &AMP;#124; SQL_FN_STR_LCASE &AMP;#124; SQL_FN_STR_LEFT &AMP;#124; SQL_FN_STR_LENGTH &AMP;#124; SQL_FN_STR_LOCATE &AMP;#124; SQL_FN_STR_LOCATE_2 SQL_FN_STR_LTRIM &AMP;#124; SQL_ FN_STR_RIGHT &AMP;#124; SQL_FN_STR_RTRIM &AMP;#124; SQL_FN_STR_SPACE &AMP;#124; SQL_FN_STR_SUBSTRING &AMP;#124; SQL_FN_STR_UCASE  
  
## <a name="sqlsubqueries"></a>SQL_SUBQUERIES  
 SQL_SQ_COMPARISON &AMP;#124; SQL_SQ_EXISTS &AMP;#124; SQL_SQ_IN &AMP;#124; SQL_SQ_QUANTIFIED &AMP;#124; SQL_SQ_CORRELATED_SUBQUERIES  
  
## <a name="sqltimedatefunctions"></a>SQL_TIMEDATE_FUNCTIONS  
 SQL_FN_TD_CURDATE &AMP;#124; SQL_FN_TD_CURTIME &AMP;#124; SQL_FN_TD_DAYOFMONTH &AMP;#124; SQL_FN_TD_DAYOFWEEK &AMP;#124; SQL_FN_TD_DAYOFYEAR &AMP;#124; SQL_FN_TD_HOUR &AMP;#124; SQL_FN_TD_MINUTE &AMP;#124; SQL_FN_TD_MONTH &AMP;#124; SQL_FN_TD_NOW &AMP;#124; SQL_FN_TD_SECOND &AMP;#124; SQL_FN_TD_WEEK &AMP;#124; SQL_FN_TD_YEAR
