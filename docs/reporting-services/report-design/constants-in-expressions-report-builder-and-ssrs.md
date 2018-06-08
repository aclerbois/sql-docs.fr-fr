---
title: Constantes dans les expressions (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b8ae650b-0f46-4848-b62b-15f8a40751b8
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: d639171653906294640e327abb25b08a99bc38dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="constants-in-expressions-report-builder-and-ssrs"></a>Constantes dans les expressions (Générateur de rapports et SSRS)
  Une constante se compose de texte littéral ou de texte prédéfini. Le processeur de rapports a accès aux constantes prédéfinies afin que, lorsque vous les incluez dans une expression, les valeurs qu'elles représentent soient substituées dans l'expression avant son évaluation.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="literal-text"></a>Texte littéral  
 Dans une expression, le texte littéral est le texte qui figure entre guillemets doubles. Vous pouvez également taper le texte directement dans une zone de texte sans guillemets doubles s'il ne fait pas partie d'une expression. Si la valeur de la zone de texte ne commence pas par un signe égal (=), le texte est traité comme texte littéral. Le tableau suivant répertorie plusieurs exemples de texte littéral dans une expression.  
  
|Constante|Texte affiché|Texte de l'expression|  
|--------------|------------------|---------------------|  
|Report run at:|<\<Expr>>|`="Report run at: " & Globals!ExecutionTime`|  
|Adventure Works Cycles|Adventure Works Cycles|Adventure Works Cycles|  
|[Texte affiché entre crochets]|\\[Texte affiché entre crochets\\]|[Texte affiché entre crochets]|  
  
## <a name="rdl-constants"></a>Constantes RDL  
 Vous pouvez utiliser des constantes définies en RDL (Report Definition Language) dans une expression. Dans la boîte de dialogue **Expression** , les constantes apparaissent lorsque vous créez une expression pour une propriété de rapport qui accepte uniquement certaines valeurs valides, également appelées types énumérés. Le tableau suivant contient deux exemples.  
  
|Propriété|Description|Valeurs|  
|--------------|-----------------|------------|  
|TextAlign|Valeurs valides pour aligner le texte dans une zone de texte.|Général, Gauche, Centre, Droit|  
|BorderStyle|Valeurs valides pour une ligne ajoutée à un rapport.|Par défaut, Aucune, Pointillés, Tirets, Unie, Double, Tiret-point, Tiret-point-point|  
  
## <a name="visual-basic-constants"></a>Constantes Visual Basic  
 Vous pouvez utiliser des constantes définies dans la bibliothèque d'exécutables [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] dans une expression. Par exemple, vous pouvez utiliser la constante **DateInterval.Day**. Pour la date du 10 janvier 2008, l'expression suivante retourne le nombre 10 :  
  
 `=DatePart("d",Globals!ExecutionTime)`  
  
## <a name="clr-constants"></a>Constantes CLR  
 Vous pouvez utiliser des constantes définies dans les classes CLR (Common Language Runtime) [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] dans une expression. Le tableau suivant contient un exemple de couleur définie par le système.  
  
|Constante|Description|  
|--------------|-----------------|  
|MistyRose|Lorsque vous créez une expression pour une propriété de rapport basée sur la couleur d'arrière-plan, vous pouvez spécifier une couleur par nom. Les noms valides sont répertoriés dans la boîte de dialogue **Expression** .|  
  
## <a name="see-also"></a> Voir aussi  
 [Boîte de dialogue Expression](http://msdn.microsoft.com/library/e6c74ccb-4594-4d4f-b958-618d710e34eb)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Boîte de dialogue Expression &#40;Générateur de rapports&#41;](http://msdn.microsoft.com/library/e89c4d97-5d41-4b55-8695-79329edac15d)  
  
  
