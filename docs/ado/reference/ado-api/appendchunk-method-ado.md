---
title: AppendChunk, méthode (ADO) | Documents Microsoft
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
- _Parameter::AppendChunk
- Field20::AppendChunk
helpviewer_keywords:
- AppendChunk method [ADO]
ms.assetid: c648b5a8-d4f1-4d16-836e-3957feb03617
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3ba8020b9bf2666ba3b2ab0ffe9156c771360c95
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="appendchunk-method-ado"></a>AppendChunk, méthode (ADO)
Ajoute des données à un texte de grande taille ou les données binaires [champ](../../../ado/reference/ado-api/field-object.md), ou à un [paramètre](../../../ado/reference/ado-api/parameter-object.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.AppendChunk Data  
```  
  
#### <a name="parameters"></a>Paramètres  
 *objet*  
 A **champ** ou **paramètre** objet.  
  
 *Données*  
 A **Variant** qui contient les données à ajouter à l’objet.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **AppendChunk** méthode sur un **champ** ou **paramètre** objet à remplir avec des données binaires longues ou de caractères. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la **AppendChunk** méthode pour manipuler les valeurs longues dans les parties plutôt que dans leur intégralité.  
  
## <a name="field"></a>Champ  
 Si le **adFldLong** bit dans le [attributs](../../../ado/reference/ado-api/attributes-property-ado.md) propriété d’un **champ** objet a la valeur **true**, que vous pouvez utiliser la  **AppendChunk** méthode pour ce champ.  
  
 La première **AppendChunk** appeler sur un **champ** objet écrit des données dans le champ, remplaçant les données existantes. Ultérieures **AppendChunk** ajoutent des appels aux données existantes. Si vous ajoutez des données à un champ et puis de définir ou de lire la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé l’ajout de données dans le premier champ. Si vous appelez le **AppendChunk** méthode sur le premier champ, ADO interprète l’appel en tant que nouvelle **AppendChunk** opération et remplace les données existantes. L’accès à d’autres champs [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) les objets qui ne sont pas des clones du premier **Recordset** objet n’interrompra pas **AppendChunk** operations.  
  
 S’il n’existe aucun enregistrement actif lorsque vous appelez **AppendChunk** sur un **champ** de l’objet, une erreur se produit.  
  
> [!NOTE]
>  Le **AppendChunk** méthode ne fonctionne pas sur **champ** les objets d’un [enregistrement Object (ADO)](../../../ado/reference/ado-api/record-object-ado.md) objet. Il n’effectue aucune opération et génère une erreur d’exécution.  
  
## <a name="parameter"></a>Paramètre  
 Si le **adParamLong** bit dans le **attributs** propriété d’un **paramètre** objet a la valeur **true**, que vous pouvez utiliser la  **AppendChunk** méthode pour ce paramètre.  
  
 La première **AppendChunk** appeler sur un **paramètre** objet écrit des données dans ce paramètre en remplaçant les données existantes. Ultérieures **AppendChunk** appelle sur un **paramètre** objet Ajouter paramètre aux données existantes. Un **AppendChunk** appel qui passe une valeur null annule toutes les données de paramètre.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Field, objet](../../../ado/reference/ado-api/field-object.md)|[Parameter, objet](../../../ado/reference/ado-api/parameter-object.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [AppendChunk et GetChunk, méthodes-exemple (VB)](../../../ado/reference/ado-api/appendchunk-and-getchunk-methods-example-vb.md)   
 [AppendChunk et GetChunk, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/appendchunk-and-getchunk-methods-example-vc.md)   
 [Attributs, propriété (ADO)](../../../ado/reference/ado-api/attributes-property-ado.md)   
 [GetChunk, méthode (ADO)](../../../ado/reference/ado-api/getchunk-method-ado.md)
