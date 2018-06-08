---
title: Nombre de lignes extraites et état | Documents Microsoft
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
- row status array [ODBC]
- number of rows fetched [ODBC]
- result sets [ODBC], row status array
ms.assetid: a069b979-5108-4905-932f-8ae8e7905ff2
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 16bc3a279e2d4565529dd175c16bce01f9431403
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="number-of-rows-fetched-and-status"></a>Nombre de lignes extraites et l’état
Si l’attribut SQL_ATTR_ROWS_FETCHED_PTR d’instruction a été définie, elle spécifie une mémoire tampon qui retourne le nombre de lignes lues par l’appel à **SQLFetch** ou **SQLFetchScroll**et les lignes d’erreur. (Ce nombre est un nombre de toutes les lignes qui n’ont pas l’état SQL_ROW_NO_ROWS.) Après un appel à **SQLBulkOperations** ou **SQLSetPos**, la mémoire tampon contient le nombre de lignes affectées par une opération en bloc effectuée par la fonction. Si l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR a été défini, **SQLFetch** ou **SQLFetchScroll** retourne le *tableau d’état de ligne,* qui fournit l’état de chaque ligne retournée. À la fois de la mémoire tampon pointée par ces champs sont allouées par l’application et remplies par le pilote. Une application doit s’assurer que ces pointeurs restent valides jusqu'à ce que le curseur est fermé.  
  
 Entrées dans l’état de tableau de statut de la ligne si chaque ligne a été extraite avec succès, si elle a été mise à jour, ajouté ou supprimé depuis sa dernière extraction, et si une erreur s’est produite lors de l’extraction de la ligne. Si **SQLFetch** ou **SQLFetchScroll** rencontre une erreur lors de la récupération d’une ligne d’un ensemble de lignes de plusieurs ligne, ou si **SQLBulkOperations** avec un *opération* argument de SQL_FETCH_BY_BOOKMARK rencontre une erreur lors de l’exécution d’une extraction en bloc, il définit la valeur correspondante dans le tableau d’état de ligne à SQL_ROW_ERROR continue d’extraction de lignes et retourne SQL_SUCCESS_WITH_INFO. Pour plus d’informations sur la gestion des erreurs et le tableau d’état de ligne, consultez la [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md) et [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md) des descriptions de fonction.
