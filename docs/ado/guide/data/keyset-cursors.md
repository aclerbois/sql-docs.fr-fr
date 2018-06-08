---
title: Les curseurs keyset | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Keyset cursors [ADO]
- cursors [ADO], Keyset
ms.assetid: 14b51b17-6fd9-4146-af45-ca4b0fe6d48a
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6abebe52390c8c3423cd3c41f212236e051e1972
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="keyset-cursors"></a>Curseurs keyset
Le curseur de jeu de clés fournit les fonctionnalités entre un statique et un curseur dynamique dans sa capacité à détecter les modifications. À un curseur statique, il ne détecte pas toujours les modifications apportées à l’appartenance et l’ordre du jeu de résultats. Comme un curseur dynamique, il détecte des modifications aux valeurs de lignes dans le jeu de résultats.  
  
 Curseurs sont contrôlées par un ensemble d’identificateurs uniques (clés) appelé jeu de clés. Les clés sont créées à partir d'un ensemble de colonnes qui identifient uniquement les lignes de l'ensemble de résultats. Le jeu de clés est l’ensemble de valeurs de clé à partir de toutes les lignes retournées par l’instruction de requête.  
  
 Avec des curseurs pilotés par jeu de clés, une clé est générée et enregistrée pour chaque ligne du curseur et stockée sur la station de travail ou sur le serveur. Lorsque vous accédez à chaque ligne, la clé stockée est utilisée pour extraire les valeurs de données en cours à partir de la source de données. Dans un curseur keyset, l’appartenance au jeu de résultats est figée lorsque le jeu de clés est pleine. Par la suite, les ajouts ou modifications qui affectent l’appartenance ne font pas partie du jeu jusqu'à ce qu’il est rouvert de résultats.  
  
 Modifications apportées aux valeurs de données (effectuées par le propriétaire du jeu de clés ou d’autres processus) sont visibles lorsque l’utilisateur fait défiler le jeu de résultats. Les insertions effectuées en dehors du curseur (par d’autres processus) sont visibles uniquement si le curseur est fermé et rouvert. Les insertions effectuées à l’intérieur du curseur sont visibles à la fin du jeu de résultats.  
  
 Lorsqu’un curseur keyset tente de récupérer une ligne qui a été supprimée, la ligne apparaît comme un « trou » dans le jeu de résultats. La clé pour la ligne existe dans le jeu de clés, mais la ligne n’existe plus dans le jeu de résultats. Si les valeurs de clé dans une ligne sont mis à jour, la ligne est considérée comme supprimée, puis inséré, donc ces lignes apparaissent également sous forme de trous dans le jeu de résultats. Lors d’un curseur keyset peut toujours détecter les lignes supprimées par d’autres processus, il peut éventuellement supprimer les clés de lignes. Curseurs que cela ne peut pas détecter leurs propres suppressions car la preuve a été supprimée.  
  
 Une mise à jour une colonne de clé équivaut à la suppression de l’ancienne clé suivie par une insertion de la nouvelle clé. La nouvelle valeur de clé n’est pas visible si la mise à jour n’a pas été mise à travers le curseur. Si la mise à jour a été effectuée via le curseur, la nouvelle valeur de clé est visible à la fin du jeu de résultats.  
  
 Il existe une variante sur les curseurs pilotés par jeu de clés appelée curseurs standards. Dans un curseur standard piloté par jeu de clés, l’appartenance des lignes du jeu de résultats et l’ordre des lignes sont fixés à l’ouverture du curseur, mais les modifications apportées aux valeurs qui sont effectuées par le propriétaire du curseur et les modifications validées apportées par d’autres processus sont visibles. Si une modification ne permet pas d’une ligne pour l’appartenance ou affecte l’ordre d’une ligne, la ligne ne disparaît ni ne déplacez, sauf si le curseur est fermé et rouvert. Données insérées n’apparaissant pas, mais les modifications apportées aux données existantes s’affichent que les lignes sont extraites.  
  
 Le curseur est difficile à utiliser correctement car la sensibilité aux modifications de données dépend des circonstances différentes, comme décrit ci-dessus. Toutefois, si votre application n’est pas concernée par les mises à jour simultanées permettre gérer par programme les mauvaises clés et doit accéder directement à certaines lignes dotées de clés, le curseur keyset fonctionnent pour vous. Utilisez le **adOpenKeyset CursorTypeEnum** pour indiquer que vous souhaitez utiliser un curseur de jeu de clés dans ADO.  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs avant uniquement](../../../ado/guide/data/forward-only-cursors.md)   
 [Curseurs statiques](../../../ado/guide/data/static-cursors.md)   
 [Curseurs dynamiques](../../../ado/guide/data/dynamic-cursors.md)
