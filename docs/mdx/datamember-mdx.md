---
title: DataMember (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2979799686ad1d97018471111c57670383f7f4f8
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577851"
---
# <a name="datamember-mdx"></a>DataMember (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le membre de données générées par le système associé à un membre non-feuille d'une dimension.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Member_Expression.DataMember  
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
## <a name="remarks"></a>Notes  
 Cette fonction fonctionne sur des membres non-feuille dans n’importe quelle hiérarchie et peut être utilisée par le [UPDATE CUBE Statement (MDX)](../mdx/mdx-data-manipulation-update-cube.md) commande pour l’écriture différée des données à un membre non-feuille directement, plutôt qu’à des descendants du membre feuille.  
  
> [!NOTE]  
>  Retourne le membre spécifié si ce dernier est un membre feuille ou si le membre non-feuille n'est associé à aucun membre de données.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant utilise le **DataMember** fonction dans une mesure calculée pour afficher le quota de ventes pour chaque employé individuel :  
  
```  
WITH MEMBER measures.InvidualQuota AS   
([Employee].[Employees].currentmember.datamember, [Measures].[Sales Amount Quota])  
,FORMAT_STRING='Currency'  
SELECT {[Measures].[Sales Amount Quota],[Measures].InvidualQuota} ON COLUMNS,  
[Employee].[Employees].MEMBERS ON ROWS  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)   
 [Concepts clés pour MDX &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md)  
  
  
