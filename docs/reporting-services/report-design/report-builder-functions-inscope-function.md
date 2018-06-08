---
title: Fonction InScope (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a8cd209a-e5d3-4dce-ab2d-f271f6c54955
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: a87fd01638f68b85832e2bc676b19abad7291feb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-builder-functions---inscope-function"></a>Fonctions du Générateur de rapports - InScope
  Indique si l'instance actuelle d'un élément se trouve dans l'étendue spécifiée.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Syntaxe  
  
```  
InScope(scope)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *portée*  
 (**Chaîne**) Nom d’un dataset, d’une région de données ou d’un groupe qui spécifie une étendue.  
  
## <a name="return-type"></a>Type de retour  
 Retourne une valeur **booléenne**.  
  
## <a name="remarks"></a>Notes   
 La fonction **InScope** teste l’étendue de l’instance actuelle d’un élément de rapport pour l’appartenance à l’étendue spécifiée par le paramètre *scope*.  
  
 *Scope* ne peut pas être une expression.  
  
 En règle générale, la fonction **InScope** est utilisée dans les régions de données avec définition d’étendue dynamique. Ainsi, la fonction **InScope** peut être utilisée dans un lien d’extraction situé dans les cellules d’une région de données pour fournir un autre nom de rapport et des jeux de paramètres différents en fonction de la cellule sur laquelle l’utilisateur clique. En voici un exemple :  
  
-   L’expression suivante, utilisée comme nom de rapport dans un lien d’extraction, ouvre le rapport `ProductDetail` si l’utilisateur clique sur une cellule située dans le groupe `Month` et le rapport `ProductSummary` s’il clique sur une autre cellule.  
  
    ```  
    =Iif(InScope("Month"), "ProductDetail", "ProductSummary")  
    ```  
  
-   L’expression suivante, utilisée dans la propriété **Omit** d’un paramètre de rapport d’extraction, passe le paramètre au rapport cible uniquement si la cellule sur laquelle l’utilisateur clique se trouve dans le groupe `Product` .  
  
    ```  
    =Not(InScope("Product"))  
    ```  
  
 Pour plus d’informations, consultez [Référence aux fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) et [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="example"></a> Exemple  
 L'exemple de code ci-dessous indique si l'instance actuelle de l'élément se trouve dans l'étendue du groupe, de la région de données ou du dataset `Product` .  
  
```  
=InScope("Product")  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
  
