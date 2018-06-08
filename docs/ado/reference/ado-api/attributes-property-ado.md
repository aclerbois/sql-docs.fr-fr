---
title: Attributs, propriété (ADO) | Documents Microsoft
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
- Connection15::Attributes
- Field20::Attributes
- _Parameter::Attributes
helpviewer_keywords:
- Attributes property [ADO]
ms.assetid: acc15d40-68a6-4ba9-85bd-12d331aecaa6
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fe7d17ab86f7ed60c863d0d3d0f2b32afd9972f3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="attributes-property-ado"></a>Attributs, propriété (ADO)
Indique une ou plusieurs caractéristiques d’un objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **Long** valeur.  
  
 Pour un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet, le **attributs** propriété est en lecture/écriture, et sa valeur peut être la somme d’une ou plusieurs [XactAttributeEnum](../../../ado/reference/ado-api/xactattributeenum.md) valeurs. La valeur par défaut est zéro (0).  
  
 Pour un [paramètre](../../../ado/reference/ado-api/parameter-object.md) objet, le **attributs** propriété est en lecture/écriture, et sa valeur peut être la somme d’une ou plusieurs [ParameterAttributesEnum](../../../ado/reference/ado-api/parameterattributesenum.md) valeurs. La valeur par défaut est **adParamSigned**.  
  
 Pour un [champ](../../../ado/reference/ado-api/field-object.md) objet, le **attributs** propriété peut être la somme d’une ou plusieurs [FieldAttributeEnum](../../../ado/reference/ado-api/fieldattributeenum.md) valeurs. Il est généralement en lecture seule. Toutefois, pour les nouveaux **champ** les objets qui ont été ajoutées à la [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection d’un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md), **attributs** est en lecture/écriture une fois la [valeur](../../../ado/reference/ado-api/value-property-ado.md) propriété pour le **champ** a été spécifié et la nouvelle **champ** a été ajoutée par le fournisseur de données en appelant le [ Mise à jour](../../../ado/reference/ado-api/update-method.md) méthode de la **champs** collection.  
  
 Pour un [propriété](../../../ado/reference/ado-api/property-object-ado.md) objet, le **attributs** propriété est en lecture seule et sa valeur peut être la somme d’une ou plusieurs [PropertyAttributesEnum](../../../ado/reference/ado-api/propertyattributesenum.md) valeurs.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **attributs** propriété pour définir ou retourner les caractéristiques de **connexion** objets, **paramètre** objets, **champ** objets, ou **Propriété** objets.  
  
 Lorsque vous définissez plusieurs attributs, vous pouvez additionner les constantes appropriées. Si vous définissez la valeur de propriété à une somme, y compris les constantes incompatibles, une erreur se produit.  
  
> [!NOTE]
>  **Utilisation du Service de données à distance** cette propriété n’est pas disponible sur un côté client **connexion** objet.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)|[Field, objet](../../../ado/reference/ado-api/field-object.md)|  
|[Parameter, objet](../../../ado/reference/ado-api/parameter-object.md)|[Property, objet (ADO)](../../../ado/reference/ado-api/property-object-ado.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs et propriétés nom-exemple (VB)](../../../ado/reference/ado-api/attributes-and-name-properties-example-vb.md)   
 [Attributs et propriétés nom-exemple (VC ++)](../../../ado/reference/ado-api/attributes-and-name-properties-example-vc.md)   
 [AppendChunk, méthode (ADO)](../../../ado/reference/ado-api/appendchunk-method-ado.md)   
 [BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO)](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md)   
 [GetChunk, méthode (ADO)](../../../ado/reference/ado-api/getchunk-method-ado.md)
