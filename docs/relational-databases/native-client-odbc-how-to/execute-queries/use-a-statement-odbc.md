---
title: Utiliser une instruction (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-how-to
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- statements [ODBC]
ms.assetid: f7573f8f-6f21-4e03-8dd5-a5f2ea4878cc
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7a0689e169fd968d8733b003f5d638d8c31c39b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-a-statement-odbc"></a>Utiliser une instruction (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

    
### <a name="to-use-a-statement"></a>Pour utiliser une instruction  
  
1.  Appelez [SQLAllocHandle](http://go.microsoft.com/fwlink/?LinkId=58396) avec SQL_HANDLE_STMT comme *HandleType* de manière à allouer un descripteur d’instruction.  
  
2.  Si vous le souhaitez, appelez [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) pour définir des options d’instruction ou [SQLGetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) pour obtenir des attributs d’instruction.  
  
     Pour utiliser des curseurs côté serveur, vous devez affecter aux attributs de curseur des valeurs autres que leurs valeurs par défaut.  
  
3.  Si vous le souhaitez et si l'instruction doit être exécutée plusieurs fois, préparez son exécution avec la fonction [SQLPrepare](http://go.microsoft.com/fwlink/?LinkId=59360).  
  
4.  Si vous le souhaitez et si l'instruction a des marqueurs de paramètres liés, liez ces marqueurs de paramètres à des variables de programme à l'aide de [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md). Si l'instruction a été préparée, vous pouvez appeler [SQLNumParams](http://go.microsoft.com/fwlink/?LinkId=58404) et [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) to find the number et characteristics of the parameters.  
  
5.  Exécutez une instruction directement en utilisant SQLExecDirect.  
  
     \- ou -  
  
     Si l'instruction a été préparée, exécutez-la plusieurs fois à l'aide de [SQLExecute](http://go.microsoft.com/fwlink/?LinkId=58400).  
  
     \- ou -  
  
     Appelez une fonction de catalogue qui retourne des résultats.  
  
6.  Traitez les résultats en liant les colonnes du jeu de résultats aux variables de programme, en déplaçant des données des colonnes de jeu de résultats vers des variables de programme à l'aide de [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md), ou une combinaison des deux méthodes.  
  
     Extrayez une ligne à la fois dans le jeu de résultats d'une instruction.  
  
     \- ou -  
  
     Extrayez plusieurs lignes à la fois dans le jeu de résultats en utilisant un curseur de bloc.  
  
     \- ou -  
  
     Appelez [SQLRowCount](../../../relational-databases/native-client-odbc-api/sqlrowcount.md) pour déterminer le nombre de lignes affectées par une instruction INSERT, UPDATE ou DELETE.  
  
     Si l'instruction SQL peut avoir plusieurs jeux de résultats, appelez [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) à la fin de chaque jeu de résultats pour savoir si des jeux de résultats supplémentaires doivent être traités.  
  
7.  Une fois les résultats traités, les actions suivantes peuvent être nécessaires pour rendre le descripteur d'instruction disponible pour exécuter une nouvelle instruction :  
  
    -   Si vous n'avez pas appelé [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) jusqu'à ce qu'il ait retourné SQL_NO_DATA, appelez [SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md) pour fermer le curseur.  
  
    -   Si vous avez lié des marqueurs de paramètres à des variables de programme, appelez [SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md) avec *Option* ayant la valeur SQL_RESET_PARAMS pour libérer les paramètres liés.  
  
    -   Si vous avez lié des colonnes de jeu de résultats à des variables de programme, appelez [SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md) avec *Option* ayant la valeur SQL_UNBIND pour libérer les colonnes liées.  
  
    -   Pour réutiliser le descripteur d'instruction, allez à l'Étape 2.  
  
8.  Appelez [SQLFreeHandle](../../../relational-databases/native-client-odbc-api/sqlfreehandle.md) avec SQL_HANDLE_STMT comme *HandleType* pour libérer le descripteur d’instruction.  
  
## <a name="see-also"></a>Voir aussi  
 [L’exécution de rubriques de procédures de requêtes & #40 ; ODBC & #41 ;](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
  
