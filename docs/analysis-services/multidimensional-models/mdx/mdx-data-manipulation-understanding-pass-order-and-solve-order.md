---
title: Présentation des ordre de passage et résoudre Order (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b4b865293cb9c76fb46e8fe12befb2a000d21907
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-data-manipulation---understanding-pass-order-and-solve-order"></a>Manipulation de données MDX - présentation de passage de commande et l’ordre de résolution
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Lorsqu'un cube est calculé dans le cadre d'un script MDX, il peut subir de nombreuses étapes de calcul, en fonction de l'utilisation de diverses fonctionnalités liées au calcul. Chacune de ces étapes porte le nom de test de calcul.  
  
 Un test de calcul peut être désigné par sa position ordinale, appelée numéro du test de calcul. Le nombre de tests nécessaires au calcul complet de toutes les cellules d'un cube représente la profondeur du test de calcul du cube.  
  
 Les données de table de faits et d'écriture différée affectent uniquement le test 0. Les scripts remplissent les données après le test 0 ; chaque instruction d'assignation et de calcul d'un script crée un test. En dehors du script MDX, les références au test absolu 0 correspondent au dernier test créé par le script pour le cube.  
  
 Des membres calculés sont créés à tous les tests, mais l'expression est appliquée au test actuel. Les tests antérieurs contiennent la mesure calculée, mais avec une valeur Null.  
  
## <a name="solve-order"></a>Ordre de résolution  
 L'ordre de résolution détermine la priorité de calcul en cas de concurrence entre des expressions. Au sein d'un même test de calcul, l'ordre de résolution détermine deux choses :  
  
-   l’ordre dans lequel [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] évalue les dimensions, les membres, les membres calculés, les cumuls personnalisés et les cellules calculées ;  
  
-   l’ordre dans lequel [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] évalue les membres personnalisés, les membres calculés, les cumuls personnalisés et les cellules calculées.  
  
 Le membre qui a l'ordre de résolution le plus élevé est prioritaire.  
  
> [!NOTE]  
>  L'exception à cette règle de priorité concerne la fonction d'agrégation. Les membres calculés avec la fonction d'agrégation ont un ordre de résolution inférieur à toute mesure calculée à intersection.  
  
## <a name="solve-order-values-and-precedence"></a>Valeurs et priorité de l'ordre de résolution  
 Les valeurs d'ordre de résolution sont comprises entre -8 181 et 65 535. Dans cette plage, certaines valeurs d'ordre de résolution correspondent à des types de calculs spécifiques, comme illustré dans le tableau suivant.  
  
|Calcul|Ordre de résolution|  
|-----------------|-----------------|  
|Formules de membres personnalisées|-5119|  
|Opérateurs unaires|-5119|  
|Calcul des valeurs totales affichées|-4096|  
|Tous les autres calculs (sauf indication contraire)|0|  
  
 Il est vivement recommandé d'utiliser uniquement des entiers positifs lors de la définition des valeurs d'ordre de résolution. Si vous assignez des valeurs inférieures aux valeurs d'ordre de résolution mentionnées dans le tableau précédent, le test de calcul peut devenir imprévisible. Par exemple, le calcul d'un membre calculé reçoit une valeur d'ordre de résolution qui est inférieure à la valeur de la formule de cumul personnalisé par défaut, à savoir -5 119. Une valeur d'ordre de résolution aussi faible entraîne le calcul des membres calculés avant le calcul des formules de cumul personnalisé, ce qui peut produire des résultats incorrects.  
  
### <a name="creating-and-changing-solve-order"></a>Création et modification de l'ordre de résolution  
 Dans le Concepteur de cube, dans le **volet Calculs**, vous pouvez modifier l’ordre de résolution des membres calculés et des cellules calculées en modifiant l’ordre des calculs.  
  
 Dans MDX, vous pouvez utiliser le mot clé **SOLVE_ORDER** pour créer ou modifier des membres calculés et des cellules calculées.  
  
## <a name="solve-order-examples"></a>Exemples d'ordres de résolution  
 Afin d'illustrer la complexité potentielle de l'ordre de résolution, la série suivante de requêtes MDX commence par deux requêtes qui ne présentent individuellement aucun problème d'ordre de résolution. Ces deux requêtes sont ensuite combinées dans une requête qui nécessite un ordre de résolution.  
  
> [!NOTE]  
>  Vous pouvez exécuter ces requêtes MDX sur l'exemple de base de données multidimensionnelles Adventure Works. Vous pouvez télécharger l’exemple de [modèles multidimensionnels AdventureWorks SQL Server 2012](http://msftdbprodsamples.codeplex.com/releases/view/55330) à partir du site web Codeplex.  
  
### <a name="query-1differences-in-income-and-expenses"></a>Requête 1—Différences entre les revenus et les dépenses  
 Pour la première requête MDX, calculez la différence entre les ventes et les coûts pour chaque année en construisant une requête MDX simple semblable à l'exemple suivant :  
  
```  
WITH   
MEMBER  
[Date].[Calendar].[Year Difference] AS ([Date].[Calendar].[Calendar Year].&[2008] - [Date].[Calendar].[Calendar Year].&[2007] )  
SELECT   
{[Measures].[Internet Sales Amount], [Measures].[Internet Total Product Cost] }  
ON COLUMNS ,  
{ [Date].[Calendar].[Calendar Year].&[2007], [Date].[Calendar].[Calendar Year].&[2008], [Date].[Year Difference] }  
ON ROWS  
FROM [Adventure Works]  
```  
  
 Dans cette requête, il n'existe qu'un seul membre calculé, `Year Difference`. L'ordre de résolution ne pose par conséquent aucun problème, tant que le cube n'utilise pas de membre calculé.  
  
 Cette requête MDX produit un jeu de résultats semblable au tableau suivant.  
  
||Internet Sales Amount|Internet Total Product Cost|  
|-|---------------------------|---------------------------------|  
|**CY 2007**|$9,791,060.30|$5,718,327.17|  
|**CY 2008**|$9,770,899.74|$5,721,205.24|  
|**Year Difference**|($20,160.56)|$2,878.06|  
  
### <a name="query-2percentage-of-income-after-expenses"></a>Requête 2—Pourcentage de revenus après dépenses  
 Pour la seconde requête, calculez le pourcentage de revenus après dépenses pour chaque année à l'aide de la requête MDX suivante :  
  
```  
WITH   
MEMBER  
[Measures].[Profit Margin] AS   
([Measures].[Internet Sales Amount] - [Measures].[Internet Total Product Cost] ) / [Measures].[Internet Sales Amount] ,  
FORMAT_STRING="Percent"  
SELECT   
{[Measures].[Internet Sales Amount], [Measures].[Internet Total Product Cost], [Measures].[Profit Margin] }  
ON COLUMNS ,  
{ [Date].[Calendar].[Calendar Year].&[2007], [Date].[Calendar].[Calendar Year].&[2008] }  
ON ROWS  
FROM [Adventure Works]  
```  
  
 Cette requête MDX, comme la précédente, ne contient qu'un seul membre calculé, `Profit Margin`; elle ne présente donc aucune complication d'ordre de résolution.  
  
 Cette requête MDX produit un jeu de résultats légèrement différent, semblable au tableau suivant.  
  
||Internet Sales Amount|Internet Total Product Cost|Marge de bénéfice|  
|-|---------------------------|---------------------------------|-------------------|  
|**CY 2007**|$9,791,060.30|$5,718,327.17|41.60%|  
|**CY 2008**|$9,770,899.74|$5,721,205.24|41.45%|  
  
 La différence entre les jeux de résultats des deux requêtes provient de la différence d'emplacement du membre calculé. Dans la première requête, le membre calculé fait partie de l'axe ROWS, et non de l'axe COLUMNS illustré dans la deuxième requête. Cette différence d'emplacement devient importante dans la requête suivante, qui combine les deux membres calculés dans une même requête MDX.  
  
### <a name="query-3combined-year-difference-and-net-income-calculations"></a>Requête 3—Calculs combinés de différence sur l'année et de revenus nets  
 Dans cette dernière requête combinant les deux exemples précédents en une seule requête MDX, l'ordre de résolution devient important en raison des calculs à la fois sur les colonnes et sur les lignes. Pour faire en sorte que les calculs s’exécutent dans le bon ordre, définissez l’ordre d’exécution des calculs à l’aide du mot clé **SOLVE_ORDER** .  
  
 Le mot clé **SOLVE_ORDER** spécifie l’ordre de résolution des membres calculés dans une requête MDX ou une commande **CREATE MEMBER** . Les valeurs entières utilisées avec le mot clé **SOLVE_ORDER** sont relatives, elles n’ont pas besoin de commencer à zéro ni d’être consécutives. La valeur indique simplement à la requête MDX de calculer un membre en fonction des valeurs dérivées du calcul des membres ayant une valeur supérieure. Si un membre calculé est défini sans le mot clé **SOLVE_ORDER** , la valeur par défaut de ce membre calculé est zéro.  
  
 Par exemple, si vous combinez les calculs utilisés dans les deux premiers exemples de requêtes, les deux membres calculés, `Year Difference` et `Profit Margin`, se croisent à une cellule unique dans le dataset des résultats de l'exemple de requête MDX. Seul l’ordre de résolution permet de déterminer la façon dont [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] évaluera cette cellule. Les formules utilisées pour construire cette cellule produiront différents résultats, en fonction de l'ordre de résolution des deux membres calculés.  
  
 Tout d'abord, essayez de combiner les calculs utilisés dans les deux premières requêtes dans la requête MDX suivante :  
  
```  
WITH   
MEMBER  
[Date].[Calendar].[Year Difference] AS ([Date].[Calendar].[Calendar Year].&[2008] - [Date].[Calendar].[Calendar Year].&[2007] ) ,  
SOLVE_ORDER = 1  
MEMBER  
[Measures].[Profit Margin] AS   
( [Measures].[Internet Sales Amount] - [Measures].[Internet Total Product Cost] ) / [Measures].[Internet Sales Amount] ,  
FORMAT_STRING="Percent" ,  
SOLVE_ORDER = 2  
SELECT   
{[Measures].[Internet Sales Amount], [Measures].[Internet Total Product Cost], [Measures].[Profit Margin] }  
ON COLUMNS ,  
{ [Date].[Calendar].[Calendar Year].&[2007], [Date].[Calendar].[Calendar Year].&[2008], [Date].[Year Difference] }  
ON ROWS  
FROM [Adventure Works]  
```  
  
 Dans cet exemple de requête MDX combinée, `Profit Margin` a l’ordre de résolution le plus élevé ; il est donc prioritaire quand les deux expressions interagissent. [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] évalue la cellule en question à l'aide de la formule `Profit Margin`. Le tableau suivant présente les résultats de ces calculs imbriqués.  
  
||Internet Sales Amount|Internet Total Product Cost|Marge de bénéfice|  
|-|---------------------------|---------------------------------|-------------------|  
|**CY 2007**|$9,791,060.30|$5,718,327.17|41.60%|  
|**CY 2008**|$9,770,899.74|$5,721,205.24|41.45%|  
|**Year Difference**|($20,160.56)|$2,878.06|114.28%|  
  
 Le résultat mentionné dans la cellule partagée est basé sur la formule de `Profit Margin`. Autrement dit, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] calcule le résultat de la cellule partagée avec les données `Year Difference` , ce qui donne la formule suivante (le résultat est arrondi pour plus de clarté) :  
  
```  
((9,770,899.74 - 9,791,060.30) - (5,721,205.24 - 5,718,327.17)) / (9,770,899.74 - 9,791,060.30) = 1.14275744   
```  
  
 ou  
  
```  
(23,038.63) / (20,160.56) = 114.28%  
```  
  
 Ce résultat est clairement incorrect. Cependant, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] calcule le résultat de la cellule partagée différemment si vous changez les ordres de résolution des membres calculés dans la requête MDX. La requête MDX combinée suivante inverse l'ordre de résolution des membres calculés :  
  
```  
WITH   
MEMBER  
[Date].[Calendar].[Year Difference] AS ([Date].[Calendar].[Calendar Year].&[2008] - [Date].[Calendar].[Calendar Year].&[2007] ) ,  
SOLVE_ORDER = 2  
MEMBER  
[Measures].[Profit Margin] AS   
( [Measures].[Internet Sales Amount] - [Measures].[Internet Total Product Cost] ) / [Measures].[Internet Sales Amount] ,  
FORMAT_STRING="Percent" ,  
SOLVE_ORDER = 1  
SELECT   
{[Measures].[Internet Sales Amount], [Measures].[Internet Total Product Cost], [Measures].[Profit Margin] }  
ON COLUMNS ,  
{ [Date].[Calendar].[Calendar Year].&[2007], [Date].[Calendar].[Calendar Year].&[2008], [Date].[Year Difference] }  
ON ROWS  
FROM [Adventure Works]  
```  
  
 L’ordre de résolution des membres calculés ayant été modifié, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] utilise la formule `Year Difference` pour évaluer la cellule, comme illustré dans le tableau suivant.  
  
||Internet Sales Amount|Internet Total Product Cost|Marge de bénéfice|  
|-|---------------------------|---------------------------------|-------------------|  
|**CY 2007**|$9,791,060.30|$5,718,327.17|41.60%|  
|**CY 2008**|$9,770,899.74|$5,721,205.24|41.45%|  
|**Year Difference**|($20,160.56)|$2,878.06|(0.15%)|  
  
 Comme cette requête utilise la formule `Year Difference` avec les données `Profit Margin` , la formule de la cellule partagée ressemble au calcul suivant :  
  
```  
(($9,770,899.74 - 5,721,205.24) / $9,770,899.74) - ((9,791,060.30 - 5,718,327.17) / 9,791,060.30) = -0.15   
```  
  
 ou  
  
```  
0.4145 - 0.4160= -0.15  
```  
  
## <a name="additional-considerations"></a>Considérations supplémentaires  
 L'ordre de résolution peut s'avérer très délicat à gérer, notamment dans les cubes qui contiennent de très nombreuses dimensions impliquant des membres calculés, des formules de cumul personnalisé ou des cellules calculées. Au moment où [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] évalue une requête MDX, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] prend en compte les valeurs d'ordre de résolution de tous les éléments impliqués dans un test de calcul donné, y compris les dimensions du cube spécifié dans la requête MDX.  
  
## <a name="see-also"></a>Voir aussi  
 [CalculationCurrentPass & #40 ; MDX & #41 ;](../../../mdx/calculationcurrentpass-mdx.md)   
 [CalculationPassValue & #40 ; MDX & #41 ;](../../../mdx/calculationpassvalue-mdx.md)   
 [CRÉER une instruction MEMBER & #40 ; MDX & #41 ;](../../../mdx/mdx-data-definition-create-member.md)   
 [Manipulation de données & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-data-manipulation-manipulating-data.md)  
  
  
