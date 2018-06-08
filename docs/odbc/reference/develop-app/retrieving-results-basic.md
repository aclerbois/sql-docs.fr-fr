---
title: La récupération des résultats (Basic) | Documents Microsoft
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
- result sets [ODBC], about result sets
- data sources [ODBC], result sets
- empty result sets [ODBC]
ms.assetid: 052870e3-3f3f-4f07-91da-b649348225f4
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b09820c0716d6d7a8261ede3b5a4173dc73f384d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieving-results-basic"></a>La récupération des résultats (Basic)
A *jeu de résultats* est un ensemble de lignes dans la source de données qui correspond à certains critères. Est une table conceptuelle qui résulte d’une requête et qui est disponible pour une application dans un format tabulaire. **Sélectionnez** instructions, fonctions de catalogue et certaines procédures créent des jeux de résultats. Dans l’exemple suivant, la première instruction SQL crée un jeu de résultats contenant toutes les lignes et toutes les colonnes de la table Orders, et la deuxième instruction SQL crée un jeu de résultats contenant les colonnes OrderID, vendeur et l’état pour les lignes de la table Orders dans lequel l’état est ouvert :  
  
```  
SELECT * FROM Orders  
SELECT OrderID, SalesPerson, Status FROM Orders WHERE Status = 'OPEN'  
```  
  
 Un jeu de résultats peut être vide, ce qui est différent d’aucun jeu de résultats. Par exemple, l’instruction SQL suivante crée un jeu de résultats vide :  
  
```  
SELECT * FROM Orders WHERE 1 = 2  
```  
  
 Un jeu de résultats vide est similaire à partir de n’importe quel autre ensemble, sauf qu’elle n’a aucune ligne de résultats. Par exemple, l’application peut récupérer des métadonnées pour le jeu de résultats peut essayer de récupérer les lignes et doit fermer le curseur sur le jeu de résultats.  
  
 Le processus d’extraction de lignes à partir de la source de données et leur renvoi à l’application est appelé *extraction*. Cette section décrit les composants de base de ce processus. Pour plus d’informations sur les rubriques plus avancées, telles que le bloc et les curseurs de défilement, consultez [curseurs de bloc](../../../odbc/reference/develop-app/block-cursors.md) et [curseurs permettant le défilement](../../../odbc/reference/develop-app/scrollable-cursors.md). Pour plus d’informations sur la mise à jour, suppression et l’insertion de lignes, consultez [mise à jour la vue d’ensemble des données](../../../odbc/reference/develop-app/updating-data-overview.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Un jeu de résultats a-t-il été créé ?](../../../odbc/reference/develop-app/was-a-result-set-created.md)  
  
-   [Métadonnées des jeux de résultats](../../../odbc/reference/develop-app/result-set-metadata.md)  
  
-   [Liaison de colonnes](../../../odbc/reference/develop-app/binding-columns.md)  
  
-   [Extraction de données](../../../odbc/reference/develop-app/fetching-data.md)  
  
-   [Fermeture du curseur](../../../odbc/reference/develop-app/closing-the-cursor.md)
