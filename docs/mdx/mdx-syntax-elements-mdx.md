---
title: Éléments de syntaxe MDX (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b530205a5165c9be77710bf86e8600174be890dc
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581021"
---
# <a name="mdx-syntax-elements-mdx"></a>Éléments de la syntaxe MDX (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Les expressions MDX (Multidimensional Expressions) comportent plusieurs éléments utilisés par la plupart des instructions ou ayant des répercussions sur ces dernières :  
  
|Terme|Définition|  
|----------|----------------|  
|[Identificateurs](../mdx/identifiers-mdx.md)|Les identificateurs sont les noms d'objets tels que des cubes, des dimensions, des membres et des mesures.|  
|**Types de données**|Définissent les types de données contenus dans des cellules, des propriétés de membre et des propriétés de cellule. La syntaxe MDX ne prend en charge que le type de données OLE VARIANT. Pour plus d'informations sur les contraintes, la conversion et la manipulation du type de données VARIANT, consultez « VARIANT and VARIANTARG » (en anglais) dans la documentation du Kit de développement Platform SDK.|  
|[Expressions &#40;MDX&#41;](../mdx/expressions-mdx.md)|Les expressions sont des unités de syntaxe qui [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peut résoudre en valeurs (scalaires) uniques ou des objets. Elles comprennent des fonctions qui retournent une valeur unique, une expression d'ensemble, etc.|  
|[Opérateurs](../mdx/operators-mdx-syntax.md)|Les opérateurs sont des éléments de syntaxe ajoutés à une ou plusieurs expressions MDX simples pour en créer de plus complexes.|  
|[Fonctions](../mdx/functions-mdx-syntax.md)|Les fonctions sont des éléments de syntaxe qui acceptent une ou plusieurs valeurs d'entrée (voire aucune) et retournent une valeur scalaire ou un objet. Exemples le [somme](../mdx/sum-mdx.md) fonction pour l’ajout de plusieurs valeurs, la [membres](../mdx/members-set-mdx.md) la fonction pour retourner un jeu de membres à partir d’une dimension ou un niveau et ainsi de suite.|  
|[Commentaires](../mdx/comments-mdx-syntax.md)|Les commentaires sont des parties de texte insérées dans des instructions ou des scripts MDX afin d'expliquer le but d'une instruction. [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] n’exécute pas les commentaires.|  
|[Mots clés réservés](../mdx/reserved-keywords-mdx-syntax.md)|Les mots clés réservés sont des mots dont l'utilisation est réservée à MDX et qui ne doivent pas être utilisés comme nom d'objet dans des instructions MDX.|  
|[Membres, Tuples et jeux](../analysis-services/multidimensional-models/mdx/working-with-members-tuples-and-sets-mdx.md)|Les membres, tuples et jeux sont des concepts essentiels des données multidimensionnelles que vous devez comprendre avant de créer une requête MDX.|  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions multidimensionnelles &#40;MDX&#41; référence](../mdx/multidimensional-expressions-mdx-reference.md)  
  
  
