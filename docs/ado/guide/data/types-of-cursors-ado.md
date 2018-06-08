---
title: Types de curseurs (ADO) | Documents Microsoft
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
- cursors [ADO], types
ms.assetid: 7cc01544-e814-403b-bbfe-a2750bf921bd
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2a90421cb473d280586ed3c7877e2a188f545eda
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="types-of-cursors-ado"></a>Types de curseurs (ADO)
En règle générale, votre application doit utiliser le curseur de la plus simple qui fournit l’accès aux données requises. Toutes les caractéristiques supplémentaires au-delà des principes de base (avant uniquement, en lecture seule, statique, défilement, sans tampon) a un prix : dans la mémoire du client, de la charge réseau ou les performances. Dans de nombreux cas, les options de curseur par défaut génèrent un curseur plus complexe que votre application a réellement besoin.  
  
 Votre choix du type de curseur dépend de la manière dont votre application utilise le jeu de résultats et également plusieurs considérations de conception, y compris la taille du jeu de résultats, le pourcentage des données susceptibles d’être utilisés, la sensibilité aux modifications de données et des performances des applications configuration requise.  
  
 Au minimum, le choix de curseur dépend de la nécessité de modifier ou simplement afficher les données :  
  
-   Si vous devez parcourir un jeu de résultats, mais pas les données modifiées, utilisez un [avant uniquement](../../../ado/guide/data/forward-only-cursors.md) ou [statique](../../../ado/guide/data/static-cursors.md) curseur.  
  
-   Si vous avez un grand jeu de résultats et la nécessité pour sélectionner uniquement quelques lignes, utilisez un [keyset](../../../ado/guide/data/keyset-cursors.md) curseur.  
  
-   Si vous souhaitez synchroniser un jeu de résultats avec récente ajoute, modifie et supprime tous les utilisateurs simultanés, utilisez un [dynamique](../../../ado/guide/data/dynamic-cursors.md) curseur.  
  
 Bien que chaque type de curseur semble différent, gardez à l’esprit que ces types de curseur ne sont pas beaucoup des variétés comme simplement le résultat des caractéristiques qui se chevauchent et les options.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Curseurs avant uniquement](../../../ado/guide/data/forward-only-cursors.md)  
  
-   [Curseurs statiques](../../../ado/guide/data/static-cursors.md)  
  
-   [Curseurs de jeu de clés](../../../ado/guide/data/keyset-cursors.md)  
  
-   [Curseurs dynamiques](../../../ado/guide/data/dynamic-cursors.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs avant uniquement](../../../ado/guide/data/forward-only-cursors.md)   
 [Curseurs statiques](../../../ado/guide/data/static-cursors.md)   
 [Curseurs keyset](../../../ado/guide/data/keyset-cursors.md)   
 [Curseurs dynamiques](../../../ado/guide/data/dynamic-cursors.md)
