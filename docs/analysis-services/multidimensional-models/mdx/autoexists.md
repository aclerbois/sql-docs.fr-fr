---
title: La fonctionnalité Autoexists | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5f7c111f85e3b43a560b70171f8af470a9fc505a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="autoexists"></a>Autoexists
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  La fonctionnalité d’auto-existence, ou *autoexists* , limite l’espace du cube aux cellules qui existent réellement dans le cube, par opposition à celles qui pourraient exister en créant toutes les combinaisons possibles de membres de la hiérarchie d’attribut à partir de la même hiérarchie. En effet, les membres d'une hiérarchie d'attribut ne peuvent coexister avec les membres d'une autre hiérarchie d'attribut au sein de la même dimension. Lorsque deux hiérarchies d'attribut, ou plus, de la même dimension sont utilisées dans une instruction SELECT, Analysis Services évalue les expressions des attributs pour s'assurer que les membres de ces attributs sont correctement limités afin de répondre aux critères de tous les autres attributs.  
  
 Supposons, par exemple, que vous utilisez des attributs de la dimension de Geography. Si vous avez une expression qui retourne tous les membres de l'attribut Ville et une autre expression qui limite les membres de l'attribut Pays à tous les pays d'Europe, il en résultera une limitation des membres de l'attribut Ville aux seules villes qui appartiennent à des pays d'Europe. Cela est dû à la particularité de la fonctionnalité Autoexists d'Analysis Services. En effet, elle tente d'empêcher que des enregistrements de la dimension exclus d'une expression d'attribut soient exclus par les autres expressions d'attributs. La fonctionnalité Autoexists peut également être interprétée comme l'intersection obtenue entre les différentes expressions d'attributs sur les lignes de la dimension.  
  
## <a name="cell-existence"></a>Existence des cellules  
 Les cellules suivantes existent toujours :  
  
-   Le membre (All), de chaque hiérarchie, lorsqu'il est croisé avec des membres d'autres hiérarchies dans la même dimension.  
  
-   Les membres calculés lorsqu'ils sont croisés avec leurs frères et sœurs non calculés, ou avec les parents ou descendants de leurs frères et sœurs non calculés.  
  
## <a name="providing-non-existing-cells"></a>Création de cellules inexistantes  
 Une cellule inexistante est une cellule fournie par le système en réponse à une requête ou à un calcul qui demande une cellule qui n'existe pas dans le cube. Par exemple, si vous travaillez avec un cube muni d'une hiérarchie d'attribut Ville, d'une hiérarchie d'attribut Pays qui appartient à la dimension Zone géographique, et d'une mesure Montant des ventes sur Internet, l'espace de ce cube inclut uniquement les membres qui coexistent entre eux. Par exemple, si la hiérarchie d'attribut Ville inclut les villes de New York, Londres, Paris, Tokyo et Melbourne et la hiérarchie d'attribut Pays les pays États-Unis, Grande-Bretagne, France, Japon et Australie, l'espace du cube n'inclut pas l'espace (cellule) à l'intersection de Paris et États-Unis.  
  
 Lorsque vous interrogez des cellules qui n'existent pas, ces cellules retournent des valeurs NULL, ce qui signifie qu'elles ne peuvent pas contenir des calculs et que vous ne pouvez pas définir un calcul autorisé à écrire dans l'espace concerné. Par exemple, l'instruction suivante inclut des cellules qui n'existent pas :  
  
```  
SELECT [Customer].[Gender].[Gender].Members ON COLUMNS,  
{[Customer].[Customer].[Aaron A. Allen]  
   ,[Customer].[Customer].[Abigail Clark]} ON ROWS   
FROM [Adventure Works]  
WHERE Measures.[Internet Sales Amount]  
```  
  
> [!NOTE]  
>  Cette requête utilise la fonction [Members (Set) (MDX)](../../../mdx/members-set-mdx.md) pour retourner l’ensemble de membres de la hiérarchie d’attribut Gender sur l’axe des colonnes, puis croise cet ensemble avec l’ensemble de membres spécifié à partir de la hiérarchie d’attribut Customer sur l’axe des lignes.  
  
 Lorsque vous exécutez la requête ci-dessus, la cellule à l'intersection de Aaron A. Allen et Female affiche une valeur NULL, tout comme la cellule à l'intersection d'Abigail Clark et Male. Ces cellules n'existent pas et ne peuvent contenir de valeur. En revanche, les cellules non existantes peuvent apparaître dans le résultat que retourne une requête.  
  
 Quand vous utilisez la fonction [Crossjoin (MDX)](../../../mdx/crossjoin-mdx.md) pour retourner le produit croisé des membres de la hiérarchie d’attributs à partir des hiérarchies d’attributs de la même dimension, l’existence automatique limite les tuples retournés à l’ensemble des tuples qui existent réellement, au lieu de retourner un produit cartésien complet. Par exemple, exécutez et examinez les résultats de l'exécution de la requête suivante :  
  
```  
SELECT CROSSJOIN  
   (  
      {[Customer].[Country].[United States]},  
         [Customer].[State-Province].Members  
  ) ON 0   
FROM [Adventure Works]  
WHERE Measures.[Internet Sales Amount]  
```  
  
> [!NOTE]  
>  Remarquez l'emploi de 0, soit la formule abrégée de Axes(0), pour désigner l'axe des colonnes.  
  
 La requête ci-dessus retourne uniquement les cellules des membres de chaque hiérarchie d'attribut dans la requête qui coexistent entre eux. Vous pouvez également écrire cette requête en utilisant la nouvelle variante * de la fonction [* (Crossjoin) (MDX)](../../../mdx/crossjoin-mdx-operator-reference.md) .  
  
```  
SELECT   
   [Customer].[Country].[United States] *   
      [Customer].[State-Province].Members  
ON 0   
FROM [Adventure Works]  
WHERE Measures.[Internet Sales Amount]  
```  
  
 Cette requête peut aussi être écrite de la manière suivante :  
  
```  
SELECT [Customer].[State-Province].Members  
ON 0   
FROM [Adventure Works]  
WHERE (Measures.[Internet Sales Amount],  
   [Customer].[Country].[United States])  
```  
  
 Les valeurs retournées des cellules sont identiques, même si les métadonnées dans l'ensemble de résultats apparaissent différemment. Par exemple, dans la requête ci-dessus, la hiérarchie Country a été déplacée vers l'axe de segment (dans la clause WHERE) et ne peut donc s'afficher de manière explicite dans l'ensemble de résultats.  
  
 Chacune des trois requêtes présentées ci-dessus illustre les effets du comportement de l’auto-existence dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
## <a name="deep-and-shallow-autoexists"></a>Fonctionnalités Deep Autoexists et Shallow Autoexists  
 Autoexists peut être appliquée en profondeur ou superficiellement aux expressions. La fonctionnalité**Deep Autoexists** signifie que toutes les expressions sont évaluées pour rencontrer l’espace le plus profond possible après l’application des expressions de découpage, des expressions de sous-sélection dans l’axe, etc. La fonctionnalité**Shallow Autoexists** permet d’évaluer les expressions externes avant l’expression actuelle et de passer ces résultats à l’expression actuelle. La fonctionnalité Deep Autoexists est paramétrée par défaut.  
  
 Le scénario et les exemples suivants illustrent les différents types de fonctionnalités Autoexists. Dans les exemples suivants, nous allons créer deux jeux : l'un sous forme d'expression calculée et l'autre sous forme d'expression constante.  
  
 `//Obtain the Top 10 best reseller selling products by Name`  
  
 `with member [Measures].[PCT Discount] AS '[Measures].[Discount Amount]/[Measures].[Reseller Sales Amount]', FORMAT_STRING = 'Percent'`  
  
 `set Top10SellingProducts as 'topcount([Product].[Model Name].children, 10, [Measures].[Reseller Sales Amount])'`  
  
 `set Preferred10Products as '`  
  
 `{[Product].[Model Name].&[Mountain-200],`  
  
 `[Product].[Model Name].&[Road-250],`  
  
 `[Product].[Model Name].&[Mountain-100],`  
  
 `[Product].[Model Name].&[Road-650],`  
  
 `[Product].[Model Name].&[Touring-1000],`  
  
 `[Product].[Model Name].&[Road-550-W],`  
  
 `[Product].[Model Name].&[Road-350-W],`  
  
 `[Product].[Model Name].&[HL Mountain Frame],`  
  
 `[Product].[Model Name].&[Road-150],`  
  
 `[Product].[Model Name].&[Touring-3000]`  
  
 `}'`  
  
 `select {[Measures].[Reseller Sales Amount], [Measures].[Discount Amount], [Measures].[PCT Discount]} on 0,`  
  
 `Top10SellingProducts on 1`  
  
 `from [Adventure Works]`  
  
 Le jeu de résultats obtenu est le suivant :  
  
|||||  
|-|-|-|-|  
||**Reseller Sales Amount**|**Discount Amount**|**PCT Discount**|  
|**Mountain-200**|**14 356 699,36 $**|**19 012,71 $**|**0.13**|  
|**Road-250**|**9 377 457,68 $**|**4 032,47 $**|**0.04%**|  
|**Mountain-100**|**8 568 958,27 $**|**139 393,27 $**|**1.63**|  
|**Road-650**|**7 442 141,81 $**|**39 698,30 $**|**0.53**|  
|**Touring-1000**|**6 723 794,29 $**|**166 144,17 $**|**2.47**|  
|**Road-550-W**|**3 668 383,88 $**|**1 901,97 $**|**0.05**|  
|**Road-350-W**|**3 665 932,31 $**|**20 946,50 $**|**0.57%**|  
|**HL Mountain Frame**|**3 365 069,27 $**|**$174.11**|**0.01**|  
|**Road-150**|**2 363 805,16 $**|**$0.00**|**0.00%**|  
|**Touring-3000**|**2 046 508,26 $**|**79 582,15 $**|**3.89%**|  
  
 Le jeu de produits obtenu semble être le même que jeu Preferred10Products, qui se présente ainsi :  
  
 `with member [Measures].[PCT Discount] AS '[Measures].[Discount Amount]/[Measures].[Reseller Sales Amount]', FORMAT_STRING = 'Percent'`  
  
 `set Top10SellingProducts as 'topcount([Product].[Model Name].children, 10, [Measures].[Reseller Sales Amount])'`  
  
 `set Preferred10Products as '`  
  
 `{[Product].[Model Name].&[Mountain-200],`  
  
 `[Product].[Model Name].&[Road-250],`  
  
 `[Product].[Model Name].&[Mountain-100],`  
  
 `[Product].[Model Name].&[Road-650],`  
  
 `[Product].[Model Name].&[Touring-1000],`  
  
 `[Product].[Model Name].&[Road-550-W],`  
  
 `[Product].[Model Name].&[Road-350-W],`  
  
 `[Product].[Model Name].&[HL Mountain Frame],`  
  
 `[Product].[Model Name].&[Road-150],`  
  
 `[Product].[Model Name].&[Touring-3000]`  
  
 `}'`  
  
 `select {[Measures].[Reseller Sales Amount], [Measures].[Discount Amount], [Measures].[PCT Discount]} on 0,`  
  
 `Preferred10Products on 1`  
  
 `from [Adventure Works]`  
  
 Conformément aux résultats suivants, les deux jeux (Top10SellingProducts, Preferred10Products) sont identiques :  
  
|||||  
|-|-|-|-|  
||**Reseller Sales Amount**|**Discount Amount**|**PCT Discount**|  
|**Mountain-200**|**14 356 699,36 $**|**19 012,71 $**|**0.13**|  
|**Road-250**|**9 377 457,68 $**|**4 032,47 $**|**0.04%**|  
|**Mountain-100**|**8 568 958,27 $**|**139 393,27 $**|**1.63**|  
|**Road-650**|**7 442 141,81 $**|**39 698,30 $**|**0.53**|  
|**Touring-1000**|**6 723 794,29 $**|**166 144,17 $**|**2.47**|  
|**Road-550-W**|**3 668 383,88 $**|**1 901,97 $**|**0.05**|  
|**Road-350-W**|**3 665 932,31 $**|**20 946,50 $**|**0.57%**|  
|**HL Mountain Frame**|**3 365 069,27 $**|**$174.11**|**0.01**|  
|**Road-150**|**2 363 805,16 $**|**$0.00**|**0.00%**|  
|**Touring-3000**|**2 046 508,26 $**|**79 582,15 $**|**3.89%**|  
  
 L'exemple suivant illustre le concept de la fonctionnalité Deep Autoexists. Dans l'exemple, nous filtrons Top10SellingProducts sur l'attribut [Product].[Product Line] pour les membres du groupe [Mountain]. Notez que les deux attributs (slicer et axis) appartiennent à la même dimension, [Product].  
  
 `with member [Measures].[PCT Discount] AS '[Measures].[Discount Amount]/[Measures].[Reseller Sales Amount]', FORMAT_STRING = 'Percent'`  
  
 `set Top10SellingProducts as 'topcount([Product].[Model Name].children, 10, [Measures].[Reseller Sales Amount])'`  
  
 `// Preferred10Products set removed for clarity`  
  
 `select {[Measures].[Reseller Sales Amount], [Measures].[Discount Amount], [Measures].[PCT Discount]} on 0,`  
  
 `Top10SellingProducts on 1`  
  
 `from [Adventure Works]`  
  
 `where [Product].[Product Line].[Mountain]`  
  
 Génère les jeux de résultats suivants :  
  
|||||  
|-|-|-|-|  
||**Reseller Sales Amount**|**Discount Amount**|**PCT Discount**|  
|**Mountain-200**|**14 356 699,36 $**|**19 012,71 $**|**0.13**|  
|**Mountain-100**|**8 568 958,27 $**|**139 393,27 $**|**1.63**|  
|**HL Mountain Frame**|**3 365 069,27 $**|**$174.11**|**0.01**|  
|**Mountain-300**|**1 907 249,38 $**|**$876.95**|**0.05**|  
|**Mountain-500**|**1 067 327,31 $**|**17 266,09 $**|**1.62**|  
|**Mountain-400-W**|**592 450,05 $**|**$303.49**|**0.05**|  
|**LL Mountain Frame**|**521 864,42 $**|**$252.41**|**0.05**|  
|**ML Mountain Frame-W**|**482 953,16 $**|**$206.95**|**0.04%**|  
|**ML Mountain Frame**|**343 785,29 $**|**$161.82**|**0.05**|  
|**Women's Mountain Shorts**|**260 304,09 $**|**6 675,56 $**|**2.56**|  
  
 Dans le jeu de résultats ci-dessus, nous avons sept nouveaux venus dans la liste des articles Top10SellingProducts ; de plus, Mountain-200, Mountain-100 et HL Mountain Frame ont été déplacés en haut de la liste. Dans le jeu de résultats précédent, ces trois valeurs étaient entrecoupées.  
  
 On parle alors de fonctionnalité Deep Autoexists, car le jeu Top10SellingProducts est évalué pour répondre aux conditions de découpage de la requête. La fonctionnalité Deep Autoexists signifie que toutes les expressions seront évaluées pour rencontrer l'espace le plus profond possible après l'application des expressions de découpage, des expressions de sous-sélection dans l'axe, etc.  
  
 Toutefois, vous pouvez souhaiter effectuer l'analyse sur Top10SellingProducts comme équivalent à Preferred10Products, comme dans l'exemple suivant :  
  
 `with member [Measures].[PCT Discount] AS '[Measures].[Discount Amount]/[Measures].[Reseller Sales Amount]', FORMAT_STRING = 'Percent'`  
  
 `set Top10SellingProducts as 'topcount([Product].[Model Name].children, 10, [Measures].[Reseller Sales Amount])'`  
  
 `set Preferred10Products as '`  
  
 `{[Product].[Model Name].&[Mountain-200],`  
  
 `[Product].[Model Name].&[Road-250],`  
  
 `[Product].[Model Name].&[Mountain-100],`  
  
 `[Product].[Model Name].&[Road-650],`  
  
 `[Product].[Model Name].&[Touring-1000],`  
  
 `[Product].[Model Name].&[Road-550-W],`  
  
 `[Product].[Model Name].&[Road-350-W],`  
  
 `[Product].[Model Name].&[HL Mountain Frame],`  
  
 `[Product].[Model Name].&[Road-150],`  
  
 `[Product].[Model Name].&[Touring-3000]`  
  
 `}'`  
  
 `select {[Measures].[Reseller Sales Amount], [Measures].[Discount Amount], [Measures].[PCT Discount]} on 0,`  
  
 `Preferred10Products on 1`  
  
 `from [Adventure Works]`  
  
 `where [Product].[Product Line].[Mountain]`  
  
 Génère les jeux de résultats suivants :  
  
|||||  
|-|-|-|-|  
||**Reseller Sales Amount**|**Discount Amount**|**PCT Discount**|  
|**Mountain-200**|**14 356 699,36 $**|**19 012,71 $**|**0.13**|  
|**Mountain-100**|**8 568 958,27 $**|**139 393,27 $**|**1.63**|  
|**HL Mountain Frame**|**3 365 069,27 $**|**$174.11**|**0.01**|  
  
 Dans les résultats ci-dessus, le découpage donne un résultat qui contient uniquement les produits de Preferred10Products qui font partie du groupe [Mountain] dans [Product].[Product Line], comme prévu car Preferred10Products est une expression constante.  
  
 Ce jeu de résultats est également interprété comme une fonctionnalité Shallow Autoexists. En effet, l'expression est évaluée avant la clause de découpage. Dans l'exemple précédent, l'expression était une expression constante à des fins d'illustration pour présenter le concept.  
  
 Le comportement d’Autoexists peut être modifié au niveau de la session à l’aide de la propriété de chaîne de connexion **Autoexists** . L’exemple suivant commence en ouvrant une nouvelle session et en ajoutant la propriété *Autoexists=3* à la chaîne de connexion. Vous devez ouvrir une nouvelle connexion pour exécuter l'exemple. Une fois la connexion établie avec le paramètre Autoexists, elle reste effective jusqu'à ce qu'elle soit interrompue.  
  
 `with member [Measures].[PCT Discount] AS '[Measures].[Discount Amount]/[Measures].[Reseller Sales Amount]', FORMAT_STRING = 'Percent'`  
  
 `set Top10SellingProducts as 'topcount([Product].[Model Name].children, 10, [Measures].[Reseller Sales Amount])'`  
  
 `//Preferred10Products set removed for clarity`  
  
 `select {[Measures].[Reseller Sales Amount], [Measures].[Discount Amount], [Measures].[PCT Discount]} on 0,`  
  
 `Top10SellingProducts on 1`  
  
 `from [Adventure Works]`  
  
 `where [Product].[Product Line].[Mountain]`  
  
 Le jeu de résultats suivant présente maintenant le comportement superficiel d'Autoexists.  
  
|||||  
|-|-|-|-|  
||**Reseller Sales Amount**|**Discount Amount**|**PCT Discount**|  
|**Mountain-200**|**14 356 699,36 $**|**19 012,71 $**|**0.13**|  
|**Mountain-100**|**8 568 958,27 $**|**139 393,27 $**|**1.63**|  
|**HL Mountain Frame**|**3 365 069,27 $**|**$174.11**|**0.01**|  
  
 Comportement d’Autoexists peut être modifié à l’aide de la fonctionnalité AUTOEXISTS = [1 | 2 | paramètre 3] dans la chaîne de connexion ; consultez [pris en charge les propriétés XMLA &#40;XMLA&#41; ](../../../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md) et <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString%2A> pour l’utilisation des paramètres.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts clés dans MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md)   
 [Espace du cube](../../../analysis-services/multidimensional-models/mdx/cube-space.md)   
 [Tuples](../../../analysis-services/multidimensional-models/mdx/tuples.md)   
 [Utilisation des membres, Tuples et jeux & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/working-with-members-tuples-and-sets-mdx.md)   
 [Valeurs totales et Non visibles](../../../analysis-services/multidimensional-models/mdx/visual-totals-and-non-visual-totals.md)   
 [Référence du langage MDX & #40 ; MDX & #41 ;](../../../mdx/mdx-language-reference-mdx.md)   
 [Expressions multidimensionnelles & #40 ; MDX & #41 ; Référence](../../../mdx/multidimensional-expressions-mdx-reference.md)  
  
  
