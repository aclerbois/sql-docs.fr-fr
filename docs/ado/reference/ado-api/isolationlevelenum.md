---
title: IsolationLevelEnum | Documents Microsoft
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
- IsolationLevelEnum
helpviewer_keywords:
- IsolationLevelEnum enumeration [ADO]
ms.assetid: 8e17a7bc-b8a3-4ae2-b6c9-ce088ad31fdf
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a22b958066b681f70187dc5e49fcd36752f510b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="isolationlevelenum"></a>IsolationLevelEnum
Spécifie le niveau d’isolation des transactions pour un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**: adXactUnspecified**|-1|Indique que le fournisseur utilise un niveau d’isolation que le nombre spécifié, mais que le niveau ne peut pas être déterminé.|  
|**adXactChaos**|16|Indique que les modifications en attente de transactions mieux isolées ne peut pas être remplacée.|  
|**adXactBrowse**|256|Indique qu’à partir d’une transaction vous pouvez visualiser les modifications non validées dans d’autres transactions.|  
|**adXactReadUncommitted**|256|Identique à **à adXactBrowse**.|  
|**adXactCursorStability**|4096|Indique que d’une transaction vous pouvez visualiser les modifications dans d’autres transactions uniquement une fois qu’ils ont été validées.|  
|**adXactReadCommitted**|4096|Identique à **à adXactCursorStability**.|  
|**adXactRepeatableRead**|65536|Indique qu’à partir d’une transaction vous ne pouvez pas voir les modifications apportées dans d’autres transactions, mais qu’une nouvelle requête peut extraire de nouveau **Recordset** objets.|  
|**adXactIsolated**|1048576|Indique que les transactions sont effectuées de manière isolée des autres transactions.|  
|**adXactSerializable**|1048576|Identique à **à adXactIsolated**.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.IsolationLevel.UNSPECIFIED|  
|AdoEnums.IsolationLevel.CHAOS|  
|AdoEnums.IsolationLevel.BROWSE|  
|AdoEnums.IsolationLevel.READUNCOMMITTED|  
|AdoEnums.IsolationLevel.CURSORSTABILITY|  
|AdoEnums.IsolationLevel.READCOMMITTED|  
|AdoEnums.IsolationLevel.REPEATABLEREAD|  
|AdoEnums.IsolationLevel.ISOLATED|  
|AdoEnums.IsolationLevel.SERIALIZABLE|  
  
## <a name="applies-to"></a>S'applique à  
 [IsolationLevel, propriété](../../../ado/reference/ado-api/isolationlevel-property.md)
