---
title: 'Annexe f : bibliothèque de curseurs ODBC | Documents Microsoft'
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
- ODBC cursor library [ODBC], about cursor library
- ODBC cursor library [ODBC]
- cursor library [ODBC], about cursor library
- cursor library [ODBC]
ms.assetid: a03084df-4e48-48ef-917d-4a3fae48a605
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 10e43272c7d9a75956aa162398c2869c558cdea1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="appendix-f-odbc-cursor-library"></a>Annexe f : bibliothèque de curseurs ODBC
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 La bibliothèque de curseurs ODBC (Odbccr32.dll) prend en charge des curseurs permettant le défilement de bloc pour un pilote qui est compatible avec le niveau de conformité au niveau d’API 1 et peut être redistribuée par les développeurs avec leurs applications ou des pilotes. La bibliothèque de curseurs prend également en charge des instructions de suppression et de mise à jour positionnée pour les jeux de résultats générés par **sélectionnez** instructions. Il prend en charge uniquement curseurs statiques et avant uniquement, la bibliothèque de curseurs répond aux besoins de nombreuses applications. En outre, elle peut fournir les bonnes performances, en particulier pour les jeux de résultats de taille petite à moyenne taille et pour les applications qui n’ont pas de prise en charge du curseur de bonne.  
  
 La bibliothèque de curseurs est une bibliothèque de liens dynamiques (DLL) qui réside entre le Gestionnaire de pilotes et le pilote. Lorsqu’une application appelle une fonction, le Gestionnaire de pilotes appelle la fonction dans la bibliothèque de curseurs, qui exécute la fonction ou appelle dans le pilote spécifié. Pour une connexion donnée, une application spécifie si la bibliothèque de curseurs est toujours utilisée, utilisée si le pilote ne prend pas en charge les curseurs de défilement ou jamais utilisée.  
  
 La bibliothèque de curseurs s’affiche en tant que pilote pour le Gestionnaire de pilotes. Si la bibliothèque de curseurs réside entre le Gestionnaire de pilotes et une API ODBC 2. *x* pilote, la bibliothèque de curseurs apparaît comme un ODBC 2. *x* pilote. Si la bibliothèque de curseurs réside entre le Gestionnaire de pilotes et une ODBC 3 *.x* pilote, la bibliothèque de curseurs apparaît comme un ODBC 3 *.x* pilote. Le comportement affiché par la bibliothèque de curseurs varie selon la version du pilote que fonctionne avec, à l’exception des décalages de liaison, qui est pris en charge pour les deux API ODBC 2. *x* et ODBC 3. *x* pilotes.  
  
 Pour implémenter des curseurs de bloc en **SQLFetch** et **SQLFetchScroll**, la bibliothèque de curseurs appelle à plusieurs reprises **SQLFetch** dans le pilote. Pour implémenter le défilement, il met en cache les données qu’il a récupéré en mémoire et dans les fichiers de disque. Lorsqu’une application demande un nouvel ensemble de lignes, la bibliothèque de curseurs extrait selon les besoins du pilote ou du cache.  
  
 Pour implémenter la mise à jour positionnée et supprimer des instructions, la bibliothèque de curseurs construit une **mettre à jour** ou **supprimer** instruction avec un **où** clause qui spécifie la valeur mise en cache de chaque colonne liée de la ligne. Lorsqu’il exécute une instruction de mise à jour positionnée, la bibliothèque de curseurs met à jour son cache à partir des valeurs dans les tampons de l’ensemble de lignes.  
  
 Pour plus d’informations sur la bibliothèque de curseurs ODBC, consultez les sections suivantes de cette annexe :  
  
-   [Utilisation de la bibliothèque de curseurs ODBC](../../../odbc/reference/appendixes/using-the-odbc-cursor-library.md)  
  
-   [Exécution d’instructions de mise à jour et de suppression positionnées](../../../odbc/reference/appendixes/executing-positioned-update-and-delete-statements.md)  
  
-   [Exemple de code de la bibliothèque de curseurs](../../../odbc/reference/appendixes/cursor-library-code-example.md)  
  
-   [Remarques sur l’implémentation](../../../odbc/reference/appendixes/implementation-notes.md)  
  
-   [Codes d’erreur de la bibliothèque de curseurs ODBC](../../../odbc/reference/appendixes/odbc-cursor-library-error-codes.md)
