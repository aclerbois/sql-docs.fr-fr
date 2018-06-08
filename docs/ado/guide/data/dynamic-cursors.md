---
title: Les curseurs dynamiques | Documents Microsoft
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
- cursors [ADO], dynamic
- dynamic cursors [ADO]
ms.assetid: 00460f30-8cf7-494e-82df-41012f40ae51
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a5a7ee6573645ed4f65e087fa35283f352e6bdce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dynamic-cursors"></a>Curseurs dynamiques
Les curseurs dynamiques détectent toutes les modifications apportées aux lignes du jeu de résultats, que les modifications se produisent à partir de l’intérieur du curseur ou par d’autres utilisateurs en dehors du curseur. Tous les insert, update et les instructions delete effectuées par tous les utilisateurs sont visibles à travers le curseur. Le curseur dynamique peut détecter les modifications apportées aux lignes, ordre et les valeurs dans le jeu de résultats après l’ouverture du curseur. Mises à jour effectuées en dehors du curseur ne sont pas visibles jusqu'à ce qu’elles sont validées (sauf si le niveau d’isolation de transaction de curseur a la valeur « non validée »).  
  
 Par exemple, un curseur dynamique extrait deux lignes et une autre application, puis met à jour l’une de ces lignes et supprime l’autre. Si le curseur dynamique extrait ces lignes, elle ne trouvera pas la ligne supprimée, mais il affiche les nouvelles valeurs de la ligne mise à jour.  
  
 Le curseur dynamique est un bon choix si votre application doit détecter toutes les mises à jour simultanées apportées par d’autres utilisateurs. Utilisez le **adOpenDynamic CursorTypeEnum** pour indiquer que vous souhaitez utiliser un curseur dynamique dans ADO.  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs avant uniquement](../../../ado/guide/data/forward-only-cursors.md)   
 [Curseurs statiques](../../../ado/guide/data/static-cursors.md)   
 [Curseurs de jeu de clés](../../../ado/guide/data/keyset-cursors.md)
