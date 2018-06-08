---
title: SQLSetPos (bibliothèque de curseurs) | Documents Microsoft
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
- SQLSetPos function [ODBC], Cursor Library
ms.assetid: 574399c3-2bb2-4d19-829c-7c77bd82858d
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b8a683d5b523205aa369769a612cf2b27cdb4800
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetpos-cursor-library"></a>SQLSetPos (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Cette rubrique décrit l’utilisation de la **SQLSetPos** fonction dans la bibliothèque de curseurs. Pour des informations générales sur **SQLSetPos**, consultez [SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
 La bibliothèque de curseurs prend en charge l’opération SQL_POSITION uniquement pour le *opération* argument dans **SQLSetPos**. Il prend en charge la valeur SQL_LOCK_NO_CHANGE uniquement pour les *LockType* argument.  
  
 Si le pilote ne prend pas en charge les opérations en bloc, la bibliothèque de curseurs retourne SQLSTATE HYC00 (non compatibles avec le pilote) lorsque **SQLSetPos** est appelée avec *RowNumber* égale à 0. Ce comportement de pilote n’est pas recommandé.  
  
 La bibliothèque de curseurs ne prend pas en charge les opérations SQL_UPDATE et SQL_DELETE dans un appel à **SQLSetPos**. Le curseur bibliothèque met en œuvre un positionnées mettre à jour ou supprimez l’instruction SQL en créant une recherche mettre à jour ou supprimez l’instruction avec une clause WHERE qui énumère les valeurs stockées dans son cache pour chaque colonne dépendante. Pour plus d’informations, consultez [traitement positionné instructions Update et Delete](../../../odbc/reference/appendixes/processing-positioned-update-and-delete-statements.md).  
  
 Si le pilote ne prend pas en charge les curseurs statiques, une application utilisant la bibliothèque de curseurs doit appeler **SQLSetPos** uniquement sur un ensemble de lignes extraite en **SQLExtendedFetch** ou **SQLFetchScroll**, et non par **SQLFetch**. La bibliothèque de curseurs implémente **SQLExtendedFetch** et **SQLFetchScroll** en effectuant des appels répétés de **SQLFetch** (avec une taille d’ensemble de lignes de 1) dans le pilote. La bibliothèque de curseurs passe des appels à **SQLFetch**, et sur l’autre par ailleurs, par le biais du pilote. Si **SQLSetPos** est appelée sur un ensemble de lignes de plusieurs ligne extraite en **SQLFetch** lorsque le pilote ne prend pas en charge les curseurs statiques, l’appel échoue car **SQLSetPos** ne fonctionne pas avec les curseurs avant uniquement. Cela se produit même si une application a été appelé **SQLSetStmtAttr** à la valeur SQL_ATTR_CURSOR_TYPE SQL_CURSOR_STATIC, qui prend en charge de la bibliothèque de curseurs même si le pilote ne prend pas en charge les curseurs statiques.
