---
title: SQLSetStmtAttr (bibliothèque de curseurs) | Documents Microsoft
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
- SQLSetStmtAttr function [ODBC], Cursor Library
ms.assetid: 6018a733-c2c8-4047-92ec-92cf85031767
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bb02eae512230d039cd87eafabfeffaf0af13b6e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetstmtattr-cursor-library"></a>SQLSetStmtAttr (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Cette rubrique décrit l’utilisation de la **SQLSetStmtAttr** fonction dans la bibliothèque de curseurs. Pour des informations générales sur **SQLSetStmtAttr**, consultez [fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
 La bibliothèque de curseurs prend en charge les attributs de l’instruction suivante avec **SQLSetStmtAttr**:  
  
|||  
|-|-|  
|SQL_ATTR_CONCURRENCY|SQL_ATTR_ROW_BIND_OFFSET_PTR|  
|SQL_ATTR_CURSOR_TYPE|SQL_ATTR_ROW_BIND_TYPE|  
|SQL_ATTR_FETCH_BOOKMARK_PTR|SQL_ATTR_ROWSET_ARRAY_SIZE|  
|SQL_ATTR_PARAM_BIND_OFFSET_PTR|SQL_ATTR_SIMULATE_CURSOR|  
|SQL_ATTR_PARAM_BIND_TYPE|SQL_ATTR_USE_BOOKMARKS|  
  
 La bibliothèque de curseurs prend en charge uniquement les valeurs SQL_CURSOR_FORWARD_ONLY et SQL_CURSOR_STATIC de l’attribut d’instruction SQL_ATTR_CURSOR_TYPE.  
  
 Pour les curseurs avant uniquement, la bibliothèque de curseurs prend en charge la valeur SQL_CONCUR_READ_ONLY de l’attribut d’instruction SQL_ATTR_CONCURRENCY. Pour les curseurs statiques, la bibliothèque de curseurs prend en charge les valeurs SQL_CONCUR_READ_ONLY et SQL_CONCUR_VALUES de l’attribut d’instruction SQL_ATTR_CONCURRENCY.  
  
 La bibliothèque de curseurs prend en charge uniquement la valeur SQL_SC_NON_UNIQUE de l’attribut d’instruction SQL_ATTR_SIMULATE_CURSOR.  
  
 Bien que la spécification ODBC prend en charge les appels à **SQLSetStmtAttr** avec les attributs SQL_ATTR_PARAM_BIND_TYPE ou SQL_ATTR_ROW_BIND_TYPE après **SQLFetch** ou **SQLFetchScroll** a été appelée, le curseur n’est pas le cas de bibliothèque. Avant d’avoir modifié le type de liaison dans la bibliothèque de curseurs, l’application doit fermer le curseur. La bibliothèque de curseurs prend en charge la modification des SQL_ATTR_ROW_BIND_OFFSET_PTR, SQL_ATTR_PARAM_BIND_OFFSET_PTR, SQL_ATTR_ROWS_FETCHED_PTR, et les attributs d’instruction SQL_ATTR_PARAMS_PROCESSED_PTR lorsqu’un curseur est ouvert.  
  
 Une application peut appeler **SQLSetStmtAttr** avec un **attribut** de SQL_ATTR_ROW_ARRAY_SIZE pour modifier la taille de l’ensemble de lignes tandis que le curseur est ouvert. La nouvelle taille de l’ensemble de lignes prendra effet lors du prochain **SQLFetchScroll** ou **SQLFetch** est appelée.  
  
 La bibliothèque de curseurs prend en charge la définition de l’attribut d’instruction SQL_ATTR_PARAM_BIND_OFFSET_PTR ou SQL_ATTR_ROW_BIND_OFFSET_PTR pour activer les décalages de liaison. Le décalage de la liaison n’est pas servir pour les appels à **SQLFetch** lorsque la bibliothèque de curseurs est utilisée avec une API ODBC 2. *x* pilote.  
  
 La bibliothèque de curseurs prend en charge la définition de l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS sur SQL_UB_VARIABLE.
