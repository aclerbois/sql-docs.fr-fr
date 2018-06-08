---
title: Contexte de calcul | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 340d03ba8d0c5a66d89937627ab9389fc49abcae
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="calculation-context"></a>Contexte de calcul
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Le contexte de calcul est le sous-espace connu du cube où une expression est évaluée et où toutes les coordonnées sont explicitement connues ou peuvent être dérivées de l'expression.  
  
## <a name="determining-the-calculation-context"></a>Détermination du contexte de calcul  
 Chaque jeu, membre, tuple ou fonction numérique s'exécute dans le contexte de l'instruction ou de l'expression MDX à part entière. Lorsqu'un argument (par exemple, un tuple) est transmis à une fonction, seules certaines coordonnées dans l'espace du cube sont fournies de manière explicite. Les autres coordonnées sont obtenues sur la base du contexte de calcul actuel.  
  
 Le contexte de calcul des coordonnées des cellules et des membres d'attribut non spécifiés est déterminé dans l'ordre suivant :  
  
1.  Clause FROM (le cas échéant) : cette clause peut spécifier soit un cube tout entier, soit un sous-cube présenté sous la forme d'une instruction SELECT.  
  
2.  Clause WHERE (le cas échéant) : clause également appelée *axe de segment*et dans laquelle vous spécifiez un jeu, un tuple ou un membre qui restreint les membres retournés par une requête dans les axes des colonnes et des lignes. D'un point de vue conceptuel, le membre par défaut de chaque hiérarchie d'attribut qui n'est pas explicitement défini dans l'axe des colonnes ou l'axe des lignes appartient à l'axe de segment.  
  
    > [!NOTE]  
    >  Lorsque vous spécifiez les coordonnées des cellules d'un attribut en particulier à la fois sur l'axe de segment et sur un autre axe, les coordonnées spécifiées dans la fonction peuvent être prioritaires dans le choix des membres du jeu sur l'axe. Les fonctions [Filter (MDX)](../../../mdx/filter-mdx.md) et [Order (MDX)](../../../mdx/order-mdx.md) sont des exemples de ce type de fonction ; vous pouvez filtrer ou classer un résultat par membres d’attribut exclus du contexte de calcul par la clause WHERE ou par une instruction SELECT dans la clause FROM.  
  
3.  Jeux nommés et membres calculés définis dans la requête ou l'expression.  
  
4.  Tuples et jeux spécifiés sur les axes des lignes et des colonnes à l'aide du membre par défaut des attributs qui n'apparaissent pas sur les axes des lignes ou des colonnes ou sur l'axe de segment.  
  
5.  Cellules du cube ou du sous-cube sur chaque axe avec suppression des tuples vides sur l'axe et application de la clause HAVING.  
  
6.  Pour plus d’informations, consultez [Establishing Cube Context in a Query &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/establishing-cube-context-in-a-query-mdx.md).  
  
 Dans la requête qui suit, le contexte de calcul de l'axe des lignes est limité par les membres d'attribut Country et Calendar Year spécifiés dans la clause WHERE.  
  
```  
SELECT Customer.City.City.Members ON 0  
FROM [Adventure Works]  
WHERE (Customer.Country.France, [Date].[Calendar].[Calendar Year].[CY 2004],  
   Measures.[Internet Sales Amount])  
```  
  
 Cependant, si vous modifiez cette requête en spécifiant la fonction **FILTER** sur l’axe des lignes, puis que vous utilisez un membre de la hiérarchie d’attribut Année civile dans la fonction **FILTER** , vous pouvez modifier le membre d’attribut de la hiérarchie d’attribut Année civile utilisé pour fournir le contexte de calcul des membres du jeu sur l’axe des colonnes.  
  
```  
SELECT FILTER  
   (  
      Customer.City.City.Members,   
         ([Date].[Calendar].[Calendar Year].[CY 2003],  
            Measures.[Internet Order Quantity]) > 75   
   ) ON 0  
FROM [Adventure Works]  
WHERE (Customer.Country.France,  
   [Date].[Calendar].[Calendar Year].[CY 2004],  
   Measures.[Internet Sales Amount])  
```  
  
 Dans la requête ci-dessus, le contexte de calcul des cellules dans les tuples qui apparaissent sur l'axe des colonnes est filtré par le membre CY 2003 de la hiérarchie d'attribut Calendar Year, même si le contexte de calcul nominal de la hiérarchie d'attribut Calendar Year est CY 2004. De plus, il est filtré par la mesure Internet Order Quantity. Néanmoins, une fois les membres du jeu définis sur l'axe des colonnes, le contexte de calcul des valeurs des membres qui apparaissent sur l'axe est de nouveau déterminé par la clause WHERE.  
  
> [!IMPORTANT]  
>  Pour accroître les performances de la requête, pensez à éliminer les membres et les tuples le plus tôt possible au cours du processus de résolution. De cette manière, les calculs de temps des requêtes complexes dans le jeu de membres final peuvent cibler le plus petit nombre de cellules possible.  
  
## <a name="see-also"></a>Voir aussi  
 [Définition d’un contexte de Cube dans une requête & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/establishing-cube-context-in-a-query-mdx.md)   
 [Principes de base de requête MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)   
 [Concepts clés dans MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md)  
  
  
