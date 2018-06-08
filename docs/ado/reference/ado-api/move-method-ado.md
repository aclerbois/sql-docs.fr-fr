---
title: Move, méthode (ADO) | Documents Microsoft
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
- Recordset15::Move
- Recordset15::raw_Move
helpviewer_keywords:
- Move method [ADO]
ms.assetid: 13fe9381-d00b-4f4a-9162-83c3f21b3837
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a596576b742eed097d0f7f89f5c13b70adb99477
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="move-method-ado"></a>Move, méthode (ADO)
Déplace la position de l’enregistrement actif dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Move NumRecords, Start  
```  
  
#### <a name="parameters"></a>Paramètres  
 *NumRecords*  
 Connecté **Long** expression qui spécifie le nombre d’enregistrements qui se déplace de la position actuelle.  
  
 *Démarrer*  
 Ce paramètre est facultatif. A **chaîne** valeur ou **Variant** qui correspond à un signet. Vous pouvez également utiliser un [BookmarkEnum](../../../ado/reference/ado-api/bookmarkenum.md) valeur.  
  
## <a name="remarks"></a>Notes  
 Le **déplacer** méthode est prise en charge sur tous les **Recordset** objets.  
  
 Si le *NbEnregistrements* argument est supérieure à zéro, la position actuelle se déplace vers l’avant (vers la fin de la **Recordset**). Si *NbEnregistrements* est inférieur à zéro, la position actuelle se déplace vers l’arrière (vers le début de la **Recordset**).  
  
 Si le **déplacer** appel serait déplace la position actuelle à un point avant le premier enregistrement, ADO définit l’enregistrement actif à la position avant le premier enregistrement dans le jeu d’enregistrements ([BOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) est **True** ). Une tentative de déplacement vers l’arrière lorsque la **BOF** propriété est déjà **True** génère une erreur.  
  
 Si le **déplacer** appel serait déplace la position actuelle vers un point après le dernier enregistrement, ADO définit l’enregistrement actif à la position après le dernier enregistrement dans le jeu d’enregistrements ([EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) est **True** ). Une tentative de déplacement vers l’avant lorsque la **EOF** propriété est déjà **True** génère une erreur.  
  
 Appel de la **déplacer** méthode vide **Recordset** objet génère une erreur.  
  
 Si vous passez le *Démarrer* argument, le déplacement est relatif à l’enregistrement de ce signet, en supposant que le **Recordset** objet prend en charge les signets. Si non spécifié, le déplacement est relatif à l’enregistrement actif.  
  
 Si vous utilisez la [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) propriété à mettre en cache localement les enregistrements à partir du fournisseur, passant un *NbEnregistrements* argument qui déplace la position actuelle en dehors du groupe actuel d’enregistrements mis en cache force ADO à récupérer un nouveau groupe d’enregistrements, à partir de l’enregistrement de destination. Le **CacheSize** propriété détermine la taille du groupe qui vient d’être extrait et l’enregistrement de destination est le premier enregistrement récupéré.  
  
 Si le **Recordset** objet est avant uniquement, un utilisateur peut toujours se transmettre un *NbEnregistrements* argument inférieur à zéro, à condition que la destination soit dans le jeu actuel d’enregistrements mis en cache. Si le **déplacer** appel serait déplace la position actuelle à un enregistrement avant le premier enregistrement mis en cache, une erreur se produit. Par conséquent, vous pouvez utiliser un cache d’enregistrements qui prend en charge le défilement complète sur un fournisseur qui prend en charge le défilement vers l’avant uniquement. Étant donné que les enregistrements mis en cache sont chargés en mémoire, vous devez éviter la mise en cache plus d’enregistrements que nécessaire. Même si uniquement un curseur avant **Recordset** déplace les prend en charge l’objet vers l’arrière de cette façon, l’appel du [MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md) méthode sur n’importe quel avant uniquement **Recordset** objet continue génère une erreur.  
  
> [!NOTE]
>  Prise en charge d’un déplacement vers l’arrière dans un curseur avant uniquement **Recordset** n’est pas prévisible, en fonction de votre fournisseur. Si l’enregistrement actif a été positionné après le dernier enregistrement dans le **Recordset**, **déplacer** vers l’arrière ne peut pas entraîner la position actuelle correcte.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode Move (VB)](../../../ado/reference/ado-api/move-method-example-vb.md)   
 [Exemple de méthode Move (VBScript)](../../../ado/reference/ado-api/move-method-example-vbscript.md)   
 [Exemple de méthode Move (VC ++)](../../../ado/reference/ado-api/move-method-example-vc.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO)](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (RDS)](../../../ado/reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [MoveRecord, méthode (ADO)](../../../ado/reference/ado-api/moverecord-method-ado.md)
