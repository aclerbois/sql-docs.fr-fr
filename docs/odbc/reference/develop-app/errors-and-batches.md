---
title: Erreurs et des traitements | Documents Microsoft
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
- batches [ODBC], errors
- sql_success_with_info [ODBC]
- sql_success [ODBC]
- SQL statements [ODBC], batches
- sql_error [ODBC]
ms.assetid: 6debd41d-9f4c-4f4c-a44b-2993da5306f0
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d00672166039421fb8518c52a58d19d73d02d477
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errors-and-batches"></a>Erreurs et des traitements
Lorsqu’une erreur se produit lors de l’exécution d’un lot d’instructions SQL, un des quatre résultats suivants sont possibles. (Chaque résultat possible est spécifique à la source de données et peut même dépend les instructions incluses dans le lot.)  
  
-   Aucune instruction du lot n’est exécutées.  
  
-   Aucune instruction du lot n’est exécutées et la transaction est restaurée.  
  
-   Toutes les instructions avant l’instruction d’erreur sont exécutées.  
  
-   Toutes les instructions à l’exception de l’instruction d’erreur sont exécutées.  
  
 Dans les deux premiers cas, **SQLExecute** et **SQLExecDirect** retourne SQL_ERROR. Dans les deux derniers cas, elles peuvent retourner SQL_SUCCESS_WITH_INFO ou SQL_SUCCESS, selon l’implémentation. Dans tous les cas, obtenir des informations d’erreur pouvant être récupérées avec **SQLGetDiagField**, **SQLGetDiagRec**, ou **SQLError**. Toutefois, la nature et la profondeur de ces informations est spécifique à la source de données. En outre, cette information est peu probable qu’exactement l’instruction dans l’erreur.
