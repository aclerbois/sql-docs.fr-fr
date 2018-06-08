---
title: Commande (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ff748fab21311bf7b881bcc4594e3a0dccb5d1b3
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581041"
---
# <a name="order-mdx"></a>Order (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Organise les membres d'un jeu spécifié, en conservant ou non la hiérarchie.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Numeric expression syntax  
Order(Set_Expression, Numeric_Expression   
[ , { ASC | DESC | BASC | BDESC } ] )  
  
String expression syntax  
Order(Set_Expression, String_Expression   
[ , { ASC | DESC | BASC | BDESC } ] )  
  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Numeric_expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
 *String_Expression*  
 Expression de chaîne valide qui correspond généralement à une expression MDX (Multidimensional Expressions) valide des coordonnées des cellules qui retournent un nombre exprimé sous forme de chaîne.  
  
## <a name="remarks"></a>Notes  
 Le **ordre** fonction peut être hiérarchique (comme spécifié à l’aide de la **ASC** ou **DESC** indicateur) ou non hiérarchique (comme spécifié à l’aide de la **BASC** ou **BDESC** indicateur ; le **B** « hiérarchie »). Si **ASC** ou **DESC** est spécifié, le **commande** fonction réorganise d’abord les membres en fonction de leur position dans la hiérarchie et puis elle ordonne chaque niveau. Si **BASC** ou **BDESC** est spécifié, le **commande** fonction organise les membres du jeu, indépendamment de la hiérarchie. Aucun indicateur n’est spécifié, **ASC** est la valeur par défaut.  
  
 Si le **commande** fonction est utilisée avec un jeu où deux ou plusieurs hiérarchies sont joints entre eux et le **DESC** indicateur est utilisé, seuls les membres de la dernière hiérarchie dans le jeu sont triés. Il s'agit là d'une différence par rapport à Analysis Services 2000 où toutes les hiérarchies du jeu étaient triées.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant renvoie, à partir de la **Adventure Works** de cube, le nombre de commandes du revendeur pour tous les trimestres calendaires à partir de la hiérarchie de calendrier sur la dimension Date. Le **commande** fonction réorganise le jeu pour l’axe des lignes. Le **commande** fonction trie le jeu par `[Reseller Order Count]` décroissant hiérarchique telle que déterminée par la `[Calendar]` hiérarchie.  
  
 `SELECT`  
  
 `Measures.[Reseller Order Count] ON COLUMNS,`  
  
 `Order(`  
  
 `[Date].[Calendar].[Calendar Quarter].MEMBERS`  
  
 `,Measures.[Reseller Order Count]`  
  
 `,DESC`  
  
 `) ON ROWS`  
  
 `FROM [Adventure Works]`  
  
 Notez dans cet exemple, lorsque le **DESC** indicateur est remplacée par **BDESC**, la hiérarchie est interrompue et la liste de trimestres calendaires est retournée sans tenir compte de la hiérarchie :  
  
 `SELECT`  
  
 `Measures.[Reseller Order Count] ON COLUMNS,`  
  
 `Order(`  
  
 `[Date].[Calendar].[Calendar Quarter].MEMBERS`  
  
 `,Measures.[Reseller Order Count]`  
  
 `,BDESC`  
  
 `) ON ROWS`  
  
 `FROM [Adventure Works]`  
  
 L'exemple ci-dessous retourne la mesure Reseller Sales pour les cinq premières sous-catégories de vente de produits, quelle que soit la hiérarchie et conformément à la mesure Reseller Gross Profit (marge brute du revendeur). Le **sous-ensemble** fonction est utilisée pour retourner uniquement les 5 premiers tuples dans le jeu une fois que le résultat est trié à l’aide de la **ordre** (fonction).  
  
 `SELECT Subset`  
  
 `(Order`  
  
 `([Product].[Product Categories].[SubCategory].members`  
  
 `,[Measures].[Reseller Gross Profit]`  
  
 `,BDESC`  
  
 `)`  
  
 `,0`  
  
 `,5`  
  
 `) ON 0`  
  
 `FROM [Adventure Works]`  
  
 L’exemple suivant utilise le **rang** fonction pour classer les membres de la hiérarchie City, en fonction de la mesure Reseller Sales Amount, puis les affiche dans l’ordre de classement. À l’aide de la **commande** de fonction à la première commande le jeu de membres de la hiérarchie de la ville, le tri est effectué en une seule fois et suivi d’une analyse linéaire avant d’être présenté dans l’ordre de tri.  
  
```  
WITH   
SET OrderedCities AS Order  
   ([Geography].[City].[City].members  
   , [Measures].[Reseller Sales Amount], BDESC  
   )  
MEMBER [Measures].[City Rank] AS Rank  
   ([Geography].[City].CurrentMember, OrderedCities)  
SELECT {[Measures].[City Rank],[Measures].[Reseller Sales Amount]}  ON 0   
,Order  
   ([Geography].[City].[City].MEMBERS  
   ,[City Rank], ASC)  
    ON 1  
FROM [Adventure Works]  
```  
  
 L’exemple suivant retourne le nombre de produits dans le jeu qui sont uniques, à l’aide de la **commande** fonction pour classer les tuples non vides avant d’utiliser le **filtre** (fonction). Le **CurrentOrdinal** fonction est utilisée pour comparer et éliminer les liens.  
  
```  
WITH MEMBER [Measures].[PrdTies] AS Count  
   (Filter  
      (Order  
        (NonEmpty  
          ([Product].[Product].[Product].Members  
          , {[Measures].[Reseller Order Quantity]}  
          )  
       , [Measures].[Reseller Order Quantity]  
       , BDESC  
       ) AS OrdPrds  
    , (OrdPrds.CurrentOrdinal < OrdPrds.Count   
       AND [Measures].[Reseller Order Quantity] =   
          ( [Measures].[Reseller Order Quantity]  
            , OrdPrds.Item  
               (OrdPrds.CurrentOrdinal  
               )  
            )  
         )  
         OR (OrdPrds.CurrentOrdinal > 1   
            AND [Measures].[Reseller Order Quantity] =   
               ([Measures].[Reseller Order Quantity]  
               , OrdPrds.Item  
                  (OrdPrds.CurrentOrdinal-2)  
                )  
             )  
          )  
       )  
SELECT {[Measures].[PrdTies]} ON 0  
FROM [Adventure Works]  
```  
  
 Pour comprendre comment les **DESC** indicateur fonctionne avec les jeux de tuples, envisagez d’abord les résultats de la requête suivante :  
  
```  
  
SELECT  
{[Measures].[Tax Amount]} ON 0,  
ORDER(  
[Sales Territory].[Sales Territory].[Group].MEMBERS  
,[Measures].[Tax Amount], DESC)  
ON 1  
FROM [Adventure Works]  
  
```  
  
 Sur l'axe des lignes, vous pouvez voir que les groupes du secteur de vente ont été classés par ordre décroissant par Montant des taxes, comme suit : Amérique du Nord, Europe, Pacifique, NA. Voir maintenant que se passe-t-il si nous jointure croisée du jeu de groupes du secteur de vente avec le jeu de sous-catégories de produits et appliquer le **commande** fonctionnent de la même façon, comme suit :  
  
```  
  
SELECT  
{[Measures].[Tax Amount]} ON 0,  
ORDER(  
[Sales Territory].[Sales Territory].[Group].MEMBERS  
*  
{[Product].[Product Categories].[subCategory].Members}  
,[Measures].[Tax Amount], DESC)  
ON 1  
FROM [Adventure Works]  
  
```  
  
 Alors que le jeu de sous-catégories de produits a été trié par ordre décroissant, hiérarchique, les groupes du secteur de vente ne sont plus triés et s'affichent dans l'ordre dans lequel ils apparaissent dans la hiérarchie : Europe, NA, Amérique du Nord et Pacifique. Cela vient du fait que seule la dernière hiérarchie du jeu de tuples, sous-catégories de produits, est triée. Pour reproduire le comportement d’Analysis Services 2000, utilisez une série d’imbriqués **générer** fonctions pour trier chaque jeu avant sa jointure croisée, par exemple :  
  
```  
  
SELECT  
{[Measures].[Tax Amount]} ON 0,  
GENERATE(  
ORDER(  
[Sales Territory].[Sales Territory].[Group].MEMBERS  
,[Measures].[Tax Amount], DESC)  
,  
ORDER(  
[Sales Territory].[Sales Territory].CURRENTMEMBER  
*  
{[Product].[Product Categories].[subCategory].Members}  
,[Measures].[Tax Amount], DESC))  
ON 1  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
