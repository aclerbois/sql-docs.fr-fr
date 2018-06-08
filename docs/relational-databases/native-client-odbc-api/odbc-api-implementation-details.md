---
title: Détails d’implémentation ODBC API | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- ODBC, functions
- SQL Server Native Client ODBC driver, SQL Server-specific behaviors
- ODBC, SQL Server-specific behaviors
- functions [ODBC]
ms.assetid: dca92489-f179-4b1f-997c-adcc46aa17a3
caps.latest.revision: 45
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d047d5278c21173bac8bb70fa9320dbb2187d095
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707517"
---
# <a name="odbc-api-implementation-details"></a>ODBC API Implementation Details
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Open Database Connectivity (ODBC) est une interface de programmation d'applications Microsoft Win32 utilisée par les applications pour accéder aux données dans des sources de données ODBC.  
  
 La référence de pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ne documente pas tous les appels de fonction ODBC. Seules les fonctions qui ont des paramètres ou des comportements spécifiques au pilote en cas d'utilisation avec le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sont discutées.  
  
 Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client est conforme avec la spécification ODBC 3.51. Pour obtenir une référence complète de ODBC 3.51, téléchargez Microsoft Data Access Components SDK à partir de la [accès aux données et le centre de développement](http://go.microsoft.com/fwlink?linkid=4173), ou d’afficher le [de référence du programmeur ODBC](http://go.microsoft.com/fwlink/?LinkId=45250) en ligne.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)  
  
-   [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md)  
  
-   [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
-   [SQLCancel](../../relational-databases/native-client-odbc-api/sqlcancel.md)  
  
-   [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md)  
  
-   [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md)  
  
-   [SQLColumnPrivileges](../../relational-databases/native-client-odbc-api/sqlcolumnprivileges.md)  
  
-   [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md)  
  
-   [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md)  
  
-   [SQLConnect](../../relational-databases/native-client-odbc-api/sqlconnect.md)  
  
-   [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md)  
  
-   [SQLDescribeParam](../../relational-databases/native-client-odbc-api/sqldescribeparam.md)  
  
-   [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md)  
  
-   [SQLDrivers](../../relational-databases/native-client-odbc-api/sqldrivers.md)  
  
-   [SQLEndTran](../../relational-databases/native-client-odbc-api/sqlendtran.md)  
  
-   [SQLExecDirect](../../relational-databases/native-client-odbc-api/sqlexecdirect.md)  
  
-   [SQLExecute](../../relational-databases/native-client-odbc-api/sqlexecute.md)  
  
-   [SQLFetch](../../relational-databases/native-client-odbc-api/sqlfetch.md)  
  
-   [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)  
  
-   [SQLForeignKeys](../../relational-databases/native-client-odbc-api/sqlforeignkeys.md)  
  
-   [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md)  
  
-   [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md)  
  
-   [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)  
  
-   [SQLGetCursorName](../../relational-databases/native-client-odbc-api/sqlgetcursorname.md)  
  
-   [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md)  
  
-   [SQLGetDescField](../../relational-databases/native-client-odbc-api/sqlgetdescfield.md)  
  
-   [SQLGetDiagField](../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md)  
  
-   [SQLGetFunctions](../../relational-databases/native-client-odbc-api/sqlgetfunctions.md)  
  
-   [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md)  
  
-   [SQLGetStmtAttr](../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md)  
  
-   [SQLGetTypeInfo](../../relational-databases/native-client-odbc-api/sqlgettypeinfo.md)  
  
-   [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md)  
  
-   [SQLNativeSql](../../relational-databases/native-client-odbc-api/sqlnativesql.md)  
  
-   [SQLNumParams](../../relational-databases/native-client-odbc-api/sqlnumparams.md)  
  
-   [SQLNumResultCols](../../relational-databases/native-client-odbc-api/sqlnumresultcols.md)  
  
-   [SQLParamData](../../relational-databases/native-client-odbc-api/sqlparamdata.md)  
  
-   [SQLPrimaryKeys](../../relational-databases/native-client-odbc-api/sqlprimarykeys.md)  
  
-   [SQLProcedureColumns](../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md)  
  
-   [SQLProcedures](../../relational-databases/native-client-odbc-api/sqlprocedures.md)  
  
-   [SQLPutData](../../relational-databases/native-client-odbc-api/sqlputdata.md)  
  
-   [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md)  
  
-   [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)  
  
-   [SQLSetDescField](../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)  
  
-   [SQLSetDescRec](../../relational-databases/native-client-odbc-api/sqlsetdescrec.md)  
  
-   [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md)  
  
-   [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)  
  
-   [SQLSpecialColumns](../../relational-databases/native-client-odbc-api/sqlspecialcolumns.md)  
  
-   [SQLStatistics](../../relational-databases/native-client-odbc-api/sqlstatistics.md)  
  
-   [SQLTablePrivileges](../../relational-databases/native-client-odbc-api/sqltableprivileges.md)  
  
-   [SQLTables](../../relational-databases/native-client-odbc-api/sqltables.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;ODBC&#41; référence](http://msdn.microsoft.com/library/06b7edee-8636-49d9-9b5c-2c710bf4fa2d)   
 [Génération d’applications avec SQL Server Native Client](../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
