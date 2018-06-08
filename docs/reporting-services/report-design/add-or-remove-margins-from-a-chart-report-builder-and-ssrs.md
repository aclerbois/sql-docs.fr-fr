---
title: Ajouter ou supprimer des marges dans un graphique (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 91c43f58-5771-4d33-a54d-0e802d2f5cba
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: a42d78cca3bc4835771c2e9fcf30a184232aeb38
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-or-remove-margins-from-a-chart-report-builder-and-ssrs"></a>Ajouter ou supprimer des marges dans un graphique (Générateur de rapports et SSRS)
Pour les types de graphiques Histogrammes et Nuages de points dans les rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , le graphique ajoute automatiquement des marges latérales aux extrémités de l’axe des abscisses. Dans les graphiques à barres, le graphique ajoute automatiquement des marges latérales aux extrémités de l'axe des ordonnées. Dans tous les autres types de graphiques, aucune marge latérale n'est ajoutée. Vous ne pouvez pas modifier la taille de la marge.  
  
 Cette rubrique ne s'applique pas aux types de graphiques à secteurs, en anneau, en entonnoir ni en pyramide.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-enable-or-disable-side-margins"></a>Pour activer ou désactiver les marges latérales  
  
1.  Cliquez avec le bouton droit sur l’axe, puis sélectionnez **Propriétés de l’axe**. La boîte de dialogue **Propriétés de l’axe vertical** ou **Propriétés de l’axe** s’affiche.  
  
2.  Dans la page **Options de l’axe** , définissez la propriété **Marges latérales** :  
  
    -   **Automatique** Le graphique détermine s’il faut ajouter une marge latérale en fonction du type de graphique.  
  
    -   **Désactivé** Les graphiques à barres, histogrammes et nuages de points n’ont pas de marges latérales.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme des étiquettes des axes sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [Boîte de dialogue Propriétés de l’axe, Options de l’axe &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/b276e210-7a12-48ae-971b-7dabae51df11)   
 [Spécifier un intervalle d’axe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md)   
 [Mettre en forme les étiquettes des axes en tant que dates ou devises &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
  
