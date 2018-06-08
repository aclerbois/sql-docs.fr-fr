---
title: Tail (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5192deedcb6085767fc00f68bb17f6adddb38679
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581241"
---
# <a name="tail-mdx"></a>Tail (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne un sous-ensemble de la fin d'un jeu.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Tail(Set_Expression [ ,Count ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Nombre*  
 Expression numérique valide qui précise le nombre de tuples à retourner.  
  
## <a name="remarks"></a>Notes  
 Le **fin** fonction retourne le nombre spécifié de tuples à partir de la fin du jeu spécifié. L'ordre des éléments est conservé. La valeur par défaut *nombre* est 1. Si le nombre de tuples spécifié est inférieur à 1, la fonction retourne le jeu vide. Si le nombre de tuples spécifié dépasse le nombre de tuples dans le jeu, la fonction retourne le jeu d'origine.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne la mesure Reseller Sales pour les cinq premières sous-catégories de vente de produits, quelle que soit la hiérarchie et conformément à la mesure Reseller Gross Profit (marge brute du revendeur). Le **fin** fonction est utilisée pour retourner uniquement les cinq derniers jeux dans le résultat, une fois que le résultat est classé à l’aide de la **ordre** (fonction).  
  
```  
SELECT Tail  
   (Order   
      ([Product].[Product Categories].[SubCategory].members  
         ,[Measures].[Reseller Gross Profit]  
         ,BASC  
      )  
   ,5  
   ) ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
