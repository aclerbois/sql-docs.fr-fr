---
title: Méthode WriteText | Documents Microsoft
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
- _Stream::raw_WriteText
- _Stream::WriteText
helpviewer_keywords:
- WriteText method [ADO]
ms.assetid: 7a669048-13f4-4574-a2b1-985e089729d5
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a2b12293935df6f9afaf6a1691e2decce3f6c6f9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="writetext-method"></a>WriteText, méthode
Écrit une chaîne de texte spécifiée dans un [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.WriteText Data, Options  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Données*  
 A **chaîne** valeur qui contient le texte de caractères à écrire.  
  
 *Options*  
 Ce paramètre est facultatif. A [StreamWriteEnum](../../../ado/reference/ado-api/streamwriteenum.md) valeur qui spécifie si un caractère séparateur de ligne doit être écrit à la fin de la chaîne spécifiée.  
  
## <a name="remarks"></a>Notes  
 Les chaînes spécifiées sont écrites dans le **flux** objet sans espaces insérés ni caractères entre chaque chaîne.  
  
 En cours [Position](../../../ado/reference/ado-api/position-property-ado.md) est définie sur le caractère suivant les données écrites. Le **WriteText** méthode ne tronque pas le reste des données dans un flux de données. Si vous voulez tronquer ces caractères, appelez [SetEOS](../../../ado/reference/ado-api/seteos-method.md).  
  
 Si vous écrivez après actuel [fin du support](../../../ado/reference/ado-api/eos-property.md) position, la [taille](../../../ado/reference/ado-api/size-property-ado-stream.md) de la **flux** augmente pour contenir les nouveaux caractères, et **fin du support** déplace vers le nouveau dernier octet de la **flux**.  
  
> [!NOTE]
>  Le **WriteText** méthode est utilisée avec des flux de texte ([Type](../../../ado/reference/ado-api/type-property-ado-stream.md) est **adTypeText**). Pour les flux binaires (**Type** est **adTypeBinary**), utilisez [écrire](../../../ado/reference/ado-api/write-method.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Write, méthode](../../../ado/reference/ado-api/write-method.md)
