---
title: Liaison de colonnes pour une utilisation avec les curseurs de bloc | Documents Microsoft
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
- column-wise binding [ODBC]
- row-wise binding [ODBC]
- result sets [ODBC], binding columns
- cursors [ODBC], block
- binding columns [ODBC]
- block cursors [ODBC]
- result sets [ODBC], block cursors
ms.assetid: 231beede-cdfa-4e28-8b10-2760b983250f
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f9bee14abc4bbdbad17360666d40a7d59af5480d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="binding-columns-for-use-with-block-cursors"></a>Liaison de colonnes pour une utilisation avec les curseurs de bloc
Étant donné que les curseurs de bloc retournent plusieurs lignes, les applications qui les utilisent doivent lier un tableau de variables à chaque colonne plutôt qu’une variable unique. Ces tableaux est désignés collectivement par le *tampons de l’ensemble de lignes*. Voici les deux styles de liaison :  
  
-   Lier un tableau à chaque colonne. Il s’agit *liaison selon les colonnes* , car chaque structure de données (tableau) contient des données pour une colonne unique.  
  
-   Définissez une structure pour contenir les données d’une ligne entière et de lier un tableau de ces structures. Il s’agit *liaison selon les lignes* , car chaque structure de données contient les données d’une seule ligne.  
  
 Comme lors de l’application lie des variables uniques aux colonnes, il appelle **SQLBindCol** pour lier les tableaux de colonnes. La seule différence est que les adresses sont transmises sont des adresses de tableau, les adresses de variables non unique. L’application définit l’attribut d’instruction SQL_BIND_BY_COLUMN pour spécifier si elle est à l’aide de la liaison selon les colonnes ou lignes. S’il faut utiliser la liaison selon les colonnes ou lignes est essentiellement une question de préférence de l’application. La liaison peut correspondre plus étroitement à la disposition de l’application des données, auquel cas il serait offrent de meilleures performances.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Liaison selon les colonnes](../../../odbc/reference/develop-app/column-wise-binding.md)  
  
-   [Liaison selon les lignes](../../../odbc/reference/develop-app/row-wise-binding.md)
