---
title: Les instructions de sous-sélection dans les requêtes | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a74ddce096d58ba7b350617515bae3edc5b80c45
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="subselects-in-queries"></a>Instructions de sous-sélection dans les requêtes
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Les expressions d'instruction de sous-sélection sont des expressions SELECT imbriquées utilisées pour restreindre l'espace du cube à partir duquel SELECT externe est évalué. Les instructions de sous-sélection vous permettent de définir un nouvel espace sur lequel tous les calculs sont évalués.  
  
## <a name="subselects-by-example"></a>Instructions de sous-sélection par exemple  
 Commençons par un exemple illustrant comment les instructions de sous-sélection peuvent aider à produire les résultats que nous souhaitons afficher. Supposons qu'il vous est demandé de créer une table qui montre le comportement de ventes, sur des années, pour les 10 produits principaux.  
  
 Le résultat doit ressembler à la table suivante :  
  
|||||  
|-|-|-|-|  
||Somme d'années|Année 1|...|  
|Somme des 10 produits principaux||||  
|Produit A||||  
|...||||  
  
 Pour ce faire, on pourrait écrire l'expression MDX suivante :  
  
```  
SELECT [Date].[Calendar Year].MEMBERS on 0  
     , TOPCOUNT( [Product].[Product].MEMBERS  
               , 10  
               , [Measures].[Sales Amount]  
               ) ON 1  
  FROM [Adventure Works]  
```  
  
 Les résultats retournés sont les suivants :  
  
|||||||  
|-|-|-|-|-|-|  
||All Periods|CY 2005|CY 20062|CY 2007|CY 2008|  
|All Products|80 450 596,98 $|8 065 435,31 $|24 144 429,65 $|32 202 669,43 $|16 038 062,60 $|  
|Mountain-200 Black, 38|1 634 647,94 $|(Null)|(Null)|894 207,97 $|740 439,97 $|  
|Mountain-200 Black, 42|1 285 524,65 $|(Null)|(Null)|722 137,65 $|563 387,00 $|  
|Mountain-200 Silver, 38|1 181 945,82 $|(Null)|(Null)|634 600,78 $|547 345 03 $|  
|Mountain-200 Black, 46|995 927,43 $|(Null)|(Null)|514 995,76 $|480 931,68 $|  
|Mountain-200 Silver, 42|1 005 111,77 $|(Null)|(Null)|529 543,29 $|475 568,49 $|  
|Mountain-200 Silver, 46|975 932,56 $|(Null)|(Null)|526 759,30 $|449 173,26 $|  
|Road-150 Red, 56|792 228,98 $|382 159,24 $|410 069,74 $|(Null)|(Null)|  
|Mountain-200 Black, 38|1 471 078,72 $|(Null)|789 958,49 $|681 120,23 $|(Null)|  
|Road-350-W Yellow, 48|1 380 253,88 $|(Null)|(Null)|744 988,37 $|635 265,50 $|  
  
 Ce résultat est très proche de ce que nous recherchons ; à ceci près que la requête a retourné 9 produits et non pas 10 et que le total All Products reflète la somme de tous les produits et non la somme des 9 produits principaux (dans ce cas). Une autre tentative de résoudre le problème est présentée dans la requête MDX suivante :  
  
```  
SELECT [Date].[Calendar Year].MEMBERS on 0  
     , TOPCOUNT( [Product].[Product].CHILDREN, 10, [Measures].[Sales Amount]) ON 1  
  FROM [Adventure Works]  
```  
  
 Les résultats retournés sont les suivants :  
  
|||||||  
|-|-|-|-|-|-|  
||All Periods|CY 2005|CY 2006|CY 2007|CY 2008|  
|Mountain-200 Black, 38|1 634 647,94 $|(Null)|(Null)|894 207,97 $|740 439,97 $|  
|Mountain-200 Black, 42|1 285 524,65 $|(Null)|(Null)|722 137,65 $|563 387,00 $|  
|Mountain-200 Silver, 38|1 181 945,82 $|(Null)|(Null)|634 600,78 $|547 345 03 $|  
|Mountain-200 Black, 46|995 927,43 $|(Null)|(Null)|514 995,76 $|480 931,68 $|  
|Mountain-200 Silver, 42|1 005 111,77 $|(Null)|(Null)|529 543,29 $|475 568,49 $|  
|Mountain-200 Silver, 46|975 932,56 $|(Null)|(Null)|526 759,30 $|449 173,26 $|  
|Road-150 Red, 56|792 228,98 $|382 159,24 $|410 069,74 $|(Null)|(Null)|  
|Mountain-200 Black, 38|1 471 078,72 $|(Null)|789 958,49 $|681 120,23 $|(Null)|  
|Road-350-W Yellow, 48|1 380 253,88 $|(Null)|(Null)|744 988,37 $|635 265,50 $|  
|Road-150 Red, 62|566 797,97 $|234 018,86 $|332 779,11 $|(Null)|(Null)|  
  
 Ce résultat était très proche du résultat souhaité, car il ne manquait que la somme des produits. À ce stade, on peut commencer à modifier l'expression MDX ci-dessus pour ajouter la ligne manquante ; toutefois, cette tâche pourrait s'avérer fastidieuse.  
  
 Une autre approche pour résoudre le problème, consisterait à penser d'abord en termes de redéfinition de l'espace de cube sur lequel l'expression MDX est résolue. Et si le « nouveau » cube contient uniquement des données des 10 produits principaux ? Le membre All de ce cube sera ensuite ajusté en fonction des 10 produits principaux uniquement et une requête simple répondrait à nos besoins.  
  
 L'expression MDX suivante utilise une instruction de sous-sélection pour redéfinir l'espace de cube sur les 10 produits principaux et produire les résultats désirés :  
  
```  
SELECT [Date].[Calendar Year].MEMBERS on 0  
     , [Product].[Product].MEMBERS on 1  
  FROM (SELECT TOPCOUNT( [Product].[Product].CHILDREN  
                       , 10  
                       , [Measures].[Sales Amount]  
                       ) ON 0  
          FROM [Adventure Works]  
        )  
 WHERE [Measures].[Sales Amount]  
```  
  
 L'expression ci-dessus retourne les résultats suivants :  
  
|||||||  
|-|-|-|-|-|-|  
||All Periods|CY 2005|CY 2006|CY 2007|CY 2008|  
|All Products|19 997 183,30 $|1 696 815,63 $|2 816 611,28 $|7 930 797,72 $|7 552 958,66 $|  
|Mountain-200 Silver, 38|2 160 981,60 $|(Null)|(Null)|1 024 359,10 $|1 136 622,49 $|  
|Mountain-200 Silver, 42|1 914 547,85 $|(Null)|(Null)|903 061,68 $|1 011 486,18 $|  
|Mountain-200 Silver, 46|1 906 248,55 $|(Null)|(Null)|877 077,79 $|1 029 170,76 $|  
|Mountain-200 Black, 38|1 811 229,02 $|(Null)|896 511,60 $|914 717,43 $|(Null)|  
|Mountain-200 Black, 38|2 589 363,78 $|(Null)|(Null)|1 261 406,37 $|1 327 957,41 $|  
|Mountain-200 Black, 42|2 265 485,38 $|(Null)|(Null)|1 126 055,89 $|1 139 429,49 $|  
|Mountain-200 Black, 46|1 957 528,24 $|(Null)|(Null)|946 453,88 $|1 011 074,37 $|  
|Road-150 Red, 62|1 769 096,69 $|828 011,68 $|941 085,01 $|(Null)|(Null)|  
|Road-150 Red, 56|1 847 818,63 $|868 803,96 $|979 014,67 $|(Null)|(Null)|  
|Road-350-W Yellow, 48|1 774 883,56 $|(Null)|(Null)|877 665,59 $|897 217,96 $|  
  
 Les résultats ci-dessus sont exactement ce que nous cherchions.  
  
 Examinons le résultat de l'instruction de sous-sélection. L'instruction de sous-sélection a retourné un nouveau cube qui contenait toutes les autres dimensions de produits tels quels ; toutefois, dans la dimension de produit, elle a filtré tous les membres pour comparer les 10 premiers produits qui nous intéressaient. C'est comme si on avait supprimé toutes les données qui ne répondaient pas aux 10 critères principaux et recréé le cube. L'autre concept important à comprendre dans cet exemple est le fait que les 10 produits principaux ont été calculés pour le membre All de toutes les autres dimensions dans le cube ; le premier concept est vrai parce qu'aucune autre restriction de filtrage n'a été imposée dans l'instruction de sous-sélection.  
  
 Les instructions de sous-sélection peuvent être aussi complexes que souhaité ; l'exemple suivant illustre comment produire une table semblable à celle indiquée ci-dessus, mais filtrée sur France sur la dimension Sales Territory et sur Internet pour la dimension Sales Channel.  
  
```  
SELECT [Date].[Calendar Year].MEMBERS on 0  
     , [Product].[Product].MEMBERS on 1  
  FROM (SELECT TOPCOUNT( [Product].[Product].CHILDREN  
                       , 10  
                       , [Measures].[Sales Amount]  
                       ) ON 0  
             , [Sales Territory].[Sales Territory].[Region].[France] on 1  
             ,  [Sales Channel].[Sales Channel].[Internet] on 2  
          FROM [Adventure Works]  
        )  
 WHERE [Measures].[Sales Amount]  
```  
  
 Génère les résultats suivants :  
  
|||||||  
|-|-|-|-|-|-|  
||All Periods|CY 2005|CY 2006|CY 2007|CY 2008|  
|All Products|748 682,49 $|32 204,43 $|73 125,18 $|269 506,56 $|373 846,32 $|  
|Mountain-200 Silver, 38|90 479,61 $|(Null)|(Null)|41 759,82 $|48 719,79 $|  
|Mountain-200 Silver, 42|97 439,58 $|(Null)|(Null)|39 439,83 $|57 999,75 $|  
|Mountain-200 Silver, 46|102 079,56 $|(Null)|(Null)|27 839,88 $|74 239,68 $|  
|Mountain-200 Black, 38|26 638,28 $|(Null)|12 294,59 $|14 343,69 $|(Null)|  
|Mountain-200 Black, 38|96 389,58 $|(Null)|(Null)|41 309,82 $|55 079,76 $|  
|Mountain-200 Black, 42|80 324,65 $|(Null)|(Null)|43 604,81 $|36 719,84 $|  
|Mountain-200 Black, 46|107 864,53 $|(Null)|(Null)|45 899,80 $|61 964,73 $|  
|Road-150 Red, 62|46 517,51 $|14 313,08 $|32 204,43 $|(Null)|(Null)|  
|Road-150 Red, 56|46 517,51 $|17 891,35 $|28 626,16 $|(Null)|(Null)|  
|Road-350-W Yellow, 48|54 431,68 $|(Null)|(Null)|15 308,91 $|39 122,77 $|  
  
 Les résultats ci-dessus indiquent les 10 produits principaux vendus en France via le canal Internet.  
  
## <a name="subselect-statement"></a>Instruction de sous-sélection  
 La notation BNF pour l'instruction de sous-sélection est :  
  
```  
[WITH [<calc-clause> ...]]  
SELECT [<axis-spec> [, <axis-spec> ...]]  
FROM [<identifier> | (< sub-select-statement >)]  
[WHERE <slicer>]  
[[CELL] PROPERTIES <cellprop> [, <cellprop> ...]]  
  
< sub-select-statement > :=  
   SELECT [<axis-spec> [, <axis-spec> ...]]  
   FROM [<identifier> | (< sub-select-statement >)]  
   [WHERE <slicer>]  
  
```  
  
 L'instruction de sous-sélection est une autre instruction Select dans laquelle les spécifications de l'axe et la spécification du segment filtrent l'espace du cube sur lequel la sélection externe est évaluée.  
  
 Lorsqu'un membre est spécifié dans une clause de l'axe ou du segment, ce membre et ses ascendants et descendants sont inclus dans l'espace de sous-cube pour l'instruction de sous-sélection ; tous les membres frères non mentionnés, dans la clause de l'axe ou du segment, et leurs descendants sont filtrés à partir du sous-espace. De cette manière, l'espace de la sélection externe été limité aux membres existants dans la clause d'axe ou clause de segment, avec leurs ascendants et descendants comme indiqué plus haut.  
  
 Parce que le membre All de toutes les dimensions non mentionnées dans les clauses de l'axe ou du segment appartient à l'espace de la sélection ; tous les descendants du membre All sur ces dimensions font également partie de l'espace de la sous-sélection.  
  
 Le membre All, dans toutes les dimensions, dans l'espace de sous-cube, est réévalué pour refléter l'impact de la contrainte du nouvel espace.  
  
 L'exemple suivant illustre ce qui est indiqué plus haut ; la première expression MDX aide à afficher les valeurs non filtrées dans le cube, la deuxième expression MDX illustre l'effet du filtrage dans la clause de sous-sélection :  
  
```  
SELECT { [Customer].[Customer Geography].[All Customers]  
       , [Customer].[Customer Geography].[Country].&[United States]  
       , [Customer].[Customer Geography].[State-Province].&[OR]&[US]  
       , [Customer].[Customer Geography].[City].&[Portland]&[OR]  
       , [Customer].[Customer Geography].[State-Province].&[WA]&[US]  
       , [Customer].[Customer Geography].[City].&[Seattle]&[WA]  
       } ON 1  
     ,  {[Measures].[Internet Sales Amount], [Measures].[Reseller Sales Amount]} ON 0  
  FROM [Adventure Works]  
```  
  
 Retourne les valeurs suivantes :  
  
||||  
|-|-|-|  
||Internet Sales Amount|Reseller Sales Amount|  
|All Customers|29 358 677,22 $|80 450 596,98 $|  
|United States|9 389 789,51 $|80 450 596,98 $|  
|Oregon|1 170 991,54 $|80 450 596,98 $|  
|Portland|110 649,54 $|80 450 596,98 $|  
|Washington|2 467 248,34 $|80 450 596,98 $|  
|Seattle|75 164,86 $|80 450 596,98 $|  
  
 Dans l'exemple ci-dessus, Seattle est un enfant de Washington, Portland est un enfant d'Oregon, Oregon et Washington sont enfants de United States et United States est un enfant de [Customer Geography]. [All Customers]. Tous les membres indiqués dans cet exemple ont d'autres sœurs qui contribuent à la valeur d'agrégation parent ; c.-à-d. que Spokane, Tacoma et Everett sont villes sœurs de Seattle et toutes contribuent au Montant des ventes sur Internet de Washington. La valeur Reseller Sales Amount est indépendante de l'attribut Customer Geography ; par conséquent, la valeur All est affichée dans les résultats. L'expression MDX suivante illustre l'effet de filtrage de la clause d'instruction de sous-sélection :  
  
```  
SELECT { [Customer].[Customer Geography].[All Customers]  
       , [Customer].[Customer Geography].[Country].&[United States]  
       , [Customer].[Customer Geography].[State-Province].&[OR]&[US]  
       , [Customer].[Customer Geography].[City].&[Portland]&[OR]  
       , [Customer].[Customer Geography].[State-Province].&[WA]&[US]  
       , [Customer].[Customer Geography].[City].&[Seattle]&[WA]  
       } ON 1  
     ,  {[Measures].[Internet Sales Amount], [Measures].[Reseller Sales Amount]} ON 0  
  FROM ( SELECT [Customer].[State-Province].&[WA]&[US] ON 0  
           FROM [Adventure Works]  
        )  
```  
  
 Retourne les valeurs suivantes :  
  
||||  
|-|-|-|  
||Internet Sales Amount|Reseller Sales Amount|  
|All Customers|2 467 248,34 $|80 450 596,98 $|  
|United States|2 467 248,34 $|80 450 596,98 $|  
|Washington|2 467 248,34 $|80 450 596,98 $|  
|Seattle|75 164,86 $|80 450 596,98 $|  
  
 Les résultats ci-dessus indiquent que seuls les ascendants et descendants de Washington State font partie du sous-espace dans lequel l'instruction de sélection externe a été évaluée ; Oregon et Portland ont été supprimés du sous-cube parce qu'Oregon et tous les autres États frères n'ont pas été mentionnés dans l'instruction de sous-sélection lorsque Washington a été indiqué.  
  
 Le membre All a été ajusté pour refléter le filtrage sur Washington ; il n'a pas uniquement été ajusté dans la dimension [Customer Geography Geography] mais dans toutes les autres dimensions croisées avec [Customer Geography]. Toutes les dimensions non croisées avec [Customer Geography] restent dans le sous-cube non modifié.  
  
 Les deux instructions MDX suivantes illustrent comment le membre All dans d'autres dimensions est ajusté pour refléter l'effet de filtrage de l'instruction de sous-sélection. La première requête affiche les résultats non modifiés et la seconde indique l'impact du filtrage :  
  
```  
SELECT { [Customer].[Customer Geography].[All Customers]  
       , [Customer].[Customer Geography].[Country].&[United States]  
       , [Customer].[Customer Geography].[State-Province].&[OR]&[US]  
       , [Customer].[Customer Geography].[City].&[Portland]&[OR]  
       , [Customer].[Customer Geography].[State-Province].&[WA]&[US]  
       , [Customer].[Customer Geography].[City].&[Seattle]&[WA]  
       } ON 1  
     ,   [Product].[Product Line].MEMBERS ON 0  
  FROM [Adventure Works]  
 WHERE [Measures].[Internet Sales Amount]  
```  
  
||||||||  
|-|-|-|-|-|-|-|  
||All Products|Accessory|Components|Mountain|Road|Touring|  
|All Customers|29 358 677,22 $|604 053,30 $|(Null)|10 251 183,52 $|14 624 108,58 $|3 879 331,82 $|  
|United States|9 389 789,51 $|217 168,79 $|(Null)|3 547 956,78 $|4 322 438,41 $|1 302 225,54 $|  
|Oregon|1 170 991,54 $|30 513,17 $|(Null)|443 607,98 $|565 372,10 $|131 498,29 $|  
|Portland|110 649,54 $|2 834,17 $|(Null)|47 099,91 $|53 917,17 $|6 798,29 $|  
|Washington|2 467 248,34 $|62 662,92 $|(Null)|945 219,38 $|1 155 880,07 $|303 485,97 $|  
|Seattle|75 164,86 $|2 695,74 $|(Null)|19 914,53 $|44 820,06 $|7 734,54 $|  
  
```  
SELECT { [Customer].[Customer Geography].[All Customers]  
       , [Customer].[Customer Geography].[Country].&[United States]  
       , [Customer].[Customer Geography].[State-Province].&[OR]&[US]  
       , [Customer].[Customer Geography].[City].&[Portland]&[OR]  
       , [Customer].[Customer Geography].[State-Province].&[WA]&[US]  
       , [Customer].[Customer Geography].[City].&[Seattle]&[WA]  
       } ON 1  
     ,   [Product].[Product Line].MEMBERS ON 0  
  FROM ( SELECT [Customer].[State-Province].&[WA]&[US] ON 0  
           FROM [Adventure Works]  
        )  
 WHERE [Measures].[Internet Sales Amount]  
```  
  
||||||||  
|-|-|-|-|-|-|-|  
||All Products|Accessory|Components|Mountain|Road|Touring|  
|All Customers|2 467 248,34 $|62 662,92 $|(Null)|945 219,38 $|1 155 880,07 $|303 485,97 $|  
|United States|2 467 248,34 $|62 662,92 $|(Null)|945 219,38 $|1 155 880,07 $|303 485,97 $|  
|Washington|2 467 248,34 $|62 662,92 $|(Null)|945 219,38 $|1 155 880,07 $|303 485,97 $|  
|Seattle|75 164,86 $|2 695,74 $|(Null)|19 914,53 $|44 820,06 $|7 734,54 $|  
  
 Les résultats ci-dessus indiquent que les valeurs All Products ont été ajustées uniquement sur les valeurs de Washington State, comme attendu.  
  
 Les instructions de sous-sélection peuvent être imbriquées sans limite de profondeur d'imbrication, sauf en ce qui concerne la mémoire disponible. L'instruction de sous-sélection la plus profonde définit le sous-espace de début auquel le filtrage est appliqué et transmis à la sélection externe suivante. Il est important de remarquer que l'imbrication n'est pas une opération commutative ; donc l'ordre dans lequel l'imbrication est définie peut produire des résultats différents. Les exemples suivants doivent indiquer la différence lors du choix d'un ordre d'imbrication.  
  
```  
SELECT [Sales Territory].[Sales Territory Region].MEMBERS on 0  
     , [Product].[Product].MEMBERS on 1  
  FROM (SELECT TOPCOUNT( [Product].[Product].CHILDREN, 5, [Measures].[Sales Amount]) ON 0  
          FROM (SELECT TOPCOUNT( [Sales Territory].[Sales Territory Region].CHILDREN, 5, [Measures].[Sales Amount]) on 0  
                  FROM [Adventure Works]  
               )  
        )  
 WHERE [Measures].[Sales Amount]  
```  
  
 Retourne les résultats suivants.  
  
||||||||  
|-|-|-|-|-|-|-|  
||All Sales Territories|Australia|Canada|Central|Northwest|Southwest|  
|All Products|7 591 495,49 $|1 281 059,99 $|1 547 298,12 $|600 205,79 $|1 924 763,50 $|2 238 168,08 $|  
|Mountain-200 Silver, 38|1 449 576,15 $|248 702,93 $|275 052,45 $|141 103,65 $|349 487,01 $|435 230,12 $|  
|Mountain-200 Black, 38|1 722 896,50 $|218 024,05 $|418 726,43 $|123 929,46 $|486 694,63 $|475 521,93 $|  
|Mountain-200 Black, 42|1 573 655,14 $|239 137,96 $|319 921,61 $|130 102,75 $|420 445,84 $|464 046,98 $|  
|Mountain-200 Black, 46|1 420 500,58 $|192 320,16 $|230 875,99 $|117 044,49 $|424 813,66 $|455 446,27 $|  
|Road-150 Red, 56|1 424 867,11 $|382 874,89 $|302 721,64 $|88 025,44 $|243 322,36 $|407 922,78 $|  
  
```  
SELECT [Sales Territory].[Sales Territory Region].MEMBERS on 0  
     , [Product].[Product].MEMBERS on 1  
  FROM (SELECT TOPCOUNT( [Sales Territory].[Sales Territory Region].CHILDREN, 5, [Measures].[Sales Amount]) ON 0  
          FROM (SELECT TOPCOUNT( [Product].[Product].CHILDREN, 5, [Measures].[Sales Amount]) on 0  
                  FROM [Adventure Works]  
               )  
        )  
 WHERE [Measures].[Sales Amount]  
```  
  
 Retourne les résultats suivants.  
  
||||||||  
|-|-|-|-|-|-|-|  
||All Sales Territories|Australia|Canada|Northwest|Southwest|United Kingdom|  
|All Products|7 938 218,56 $|1 096 312,24 $|1 474 255,49 $|2 042 674,72 $|2 238 099,55 $|1 086 876,56 $|  
|Mountain-200 Silver, 38|1 520 958,53 $|248 702,93 $|275 052,45 $|349 487,01 $|435 230,12 $|212 486,03 $|  
|Mountain-200 Silver, 42|1 392 237,14 $|198 127,15 $|229 679,01 $|361 233,58 $|407 854,24 $|195 343,16 $|  
|Mountain-200 Black, 38|1 861 703,23 $|218 024,05 $|418 726,43 $|486 694,63 $|475 521,93 $|262 736,19 $|  
|Mountain-200 Black, 42|1 702 427,25 $|239 137,96 $|319 921,61 $|420 445,84 $|464 046,98 $|258 874,87 $|  
|Mountain-200 Black, 46|1 460 892,41 $|192 320,16 $|230 875,99 $|424 813,66 $|455 446,27 $|157 436,31 $|  
  
 Comme vous pouvez le voir, il y a des différences entre les résultats des deux jeux. La première requête a permis d'identifier les produits qui se sont le mieux vendus dans les 5 régions où les meilleures ventes ont été réalisées, la deuxième requête a permis de déterminer où les 5 produits les plus performants ont été vendus.  
  
### <a name="remarks"></a>Notes  
 Les instructions de sous-sélection ont les restrictions et limitations suivantes :  
  
-   La clause WHERE ne filtre pas le sous-espace.  
  
-   La clause WHERE modifie uniquement le membre par défaut dans le sous-cube.  
  
-   La clause NON EMPTY n’est pas autorisée dans une clause d’axe ; utilisez à la place une expression de fonction [NonEmpty &#40;MDX&#41;](../../../mdx/nonempty-mdx.md).  
  
-   La clause HAVING n’est pas autorisée dans une clause d’axe ; utilisez à la place une expression de fonction [Filter &#40;MDX&#41;](../../../mdx/filter-mdx.md).  
  
-   Par défaut les membres calculés ne sont pas autorisés dans les instructions de sous-sélection ; Toutefois, cette restriction peut être modifiée, dans une base par session, en affectant une valeur pour le **sous-requêtes** propriété de chaîne de connexion dans <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString%2A> ou **DBPROP_MSMD_SUBQUERIES** propriété dans [ Les propriétés XMLA prises en charge &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md). Consultez [Membres calculés dans les sous-sélections et les sous-cubes](../../../analysis-services/multidimensional-models/mdx/calculated-members-in-subselects-and-subcubes.md) pour obtenir une explication détaillée du comportement des membres calculés en fonction des valeurs de **SubQueries** ou **DBPROP_MSMD_SUBQUERIES**.  
  
  
