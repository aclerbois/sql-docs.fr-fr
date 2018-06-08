---
title: Curseurs pilotés par jeu de clés | Documents Microsoft
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
- keyset-driven cursors [ODBC]
- cursors [ODBC], key-set driven
ms.assetid: 01769f43-1d9c-4685-84fa-15a6465335e9
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 13bb740a6d76ae97541f39b73d1cecf60bef61fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="keyset-driven-cursors"></a>Curseurs pilotés par jeu de clés
Un curseur keyset réside entre statique et d’un curseur dynamique dans sa capacité à détecter les modifications. À un curseur statique, il ne détecte pas toujours les modifications apportées à l’appartenance et l’ordre du jeu de résultats. Comme un curseur dynamique, il détecte les modifications apportées aux valeurs des lignes du jeu de résultats (selon le niveau d’isolation de la transaction, tel que défini par l’attribut de connexion SQL_ATTR_TXN_ISOLATION).  
  
 Lorsqu’un curseur est ouvert, il enregistre les clés pour le jeu de résultats entier ; Cela permet de corriger l’apparent appartenance et l’ordre du jeu de résultats. Lorsque le curseur fait défiler le jeu de résultats, il utilise les clés dans ce *keyset* pour récupérer les valeurs de données en cours pour chaque ligne. Par exemple, un curseur keyset extrait une ligne et une autre application, puis met à jour cette ligne. Si le curseur réextrait la ligne, les valeurs sont nouveaux, car il à nouveau extraites selon la ligne à l’aide de sa clé. Pour cette raison, les curseurs de toujours détectent les modifications apportées par eux-mêmes et d’autres.  
  
 Lorsque le curseur tente de récupérer une ligne qui a été supprimée, cette ligne apparaît comme un « trou » dans le jeu de résultats : la clé pour la ligne existe dans le jeu de clés, mais la ligne n’existe plus dans le jeu de résultats. Si les valeurs de clé dans une ligne sont mis à jour, la ligne est considérée comme supprimée, puis inséré, donc ces lignes apparaissent également sous forme de trous dans le jeu de résultats. Lors d’un curseur keyset peut toujours détecter les lignes supprimées par d’autres, il peut éventuellement supprimer les clés pour les lignes supprimées lui-même le jeu de clés. Curseurs que cela ne peut pas détecter leurs propres suppressions. Indique si un curseur keyset particulier détecte ses propres suppressions est signalé via l’option SQL_STATIC_SENSITIVITY dans **SQLGetInfo**.  
  
 Lignes insérées par d’autres ne sont jamais visibles par un curseur keyset, car aucune clé pour ces lignes n’existe dans le jeu de clés. Toutefois, un curseur keyset permettre éventuellement ajouter des clés pour les lignes qu’il insère pour le jeu de clés. Curseurs que cela peuvent détecter leurs propres insertions. Indique si un curseur keyset particulier détecte ses propres insertions est signalé via l’option SQL_STATIC_SENSITIVITY dans **SQLGetInfo**.  
  
 Le tableau d’état de ligne spécifié par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR peut contenir SQL_ROW_SUCCESS, SQL_ROW_SUCCESS_WITH_INFO ou SQL_ROW_ERROR pour n’importe quelle ligne. Elle retourne SQL_ROW_UPDATED, SQL_ROW_DELETED ou SQL_ROW_ADDED pour les lignes qu’il détecte une mise à jour, supprimées ou insérées.  
  
 Curseurs sont souvent implémentées en créant une table temporaire qui contient les clés pour chaque ligne du jeu de résultats. Étant donné que le curseur doit également déterminer si les lignes ont été mis à jour, cette table contient généralement une colonne avec les informations de contrôle de version de ligne.  
  
 Pour faire défiler sur le jeu de résultats d’origine, le curseur keyset ouvre un curseur statique sur la table temporaire. Pour récupérer une ligne dans le jeu de résultats d’origine, le curseur récupère d’abord la clé appropriée de la table temporaire, puis récupère les valeurs actuelles de la ligne. Si les curseurs de bloc sont utilisés, le curseur doit récupérer plusieurs lignes et les clés.
