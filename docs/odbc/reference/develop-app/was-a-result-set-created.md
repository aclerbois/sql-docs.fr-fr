---
title: Est le résultat créé ? | Microsoft Docs
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
- result sets [ODBC], determining if created
ms.assetid: 4a83b8cb-2d57-4e64-b497-80bd587ee1f9
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0736ddcd39ce75b83dbbb4d6e0a8fc500073dd19
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="was-a-result-set-created"></a>Est le résultat créé ?
Dans la plupart des cas, les programmeurs d’applications savent si les instructions de que l’exécution de leur application crée un jeu de résultats. C’est le cas si l’application utilise des instructions SQL codées en dur écrites par le programmeur. Il est généralement le cas lorsque l’application construit des instructions SQL en cours d’exécution : le programmeur peut inclure facilement du code qui signale si une **sélectionnez** instruction ou une **insérer** instruction est en cours de construction. Dans certaines situations, le programmeur ne peut pas savoir si une instruction crée un jeu de résultats. Cela est vrai si l’application fournit un moyen de l’utilisateur à entrer et à exécuter une instruction SQL. Il est également vrai lorsque l’application crée une instruction en cours d’exécution pour exécuter une procédure.  
  
 Dans ce cas, l’application appelle **SQLNumResultCols** pour déterminer le nombre de colonnes dans le jeu de résultats. S’il s’agit de 0, l’instruction n’a pas créé à un jeu de résultats ; s’il est tout autre nombre, l’instruction n’a créé un jeu de résultats.  
  
 L’application peut appeler **SQLNumResultCols** à tout moment après l’instruction est préparée ou exécutée. Toutefois, étant donné que certaines sources de données ne peut pas décrire facilement les jeux de résultats qui seront créés par les instructions préparées, performances en souffriront si **SQLNumResultCols** est appelée après une instruction est préparée mais avant son exécution.  
  
 Certaines sources de données prennent également en charge la détermination du nombre de lignes qui une instruction SQL retourne un jeu de résultats. Pour ce faire, l’application appelle **SQLRowCount**. Exactement ce que représente le nombre de lignes est indiqué par le paramètre de l’option SQL_DYNAMIC_CURSOR_ATTRIBUTES2, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2, SQL_KEYSET_CURSOR_ATTRIBUTES2 ou SQL_STATIC_CURSOR_ATTRIBUTES2 (selon le type du curseur) retourné par un appel à **SQLGetInfo**. Ce masque de bits indique pour chaque type de curseur si le nombre de lignes retourné est exacte et approximative ou n’est pas du tout disponible. Si le nombre de lignes pour statique ou les curseurs pilotés par jeu de clés sont affectées par les modifications apportées dans **SQLBulkOperations** ou **SQLSetPos**, ou par mise à jour positionnée ou les instructions delete, dépend des autres bits retournés par les mêmes arguments option répertoriées précédemment. Pour plus d’informations, consultez la [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) description de fonction.
