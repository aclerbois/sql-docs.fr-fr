---
title: Valeurs totales et Non visibles | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 90925c99f281d128ff197fb7b0ef3b088faf5b7d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="visual-totals-and-non-visual-totals"></a>Valeurs totales affichées et non affichées
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Les valeurs totales affichées sont les totaux à la fin d'une colonne ou d'une ligne qui additionnent tous les éléments dans la colonne ou la ligne. C'est le comportement par défaut pour la plupart des tables en cas d'affichage. Toutefois, dans certains cas, l'utilisateur peut souhaiter afficher uniquement certaines colonnes dans une table mais garder les totaux pour la ligne entière, y compris ceux qui ne sont pas affichés. On parle de **valeurs totales non affichées**, parce que le total est calculé à partir des valeurs visibles et non visibles.  
  
 Le scénario suivant montre le comportement des valeurs totales non affichées. La première étape montre le comportement par défaut des valeurs totales affichées.  
  
 L'exemple suivant illustre une requête sur [Adventure Works] pour obtenir les chiffres [Reseller Sales Amount] dans une table où les catégories de produits sont représentées par les colonnes et les types de secteur d'activité des revendeurs sont représentés par les lignes. Notez que les totaux sont donnés pour les produits et les revendeurs à la fois lorsque l'instruction SELECT suivante est émise :  
  
 `select [Category].members on 0,`  
  
 `[Business Type].members on 1`  
  
 `from [Adventure Works]`  
  
 `where [Measures].[Reseller Sales Amount]`  
  
 Génère les résultats suivants :  
  
|||||||  
|-|-|-|-|-|-|  
||**All Products**|**Accessories**|**Bikes**|**Clothing**|**Components**|  
|**All Resellers**|**80 450 596,98 $**|**571 297,93 $**|**66 302 381,56 $**|**1 777 840,84 $**|**11 799 076,66 $**|  
|**Specialty Bike Shop**|**6 756 166,18 $**|**65 125,48 $**|**6 080 117,73 $**|**252 933,91 $**|**357 989,07 $**|  
|**Value Added Reseller**|**34 967 517,33 $**|**175 002,81 $**|**30 892 354,33 $**|**592 385,71 $**|**3 307 774,48 $**|  
|**Warehouse**|**38 726 913,48 $**|**331 169,64 $**|**29 329 909,50 $**|**932 521,23 $**|**8 133 313,11 $**|  
  
## <a name="non-visual-on-rows-and-columns"></a>Non affichées sur les lignes et les colonnes  
 Pour créer une table contenant uniquement les données pour les produits Accessories et Clothing, ainsi que les revendeurs Value Added Reseller et Warehouse, tout en conservant les totaux globaux, vous pouvez utiliser NON VISUAL comme suit :  
  
 `select [Category].members on 0,`  
  
 `[Business Type].members on 1`  
  
 `from NON VISUAL (Select {[Category].Accessories, [Category].Clothing} on 0,`  
  
 `{[Business Type].[Value Added Reseller], [Business Type].[Warehouse]} on 1`  
  
 `from [Adventure Works])`  
  
 `where [Measures].[Reseller Sales Amount]`  
  
 Génère les résultats suivants :  
  
|||||  
|-|-|-|-|  
||**All Products**|**Accessories**|**Clothing**|  
|**All Resellers**|**80 450 596,98 $**|**571 297,93 $**|**1 777 840,84 $**|  
|**Value Added Reseller**|**34 967 517,33 $**|**175 002,81 $**|**592 385,71 $**|  
|**Warehouse**|**38 726 913,48 $**|**331 169,64 $**|**932 521,23 $**|  
  
## <a name="non-visual-on-rows"></a>Non affichées sur les lignes  
 Pour créer une table qui additionne visuellement les colonnes mais qui, pour les totaux de ligne, affiche le total réel de tout l'élément [Category], vous pouvez utiliser la requête suivante :  
  
 `select [Category].members on 0,`  
  
 `[Business Type].members on 1`  
  
 `from NON VISUAL (Select {[Category].Accessories, [Category].Clothing} on 0`  
  
 `from ( Select {[Business Type].[Value Added Reseller], [Business Type].[Warehouse]} on 0`  
  
 `from [Adventure Works])`  
  
 `)`  
  
 `where [Measures].[Reseller Sales Amount]`  
  
 Notez que NON VISUAL est uniquement appliqué à [Category].  
  
 La requête ci-dessus génère les résultats suivants :  
  
|||||  
|-|-|-|-|  
||All Products|Accessories|Clothing|  
|All Resellers|73 694 430,80 $|506 172,45 $|1 524 906,93 $|  
|Value Added Reseller|34 967 517,33 $|175 002,81 $|592 385,71 $|  
|Warehouse|38 726 913,48 $|331 169,64 $|932 521,23 $|  
  
 Lorsque l'on effectue une comparaison avec les résultats précédents, on observe que la ligne [All Resellers] est additionnée avec les valeurs affichées pour [Value Added Reseller] et [Warehouse] mais que la colonne [All Products] affiche la valeur totale pour tous les produits, y compris ceux qui ne sont pas affichés.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts clés dans MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md)   
 [Fonctionnalité Autoexists](../../../analysis-services/multidimensional-models/mdx/autoexists.md)   
 [Utilisation des membres, Tuples et jeux & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/working-with-members-tuples-and-sets-mdx.md)   
 [Principes de base de requête MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)   
 [La requête MDX de base & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-the-basic-query.md)   
 [Restriction de la requête avec les Axes de secteur et de requête & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-restricting-the-query.md)   
 [Définition d’un contexte de Cube dans une requête & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/establishing-cube-context-in-a-query-mdx.md)  
  
  
