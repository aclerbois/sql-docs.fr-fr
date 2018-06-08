---
title: À l’aide de la bibliothèque de curseurs ODBC | Documents Microsoft
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
- ODBC cursor library [ODBC], using cursor library
- cursor library [ODBC], using cursor library
ms.assetid: 9653f2f8-ccfc-4220-99ef-601dc0fa641c
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b9a5e1a11b5e8ba12308aaed342d6cf30e688e56
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-the-odbc-cursor-library"></a>À l’aide de la bibliothèque de curseurs ODBC
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Pour utiliser la bibliothèque de curseurs ODBC, une application :  
  
1.  Appels **SQLSetConnectAttr** avec un *attribut* de SQL_ATTR_ODBC_CURSORS pour spécifier comment la bibliothèque de curseurs doit être utilisée avec une connexion particulière. La bibliothèque de curseurs peut être toujours utilisés (SQL_CUR_USE_ODBC), jamais utilisé uniquement si le pilote ne prend pas en charge les curseurs de défilement (SQL_CUR_USE_IF_NEEDED) ou (SQL_CUR_USE_DRIVER).  
  
2.  Appels **SQLConnect**, **SQLDriverConnect**, ou **SQLBrowseConnect** pour se connecter à la source de données.  
  
3.  Appels **SQLSetStmtAttr** pour spécifier le type de curseur (SQL_ATTR_CURSOR_TYPE), la concurrence (SQL_ATTR_CONCURRENCY) et la taille de l’ensemble de lignes (SQL_ATTR_ROW_ARRAY_SIZE). La bibliothèque de curseurs prend en charge les curseurs avant uniquement et statiques. Les curseurs avant uniquement doivent être en lecture seule, tandis que les curseurs statiques peuvent être en lecture seule ou peuvent utiliser le contrôle d’accès concurrentiel optimiste comparaison de valeurs.  
  
4.  Alloue un ou plusieurs tampons de l’ensemble de lignes et les appels **SQLBindCol** une ou plusieurs fois pour lier ces mémoires tampons entraîner des colonnes du jeu.  
  
5.  Génère un jeu de résultats en exécutant un **sélectionnez** instruction ou une procédure, ou en appelant une fonction de catalogue. Si l’application exécute les instructions de mise à jour positionnée, il doit exécuter un **sélectionner pour la mise à jour** instruction pour générer le jeu de résultats.  
  
6.  Appels **SQLFetch** ou **SQLFetchScroll** une ou plusieurs fois pour faire défiler le jeu de résultats.  
  
 L’application peut modifier les valeurs de données dans les tampons de l’ensemble de lignes. Pour actualiser les tampons de l’ensemble de lignes avec des données à partir du cache de la bibliothèque curseurs, une application appelle **SQLFetchScroll** avec la *FetchOrientation* argument a la valeur SQL_FETCH_RELATIVE et *FetchOffset* argument défini à 0.  
  
 Pour récupérer des données à partir d’une colonne indépendante, l’application appelle **SQLSetPos** pour positionner le curseur sur la ligne de votre choix. Il appelle ensuite **SQLGetData** pour récupérer les données.  
  
 Pour déterminer le nombre de lignes qui ont été récupérées à partir de la source de données, l’application appelle **SQLRowCount**.
