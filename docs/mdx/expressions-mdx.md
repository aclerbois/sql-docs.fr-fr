---
title: Les expressions (MDX Multidimensional) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 79909f574d818a599f30ad051be9cf6a980430a9
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579481"
---
# <a name="expressions-mdx"></a>Expressions (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Une expression est une combinaison d’identificateurs, de valeurs et d’opérateurs qui [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peut évaluer pour obtenir un résultat. Les données peuvent être utilisées à différents endroits lors de l'accès ou la modification des données. Vous pouvez, par exemple, utiliser une expression en tant qu'élément des données à récupérer dans une requête ou en tant que condition de recherche de données répondant à un jeu de critères.  
  
## <a name="simple-and-complex-expressions"></a>Expressions simples et complexes  
 Dans la syntaxe MDX, une expression peut être simple ou complexe :  
  
 Une expression simple peut être l'une des expressions suivantes :  
  
 Constante  
 Dans la syntaxe MDX, une constante est un symbole représentant une valeur unique spécifique. Les valeurs de chaîne, numériques et de date peuvent être restituées sous forme de constantes. Contrairement aux constants numériques, les constantes de chaîne et de date doivent être délimitées par des guillemets simples (').  
  
 Fonction scalaire  
 Dans la syntaxe MDX, une fonction scalaire retourne une valeur unique au sein du contexte d'évaluation. Cette distinction est importante pour comprendre la manière dont MDX résout les fonctions scalaires, car la plupart des expressions, instructions et scripts MDX ne sont pas évalués sur un élément de données unique, mais bien de façon itérative sur un groupe d'éléments de données, tels que ces cellules ou des membres. Cependant, au moment de l'évaluation de la fonction scalaire, elle vérifie généralement un élément de données unique.  
  
 Identificateur d'objet  
 La syntaxe MDX est orientée objet en raison de la nature des données multidimensionnelles. Les identificateurs d'objet sont considérés comme des expressions simples dans la syntaxe MDX. Pour plus d’informations sur les identificateurs, consultez [identificateurs &#40;MDX&#41;](../mdx/identifiers-mdx.md).  
  
 Une expression complexe peut être une combinaison de ces entités associées par des opérateurs.  
  
## <a name="expression-results"></a>Résultats des expressions  
 Pour une expression simple composée d'une seule constante ou variable ou d'une seule fonction scalaire ou nom de colonne, les type de données, classement, précision, échelle et valeur de l'expression sont ceux de l'élément référencé. Comme la syntaxe MDX ne prend directement en charge que le type de données OLE VARIANT, la contrainte ne doit se produire que lors de l'utilisation d'expressions simples.  
  
 Pour une expression complexe, la contrainte peut se produire lors de l'utilisation de plusieurs expressions simples possédant différents types de données.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 La requête suivante affiche des exemples de mesures calculées dont les définitions sont des expressions simples :  
  
 `WITH`  
  
 `MEMBER MEASURES.CONSTANTVALUE AS 1`  
  
 `MEMBER MEASURES.SCALARFUNCTION AS [Date].[Calendar Year].CURRENTMEMBER.NAME`  
  
 `MEMBER MEASURES.OBJECTIDENTIFIER AS [Measures].[Internet Sales Amount]`  
  
 `SELECT {MEASURES.CONSTANTVALUE,MEASURES.SCALARFUNCTION,MEASURES.OBJECTIDENTIFIER } ON 0,`  
  
 `[Date].[Calendar Year].MEMBERS ON 1`  
  
 `FROM [Adventure Works]`  
  
 Une expression peut également être un calcul, tel que `[Measures].[Discount Amount] * 1.5`. L'exemple suivant illustre l'utilisation d'un calcul pour définir un membre dans une instruction MDX SELECT :  
  
```  
WITH   
   MEMBER [Measures].[Special Discount] AS  
   [Measures].[Discount Amount] * 1.5  
SELECT   
   [Measures].[Special Discount] on COLUMNS,  
   NON EMPTY [Product].[Product].MEMBERS  ON Rows  
FROM [Adventure Works]  
WHERE [Product].[Category].[Bikes]  
```  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Utilisation d’expressions de cube et de sous-cube](../mdx/using-cube-and-subcube-expressions.md)|Définit les expressions de cube et de sous-cube.|  
|[Utilisation d’expressions de dimension](../mdx/using-dimension-expressions.md)|Définit les expressions de dimension.|  
|[Utilisation d’expressions de membre](../mdx/using-member-expressions.md)|Définit les expressions de membre.|  
|[Utilisation d’expressions de tuple](../mdx/using-tuple-expressions.md)|Définit les expressions de tuple.|  
|[Utilisation d’expressions de jeu](../mdx/using-set-expressions.md)|Définit les expressions de jeu.|  
|[Utilisation d’expressions scalaires](../mdx/using-scalar-expressions.md)|Définit les expressions scalaires.|  
|[Manipulation de valeurs vides](../mdx/working-with-empty-values.md)|Décrit une valeur vide et la manière dont les valeurs de ce type sont gérées.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence du langage MDX &#40;MDX&#41;](../mdx/mdx-language-reference-mdx.md)   
 [Principes de base de requête MDX &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)  
  
  
