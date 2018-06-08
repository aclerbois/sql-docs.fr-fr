---
title: Référence de l’API ODBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: dllExport
ms.assetid: b7a49774-f458-44ce-9a04-a0457501405b
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c3920299ded492ef40bee8bd181e5ea60aaa9960
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-api-reference"></a>Référence de l’API ODBC
Les rubriques de cette section décrivent chaque fonction ODBC dans l’ordre alphabétique. Chaque fonction est définie en tant que fonction de langage de programmation C. Descriptions sont les suivantes :  
  
-   Fonction  
  
-   Version d’ODBC  
  
-   Niveau de conformité standard CLI  
  
-   Syntaxe  
  
-   Arguments  
  
-   Valeurs retournées  
  
-   Diagnostics  
  
-   Commentaires sur l’utilisation et d’implémentation  
  
-   Exemple de code  
  
-   Références à des fonctions connexes  
  
 Le niveau de conformité standard CLI peut prendre l’une des opérations suivantes : ISO 92, Open Group, ODBC ou obsolète. Une fonction marquée comme conforme ISO 92 apparaît également dans la version 1, ouvrir le groupe Open Group étant un sur-ensemble pur de ISO 92. Une fonction en tant que groupe ouvert-conforme apparaît également dans ODBC 3. *x*, étant donné que ODBC 3. *x* est un sur-ensemble pur de Open Group version 1. Une fonction marquée comme conforme ODBC apparaît dans aucun standard. Une fonction marquée comme déconseillée a été déconseillée dans ODBC 3. *x*.  
  
 La gestion des informations de diagnostic est décrite dans le [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) description de fonction. Le texte associé aux valeurs SQLSTATE est inclus pour fournir une description de la condition, mais n’est pas censé prescrire un texte spécifique.  
  
> [!NOTE]  
>  Pour plus d’informations spécifiques au pilote sur les fonctions ODBC, consultez la section pour le pilote.  
  
 Cette section contient des rubriques pour les fonctions suivantes :  
  
-   [SQLAllocConnect, fonction](../../../odbc/reference/syntax/sqlallocconnect-function.md)  
  
-   [SQLAllocEnv, fonction](../../../odbc/reference/syntax/sqlallocenv-function.md)  
  
-   [SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)  
  
-   [SQLAllocStmt, fonction](../../../odbc/reference/syntax/sqlallocstmt-function.md)  
  
-   [SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)  
  
-   [SQLBindParameter, fonction](../../../odbc/reference/syntax/sqlbindparameter-function.md)  
  
-   [SQLBrowseConnect, fonction](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)  
  
-   [SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)  
  
-   [SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)  
  
-   [SQLCancelHandle, fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md)  
  
-   [SQLCloseCursor, fonction](../../../odbc/reference/syntax/sqlclosecursor-function.md)  
  
-   [SQLColAttribute, fonction](../../../odbc/reference/syntax/sqlcolattribute-function.md)  
  
-   [SQLColAttributes, fonction](../../../odbc/reference/syntax/sqlcolattributes-function.md)  
  
-   [SQLColumnPrivileges, fonction](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)  
  
-   [SQLColumns, fonction](../../../odbc/reference/syntax/sqlcolumns-function.md)  
  
-   [SQLCompleteAsync, fonction](../../../odbc/reference/syntax/sqlcompleteasync-function.md)  
  
-   [SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)  
  
-   [SQLCopyDesc, fonction](../../../odbc/reference/syntax/sqlcopydesc-function.md)  
  
-   [SQLDataSources, fonction](../../../odbc/reference/syntax/sqldatasources-function.md)  
  
-   [SQLDescribeCol, fonction](../../../odbc/reference/syntax/sqldescribecol-function.md)  
  
-   [SQLDescribeParam, fonction](../../../odbc/reference/syntax/sqldescribeparam-function.md)  
  
-   [SQLDisconnect, fonction](../../../odbc/reference/syntax/sqldisconnect-function.md)  
  
-   [SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)  
  
-   [SQLDrivers, fonction](../../../odbc/reference/syntax/sqldrivers-function.md)  
  
-   [SQLEndTran, fonction](../../../odbc/reference/syntax/sqlendtran-function.md)  
  
-   [SQLError, fonction](../../../odbc/reference/syntax/sqlerror-function.md)  
  
-   [SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)  
  
-   [SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)  
  
-   [SQLExtendedFetch, fonction](../../../odbc/reference/syntax/sqlextendedfetch-function.md)  
  
-   [SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)  
  
-   [SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)  
  
-   [SQLForeignKeys, fonction](../../../odbc/reference/syntax/sqlforeignkeys-function.md)  
  
-   [SQLFreeConnect, fonction](../../../odbc/reference/syntax/sqlfreeconnect-function.md)  
  
-   [SQLFreeEnv, fonction](../../../odbc/reference/syntax/sqlfreeenv-function.md)  
  
-   [SQLFreeHandle, fonction](../../../odbc/reference/syntax/sqlfreehandle-function.md)  
  
-   [SQLFreeStmt, fonction](../../../odbc/reference/syntax/sqlfreestmt-function.md)  
  
-   [SQLGetConnectAttr, fonction](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)  
  
-   [SQLGetConnectOption, fonction](../../../odbc/reference/syntax/sqlgetconnectoption-function.md)  
  
-   [SQLGetCursorName, fonction](../../../odbc/reference/syntax/sqlgetcursorname-function.md)  
  
-   [SQLGetData, fonction](../../../odbc/reference/syntax/sqlgetdata-function.md)  
  
-   [SQLGetDescField, fonction](../../../odbc/reference/syntax/sqlgetdescfield-function.md)  
  
-   [SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)  
  
-   [SQLGetDiagField, fonction](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)  
  
-   [SQLGetDiagRec, fonction](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)  
  
-   [SQLGetEnvAttr, fonction](../../../odbc/reference/syntax/sqlgetenvattr-function.md)  
  
-   [SQLGetFunctions, fonction](../../../odbc/reference/syntax/sqlgetfunctions-function.md)  
  
-   [SQLGetInfo, fonction](../../../odbc/reference/syntax/sqlgetinfo-function.md)  
  
-   [SQLGetStmtAttr, fonction](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)  
  
-   [SQLGetStmtOption, fonction](../../../odbc/reference/syntax/sqlgetstmtoption-function.md)  
  
-   [SQLGetTypeInfo, fonction](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)  
  
-   [SQLMoreResults, fonction](../../../odbc/reference/syntax/sqlmoreresults-function.md)  
  
-   [SQLNativeSql, fonction](../../../odbc/reference/syntax/sqlnativesql-function.md)  
  
-   [SQLNumParams, fonction](../../../odbc/reference/syntax/sqlnumparams-function.md)  
  
-   [SQLNumResultCols, fonction](../../../odbc/reference/syntax/sqlnumresultcols-function.md)  
  
-   [SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)  
  
-   [SQLParamOptions, fonction](../../../odbc/reference/syntax/sqlparamoptions-function.md)  
  
-   [SQLPrepare, fonction](../../../odbc/reference/syntax/sqlprepare-function.md)  
  
-   [SQLPrimaryKeys, fonction](../../../odbc/reference/syntax/sqlprimarykeys-function.md)  
  
-   [SQLProcedureColumns, fonction](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)  
  
-   [SQLProcedures, fonction](../../../odbc/reference/syntax/sqlprocedures-function.md)  
  
-   [SQLPutData, fonction](../../../odbc/reference/syntax/sqlputdata-function.md)  
  
-   [SQLRowCount, fonction](../../../odbc/reference/syntax/sqlrowcount-function.md)  
  
-   [SQLSetConnectAttr, fonction](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)  
  
-   [SQLSetConnectOption, fonction](../../../odbc/reference/syntax/sqlsetconnectoption-function.md)  
  
-   [SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)  
  
-   [SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)  
  
-   [SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)  
  
-   [SQLSetEnvAttr, fonction](../../../odbc/reference/syntax/sqlsetenvattr-function.md)  
  
-   [SQLSetParam, fonction](../../../odbc/reference/syntax/sqlsetparam-function.md)  
  
-   [SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)  
  
-   [SQLSetScrollOptions, fonction](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)  
  
-   [SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)  
  
-   [SQLSetStmtOption, fonction](../../../odbc/reference/syntax/sqlsetstmtoption-function.md)  
  
-   [SQLSpecialColumns, fonction](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)  
  
-   [SQLStatistics, fonction](../../../odbc/reference/syntax/sqlstatistics-function.md)  
  
-   [SQLTablePrivileges, fonction](../../../odbc/reference/syntax/sqltableprivileges-function.md)  
  
-   [SQLTables, fonction](../../../odbc/reference/syntax/sqltables-function.md)  
  
-   [SQLTransact, fonction](../../../odbc/reference/syntax/sqltransact-function.md)
