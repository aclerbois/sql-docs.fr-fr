---
title: Identificateurs (MDX) | Documents Microsoft
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
dev_langs:
- kbMDX
helpviewer_keywords:
- formats [Analysis Services]
- Multidimensional Expressions [Analysis Services], identifiers
- identifiers [MDX]
- MDX [Analysis Services], identifiers
- delimited identifiers [MDX]
- regular identifiers [MDX]
- formats [Analysis Services], identifiers
ms.assetid: 739a8a67-bef3-4b56-961d-ee96cfc36b5b
caps.latest.revision: 32
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: a04d387c0ee40d825fddf3c50f02793e722cdbc8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="identifiers-mdx"></a>Identificateurs (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Un identificateur est le nom d’un [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] objet. Tous les objets [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peuvent et doivent posséder un identificateur. Ces objets comprennent les cubes, les dimensions, les hiérarchies, les niveaux, les membres, etc. L'identificateur d'un objet permet de faire référence à l'objet dans des instructions MDX (Multidimensional Expressions).  
  
 Selon le nom que vous attribuez à l'objet, son identificateur sera un identificateur régulier ou délimité.  
  
> [!NOTE]  
>  Les identificateurs réguliers ou délimités doivent contenir entre 1 et 100 caractères.  
  
## <a name="using-regular-identifiers"></a>Utilisation d'identificateurs réguliers  
 Un identificateur régulier est un nom d'objet conforme aux règles de mise en forme suivantes. Il peut être utilisé avec ou sans délimiteurs.  
  
### <a name="formatting-rules-for-regular-identifiers"></a>Règles de mise en forme des identificateurs réguliers  
  
1.  Le premier caractère doit être l'un des suivants :  
  
    -   Des lettres définies par Unicode Standard 2.0. Outre les caractères alphabétiques d'autres langues, elles incluent les caractères latins a-z et A-Z.  
  
    -   Le trait de soulignement (_).  
  
2.  Les caractères suivants peuvent être :  
  
    -   Lettres définies dans Unicode Standard 2.0.  
  
    -   Des nombres décimaux de Basic Latin ou d'autres scripts nationaux.  
  
    -   Le trait de soulignement (_).  
  
3.  L'identificateur ne doit pas être un mot réservé MDX. En effet, ils ne respectent pas la casse dans la syntaxe MDX. Pour plus d’informations, consultez [mots clés réservés &#40;syntaxe MDX&#41;](../mdx/reserved-keywords-mdx-syntax.md).  
  
4.  Les espaces incorporés ou les caractères spéciaux ne sont pas autorisés.  
  
### <a name="examples-of-regular-identifiers"></a>Exemples d'identificateurs réguliers  
 Dans l'instruction MDX suivante, les identificateurs (`Measures`, `Product` et `Style`) respectent les règles de mise en forme des identificateurs réguliers. Ceux-ci n'ont pas besoin de délimiteurs.  
  
 `SELECT Measures.MEMBERS ON COLUMNS,`  
  
 `Product.Style.CHILDREN ON ROWS`  
  
 `FROM [Adventure Works]`  
  
 ``  
  
 Même s'ils ne sont pas nécessaires, vous pouvez utiliser des délimiteurs avec les identificateurs réguliers. Dans l'instruction MDX suivante, les identificateurs réguliers `Measures`, `Product` et `Style` sont correctement délimités par des crochets.  
  
 `SELECT [Measures].MEMBERS ON COLUMNS,`  
  
 `[Product].[Style].CHILDREN ON ROWS`  
  
 `FROM [Adventure Works]`  
  
 ``  
  
## <a name="using-delimited-identifiers"></a>Utilisation d'identificateurs délimités  
 S'il ne respecte pas les règles de mise en forme des identificateurs réguliers, l'identificateur doit toujours être délimité à l'aide de crochets ([]).  
  
> [!NOTE]  
>  Les délimiteurs sont uniquement réservés aux identificateurs. Ils ne peuvent être utilisés pour les mots clés, que ces derniers soient marqués comme étant réservés ou non dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
 Les identificateurs délimités peuvent être utilisés dans les cas suivants :  
  
-   Lorsque le nom d'un objet ou une partie du nom utilise des mots réservés.  
  
     Il est conseillé de ne pas utiliser les mots clés réservés comme noms d'objets. Bases de données mis à niveau à partir de versions antérieures de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peuvent contenir des identificateurs qui incluent des mots ne réservés pas dans la version antérieure, mais sont des mots réservés pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. Aussi longtemps que vous ne modifiez pas l'identificateur de l'objet, vous pouvez faire référence à l'objet à l'aide d'un identificateur délimité.  
  
-   Lorsque le nom d'un objet utilise des caractères non répertoriés comme identificateurs qualifiés.  
  
     [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] permet à un identificateur délimité à utiliser n’importe quel caractère dans la page de codes actuelle. Un manque de discernement dans l'utilisation de caractères spéciaux dans un nom d'objet peut rendre difficiles la lecture et la maintenance des instructions et scripts MDX.  
  
### <a name="formatting-rules-for-delimited-identifiers"></a>Règles de mise en forme des identificateurs délimités  
 Le corps d'un identificateur délimité peut contenir n'importe quelle combinaison de caractères dans la page de codes actuelle, notamment les caractères de délimitation proprement dits. Si le corps de l'identificateur délimité contient des caractères de délimitation, un traitement particulier est nécessaire :  
  
-   Si le corps de l'identificateur ne contient qu'un crochet gauche ([), aucun traitement supplémentaire n'est requis.  
  
-   Si le corps de l'identificateur contient un crochet droit (]), vous devez spécifier deux crochets droits (]]).  
  
### <a name="examples-of-delimited-identifiers"></a>Exemples d'identificateurs délimités  
 Dans l'instruction MDX hypothétique suivante, `Sales Volume`, `Sales Cube` et `select` sont des identificateurs délimités :  
  
 `-- The [Sales Volume] and [Sales Cube] identifiers contain a space.`  
  
 `SELECT Measures.[Sales Volume]`  
  
 `FROM [Sales Cube]`  
  
 `WHERE Product.[select]`  
  
 `-- The [select] identifier is a reserved keyword.`  
  
 Dans l'exemple suivant, le nom d'un objet est `Total Profit [Domestic]`. Pour faire référence à cet objet, vous devez utiliser l'identificateur délimité suivant :  
  
 `[Total Profit [Domestic]]]`  
  
 Remarquez qu'il n'a pas été nécessaire de modifier le crochet gauche situé avant `Domestic` pour créer l'identificateur délimité. Cependant le crochet droit qui suit `Domestic` a dû être remplacé par deux crochets droits.  
  
### <a name="delimiting-identifiers-with-multiple-parts"></a>Identificateurs de délimitation en plusieurs parties  
 Lorsque vous utilisez des noms d’objet qualifié, vous devrez délimiter plusieurs des identificateurs qui le composent le nom d’objet. Par exemple, l'identificateur Front Brakes du code suivant doit être délimité.  
  
 SELECT [Measures].MEMBERS ON COLUMNS,  
  
 [Product].[Product].[Front Brakes] ON ROWS  
  
 FROM [Adventure Works]  
  
 En outre, l'identificateur Measures de l'exemple précédent a été délimité pour illustrer la délimitation de plusieurs identificateurs.  
  
## <a name="see-also"></a>Voir aussi  
 [Référence du langage MDX & #40 ; MDX & #41 ;](../mdx/mdx-language-reference-mdx.md)   
 [Principes de base de requête MDX & #40 ; Analysis Services & #41 ;](../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)   
 [Éléments de syntaxe MDX &#40;MDX&#41;](../mdx/mdx-syntax-elements-mdx.md)  
  
  
