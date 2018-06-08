---
title: Propriété de la fin du support | Documents Microsoft
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
- EOS
- Stream::EOS
helpviewer_keywords:
- EOS property
ms.assetid: 57e08c5f-f3ed-4ecd-8c66-50b83b1031d1
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 25bab5a0079b07e971200d28bc69e7b36b528523
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="eos-property"></a>Propriété de la fin du support
Indique si la position actuelle est à la fin de la [flux](../../../ado/reference/ado-api/stream-object-ado.md).  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne un **booléenne** valeur qui indique si la position actuelle est à la fin du flux de données. **Fin du support** retourne **True** s’il existe plus d’octets dans le flux ; il retourne **False** s’il existe des octets après la position actuelle.  
  
 Pour définir la position de fin de flux de données, utilisez la [SetEOS](../../../ado/reference/ado-api/seteos-method.md) (méthode). Pour déterminer la position actuelle, utilisez la [Position](../../../ado/reference/ado-api/position-property-ado.md) propriété.  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Fin du support et LineSeparator, propriétés et SkipLine, méthode-exemple (VB)](../../../ado/reference/ado-api/eos-and-lineseparator-properties-and-skipline-method-example-vb.md)   
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)
