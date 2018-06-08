---
title: Mise à jour des données avec SQLSetPos | Documents Microsoft
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
- updating data [ODBC], SQLSetPos
- data updates [ODBC], SQLSetPos
- SQLSetPos function [ODBC], updating data
ms.assetid: e9625b59-06a0-4883-b155-b932ba7528d9
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3fecebb38d9e34a2a06fb0e5b70131cc57d8deff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-data-with-sqlsetpos"></a>Mise à jour des données avec SQLSetPos
Les applications peuvent mettre à jour ou supprimer les lignes de l’ensemble de lignes avec **SQLSetPos**. Appel de **SQLSetPos** est une alternative pratique à la construction et l’exécution d’une instruction SQL. Il permet à un pilote ODBC prend en charge les mises à jour positionnées même lorsque la source de données ne prend pas en charge les instructions SQL positionnées. Il fait partie du paradigme de réaliser un accès de base de données au moyen d’appels de fonction.  
  
 **SQLSetPos** fonctionne sur l’ensemble de lignes en cours et peut être utilisé uniquement après un appel à **SQLFetchScroll**. L’application spécifie le numéro de la ligne à mettre à jour, delete ou insert, et le pilote récupère les nouvelles données pour cette ligne à partir de mémoires tampons de l’ensemble de lignes. **SQLSetPos** peut également être utilisé pour désigner une ligne spécifiée en tant que la ligne actuelle, ou pour actualiser une ligne particulière dans l’ensemble de lignes à partir de la source de données.  
  
 Taille de l’ensemble de lignes est définie par un appel à **SQLSetStmtAttr** avec un *attribut* argument de SQL_ATTR_ROW_ARRAY_SIZE. **SQLSetPos** utilise une nouvelle taille de l’ensemble de lignes, toutefois, qu’après un appel à **SQLFetch** ou **SQLFetchScroll**. Par exemple, si la taille de l’ensemble de lignes est modifiée, **SQLSetPos** est appelée, puis **SQLFetch** ou **SQLFetchScroll** est appelée et l’appel à **SQLSetPos** utilise l’ancienne taille d’ensemble de lignes lors de la **SQLFetch** ou **SQLFetchScroll** utilise la nouvelle taille de l’ensemble de lignes.  
  
 La première ligne de l'ensemble de lignes porte le numéro 1. Le *RowNumber* argument dans **SQLSetPos** doit identifier une ligne dans l’ensemble de lignes ; autrement dit, sa valeur doit être comprise entre 1 et le nombre de lignes qui ont été extraites le plus récemment (qui peut être inférieure à la taille de l’ensemble de lignes). Si *RowNumber* est 0, l’opération s’applique à chaque ligne de l’ensemble de lignes.  
  
 Étant donné que la plupart des interactions avec les bases de données relationnelles sont effectuée à partir de SQL, **SQLSetPos** n’est pas largement pris en charge. Toutefois, un pilote peut il émuler facilement en créant et en exécutant une **mise à jour** ou **supprimer** instruction.  
  
 Pour déterminer quelles opérations **SQLSetPos** prend en charge, une application appelle **SQLGetInfo** avec l’option informations SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 ou SQL_STATIC_CURSOR_ATTRIBUTES1 (selon le type du curseur).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Mise à jour de lignes dans l’ensemble de lignes avec SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md)  
  
-   [Suppression de lignes dans l’ensemble de lignes avec SQLSetPos](../../../odbc/reference/develop-app/deleting-rows-in-the-rowset-with-sqlsetpos.md)
