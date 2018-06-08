---
title: RecordOpenOptionsEnum | Documents Microsoft
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
- RecordOpenOptionsEnum
helpviewer_keywords:
- RecordOpenOptionsEnum enumeration [ADO]
ms.assetid: 9028aba4-90fc-4dfc-88e4-fa8a7b6fedee
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c7372e17fb7e18cec3cbb25a1850433e41e918a7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="recordopenoptionsenum"></a>RecordOpenOptionsEnum
Spécifie les options pour l’ouverture un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md). Ces valeurs peuvent être combinées à l’aide d’ou.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adDelayFetchFields**|0x8000|Indique au fournisseur que les champs associés le **enregistrement** ne doivent pas être récupérée au départ, mais peuvent être récupérés à la première tentative d’accéder au champ. Le comportement par défaut, indiqué par l’absence de cet indicateur, consiste à récupérer toutes les **enregistrement** les champs de l’objet.|  
|**adDelayFetchStream**|0x4000|Indique au fournisseur qui le flux par défaut associé à la **enregistrement** ne doivent pas être récupérées initialement. Le comportement par défaut, indiqué par l’absence de cet indicateur, est d’extraire la chaîne par défaut associée à la **enregistrement** objet.|  
|**adOpenAsync**|0x1000|Indique que le **enregistrement** objet est ouvert en mode asynchrone.|  
|**adOpenExecuteCommand**|0x10000|Indique que la chaîne Source contienne le texte de la commande qui doit être exécuté. Cette valeur est équivalente à la **adCmdText** option **Recordset.Open**.|  
|**adOpenRecordUnspecified**|-1|Valeur par défaut. Indique aucune option n’est spécifiée.|  
|**adOpenOutput**|0x800000|Indique que, si la source pointe sur un nœud qui contient un script exécutable (comme un. Page ASP), puis l’ouvert **enregistrement** contiendra les résultats du script exécuté. Cette valeur est uniquement valide avec les enregistrements sans collection.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [Open, méthode (objet Record ADO)](../../../ado/reference/ado-api/open-method-ado-record.md)
