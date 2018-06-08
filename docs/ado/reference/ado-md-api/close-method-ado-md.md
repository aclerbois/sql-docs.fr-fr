---
title: Close (méthode) (ADO MD) | Documents Microsoft
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
apitype: COM
f1_keywords:
- Close
- Cellset::Close
helpviewer_keywords:
- Close method [ADO MD]
ms.assetid: a3aa594d-f9d4-4654-8625-ec20153ff5d9
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 918cd96221d4d4e4c185bd1e09039069f9ce1128
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="close-method-ado-md"></a>Close (méthode) (ADO MD)
Ferme un ensemble de cellules ouvert.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Cellset.Close  
```  
  
## <a name="remarks"></a>Notes  
 À l’aide de la **fermer** méthode pour fermer une [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md) objet libère les données associées, y compris les données dans aucun liées [cellule](../../../ado/reference/ado-md-api/cell-object-ado-md.md), [axe](../../../ado/reference/ado-md-api/axis-object-ado-md.md), [Position](../../../ado/reference/ado-md-api/position-object-ado-md.md), ou [membre](../../../ado/reference/ado-md-api/member-object-ado-md.md) objets. Fermeture d’un **ensemble de cellules** ne le supprime pas de la mémoire ; vous pouvez modifier ses paramètres de propriété et ouvrez à nouveau ultérieurement. Pour éliminer définitivement un objet de la mémoire, affectez la variable objet **rien**.  
  
 Vous pouvez ensuite appeler la [ouvrir](../../../ado/reference/ado-md-api/open-method-ado-md.md) méthode pour rouvrir le **ensemble de cellules** à l’aide de la même ou à une autre chaîne source. Alors que le **ensemble de cellules** objet est fermé, la récupération de toutes les propriétés ou appeler une méthode qui font référence aux données sous-jacentes ou métadonnées génère une erreur.  
  
## <a name="applies-to"></a>S'applique à  
 [Cellset, objet (ADO MD)](../../../ado/reference/ado-md-api/cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Objet de l’axe (ADO MD)](../../../ado/reference/ado-md-api/axis-object-ado-md.md)   
 [Objet de cellule (ADO MD)](../../../ado/reference/ado-md-api/cell-object-ado-md.md)   
 [Objet de membre (ADO MD)](../../../ado/reference/ado-md-api/member-object-ado-md.md)   
 [Open (méthode) (ADO MD)](../../../ado/reference/ado-md-api/open-method-ado-md.md)   
 [Position, objet (ADO MD)](../../../ado/reference/ado-md-api/position-object-ado-md.md)   
 [State, propriété (ADO MD)](../../../ado/reference/ado-md-api/state-property-ado-md.md)
