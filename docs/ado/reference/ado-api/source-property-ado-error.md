---
title: Source, propriété (erreur ADO) | Documents Microsoft
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
- Error::get_Source
- Error::Source
- Error::GetSource
helpviewer_keywords:
- Source property [ADO Error]
ms.assetid: 4044ba15-f013-4c4c-9fe1-b4410fe9a778
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1db468ae4575a494b03efc5cf9eb3372b6d5cab2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="source-property-ado-error"></a>Source, propriété (erreur ADO)
Indique le nom de l’objet ou l’application qui a généré une erreur.  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un **chaîne** valeur qui indique le nom d’un objet ou l’application.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **Source** propriété sur une [erreur](../../../ado/reference/ado-api/error-object.md) objet afin de déterminer le nom de l’objet ou l’application qui a généré une erreur. Cela peut être l’objet nom de classe ou l’ID programmatique. Pour les erreurs dans ADO, la valeur de propriété sera **ADODB. *** ObjectName*, où *ObjectName* est le nom de l’objet qui a déclenché l’erreur. Pour ADOX et ADO MD, la valeur sera **ADOX. *** ObjectName* et **ADOMD. *** ObjectName,* respectivement.  
  
 En fonction de la documentation à partir de la **Source**, [nombre](../../../ado/reference/ado-api/number-property-ado.md), et [Description](../../../ado/reference/ado-api/description-property.md) propriétés de **erreur** des objets, vous pouvez écrire du code qui gère l’erreur en conséquence.  
  
 Le **Source** propriété est en lecture seule pour **erreur** objets.  
  
## <a name="applies-to"></a>S'applique à  
 [Error, objet](../../../ado/reference/ado-api/error-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Description, HelpContext, HelpFile, Native Error, nombre, Source et SQLState, propriétés-exemple (VB)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description, HelpContext, HelpFile, Native Error, nombre, Source et SQLState, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description (propriété)](../../../ado/reference/ado-api/description-property.md)   
 [HelpContext, HelpFile, propriétés](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)   
 [Number, propriété (ADO)](../../../ado/reference/ado-api/number-property-ado.md)   
 [Source, propriété (enregistrement ADO)](../../../ado/reference/ado-api/source-property-ado-record.md)   
 [Source, propriété (objet Recordset ADO)](../../../ado/reference/ado-api/source-property-ado-recordset.md)
