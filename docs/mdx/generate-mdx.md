---
title: Générer (MDX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: ''
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- GENERATE
dev_langs:
- kbMDX
helpviewer_keywords:
- Generate function
ms.assetid: 696a229d-c2f1-47b7-9dca-7b0a6b547d9b
caps.latest.revision: 32
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: be6b05c0738b2407d6d803bae471a73ead15e353
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="generate-mdx"></a>Generate (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Applique un jeu à chaque membre d'un autre jeu, puis effectue la jointure par union des jeux résultants. Cette fonction retourne également une chaîne concaténée créée par l'évaluation d'une expression de chaîne sur un jeu.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set expression syntax  
Generate( Set_Expression1 ,  Set_Expression2 [ , ALL ]  )  
  
String expression syntax  
Generate( Set_Expression1 ,  String_Expression [ ,Delimiter ]  )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression1*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Set_Expression2*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *String_Expression*  
 Expression de chaîne valide qui correspond généralement au nom du membre actuel (CurrentMember.Name) de chaque tuple dans le jeu spécifié.  
  
 *Délimiteur*  
 Délimiteur valide exprimé en tant qu'expression de chaîne.  
  
## <a name="remarks"></a>Notes  
 Si un deuxième jeu est spécifié, le **générer** fonction retourne un jeu généré en appliquant les tuples du deuxième jeu à chaque tuple dans le premier jeu *,* et puis associant les jeux obtenus par union. Si **tous les** est spécifié, la fonction conserve les doublons dans le jeu résultant.  
  
 Si une expression de chaîne est spécifiée, le **générer** fonction retourne une chaîne générée en évaluant l’expression de chaîne spécifiée par rapport à chaque tuple dans le premier jeu *,* puis en concaténant les résultats. Vous pouvez éventuellement délimiter la chaîne en séparant chaque résultat dans la chaîne concaténée obtenue.  
  
## <a name="examples"></a>Exemples  
  
### <a name="set"></a>Définissez  
 Dans l'exemple suivant, la requête retourne un jeu qui contient le montant des ventes sur Internet de Mesure quatre fois, parce qu'il y a quatre membres dans le jeu [Date].[Calendar Year].[Calendar Year].MEMBERS :  
  
```  
SELECT   
GENERATE( [Date].[Calendar Year].[Calendar Year].MEMBERS  
, {[Measures].[Internet Sales Amount]}, ALL)  
ON 0  
FROM [Adventure Works]  
```  
  
 La suppression de ALL modifie la requête afin que le Montant des ventes sur Internet soit retourné une fois uniquement :  
  
```  
SELECT   
GENERATE( [Date].[Calendar Year].[Calendar Year].MEMBERS  
, {[Measures].[Internet Sales Amount]})  
ON 0  
FROM [Adventure Works]  
```  
  
 L’utilisation pratique la plus commune de **générer** est définie pour évaluer un type complexe expression, telle que TopCount, sur un jeu de membres. L'exemple de requête suivant affiche les 10 premiers produits pour chaque année civile sur les lignes :  
  
```  
SELECT   
{[Measures].[Internet Sales Amount]}  
ON 0,  
GENERATE(   
[Date].[Calendar Year].[Calendar Year].MEMBERS  
, TOPCOUNT(  
[Date].[Calendar Year].CURRENTMEMBER  
*  
[Product].[Product].[Product].MEMBERS  
,10, [Measures].[Internet Sales Amount]))  
ON 1  
FROM [Adventure Works]  
```  
  
 Notez qu’un différent des 10 premiers sont affiché pour chaque année et que l’utilisation de **générer** est la seule façon d’obtenir ce résultat. La simple jonction croisée des années civiles et du jeu des 10 premiers produits affichent les 10 premiers produits pour toutes les périodes, pour chaque année, comme illustré dans l'exemple suivant :  
  
```  
SELECT   
{[Measures].[Internet Sales Amount]}  
ON 0,  
[Date].[Calendar Year].[Calendar Year].MEMBERS  
*   
TOPCOUNT(  
[Product].[Product].[Product].MEMBERS  
,10, [Measures].[Internet Sales Amount])  
ON 1  
FROM [Adventure Works]  
```  
  
### <a name="string"></a>Chaîne  
 L’exemple suivant illustre l’utilisation de **générer** pour retourner une chaîne :  
  
```  
WITH   
MEMBER MEASURES.GENERATESTRINGDEMO AS  
GENERATE(   
[Date].[Calendar Year].[Calendar Year].MEMBERS,  
[Date].[Calendar Year].CURRENTMEMBER.NAME)  
MEMBER MEASURES.GENERATEDELIMITEDSTRINGDEMO AS  
GENERATE(   
[Date].[Calendar Year].[Calendar Year].MEMBERS,  
[Date].[Calendar Year].CURRENTMEMBER.NAME, " AND ")  
SELECT   
{MEASURES.GENERATESTRINGDEMO, MEASURES.GENERATEDELIMITEDSTRINGDEMO}  
ON 0  
FROM [Adventure Works]  
```  
  
> [!NOTE]  
>  Cette forme de la **générer** fonction peut être utile lors du débogage des calculs, car elle permet de retourner une chaîne qui affiche les noms de tous les membres dans un jeu. Cela peut être plus facile à lire que la représentation MDX stricte d’un jeu qui le [SetToStr &#40;MDX&#41; ](../mdx/settostr-mdx.md) fonction renvoie.  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
