---
title: Traiter les résultats (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-how-to
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- processing results [ODBC]
ms.assetid: 4810fe3f-78ee-4f0d-8bcc-a4659fbcf46f
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 41cca358deb729fda5d9659b9b76040cf8c3cc50
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="processing-results---process-results"></a>Le traitement des résultats - résultats du processus
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

Le traitement des résultats dans une application ODBC implique tout d’abord déterminer les caractéristiques du jeu de résultats, puis récupérer les données dans des variables de programme à l’aide [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) ou [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md).  
  
### <a name="to-process-results"></a>Pour traiter des résultats  
  
1.  Récupérez les informations du jeu de résultats.  
  
2.  Si des colonnes dépendantes sont utilisées, pour chaque colonne à lier, appelez [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) pour lier une mémoire tampon de programme à la colonne.  
  
3.  Pour chaque ligne de l'ensemble de résultats :  
  
    -   Appelez [SQLFetch](http://go.microsoft.com/fwlink/?LinkId=58401) pour obtenir la ligne suivante.  
  
    -   Si les colonnes dépendantes sont utilisées, utilisez les données à présent disponibles dans les mémoires tampons des colonnes dépendantes.  
  
    -   Si des colonnes indépendantes sont utilisées, appelez [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) une ou plusieurs fois pour obtenir les données pour les colonnes indépendantes après la dernière colonne dépendante. Les appels à **SQLGetData** doivent être dans l’ordre croissant des numéros de colonnes.  
  
    -   Appelez plusieurs fois **SQLGetData** pour obtenir des données à partir d’une colonne text ou image.  
  
4.  Quand [SQLFetch](http://go.microsoft.com/fwlink/?LinkId=58401) signale la fin du jeu de résultats en retournant SQL_NO_DATA, appelez [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) pour déterminer si un autre jeu de résultats est disponible.  
  
    -   S'il retourne SQL_SUCCESS, un autre jeu de résultats est disponible.  
  
    -   S'il retourne SQL_NO_DATA, aucun autre jeu de résultats n'est disponible.  
  
    -   S’il retourne SQL_SUCCESS_WITH_INFO ou SQL_ERROR, appelez [SQLGetDiagRec](http://go.microsoft.com/fwlink/?LinkId=58402) pour déterminer si la sortie à partir d’une instruction PRINT ou RAISERROR est disponible.  
  
         Si des paramètres d'instruction liés sont utilisés pour les paramètres de sortie ou la valeur de retour d'une procédure stockée, utilisez les données à présent disponibles dans les mémoires tampons de paramètres liés. Par ailleurs, quand des paramètres liés sont utilisés, chaque appel à [SQLExecute](http://go.microsoft.com/fwlink/?LinkId=58400) ou à [SQLExecDirect](http://go.microsoft.com/fwlink/?LinkId=58399) aura exécuté l’instruction SQL *S* fois, où *S* est le nombre d’éléments présents dans le tableau de paramètres liés. Cela signifie qu’il y aura *S* jeux de résultats à traiter, où chaque jeu de résultats comprend l’ensemble des jeux de résultats, des paramètres de sortie et des codes de retour habituellement retournés par une exécution unique de l’instruction SQL.  
  
    > [!NOTE]  
    >  Lorsqu'un jeu de résultats contient des lignes calculées, chaque ligne calculée est rendue disponible comme un jeu de résultats distinct. Ces jeux de résultats calculés sont intercalés au sein des lignes normales et séparent les lignes normales en plusieurs jeux de résultats.  
  
5.  Vous pouvez également appeler [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) avec SQL_UNBIND pour libérer tous les tampons de colonnes liées éventuels.  
  
6.  Si un autre jeu de résultats est disponible, allez à l’étape 1.  
  
> [!NOTE]  
>  Pour annuler le traitement d’un jeu de résultats avant que [SQLFetch](http://go.microsoft.com/fwlink/?LinkId=58401) ne retourne SQL_NO_DATA, appelez [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md).  
  
## <a name="see-also"></a>Voir aussi  
[Récupérer les informations du jeu de résultats & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-how-to/processing-results-retrieve-result-set-information.md)   
  
  
