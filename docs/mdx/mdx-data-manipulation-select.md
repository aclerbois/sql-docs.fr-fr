---
title: Instruction SELECT (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 787bd07976f7472ae5f86c347c5ba06493414d7a
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579981"
---
# <a name="mdx-data-manipulation---select"></a>Manipulation de données MDX - SELECT
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Récupère les données d'un cube spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
[ WITH <SELECT WITH clause>   
   [ , <SELECT WITH clause>...n ]   
]   
SELECT   
     [ *   
    | ( <SELECT query axis clause>   
                  [ , <SELECT query axis clause>,...n ]   
            )   
            ]  
FROM   
   <SELECT subcube clause>   
      [ <SELECT slicer axis clause> ]  
      [ <SELECT cell property list clause> ]  
  
<SELECT WITH clause> ::=  
     ( CELL CALCULATION <CREATE CELL CALCULATION body clause> )   
   | ( [ CALCULATED ] MEMBER <CREATE MEMBER body clause>)   
   | ( SET <CREATE SET body clause>)  
   | ( MEASURE = <measure body clause> )  
  
<SELECT query axis clause> ::=  
   [ NON EMPTY ] Set_Expression  
   [ <SELECT dimension property list clause> ]   
      ON   
            Integer_Expression   
       | AXIS(Integer)   
       | COLUMNS   
       | ROWS   
       | PAGES   
       | SECTIONS   
       | CHAPTERS   
  
<SELECT subcube clause> ::=  
      Cube_Name   
   | [NON VISUAL] (SELECT   
                  [ *   
       | ( <SELECT query axis clause> [ ,   
           <SELECT query axis clause>,...n ] )   
         ]   
            FROM   
         <SELECT subcube clause>   
         <SELECT slicer axis clause> )  
  
<SELECT slicer axis clause> ::=   
      WHERE Tuple_Expression  
  
<SELECT cell property list clause> ::=   
   [ CELL ] PROPERTIES CellProperty_Name   
      [ , CellProperty_Name,...n ]  
  
<SELECT dimension property list clause> ::=  
   [DIMENSION] PROPERTIES   
      (DimensionProperty_Name   
         [,DimensionProperty_Name,...n ] )   
    | (LevelProperty_Name   
         [, LevelProperty_Name,...n ] )   
    | (MemberProperty_Name   
         [, MemberProperty_Name,...n ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Entier*  
 Entier entre 0 et 127.  
  
 *Cube_Name*  
 Chaîne valide qui précise le nom d'un cube.  
  
 *Tuple_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un tuple.  
  
 *CellProperty_Name*  
 Chaîne valide qui représente une propriété de cellule.  
  
 *DimensionProperty_Name*  
 Chaîne valide qui représente une propriété de dimension.  
  
 *LevelProperty_Name*  
 Chaîne valide qui représente une propriété de niveau.  
  
 *MemberProperty_Name*  
 Chaîne valide qui représente une propriété de membre.  
  
## <a name="remarks"></a>Notes  
 L'expression `<SELECT slicer axis clause>` doit contenir des membres de dimensions et de hiérarchies autres que ceux référencés dans les expressions `<SELECT query axis clause>` spécifiées.  
  
 Si un attribut du cube est omis dans les expressions `<SELECT query axis clause>` spécifiées et dans la valeur `<SELECT slicer axis clause>`, le membre par défaut de l'attribut est implicitement ajouté à l'axe de secteur.  
  
 L'option NON VISUAL de l'instruction de sous-sélection vous permet de filtrer les membres tout en conservant les totaux réels au lieu des totaux filtrés. Cela vous permet de lancer une requête pour les dix meilleures ventes (personnes/produits/régions) et d'obtenir le total réel des ventes pour tous les membres faisant l'objet de la requête, au lieu de la valeur totale des ventes pour les dix meilleures ventes renvoyées. Pour plus d'informations, consultez les exemples ci-après.  
  
 Les membres calculés peuvent être inclus dans \<clause d’axe de requête SELECT > chaque fois que la connexion a été ouvert à l’aide du paramètre de chaîne de connexion *sous-requêtes = 1*; consultez [pris en charge les propriétés XMLA &#40; XMLA&#41; ](../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md) et <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString%2A> pour l’utilisation des paramètres. Voici un exemple concernant les membres calculés dans les sous-sélections.  
  
## <a name="autoexists"></a>Autoexists  
 Lorsque deux attributs, ou plus, de la dimension sont utilisés dans une instruction SELECT, Analysis Services évalue les expressions des attributs pour s'assurer que les membres de ces attributs sont correctement limités afin de répondre aux critères de tous les autres attributs. Supposons, par exemple, que vous utilisez des attributs de la dimension de Geography. Si vous avez une expression qui retourne tous les membres de l'attribut City et une autre expression qui limite les membres de l'attribut Country à tous les pays d'Europe, il en résultera une limitation des membres de City aux seules villes qui appartiennent à des pays d'Europe. Cette caractéristique d'Analysis Services, appelée Autoexists, s'applique uniquement à des attributs dans la même dimension. En effet, elle tente d'empêcher que des enregistrements de la dimension exclus d'une expression d'attribut soient exclus par les autres expressions d'attributs. La fonctionnalité Autoexists peut également être interprétée comme l'intersection obtenue entre les différentes expressions d'attributs sur les enregistrements de dimension. Observez les exemples ci-dessous :  
  
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
  
 Dans les exemples précédents, nous avons créé deux jeux : l'un sous forme d'expression calculée et l'autre sous forme d'expression constante. Ces exemples illustrent les différentes versions de la fonctionnalité Autoexists.  
  
 Autoexists peut être appliquée en profondeur ou superficiellement aux expressions. La valeur par défaut est Deep. L'exemple suivant illustre le concept de la fonctionnalité Deep Autoexists. Dans l'exemple, nous filtrons Top10SellingProducts sur l'attribut [Product].[Product Line] pour les membres du groupe [Mountain]. Notez que les deux attributs (slicer et axis) appartiennent à la même dimension, [Product].  
  
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
  
 Dans le jeu de résultats précédent, nous avons sept nouveaux venus dans la liste des articles Top10SellingProducts ; de plus, Mountain-200, Mountain-100 et HL Mountain Frame ont été déplacés en haut de la liste. Dans ce même jeu de résultats, ces trois valeurs étaient entrecoupées.  
  
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
  
 Comportement d’Autoexists peut être modifié à l’aide de la fonctionnalité AUTOEXISTS = [1 | 2 | paramètre 3] dans la chaîne de connexion ; consultez [pris en charge les propriétés XMLA &#40;XMLA&#41; ](../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md) et <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString%2A> pour l’utilisation des paramètres.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne la somme de la `Measures.[Order Quantity]` membre, agrégé sur les huit premiers mois de l’année civile 2003 qui sont contenus dans le `Date` dimension, à partir de la **Adventure Works** cube.  
  
```  
WITH MEMBER [Date].[Calendar].[First8Months2003] AS  
    Aggregate(  
        PeriodsToDate(  
            [Date].[Calendar].[Calendar Year],   
            [Date].[Calendar].[Month].[August 2003]  
        )  
    )  
SELECT   
    [Date].[Calendar].[First8Months2003] ON COLUMNS,  
    [Product].[Category].Children ON ROWS  
FROM  
    [Adventure Works]  
WHERE  
    [Measures].[Order Quantity]  
```  
  
 Pour comprendre **NON VISUAL** l’exemple suivant est une requête sur [Adventure Works] pour obtenir les chiffres [Reseller Sales Amount] dans une table où les catégories de produits sont les colonnes et les types d’entreprise reseller sont les lignes. Notez que les totaux sont fournis pour les produits et pour les revendeurs.  
  
 L'instruction SELECT suivante :  
  
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
  
 Pour créer une table contenant uniquement les données pour les produits Accessories et Clothing, ainsi que les revendeurs Value Added Reseller et Warehouse, tout en conservant les totaux globaux, vous pouvez utiliser NON VISUAL comme suit :  
  
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
  
 L'exemple suivant montre comment utiliser des membres calculés comme élément de filtrage dans des instructions de sous-sélection. Pour être en mesure de reproduire cet exemple, la connexion doit être établie à l’aide du paramètre de chaîne de connexion *sous-requêtes = 1*.  
  
 `select Measures.allmembers on 0`  
  
 `from (`  
  
 `Select { [Measures].[Reseller Sales Amount]`  
  
 `, [Measures].[Reseller Total Product Cost]`  
  
 `, [Measures].[Reseller Gross Profit]`  
  
 `, [Measures].[Reseller Gross Profit Margin]`  
  
 `} on 0`  
  
 `from [Adventure Works]`  
  
 `)`  
  
 La requête ci-dessus génère les résultats suivants :  
  
|||||  
|-|-|-|-|  
|Reseller Sales Amount|Reseller Total Product Cost|Reseller Gross Profit|Reseller Gross Profit Margin|  
|80 450 596,98 $|$79,980,114.38|$470,482.60|0.58%|  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts clés pour MDX &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md)   
 [Les instructions de Manipulation de données MDX &#40;MDX&#41;](../mdx/mdx-data-manipulation-statements-mdx.md)   
 [Restriction de la requête avec les Axes de requête et segment &#40;MDX&#41;](~/analysis-services/multidimensional-models/mdx/mdx-query-and-slicer-axes-restricting-the-query.md)  
  
  

