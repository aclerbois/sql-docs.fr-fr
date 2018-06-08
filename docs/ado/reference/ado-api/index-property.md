---
title: Propriété index | Documents Microsoft
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
- Recordset21::Index
helpviewer_keywords:
- Index property
ms.assetid: 1c79e271-21ec-41a8-8163-c5e89f0001a7
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 01e7883719900e85fdb7227f50c90ed265f73c30
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="index-property"></a>Propriété index
Indique le nom de l’index actuellement en vigueur pour un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **chaîne** valeur, qui est le nom de l’index.  
  
## <a name="remarks"></a>Notes  
 L’index nommé par le **Index** propriété doit avoir été préalablement déclarée sur la table de base sous-jacente la **Recordset** objet. Autrement dit, l’index doit être déclaré par programme comme une ADOX [Index](../../../ado/reference/adox-api/index-object-adox.md) objet, ou lorsque la table de base a été créée.  
  
 Une erreur d’exécution se produit si l’index ne peut pas être défini. Le **Index** propriété ne peut pas être définie dans les conditions suivantes :  
  
-   Dans un [WillChangeRecordset](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md) ou **RecordsetChangeComplete** Gestionnaire d’événements.  
  
-   Si le **Recordset** exécute encore une opération (qui peut être déterminée par le [état](../../../ado/reference/ado-api/state-property-ado.md) propriété).  
  
-   Si un filtre a été défini sur le **Recordset** avec la [filtre](../../../ado/reference/ado-api/filter-property.md) propriété.  
  
 Le **Index** propriété peut toujours être définie avec succès si la **Recordset** est fermée, mais la **Recordset** s’ouvre pas correctement, ou l’index ne sera pas utilisable, si le fournisseur sous-jacent ne prend pas en charge les index.  
  
 Si l’index peut être définie, la position de ligne actuelle peut changer. Cela entraîne une mise à jour la [AbsolutePosition](../../../ado/reference/ado-api/absoluteposition-property-ado.md) propriété et déclenchent la **WillChangeRecordset**, **RecordsetChangeComplete**, [WillMove ](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md), et [MoveComplete](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md) événements.  
  
 Si l’index peut être défini et le [LockType](../../../ado/reference/ado-api/locktype-property-ado.md) propriété **adLockPessimistic** ou **adLockOptimistic**, puis implicite [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) opération est effectuée. Cela libère les groupes en cours et affectées. Un filtre existant est libéré, et la position de ligne actuelle est modifiée pour la première ligne de réorganisés **Recordset**.  
  
 Le **Index** propriété est utilisée conjointement avec la [recherche](../../../ado/reference/ado-api/seek-method.md) (méthode). Si le fournisseur sous-jacent ne prend pas en charge la **Index** propriété et par conséquent la **recherche** méthode, envisagez d’utiliser le [trouver](../../../ado/reference/ado-api/find-method-ado.md) (méthode) à la place. Déterminer si le **Recordset** objet prend en charge les index avec la [prend en charge](../../../ado/reference/ado-api/supports-method.md)**(adIndex)** (méthode).  
  
 La fonction intégrée **Index** propriété n’est pas liée à la dynamique [optimiser](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md) propriété, bien que les deux portent sur les index.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche la méthode et les Index, propriété-Exemple (VB)](../../../ado/reference/ado-api/seek-method-and-index-property-example-vb.md)   
 [Objet index (ADOX)](../../../ado/reference/adox-api/index-object-adox.md)   
 [Seek, méthode](../../../ado/reference/ado-api/seek-method.md)
