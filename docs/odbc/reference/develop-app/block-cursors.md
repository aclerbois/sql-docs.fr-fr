---
title: Les curseurs de bloc | Documents Microsoft
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
- cursors [ODBC], block
- block cursors [ODBC]
- result sets [ODBC], block cursors
ms.assetid: 1a92b5d8-7c6e-4ce5-8c99-600a387026aa
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 21bbfa223e2eb58df8fc89b69f7f9df3c08983e8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="block-cursors"></a>Curseurs de bloc
De nombreuses applications passent beaucoup de temps mettre des données sur le réseau. Partie de ce temps est consacré à mettre jour les données sur le réseau et partie est passée sur le réseau de surcharge, telles que l’appel effectué par le pilote pour demander une ligne de données. L’heure de ce dernier peut être réduit si l’application effectue une utilisation efficace de *bloc,* ou *fat,* *curseurs,* qui peut retourner plusieurs lignes à la fois.  
  
 Toujours une application a la possibilité d’utiliser un curseur de bloc. Sur les sources de données à partir de laquelle qu’une seule ligne à la fois peut être extraites, les curseurs de bloc doivent être simulées dans le pilote. Pour ce faire, vous pouvez effectuer plusieurs extractions de ligne unique. Bien que cela soit peu susceptible de fournir des gains de performances, il ouvre des opportunités pour les applications. De telles applications puis bénéficient d’améliorer les performances de SGBD implémente des curseurs de bloc en mode natif et les pilotes associés à ces SGBD les exposent.  
  
 Les lignes retournées dans une seule extraction avec un curseur de bloc sont appelés les *ensemble de lignes*. Il est important de ne pas confondre l’ensemble de lignes avec le jeu de résultats. Le jeu de résultats est maintenu à la source de données, tandis que l’ensemble de lignes est conservé dans les tampons de l’application. Alors que le jeu de résultats est fixe, l’ensemble de lignes n’est pas, il change de position et de contenu chaque fois un nouvel ensemble de lignes est extraites. Tout comme un curseur de la seule ligne, telles que les points de curseur avant uniquement SQL traditionnels à une ligne en cours, un curseur de bloc pointe vers l’ensemble de lignes, ce qui peut être considéré comme *lignes actuelles*.  
  
 Pour effectuer des opérations qui fonctionnent sur une seule ligne lorsque plusieurs lignes qui ont été extraites, l’application doit indiquer tout d’abord la ligne qui est la ligne actuelle. La ligne actuelle est requis par les appels à **SQLGetData** et positionné mise à jour et supprimer les instructions. Lorsque le curseur de bloc retourne tout d’abord un ensemble de lignes, la ligne actuelle est la première ligne de l’ensemble de lignes. Pour modifier la ligne actuelle, l’application appelle **SQLSetPos** ou **SQLBulkOperations** (mettre à jour par le signet). L’illustration suivante montre la relation entre le jeu de résultats, ensemble de lignes, ligne courante, ensemble de lignes du curseur et curseur de bloc. Pour plus d’informations, consultez [à l’aide de curseurs de bloc](../../../odbc/reference/develop-app/using-block-cursors.md), plus loin dans cette section, et [positionné instructions Update et Delete](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md) et [mise à jour des données avec SQLSetPos](../../../odbc/reference/develop-app/updating-data-with-sqlsetpos.md).  
  
 ![L’extraction suivante, avant, les premier et dernier ensembles de lignes](../../../odbc/reference/develop-app/media/pr20_2.gif "pr20_2")  
  
 Si un curseur est un curseur de bloc est indépendante de s’il s’agit permettant le défilement. Par exemple, la plupart du travail dans une application de rapports est consacré à la récupération et l’impression des lignes. Pour cette raison, il sont plus rapides avec un curseur avant uniquement, curseur de bloc. Il utilise un curseur avant uniquement afin d’éviter les dépenses d’un curseur de défilement et un curseur de bloc pour réduire le trafic réseau.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Liaison de colonnes pour une utilisation avec des curseurs de bloc](../../../odbc/reference/develop-app/binding-columns-for-use-with-block-cursors.md)  
  
-   [Utilisation de curseurs de bloc](../../../odbc/reference/develop-app/using-block-cursors.md)  
  
-   [Tableau d’état des lignes](../../../odbc/reference/develop-app/row-status-array.md)
