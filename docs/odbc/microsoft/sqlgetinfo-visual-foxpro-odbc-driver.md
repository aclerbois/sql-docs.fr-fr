---
title: SQLGetInfo (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- SQLGetInfo function [ODBC], Visual FoxPro ODBC Driver
ms.assetid: fbc39e3d-67d9-4331-bf5f-76dbd74c4c45
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b05ab71a12059535986cbd452e993e01178342fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetinfo-visual-foxpro-odbc-driver"></a>SQLGetInfo (le pilote ODBC Visual FoxPro)
> [!NOTE]  
>  Cette rubrique contient des informations spécifiques au pilote ODBC Visual FoxPro. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Prise en charge : complet  
  
 Conformité d’API ODBC : Niveau 1  
  
 Retourne des informations générales sur le pilote ODBC Visual FoxPro et de la source de données associée à un handle de connexion, *pas*. La liste suivante affiche la valeur retournée par le pilote ODBC Visual FoxPro pour chaque *fInfoType* argument et commentaires concernant les valeurs retournées.  
  
 Pour plus d’informations, consultez [SQLGetInfo](../../odbc/reference/syntax/sqlgetinfo-function.md) dans les *de référence du programmeur ODBC*.  
  
## <a name="a"></a>Un  
 SQL_ACCESSIBLE_PROCEDURES retourne n '.  
  
 SQL_ACCESSIBLE_TABLES retourne « Y ».  
  
 SQL_ACTIVE_CONNECTIONS retourne 0.  
  
 SQL_ACTIVE_STATEMENTS retourne 0.  
  
 SQL_ALTER_TABLE retourne SQL_AT_ADD_COLUMN ou SQL_AT_DROP_COLUMN.  
  
## <a name="b"></a>B  
 SQL_BOOKMARK_PERSISTENCE retourne SQL_BP_SCROLL.  
  
## <a name="c"></a>C  
 SQL_COLUMN_ALIAS retourne « Y ».  
  
 SQL_CONCAT_NULL_BEHAVIOR retourne SQL_CB_NULL.  
  
 SQL_CONVERT_BIGINT retourne 0. Le pilote ODBC Visual FoxPro ne prend pas en charge *BigInt*.  
  
 SQL_CONVERT_BINARY retourne 0.  
  
 SQL_CONVERT_BIT retourne 0.  
  
 SQL_CONVERT_CHAR retourne 0.  
  
 SQL_CONVERT_DATE retourne 0.  
  
 SQL_CONVERT_DECIMAL retourne 0.  
  
 SQL_CONVERT_DOUBLE retourne 0.  
  
 SQL_CONVERT_FLOAT retourne 0.  
  
 SQL_CONVERT_INTEGER retourne 0.  
  
 SQL_CONVERT_LONGVARBINARY retourne 0.  
  
 SQL_CONVERT_LONGVARCHAR retourne 0.  
  
 SQL_CONVERT_NUMERIC retourne 0.  
  
 SQL_CONVERT_REAL retourne 0.  
  
 SQL_CONVERT_SMALLINT retourne 0.  
  
 SQL_CONVERT_TIME retourne 0.  
  
 SQL_CONVERT_TIMESTAMP retourne 0.  
  
 SQL_CONVERT_TINYINT retourne 0.  
  
 SQL_CONVERT_VARBINARY retourne 0.  
  
 SQL_CONVERT_VARCHAR retourne 0.  
  
 SQL_CONVERT_FUNCTIONS retourne 0.  
  
 SQL_CORRELATION_NAME retourne SQL_CN_ANY.  
  
 SQL_CURSOR_COMMIT_BEHAVIOR retourne SQL_CB_PRESERVE.  
  
 SQL_CURSOR_ROLLBACK_BEHAVIOR retourne SQL_CB_PRESERVE.  
  
## <a name="d"></a>D  
 SQL_DATA_SOURCE_NAME retourne la valeur passée en tant que DSN à [SQLConnect](../../odbc/microsoft/sqlconnect-visual-foxpro-odbc-driver.md), ou [SQLDriverConnect](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md); retourne une chaîne vide si aucune source de données n’est spécifié.  
  
 SQL_DATA_SOURCE_READ_ONLY retourne n '.  
  
 SQL_DATABASE_NAME retourne un chemin d’accès UNC complet à la base de données si la source de données est un [base de données](../../odbc/microsoft/visual-foxpro-terminology.md). Si la source de données se connecte à un répertoire de [tables](../../odbc/microsoft/visual-foxpro-terminology.md), la fonction retourne le chemin d’accès au répertoire.  
  
 SQL_DBMS_NAME retourne « Visual FoxPro ».  
  
 SQL_DBMS_VER retourne « 03.00.0000 ».  
  
 SQL_DEFAULT_TXN_ISOLATION retourne SQL_TXN_READ_COMMITTED. Lectures de données modifiées ne sont pas possibles, mais fantômes et des lectures non reproductibles sont possibles.  
  
 SQL_DRIVER_HDBC est implémentée par le Gestionnaire de pilotes.  
  
 SQL_DRIVER_HENV est implémentée par le Gestionnaire de pilotes.  
  
 SQL_DRIVER_HLIB est implémentée par le Gestionnaire de pilotes.  
  
 SQL_DRIVER_HSTMT est implémentée par le Gestionnaire de pilotes.  
  
 SQL_DRIVER_NAME retourne « vfpodbc.dll ».  
  
 SQL_DRIVER_ODBC_VER retourne « 02.50 » (SQL_SPEC_MAJOR, SQL_SPEC_MINOR).  
  
 SQL_DRIVER_VER retourne « 01.00.0000 ».  
  
## <a name="e"></a>E  
 SQL_EXPRESSIONS_IN_ORDERBY retourne n '.  
  
## <a name="f"></a>F  
 SQL_FETCH_DIRECTION retourne :  
  
-   SQL_FD_FETCH_NEXT  
  
-   SQL_FD_FETCH_BOOKMARK  
  
-   SQL_FD_FETCH_ABSOLUTE  
  
-   SQL_FD_FETCH_PRIOR  
  
-   SQL_FD_FETCH_LAST  
  
-   SQL_FD_FETCH_RELATIVE  
  
-   SQL_FD_FETCH_BOOKMARK.  
  
 SQL_FILE_USAGE retourne SQL_FILE_QUALIFIER à la fois pour la base de données (fichier .dbc) et pour un tableau des sources de données (fichier .dbf).  
  
## <a name="g-h"></a>G-H  
 SQL_GETDATA_EXENSIONS retourne :  
  
-   SQL_GD_ANY_COLUMN  
  
-   SQL_GD_ANY_BLOCK  
  
-   SQL_GD_ANY_BOUND  
  
-   SQL_GD_ANY_ORDER  
  
 SQL_GROUP_BY retourne SQL_GB_NO_RELATION.  
  
## <a name="i-j"></a>I-J  
 SQL_IDENTIFIER_CASE retourne sql_ic_mixed en.  
  
 SQL_IDENTIFIER_QUOTE_CHAR retourne '.  
  
## <a name="k"></a>K  
 SQL_KEYWORDS retourne « ».  
  
## <a name="l"></a>L  
 SQL_LIKE_ESCAPE_CLAUSE retourne n '.  
  
 SQL_LOCK_TYPES retourne SQL_LCK_NO_CHANGE.  
  
## <a name="m"></a>M  
 SQL_MAX_BINARY_LITERAL_LEN retourne 0.  
  
 SQL_MAX_CHAR_LITERAL_LEN retourne 254.  
  
 SQL_MAX_COLUMN_NAME_LEN retourne 128.  
  
 SQL_MAX_COLUMNS_IN_GROUP_BY renvoie la valeur 16.  
  
 SQL_MAX_COLUMNS_IN_ORDER_BY renvoie la valeur 16.  
  
 SQL_MAX_COLUMNS_IN_INDEX retourne 0.  
  
 SQL_MAX_COLUMNS_IN_SELECT retourne 254.  
  
 SQL_MAX_COLUMNS_IN_TABLE retourne 254.  
  
 SQL_MAX_CURSOR_NAME_LEN retourne 254.  
  
 SQL_MAX_INDEX_SIZE retourne 0.  
  
 SQL_MAX_OWNER_NAME_LEN retourne 0.  
  
 SQL_MAX_PROCEDURE_NAME_LEN retourne 0. Le pilote ODBC Visual FoxPro n’autorise pas l’accès direct à des procédures stockées de Visual FoxPro.  
  
 SQL_MAX_QUALIFIER_NAME_LEN retourne la longueur maximale du système d’exploitation.  
  
 SQL_MAX_ROW_SIZE retourne 254 ^ 2.  
  
 SQL_MAX_ROW_SIZE_INCLUDES_LONG retourne n '.  
  
 SQL_MAX_STATEMENT_LEN retourne 8192.  
  
 SQL_MAX_TABLE_NAME_LEN retourne 128.  
  
 SQL_MAX_TABLES_IN_SELECT renvoie la valeur 16.  
  
 SQL_MAX_USER_NAME_LEN retourne 0.  
  
 SQL_MULT_RESULT_SETS retourne « Y ».  
  
 SQL_MULTIPLE_ACTIVE_TXN retourne « Y ». Plusieurs connexions peuvent avoir plusieurs transactions ouvertes en même temps.  
  
## <a name="n"></a>N  
 SQL_NEED_LONG_DATA_LEN retourne n '.  
  
 SQL_NON_NULLABLE_COLUMNS retourne SQL_NNC_NON_NULL.  
  
 SQL_NULL_COLLATION retourne SQL_NC_LOW.  
  
 SQL_NUMERIC_FUNCTIONS retourne toutes les fonctions sauf SQL_FN_NUM_POWER, ce qui n’est pas pris en charge par le pilote ODBC de Visual FoxPro. Les fonctions suivantes sont prises en charge :  
  
-   SQL_FN_NUM_ABS  
  
-   SQL_FN_NUM_ACOS  
  
-   SQL_FN_NUM_ASIN  
  
-   SQL_FN_NUM_ATAN  
  
-   SQL_FN_NUM_ATAN2  
  
-   SQL_FN_NUM_CELING  
  
-   SQL_FN_NUM_COS  
  
-   SQL_FN_NUM_COT  
  
-   SQL_FN_NUM_DEGREES  
  
-   SQL_FN_NUM_EXP  
  
-   SQL_FN_NUM_FLOOR  
  
-   SQL_FN_NUM_LOG  
  
-   SQL_FN_NUM_LOG10  
  
-   SQL_FN_NUM_MOD  
  
-   SQL_FN_NUM_PI  
  
-   SQL_FN_NUM_RADIANS  
  
-   SQL_FN_NUM_RAND  
  
-   SQL_FN_NUM_ROUND  
  
-   SQL_FN_NUM_SIGN  
  
-   SQL_FN_NUM_SIN  
  
-   SQL_FN_NUM_SQRT  
  
-   SQL_FN_NUM_TAN  
  
## <a name="o"></a>O  
 SQL_ODBC_API_CONFORMANCE retourne SQL_OAC_LEVEL1.  
  
 SQL_ODBC_SAG_CLI_CONFORMANCE retourne SQL_OSCC_COMPLIANT.  
  
 SQL_ODBC_SQL_CONFORMANCE retourne SQL_OSC_MINIMUM. La syntaxe SQL minimale est pris en charge.  
  
 Retourne SQL_ODBC_SQL_OPT_IEF « N ».  
  
 SQL_ODBC_VER est implémentée par le Gestionnaire de pilotes.  
  
 Retourne SQL_ORDER_BY_COLUMNS_IN_SELECT « N ».  
  
 Retourne SQL_OUTER_JOINS « N ».  
  
 SQL_OWNER_TERM retourne « ». Le pilote ODBC Visual FoxPro ne prend pas en charge les propriétaires de ses objets.  
  
 SQL_OWNER_USAGE retourne 0. Le pilote ODBC Visual FoxPro ne prend pas en charge les propriétaires de ses objets.  
  
## <a name="p"></a>P  
 SQL_POS_OPERATIONS retourne SQL_POS_POSITION.  
  
 SQL_POSITIONED_STATEMENTS retourne 0.  
  
 SQL_PROCEDURE_TERM retourne « ».  
  
 SQL_PROCEDURES retourne n '.  
  
## <a name="q"></a>Q  
 SQL_QUALIFIER_LOCATION retourne SQL_QL_START.  
  
 SQL_QUALIFIER_NAME_SEPARATOR retourne ' !' ou '\\'. Le séparateur entre la base de données et la table est ' !' pour les sources de données connectés à [bases de données](../../odbc/microsoft/visual-foxpro-terminology.md), et '\\' pour les sources de données qui sont des répertoires de [libre tables](../../odbc/microsoft/visual-foxpro-terminology.md).  
  
 SQL_QUALIFIER_TERM retourne « database » ou « directory ». Le qualificateur est « de base de données » pour les sources de données connecté au [bases de données](../../odbc/microsoft/visual-foxpro-terminology.md)et « répertoire » pour les sources de données qui sont des répertoires de [libre tables](../../odbc/microsoft/visual-foxpro-terminology.md).  
  
 SQL_QUALIFIER_USAGE ne prend pas en charge SQL_QU_PRIVILEGE_DEFINITION ; Il retourne SQL_QU_DML_STATEMENT ou SQL_QU_TABLE_DEFINITION.  
  
 SQL_QUOTED_IDENTIFIER_CASE retourne sql_ic_mixed en.  
  
## <a name="r"></a>R  
 Retourne SQL_ROW_UPDATES « N ». Le pilote ODBC Visual FoxPro prend en charge les curseurs statiques et transfert d’uniquement.  
  
## <a name="s"></a>S  
 SQL_SCROLL_CONCURRENCY retourne SQL_SCCO_READ_ONLY.  
  
 SQL_SCROLL_OPTIONS retourne SQL_SO_STATIC ou SQL_SO_READONLY.  
  
 SQL_SEARCH_PATTERN_ESCAPE retourne «\\».  
  
 Sql_server_name retourne « ».  
  
 SQL_SPECIAL_CHARACTERS retourne « ~ @# $% ^ ».  
  
 SQL_STATIC_SENSITIVITY retourne 0. Le pilote ODBC Visual FoxPro ne prend pas en charge des mises à jour.  
  
 SQL_STRING_FUNCTIONS ne prend pas en charge SQL_FN_STR_INSERT, SQL_FN_STR_LOCATE, SQL_FN_STR_LOCATE_2 ou SQL_FN_STR_SOUNDEX.  
  
 Il renvoie :  
  
-   SQL_FN_STR_ASCII  
  
-   SQL_FN_STR_CHAR  
  
-   SQL_FN_STR_CONCAT  
  
-   SQL_FN_STR_DIFFERENCE  
  
-   SQL_FN_STR_LCASE  
  
-   SQL_FN_STR_LEFT  
  
-   SQL_FN_STR_LENGTH  
  
-   SQL_FN_STR_LTRIM  
  
-   SQL_FN_STR_REPEAT  
  
-   SQL_FN_STR_REPLACE  
  
-   SQL_FN_STR_RIGHT  
  
-   SQL_FN_STR_RTRIM  
  
-   SQL_FN_STR_SUBSTRING  
  
-   SQL_FN_STR_UCASE  
  
-   SQL_FN_STR_SPACE.  
  
 SQL_SUBQUERIES retourne :  
  
-   SQL_SQ_CORRELATED_SUBQUERIES  
  
-   SQL_SQ_COMPARISON  
  
-   SQL_SQ_EXISTS  
  
-   SQL_SQ_IN  
  
-   SQL_SQ_QUANTIFIED.  
  
 SQL_SYSTEM_FUNCTIONS retourne :  
  
-   SQL_FN_SYS_DBNAME  
  
-   SQL_FN_SYS_IFNULL  
  
 mais pas :  
  
-   SQL_FN_SYS_USERNAME  
  
## <a name="t"></a>T  
 SQL_TABLE_TERM retourne « table ».  
  
 SQL_TIMEDATE_ADD_INTERVALS retourne :  
  
-   SQL_FN_TSI_ SECONDE  
  
-   SQL_FN_TSI_MINUTE  
  
-   SQL_FN_TSI_HOUR  
  
-   SQL_FN_TSI_DAY  
  
-   SQL_FN_TSI_MONTH  
  
-   SQL_FN_TSI_YEAR  
  
 mais pas :  
  
-   SQL_FN_TSI_FRAC_SECOND  
  
-   SQL_FN_TSI_WEEK  
  
-   SQL_FN_TSI_QUARTER  
  
 SQL_TIMEDATE_DIFF_INTERVALS retourne :  
  
-   SQL_FN_TSI_ SECONDE  
  
-   SQL_FN_TSI_MINUTE  
  
-   SQL_FN_TSI_HOUR  
  
-   SQL_FN_TSI_DAY  
  
-   SQL_FN_TSI_MONTH  
  
-   SQL_FN_TSI_YEAR  
  
 SQL_TIMEDATE_FUNCTIONS ne prend pas en charge SQL_FN_TD_QUARTER, SQL_FN_TD_TIMESTAMPADD, SQL_FN_TD_DAYOFYEAR ou SQL_FN_TD_WEEK.  
  
 Il renvoie :  
  
-   SQL_FN_TD_CURDATE  
  
-   SQL_FN_TD_CURTIME  
  
-   SQL_FN_TD_DAYNAME  
  
-   SQL_FN_TD_DAYOFMONTH  
  
-   SQL_FN_TD_DAYOFWEEK  
  
-   SQL_FN_TD_HOUR  
  
-   SQL_FN_TD_MINUTE  
  
-   SQL_FN_TD_MONTH  
  
-   SQL_FN_TD_MONTHNAME  
  
-   SQL_FN_TD_NOW  
  
-   SQL_FN_TD_SECOND  
  
-   SQL_FN_TD_TIMESTAMPDIFF  
  
-   SQL_FN_TD_YEAR.  
  
 SQL_TXN_CAPABLE retourne SQL_TC_DML.  
  
 SQL_TXN_ISOLATION_OPTION retourne SQL_TXN_READ_COMMITTED.  
  
## <a name="u-z"></a>U-Z  
 SQL_UNION retourne SQL_U_UNION ou SQL_U_UNION_ALL.  
  
 SQL_USER_NAME retourne \<vide >.
