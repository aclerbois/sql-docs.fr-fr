---
title: Contrôle d’accès concurrentiel | Documents Microsoft
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
- transactions [ODBC], concurrency control
- concurrency control [ODBC]
ms.assetid: 75e4adb3-3d43-49c5-8c5e-8df96310d912
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2fd05a84e70b601180ce67a94cbdc4caabf7e37e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="concurrency-control"></a>Contrôle d'accès concurrentiel
*Accès concurrentiel* est la possibilité de deux transactions utilisent les mêmes données en même temps, et avec accrue des transactions isolation devrait être une concurrence réduite. Il s’agit, car l’isolation des transactions est généralement implémentée par le verrouillage de lignes et que plusieurs lignes sont verrouillées, moins de transactions peuvent être effectuées au moins temporairement bloqué par une ligne verrouillée. Lors de la concurrence réduite est généralement considérée comme un compromis pour les transaction d’isolation élevés nécessaires pour maintenir l’intégrité de base de données, il peut devenir un problème dans un environnement interactif avec une activité élevée en lecture/écriture qui utilisent des curseurs.  
  
 Par exemple, une application exécute l’instruction SQL **sélectionnez \* de commandes**. Il appelle **SQLFetchScroll** pour faire défiler le résultat défini et autorise l’utilisateur à mettre à jour, supprimer ou insérer des commandes. Une fois que l’utilisateur met à jour, supprime ou insère une commande, l’application valide la transaction.  
  
 Si le niveau d’isolement est Repeatable Read, la transaction peut, selon la façon dont il est implémenté, verrouiller chaque ligne retournée par **SQLFetchScroll**. Si le niveau d’isolement est Serializable, la transaction peut verrouiller la table Orders entière. Dans les deux cas, la transaction libère ses verrous uniquement lorsqu’elle est validée ou restaurée. Par conséquent, si l’utilisateur passe beaucoup de temps à lire les commandes et très peu de temps à la mise à jour, suppression, ou les insérer, la transaction peut verrouiller facilement un grand nombre de lignes, ce qui les rend indisponibles à d’autres utilisateurs.  
  
 Il s’agit d’un problème même si le curseur est en lecture seule et l’application permet à l’utilisateur à lire uniquement les commandes existantes. Dans ce cas, l’application valide la transaction et libère les verrous, lorsqu’il appelle **SQLCloseCursor** (en mode de validation automatique) ou **SQLEndTran** (en mode de validation manuelle).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Types d’accès concurrentiels](../../../odbc/reference/develop-app/concurrency-types.md)  
  
-   [Accès concurrentiel optimiste](../../../odbc/reference/develop-app/optimistic-concurrency.md)
