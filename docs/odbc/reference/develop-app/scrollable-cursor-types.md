---
title: Les Types de curseur de défilement | Documents Microsoft
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
- scrollable cursors [ODBC]
- cursors [ODBC], scrollable
ms.assetid: dbd32576-0453-4e90-ae45-1a81cee8259d
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 54acbd1010d546649b1ad92a34289fa4d04da162
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="scrollable-cursor-types"></a>Types de curseur de défilement
Les quatre types de curseurs de défilement sont statiques, dynamiques, pilotés par jeu de clés et mixte. Les curseurs statiques détectent peu ou pas de modifications, mais sont relativement peu coûteux à implémenter. Les curseurs dynamiques détectent toutes les modifications mais sont coûteux à implémenter. Les curseurs pilotés par jeu de clés et mixtes se situent entre la plupart des modifications mais utilisent moins de ressources que les curseurs dynamiques.  
  
 Les termes suivants sont utilisés pour définir les caractéristiques de chaque type de curseur de défilement :  
  
-   **Propriétaire des mises à jour, suppressions et insertions.** Les mises à jour, suppressions et insertions effectuées via le curseur, soit par un appel à **SQLBulkOperations** ou **SQLSetPos** ou avec une position mettre à jour ou supprimer l’instruction.  
  
-   **Autres mises à jour, supprime et insère.** Les mises à jour, suppressions et insertions ne pas effectuées par le curseur, y compris celles effectuées par d’autres opérations dans la même transaction, celles effectuées par d’autres transactions et celles effectuées par d’autres applications.  
  
-   **Appartenance au groupe.** L’ensemble de lignes du jeu de résultats.  
  
-   **Commande.** L’ordre dans lequel les lignes sont renvoyées par le curseur.  
  
-   **valeurs.** Les valeurs dans chaque ligne du jeu de résultats.  
  
 Pour plus d’informations sur la façon de mettre à jour, supprimer et insérer des données, consultez [mise à jour la vue d’ensemble des données](../../../odbc/reference/develop-app/updating-data-overview.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Curseurs statiques dans ODBC](../../../odbc/reference/develop-app/odbc-static-cursors.md)  
  
-   [Curseurs dynamiques dans ODBC](../../../odbc/reference/develop-app/odbc-dynamic-cursors.md)  
  
-   [Curseurs de jeu de clés](../../../odbc/reference/develop-app/keyset-driven-cursors.md)  
  
-   [Curseurs mixtes](../../../odbc/reference/develop-app/mixed-cursors.md)
