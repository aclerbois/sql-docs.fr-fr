---
title: CopyTo (méthode) (ADO) | Documents Microsoft
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
- _Stream::raw_CopyTo
- _Stream::CopyTo
helpviewer_keywords:
- CopyTo method [ADO]
ms.assetid: b4aa5714-916b-48b8-8b09-cc2708379602
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 32e889ea84c02a544ccad53cd9c274c360994bfe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="copyto-method-ado"></a>CopyTo (méthode) (ADO)
Copie le nombre spécifié de caractères ou d’octets (en fonction de [Type](../../../ado/reference/ado-api/type-property-ado-stream.md)) dans le [flux](../../../ado/reference/ado-api/stream-object-ado.md) vers un autre **flux** objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.CopyTo DestStream, NumChars  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DestStream*  
 Valeur de variable objet qui contient une référence à une ouverture **flux** objet. En cours **flux** est copié vers la destination **flux** spécifié par *DestStream*. La destination **flux** doit déjà être ouvert. Dans le cas contraire, une erreur d’exécution se produit.  
  
> [!NOTE]
>  Le *DestStream* paramètre ne peut pas être un proxy de **flux** , car cela nécessite un accès à une interface privée sur l’objet le **flux** objet ne peut pas être exécutée à distance sur le client.  
  
 *NumChars*  
 Ce paramètre est facultatif. Un **entier** valeur qui spécifie le nombre d’octets ou de caractères à copier à partir de la position actuelle dans la source de **flux** vers la destination **flux**. La valeur par défaut est – 1, qui spécifie que tous les caractères ou octets sont copiés à partir de la position actuelle pour [fin du support](../../../ado/reference/ado-api/eos-property.md).  
  
## <a name="remarks"></a>Notes  
 Cette méthode copie le nombre spécifié de caractères ou d’octets à partir de la position actuelle spécifiée par le [Position](../../../ado/reference/ado-api/position-property-ado.md) propriété. Si le nombre spécifié est supérieur au nombre d’octets jusqu'à disponible **fin du support**, alors que des caractères ou octets à partir de la position actuelle pour **fin du support** sont copiés. Si la valeur de *NumChars* est – 1, ou est omis, tous les caractères ou octets à partir de la position actuelle sont copiés.  
  
 Qu’en présence de caractères ou octets dans le flux de destination, tout le contenu au-delà du point où se termine la copie reste et n’est pas tronqué. **Position** devient l’octet suivant immédiatement le dernier octet copié. Si vous voulez tronquer ces octets, appelez [SetEOS](../../../ado/reference/ado-api/seteos-method.md).  
  
 **CopyTo** doit être utilisé pour copier des données vers une destination **flux** du même type que la source de **flux** (leurs **Type** les paramètres de propriété sont les deux **adTypeText** ou les deux **adTypeBinary**). Pour le texte **flux** des objets, vous pouvez modifier le [Charset](../../../ado/reference/ado-api/charset-property-ado.md) définition de la propriété de la destination **flux** permet de convertir un jeu de caractères à un autre. En outre, le texte **flux** objets peuvent être copiés avec succès en binaire **flux** objets mais binaire **flux** objets ne peut pas être copiés dans le texte **flux**  objets.  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)
