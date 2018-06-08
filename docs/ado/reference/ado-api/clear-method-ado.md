---
title: Clear (méthode) (ADO) | Documents Microsoft
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
- Errors::raw_Clear
- Errors::Clear
helpviewer_keywords:
- Clear method [ADO]
ms.assetid: 0a61ba7a-20b8-426a-91a0-9040e7c5a98a
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 238a936bb0bf0abe518b8a6268ace23406aac114
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clear-method-ado"></a>Clear (méthode) (ADO)
Supprime tous les [erreur](../../../ado/reference/ado-api/error-object.md) des objets de la [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Errors.Clear  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez le **clair** méthode sur le [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection auquel supprimer toutes les existantes [erreur](../../../ado/reference/ado-api/error-object.md) les objets de la collection. Lorsqu’une erreur se produit, ADO efface automatiquement le **erreurs** collection et la remplit avec **erreur** objets basés sur la nouvelle erreur.  
  
 Certaines propriétés et méthodes retournent des avertissements qui apparaissent sous la forme **erreur** des objets dans le **erreurs** collection mais qui n’empêchent pas l’exécution d’un programme. Avant d’appeler le [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md), ou [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) méthodes sur un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) de l’objet ; la [ouvrir](../../../ado/reference/ado-api/open-method-ado-connection.md) méthode sur un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) ; l’objet ou de définir la [filtre](../../../ado/reference/ado-api/filter-property.md) propriété sur un **Recordset** de l’objet, appelez le **effacer**méthode sur le **erreurs** collection. Ainsi, vous pouvez lire la [nombre](../../../ado/reference/ado-api/count-property-ado.md) propriété de la **erreurs** collection pour tester les avertissements retournés.  
  
## <a name="applies-to"></a>S'applique à  
 [Errors, collection (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Execute, Requery et Clear, méthodes-exemple (VB)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vb.md)   
 [Execute, Requery et Clear, méthodes-exemple (VBScript)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vbscript.md)   
 [Execute, Requery et Clear, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vc.md)   
 [CancelBatch, méthode (ADO)](../../../ado/reference/ado-api/cancelbatch-method-ado.md)   
 [DELETE, méthode (Collection de champs ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [DELETE, méthode (Collection de paramètres ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [DELETE, méthode (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [Filter, propriété](../../../ado/reference/ado-api/filter-property.md)   
 [Resync, méthode](../../../ado/reference/ado-api/resync-method.md)   
 [UpdateBatch, méthode](../../../ado/reference/ado-api/updatebatch-method.md)
