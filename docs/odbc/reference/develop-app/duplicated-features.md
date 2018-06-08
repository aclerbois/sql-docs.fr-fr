---
title: Dupliqué fonctionnalités | Documents Microsoft
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
- duplicated functions [ODBC]
- compatibility [ODBC], duplicated functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], duplicated functions
- backward compatibility [ODBC], duplicated functions
ms.assetid: 641b16bc-f791-46d8-b093-31736473fe3d
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 72daebe68aa1edbd98ae904f9d5a08d39c3778d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="duplicated-features"></a>Fonctionnalités en double
ODBC 2 suivante. *x* fonctions ont été dupliquées par ODBC 3. *x* fonctions. Par conséquent, l’API ODBC 2. *x* fonctions sont déconseillées dans ODBC 3. *x*. ODBC 3. *x* fonctions sont appelées fonctions de remplacement.  
  
 Lorsqu’une application utilise un déconseillées ODBC 2. *x* fonction et le pilote sous-jacent est un ODBC 3. *x* pilote, le Gestionnaire de pilotes est mappé à l’appel de fonction à la fonction de remplacement correspondante. La seule exception à cette règle est **SQLExtendedFetch**. (Voir la note à la fin du tableau suivant). Pour plus d’informations sur ces mappages, consultez [mappage des fonctions déconseillées](../../../odbc/reference/appendixes/mapping-deprecated-functions.md) dans l’annexe g : pilote recommandations pour la compatibilité descendante.  
  
 Lorsqu’une application utilise une fonction de remplacement et le pilote sous-jacent est une API ODBC 2. *x* pilote, le Gestionnaire de pilotes est mappé à l’appel de fonction à la fonction déconseillée correspondante.  
  
|ODBC 2. *x* (fonction)|ODBC 3. *x* (fonction)|  
|-------------------------|-------------------------|  
|**SQLAllocConnect**|**SQLAllocHandle**|  
|**SQLAllocEnv**|**SQLAllocHandle**|  
|**SQLAllocStmt**|**SQLAllocHandle**|  
|**SQLColAttributes**|**SQLColAttribute**|  
|**SQLError**|**SQLGetDiagRec**|  
|**SQLExtendedFetch**[1]|**SQLFetchScroll**|  
|**SQLFreeConnect**|**SQLFreeHandle**|  
|**SQLFreeEnv**|**SQLFreeHandle**|  
|**SQLGetConnectOption**|**SQLGetConnectAttr**|  
|**SQLGetStmtOption**|**SQLGetStmtAttr**|  
|**SQLParamOptions**|**SQLSetStmtAttr**, **SQLGetStmtAttr**|  
|**SQLSetConnectOption**|**SQLSetConnectAttr**|  
|**SQLSetParam**|**SQLBindParameter**|  
|**SQLSetStmtOption**|**SQLSetStmtAttr**|  
|**SQLTransact**|**SQLEndTran**|  
  
 [1] la fonction **SQLExtendedFetch** est une fonctionnalité en double ; **SQLFetchScroll** fournit les mêmes fonctionnalités dans ODBC 3. *x*. Toutefois, le Gestionnaire de pilotes ne mappe pas **SQLExtendedFetch** à **SQLFetchScroll** lors du passage par rapport à un ODBC 3. *x* pilote. Pour plus d’informations, consultez [ce que le Gestionnaire de pilotes fait](../../../odbc/reference/appendixes/what-the-driver-manager-does.md) dans l’annexe g : pilote recommandations pour la compatibilité descendante. Le Gestionnaire de pilotes mappe **SQLFetchScroll** à **SQLExtendedFetch** lors du passage par rapport à une API ODBC 2. *x* pilote.  
  
> [!NOTE]  
>  La fonction **SQLBindParam** est un cas spécial. **SQLBindParam** est une fonctionnalité en double. Ce n’est pas un ODBC 2 *.x* fonction, mais une fonction qui est présente dans les normes Open Group et ISO. La fonctionnalité fournie par cette fonction est entièrement utilisée par des **SQLBindParameter**. Par conséquent, le Gestionnaire de pilotes est mappé à un appel à **SQLBindParam** à **SQLBindParameter** lorsque le pilote sous-jacent est un ODBC 3. *x* pilote. Toutefois, lorsque le pilote sous-jacent est une API ODBC 2 *.x* pilote, le Gestionnaire de pilotes n’effectue pas de ce mappage.
