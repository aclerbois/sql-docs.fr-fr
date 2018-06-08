---
title: Opérateurs de jeu | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1388c5ef1f263f4ef326485c662a82ee11c569d0
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580951"
---
# <a name="set-operators"></a>Opérateurs de jeu
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Dans la syntaxe MDX (Multidimensional Expressions), les opérateurs de jeu exécutent des opérations sur des membres ou des jeux et retournent un jeu. Ils sont généralement utilisés comme alternative à plusieurs fonctions de jeu dans les expressions MDX.  
  
 MDX prend en charge les opérateurs de jeu répertoriés dans le tableau suivant.  
  
|Opérateur|Description|  
|--------------|-----------------|  
|[- (Sauf)](../mdx/except-mdx-operator.md)|Retourne les différences entre deux jeux, en supprimant les membres en double.<br /><br /> Cet opérateur est fonctionnellement équivalent à la [sauf](../mdx/except-mdx-function.md) (fonction).|  
|[* (Jointure croisée)](../mdx/crossjoin-mdx-operator-reference.md)|Renvoie le produit croisé de deux jeux de données<br /><br /> Cet opérateur est fonctionnellement équivalent à la [Crossjoin](../mdx/crossjoin-mdx.md) (fonction).|  
|[: (Plage)](../mdx/range-mdx.md)|Retourne un jeu naturellement ordonné, dont les extrémités sont représentées par les deux membres spécifiés, entre lesquels figurent les autres membres du jeu.|  
|[+ (Union)](../mdx/union-mdx-operator-reference.md)|Retourne l'union de deux jeux, en excluant les membres en double.<br /><br /> Cet opérateur est fonctionnellement équivalent à la [Union &#40;MDX&#41; ](../mdx/union-mdx.md) (fonction).|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)   
 [Référence des opérateurs MDX &#40;MDX&#41;](../mdx/mdx-operator-reference-mdx.md)   
 [Opérateurs &#40;syntaxe MDX&#41;](../mdx/operators-mdx-syntax.md)  
  
  
