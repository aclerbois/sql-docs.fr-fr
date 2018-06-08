---
title: Méthode SaveToFile | Documents Microsoft
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
- _Stream::raw_SaveToFile
- _Stream::SaveToFile
helpviewer_keywords:
- SaveToFile method [ADO]
ms.assetid: 8a8594f2-422b-4d2e-94f8-7fe337445900
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e9beb8a1e2bfcc307c93b293a35dcac26cf0847a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="savetofile-method"></a>SaveToFile, méthode
Enregistre le contenu binaire d’un [flux](../../../ado/reference/ado-api/stream-object-ado.md) dans un fichier.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.SaveToFile FileName, SaveOptions  
```  
  
#### <a name="parameters"></a>Paramètres  
 *FileName*  
 A **chaîne** valeur qui contient le nom qualifié complet du fichier dans lequel le contenu de la **flux** sera enregistré. Vous pouvez enregistrer dans n’importe quel emplacement local valide ou n’importe quel emplacement que vous avez accès via une valeur UNC.  
  
 *SaveOptions*  
 A [SaveOptionsEnum](../../../ado/reference/ado-api/saveoptionsenum.md) valeur qui spécifie si un nouveau fichier doit être créé par **SaveToFile**, s’il n’existe pas. Valeur par défaut est **adSaveCreateNotExists**. Avec ces options, vous pouvez spécifier qu’une erreur se produit si le fichier spécifié n’existe pas. Vous pouvez également spécifier **SaveToFile** remplace le contenu actuel d’un fichier existant.  
  
> [!NOTE]
>  Si vous remplacez un fichier existant (lorsque **valeur adSaveCreateOverwrite** est défini), **SaveToFile** tronque tous les octets à partir du fichier existant d’origine qui suivent la nouvelle [fin du support](../../../ado/reference/ado-api/eos-property.md).  
  
## <a name="remarks"></a>Notes  
 **SaveToFile** peut être utilisé pour copier le contenu d’un **flux** objet dans un fichier local. Il n’existe aucun changement dans le contenu ou les propriétés de la **flux** objet. Le **flux** objet doit être ouvert avant d’appeler **SaveToFile**.  
  
 Cette méthode ne modifie pas l’association de la **flux** objet et sa source sous-jacente. Le **flux** objet sera toujours associé à l’URL d’origine ou **enregistrement** qui était la source de son ouverture.  
  
 Après un **SaveToFile** opération, la position actuelle ([Position](../../../ado/reference/ado-api/position-property-ado.md)) dans le flux est définie au début du flux (0).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open (méthode) (flux ADO)](../../../ado/reference/ado-api/open-method-ado-stream.md)   
 [Save, méthode](../../../ado/reference/ado-api/save-method.md)
