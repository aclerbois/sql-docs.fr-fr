---
title: Action d’exploration (Générateur de rapports et SSRS) | Microsoft Docs
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
f1_keywords:
- "10249"
- "10186"
- "10092"
- "10167"
- "10174"
- sql13.rtp.rptdesigner.charttitleproperties.visibility.f1
- "10155"
- sql13.rtp.rptdesigner.chartproperties.visibility.f1
- sql13.rtp.rptdesigner.pictureproperties.visibility.f1
- sql13.rtp.rptdesigner.majorgridlineproperties.visibility.f1
- "10123"
- "10425"
- sql13.rtp.rptdesigner.axisproperties.visibility.f1
- "10217"
- "10161"
- "10215"
- sql13.rtp.rptdesigner.legendproperties.visibility.f1
- "10258"
- "10144"
- sql13.rtp.rptdesigner.subreportproperties.visibility.f1
- sql13.rtp.rptdesigner.textboxproperties.visibility.f1
- "10062"
- sql13.rtp.rptdesigner.serieslabelproperties.visibility.f1
- sql13.rtp.rptdesigner.rectangleproperties.visibility.f1
- sql13.rtp.rptdesigner.calculatedseriesproperties.visibility.f1
- sql13.rtp.rptdesigner.chartareaproperties.visibility.f1
- "10053"
- sql13.rtp.rptdesigner.minorgridlineproperties.visibility.f1
- sql13.rtp.rptdesigner.seriesproperties.visibility.f1
ms.assetid: 1f8d1ef2-0daf-40c6-9ba7-3b391249bcd4
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: db793e5608aca874a98f6125136429244eb5ffff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drilldown-action-report-builder-and-ssrs"></a>Action d'exploration (Générateur de rapports et SSRS)
  En fournissant des icônes plus et moins (+/-) dans une zone de texte, vous pouvez permettre aux utilisateurs de masquer et d'afficher des éléments de manière interactive. Cela s'appelle une action d' *exploration* . Pour une table ou une matrice, vous pouvez afficher ou masquer des colonnes et des lignes statiques ou des colonnes et des lignes qui sont associées à des groupes.  
  
 ![rs_drilldown](../../reporting-services/report-design/media/rs-drilldown.gif "rs_drilldown")  
  
 Dans cette illustration, l'utilisateur clique sur les signes plus (+) situés dans le rapport pour afficher les données de détail.  
  
 Par exemple, vous pouvez masquer à la base toutes les lignes sauf la ligne de synthèse d'un groupe externe dans le cadre d'une table avec des groupes de lignes. Pour chaque groupe interne (y compris le groupe de détails), vous pouvez ajouter une icône Développer/Réduire à la cellule de regroupement du groupe conteneur. Lors du rendu du rapport, l'utilisateur peut cliquer sur la zone de texte pour développer ou réduire les données de détail. Pour plus d’informations, consultez [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md).  
  
 Pour autoriser les utilisateurs à développer ou réduire un élément, définissez les propriétés de visibilité de cet élément.  
  
> [!NOTE]  
>  Lorsque vous créez un rapport avec une action d'exploration, les informations de visibilité doivent être définies sur le groupe, la colonne ou la ligne à masquer, et pas simplement sur une zone de texte unique dans la ligne ou la colonne. De plus, la zone de texte que vous utilisez pour la bascule doit figurer dans une étendue contenante qui contrôle l'élément que vous souhaitez afficher ou masquer.  
>   
>  Par exemple, pour masquer une ligne associée à un groupe imbriqué, la zone de texte doit figurer dans une ligne associée au groupe parent ou à un niveau plus élevé dans la hiérarchie de la relation contenant-contenu.  
>   
>  Pour plus d’informations sur la configuration des informations de visibilité d’un groupe, d’une colonne ou d’une ligne, consultez [Ajouter une action Développer ou Réduire à un élément &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-expand-or-collapse-action-to-an-item-report-builder-and-ssrs.md).  
  
 Pour plus d’informations sur le masquage des éléments de rapport, consultez [Masquer un élément &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/hide-an-item-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="comparing-drilldown-and-drillthrough-reports"></a>Comparaison entre les rapports d'exploration et d'extraction  
 Dans un rapport d'extraction, un utilisateur clique sur un bouton plus ou moins afin de développer ou réduire une section d'un rapport et d'afficher les données de détail présentes. Dans un rapport d'extraction, l'utilisateur clique sur le lien d'une valeur de synthèse, ce qui entraîne l'ouverture d'un rapport associé distinct permettant d'afficher des données de détail. Les données de détail sont uniquement récupérées au moment de l'exécution du rapport détaillé. En règle générale, les rapports d'extraction requièrent moins de ressources que les rapports d'exploration. Pour plus d’informations, consultez [Extraction, exploration, sous-rapports et régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drillthrough-drilldown-subreports-and-nested-data-regions.md).  
  
## <a name="rendering-extension-support-for-hidden-report-items"></a>Prise en charge des extensions de rendu pour les éléments de rapport masqués  
 La bascule d'affichage/de masquage d'éléments de rapport n'est prise en charge que par les extensions de rendu gérant l'interactivité utilisateur, par exemple l'extension de rendu HTML utilisée lorsque vous exécutez un rapport dans le Générateur de rapports et le Gestionnaire de rapports. D'autres extensions de rendu affichent des éléments masqués. La visibilité conditionnelle des éléments de rapport prise en charge est listée ci-dessous :  
  
-   En HTML, si des éléments sont masqués, ils ne sont pas visibles dans la source HTML.  
  
-   L'extension de rendu XML affiche tous les éléments de rapport, qu'ils soient masqués ou non.  
  
-   L'extension de rendu Excel affiche et développe les lignes et les colonnes masquées d'une table, d'une matrice ou d'une liste. Toutes les lignes et les colonnes sont visibles.  
  
 Pour plus d’informations, consultez [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Extraction, exploration, sous-rapports et régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drillthrough-drilldown-subreports-and-nested-data-regions.md)   
 [Tri interactif, Explorateurs de documents et liens &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-document-maps-and-links-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
  
  
