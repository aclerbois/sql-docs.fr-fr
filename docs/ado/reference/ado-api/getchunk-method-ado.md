---
title: GetChunk, méthode (ADO) | Documents Microsoft
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
- Field20::raw_GetChunk
- Field20::GetChunk
helpviewer_keywords:
- GetChunk method [ADO]
ms.assetid: fc268e22-205b-44a3-9038-ffed51e23e10
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 38397b5573cfb7ddb10a7454833f4e9b512b769b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getchunk-method-ado"></a>GetChunk, méthode (ADO)
Retourne l’ensemble ou une partie du contenu d’un texte de grande taille ou les données binaires [champ](../../../ado/reference/ado-api/field-object.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
variable = field.GetChunk(Size)  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un **variante**.  
  
#### <a name="parameters"></a>Paramètres  
 *Taille*  
 A **Long** expression est égale au nombre d’octets ou de caractères que vous souhaitez récupérer.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **GetChunk** méthode sur un **champ** objet pour extraire tout ou partie de ses données binaires longues ou de caractères. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la **GetChunk** méthode pour manipuler les valeurs longues dans des parties, plutôt que dans leur intégralité.  
  
 Les données qui un **GetChunk** appel retourne est affectée à *variable*. Si *taille* est supérieur aux données restantes, la **GetChunk** méthode retourne uniquement les données restantes sans remplissage *variable* avec des espaces vides. Si le champ est vide, le **GetChunk** méthode retourne une valeur null.  
  
 Chaque **GetChunk** appel récupère les données en commençant à partir de laquelle la précédente **GetChunk** appel s’est arrêté. Toutefois, si vous récupérez des données à partir d’un champ et que vous définissez ou de lire la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé la récupération de données à partir du premier champ. Si vous appelez le **GetChunk** méthode sur le premier champ, ADO interprète l’appel en tant que nouvelle **GetChunk** opération et démarre la lecture à partir du début des données. L’accès à d’autres champs [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) les objets qui ne sont pas des clones du premier **Recordset** objet n’interrompra pas **GetChunk** operations.  
  
 Si le **adFldLong** bit dans le [attributs](../../../ado/reference/ado-api/attributes-property-ado.md) propriété d’un **champ** objet a la valeur **True**, que vous pouvez utiliser la **GetChunk**  (méthode) pour ce champ.  
  
 S’il n’existe aucun enregistrement actif lorsque vous utilisez la **GetChunk** méthode sur un **champ** de l’objet, l’erreur 3021 (aucun enregistrement actif) survient.  
  
> [!NOTE]
>  Le **GetChunk** méthode ne fonctionne pas sur **champ** les objets d’un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) objet. Il n’effectue aucune opération et génère une erreur d’exécution.  
  
## <a name="applies-to"></a>S'applique à  
 [Field, objet](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AppendChunk et GetChunk, méthodes-exemple (VB)](../../../ado/reference/ado-api/appendchunk-and-getchunk-methods-example-vb.md)   
 [AppendChunk et GetChunk, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/appendchunk-and-getchunk-methods-example-vc.md)   
 [AppendChunk, méthode (ADO)](../../../ado/reference/ado-api/appendchunk-method-ado.md)   
 [Attributes, propriété (ADO)](../../../ado/reference/ado-api/attributes-property-ado.md)
