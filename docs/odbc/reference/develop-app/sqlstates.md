---
title: SQLSTATE | Documents Microsoft
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
- diagnostic information [ODBC], sqlstates
- SQLSTATE [ODBC]
ms.assetid: f29fff2e-3d09-4a8c-a2f9-2059062cbebf
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9408522a47b442e3001e09cdca2d636063f73fb5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlstates"></a>SQLSTATE
SQLSTATE fournit des informations détaillées sur la cause d’un avertissement ou d’erreur. Le SQLSTATE dans ce guide est basées sur celles disponibles dans la spécification ISO/IEF CLI, bien que ces SQLSTATE qui commencent par messagerie instantanée est spécifiques à ODBC.  
  
 Contrairement aux codes de retour, le SQLSTATE dans ce manuel est des recommandations et pilotes n’êtes pas obligés de les retourner. Par conséquent, tandis que les pilotes doivent retourner le SQLSTATE approprié pour toute erreur ou avertissement, qu'ils sont capables de détecter, les applications ne doivent pas compter sur ce produit toujours. Les raisons de cette situation sont double :  
  
-   **Non-exhaustivité** bien que ce manuel répertorie un grand nombre d’erreurs et avertissements et les causes possibles de ces erreurs et les avertissements, il n’est pas terminée et ne seront probablement jamais ; les implémentations de pilote varient simplement trop. Tous les pilotes ne renvoie tous les SQLSTATE répertoriées dans ce manuel et peuvent retourner SQLSTATE non répertoriés dans ce manuel.  
  
-   **Complexité** certains moteurs de base de données, les moteurs de base de données relationnelle en particulier, renvoyer des milliers d’erreurs et avertissements. Les pilotes de ces moteurs sont peu susceptibles de mapper tous ces erreurs et avertissements à SQLSTATE en raison de l’effort impliqué, l’inexactness des mappages, la taille du code qui en résulte et la valeur basse du code qui en résulte, qui renvoie souvent des erreurs de programmation qui ne doivent jamais se rencontrer au moment de l’exécution. Par conséquent, les pilotes doivent mapper autant d’erreurs et avertissements semble raisonnable et veillez à mapper ces erreurs et des avertissements dans la logique d’application qui peuvent être basées, par exemple SQLSTATE 01004 (données tronquées).  
  
 SQLSTATE n’étant pas fiable retournées, la plupart des applications affichent simplement les à l’utilisateur, ainsi que leur message de diagnostic associé, qui est souvent adaptée à l’erreur ou d’avertissement qui s’est produite et le code d’erreur natif. Il est rarement aucune perte de fonctionnalité à effectuer, étant donné que les applications ne peuvent pas de base logique de programmation sur la plupart des SQLSTATE quand même. Par exemple, supposons que **SQLExecDirect** retourne SQLSTATE 42000 (syntaxe ou violation d’accès). Si l’instruction SQL qui a provoqué cette erreur est codé en dur ou générée par l’application, il s’agit d’une erreur de programmation et le code doit être corrigé. Si l’instruction SQL est entrée par l’utilisateur, il s’agit d’une erreur de l’utilisateur et l’application a terminé tout ce qui est possible en informant l’utilisateur du problème.  
  
 Lorsque les applications sans se baser logique de programmation sur SQLSTATE, ils doivent être préparées pour la valeur SQLSTATE ne pas à retourner ou pour une autre SQLSTATE à retourner. Qui SQLSTATE est renvoyés de façon fiable peuvent être basé uniquement sur l’expérience avec de nombreux pilotes. Toutefois, la règle générale est que SQLSTATE pour les erreurs qui se produisent dans le pilote ou le Gestionnaire de pilotes, par opposition à la source de données est plus susceptibles d’être renvoyé de manière fiable. Par exemple, la plupart des pilotes retourner probablement SQLSTATE HYC00 (fonctionnalité facultative non implémentée), alors que les pilotes moins retournent probablement SQLSTATE 42021 (colonne existe déjà).  
  
 Le SQLSTATE suivants indique les erreurs d’exécution ou des avertissements et est de bons candidats sur lequel baser la logique de programmation. Toutefois, il n’existe aucune garantie que tous les pilotes de les retournent.  
  
-   01004 (données tronquées)  
  
-   01 s 02 (Option la valeur modifiée)  
  
-   HY008 (opération annulée)  
  
-   HYC00 (fonctionnalité facultative non implémentée)  
  
-   HYT00 (délai d’attente expiré)  
  
 SQLSTATE HYC00 (fonctionnalité facultative non implémentée) est particulièrement importante, car il est le seul moyen de dans laquelle une application peut déterminer si un pilote prend en charge un attribut d’instruction ou une connexion particulière.  
  
 Pour obtenir une liste complète de SQLSTATE et les fonctions qui retournent les, consultez [annexe a : les Codes d’erreur ODBC](../../../odbc/reference/appendixes/appendix-a-odbc-error-codes.md). Pour obtenir une explication détaillée des conditions dans lesquelles chaque fonction peut retourner un SQLSTATE particulier, consultez cette fonction.
