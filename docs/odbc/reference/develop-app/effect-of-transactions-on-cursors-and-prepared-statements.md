---
title: Effet des Transactions sur les curseurs et les instructions préparées | Documents Microsoft
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
- rolling back transactions [ODBC]
- committing transactions [ODBC]
- transactions [ODBC], rolling back
- cursors [ODBC], transaction commits or roll backs
- prepared statements [ODBC]
- transactions [ODBC], cursors
ms.assetid: 523e22a2-7b53-4c25-97c1-ef0284aec76e
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ceb7aaae6967376c454d32633cda6043bc281825
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="effect-of-transactions-on-cursors-and-prepared-statements"></a>Effet des Transactions sur les curseurs et les instructions préparées
Validation ou la restauration d’une transaction a l’effet suivant sur les plans d’accès et des curseurs :  
  
-   Tous les curseurs sont fermés, et les plans d’accès pour les instructions préparées sur cette connexion sont supprimés.  
  
-   Tous les curseurs sont fermés, et les plans d’accès pour les instructions préparées sur cette connexion restent intacts.  
  
-   Tous les curseurs restent ouverts et les plans d’accès pour les instructions préparées sur cette connexion restent intacts.  
  
 Par exemple, qu'une source de données présente le comportement premier dans cette liste, la plus restrictive de ces comportements. Maintenant Supposons qu’une application effectue les opérations suivantes :  
  
1.  Définit le mode de validation pour la validation manuelle.  
  
2.  Crée un jeu de résultats des commandes client sur l’instruction 1.  
  
3.  Crée un jeu de résultats des lignes dans une commande client dans l’instruction 2, lorsque l’utilisateur met en surbrillance cet ordre.  
  
4.  Appels **SQLExecute** pour exécuter une instruction de mise à jour positionnée qui a été préparée sur instruction 3, lorsque l’utilisateur met à jour une ligne.  
  
5.  Appels **SQLEndTran** pour valider l’instruction de mise à jour positionnée.  
  
 En raison du comportement de la source de données, l’appel à **SQLEndTran** à l’étape 5 provoque pour fermer les curseurs sur les instructions 1 et 2 et pour supprimer le plan d’accès sur toutes les instructions. L’application doit exécuter des jeux d’instructions 1 et 2 pour recréer le résultat et reprepare l’instruction sur l’instruction 3.  
  
 En mode de validation automatique, fonctions autres que **SQLEndTran** valider des transactions :  
  
-   **SQLExecute** ou **SQLExecDirect** dans l’exemple précédent, l’appel à **SQLExecute** à l’étape 4 valide une transaction. Cela provoque la source de données fermer les curseurs sur les instructions 1 et 2 et de supprimer le plan d’accès sur toutes les instructions de cette connexion.  
  
-   **SQLBulkOperations** ou **SQLSetPos** dans l’exemple précédent, supposons que dans l’étape 4 l’application appelle **SQLSetPos** avec l’option SQL_UPDATE sur instruction 2, au lieu d’exécuter un instruction de mise à jour positionnée sur instruction 3. Valide une transaction entraîne la source de données fermer les curseurs sur les instructions 1 et 2 et ignore tous les plans d’accès sur cette connexion.  
  
-   **SQLCloseCursor** dans l’exemple précédent, supposons que lorsque l’utilisateur met en surbrillance une autre commande client, l’application appelle **SQLCloseCursor** sur instruction 2 avant de créer un résultat des lignes pour les nouvelles ventes commande. L’appel à **SQLCloseCursor** valide le **sélectionnez** instruction qui a créé le jeu de résultats de lignes et provoque la source de données fermer le curseur lors de l’instruction 1 et puis ignore tous les plans d’accès sur cette connexion.  
  
 Les applications, en particulier sur un écran dans lequel l’utilisateur parcourt le jeu de résultats et les mises à jour ou supprime des lignes, doivent être prudent coder autour de ce comportement.  
  
 Pour déterminer le comportement d’une source de données lorsqu’une transaction est validée ou restaurée, une application appelle **SQLGetInfo** avec les options SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR.
