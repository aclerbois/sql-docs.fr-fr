---
title: DrilldownLevel (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 99e8c47164d920ec531bf6ab51e35979b060c35d
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578001"
---
# <a name="drilldownlevel-mdx"></a>DrilldownLevel (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Explore les membres d'un jeu un niveau en dessous du niveau le plus bas représenté dans le jeu.  
  
 Spécifiant le niveau d’exploration dans bas est facultatif, mais si vous ne définissez pas le niveau, vous pouvez utiliser un **expression de niveau** ou **niveau d’index**. Ces arguments s'excluent mutuellement. Enfin, si des membres calculés sont présents dans la requête, vous pouvez spécifier un argument pour les inclure dans l'ensemble de lignes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
DrilldownLevel(Set_Expression [,[Level_Expression] ,[Index]] [,INCLUDE_CALC_MEMBERS])  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Level_Expression*  
 (Facultatif). Une expression MDX qui identifie explicitement le niveau à explorer. Si vous spécifiez une expression de niveau, ignorez l'argument d'index du niveau inférieur.  
  
 *Index*  
 (Facultatif). Expression numérique valide qui spécifie le nombre de hiérarchies pour lesquelles procéder à une extraction vers le bas au sein du jeu. Vous pouvez utiliser le niveau d'index de Level_Expression pour identifier explicitement le niveau jusqu'où explorer.  
  
 *Include_Calc_Members*  
 (Facultatif). Un indicateur spécifiant s'il faut inclure les membres calculés (s'ils existent) au niveau d'exploration.  
  
## <a name="remarks"></a>Notes  
 Le **DrilldownLevel** fonction retourne un jeu d’enfant de membres dans un ordre hiérarchique, basé sur les membres inclus dans le jeu spécifié. L'ordre est conservé parmi les membres d'origine dans le jeu spécifié, à ceci près que tous les membres enfants inclus dans le jeu de résultats de la fonction sont placés immédiatement sous leur membre parent.  
  
 Pour une structure de données hiérarchique multiniveau donnée, vous pouvez choisir explicitement un niveau jusqu'où explorer. Il y a deux moyens mutuellement exclusifs de spécifier le niveau. La première approche consiste à définir le **level_expression** argument à l’aide d’une expression MDX qui retourne le niveau, une autre approche consiste à spécifier le **index** argument, à l’aide d’une expression numérique qui spécifie le niveau par numéro.  
  
 Si une expression de niveau est spécifiée, la fonction construit un jeu dans un ordre hiérarchique en récupérant uniquement les enfants des membres situés au niveau spécifié. Si une expression de niveau est spécifiée et qu'il n'y a pas de membre à ce niveau, l'expression de niveau est ignorée.  
  
 Si vous spécifiez une valeur d'index, la fonction construit un jeu dans l'ordre hiérarchique en récupérant seulement les enfants des membres situés au niveau le plus bas suivant de la hiérarchie référencée dans le jeu spécifié, étant donné un index de base zéro.  
  
 Si aucune expression de niveau ou valeur d'index n'est spécifiée, la fonction génère un jeu dans l'ordre hiérarchique en extrayant uniquement les enfants des membres situés au niveau le plus bas de la première dimension référencée dans le jeu spécifié.  
  
 Interrogez la propriété XMLA MdpropMdxDrillFunctions vous permet de vérifier le niveau de prise en charge par le serveur pour les fonctions d’extraction ; consultez [pris en charge les propriétés XMLA &#40;XMLA&#41; ](../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md) pour plus d’informations.  
  
## <a name="examples"></a>Exemples  
 Vous pouvez essayer les exemples suivants dans la fenêtre de requête MDX dans SSMS, en utilisant le cube Adventure Works.  
  
 **Exemple 1 : utilisation d’une syntaxe minimale**  
  
 Le premier exemple montre la syntaxe minimale **DrilldownLevel**. Le seul argument requis est une expression définie. Notez que lorsque vous exécutez cette requête, vous obtenez le parent [All Categories] et les membres du niveau suivant vers le bas : [Accessories], [Bikes], et ainsi de suite. Bien que cet exemple soit simple, il illustre l’objectif de base de la **DrilldownLevel** fonction, qui est d’Explorer jusqu’au niveau inférieur suivant.  
  
```  
SELECT DRILLDOWNLEVEL({[Product].[Product Categories]} * {[Sales Territory].[Sales Territory]}}) ON COLUMNS  
FROM [Adventure Works]  
```  
  
 Exemple 2 : syntaxe alternative utilisant un niveau d'index explicite  
  
 Cet exemple montre la syntaxe alternative, où le niveau d'index est spécifié via une expression numérique. Dans ce cas, le niveau d'index est 0. Pour un index de base zéro, il s'agit de l'index de niveau le plus bas.  
  
```  
SELECT  
DRILLDOWNLEVEL({[Product].[Product Categories]} * {[Sales Territory].[Sales Territory]},,0) ON COLUMNS  
FROM [Adventure Works]  
```  
  
 Notez que le jeu de résultats est identique à celui de la requête précédente. En règle générale, la définition du niveau d'index n'est pas nécessaire, sauf si vous voulez que l'exploration commence à un niveau spécifique. Réexécutez la requête précédente en définissant la valeur de l'index à 1, puis à 2. Avec la valeur de l'index définie à 1, vous voyez que l'exploration commence au deuxième niveau de la hiérarchie. Avec la valeur de l'index définie à 2, l'exploration commence au troisième niveau de la hiérarchie, le niveau le plus élevé dans cet exemple. Plus l'expression numérique est élevée, plus le niveau de l'index est élevé.  
  
 **Exemple 3 : utilisation d’une expression de niveau**  
  
 L'exemple suivant montre comment utiliser une expression de niveau. Pour un jeu qui représente une structure hiérarchique, l'utilisation d'une expression de niveau vous permet de choisir un niveau dans la hiérarchie pour commencer l'exploration.  
  
 Dans cet exemple, le niveau de zoom commence à [City], comme deuxième argument de la **DrilldownLevel** (fonction). Quand vous exécutez cette requête, l'exploration commence au niveau [City], pour les états Washington et Oregon. Par la **DrilldownLevel** (fonction), également de jeu de résultats inclut les membres du niveau suivant vers le bas, [Postal codes].  
  
```  
SELECT [Measures].[Internet Sales Amount] ON COLUMNS,  
   NON EMPTY (  
   DRILLDOWNLEVEL(  
       {[Customer].[Customer Geography].[Country].[United States],  
           DESCENDANTS(  
             { [Customer].[Customer Geography].[State-Province].[Washington],    
               [Customer].[Customer Geography].[State-Province].[Oregon]},   
               [Customer].[Customer Geography].[City]) } ,  
[Customer].[Customer Geography].[City] ) )  ON ROWS  
FROM [Adventure Works]  
```  
  
 **Exemple 4 : inclusion des membres calculés**  
  
 Le dernier exemple montre un membre calculé, qui apparaît au bas du résultat défini lorsque vous ajoutez le **include_calculated_members** indicateur. Notez que l'indicateur est spécifié comme quatrième paramètre.  
  
 Cet exemple fonctionne, car le membre calculé est au même niveau que les membres non calculés. Le membre calculé [West Coast] est composé de membres de [United States], plus tous les membres du niveau inférieur à [United States].  
  
```  
WITH MEMBER   
[Customer].[Customer Geography].[Country].&[United States].[West Coast] AS  
[Customer].[Customer Geography].[State-Province].&[OR]&[US] +  
[Customer].[Customer Geography].[State-Province].&[WA]&[US] +  
[Customer].[Customer Geography].[State-Province].&[CA]&[US]  
SELECT [Measures].[Internet Order Count] ON 0,  
DRILLDOWNLEVEL([Customer].[Customer Geography].[Country].&[United States],,,INCLUDE_CALC_MEMBERS) on 1  
FROM [Adventure Works]  
```  
  
 Si vous supprimez seulement l'indicateur et que vous réexécutez la requête, vous obtenez les mêmes résultats sans le membre calculé, [West Coast].  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
