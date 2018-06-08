---
title: Ajouter une expression (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a60ee091-b4ed-41e1-9b6a-032c316cd03f
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: bf0668b86fa7792007bba585a3a5cd82a2647756
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-an-expression-report-builder-and-ssrs"></a>Ajouter une expression (Générateur de rapports et SSRS)
  Les expressions sont utilisées dans l'ensemble d'un rapport pour définir des propriétés d'éléments de rapport, des filtres, des groupes, un ordre de tri, des chaînes de connexion et des valeurs de paramètres. Les expressions commencent par un signe égal (=) et sont écrites en langage [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Elles sont évaluées au moment de l'exécution par le processeur de rapports, qui associe le résultat d'évaluation aux éléments de disposition du rapport.  
  
 Les expressions peuvent être simples ou complexes. Une expression simple fait référence à un élément unique dans une collection intégrée. Les expressions complexes peuvent contenir des constantes, opérateurs, éléments de collecte globaux et appels de fonction. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-add-an-expression-to-a-text-box"></a>Pour ajouter une expression à une zone de texte  
  
-   En mode **Conception** , cliquez sur la zone de texte sur l'aire de conception à laquelle ajouter une expression.  
  
    -   Pour une expression simple, tapez le texte affiché de l'expression dans la zone de texte. Par exemple, pour le champ de dataset Sales, tapez `[Sales]`.  
  
    -   Pour une expression complexe, cliquez avec le bouton droit sur la zone de texte, puis sélectionnez **Expression**. La boîte de dialogue **Expression** s'affiche. Tapez ou créez interactivement votre expression après le signe égal (=) dans le volet d'expression, puis cliquez sur OK.  
  
         L'expression apparaît sur l'aire de conception sous la forme `<<Expr>>`.  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme du texte et des espaces réservés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)   
 [Zones de texte &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/text-boxes-report-builder-and-ssrs.md)   
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)   
 [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)   
 [Boîte de dialogue Expression &#40;Générateur de rapports&#41;](http://msdn.microsoft.com/library/e89c4d97-5d41-4b55-8695-79329edac15d)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Ajouter du code à un rapport &#40;SSRS&#41;](../../reporting-services/report-design/add-code-to-a-report-ssrs.md)  
  
  
