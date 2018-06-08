---
title: Gestionnaire de pilotes&#39;rôle s dans le processus de connexion | Documents Microsoft
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
- driver manager [ODBC], role in connection process
- connecting to data source [ODBC], driver manager
- connecting to driver [ODBC], driver manager
- ODBC driver manager [ODBC]
ms.assetid: 77c05630-5a8b-467d-b80e-c705dc06d601
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0ba98332eb196811b71f0cc755f92c84cc9d4091
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="driver-manager39s-role-in-the-connection-process"></a>Gestionnaire de pilotes&#39;rôle s dans le processus de connexion
N’oubliez pas que les applications n’appellent pas directement les fonctions de pilote. Au lieu de cela, ils appellent des fonctions de gestionnaire de pilotes portant le même nom et le Gestionnaire de pilotes appelle les fonctions de pilote. Généralement, ceci est presque immédiatement. Par exemple, l’application appelle **SQLExecute** dans le Gestionnaire de pilotes et après quelques vérifications d’erreurs, le Gestionnaire de pilotes appelle **SQLExecute** dans le pilote.  
  
 Le processus de connexion est différent. Lorsque l’application appelle **SQLAllocHandle** avec les options de SQL_HANDLE_ENV et SQL_HANDLE_DBC, la fonction alloue des handles uniquement dans le Gestionnaire de pilotes. Le Gestionnaire de pilotes n’appelle pas cette fonction dans le pilote, car il ne connaît pas le pilote à appeler. De même, si l’application passe le handle d’une connexion non connecté à **SQLSetConnectAttr** ou **SQLGetConnectAttr**, seul le Gestionnaire de pilotes exécute la fonction. Il stocke ou obtient la valeur d’attribut à partir de sa connexion gèrent et retourne SQLSTATE 08003 (connexion non ouverte) lors de l’obtention d’une valeur pour un attribut qui n’a pas été définie et pour quels ODBC ne définit pas de valeur par défaut.  
  
 Lorsque l’application appelle **SQLConnect**, **SQLDriverConnect**, ou **SQLBrowseConnect**, le Gestionnaire de pilotes détermine tout d’abord le pilote à utiliser. Il vérifie ensuite pour déterminer si un pilote est actuellement chargé sur la connexion :  
  
-   Si aucun pilote n’est chargé sur la connexion, le Gestionnaire de pilotes vérifie si le pilote spécifié est chargé sur une autre connexion dans le même environnement. Si le Gestionnaire de pilotes charge pas, le pilote sur la connexion et les appels **SQLAllocHandle** dans le pilote avec l’option de SQL_HANDLE_ENV.  
  
     Le Gestionnaire de pilotes appelle ensuite **SQLAllocHandle** dans le pilote avec l’option de SQL_HANDLE_DBC, si elle a simplement chargé. Si l’application de définie des attributs de connexion, le Gestionnaire de pilotes appelle **SQLSetConnectAttr** dans le pilote ; si une erreur se produit, fonction de connexion du Gestionnaire de pilotes retourne SQLSTATE IM006 (le pilote **SQLSetConnectAttr** échec). Enfin, le Gestionnaire de pilotes appelle la fonction de connexion dans le pilote.  
  
-   Si le pilote spécifié est chargé sur la connexion, le Gestionnaire de pilotes appelle uniquement la fonction de connexion dans le pilote. Dans ce cas, le pilote doit garantir que tous les attributs de connexion sur la connexion leurs paramètres actuels.  
  
-   Si un autre pilote est chargé sur la connexion, le Gestionnaire de pilotes appelle **SQLFreeHandle** dans le pilote pour libérer la connexion. S’il n’y a pas d’autres connexions qui utilisent le pilote, le Gestionnaire de pilotes appelle **SQLFreeHandle** dans le pilote pour libérer de l’environnement et décharge le pilote. Le Gestionnaire de pilotes puis effectue les mêmes opérations que lorsqu’un pilote n’est pas chargé sur la connexion.  
  
 Le Gestionnaire de pilotes se verrouille le handle d’environnement (*henv*) avant l’appel d’un pilote **SQLAllocHandle** et **SQLFreeHandle** lorsque *HandleType* a la valeur **SQL_HANDLE_DBC**.  
  
 Lorsque l’application appelle **SQLDisconnect**, les appels du Gestionnaire de pilotes **SQLDisconnect** dans le pilote. Toutefois, elle laisse le pilote chargé dans le cas où l’application se reconnecte au pilote. Lorsque l’application appelle **SQLFreeHandle** avec l’option SQL_HANDLE_DBC, appelle le Gestionnaire de pilotes **SQLFreeHandle** dans le pilote. Si le pilote n’est pas utilisé par les autres connexions, le Gestionnaire de pilotes appelle ensuite **SQLFreeHandle** dans le pilote avec la SQL_HANDLE_ENV option et décharge le pilote.
