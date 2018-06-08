---
title: Espace du cube | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 58f1a22cbba10eff6c10d2fc70dffcb13632b15d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="cube-space"></a>Espace du cube
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  L'espace du cube est le produit des membres des hiérarchies d'attribut d'un cube associés aux mesures du cube. Par conséquent, l'espace du cube est déterminé par le produit combinatoire de tous les membres de la hiérarchie d'attribut dans le cube et par les mesures du cube, et définit la taille maximale du cube. Il est important de noter que cet espace inclut toutes les combinaisons possibles de membres de la hiérarchie d'attribut ; même les combinaisons qui peuvent être jugées comme impossibles dans le monde réel, c'est-à-dire les combinaisons où la ville est Paris et les pays sont l'Angleterre, l'Espagne, le Japon, l'Inde ou ailleurs.  
  
## <a name="autoexists-and-cube-space"></a>Fonctionnalité Autoexists et espace du cube  
 La fonctionnalité d'auto-existence, *Autoexists* limite l'espace du cube aux cellules qui existent réellement. Les membres d'une hiérarchie d'attribut au sein d'une dimension ne peuvent coexister avec les membres d'une autre hiérarchie d'attribut au sein de la même dimension.  
  
 Par exemple, si vous travaillez avec un cube muni d'une hiérarchie d'attribut Ville, d'une hiérarchie d'attribut Pays et d'une mesure Volume de vente Internet, l'espace du cube inclut uniquement les membres qui coexistent entre eux. Par exemple, si la hiérarchie d'attribut Ville inclut les villes de New York, Londres, Paris, Tokyo et Melbourne et la hiérarchie d'attribut Pays les pays États-Unis, Royaume-Uni, France, Japon et Australie, l'espace du cube n'inclut pas l'espace (cellule) à l'intersection de Paris et États-Unis.  
  
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
  
 Chacune des trois requêtes présentées ci-dessus illustre les effets du comportement de l’existence automatique dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
## <a name="user-defined-hierarchies-and-cube-space"></a>Hiérarchies définies par l'utilisateur et espace du cube  
 Les exemples présentés précédemment dans cette rubrique définissent des positions au sein de l'espace du cube par le biais des hiérarchies d'attribut. Néanmoins, vous pouvez également définir une position dans l'espace du cube en utilisant des hiérarchies définies par l'utilisateur qui ont été conçues à partir des hiérarchies d'attribut d'une dimension. Une hiérarchie définie par l'utilisateur est une hiérarchie de hiérarchies d'attribut conçue pour faciliter la navigation des utilisateurs dans les données du cube.  
  
 Par exemple, la requête **CROSSJOIN** de la section précédente aurait pu aussi être écrite de la manière suivante :  
  
```  
SELECT CROSSJOIN  
   (  
      {[Customer].[Country].[United States]},  
         [Customer].[Customer Geography].[State-Province].Members  
   )   
ON 0   
FROM [Adventure Works]  
WHERE Measures.[Internet Sales Amount]  
```  
  
 Dans la requête ci-dessus, la hiérarchie définie par l'utilisateur Customer Geography incluse dans la dimension Customer est utilisée pour définir la position dans l'espace du cube définie auparavant à l'aide d'une hiérarchie d'attribut. La même position dans l'espace du cube peut être définie soit au moyen de hiérarchies d'attribut, soit à l'aide de hiérarchies définies par l'utilisateur.  
  
##  <a name="AttribRelationships"></a> Relations d'attributs et espace du cube  
 La définition de relations d'attributs entre des attributs associés améliore les performances des requêtes (grâce à la création facilitée d'agrégations adaptées) et affecte le membre d'une hiérarchie d'attribut associée qui apparaît avec un membre de hiérarchie d'attribut. Par exemple, lorsque vous définissez un tuple qui inclut un membre de la hiérarchie d'attribut City et que le tuple ne définit pas explicitement le membre de la hiérarchie d'attribut Country, vous pouvez vous attendre à ce que le membre par défaut de la hiérarchie d'attribut Country corresponde au membre associé de la hiérarchie d'attribut Country. Néanmoins, cette situation ne vaut que si une relation d'attribut est définie entre les hiérarchies d'attribut City et Country.  
  
 L'exemple ci-après retourne le membre d'une hiérarchie d'attribut associée qui ne figure pas de manière explicite dans la requête.  
  
```  
WITH MEMBER Measures.x AS   
   Customer.Country.CurrentMember.Name  
SELECT Measures.x ON 0,  
Customer.City.Members ON 1  
FROM [Adventure Works]  
```  
  
> [!NOTE]  
>  Notez que le mot clé **WITH** est utilisé avec les fonctions [CurrentMember (MDX)](../../../mdx/currentmember-mdx.md) et [Name (MDX)](../../../mdx/name-mdx.md) pour créer un membre calculé à utiliser dans la requête. Pour plus d’informations, consultez [Requête MDX de base &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-query-the-basic-query.md).  
  
 Dans la requête précédente, le nom du membre de la hiérarchie d'attribut Country associé à chaque membre de la hiérarchie d'attribut State est retourné. Le membre Country attendu apparaît (puisqu'une relation d'attribut est définie entre les attributs City et Country). Par contre, si aucune relation d'attribut n'est définie entre les hiérarchies d'attribut de la même dimension, le membre (All) doit être retourné comme dans la requête suivante.  
  
```  
WITH MEMBER Measures.x AS   
   Customer.Education.Currentmember.Name  
SELECT Measures.x  ON 0,   
Customer.City.Members ON 1  
FROM [Adventure Works]  
```  
  
 Dans cette requête, le membre (All) (« All Customers ») est retourné parce qu'il n'existe aucune relation entre Education et City. Le membre (All) de la hiérarchie d'attribut Education serait alors le membre par défaut de cette hiérarchie utilisé dans n'importe quel tuple impliquant la hiérarchie d'attribut City où aucun membre Education n'est explicitement fourni.  
  
## <a name="calculation-context"></a>Contexte de calcul  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts clés dans MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md)   
 [Tuples](../../../analysis-services/multidimensional-models/mdx/tuples.md)   
 [Fonctionnalité Autoexists](../../../analysis-services/multidimensional-models/mdx/autoexists.md)   
 [Utilisation des membres, Tuples et jeux & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/working-with-members-tuples-and-sets-mdx.md)   
 [Valeurs totales et Non visibles](../../../analysis-services/multidimensional-models/mdx/visual-totals-and-non-visual-totals.md)   
 [Référence du langage MDX & #40 ; MDX & #41 ;](../../../mdx/mdx-language-reference-mdx.md)   
 [Expressions multidimensionnelles & #40 ; MDX & #41 ; Référence](../../../mdx/multidimensional-expressions-mdx-reference.md)  
  
  
