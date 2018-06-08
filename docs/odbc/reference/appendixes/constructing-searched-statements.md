---
title: Construction de recherche des instructions | Documents Microsoft
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
- searched statements [ODBC]
- ODBC cursor library [ODBC], statement processing
- ODBC cursor library [ODBC], searched statements
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
- cursor library [ODBC], searched statements
- SQL statements [ODBC], searched statements
ms.assetid: e429254c-c43f-4fbf-98b2-5f1ed53501ff
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: feeebf8ffad30aa5897471c95e8916617e3907ee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="constructing-searched-statements"></a>Construction de recherche des instructions
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Pour prendre en charge la mise à jour positionnée et supprimer des instructions, la bibliothèque de curseurs construit une recherche **mettre à jour** ou **supprimer** instruction de la position. Pour prendre en charge les appels à **SQLGetData** dans un bloc de données, la bibliothèque de curseurs construit une recherche **sélectionnez** instruction pour créer un résultat de la valeur qui contient la ligne actuelle de données. Dans chacune de ces instructions, le **où** clause énumère les valeurs stockées dans le cache pour chaque colonne dépendante qui renvoie SQL_PRED_SEARCHABLE ou SQL_PRED_BASIC pour l’identificateur de champ SQL_DESC_SEARCHABLE dans **SQLColAttribute**.  
  
> [!CAUTION]  
>  Le **où** clause construit par la bibliothèque de curseurs pour identifier la ligne actuelle peut ne pas identifier toutes les lignes, pour identifier une ligne différente ou identifier plusieurs lignes.  
  
 Si une mise à jour positionnée ou une instruction delete affecte plusieurs lignes, la bibliothèque de curseurs met à jour le tableau d’état de ligne uniquement pour la ligne sur laquelle le curseur est positionné et retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01001 (conflit d’opération de curseur). Si l’instruction n’identifie pas de toutes les lignes, la bibliothèque de curseurs ne met pas à jour le tableau d’état de ligne et retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01001 (conflit d’opération de curseur). Une application peut appeler **SQLRowCount** pour déterminer le nombre de lignes qui ont été mis à jour ou supprimés.  
  
 Si le **sélectionnez** clause utilisé pour positionner le curseur pour un appel à **SQLGetData** identifie plusieurs lignes, **SQLGetData** n’est pas garanti pour retourner les données correctes. Si elle n’identifie pas de toutes les lignes, **SQLGetData** retourne SQL_NO_DATA.  
  
 Si une application est conforme aux instructions suivantes, le **où** clause construit par la bibliothèque de curseurs doit identifier de façon unique la ligne actuelle, sauf lorsque cela est impossible, par exemple lorsque la source de données contient des lignes en double.  
  
-   **Lier les colonnes qui identifient de façon unique la ligne.** Si les colonnes liées n’identifient pas la ligne, le **où** clause construit par la bibliothèque de curseurs peut identifier plusieurs lignes. Dans une mise à jour positionnée ou une instruction delete, une telle clause peut entraîner plusieurs lignes d’être mis à jour ou supprimé. Dans un appel à **SQLGetData**, une telle clause peut entraîner le pilote retourne les données de la ligne incorrecte. Liaison de toutes les colonnes d’une clé unique garantit que chaque ligne est identifié de manière unique.  
  
-   **Allouer des tampons de données assez volumineux qui n’est pas tronqué.** Cache de la bibliothèque curseur est une copie des valeurs dans les tampons de l’ensemble de lignes liés à un jeu de résultats avec **SQLBindCol**. Si les données sont tronquées lorsqu’elle est placée dans ces mémoires tampons, il sera tronqué également dans le cache. A **où** clause construite à partir de valeurs tronquées ne peut pas identifier correctement la ligne sous-jacente dans la source de données.  
  
-   **Spécifiez des tampons de longueur de non null pour les données binaires de C.** La bibliothèque de curseurs alloue des tampons de longueur dans son cache que si la *StrLen_or_IndPtr* argument dans **SQLBindCol** n’est pas null. Lorsque le *TargetType* argument est SQL_C_BINARY, la bibliothèque de curseurs nécessite la longueur des données binaires pour construire un **où** clause à partir des données. S’il n’existe aucune mémoire tampon de longueur pour une colonne SQL_C_BINARY et l’application appelle **SQLGetData** ou tente d’exécuter une mise à jour positionnée ou la bibliothèque de curseurs, l’instruction delete retourne SQL_ERROR et SL014 SQLSTATE (une requête positionnée était le nombre de colonnes d’émis et pas tous les champs ont été mis en mémoire tampon).  
  
-   **Spécifiez des tampons de longueur de non null pour les colonnes autorisant des valeurs NULL.** La bibliothèque de curseurs alloue des tampons de longueur dans son cache que si la *StrLen_or_IndPtr* argument dans **SQLBindCol** n’est pas null. Dans la mesure où les SQL_NULL_DATA est stocké dans la mémoire tampon de longueur, la bibliothèque de curseurs suppose que n’importe quelle colonne pour l’aucune longueur tampon est indiquée est non nullable. Si aucune colonne de longueur n’est spécifiée pour une colonne acceptant les valeurs NULL, la bibliothèque de curseurs construit un **où** clause qui utilise la valeur de données pour la colonne. Cette clause ne peut pas identifier correctement la ligne.
