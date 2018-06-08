---
title: ConnectModeEnum | Documents Microsoft
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
- ConnectModeEnum
helpviewer_keywords:
- ConnectModeEnum enumeration [ADO]
ms.assetid: 3792c294-5161-4538-a908-22a5fc50b85f
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8f35019573f17e49fea3bdec7bfe6afc17b844f7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connectmodeenum"></a>ConnectModeEnum
Spécifie les autorisations disponibles pour la modification des données dans un [connexion](../../../ado/reference/ado-api/connection-object-ado.md), ouvrez un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md), ou spécifier des valeurs pour le [Mode](../../../ado/reference/ado-api/mode-property-ado.md) propriété de la  **Enregistrement** et [flux](../../../ado/reference/ado-api/stream-object-ado.md) objets.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adModeRead**|1|Indique les autorisations en lecture seule.|  
|**adModeReadWrite**|3|Indique les autorisations de lecture/écriture.|  
|**adModeRecursive**|0x400000|Utilisé conjointement avec les autres *\*ShareDeny\** valeurs (**adModeShareDenyNone**, **adModeShareDenyWrite**, ou **adModeShareDenyRead**) pour propager les restrictions de partage à tous les sous-enregistrements actuelles **enregistrement**. Il n’a aucun effet si la **enregistrement** n’a pas d’enfants. Une erreur d’exécution est générée s’il est utilisé avec **adModeShareDenyNone** uniquement. Toutefois, il peut être utilisé avec **adModeShareDenyNone** lorsqu’elles sont combinées avec d’autres valeurs. Par exemple, vous pouvez utiliser «**adModeRead** ou **adModeShareDenyNone** ou **encore adModeRecursive**».|  
|**adModeShareDenyNone**|16|Permet à d’autres personnes d’ouvrir une connexion avec des autorisations. Aucun accès en lecture ou en écriture ne peut être refusé à d'autres personnes.|  
|**adModeShareDenyRead**|4|Empêche d’autres personnes d’ouvrir une connexion avec les autorisations de lecture.|  
|**adModeShareDenyWrite**|8|Empêche d’autres personnes d’ouvrir une connexion avec des autorisations d’écriture.|  
|**adModeShareExclusive**|12|Empêche d’autres personnes d’ouvrir une connexion.|  
|**adModeUnknown**|0|Valeur par défaut. Indique que les autorisations n’ont pas encore été définies ou qu’il ne peut pas être déterminées.|  
|**adModeWrite**|2|Indique les autorisations en écriture seule.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.ConnectMode.READ|  
|AdoEnums.ConnectMode.READWRITE|  
|(Il n’existe aucun équivalent de pour AdoEnums.ConnectMode.RECURSIVE)|  
|AdoEnums.ConnectMode.SHAREDENYNONE|  
|AdoEnums.ConnectMode.SHAREDENYREAD|  
|AdoEnums.ConnectMode.SHAREDENYWRITE|  
|AdoEnums.ConnectMode.SHAREEXCLUSIVE|  
|AdoEnums.ConnectMode.UNKNOWN|  
|AdoEnums.ConnectMode.WRITE|  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Mode, propriété (ADO)](../../../ado/reference/ado-api/mode-property-ado.md)|[Open, méthode (objet Record ADO)](../../../ado/reference/ado-api/open-method-ado-record.md)|  
|[Open, méthode (objet Stream ADO)](../../../ado/reference/ado-api/open-method-ado-stream.md)|[Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)|
