---
title: SQLGetData (bibliothèque de curseurs) | Documents Microsoft
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
- SQLGetData function [ODBC], Cursor Library
ms.assetid: ff40c9c0-b847-4426-a099-1bff47e6e872
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 96ffa184247c4fd7d05300952133c19127f979af
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetdata-cursor-library"></a>SQLGetData (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Cette rubrique décrit l’utilisation de la **SQLGetData** fonction dans la bibliothèque de curseurs. Pour des informations générales sur **SQLGetData**, consultez [fonction SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
 La bibliothèque de curseurs implémente **SQLGetData** en commençant par construire un **sélectionnez** instruction avec un **où** clause qui énumère les valeurs stockées dans son cache pour chaque colonne dépendante dans la ligne actuelle. Il exécute ensuite le **sélectionnez** instruction à nouveau la ligne et les appels **SQLGetData** dans le pilote pour récupérer les données à partir de la source de données (par opposition au cache).  
  
> [!CAUTION]  
>  Le **où** clause construit par la bibliothèque de curseurs pour identifier la ligne actuelle peut ne pas identifier toutes les lignes, pour identifier une ligne différente ou identifier plusieurs lignes. Pour plus d’informations, consultez [construire des instructions recherché](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 Si l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a la valeur SQL_UB_VARIABLE, **SQLGetData** peut être appelée sur la colonne 0 pour retourner des données de signet.  
  
 Les appels à **SQLGetData** sont soumis aux restrictions suivantes :  
  
-   **SQLGetData** ne peut pas être appelée pour les curseurs avant uniquement.  
  
-   **SQLGetData** peut être appelée uniquement lorsque les conditions suivantes sont remplies : un **sélectionnez** instruction générée le jeu de résultats ; le **sélectionnez** instruction ne contenait pas d’une jointure, une  **UNION** clause, ou un **GROUP BY** clause ; et toutes les colonnes qui a utilisé un alias ou une expression dans la liste select n’étaient pas liés avec **SQLBindCol**.  
  
-   Si le pilote prend en charge qu’une seule instruction active, la bibliothèque de curseurs extrait le reste du résultat défini avant l’exécution du **sélectionnez** instruction et en appelant **SQLGetData**.
