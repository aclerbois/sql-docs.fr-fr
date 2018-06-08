---
title: StreamReadEnum | Documents Microsoft
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
- StreamReadEnum
helpviewer_keywords:
- StreamReadEnum enumeration [ADO]
ms.assetid: cfa1b416-003a-436f-a21b-bd2397e54db3
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 99a38cd3fb2fd58c021113fa99f5b3b52fbb865a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="streamreadenum"></a>StreamReadEnum
Spécifie si l’ensemble du flux ou la ligne suivante doit être lue à partir d’un [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adReadAll**|-1|Valeur par défaut. Lit tous les octets dans le flux, à partir de la position actuelle et les versions ultérieures du [fin du support](../../../ado/reference/ado-api/eos-property.md) marqueur. Cela est uniquement valide **StreamReadEnum** valeur avec des flux binaires ([Type](../../../ado/reference/ado-api/type-property-ado-stream.md) est **adTypeBinary**).|  
|**adReadLine**|-2|Lit la ligne suivante dans le flux (désignée par le [LineSeparator](../../../ado/reference/ado-api/lineseparator-property-ado.md) propriété).|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Read, méthode](../../../ado/reference/ado-api/read-method.md)|[ReadText, méthode](../../../ado/reference/ado-api/readtext-method.md)|
