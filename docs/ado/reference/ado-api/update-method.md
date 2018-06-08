---
title: La méthode Update | Documents Microsoft
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
- Recordset15::Update
helpviewer_keywords:
- Update method [ADO]
ms.assetid: 6b2a9c31-1a7e-40db-8a53-30720d0f6cc1
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2c0a5cd4e12848ecdec7c3c5168b6106dd95215c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="update-method"></a>Update (méthode)
Enregistre les modifications apportées à la ligne actuelle d’un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet, ou le [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection d’un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Update Fields, Values  
record.Fields.Update  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Fields*  
 Ce paramètre est facultatif. A **Variant** qui représente un nom unique, ou un **Variant** tableau qui représente les noms ou les positions ordinales des champs que vous souhaitez modifier.  
  
 *Valeurs*  
 Ce paramètre est facultatif. A **Variant** qui représente une valeur unique, ou un **Variant** tableau qui représente les valeurs pour les champs dans le nouvel enregistrement.  
  
## <a name="remarks"></a>Notes  
  
## <a name="recordset"></a>Ensemble d'enregistrements  
 Utilisez le **mise à jour** méthode pour enregistrer les modifications apportées à l’enregistrement en cours d’un **Recordset** objet depuis l’appel le [AddNew](../../../ado/reference/ado-api/addnew-method-ado.md) (méthode) ou parce que la modification des valeurs des champs d’un enregistrement existant. Le **Recordset** objet doit prendre en charge les mises à jour.  
  
 Pour définir les valeurs de champ, effectuez l’une des opérations suivantes :  
  
-   Affecter des valeurs à un [champ](../../../ado/reference/ado-api/field-object.md) l’objet [valeur](../../../ado/reference/ado-api/value-property-ado.md) propriété et appelez le **mise à jour** (méthode).  
  
-   Passez un nom de champ et une valeur comme arguments avec le **mise à jour** appeler.  
  
-   Passer un tableau de noms de champs et un tableau de valeurs avec le **mise à jour** appeler.  
  
 Lorsque vous utilisez des tableaux de champs et des valeurs, il doit être un nombre égal d’éléments dans les deux tableaux. En outre, l’ordre des noms de champs doit correspondre à l’ordre des valeurs de champ. Si le nombre et l’ordre des champs et les valeurs ne correspondent pas, une erreur se produit.  
  
 Si le **Recordset** objet prend en charge la mise à jour par lot, vous pouvez mettre en cache plusieurs modifications apportées à un ou plusieurs enregistrements localement jusqu'à ce que vous appeliez la [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) (méthode). Si vous modifiez l’enregistrement actif ou ajoutez un nouvel enregistrement lorsque vous appelez le **UpdateBatch** (méthode), ADO appelle automatiquement la **mise à jour** méthode pour enregistrer les modifications en attente à l’enregistrement actif avant transmettre les modifications par lot au fournisseur.  
  
 Si vous passez de l’enregistrement vous ajoutez ou modifiez avant d’appeler le **mise à jour** (méthode), ADO appelle automatiquement **mise à jour** pour enregistrer les modifications. Vous devez appeler la [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) méthode si vous souhaitez annuler les modifications apportées à l’enregistrement actif ou supprimer un enregistrement qui vient d’être ajouté.  
  
 L’enregistrement actif reste actif après avoir appelé la **mise à jour** (méthode).  
  
## <a name="record"></a>Record  
 Le **mise à jour** méthode finalise les ajouts, suppressions et mises à jour les champs dans le [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection d’un **enregistrement** objet.  
  
 Par exemple, les champs supprimés avec le **supprimer** méthode sont marquées pour suppression immédiatement, mais restent dans la collection. Le **mise à jour** méthode doit être appelée pour supprimer réellement ces champs à partir de la collection du fournisseur.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)|[Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Mise à jour et CancelUpdate, méthodes-exemple (VB)](../../../ado/reference/ado-api/update-and-cancelupdate-methods-example-vb.md)   
 [Mise à jour et CancelUpdate, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/update-and-cancelupdate-methods-example-vc.md)   
 [AddNew (méthode) (ADO)](../../../ado/reference/ado-api/addnew-method-ado.md)   
 [CancelUpdate, méthode (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [EditMode, propriété](../../../ado/reference/ado-api/editmode-property.md)   
 [UpdateBatch, méthode](../../../ado/reference/ado-api/updatebatch-method.md)
