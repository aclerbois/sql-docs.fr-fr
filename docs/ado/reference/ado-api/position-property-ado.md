---
title: Position, propriété (ADO) | Documents Microsoft
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
- _Stream::Position
helpviewer_keywords:
- Position property [ADO]
ms.assetid: daa8319a-49aa-4c1c-9af6-0b01e9ab2f9d
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c931d220a9418a790ff79d6b61c89004800a0061
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="position-property-ado"></a>Position, propriété (ADO)
Indique la position actuelle dans un [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **Long** valeur qui spécifie le décalage, en octets, de la position actuelle à partir du début du flux de données. La valeur par défaut est 0, qui représente le premier octet dans le flux de données.  
  
## <a name="remarks"></a>Notes  
 La position actuelle peut être déplacée vers un point après la fin du flux de données. Si vous spécifiez la position actuelle au-delà de la fin du flux, le [taille](../../../ado/reference/ado-api/size-property-ado-stream.md) de la **flux** objet augmente en conséquence. Les nouveaux octets ajoutés de cette façon est null.  
  
> [!NOTE]
>  **Position** mesure toujours des octets. Pour les flux de texte à l’aide de jeux de caractères multioctets, multipliez la position par la taille de caractères pour déterminer le nombre de caractères. Par exemple, pour un jeu de caractères codés sur deux, le premier caractère est à la position 0, le deuxième caractère à la position 2, le troisième caractère à la position 4 et ainsi de suite.  
  
> [!NOTE]
>  Les valeurs négatives ne peut pas être utilisés pour modifier la position actuelle dans un **flux**. Seuls les nombres positifs peuvent être utilisés pour **Position**.  
  
> [!NOTE]
>  Pour en lecture seule **flux** ADO ne renvoie pas une erreur si des objets **Position** est défini sur une valeur supérieure à la **taille** de la **flux**. Cela ne modifie pas la taille de la **flux**, ou modifiez la **flux** contenu. Toutefois, cela doit être évité, car il en résulte un sans signification **Position**valeur.  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Charset, propriété (ADO)](../../../ado/reference/ado-api/charset-property-ado.md)
