---
title: Size, propriété (paramètre ADO) | Documents Microsoft
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
- _Parameter::Size
helpviewer_keywords:
- Size property [ADO Parameter]
ms.assetid: e6bad449-ebdb-4dd3-886a-9e6f1e7ee5d2
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: faa4e918d291f9d6dc095f05a40b6d71729bb5da
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="size-property-ado-parameter"></a>Propriété Size (paramètre ADO)
Indique la taille maximale, en octets ou de caractères, d’un [paramètre](../../../ado/reference/ado-api/parameter-object.md) objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **Long** valeur qui indique la taille maximale en octets ou de caractères d’une valeur dans un **paramètre** objet.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **taille** propriété pour déterminer la taille maximale des valeurs devant être écrites ou lues à partir la [valeur](../../../ado/reference/ado-api/value-property-ado.md) propriété d’un **paramètre** objet.  
  
 Si vous spécifiez un type de données de longueur variable pour un **paramètre** objet (par exemple, les **chaîne** type, tel que **adVarChar**), vous devez définir l’objet  **Taille** propriété avant de l’ajouter à la [paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md) collection ; sinon, une erreur se produit.  
  
 Si vous avez déjà ajouté le **paramètre** de l’objet à la **paramètres** collection d’un [commande](../../../ado/reference/ado-api/command-object-ado.md) objet et que vous modifiez son type pour un type de données de longueur variable, vous devez définir le **paramètre** l’objet **taille** propriété avant d’exécuter le **commande** l’objet ; sinon, une erreur se produit.  
  
 Si vous utilisez la [Actualiser](../../../ado/reference/ado-api/refresh-method-ado.md) méthode pour obtenir des informations sur les paramètres du fournisseur d’et il renvoie au moins un type de données de longueur variable **paramètre** ADO peut allouer de mémoire pour les paramètres en fonction des objets de leur taille maximale potentielle, ce qui peut provoquer une erreur lors de l’exécution. Pour éviter une erreur, vous devez définir explicitement la **taille** propriété de ces paramètres avant d’exécuter la commande.  
  
 Le **taille** propriété est en lecture/écriture.  
  
## <a name="applies-to"></a>S'applique à  
 [Parameter, objet](../../../ado/reference/ado-api/parameter-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, CommandText, CommandTimeout, CommandType, la taille et Direction, propriétés-exemple (VB)](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, la taille et Direction, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, la taille et Direction, propriétés-exemple (JScript)](../../../ado/reference/ado-api/activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Size, propriété (objet Stream ADO)](../../../ado/reference/ado-api/size-property-ado-stream.md)
