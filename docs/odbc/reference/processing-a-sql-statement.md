---
title: Traitement d’une instruction SQL | Documents Microsoft
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
- sending SQL statements to DBMS [ODBC]
- SQL statements [ODBC], processing
- SQL [ODBC], processing statements
- statement processing [ODBC]
- SQL statements [ODBC]
- ODBC [ODBC], SQL
ms.assetid: 96270c4f-2efd-4dc1-a985-ed7fd5658db2
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7068b61081a368382b109af99f60bf6bf254e800
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="processing-a-sql-statement"></a>Traitement d’une instruction SQL
Avant d’aborder les techniques d’utilisation de SQL par programme, il est nécessaire décrire le mode de traitement d’une instruction SQL. Les étapes sont communes à toutes les techniques de trois, bien que chaque technique leur exécution à des moments différents. L’illustration suivante montre les étapes impliquées dans le traitement d’une instruction SQL, qui sont décrites dans le reste de cette section.  
  
 ![Étapes de traitement d’une instruction SQL](../../odbc/reference/media/pr01.gif "pr01")  
  
 Pour traiter une instruction SQL, un SGBD exécute les cinq étapes suivantes :  
  
1.  D’abord, le SGBD analyse l’instruction SQL. Il scinde l’instruction en mots individuels, appelés jetons, permet de s’assurer que l’instruction a un verbe valid et clauses valides et ainsi de suite. Les fautes d’orthographe et les erreurs de syntaxe peuvent être détectées lors de cette étape.  
  
2.  Le SGBD valide de l’instruction. Il vérifie l’instruction sur le catalogue système. Toutes les tables nommées dans l’instruction existent dans la base de données ? Toutes les colonnes existent et sont les noms de colonne et non équivoque ? L’utilisateur dispose des privilèges nécessaires pour exécuter l’instruction ? Certaines erreurs sémantiques peuvent être détectées lors de cette étape.  
  
3.  Le SGBD génère un plan d’accès pour l’instruction. Le plan d’accès est une représentation binaire des étapes nécessaires pour mener à bien l’instruction ; Il est l’équivalent de SGBD du code exécutable.  
  
4.  Le SGBD optimise le plan d’accès. Il examine les différentes méthodes pour mener à bien le plan d’accès. Un index peut être utilisé pour accélérer une recherche ? Doit le SGBD une condition de recherche d’abord appliquer à la Table A et puis de le joindre à la Table B, ou il doit commencer par la jointure et utiliser la condition de recherche par la suite ? Une recherche séquentielle via une table peut être évitée ou réduite à un sous-ensemble de la table ? Après avoir exploré les alternatives, le SGBD choisit un d’eux.  
  
5.  Le SGBD exécute l’instruction par le plan d’accès en cours d’exécution.  
  
 Les étapes utilisées pour traiter une instruction SQL varient dans l’accès de base de données que dont ils ont besoin et le temps qu’ils prennent. L’analyse d’une instruction SQL ne nécessite pas d’accès à la base de données et peut être très rapide. L’optimisation, est en revanche, un processeur beaucoup de traiter et requiert l’accès pour le catalogue système. Pour une requête complexe, contenant plusieurs tables, l’optimiseur peut Explorer des milliers de différentes façons de procéder à la même requête. Toutefois, le coût de l’exécution de la requête mal est généralement très élevé regagnée plus de la durée de l’optimisation de la vitesse d’exécution de requête. C’est encore plus important si le même plan d’optimiser l’accès peut être utilisé plusieurs fois pour effectuer des requêtes répétitives.
