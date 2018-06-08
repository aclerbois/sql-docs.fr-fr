---
title: Opérateurs de concaténation | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a59ba79a2ba6cde723b79bb459734e7a8b5d752a
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578431"
---
# <a name="concatenation-operators"></a>Opérateurs de concaténation
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  L'opérateur de concaténation est le signe plus (+). Vous pouvez combiner, ou concaténer, deux ou plusieurs chaînes de caractères en une seule. Vous pouvez également concaténer des chaînes binaires.  
  
 Le code suivant est un exemple d'opérateur de concaténation combinant le nom du produit et son nom unique :  
  
```  
WITH MEMBER Measures.ProductName AS   
   Product.Product.CurrentMember.Name + " (" + Product.Product.CurrentMember.UniqueName + ")"  
SELECT   
   {Measures.ProductName} ON COLUMNS,  
   Product.Product.Members ON ROWS  
FROM [Adventure Works]  
```  
  
## <a name="language-considerations"></a>Observations relatives au langage  
 Lorsque les chaînes utilisées dans une concaténation possèdent toutes deux le même classement, la chaîne concaténée obtenue possède un classement identique aux entrées. Lorsque les chaînes utilisées dans une concaténation possèdent des classements différents, les règles de priorité de classement déterminent celui de la chaîne concaténée obtenue. Pour plus d’informations, consultez [Langues et classements &#40;Analysis Services&#41;](../analysis-services/languages-and-collations-analysis-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des opérateurs MDX &#40;MDX&#41;](../mdx/mdx-operator-reference-mdx.md)   
 [Opérateurs &#40;syntaxe MDX&#41;](../mdx/operators-mdx-syntax.md)  
  
  
