---
title: 'Comment : insérer des lignes dans la colonne de géographie (ODBC) | Documents Microsoft'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-how-to
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 0b6516f7-1fc0-4b01-a2d0-add0571070d5
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d87e74ee6e34aedb3cef80b6a05eb8aa7241c995
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-insert-rows-into-geography-column-odbc"></a>Procédure : insérer des lignes dans la colonne de géographie (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Cet exemple insère deux lignes dans une table avec une colonne de type geography à partir d'une entrée WKB (Well-Known Binary) à l'aide de 2 liaisons différentes (SQLCCHAR et SQLCBINARY). Il sélectionne ensuite une ligne de cette table et utilise ::STAsText() pour l'afficher. La représentation WKB est 0x01010000000700ECFAD03A4C4001008000B5DF07C0 et l'application se charge de l'impression sur la console : POINT(56.4595 -2.9842).  
  
 Cet exemple ne requiert pas de source de données ODBC, mais s'exécute par défaut sur l'instance locale de SQL Server.  
  
 Il ne fonctionne pas avec les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 Pour plus d’informations sur le stockage spatial, consultez [les données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md).  
  
## <a name="example"></a>Exemple  
 La première liste de code ([!INCLUDE[tsql](../../includes/tsql-md.md)]) crée une table utilisée par cet exemple.  
  
 Compilez la deuxième liste de code (C++) avec odbc32.lib et user32.lib. Assurez-vous que votre variable d'environnement INCLUDE inclut le répertoire qui contient sqlncli.h.  
  
 Si vous générez et exécutez cet exemple comme une application 32 bits sur un système d'exploitation 64 bits, vous devez créer la source de données ODBC avec l'administrateur ODBC dans %windir%\SysWOW64\odbcad32.exe.  
  
 Cet exemple vous permet de vous connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par défaut de votre ordinateur. Pour vous connecter à une instance nommée, modifiez la définition de la source de données ODBC pour spécifier l'instance en utilisant le format suivant : serveur\namedinstance. Par défaut, [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] est installé dans une instance nommée.  
  
 La troisième liste de code ([!INCLUDE[tsql](../../includes/tsql-md.md)]) supprime la table utilisée par cet exemple.  
  
```  
use tempdb  
GO  
  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'SpatialSample')  
   DROP TABLE SpatialSample  
  
CREATE TABLE SpatialSample (Name varchar(10), Geog Geography)  
GO  
```  
  
```  
// compile with: odbc32.lib user32.lib  
#include <windows.h>  
#include <Sqlext.h>  
#include <mbstring.h>  
#include "sqlncli.h"  
#include <string.h>  
#include <stdio.h>  
  
#define MAX_DATA 1024  
#define MYSQLSUCCESS(rc) ( (rc == SQL_SUCCESS) || (rc == SQL_SUCCESS_WITH_INFO) )  
  
SQLCHAR szDSN[] = "Driver={SQL Server Native Client 10.0};Server=.;Database=tempdb;Trusted_Connection=Yes;";  
  
class direxec {  
      RETCODE rc;   // ODBC return code  
      HENV henv;   // Environment  
      HDBC hdbc;   // Connection Handle  
      HSTMT hstmt;   // Statement Handle  
      SQLHDESC hdesc;   // Descriptor handle  
      SQLCHAR szData[MAX_DATA];   // Returned Data Storage  
      SDWORD cbData;   // Output Lenght of data   
  
      SQLCHAR szConnStrOut[MAX_DATA + 1];  
      SWORD swStrLen;  
  
public:  
      void sqlconn();   // Allocate env, stat and conn  
      void sqldisconn();   // Free pointers to env, stat, conn and disconnect  
      void error_out();   // Display errors  
      void check_rc(RETCODE rc);   // Checks for success of the return code  
      void SqlInsertFromChar();   // Insert a WKB in character form  
      void SqlInsertFromBinary();   // Insert a WKB in binary form   
      void SqlSelectGeogAsText(); // Retrieve the geography as Text.  
};   
  
// Allocate environment handles, connection handle, connect to data source, and allocate statement handle  
void direxec::sqlconn() {  
      // Allocate the enviroment handle  
      rc = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
      check_rc(rc);  
  
      // Set the ODBC version to version 3  
      rc = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
      check_rc(rc);  
  
      // Allocate the database connection handle  
      rc = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
      check_rc(rc);  
  
      // Connect to the database  
      rc = SQLDriverConnect(hdbc, NULL, szDSN, SQL_NTS, szConnStrOut, MAX_DATA, &swStrLen, SQL_DRIVER_NOPROMPT);  
      check_rc(rc);  
  
      // Allocate the statement handle  
      rc = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
      check_rc(rc);    
  
      // Allocate the descriptor handle  
      rc = rc = SQLAllocHandle(SQL_HANDLE_DESC, hdbc, &hdesc);  
      check_rc(rc);  
}   
  
// Display error message from the DiagRecord  
void direxec::error_out() {  
      // String to hold the SQL State  
      SQLCHAR szSQLSTATE[10];   
  
      // Error code  
      SDWORD nErr;  
  
      // The error message  
      SQLCHAR msg[SQL_MAX_MESSAGE_LENGTH + 1];  
  
      // Size of the message  
      SWORD cbmsg;  
  
      // If hstmt is not null use that for getting the DiagRec  
      if (hstmt)  
            rc = SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, 1, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg);  
      // else get the diag record from the env  
      else  
            rc = SQLGetDiagRec(SQL_HANDLE_ENV, henv, 1, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg);  
  
      // If the rc is successful, show the message using a message box  
      if ( rc == SQL_SUCCESS) {  
            printf((char *)szData, "Error:\nSQLSTATE=%s,Native error=%ld, msg='%s'", szSQLSTATE, nErr, msg);  
            MessageBox(NULL, (const char *)szData, "ODBC Error", MB_OK);  
      }  
}  
  
// Checks the return code.  If failure, displays the error, free the memory and exits the program  
void direxec::check_rc(RETCODE rc) {  
      if (!MYSQLSUCCESS(rc)) {  
            error_out();  
            SQLFreeEnv(henv);  
            SQLFreeConnect(hdbc);  
            exit(-1);  
      }   
}  
  
void direxec::SqlInsertFromBinary() {     
      rc = SQLPrepare(hstmt, (SQLCHAR*) "INSERT INTO SpatialSample(Name,Geog) values('Sample1',Geography::STGeomFromWKB(?,4326))", SQL_NTS);  
      check_rc(rc);  
  
      SQLCHAR szBytes [] = "\x01\x01\x00\x00\x00\x07\x00\xEC\xFA\xD0\x3A\x4C\x40\x01\x00\x80\x00\xB5\xDF\x07\xC0";  
      SQLLEN iDataLength = sizeof(szBytes)-1;  
  
      rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_BINARY, SQL_VARBINARY, 100, 0, szBytes, sizeof(szBytes), &iDataLength);  
      check_rc(rc);  
  
      rc = SQLExecute(hstmt);  
      check_rc(rc);  
}  
  
void direxec::SqlInsertFromChar() {     
      rc = SQLPrepare(hstmt, (SQLCHAR*) "INSERT INTO SpatialSample(Name,Geog) values('Sample2',Geography::STGeomFromWKB(?,4326))", SQL_NTS);  
      check_rc(rc);  
  
      SQLCHAR szBytes [] = "01010000000700ECFAD03A4C4001008000B5DF07C0";  
  
      rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARBINARY, 100, 0, szBytes, sizeof(szBytes), NULL);  
      check_rc(rc);  
  
      rc = SQLExecute(hstmt);  
      check_rc(rc);  
}  
  
void direxec::SqlSelectGeogAsText() {  
      rc = SQLFreeStmt(hstmt, SQL_CLOSE);  
      check_rc(rc);   
  
      rc = SQLExecDirect(hstmt, (SQLCHAR*) "SELECT geog.STAsText() FROM SpatialSample", SQL_NTS);  
      check_rc(rc);   
  
      SQLCHAR rgcAsText[MAX_DATA];  
      SQLLEN cbAsText;   
  
      rc = SQLBindCol(hstmt, 1, SQL_C_CHAR, rgcAsText, sizeof(rgcAsText), &cbAsText);  
      check_rc(rc);  
  
      rc = SQLFetch(hstmt);  
      check_rc(rc);  
  
      rgcAsText[cbAsText] = '\0';  
      printf("%s\r\n", (LPSTR)rgcAsText);  
}   
  
int main() {  
      direxec x;  
  
      // Allocate handles, and connect.  
      x.sqlconn();    
  
      // Insert 2 samples into the table  
      x.SqlInsertFromChar();  
      x.SqlInsertFromBinary();  
  
      // Select 1 row from the table and display the geography as text  
      x.SqlSelectGeogAsText();  
}  
```  
  
```  
use tempdb  
GO  
  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'SpatialSample')  
   DROP TABLE SpatialSample  
GO  
```  
  
  
