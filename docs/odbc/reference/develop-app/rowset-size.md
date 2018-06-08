---
title: Taille de l’ensemble de lignes | Documents Microsoft
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
- rowset size [ODBC]
- cursors [ODBC], block
- result sets [ODBC], rowset size
- block cursors [ODBC]
- result sets [ODBC], block cursors
ms.assetid: 60366ae8-175c-456a-ae5e-bdd860786911
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c0fa3d2feb8bcd3c4c342567e67f403edfb8029a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rowset-size"></a>Taille de l’ensemble de lignes
La taille de l’ensemble de lignes à utiliser dépend de l’application. Applications basées sur l’écran suivent généralement une des deux stratégies. La première consiste à définir la taille de l’ensemble de lignes au nombre de lignes affichées sur l’écran ; Si l’utilisateur redimensionne l’écran, l’application modifie la taille de l’ensemble de lignes en conséquence. La seconde consiste à définir la taille de l’ensemble de lignes à un plus grand nombre, telle que 100, ce qui réduit le nombre d’appels à la source de données. L’application fait défiler localement dans l’ensemble de lignes lorsque cela est possible et extrait les nouvelles lignes uniquement lorsqu’il fait défiler à l’extérieur de l’ensemble de lignes.  
  
 Autres applications, tels que les rapports, ont tendance à la valeur de la taille de l’ensemble de lignes le plus grand nombre de lignes que l’application peut gérer raisonnablement, avec un plus grand ensemble de lignes, le réseau de traitement par ligne est parfois réduit. Exactement quelle taille un ensemble de lignes peut être dépend de la taille de chaque ligne et la quantité de mémoire disponible.  
  
 Taille de l’ensemble de lignes est définie par un appel à **SQLSetStmtAttr** avec un *attribut* argument de SQL_ATTR_ROW_ARRAY_SIZE. L’application peut modifier la taille de l’ensemble de lignes, et lier le nouvel ensemble de lignes tampons (en appelant **SQLBindCol** ou en spécifiant un décalage de liaison) même après que les lignes qui ont été extraites, ou les deux. Les conséquences de la modification de la taille de l’ensemble de lignes dépendent de la fonction :  
  
-   **SQLFetch** et **SQLFetchScroll** utiliser la taille de l’ensemble de lignes au moment de l’appel pour déterminer le nombre de lignes à extraire. Toutefois, **SQLFetchScroll** avec un *FetchOrientation* d’incréments SQL_FETCH_NEXT le curseur en fonction de l’ensemble de lignes de l’extraction précédente et les extractions puis un ensemble de lignes en fonction de la taille d’ensemble de lignes en cours.  
  
-   **SQLSetPos** utilise la taille de l’ensemble de lignes qui est en vigueur à partir de l’appel précédent à **SQLFetch** ou **SQLFetchScroll**, car **SQLSetPos** opère sur un ensemble de lignes qui a déjà été défini. **SQLSetPos** également prennent en charge la nouvelle taille de l’ensemble de lignes si **SQLBulkOperations** a été appelée après la modification de la taille de l’ensemble de lignes.  
  
-   **SQLBulkOperations** utilise la taille de l’ensemble de lignes en vigueur au moment de l’appel, car elle effectue des opérations sur une table indépendante de tout ensemble de lignes extraite.
