---
title: EST (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5c36a5f45e34d6d78043f6ce8a3e3fb040ecc9f6
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578591"
---
# <a name="is-mdx"></a>IS (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Effectue une comparaison logique sur deux expressions d'objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Expression1 IS ( Expression2 | NULL )  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Expression1*  
 Expression MDX (Multidimensional Expressions) valide retournant une référence à un objet MDX.  
  
 *Expression2*  
 Expression MDX valide retournant une référence à un objet MDX.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur booléenne qui retourne **true** si les deux arguments font référence au même objet ; sinon, **false**. Si le **NULL** mot clé est spécifié, l’opérateur retourne **true** si *Expression1* est **null**; sinon, **false**.  
  
## <a name="remarks"></a>Notes  
 Le **IS** opérateur est souvent utilisée pour déterminer si tuples et membres sont idempotents, ce qui signifie qu’ils sont tout à fait équivalents.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment utiliser le **IS** opérateur afin de vérifier si le membre actuel sur un axe est un membre spécifique :  
  
 `With`  
  
 `//Returns TRUE if the currentmember is Bikes`  
  
 `Member [Measures].[IsBikes?] AS`  
  
 `[Product].[Category].CurrentMember IS [Product].[Category].&[1]`  
  
 `SELECT`  
  
 `{[Measures].[IsBikes?]} ON 0,`  
  
 `[Product].[Category].[Category].Members ON 1`  
  
 `FROM`  
  
 `[Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des opérateurs MDX &#40;MDX&#41;](../mdx/mdx-operator-reference-mdx.md)  
  
  
