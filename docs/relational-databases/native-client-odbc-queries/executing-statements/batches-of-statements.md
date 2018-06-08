---
title: Lots d’instructions | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-queries
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- statements [ODBC], batches
- batches [ODBC]
- ODBC applications, statements
- multiple statements, batches
- SQLMoreResults function
- SQLExecDirect function
ms.assetid: 057d7c1c-1428-4780-9447-a002ea741188
caps.latest.revision: 36
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 87fc93dc92da66af4371543a2650bad673546b0d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="batches-of-statements"></a>Lots d'instructions
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Un lot de [!INCLUDE[tsql](../../../includes/tsql-md.md)] instructions contient plusieurs instructions, séparées par un point-virgule ( ;), intégrées dans une chaîne unique passée à **SQLExecDirect** ou [fonction SQLPrepare](http://go.microsoft.com/fwlink/?LinkId=59360). Par exemple :  
  
```  
SQLExecDirect(hstmt,   
    "SELECT * FROM Authors; SELECT * FROM Titles",  
    SQL_NTS);  
```  
  
 Les lots peuvent se révéler plus efficaces que la soumission des instructions séparément car le trafic réseau est souvent réduit. Utilisez [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) pour vous positionner sur le prochain jeu de résultats lorsque s’est terminé avec le jeu de résultats actuel.  
  
 Les lots peuvent toujours être utilisés lorsque les attributs de curseur ODBC sont définis sur les valeurs par défaut d'un curseur avant uniquement en lecture seule avec une taille d'ensemble de lignes de 1.  
  
 Si un lot est exécuté lors de l'utilisation de curseurs côté serveur contre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le curseur côté serveur est converti implicitement en un jeu de résultats par défaut. **SQLExecDirect** ou **SQLExecute** retourne SQL_SUCCESS_WITH_INFO et un appel à **SQLGetDiagRec** retourne :  
  
```  
szSqlState = "01S02", pfNativeError = 0  
szErrorMsg = "[Microsoft][SQL Server Native Server Native Client]Cursor type changed."  
```  
  
## <a name="see-also"></a>Voir aussi  
 [L’exécution d’instructions & #40 ; ODBC & #41 ;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
  
