---
title: Mise à jour des données avec SQLBulkOperations | Documents Microsoft
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
- SQLBulkOperations function [ODBC], updating data
- data updates [ODBC], SQLBulkOperations
- updating data [ODBC], SQLBulkOperations
ms.assetid: 7645a704-341e-4267-adbe-061a9fda225b
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6b1acf3d788381f32d892432c0834cc5ee130680
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-data-with-sqlbulkoperations"></a>Mise à jour des données avec SQLBulkOperations
Les applications peuvent effectuer des opérations de mise à jour, supprimer, fetch ou d’insertion en bloc sur la table sous-jacente à la source de données avec un appel à **SQLBulkOperations**. Appel de **SQLBulkOperations** est une alternative pratique à la construction et l’exécution d’une instruction SQL. Il permet à un pilote ODBC prend en charge les mises à jour positionnées même lorsque la source de données ne prend pas en charge les instructions SQL positionnées. Il fait partie du paradigme de réaliser un accès de base de données au moyen d’appels de fonction.  
  
 **SQLBulkOperations** fonctionne sur l’ensemble de lignes en cours et peut être utilisé uniquement après un appel à **SQLFetch** ou **SQLFetchScroll**. L’application spécifie les lignes à supprimer, mettre à jour ou actualiser en mettant en cache leurs signets. Le pilote récupère les nouvelles données pour les lignes à mettre à jour ou les nouvelles données à insérer dans la table sous-jacente, à partir de mémoires tampons de l’ensemble de lignes.  
  
 La taille de l’ensemble de lignes à utiliser par **SQLBulkOperations** est définie par un appel à **SQLSetStmtAttr** avec un *attribut* argument de SQL_ATTR_ROW_ARRAY_SIZE. Contrairement aux **SQLSetPos**, qui utilise une nouvelle taille de l’ensemble de lignes uniquement après un appel à **SQLFetch** ou **SQLFetchScroll**, **SQLBulkOperations** utilise la nouvelle taille de l’ensemble de lignes après l’appel à **SQLSetStmtAttr**.  
  
 Étant donné que la plupart des interactions avec les bases de données relationnelles sont effectuée à partir de SQL, **SQLBulkOperations** n’est pas largement pris en charge. Toutefois, un pilote peut il émuler facilement en créant et en exécutant une **mise à jour**, **supprimer**, ou **insérer** instruction.  
  
 Pour déterminer quelles opérations **SQLBulkOperation** prend en charge, une application appelle **SQLGetInfo** avec l’option informations SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 ou SQL_STATIC_CURSOR_ATTRIBUTES1 (selon le type du curseur).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Mise à jour de lignes par signet avec SQLBulkOperations](../../../odbc/reference/develop-app/updating-rows-by-bookmark-with-sqlbulkoperations.md)  
  
-   [Suppression de lignes par signet avec SQLBulkOperations](../../../odbc/reference/develop-app/deleting-rows-by-bookmark-with-sqlbulkoperations.md)  
  
-   [Insertion de lignes avec SQLBulkOperations](../../../odbc/reference/develop-app/inserting-rows-with-sqlbulkoperations.md)  
  
-   [Extraction de lignes avec SQLBulkOperations](../../../odbc/reference/develop-app/fetching-rows-with-sqlbulkoperations.md)
