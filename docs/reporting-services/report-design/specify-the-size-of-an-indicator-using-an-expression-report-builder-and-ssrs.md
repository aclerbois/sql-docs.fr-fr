---
title: Spécifier la taille d’un indicateur à l’aide d’une expression (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: ab0b86f1-4882-4258-a2b6-c612faecfa4b
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 9772cf6ae8d1c5fdb9b28aa076fca3b25e612d0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-the-size-of-an-indicator-using-an-expression-report-builder-and-ssrs"></a>Spécifier la taille d'un indicateur à l'aide d'une expression (Générateur de rapports et SSRS)
  Outre la couleur, la direction et la forme, vous pouvez utiliser la taille pour optimiser l'impact visuel des indicateurs.  
  
 Un indicateur a une collection d’états nommée IndicatorStates. La collection IndicatorStates a en général plusieurs états. Chaque état est un membre de la collection et est représenté par une icône. Ensemble, les états constituent la collection IndicatorsStates.  
  
 Pour configurer dynamiquement les tailles des icônes, vous définissez les propriétés des membres de la collection IndicatorsStates dans le volet Propriétés du Générateur de rapports. Si le volet **Propriétés** n'est pas visible, sélectionnez **Propriétés** sous l'onglet **Affichage**.  
  
> [!NOTE]  
>  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous utilisez la fenêtre de **Propriétés** pour définir les propriétés de membre. Si la fenêtre **Propriétés** n'est pas ouverte, appuyez sur la touche F4.  
  
 Le volet **Propriétés** fournit l’accès aux propriétés de la collection IndicatorStates d’un indicateur. Vous configurez des icônes de différentes tailles en définissant la propriété ScaleFactor des membres de collection IndicatorStates à l’aide d’une expression. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
 L’expression utilisée dans cette procédure a également été utilisée pour créer le rapport avec différentes tailles d’indicateurs, illustré dans [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-specify-the-indicator-icon-size-using-an-expression"></a>Pour spécifier la taille de l'icône d'indicateur à l'aide d'une expression  
  
1.  Cliquez sur l'indicateur que vous souhaitez changer.  
  
2.  Dans le volet Propriétés, repérez la propriété IndicatorStates.  
  
     Si le volet Propriétés est organisé par catégorie, vous trouverez IndicatorStates dans la catégorie **States** .  
  
3.  Cliquez sur le bouton de sélection **(...)** en regard d’IndicatorStates. La boîte de dialogue relative à l' **Éditeur de collections IndicatorState** s'ouvre.  
  
     Sélectionnez tous les membres de la collection.  
  
4.  Dans la liste **Sélectionner plusieurs propriétés** , cliquez sur la flèche vers le bas en regard de ScaleFactor, puis sur **Expression**.  
  
5.  Dans la boîte de dialogue **Expression** , écrivez l'expression.  
  
     L'exemple d'expression suivant crée une icône de taille différente selon la valeur du champ **SalesYTD** .  
  
     `=IIF(Fields!SalesYTD.value = 0,0,Fields!SalesYTD.value/Max(Fields!SalesYTD.value,"Indicator"))`  
  
     Pour plus d’informations, consultez [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md).  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
  
