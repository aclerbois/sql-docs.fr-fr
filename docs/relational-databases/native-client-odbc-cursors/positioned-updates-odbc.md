---
title: Mises à jour (ODBC) positionnées | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-cursors
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- SQLSetPos function
- SQLSetCursorName function
- ODBC applications, cursors
- cursors [ODBC], positioned updates
- positioned updates [ODBC]
- ODBC cursors, positioned updates
ms.assetid: ff404e02-630f-474d-b5d4-06442b756991
caps.latest.revision: 33
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6393902a8b9a24ecac4df3ffbfff95fdac2ea686
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="positioned-updates-odbc"></a>Mises à jour positionnées (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  ODBC prend en charge deux méthodes permettant d'effectuer des mises à jour positionnées dans un curseur :  
  
-   **SQLSetPos**  
  
-   Clause WHERE CURRENT OF  
  
 L’approche la plus courante consiste à utiliser **SQLSetPos**. Il comprend les options suivantes.  
  
 SQL_POSITION  
 Positionne le curseur sur une ligne spécifique dans l'ensemble de lignes actif.  
  
 SQL_REFRESH  
 Actualise les variables de programme liées aux colonnes de jeu de résultats avec les valeurs provenant de la ligne sur laquelle le curseur est actuellement positionné.  
  
 SQL_UPDATE  
 Met à jour la ligne active du curseur avec les valeurs stockées dans les variables de programme liées aux colonnes de jeu de résultats.  
  
 SQL_DELETE  
 Supprime la ligne active du curseur.  
  
 **SQLSetPos** peuvent être utilisés avec toute instruction jeu de résultats lorsque les attributs de curseur handle d’instruction sont définis pour utiliser des curseurs côté serveur. Les colonnes de jeu de résultats doivent être liées à des variables de programme. Dès que l’application a extrait une ligne, elle appelle **SQLSetPos**(SQL_POSTION) pour positionner le curseur sur la ligne. L'application peut ensuite appeler SQLSetPos(SQL_DELETE) pour supprimer la ligne active, ou elle peut déplacer les nouvelles valeurs de données dans les variables de programme liées et appeler SQLSetPos(SQL_UPDATE) pour mettre à jour la ligne active.  
  
 Les applications peuvent mettre à jour ou supprimer les lignes de l’ensemble de lignes avec **SQLSetPos**. Appel de **SQLSetPos** est une alternative pratique à la construction et l’exécution d’une instruction SQL. **SQLSetPos** fonctionne sur l’ensemble de lignes en cours et peut être utilisé uniquement après un appel à [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md).  
  
 Taille de l’ensemble de lignes est définie par un appel à [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) avec un argument d’attribut SQL_ATTR_ROW_ARRAY_SIZE. **SQLSetPos** utilise une nouvelle taille de l’ensemble de lignes, mais uniquement après un appel à **SQLFetch** ou **SQLFetchScroll**. Par exemple, si la taille de l’ensemble de lignes est modifiée, **SQLSetPos** est appelée, puis **SQLFetch** ou **SQLFetchScroll** est appelée. L’appel à **SQLSetPos** utilise l’ancienne taille de l’ensemble de lignes, mais **SQLFetch** ou **SQLFetchScroll** utilise la nouvelle taille de l’ensemble de lignes.  
  
 La première ligne de l'ensemble de lignes porte le numéro 1. L’argument RowNumber de **SQLSetPos** doit identifier une ligne dans l’ensemble de lignes ; autrement dit, sa valeur doit être comprise entre 1 et le nombre de lignes qui ont été extraites le plus récemment. Cette valeur peut être inférieure à la taille de l'ensemble de lignes. Si RowNumber a la valeur 0, l'opération s'applique à chaque ligne de l'ensemble de lignes.  
  
 L’opération de suppression de **SQLSetPos** permet de supprimer une ou plusieurs lignes sélectionnées d’une table de la source de données. Pour supprimer des lignes avec **SQLSetPos**, l’application appelle **SQLSetPos** avec Operation défini sur SQL_DELETE et RowNumber défini sur le numéro de la ligne à supprimer. Si RowNumber a la valeur 0, toutes les lignes de l'ensemble de lignes sont supprimées.  
  
 Après avoir **SQLSetPos** est retournée, la ligne supprimée est la ligne actuelle et son statut est SQL_ROW_DELETED. La ligne ne peut pas être utilisée dans des opérations positionnées supplémentaires, telles que les appels à [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) ou **SQLSetPos**.  
  
 Lorsque vous supprimez toutes les lignes de l’ensemble de lignes (RowNumber est égal à 0), l’application peut empêcher le pilote à partir de la suppression de certaines lignes à l’aide du tableau d’opérations de la ligne comme pour l’opération de mise à jour de **SQLSetPos**.  
  
 Chaque ligne supprimée doit être une ligne qui existe dans le jeu de résultats. Si les mémoires tampons de l'application ont été remplies par extraction et si un tableau de statut de ligne a été maintenu, ses valeurs à chacune de ces positions de ligne ne doivent pas être SQL_ROW_DELETED, SQL_ROW_ERROR ou SQL_ROW_NOROW.  
  
 Les mises à jour positionnées peuvent également être réalisées à l'aide de la clause WHERE CURRENT OF sur les instructions UPDATE, DELETE et INSERT. WHERE CURRENT OF requiert un nom de curseur qu’ODBC génère lorsque la [SQLGetCursorName](../../relational-databases/native-client-odbc-api/sqlgetcursorname.md) fonction est appelée, ou que vous pouvez spécifier en appelant **SQLSetCursorName**. La procédure générale suivante est utilisée pour effectuer une mise à jour de WHERE CURRENT OF dans une application ODBC :  
  
-   Appelez **SQLSetCursorName** pour établir un nom de curseur pour le handle d’instruction.  
  
-   Générez une instruction SELECT avec une clause FOR UPDATE OF et exécutez-la.  
  
-   Appelez **SQLFetchScroll** pour récupérer un ensemble de lignes ou **SQLFetch** pour récupérer une ligne.  
  
-   Appelez **SQLSetPos** (SQL_POSITION) pour positionner le curseur sur la ligne.  
  
-   Générer et exécuter une instruction UPDATE avec une clause WHERE CURRENT OF en utilisant le nom de curseur défini avec **SQLSetCursorName**.  
  
 Vous pouvez également appeler **SQLGetCursorName** après avoir exécuté l’instruction SELECT au lieu d’appeler **SQLSetCursorName** avant d’exécuter l’instruction SELECT. **SQLGetCursorName** retourne un nom de curseur par défaut affecté par ODBC si vous ne définissez pas un nom de curseur à l’aide de **SQLSetCursorName**.  
  
 **SQLSetPos** est préférable à WHERE CURRENT OF lorsque vous utilisez des curseurs côté serveur. Si vous utilisez un curseur statique pouvant être mis à jour avec la bibliothèque de curseurs ODBC, la bibliothèque de curseurs implémente les mises à jour WHERE CURRENT OF en ajoutant une clause WHERE avec les valeurs de clé pour la table sous-jacente. Cela peut entraîner des mises à jour inattendues si les clés de la table ne sont pas uniques.  
  
## <a name="see-also"></a>Voir aussi  
 [À l’aide de curseurs & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  
