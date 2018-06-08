---
title: Trier des données dans une région de données (Générateur de rapports et SSRS)| Microsoft Docs
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
ms.assetid: 2fcb9be2-1daa-4c92-ad00-5f63cdf39f70
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 642d8f6d08c21daa478bbc159b2fe71461ba0391
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sort-data-in-a-data-region-report-builder-and-ssrs"></a>Trier des données dans une région de données (Générateur de rapports et SSRS)
  Pour changer l'ordre de tri des données dans une région de données à la première exécution d'un rapport, vous devez définir l'expression de tri sur le groupe ou la région de données. Par défaut, l'expression de tri d'un groupe a automatiquement la même valeur que l'expression de groupe.  
  
-   Dans une région de données de tableau matriciel, définissez l'expression de tri pour la région de données ou pour chaque groupe, y compris pour le groupe de détails. Si une région de données de tableau matriciel ne contient qu’un seul groupe de détails, vous pouvez définir une expression de tri dans la requête, sur la région de données ou sur le groupe de détails ; l’effet sera identique.  
  
-   Dans une région de données de graphique, définissez l’expression de tri pour les groupes de catégories et de séries afin de contrôler l’ordre de tri de chaque de groupe. L'ordre des couleurs dans une légende de graphique est déterminé par l'expression de tri des points de données dans le groupe de catégories.  
  
-   Dans une région de données de jauge, vous n'avez généralement pas besoin de trier les données, la jauge affichant une valeur unique relative à une plage. Si vous devez trier les données dans une jauge, définissez d’abord un groupe, puis l’expression de tri pour ce dernier.  
  
 Pour plus d’informations, consultez [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md).  
  
 Pour une région de données de tableau matriciel, vous pouvez également ajouter un bouton de tri interactif en haut d’un en-tête de colonne pour permettre aux utilisateurs de modifier l’ordre de tri des groupes ou des lignes de détails. Pour plus d’informations, consultez [Tri interactif &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-sort-data-in-a-tablix-data-region"></a>Pour trier les données dans une région de données de tableau matriciel  
  
1.  Dans l’aire de conception, cliquez avec le bouton droit sur un descripteur de ligne, puis cliquez sur **Propriétés du tableau matriciel**.  
  
2.  Cliquez sur **Tri**.  
  
3.  Pour chaque expression de tri, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Tapez ou sélectionnez une expression selon laquelle trier les données.  
  
    3.  Dans la liste déroulante de la colonne **Ordre** , sélectionnez le sens de tri de chaque expression. **A-Z** trie l’expression par ordre croissant. **Z-A** trie l’expression par ordre décroissant.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-sort-values-in-a-group-including-the-details-group-for-a-tablix"></a>Pour trier les valeurs dans un groupe, y compris dans un groupe de détails, d'un tableau matriciel  
  
1.  Dans l'aire de conception, cliquez dans la région de données de tableau matriciel pour la sélectionner. Le volet Regroupement affiche les groupes de lignes et de colonnes de la région de données de tableau matriciel.  
  
2.  Dans le volet Groupes de lignes, cliquez avec le bouton droit sur le nom du groupe, puis cliquez sur **Modifier le groupe**.  
  
3.  Dans la boîte de dialogue **Groupe de tableaux matriciels** , cliquez sur **Trier**.  
  
4.  Pour chaque expression de tri, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Tapez ou sélectionnez une expression selon laquelle trier les données.  
  
    3.  Dans la liste déroulante de la colonne **Ordre** , sélectionnez le sens de tri de chaque expression. **A-Z** trie l’expression par ordre croissant. **Z-A** trie l’expression par ordre décroissant.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-sort-x-axis-labels-in-alphabetical-order-on-a-chart"></a>Pour trier les étiquettes de l'axe X par ordre alphabétique sur un graphique  
  
1.  Cliquez avec le bouton droit sur un champ de la zone de dépôt Champ de catégorie, puis cliquez sur **Propriétés du groupe de catégories**.  
  
2.  Dans la boîte de dialogue **Propriétés du groupe de catégories** , cliquez sur **Tri**.  
  
3.  Pour chaque expression de tri, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Sélectionnez l'expression qui correspond à votre champ de regroupement. Vous pouvez vérifier l’expression du champ de regroupement en cliquant sur **Regroupement**.  
  
    3.  Dans la liste déroulante de la colonne **Ordre** , sélectionnez le sens de tri de chaque expression. **A-Z** trie l’expression par ordre alphabétique croissant. **Z-A** trie l’expression par ordre alphabétique décroissant.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-sort-the-data-points-in-ascending-or-descending-order-on-a-chart"></a>Pour trier les points de données par ordre croissant ou décroissant sur un graphique  
  
1.  Cliquez avec le bouton droit sur un champ de la zone de dépôt Champ de catégorie, puis cliquez sur **Propriétés du groupe de catégories**.  
  
2.  Dans la boîte de dialogue **Propriétés du groupe de catégories** , cliquez sur **Tri**.  
  
3.  Pour chaque expression de tri, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Sélectionnez l'expression qui correspond à votre champ de données. Dans la plupart des cas, il s’agit d’une valeur agrégée, telle que `=Sum(Fields!Quantity.Value)`.  
  
    3.  Dans la liste déroulante de la colonne **Ordre** , sélectionnez le sens de tri de chaque expression. **A-Z** trie l’expression par ordre croissant. **Z-A** trie l’expression par ordre décroissant.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-sort-data-in-ascending-or-descending-order-for-display-on-a-gauge"></a>Pour trier les données par ordre croissant ou décroissant sur l'affichage d'une jauge  
  
1.  Cliquez avec le bouton droit sur la jauge, puis cliquez sur **Ajouter un groupe de données**.  
  
2.  Dans la boîte de dialogue **Propriétés du groupe de panneaux de jauges** , cliquez sur **Général** si nécessaire.  
  
3.  Dans **Expressions Groupe**, cliquez **Ajouter**.  
  
4.  Dans **Regrouper sur**, tapez ou sélectionnez une expression selon laquelle regrouper les données.  
  
5.  Répétez les étapes 3 et 4 autant de fois que nécessaire pour ajouter toutes les expressions de groupe que vous souhaitez utiliser.  
  
6.  Cliquez sur **Tri**.  
  
7.  Pour chaque expression de tri, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Sélectionnez l'expression qui correspond à votre champ de regroupement. Vous pouvez vérifier l’expression du champ de regroupement en cliquant sur **Regroupement**.  
  
    3.  Dans la liste déroulante de la colonne **Ordre** , sélectionnez le sens de tri de chaque expression. **A-Z** trie l’expression par ordre croissant. **Z-A** trie l’expression par ordre décroissant.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Pour plus d’informations sur la façon dont les données sont regroupées dans une jauge, consultez [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Aide du Générateur de rapports pour les boîtes de dialogue, les volets et les Assistants](http://msdn.microsoft.com/en-us/2da24891-0b6d-4d3c-8b18-81b98752642f)   
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Mise en forme des étiquettes des axes sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [Spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs.md)  
  
  
