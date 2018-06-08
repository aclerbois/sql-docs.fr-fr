---
title: 'Étape 4 a : extraire les résultats | Documents Microsoft'
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
- application process [ODBC], fetching results
- fetches [ODBC], fetching results
ms.assetid: 77d30142-c774-473c-96fb-b364bb92ac60
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7d8d2e5bd80e47db5a5e3484aebfe2bf1d6fff9a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-4a-fetch-the-results"></a>Étape 4 a : extraire les résultats
L’étape suivante consiste à extraire les résultats, comme indiqué dans l’illustration suivante.  
  
 ![Présente l’extraction des résultats dans une application ODBC](../../../odbc/reference/develop-app/media/pr14.gif "pr14")  
  
 Si l’instruction exécutée dans « Étape 3 : générer et exécuter une instruction SQL » était un **sélectionnez** instruction ou une fonction de catalogue, l’application appelle d’abord **SQLNumResultCols** pour déterminer le nombre de colonnes dans le jeu de résultats. Cette étape n’est pas nécessaire si l’application a déjà connaît le nombre de résultats de définir des colonnes, telles que lorsque l’instruction SQL est codée en dur dans une application verticale ou personnalisée.  
  
 Ensuite, l’application récupère le nom de type de données, précision et échelle de chaque colonne du jeu de résultats avec **SQLDescribeCol**. Là encore, cela n’est pas nécessaire pour les applications telles que les applications verticales et personnalisées que vous connaissez déjà ces informations. L’application transmet ces informations à **SQLBindCol**, qui lie une variable d’application à une colonne dans le jeu de résultats.  
  
 L’application maintenant appelle **SQLFetch** pour récupérer la première ligne de données et de placer les données à partir de cette ligne dans les variables liées avec **SQLBindCol**. Si la ligne contient toutes les données de type long, il appelle ensuite **SQLGetData** pour récupérer ces données. L’application continue à appeler **SQLFetch** et **SQLGetData** pour extraire des données supplémentaires. Après avoir terminé l’extraction de données, il appelle **SQLCloseCursor** pour fermer le curseur.  
  
 Pour obtenir une description complète de la récupération des résultats, consultez [la récupération des résultats (Basic)](../../../odbc/reference/develop-app/retrieving-results-basic.md) et [la récupération des résultats (Avancé)](../../../odbc/reference/develop-app/retrieving-results-advanced.md).  
  
 L’application maintenant retourne à le « Étape 3 : générer et exécuter une instruction SQL » pour exécuter une autre instruction dans la même transaction ; ou passe à « Étape 5 : la Transaction de validation » pour valider ou restaurer la transaction.
