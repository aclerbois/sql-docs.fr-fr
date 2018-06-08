---
title: La requête MDX de base (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7f3a1610f19bc99ce063d815f74906f10ddc2c7f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-query---the-basic-query"></a>Requête MDX - la requête de base
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  La requête de base MDX (Multidimensional Expressions) est l'instruction SELECT — la plus utilisée dans MDX. Si vous connaissez la méthode selon laquelle une instruction SELECT MDX doit spécifier un jeu de résultats, la syntaxe de l'instruction SELECT et la manière de créer une requête simple à l'aide de cette instruction, vous disposez alors de connaissances suffisamment solides pour comprendre comment utiliser MDX pour interroger des données multidimensionnelles.  
  
## <a name="specifying-a-result-set"></a>Spécification d'un jeu de résultats  
 Dans MDX, l'instruction SELECT permet de spécifier un jeu de résultats doté d'un sous-ensemble de données multidimensionnelles qui ont été retournées à partir d'un cube. Pour spécifier un jeu de résultats, une requête MDX doit renfermer les informations suivantes :  
  
-   Le nombre d'axes que le jeu de résultats doit contenir. Vous pouvez spécifier jusqu'à 128 axes dans une requête MDX.  
  
-   Le jeu de membres ou de tuples à inclure sur chaque axe de la requête MDX.  
  
-   Le nom du cube qui définit le contexte de la requête MDX.  
  
-   Le jeu de membres ou de tuples à inclure sur chaque axe de secteur. Pour plus d’informations sur les axes de secteur et de requête, consultez [Restriction de la requête avec des axes de requête et de secteur &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-restricting-the-query.md).  
  
 Pour identifier les axes de requête, le cube qui sera interrogé et l'axe de secteur, l'instruction MDX SELECT utilise les clauses suivantes :  
  
-   Une clause SELECT qui détermine les axes de requête d'une instruction SELECT MDX. Pour plus d’informations sur la construction d’axes de requête dans une clause SELECT, consultez [Spécification du contenu d’un axe de requête &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-specify-the-contents-of-a-query-axis.md).  
  
-   Une clause FROM qui détermine quel cube sera interrogé. Pour plus d’informations sur la clause FROM, consultez [SELECT Statement &#40;MDX&#41;](../../../mdx/mdx-data-manipulation-select.md).  
  
-   Une clause WHERE facultative qui détermine quels membres ou tuples utiliser sur l'axe de secteur pour limiter les données retournées. Pour plus d’informations sur la construction d’un axe de secteur dans une clause WHERE, consultez [Spécification du contenu d’un axe de secteur &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-specify-the-contents-of-a-slicer-axis.md).  
  
> [!NOTE]  
>  Pour plus d’informations détaillées sur les diverses clauses de l’instruction SELECT, consultez [Instruction SELECT &#40;MDX&#41;](../../../mdx/mdx-data-manipulation-select.md).  
  
## <a name="select-statement-syntax"></a>Syntaxe de l'instruction SELECT  
 La syntaxe qui suit illustre une instruction SELECT de base qui inclut l'utilisation des clauses SELECT, FROM et WHERE :  
  
```  
[ WITH <SELECT WITH clause> [ , <SELECT WITH clause> ... ] ]   
SELECT [ * | ( <SELECT query axis clause>   
    [ , <SELECT query axis clause> ... ] ) ]  
FROM <SELECT subcube clause>   
[ <SELECT slicer axis clause> ]  
[ <SELECT cell property list clause> ]  
```  
  
 L'instruction MDX SELECT prend en charge des éléments de syntaxe facultatifs, tels que le mot clé WITH, l'utilisation de fonctions MDX pour créer des membres calculés pour une inclusion dans un axe ou un axe de secteur, et la possibilité de retourner la valeur de propriétés de cellules spécifiques dans le cadre de la requête. Pour plus d’informations sur l’instruction SELECT MDX, consultez [SELECT Statement &#40;MDX&#41;](../../../mdx/mdx-data-manipulation-select.md).  
  
### <a name="comparing-the-syntax-of-the-mdx-select-statement-to-sql"></a>Comparaison de la syntaxe de l'instruction SELECT MDX par rapport à SQL  
 Le format de syntaxe de l'instruction SELECT MDX est semblable à celui de la syntaxe SQL. Il existe toutefois quelques différences fondamentales :  
  
-   La syntaxe MDX distingue les jeux en plaçant les tuples ou les membres dans des accolades (caractères { et }). Pour plus d’informations sur la syntaxe d’un membre, d’un tuple et d’un jeu, consultez [Utilisation de membres, de tuples et de jeux &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/working-with-members-tuples-and-sets-mdx.md).  
  
-   Les requêtes MDX peuvent avoir de 0 à 128 axes de requête dans l'instruction SELECT. Tous les axes se comportent exactement de la même manière, contrairement à SQL où il y a des différences significatives entre la façon dont les lignes et les colonnes d'une requête se comportent.  
  
-   À l'instar d'une requête SQL, la clause FROM nomme la source des données pour la requête MDX. Toutefois, la clause FROM MDX est limitée à un seul cube. Des informations issues d'autres cubes peuvent être extraites, valeur par valeur, à l'aide de la fonction LookupCube.  
  
-   La clause WHERE décrit l'axe de secteur dans une requête MDX. Elle agit comme un axe supplémentaire invisible dans la requête, découpant les valeurs qui s'affichent dans les cellules du jeu de résultats ; contrairement à la clause SQL WHERE, elle n'affecte pas directement ce qui s'affiche sur l'axe des lignes de la requête. Les fonctionnalités de la clause SQL WHERE sont disponibles via d'autres fonctions MDX telles que la fonction FILTER.  
  
## <a name="select-statement-example"></a>Exemple d'instruction SELECT  
 L'exemple qui suit illustre une requête MDX de base qui utilise l'instruction SELECT. Cette requête retourne un jeu de résultats contenant le montant des ventes et des taxes 2002 et 2003 pour les secteurs du sud-ouest.  
  
```  
SELECT  
    { [Measures].[Sales Amount],   
        [Measures].[Tax Amount] } ON COLUMNS,  
    { [Date].[Fiscal].[Fiscal Year].&[2002],   
        [Date].[Fiscal].[Fiscal Year].&[2003] } ON ROWS  
FROM [Adventure Works]  
WHERE ( [Sales Territory].[Southwest] )  
```  
  
 Dans cet exemple, la requête définit les informations de jeu de résultats suivantes :  
  
-   La clause SELECT définit les axes de requête comme les membres Sales Amount et Tax Amount de la dimension Measures, et les membres 2002 et 2003 de la dimension Date.  
  
-   La clause FROM indique que la source de données est le cube Adventure Works.  
  
-   La clause WHERE définit l'axe de secteur comme le membre Southwest de la dimension Sales Territory.  
  
 Notez que cet exemple de requête utilise également les alias des axes COLUMNS et ROWS. Les positions ordinales pour ces axes auraient également pu être utilisées. L'exemple qui suit montre comment la requête MDX pourrait également être écrite, en utilisant la position ordinale de chaque axe :  
  
```  
SELECT  
    { [Measures].[Sales Amount],   
        [Measures].[Tax Amount] } ON 0,  
    { [Date].[Fiscal].[Fiscal Year].&[2002],   
        [Date].[Fiscal].[Fiscal Year].&[2003] } ON 1  
FROM [Adventure Works]  
WHERE ( [Sales Territory].[Southwest] )  
```  
  
 Pour obtenir d’autres exemples détaillés, consultez [Spécification du contenu d’un axe de requête &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-specify-the-contents-of-a-query-axis.md) et [Spécification du contenu d’un axe de secteur &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-specify-the-contents-of-a-slicer-axis.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts clés dans MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md)   
 [Instruction SELECT & #40 ; MDX & #41 ;](../../../mdx/mdx-data-manipulation-select.md)  
  
  
