---
title: "La propriété d’état (jeu d’enregistrements ADO) | Documents Microsoft"
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: ado
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: sql
ms.tgt_pltfrm: 
ms.topic: article
apitype: COM
f1_keywords:
- Recordset15::GetStatus
- Recordset15::Status
helpviewer_keywords:
- Status property [ADO Recordset]
ms.assetid: 41d70d89-880f-4850-9d17-19d9790cc8eb
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 1a8b65d43534fd75a9d6d8e659959100c5bea14f
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="status-property-ado-recordset"></a>Propriété d’état (jeu d’enregistrements ADO)
Indique l’état de l’enregistrement actif en ce qui concerne les mises à jour du lot ou d’autres opérations en bloc.  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne une somme d’une ou plusieurs [RecordStatusEnum](../../../ado/reference/ado-api/recordstatusenum.md) valeurs.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **état** propriété pour connaître les modifications en attente sur les enregistrements modifiés au cours de la mise à jour par lot. Vous pouvez également utiliser le **état** propriété pour afficher l’état des enregistrements qui échouent lors des opérations en bloc, telles que lorsque vous appelez le [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md), ou [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) méthodes sur un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) de l’objet, ou définir le [filtre](../../../ado/reference/ado-api/filter-property.md) propriété sur un **Recordset** objet en un tableau de signets. Avec cette propriété, vous pouvez déterminer comment un enregistrement donné a échoué et résoudre le problème.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété d’état (Recordset) (VB)](../../../ado/reference/ado-api/status-property-example-recordset-vb.md)   
 [Status, exemple de propriété (VC++)](../../../ado/reference/ado-api/status-property-example-vc.md)   
