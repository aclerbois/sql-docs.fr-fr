---
title: ActualSize, propriété (ADO) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Field20::ActualSize
helpviewer_keywords:
- ActualSize property [ADO]
ms.assetid: 722803d0-cef5-4d4c-b79d-3f2f58052229
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 697202ef2c88d5fa33657c74713d884f2cbe916c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="actualsize-property-ado"></a>ActualSize, propriété (ADO)
Indique la longueur réelle de la valeur d’un champ en octets.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Retourne un **Long** valeur.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **ActualSize** propriété pour retourner la longueur réelle d’un [champ](../../../ado/reference/ado-api/field-object.md) valeur de l’objet. Pour tous les champs, les **ActualSize** propriété est en lecture seule. Si ADO ne peut pas déterminer la longueur de la **champ** valeur de l’objet, le **ActualSize** propriété renvoie **adUnknown**.  
  
 Le **ActualSize** et [DefinedSize](../../../ado/reference/ado-api/definedsize-property.md) propriétés sont différentes, comme indiqué dans l’exemple suivant. A **champ** objet avec un type déclaré de **adVarChar** et une longueur maximale de 50 caractères retourne un **DefinedSize** valeur de la propriété de 50, mais la  **ActualSize** valeur de propriété retournée est la longueur des données stockées dans le champ de l’enregistrement actif. **Champs** avec un **DefinedSize** supérieure à 255 octets sont traités comme des colonnes de longueur variable.  
  
## <a name="applies-to"></a>S'applique à  
 [Field, objet](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActualSize et DefinedSize, propriétés-exemple (VB)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vb.md)   
 [ActualSize et DefinedSize, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vc.md)   
 [DefinedSize, propriété](../../../ado/reference/ado-api/definedsize-property.md)
