---
title: Traitement d’instructions qui génèrent des Messages | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-error-messages
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- PRINT statement
- messages [ODBC], statements generating messages
- statements [ODBC], message generation
- errors [ODBC], statements generating messages
- SQL Server Native Client ODBC driver, errors
- STATISTICS IO option
- STATISTICS TIME option
- DBCC statements
- SQLExecute function
- RAISERROR statement
- SQLGetDiagRec function
- ODBC error handling, statements generating messages
- SQLExecDirect function
ms.assetid: 672ebdc5-7fa1-4ceb-8d52-fd25ef646654
caps.latest.revision: 32
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9475afda740b4c2b2d6b279c0ea0a454fd397bf0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="processing-statements-that-generate-messages"></a>Traitement des instructions qui génèrent des messages
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Les options STATISTICS TIME et STATISTICS IO de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] SET sont utilisées pour obtenir des informations qui aident au diagnostic des requêtes longues. Les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prennent aussi en charge l'option SHOWPLAN pour analyser les plans de requête. Une application ODBC peut définir ces options en exécutant les instructions suivantes :  
  
```  
SQLExecDirect(hstmt, "SET SHOWPLAN ON", SQL_NTS);  
SQLExecDirect(hstmt, "SET STATISTICS TIME ON", SQL_NTS90  
);  
SQLExecDirect(hstmt, "SET STATISTICS IO ON", SQL_NTS);  
```  
  
 Lorsque SET STATISTICS TIME ou SET SHOWPLAN ont la valeur ON, **SQLExecute** et **SQLExecDirect** retourne SQL_SUCCESS_WITH_INFO, et, à ce stade, l’application peut récupérer la sortie SHOWPLAN ou STATISTICS TIME en appelant **SQLGetDiagRec** jusqu'à ce qu’il retourne SQL_NO_DATA. Chaque ligne des données SHOWPLAN est retournée au format suivant :  
  
```  
szSqlState="01000", *pfNativeError=6223,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]   
              Table Scan"  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0 a remplacé l'option SHOWPLAN par SHOWPLAN_ALL et SHOWPLAN_TEXT, qui retournent l'une et l'autre la sortie comme jeu de résultats, et non comme ensemble de messages.  
  
 Chaque ligne de STATISTICS TIME est retournée au format suivant :  
  
```  
szSqlState="01000", *pfNativeError= 3613,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
   SQL Server Parse and Compile Time: cpu time = 0 ms."  
```  
  
 La sortie de SET STATISTICS IO n'est pas disponible jusqu'à la fin d'un jeu de résultats. Pour obtenir la sortie STATISTICS IO, l’application appelle **SQLGetDiagRec** au moment **SQLFetch** ou [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) retourne SQL_NO_DATA. La sortie de STATISTICS IO est retournée au format suivant :  
  
```  
szSqlState="01000", *pfNativeError= 3615,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Table: testshow  scan count 1,  logical reads: 1,  
   physical reads: 0."  
```  
  
## <a name="using-dbcc-statements"></a>Utilisation des instructions DBCC  
 Les instructions DBCC retournent leurs données comme messages, non comme jeux de résultats. **SQLExecDirect** ou **SQLExecute** de retour SQL_SUCCESS_WITH_INFO et l’application récupère la sortie en appelant **SQLGetDiagRec** jusqu'à ce qu’il retourne SQL_NO_DATA.  
  
 Par exemple, l'instruction suivante retourne SQL_SUCCESS_WITH_INFO :  
  
```  
SQLExecDirect(hstmt, "DBCC CHECKTABLE(Authors)", SQL_NTS);  
```  
  
 Les appels à **SQLGetDiagRec** retourner :  
  
```  
szSqlState = "01000", *pfNativeError = 2536,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Checking authors"  
szSqlState = "01000", *pfNativeError = 2579,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   The total number of data pages in this table is 1."  
szSqlState = "01000", *pfNativeError = 7929,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Table has 23 data rows."  
szSqlState = "01000", *pfNativeError = 2528  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   DBCC execution completed. If DBCC printed error messages,  
   see your System Administrator."  
```  
  
## <a name="using-print-and-raiserror-statements"></a>Utilisation des instructions PRINT et RAISERROR  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] Instructions PRINT et RAISERROR retournent aussi les données en appelant **SQLGetDiagRec**. Instructions PRINT provoquent l’exécution d’instruction SQL retourner SQL_SUCCESS_WITH_INFO et un appel ultérieur à **SQLGetDiagRec** retourne un *SQLState* de 01000. Une erreur RAISERROR avec une gravité égale ou inférieure à dix se comporte de la même façon que PRINT. Une erreur RAISERROR avec une gravité de 11 ou plus provoque l’exécution à retourner SQL_ERROR et un appel ultérieur à **SQLGetDiagRec** retourne *SQLState* 42000. Par exemple, l'instruction suivante retourne SQL_SUCCESS_WITH_INFO :  
  
```  
SQLExecDirect (hstmt, "PRINT  'Some message' ", SQL_NTS);  
```  
  
 Appel de **SQLGetDiagRec** retourne :  
  
```  
szSQLState = "01000", *pfNative Error = 0,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Some message"  
```  
  
 L'instruction suivante retourne SQL_SUCCESS_WITH_INFO :  
  
```  
SQLExecDirect (hstmt, "RAISERROR ('Sample error 1.', 10, -1)",  
   SQL_NTS)  
```  
  
 Appel de **SQLGetDiagRec** retourne :  
  
```  
szSQLState = "01000", *pfNative Error = 50000,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Sample error 1."  
```  
  
 L'instruction suivante retourne SQL_ERROR :  
  
```  
SQLExecDirect (hstmt, "RAISERROR ('Sample error 2.', 11, -1)", SQL_NTS)  
```  
  
 Appel de **SQLGetDiagRec** retourne :  
  
```  
szSQLState = "42000", *pfNative Error = 50000,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Sample error 2."  
```  
  
 Le minutage de l’appel de **SQLGetDiagRec** est essentiel lors de la sortie des instructions PRINT ou RAISERROR est inclus dans un jeu de résultats. L’appel à **SQLGetDiagRec** pour récupérer le PRINT ou RAISERROR sortie doit être effectuée immédiatement après l’instruction qui reçoit SQL_ERROR ou SQL_SUCCESS_WITH_INFO. Cela est simple lorsqu'une seule instruction SQL est exécutée, comme dans les exemples ci-dessus. Dans ce cas, l’appel à **SQLExecDirect** ou **SQLExecute** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO et **SQLGetDiagRec** peut ensuite être appelée. La situation est moins simple lors du codage de boucles pour gérer la sortie d'un lot d'instructions SQL ou lors de l'exécution de procédures stockées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un jeu de résultats pour chaque instruction SELECT exécutée dans un lot ou une procédure stockée. Si le lot ou la procédure contient les instructions PRINT ou RAISERROR, leur sortie est entrelacée avec les jeux de résultats des instructions SELECT. Si la première instruction du lot ou une procédure est PRINT ou RAISERROR, la **SQLExecute** ou **SQLExecDirect** retourne SQL_SUCCESS_WITH_INFO ou SQL_ERROR et l’application doit appeler **SQLGetDiagRec** jusqu'à ce qu’il retourne SQL_NO_DATA pour extraire les informations PRINT ou RAISERROR.  
  
 Si l’instruction PRINT ou RAISERROR vient après une instruction SQL (par exemple, une instruction SELECT), puis les informations PRINT ou RAISERROR sont retournées lorsque [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md)positions le résultat de la valeur qui contient l’erreur. **SQLMoreResults** retourne SQL_SUCCESS_WITH_INFO ou SQL_ERROR selon la gravité du message. Les messages sont récupérés en appelant **SQLGetDiagRec** jusqu'à ce qu’il retourne SQL_NO_DATA.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des erreurs et des messages](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
