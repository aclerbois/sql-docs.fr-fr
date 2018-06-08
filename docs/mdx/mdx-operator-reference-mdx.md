---
title: Référence des opérateurs MDX (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0b0750732af43f1d19922b0259b35d472be57b47
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580601"
---
# <a name="mdx-operator-reference-mdx"></a>Référence des opérateurs MDX (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Le langage MDX (Multidimensional Expressions) prend en charge des opérateurs arithmétiques, logiques, de comparaison, de jeu, de chaîne et unaires. Le tableau ci-dessous répertorie et décrit les opérateurs pris en charge.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[-- &#40;Commentaire&#41; &#40;MDX&#41;](../mdx/comment-mdx-operator-reference.md)|Signale un texte de commentaire inséré par l'utilisateur.|  
|[- &#40;Sauf&#41; &#40;MDX&#41;](../mdx/except-mdx-operator.md)|Exécute une opération de jeu qui retourne la différence entre deux jeux, en supprimant les doublons.|  
|[- &#40;Négatif&#41; &#40;MDX&#41;](../mdx/negative-mdx.md)|Exécute une opération unaire qui retourne la valeur négative d'une expression numérique.|  
|[- &#40;Soustraire&#41; &#40;MDX&#41;](../mdx/subtract-mdx.md)|Exécute une opération arithmétique qui soustrait un nombre d'un autre.|  
|[&#42;&#40;Crossjoin&#41; &#40;MDX&#41;](../mdx/crossjoin-mdx-operator-reference.md)|Exécute une opération de jeu qui retourne le produit croisé de deux jeux.|  
|[&#42;&#40;Multiplier&#41; &#40;MDX&#41;](../mdx/multiply-mdx.md)|Exécute une opération arithmétique qui multiplie deux nombres.|  
|[&#40;Diviser&#41; &#40;MDX&#41;](../mdx/divide-mdx-operator-reference.md)|Exécute une opération arithmétique qui divise un nombre par un autre.|  
|[^ &#40;Power&#41; &#40;MDX&#41;](../mdx/power-mdx.md)|Exécute une opération arithmétique qui élève un nombre à la puissance indiquée par un autre nombre.|  
|[Commentaire &#40;MDX&#41;](../mdx/comment-mdx.md)|Signale un texte de commentaire inséré par l'utilisateur.|  
|[&#40;Commentaire&#41; &#40;MDX&#41;](../mdx/comment-mdx-double-slash.md)|Indique un texte défini par l'utilisateur.|  
|[: &#40;Plage&#41; &#40;MDX&#41;](../mdx/range-mdx.md)|Exécute une opération de jeu qui retourne un jeu naturellement ordonné, dont les extrémités sont représentées par les deux membres spécifiés, entre lesquels figurent les autres membres du jeu.|  
|[+ &#40;Ajouter&#41; &#40;MDX&#41;](../mdx/add-mdx.md)|Exécute une opération arithmétique qui additionne deux nombres.|  
|[+ &#40;Positif&#41; &#40;MDX&#41;](../mdx/positive-mdx.md)|Exécute une opération unaire qui retourne la valeur positive d'une expression numérique.|  
|[+ &#40;Concaténation de chaînes&#41; &#40;MDX&#41;](../mdx/string-concatenation-mdx.md)|Exécute une opération de chaîne qui concatène deux ou plusieurs chaînes de caractères, tuples, ou une combinaison de chaînes et de tuples.|  
|[+ &#40;Union&#41; &#40;MDX&#41;](../mdx/union-mdx-operator-reference.md)|Exécute une opération de jeu qui retourne une union de deux jeux en supprimant les doublons.|  
|[&#60;&#40;Moins&#41; &#40;MDX&#41;](../mdx/less-than-mdx.md)|Exécute une opération de comparaison qui détermine si la valeur d'une expression MDX est inférieure à celle d'une autre expression MDX.|  
|[&#60;= &#40;Inférieure ou égale à&#41; &#40;MDX&#41;](../mdx/less-than-or-equal-to-mdx.md)|Exécute une opération de comparaison qui détermine si la valeur d'une expression MDX est inférieure ou égale à celle d'une autre expression MDX.|  
|[&#60;&#62;&#40;Différent de&#41; &#40;MDX&#41;](../mdx/not-equal-to-mdx.md)|Exécute une opération de comparaison qui détermine si la valeur d'une expression MDX est différente de celle d'une autre expression MDX.|  
|[= &#40;Égal à&#41; &#40;MDX&#41;](../mdx/equal-to-mdx.md)|Effectue une opération de comparaison qui détermine si la valeur d’une expression MDX est égale à la valeur d’une autre expression MDX.|  
|[&#62;&#40;Supérieur&#41; &#40;MDX&#41;](../mdx/greater-than-mdx.md)|Effectue une opération de comparaison qui détermine si la valeur d’une expression MDX est supérieure à la valeur d’une autre expression MDX.|  
|[&#62;= &#40;Supérieur ou égal à&#41; &#40;MDX&#41;](../mdx/greater-than-or-equal-to-mdx.md)|Exécute une opération de comparaison qui détermine si la valeur d'une expression MDX est supérieure ou égale à celle d'une autre expression MDX.|  
|[ET &AMP;#40;MDX&AMP;#41;](../mdx/and-mdx.md)|Effectue une conjonction logique sur deux expressions numériques.|  
|[EST &AMP;#40;MDX&AMP;#41;](../mdx/is-mdx.md)|Effectue une comparaison logique sur deux expressions d'objet.|  
|[PAS &AMP;#40;MDX&AMP;#41;](../mdx/not-mdx.md)|Effectue une négation logique sur une expression numérique.|  
|[OU &AMP;#40;MDX&AMP;#41;](../mdx/or-mdx.md)|Effectue une disjonction logique sur deux expressions numériques.|  
|[XOR &AMP;#40;MDX&AMP;#41;](../mdx/xor-mdx.md)|Effectue une exclusion logique sur deux expressions numériques.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence du langage MDX &#40;MDX&#41;](../mdx/mdx-language-reference-mdx.md)  
  
  
