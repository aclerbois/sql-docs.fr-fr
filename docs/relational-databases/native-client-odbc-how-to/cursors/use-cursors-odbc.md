---
title: Utiliser des curseurs (ODBC) | Documents Microsoft
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
- cursors [ODBC], how to topics
ms.assetid: c502736f-bca0-45c3-ae25-d2ad52d296bf
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1c6d5a5e0a2b113afe01f164d6701e26c1469ab6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-cursors-odbc"></a>Utiliser des curseurs (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

    
### <a name="to-use-cursors"></a>Pour utiliser des curseurs  
  
1.  Appelez [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) pour définir les attributs de curseur souhaités :  
  
     Définissez les attributs SQL_ATTR_CURSOR_TYPE et SQL_ATTR_CONCURRENCY (option par défaut).  
  
     ou  
  
     Définissez les attributs SQL_CURSOR_SCROLLABLE et SQL_CURSOR_SENSITIVITY.  
  
2.  Appelez [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) pour définir la taille de l’ensemble de lignes en utilisant l’attribut SQL_ATTR_ROW_ARRAY_SIZE.  
  
3.  Vous pouvez également appeler [SQLSetCursorName](http://go.microsoft.com/fwlink/?LinkId=58406) pour définir un nom de curseur si des mises à jour positionnées seront effectuées à l’aide de la clause WHERE CURRENT OF.  
  
4.  Exécutez l'instruction SQL.  
  
5.  Vous pouvez également appeler [SQLGetCursorName](../../../relational-databases/native-client-odbc-api/sqlgetcursorname.md) pour obtenir le nom de curseur si des mises à jour positionnées seront effectuées à l’aide de la clause WHERE CURRENT OF et si un nom de curseur n’a pas été fourni avec [SQLSetCursorName](http://go.microsoft.com/fwlink/?LinkId=58406) à l’étape 3.  
  
6.  Appelez [SQLNumResultCols](../../../relational-databases/native-client-odbc-api/sqlnumresultcols.md) pour obtenir le nombre de colonnes (C) dans l’ensemble de lignes.  
  
     Utilisez la liaison selon les colonnes  
  
     \- ou -  
  
     Utilisez la liaison selon les lignes.  
  
7.  Extrayez des ensembles de lignes à partir du curseur, comme vous le souhaitez.  
  
8.  Appelez [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) pour déterminer si un autre jeu de résultats est disponible.  
  
    -   S'il retourne SQL_SUCCESS, un autre jeu de résultats est disponible.  
  
    -   S'il retourne SQL_NO_DATA, aucun autre jeu de résultats n'est disponible.  
  
    -   S’il retourne SQL_SUCCESS_WITH_INFO ou SQL_ERROR, appelez [SQLGetDiagRec](http://go.microsoft.com/fwlink/?LinkId=58402) pour déterminer si la sortie à partir d’une instruction PRINT ou RAISERROR est disponible.  
  
     Si des paramètres d'instruction liés sont utilisés pour les paramètres de sortie ou la valeur de retour d'une procédure stockée, utilisez les données à présent disponibles dans les mémoires tampons de paramètres liés.  
  
     Quand des paramètres liés sont utilisés, chaque appel à [SQLExecute](http://go.microsoft.com/fwlink/?LinkId=58400) ou à [SQLExecDirect](http://go.microsoft.com/fwlink/?LinkId=58399) aura exécuté l’instruction SQL S fois, où S est le nombre d’éléments présents dans le tableau de paramètres liés. Cela signifie qu'il y aura S jeux de résultats à traiter, où chaque jeu de résultats comprend l'ensemble des jeux de résultats, des paramètres de sortie et des codes de retour habituellement retournés par une exécution unique de l'instruction SQL.  
  
     Notez que lorsqu'un jeu de résultats contient des lignes calculées, chaque ligne calculée est rendue disponible comme un jeu de résultats distinct. Ces jeux de résultats calculés sont intercalés au sein des lignes normales et séparent les lignes normales en plusieurs jeux de résultats.  
  
9. Vous pouvez également appeler [SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md) avec SQL_UNBIND pour libérer tous les tampons de colonnes liées éventuels.  
  
10. Si un autre jeu de résultats est disponible, allez à l'étape 6.  
  
     À l’étape 9, l’appel de [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) sur un jeu de résultats partiellement traité efface le reste du jeu de résultats. Une autre méthode pour effacer un jeu de résultats partiellement traité consiste à appeler [SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md).  
  
     Vous pouvez contrôler le type de curseur utilisé en définissant SQL_ATTR_CURSOR_TYPE et SQL_ATTR_CONCURRENCY ou en définissant SQL_ATTR_CURSOR_SENSITIVITY et SQL_ATTR_CURSOR_SCROLLABLE. Vous ne devez pas combiner les deux méthodes de spécification de comportement du curseur.  
  
## <a name="see-also"></a>Voir aussi  
 [À l’aide des rubriques de procédures de curseurs & #40 ; ODBC & #41 ;](../../../relational-databases/native-client-odbc-how-to/cursors/using-cursors-how-to-topics-odbc.md)  
  
  
