---
title: MarshalOptions, propriété (ADO) | Documents Microsoft
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
- Recordset15::MarshalOptions
helpviewer_keywords:
- MarshalOptions property [ADO]
ms.assetid: 390c8abf-133e-40da-8b99-8f748a983e4f
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ca3cb11f425b94145b89638012844d64265bd6b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="marshaloptions-property-ado"></a>MarshalOptions, propriété (ADO)
Indique les enregistrements de la [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) doivent être marshalées vers le serveur.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un [MarshalOptions](../../../ado/reference/ado-api/marshaloptionsenum.md) valeur. La valeur par défaut est **adMarshalAll**.  
  
## <a name="remarks"></a>Notes  
 Lors de l’utilisation d’un côté client [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md), les enregistrements qui ont été modifiés sur le client mis à jour dans la couche intermédiaire ou un serveur Web via une technique appelée marshaling, le processus d’empaquetage et d’envoi de méthode d’interface paramètres au-delà des limites de thread ou processus. Définition de la **MarshalOptions** propriété peut améliorer les performances lors du marshaling des données distantes modifiées pour la mise à jour à la couche intermédiaire ou un serveur Web.  
  
> [!NOTE]
>  **Utilisation du Service de données à distance** cette propriété est utilisée uniquement sur un côté client **Recordset**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété MarshalOptions (VB)](../../../ado/reference/ado-api/marshaloptions-property-example-vb.md)   
 [MarshalOptions, exemple de propriété (VC++)](../../../ado/reference/ado-api/marshaloptions-property-example-vc.md)   
