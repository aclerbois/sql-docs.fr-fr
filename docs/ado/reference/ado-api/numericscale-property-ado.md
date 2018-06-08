---
title: NumericScale, propriété (ADO) | Documents Microsoft
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
- _Parameter::NumericScale
- Field20::NumericScale
helpviewer_keywords:
- NumericScale property [ADO]
ms.assetid: 29a02992-64be-4fcd-be13-445cba205893
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 575618ad1760cdca6c32e1b8e2797070583506de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="numericscale-property-ado"></a>NumericScale, propriété (ADO)
Indique l’échelle de valeurs numériques dans un [paramètre](../../../ado/reference/ado-api/parameter-object.md) ou [champ](../../../ado/reference/ado-api/field-object.md) objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **octets** valeur qui indique le nombre de décimales appliqué à des valeurs numériques qui sera résolu.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **NumericScale** propriété pour déterminer le nombre de chiffres à droite de la virgule décimale sera utilisé pour représenter des valeurs pour un type numérique **paramètre** ou **champ** objet.  
  
 Pour **paramètre** objets, la **NumericScale** propriété est en lecture/écriture.  
  
 Pour un **champ**objet, **NumericScale** est généralement en lecture seule. Toutefois, pour les nouveaux **champ** les objets qui ont été ajoutées à la [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection d’un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md), **NumericScale** est en lecture/écriture uniquement après la [valeur](../../../ado/reference/ado-api/value-property-ado.md) propriété pour le **champ** a été spécifié et le fournisseur de données a ajouté le nouveau **champ** en appelant le [ Mise à jour](../../../ado/reference/ado-api/update-method.md) méthode de la [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Parameter, objet](../../../ado/reference/ado-api/parameter-object.md)|[Field, objet](../../../ado/reference/ado-api/field-object.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [NumericScale et Precision, propriétés, exemple (VB)](../../../ado/reference/ado-api/numericscale-and-precision-properties-example-vb.md)   
 [NumericScale et Precision, propriétés, exemple (VC ++)](../../../ado/reference/ado-api/numericscale-and-precision-properties-example-vc.md)   
 [Precision, propriété (ADO)](../../../ado/reference/ado-api/precision-property-ado.md)
