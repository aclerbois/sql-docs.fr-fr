---
title: CancelBatch, méthode (ADO) | Documents Microsoft
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
- Recordset15::raw_CancelBatch
- Recordset15::CancelBatch
helpviewer_keywords:
- CancelBatch method [ADO]
ms.assetid: dbdc2574-e44e-4d95-b03d-4a5d9e9adf3c
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 241bf36ab3ee4babf8d4e306b9d27a350985cb20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cancelbatch-method-ado"></a>CancelBatch, méthode (ADO)
Annule une mise à jour par lot en attente.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.CancelBatchAffectRecords  
```  
  
#### <a name="parameters"></a>Paramètres  
 *AffectRecords*  
 Ce paramètre est facultatif. Un [AffectEnum](../../../ado/reference/ado-api/affectenum.md) valeur qui indique le nombre d’enregistrements le **CancelBatch** méthode affectera.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **CancelBatch** méthode pour annuler toutes les mises à jour en attente dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) en mode de mise à jour par lot. Si le **Recordset** est en mode de mise à jour immédiate, l’appel **CancelBatch** sans **adAffectCurrent** génère une erreur.  
  
 Si vous modifiez l’enregistrement actif ou que vous ajoutez un nouvel enregistrement lorsque vous appelez **CancelBatch**, ADO appelle d’abord la [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) mis en cache de méthode pour annuler les modifications. Après cela, toutes les modifications en attente dans le **Recordset** sont annulées.  
  
 L’enregistrement actif peut être déterminé après une **CancelBatch** appeler, notamment si vous étiez en train d’ajouter un nouvel enregistrement. Pour cette raison, il est conseillé de définir la position actuelle dans un emplacement connu dans le **Recordset** après le **CancelBatch** appeler. Par exemple, appelez le [MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md) (méthode).  
  
 Si la tentative d’annuler les mises à jour en attente échoue en raison d’un conflit avec les données sous-jacentes (par exemple, si un enregistrement a été supprimé par un autre utilisateur), le fournisseur retourne des avertissements dans le [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection mais n’interrompt pas exécution du programme. Une erreur d’exécution se produit uniquement en cas de conflit sur tous les enregistrements demandés. Utilisez le [filtre](../../../ado/reference/ado-api/filter-property.md) propriété (**adFilterAffectedRecords**) et le [état](../../../ado/reference/ado-api/status-property-ado-recordset.md) propriété pour localiser les enregistrements en conflit.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [UpdateBatch et CancelBatch, méthodes-exemple (VB)](../../../ado/reference/ado-api/updatebatch-and-cancelbatch-methods-example-vb.md)   
 [UpdateBatch et CancelBatch, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/updatebatch-and-cancelbatch-methods-example-vc.md)   
 [Cancel (méthode) (ADO)](../../../ado/reference/ado-api/cancel-method-ado.md)   
 [Cancel (méthode) (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)   
 [CancelUpdate, méthode (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [CancelUpdate, méthode (RDS)](../../../ado/reference/rds-api/cancelupdate-method-rds.md)   
 [Clear (méthode) (ADO)](../../../ado/reference/ado-api/clear-method-ado.md)   
 [LockType, propriété (ADO)](../../../ado/reference/ado-api/locktype-property-ado.md)   
 [UpdateBatch, méthode](../../../ado/reference/ado-api/updatebatch-method.md)
