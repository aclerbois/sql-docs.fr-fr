---
title: L’exécution positionné instructions Update et Delete | Documents Microsoft
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
- positioned deletes [ODBC]
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- ODBC cursor library [ODBC], positioned update or delete
ms.assetid: 1d64f309-2a6e-4ad1-a6b5-e81145549c56
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e7099999311d565af4eeff5943306c927d81f3b9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executing-positioned-update-and-delete-statements"></a>L’exécution des instructions de suppression et de mise à jour positionnée
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Une fois une application a extrait un bloc de données avec **SQLFetchScroll**, il peut mettre à jour ou supprimer les données dans le bloc. Pour exécuter une mise à jour positionnée ou de suppression, de l’application :  
  
1.  Appels **SQLSetPos** pour positionner le curseur sur la ligne doit être mis à jour ou supprimé.  
  
2.  Construit une mise à jour positionnée ou une instruction delete avec la syntaxe suivante :  
  
     **Mise à jour** *-nom de la table*  
  
     **Définissez** *identificateur de la colonne* **=** {*expression* &#124; **NULL**}  
  
     [**,** *identificateur de la colonne* **=** {*expression* &#124; **NULL**}]  
  
     **WHERE CURRENT OF** *nom de curseur*  
  
     **DELETE FROM** *-nom de la table* **WHERE CURRENT OF** *nom de curseur*  
  
     Le moyen le plus simple pour construire le **définir** clause dans une instruction de mise à jour positionnée consiste à utiliser des marqueurs de paramètre pour chaque colonne pour être mis à jour et **SQLBindParameter** pour lier ces mémoires tampons de l’ensemble de lignes pour la ligne à mettre à jour. Dans ce cas, le type de données C du paramètre doit être le même que le type de données C de la mémoire tampon d’ensemble de lignes.  
  
3.  Met à jour les mémoires tampons d’ensemble de lignes de la ligne actuelle si elle s’exécute une instruction de mise à jour positionnée. Après avoir exécuté une instruction de mise à jour positionnée, la bibliothèque de curseurs copie les valeurs de chaque colonne dans la ligne actuelle à son cache.  
  
    > [!CAUTION]  
    >  Si l’application ne pas correctement à jour les mémoires tampons de lignes avant d’exécuter une instruction de mise à jour positionnée, les données dans le cache sera incorrectes après l’exécution de l’instruction.  
  
4.  Exécute la mise à jour positionnée ou une instruction delete à l’aide d’une instruction autre que l’instruction associée au curseur.  
  
    > [!CAUTION]  
    >  Le **où** clause construit par la bibliothèque de curseurs pour identifier la ligne actuelle peut ne pas identifier toutes les lignes, pour identifier une ligne différente ou identifier plusieurs lignes. Pour plus d’informations, consultez [construire des instructions recherché](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 Tous les positionné mise à jour et des instructions de suppression requièrent un nom de curseur. Pour spécifier le nom du curseur, une application appelle **SQLSetCursorName** avant l’ouverture du curseur. Pour utiliser le nom de curseur généré par le pilote, une application appelle **SQLGetCursorName** après l’ouverture du curseur.  
  
 Après le curseur library exécute une mise à jour positionnée ou instruction delete, le tableau d’état, mémoires tampons d’ensemble de lignes et cache géré par la bibliothèque de curseurs contiennent les valeurs indiquées dans le tableau suivant.  
  
|Instruction utilisée|Valeur de tableau d’état de ligne|Valeurs dans<br /><br /> tampons de l’ensemble de lignes|Valeurs dans<br /><br /> mémoires tampons de cache|  
|--------------------|-------------------------------|----------------------------------|---------------------------------|  
|Mise à jour positionnée|SQL_ROW_UPDATED|Nouvelles valeurs [1]|Nouvelles valeurs [1]|  
|Suppression positionnée|SQL_ROW_DELETED|Anciennes valeurs|Anciennes valeurs|  
  
 [1], l’application doit mettre à jour les valeurs dans les tampons de l’ensemble de lignes avant d’exécuter l’instruction de mise à jour positionnée ; Après avoir exécuté l’instruction de mise à jour positionnée, la bibliothèque de curseurs copie les valeurs dans les tampons de l’ensemble de lignes dans son cache.
