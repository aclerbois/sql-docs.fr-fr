---
title: Fonction de mise en conformité | Documents Microsoft
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
- conformance levels [ODBC], function
- function conformance levels [ODBC]
- data sources [ODBC], conformance levels
- ODBC drivers [ODBC], conformance levels
ms.assetid: bb5d68cf-d238-481e-babc-2e9401b4700e
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 182b53071f5d7d4c3486a84f789954e12772e0bc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="function-conformance"></a>Mise en conformité (fonction)
Le tableau suivant indique le niveau de conformité de chaque fonction ODBC, où cela est bien défini.  
  
|Fonction|Niveau de conformité|  
|--------------|-----------------------|  
|**SQLAllocHandle**|Noyau|  
|**SQLBindCol**|Noyau|  
|**SQLBindParameter**|Principaux [1]|  
|**SQLBrowseConnect**|Niveau 1|  
|**SQLBulkOperations**|Niveau 1|  
|**SQLCancel**|Principaux [1]|  
|**SQLCloseCursor**|Noyau|  
|**SQLColAttribute**|Principaux [1]|  
|**SQLColumnPrivileges**|Niveau 2|  
|**SQLColumns**|Noyau|  
|**SQLConnect**|Noyau|  
|**SQLCopyDesc**|Noyau|  
|**SQLDataSources**|Noyau|  
|**SQLDescribeCol**|Principaux [1]|  
|**SQLDescribeParam**|Niveau 2|  
|**SQLDisconnect**|Noyau|  
|**SQLDriverConnect**|Noyau|  
|**SQLDrivers**|Noyau|  
|**SQLEndTran**|Principaux [1]|  
|**SQLExecDirect**|Noyau|  
|**SQLExecute**|Noyau|  
|**SQLFetch**|Noyau|  
|**SQLFetchScroll**|Principaux [1]|  
|**SQLForeignKeys**|Niveau 2|  
|**SQLFreeHandle**|Noyau|  
|**SQLFreeStmt**|Noyau|  
|**SQLGetConnectAttr**|Noyau|  
|**SQLGetCursorName**|Noyau|  
|**SQLGetData**|Noyau|  
|**SQLGetDescField**|Noyau|  
|**SQLGetDescRec**|Noyau|  
|**SQLGetDiagField**|Noyau|  
|**SQLGetDiagRec**|Noyau|  
|**Cas**|Noyau|  
|**SQLGetFunctions**|Noyau|  
|**SQLGetInfo**|Noyau|  
|**SQLGetStmtAttr**|Noyau|  
|**SQLGetTypeInfo**|Noyau|  
|**SQLMoreResults**|Niveau 1|  
|**SQLNativeSql**|Noyau|  
|**SQLNumParams**|Noyau|  
|**SQLNumResultCols**|Noyau|  
|**SQLParamData**|Noyau|  
|**SQLPrepare**|Noyau|  
|**SQLPrimaryKeys**|Niveau 1|  
|**SQLProcedureColumns**|Niveau 1|  
|**SQLProcedures**|Niveau 1|  
|**SQLPutData**|Noyau|  
|**SQLRowCount**|Noyau|  
|**SQLSetConnectAttr**|Principaux [2]|  
|**SQLSetCursorName**|Noyau|  
|**SQLSetDescField**|Principaux [1]|  
|**SQLSetDescRec**|Noyau|  
|**SQLSetEnvAttr**|Principaux [2]|  
|**SQLSetPos**|Niveau 1 [1]|  
|**SQLSetStmtAttr**|Principaux [2]|  
|**SQLSpecialColumns**|Principaux [1]|  
|**SQLStatistics**|Noyau|  
|**SQLTablePrivileges**|Niveau 2|  
|**SQLTables**|Noyau|  
  
 [1] fonctionnalités d’importantes de cette fonction sont disponibles uniquement à des niveaux de conformité plus élevés.  
  
 [2] définition de certains attributs pour les valeurs par défaut dépend du niveau de conformité. Pour plus d’informations, consultez la section suivante, [mise en conformité de l’attribut](../../../odbc/reference/develop-app/attribute-conformance.md).
