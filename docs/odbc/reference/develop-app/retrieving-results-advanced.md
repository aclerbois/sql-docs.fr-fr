---
title: "La récupération des résultats (avancés) | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: odbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- offsets [ODBC]
- result sets [ODBC], about result sets
- bind offsets [ODBC]
ms.assetid: bc00c379-71a7-407a-975c-898243f39bb6
caps.latest.revision: "5"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 85c3447abebf7ef6eaa538a8a1d5d00edcc007fd
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="retrieving-results-advanced"></a>La récupération des résultats (avancés)
Une application peut spécifier qu’un décalage est ajouté lié des adresses de mémoire tampon de données et l’indicateur de longueur/correspondante des adresses de mémoire tampon quand **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll**, ou **SQLSetPos** est appelée. Les résultats de ces ajouts déterminent les adresses utilisées dans ces opérations.  
  
 Les décalages de liaison permettent à une application modifier les liaisons sans appeler **SQLBindCol** pour les colonnes liées précédemment. Un appel à **SQLBindCol** pour relier des données change l’adresse de la mémoire tampon et le pointeur de longueur / d’indicateur. La reliaison avec un décalage, ajoute quant à eux, simplement un offset à l’adresse de mémoire tampon de données liées existante et adresse de mémoire tampon de longueur / d’indicateur. Lorsque les décalages sont utilisés, les liaisons sont un « modèle » de la disposition des mémoires tampons de l’application et l’application peut passer cet « modèle » à différentes zones de mémoire en modifiant le décalage. Un nouveau décalage peut être spécifié à tout moment et est toujours ajouté pour les valeurs liées à l’origine.  
  
 Pour spécifier un décalage de la liaison, l’application définit l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR à l’adresse d’une mémoire tampon SQLINTEGER. Avant que l’application appelle une fonction qui utilise les liaisons, tel que **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll**, ou **SQLSetPos**, il place un offset en octets dans cette mémoire tampon, tant que l’adresse de mémoire tampon de données, ni l’adresse de mémoire tampon de longueur / d’indicateur est 0, et que la colonne liée est dans le jeu de résultats. La somme de l’adresse et le décalage doit être une adresse valide. (Cela signifie qu’un ou les deux le décalage et l’adresse à laquelle le décalage est ajouté peuvent être non valides, tant que leur somme est une adresse valide.) L’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR est un pointeur, afin que la valeur de décalage peut être appliquée à plusieurs jeux de liaison de données, qui peut être modifiée en modifiant une valeur de décalage. Une application doit s’assurer que le pointeur reste valid jusqu'à ce que le curseur est fermé.  
  
> [!NOTE]  
>  Les décalages de liaison ne sont pas pris en charge par ODBC 2. *x* pilotes.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Curseurs de bloc](../../../odbc/reference/develop-app/block-cursors.md)  
  
-   [Curseurs avec défilement](../../../odbc/reference/develop-app/scrollable-cursors.md)  
  
-   [Bibliothèque de curseurs ODBC](../../../odbc/reference/develop-app/the-odbc-cursor-library.md)  
  
-   [Résultats multiples](../../../odbc/reference/develop-app/multiple-results.md)
