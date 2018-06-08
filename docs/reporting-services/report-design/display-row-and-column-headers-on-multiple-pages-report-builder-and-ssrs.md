---
title: Afficher des en-têtes de ligne et de colonne sur plusieurs pages (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 2422b1e2-822f-4379-9d7f-9afebb350e3f
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 1865c120f784d7e184e24ede5ccb6943b7ba439f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="display-row-and-column-headers-on-multiple-pages-report-builder-and-ssrs"></a>Afficher des en-têtes de ligne et de colonne sur plusieurs pages (Générateur de rapports et SSRS)
  Vous pouvez décider de répéter les en-têtes de ligne et de colonne sur chaque page d’un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] pour une région de données de tableau matriciel (une table, une matrice ou une liste) qui s’étend sur plusieurs pages.
  
 La façon dont vous contrôlez les lignes et les colonnes varie selon que la région de données de tableau matriciel possède ou non des en-têtes de groupes. Lorsque vous cliquez dans une région de données du tableau matriciel qui possède des en-têtes de groupes, une ligne pointillée indique les zones de tableau matriciel, comme illustré sur la figure suivante :  
  
 ![Tablix data region areas](../../reporting-services/report-design/media/rs-tablixareas.gif "Tablix data region areas")  
  
 Les en-têtes de groupe de lignes et de colonnes sont créés automatiquement lorsque vous ajoutez des groupes à l'aide de l'Assistant Nouveau tableau ou nouvelle matrice ou de l'Assistant Nouveau graphique, en ajoutant des champs au volet Regroupement ou à l'aide des menus contextuels. Si la région de données de tableau matriciel ne possède qu'un corps de tableau matriciel et aucun en-tête de groupe, les lignes et colonnes sont des membres du tableau matriciel.  
  
 Pour les membres statiques, vous pouvez afficher les lignes supérieures adjacentes ou les colonnes latérales adjacentes sur plusieurs pages.  
  
## <a name="to-display-row-headers-on-multiple-pages"></a>Pour afficher les en-têtes de ligne sur plusieurs pages  
  
1.  Cliquez avec le bouton droit sur la ligne, la colonne ou la poignée d’angle d’une région de données de tableau matriciel, puis sélectionnez **Propriétés du tableau matriciel**.  
  
2.  Dans **En-têtes de lignes**, sélectionnez **Répéter les lignes d'en-tête sur chaque page**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-display-column-headers-on-multiple-pages"></a>Pour afficher les en-têtes de colonne sur plusieurs pages  
  
1.  Cliquez avec le bouton droit sur la ligne, la colonne ou la poignée d’angle d’une région de données de tableau matriciel, puis sélectionnez **Propriétés du tableau matriciel**.  
  
2.  Dans **En-têtes de colonnes**, sélectionnez **Répéter les colonnes d'en-tête sur chaque page**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-display-a-static-row-or-column-on-multiple-pages"></a>Pour afficher une liste ou une colonne sur plusieurs pages  
  
1.  Dans l'aire de conception, cliquez sur la poignée de ligne ou de colonne de la région de données du tableau matriciel pour la sélectionner. Le volet Regroupement affiche les groupes de lignes et de colonnes.  
  
2.  Dans la partie droite du volet de regroupement, cliquez sur la flèche orientée vers le bas, puis sur **Mode avancé**. Le volet Groupes de lignes affiche les membres statiques et dynamiques hiérarchiques pour la hiérarchie de groupes de lignes et le volet Groupes de colonnes affiche une vue semblable pour la hiérarchie de groupes de colonnes.  
  
3.  Cliquez sur le membre statique correspondant au membre statique (ligne ou colonne) qui doit rester visible pendant le défilement. Le volet Propriétés affiche les propriétés du **membre du tableau matriciel** .  
  
     Si vous ne voyez pas le volet propriétés, cliquez sur l'onglet de **Afficher** en haut de la fenêtre du Générateur de rapports puis cliquez sur **Propriétés**.  
  
4.  Dans le volet Propriétés, affectez à **RepeatOnNewPage** la valeur True.  
  
5.  Définissez **KeepWithGroup** à Après.  
  
6.  Renouvelez cette opération pour tous les membres adjacents à répéter.  
  
7.  Affichez l'aperçu du rapport.  
  
 En consultant chacune des pages du rapport sur lesquelles la région de données du tableau matriciel s'étend, vous pouvez constater que les membres statiques du tableau matriciel se répètent sur chaque page.  
  
## <a name="see-also"></a> Voir aussi  
 [Recherche, affichage et gestion des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
 [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)   
 [Contrôle des sauts de page, des en-têtes, des colonnes et des lignes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/controlling-page-breaks-headings-columns-and-rows-report-builder-and-ssrs.md)   
 [Afficher des en-têtes et des pieds de page de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md)   
 [Laisser les en-têtes visibles lors du défilement d’un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/keep-headers-visible-when-scrolling-through-a-report-report-builder-and-ssrs.md)  
  
  
