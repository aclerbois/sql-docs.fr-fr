---
title: Inclure des indicateurs et des jauges dans un panneau de jauge (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 4dff9b67-b483-4c51-a822-6dbe706a6840
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: d74c221cdaa3a37f9345ef20bd067b2286d6aeff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="include-indicators-and-gauges-in-a-gauge-panel-report-builder-and-ssrs"></a>Inclure des indicateurs et des jauges dans un panneau de jauge (Générateur de rapports et SSRS)
  Le panneau de jauge est le conteneur de niveau supérieur qui contient un ou plusieurs jauges et indicateurs. Les indicateurs peuvent être incorporés dans les jauges ou placés en regard de celles-ci dans le panneau de jauge.  
  
 Si l'indicateur et la jauge sont adjacents dans le panneau de jauge et affichent des données de champs différents, vous pouvez ajouter des étiquettes pour clarifier les données transmises par la jauge et l'indicateur.  
  
 Les options de jauge et d'indicateur peuvent être définies à l'aide d'expressions. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-embed-an-indicator-in-a-gauge"></a>Pour incorporer un indicateur dans une jauge  
  
1.  Ouvrez un rapport existant ou créez un rapport qui contient une table et une matrice avec les données que vous souhaitez afficher.   
  
2.  Insérez une colonne dans votre table ou matrice. Pour plus d’informations, consultez [Insérer ou supprimer une colonne &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-column-report-builder-and-ssrs.md).  
  
3.  Sous l’onglet **Insérer** , dans le groupe **Régions de données** , cliquez sur **Jauge**, puis sur une cellule dans la nouvelle colonne. La boîte de dialogue **Sélectionner le type de jauge** s’affiche.  
  
4.  Cliquez sur **Radial**. La première jauge radiale est sélectionnée.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Cliquez sur la jauge. Le volet **Données de la jauge** s’ouvre.  
  
7.  Dans la zone **Valeurs** , dans la zone de liste **(Non spécifié)** , cliquez sur le champ dont vous souhaitez afficher les valeurs dans la jauge. Vous pouvez également faire glisser le champ à utiliser à partir du dataset du rapport.  
  
8.  Cliquez avec le bouton droit sur la jauge, cliquez sur **Ajouter un indicateur**, puis sur **Enfant**. La boîte de dialogue **Sélectionner un style d’indicateur** s’ouvre.  
  
9. Dans la boîte de dialogue **Sélectionner un style d’indicateur** , dans le volet gauche, cliquez sur le type d’indicateur de votre choix, puis sur le jeu d’indicateurs.  
  
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
11. Cliquez sur l'indicateur. Le volet **Données de la jauge** s’ouvre.  
  
12. Dans la zone **Valeurs** , dans la zone de liste **(Non spécifié)** , cliquez sur le champ dont vous souhaitez afficher les valeurs sous la forme d’un indicateur. Vous pouvez également faire glisser le champ à utiliser à partir du dataset du rapport.  
  
     Il peut s'agir du même champ que celui que vous utilisez dans la jauge ou d'un champ différent.  
  
13. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-show-an-indicator-and-gauge-side-by-side"></a>Pour afficher côte à côte un indicateur et une jauge  
  
1.  Ouvrez un rapport existant ou créez un rapport qui contient une table et une matrice avec les données que vous souhaitez afficher.  
  
2.  Insérez une colonne dans votre table ou matrice. Pour plus d’informations, consultez [Insérer ou supprimer une colonne &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-column-report-builder-and-ssrs.md).  
  
3.  Sous l’onglet **Insérer** , dans le groupe **Régions de données** , cliquez sur **Jauge**, puis sur une cellule dans la colonne que vous avez insérée. La boîte de dialogue **Sélectionner le type de jauge** s’affiche.  
  
4.  Cliquez sur **Radial**. La première jauge radiale est sélectionnée.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Cliquez sur la jauge. Le volet **Données de la jauge** s’ouvre.  
  
7.  Dans la zone **Valeurs** , dans la zone de liste **(Non spécifié)** , cliquez sur le champ dont vous souhaitez afficher les valeurs dans la jauge. Vous pouvez également faire glisser le champ à utiliser à partir du dataset du rapport.  
  
8.  Cliquez avec le bouton droit sur la jauge, cliquez sur **Ajouter un indicateur**, puis sur **Adjacent**. La boîte de dialogue **Sélectionner un style d’indicateur** s’ouvre.  
  
9. Dans la boîte de dialogue **Sélectionner un style d’indicateur** , dans le volet gauche, cliquez sur le type d’indicateur de votre choix, puis sur le jeu d’indicateurs.  
  
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
11. Cliquez sur l'indicateur. Le volet **Données de la jauge** s’ouvre.  
  
12. Dans la zone **Valeurs** , dans la zone de liste **(Non spécifié)** , cliquez sur le champ dont vous souhaitez afficher les valeurs sous la forme d’un indicateur. Vous pouvez également faire glisser le champ à utiliser à partir du dataset du rapport.  
  
     Il peut s'agir du même champ que celui que vous utilisez dans la jauge ou d'un champ différent.  
  
13. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
14. Cliquez avec le bouton droit sur le panneau de jauge, puis cliquez sur **Ajouter une étiquette**. Une étiquette est ajoutée au panneau de jauge. Répétez l'opération une fois de plus.  
  
     Le panneau de jauge a deux étiquettes.  
  
15. Faites glisser chaque étiquette vers un emplacement près de la jauge ou de l'indicateur.  
  
16. Cliquez avec le bouton droit sur l’étiquette près de la jauge, cliquez sur **Propriétés de l’étiquette**, puis tapez le texte que vous souhaitez dans la zone **Texte** .  
  
17. Cliquez avec le bouton droit sur l’étiquette près de l’indicateur, cliquez sur **Propriétés de l’étiquette**, puis tapez le texte que vous souhaitez dans la zone **Texte** .  
  
18. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
  
