---
title: Masquer des éléments de légende dans le graphique (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 92256240-0cd5-4be4-8904-d1e3b93cb6b3
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 04eafb5e3fe9a1ebbf6073b706f1635a43a715b1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="chart-legend---hide-items-report-builder"></a>Légende de graphique - Masquer des éléments (Générateur de rapports)
Par défaut, toute série ajoutée à un graphique dans un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui n’est pas à base de formes est ajoutée en tant qu’élément à la légende. Pour les graphiques à secteurs, en anneau, en entonnoir et en pyramide, toute série ajoutée au graphique ajoutera des points de données individuels à la légende.  
  
 Vous pouvez masquer tout élément de la légende. Lorsque vous masquez un élément de légende, celui-ci continuera à figurer dans le graphique. Cela peut s'avérer utile dans les cas où vous ne souhaiteriez pas afficher plus d'informations sur une série ajoutée au graphique. Par exemple, si vous avez ajouté une série calculée comme une moyenne mobile au graphique, vous pouvez masquer cette entrée dans la légende de manière à afficher davantage d'informations sur la série d'origine uniquement.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-hide-an-item-from-display-in-the-legend"></a>Pour masquer un élément dans la légende  
  
1.  Cliquez avec le bouton droit sur la série que vous souhaitez masquer et sélectionnez **Propriétés de la série**.  
  
2.  Cliquez sur **Légende**. Sélectionnez l'option **Ne pas afficher cette série dans une légende** .  
  
    > [!NOTE]  
    >  Vous ne pouvez pas masquer une série d'un groupe et pas des autres. Si vous avez ajouté un champ à la zone **Groupes de séries** , toutes les séries qui appartiennent à ce groupe seront masquées.  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme de la légende sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-legend-formatting-report-builder.md)   
 [Mise en forme des points de données sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
 [Modifier le texte d’un élément de légende &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md)   
 [Ajouter une moyenne mobile à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-moving-average-to-a-chart-report-builder-and-ssrs.md)   
 [Boîte de dialogue Propriétés de la légende, Général &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/db718f8f-f185-422f-871c-96f0749e5893)  
  
  
