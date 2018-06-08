---
title: Les curseurs avant uniquement | Documents Microsoft
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
- cursors [ADO], forward-only
- forward-only cursors [ADO]
ms.assetid: 2b1e062f-3294-4a6f-8241-a17045c4df18
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ca588a5c7efa6f5fe7dc861e292cffa7d15bb1be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="forward-only-cursors"></a>Curseurs avant uniquement
Le type de curseur par défaut, appelé un curseur avant uniquement (ou non déroulante), peut avancer uniquement dans le jeu de résultats. Un curseur avant uniquement ne prend pas en charge le défilement (la possibilité de déplacer vers l’avant et vers l’arrière dans le jeu de résultats) ; Il prend uniquement en charge l’extraction de lignes à partir du début à la fin du jeu de résultats. Avec certains curseurs avant uniquement (comme avec la bibliothèque de curseurs SQL Server), toutes les instructions delete, update et insert effectuées par l’utilisateur actuel (ou validées par d’autres utilisateurs) qu’incidence sur les lignes du jeu de résultats sont visibles que les lignes sont extraites. Étant donné que le curseur ne peut pas être le défilement arrière, toutefois, les modifications apportées aux lignes de la base de données après l’extraction de la ligne ne sont pas visibles via le curseur.  
  
 Une fois les données de la ligne actuelle sont traitées, le curseur avant uniquement libère les ressources qui ont été utilisés pour contenir ces données. Les curseurs avant uniquement sont dynamiques par défaut, c'est-à-dire que toutes les modifications sont détectées lors du traitement de la ligne actuelle. Cela permet d’ouverture de curseur plus rapide et le jeu de résultats pour afficher les mises à jour apportées aux tables sous-jacentes.  
  
 Alors que les curseurs avant uniquement ne prennent pas en charge le défilement vers l’arrière, votre application peut retourner au début du jeu de résultats en fermant et en rouvrant le curseur. Il s’agit d’un moyen efficace pour les petites quantités de données. En guise d’alternative, votre application peut lire le jeu de résultats une fois, mettre en cache les données localement, puis parcourir le cache de données local.  
  
 Si votre application ne nécessite pas de faire défiler le jeu de résultats, le curseur avant uniquement est la meilleure façon de récupérer des données rapidement avec moins de surcharge. Utilisez le **adOpenForwardOnly CursorTypeEnum** pour indiquer que vous souhaitez utiliser un curseur avant uniquement dans ADO.  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs statiques](../../../ado/guide/data/static-cursors.md)   
 [Curseurs keyset](../../../ado/guide/data/keyset-cursors.md)   
 [Curseurs dynamiques](../../../ado/guide/data/dynamic-cursors.md)
