---
title: Propriété direction | Documents Microsoft
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
- _Parameter::Direction
helpviewer_keywords:
- Direction property
ms.assetid: d5732578-3434-4dcd-a9f7-db1abd1b3b94
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 75ec2f91ab940780bdd2755c2808b902f7a7d782
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="direction-property"></a>Propriété direction
Indique si le [paramètre](../../../ado/reference/ado-api/parameter-object.md) représente un paramètre d’entrée, un paramètre de sortie, une entrée et un paramètre de sortie, ou si le paramètre est la valeur de retour d’une procédure stockée.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un [ParameterDirectionEnum](../../../ado/reference/ado-api/parameterdirectionenum.md) valeur.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **Direction** propriété pour spécifier la manière dont un paramètre est passé vers ou à partir d’une procédure. Le **Direction** propriété est en lecture/écriture ; cela vous permet de travailler avec les fournisseurs qui ne retournent pas ces informations ou pour définir ces informations lorsque vous ne souhaitez pas ADO pour effectuer un appel supplémentaire au fournisseur pour extraire des informations de paramètre.  
  
 Tous les fournisseurs peuvent déterminer la direction des paramètres dans des procédures stockées. Dans ce cas, vous devez définir le **Direction** propriété avant d’exécuter la requête.  
  
## <a name="applies-to"></a>S'applique à  
 [Parameter, objet](../../../ado/reference/ado-api/parameter-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, CommandText, CommandTimeout, CommandType, la taille et Direction, propriétés-exemple (VB)](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, la taille et Direction, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, la taille et Direction, propriétés-exemple (JScript)](../../../ado/reference/ado-api/activeconnection-commandtext-timeout-type-size-example-jscript.md)
