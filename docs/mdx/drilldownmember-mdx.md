---
title: DrilldownMember (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9f50d68c383f758bfa75d8fd50964eaaf8c89fa4
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578061"
---
# <a name="drilldownmember-mdx"></a>DrilldownMember (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Extrait vers le bas les membres dans un jeu spécifié, qui sont présents dans un second jeu spécifié.  
  
 La fonction extrait également vers le bas un jeu de tuples à l'aide de la première hiérarchie de tuple ou de la hiérarchie éventuellement spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DrillDownMember(<Set_Expression1>, <Set_Expression2> [,[<Target_Hierarchy>]] [,[RECURSIVE][,INCLUDE_CALC_MEMBERS]])  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression1*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Set_Expression2*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Target_Hierarchy*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une hiérarchie.  
  
 *Récursive*  
 Mot clé qui indique une comparaison récursive de jeux.  
  
 *Include_Calc_Members*  
 Mot clé permettant d'activer les membres calculés à inclure dans les résultats de l'extraction vers le bas.  
  
## <a name="remarks"></a>Notes  
 Cette fonction retourne un jeu de membres enfants classés par hiérarchie et inclut les membres spécifiés dans le premier jeu qui sont également présents dans le deuxième jeu. Les membres parents ne seront pas extraits vers le bas si le premier jeu contient le membre parent et un ou plusieurs enfants. Le premier jeu peut avoir n'importe quelle dimensionnalité mais le deuxième jeu doit contenir un jeu unidimensionnel. L'ordre est conservé parmi les membres d'origine dans le premier jeu, à ceci près que tous les membres enfants inclus dans l'ensemble de résultats de la fonction sont placés immédiatement après leur membre parent. La fonction construit l'ensemble de résultats en récupérant les enfants de chaque membre dans le premier jeu également présent dans le deuxième jeu. Si **récursive** est spécifié, la fonction continue de manière récursive comparer les membres du jeu sur le deuxième jeu, en récupérant les enfants de chaque membre dans le résultat de résultats défini qui est également présent dans le deuxième jeu jusqu'à ce que plus aucun membre du jeu de résultats ne sont accessibles dans le deuxième jeu.  
  
 Interrogez la propriété XMLA **MdpropMdxDrillFunctions** vous permet de vérifier le niveau de prise en charge par le serveur pour les fonctions d’extraction, consultez [pris en charge les propriétés XMLA &#40;XMLA&#41; ](../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md)pour plus d’informations.  
  
 Le premier jeu peut contenir des tuples au lieu de membres. L'exploration vers le bas des tuples est une extension d'OLE DB et retourne un jeu de tuples à la place des membres.  
  
> [!IMPORTANT]  
>  Un membre ne fait pas l'objet d'une exploration vers le bas s'il est aussitôt suivi d'un de ses enfants. L’ordre des membres dans le jeu est important pour l’exploration vers le bas * et Drillup\* familles de fonctions.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous descend les niveaux concernant l'Australie, soit le membre du premier jeu également présent dans le deuxième jeu.  
  
```  
SELECT DrilldownMember   
   ( [Geography].[Geography].Children,  
      {[Geography].[Geography].[Country].[Australia],  
        [Geography].[Geography].[State-Province].[New South Wales]}  
   )  
   ON 0  
   FROM [Adventure Works]  
```  
  
 L'exemple ci-dessous descend les niveaux concernant l'Australie, soit le membre du premier jeu également présent dans le deuxième jeu. Néanmoins, du fait de la présence de l'argument RECURSIVE, la fonction continue à comparer de manière récursive les membres de l'ensemble des résultats (membres du niveau State-Province) par rapport au deuxième jeu et extrait simultanément les enfants de chaque membre de l'ensemble de résultats (membres du niveau City) également présent dans le deuxième jeu jusqu'à ce que plus aucun membre de l'ensemble de résultats ne puisse être détecté dans le deuxième jeu.  
  
```  
SELECT DrilldownMember   
   ( [Geography].[Geography].Children,  
      {[Geography].[Geography].[Country].[Australia],  
        [Geography].[Geography].[State-Province].[New South Wales]}  
   ,RECURSIVE)  
   ON 0  
   FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
