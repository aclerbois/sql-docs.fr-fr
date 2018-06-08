---
title: CompareBookmarks, méthode (ADO) | Documents Microsoft
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
- CompareBookmarks
- Recordset20::CompareBookmarks
- Recordset20::raw_CompareBookmarks
helpviewer_keywords:
- CompareBookmarks method [ADO]
ms.assetid: d0b64286-2cc4-4a22-8f1d-9aefeebbcbc6
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f17b1bbb0793e06a5ecbbec393fd87ebcdbc311e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="comparebookmarks-method-ado"></a>CompareBookmarks, méthode (ADO)
Compare deux signets et retourne une indication de leurs valeurs relatives.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
result = recordset.CompareBookmarks(Bookmark1, Bookmark2)  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un [CompareEnum](../../../ado/reference/ado-api/compareenum.md) valeur qui indique la position de ligne relative de deux enregistrements représentée par leur signet.  
  
#### <a name="parameters"></a>Paramètres  
 *Bookmark1*  
 Le signet de la première ligne.  
  
 *Bookmark2*  
 Le signet de la deuxième ligne.  
  
## <a name="remarks"></a>Notes  
 Les signets doivent s’appliquer à la même [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet, ou un **Recordset** objet et ses [clone](../../../ado/reference/ado-api/clone-method-ado.md). Vous ne pouvez pas comparer correctement signets à partir de différents **Recordset** des objets, même si elles ont été créées à partir de la même source ou commande. Ni vous pouvez comparer des signets d’un **Recordset** objet dont le fournisseur sous-jacent ne prend pas en charge les comparaisons.  
  
 Un signet identifie de façon unique une ligne dans un **Recordset** objet. Utilisez le [signet](../../../ado/reference/ado-api/bookmark-property-ado.md) propriété de la ligne actuelle pour obtenir son signet.  
  
 Étant donné que le type de données d’un signet est spécifique à chaque fournisseur, ADO expose comme un **variante**. Par exemple, les signets SQL Server sont de type DBTYPE_R8 (**Double**). ADO expose ce type comme un **Variant** avec un sous-type de **Double**.  
  
 Lorsque vous comparez des signets, ADO ne tente pas de n’importe quel type de contrainte. Les valeurs sont simplement transmises au fournisseur où la comparaison se produit. Si les signets passés à la **CompareBookmarks** sont stockés dans des variables de types différents, il peut générer l’erreur d’incompatibilité de type suivant : « Arguments sont de type incorrect, sont hors des limites autorisées ou sont en conflit entre eux. »  
  
 Un signet qui n’est pas valide ou mal formée provoque une erreur.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple CompareBookmarks, méthode (VB)](../../../ado/reference/ado-api/comparebookmarks-method-example-vb.md)   
 [Exemple de CompareBookmarks, méthode (VC ++)](../../../ado/reference/ado-api/comparebookmarks-method-example-vc.md)   
 [Bookmark, propriété (ADO)](../../../ado/reference/ado-api/bookmark-property-ado.md)
