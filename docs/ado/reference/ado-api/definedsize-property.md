---
title: Propriété DefinedSize | Documents Microsoft
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
- Field20::DefinedSize
helpviewer_keywords:
- DefinedSize property [ADO]
ms.assetid: 3ee27314-a305-4fbc-8433-9ee9a909afd6
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b84eb083ba442fc214d63b518c8bab0c001aa229
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="definedsize-property"></a>Propriété DefinedSize
Indique la capacité de données d’une [champ](../../../ado/reference/ado-api/field-object.md) objet.  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un **Long** valeur qui reflète la taille définie pour un champ, qui varie selon le type de données de l’objet de champ, consultez [Type](../../../ado/reference/ado-api/type-property-ado.md) pour plus d’informations. Pour un champ qui utilise un type de données de longueur fixe, la valeur de retour est la taille du type de données en octets. Pour un champ qui utilise un type de données de longueur variable, cela est une des opérations suivantes :  
  
1.  La longueur maximale du champ en caractères (pour **adVarChar** et **adVarWChar**) ou en octets (pour **adVarBinary**, et **adVarNumeric**) si le champ a une longueur définie. Par exemple, **adVarChar(5)** champ a une longueur maximale de 5.  
  
2.  La longueur maximale du type de données en caractères (pour **adChar** et **adWChar**) ou en octets (pour **adBinary** et **adNumeric**) si le champ n’a pas une longueur définie.  
  
3.  ~ 0 (au niveau du bit, la valeur n’est pas 0 ; tous les bits sont définis sur 1) si le champ, ni le type de données a une longueur maximale définie.  
  
4.  Pour les types de données qui n’ont pas une longueur, il est défini sur ~ 0 (au niveau du bit, la valeur n’est pas 0 ; tous les bits sont définis sur 1).  
  
## <a name="remarks"></a>Notes  
 Utilisez le **DefinedSize** propriété pour déterminer la capacité de données d’une **champ** objet.  
  
 Le **DefinedSize** et [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) propriétés sont différentes. Par exemple, considérez un **champ** objet avec un type déclaré de **adVarChar** et un **DefinedSize** valeur de la propriété de 50, contenant un seul caractère. Le **ActualSize** valeur de propriété retournée est la longueur en octets du caractère unique.  
  
## <a name="applies-to"></a>S'applique à  
 [Field, objet](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActualSize et DefinedSize, propriétés-exemple (VB)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vb.md)   
 [ActualSize et DefinedSize, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vc.md)   
 [ActualSize, propriété (ADO)](../../../ado/reference/ado-api/actualsize-property-ado.md)
