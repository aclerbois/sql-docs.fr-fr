---
title: Défilement relative et absolue | Documents Microsoft
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
- absolute scrolling [ODBC]
- relative scrolling [ODBC]
- scrollable cursors [ODBC]
- cursors [ODBC], scrollable
ms.assetid: 3d0ff48d-fef5-4c01-bb1d-a583e6269b66
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 93f92b1cd5369ade8102b2505ef37f1da29dde4f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="relative-and-absolute-scrolling"></a>Absolues et le défilement
La plupart des options de défilement dans **SQLFetchScroll** positionnez le curseur par rapport à la position actuelle ou à une position absolue. **SQLFetchScroll** prend en charge l’extraction de la suivante, précédente, première et dernière ensembles de lignes, comme l’extraction de bien comme étant relative (extraire l’ensemble de lignes *n* lignes à partir du début de l’ensemble de lignes en cours) et absolu l’extraction (fetch le démarrage de l’ensemble de lignes à la ligne *n*). Si *n* est négatif dans une extraction absolue, les lignes sont comptées à partir de la fin du jeu de résultats. Par conséquent, une extraction absolue de ligne -1 signifie que l’extraction l’ensemble de lignes qui commence par la dernière ligne du jeu de résultats.  
  
 Les curseurs dynamiques détectent les lignes insérées et supprimées du jeu de résultats, il n’existe aucun moyen pour les curseurs dynamiques récupérer la ligne à un nombre autre que la lecture à partir du début du jeu de résultats, qui est susceptible d’être lente. En outre, l’extraction absolue n’est pas très utile dans les curseurs dynamiques, car les numéros de ligne modifié lorsque des lignes sont insérées et supprimées ; Par conséquent, l’extraction de successivement le même nombre de lignes peut produire une des lignes différentes.  
  
 Les applications qui utilisent **SQLFetchScroll** son bloc des fonctionnalités de curseur, tels que les rapports, sont susceptibles de transmettre le jeu de résultats d’une seule fois, à l’aide de l’option uniquement pour extraire l’ensemble de lignes suivant. Applications basées sur l’écran, en revanche, peuvent tirer parti de toutes les fonctions de **SQLFetchScroll**. Si l’application définit la taille de l’ensemble de lignes sur le nombre de lignes affichées sur l’écran et lie les mémoires tampon d’écran au jeu de résultats, il peut traduire directement à des appels à des opérations de barre de défilement **SQLFetchScroll**.  
  
|Opération de barre de défilement|Option de défilement SQLFetchScroll|  
|--------------------------|-------------------------------------|  
|Page précédente|SQL_FETCH_PRIOR|  
|Pg. suiv|SQL_FETCH_NEXT|  
|Ligne précédente|SQL_FETCH_RELATIVE avec *FetchOffset* égale à – 1|  
|Ligne suivante|SQL_FETCH_RELATIVE avec *FetchOffset* égal à 1|  
|Case de défilement en haut|SQL_FETCH_FIRST|  
|Case de défilement en bas|SQL_FETCH_LAST|  
|Position aléatoire de la case de défilement|SQL_FETCH_ABSOLUTE|  
  
 Ces applications doivent également positionner le curseur de défilement après une opération de défilement, ce qui nécessite le numéro de ligne en cours et le nombre de lignes. Pour le numéro de ligne actuel, les applications peuvent soit effectuer le suivi du numéro de ligne en cours ou l’appel **SQLGetStmtAttr** avec l’attribut SQL_ATTR_ROW_NUMBER à récupérer.  
  
 Le nombre de lignes dans le curseur, ce qui est la taille du résultat défini, il est disponible en tant que le champ SQL_DIAG_CURSOR_ROW_COUNT de l’en-tête de diagnostic. La valeur de ce champ est définie uniquement après avoir **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResult** a été appelée. Ce nombre peut être un nombre approximatif d’ou d’un nombre exact, en fonction des capacités du pilote. Prise en charge du pilote peut être déterminé en appelant **SQLGetInfo** avec les types d’informations de curseur attributs et en vérifiant si le bit SQL_CA2_CRC_APPROXIMATE ou SQL_CA2_CRC_EXACT est retourné pour le type de curseur.  
  
 Un nombre exact de lignes n’est jamais pris en charge pour un curseur dynamique. Pour les autres types de curseurs, le pilote peut prendre en charge des nombres de lignes exact ou approximatif, mais pas les deux. Si le pilote prend en charge ni exacte ni approximative du nombre de lignes pour un type de curseur spécifique, le champ SQL_DIAG_CURSOR_ROW_COUNT contient le nombre de lignes qui ont été extraites jusqu'à présent. Quelle que soit la définition du pilote prend en charge, **SQLFetchScroll** avec un *opération* de SQL_FETCH_LAST entraînera le champ SQL_DIAG_CURSOR_ROW_COUNT contenir le nombre exact de lignes.
