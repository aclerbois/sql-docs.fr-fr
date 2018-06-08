---
title: MemberValue (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 33e91cadc6a63f6b55403aed552c6a15c8afc03d
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580111"
---
# <a name="membervalue-mdx"></a>MemberValue (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la valeur d'un membre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Member_Expression.MemberValue  
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui prend la valeur d'un membre.  
  
## <a name="return-value"></a>Valeur de retour  
 La valeur de membre retournée contient les informations suivantes, répertoriées ci-dessous selon leur ordre d'apparition dans cette valeur :  
  
-   La liaison de la valeur, si elle a été définie.  
  
-   La clé avec le type de données d'origine, s'il n'y a aucune liaison de nom ou si la clé et la légende sont liées à la même colonne.  
  
-   La légende du membre.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-après retourne la liaison de la valeur, la clé de membre et la légende de la première date dans la dimension Date du cube Adventure Works.  
  
```  
WITH MEMBER Measures.ValueColumn as [Date].[Calendar].[July 1, 2001].MemberValue  
MEMBER Measures.KeyColumn as [Date].[Calendar].[July 1, 2001].Member_Key  
MEMBER Measures.NameColumn as [Date].[Calendar].[July 1, 2001].Member_Name  
  
SELECT {Measures.ValueColumn, Measures.KeyColumn, Measures.NameColumn}  ON 0  
from [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
