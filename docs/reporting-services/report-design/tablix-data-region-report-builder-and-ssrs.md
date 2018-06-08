---
title: Région de données de tableau matriciel (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 99f83b32-4b86-4d40-973c-9a328d23ac8b
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 0d5360091fbf0ece381b8b7444fdd8e225238563
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tablix-data-region-report-builder-and-ssrs"></a>Région de données de tableau matriciel (Générateur de rapports et SSRS)
  Dans [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)], une région de données de tableau matriciel est un élément de rapport à disposition généralisée qui affiche les données d’un rapport paginé dans des cellules organisées en lignes et colonnes. Les données du rapport peuvent être des données de détail récupérées à partir de la source de données ou des données de détail agrégées organisées en groupes de votre invention. Chaque cellule de tableau matriciel peut contenir un élément de rapport, par exemple une zone de texte ou une image, ou une autre région de données telle qu'une région de tableau matriciel, un graphique ou une jauge. Pour ajouter plusieurs éléments de rapport à une cellule, commencez par ajouter un rectangle pour servir de conteneur. Ajoutez ensuite les éléments de rapport au rectangle.  
  
 Les régions de données de table, de matrice ou de liste sont représentées sur le ruban par des modèles pour la région de données de tableau matriciel sous-jacente. Quand vous ajoutez l’un de ces modèles à un rapport, vous ajoutez en fait une région de données de tableau matriciel optimisée pour un format de données spécifique. Par défaut, un modèle de table affiche les données de détail sous forme de grille, une matrice affiche les données de groupe sous forme de grille et une liste affiche les données de détail sous une forme libre.  
  
 Par défaut, chaque cellule de tableau matriciel dans une table ou une matrice contient une zone de texte. La cellule d'une liste contient un rectangle. Vous pouvez remplacer un élément de rapport par défaut par un autre élément de rapport, par exemple une image.  
  
 Lorsque vous définissez des groupes pour une table, une matrice ou une liste, le Générateur de rapports et le Concepteur de rapports ajoute des lignes et des colonnes à la région de données de tableau matriciel et utilise ces lignes ou colonnes pour afficher des données groupées.  
  
 Pour comprendre le fonctionnement de la région de données de tableau matriciel, il convient au préalable de comprendre les points suivants :  
  
*   la différence entre données de détail et données groupées ;  
  
*   les groupes, qui sont organisés comme des membres de hiérarchies de groupes en tant que groupes de lignes sur l'axe horizontal et en tant que groupes de colonnes sur l'axe vertical ;  
  
*  l'objectif des cellules de tableau matriciel dans les quatre zones d'une région de données de tableau matriciel : le corps, les en-têtes de groupes de lignes, les en-têtes de groupes de colonnes et l'angle ;  
  
*  les lignes et les colonnes statiques et dynamiques, et leur relation aux groupes.  
  
 Cet article décrit en détail ces concepts pour expliquer la structure que le Générateur de rapports et le Concepteur de rapports ajoutent quand vous ajoutez des modèles et créez des groupes, de façon à vous permettre de modifier la structure en fonction de vos propres besoins. Le Générateur de rapports et le Concepteur de rapports fournit plusieurs indicateurs visuels pour vous aider à reconnaître la structure de la région de données de tableau matriciel. Pour plus d’informations, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="understanding-detail-and-grouped-data"></a>Fonctionnement des données de détail et des données groupées  
 Les données de détail sont toutes les données d'un dataset de rapport telles qu'extraites de la source de données. Les données de détail correspondent essentiellement à ce que vous voyez dans le volet Résultats du Concepteur de requêtes lorsque vous exécutez une requête de dataset. Les données de détail réelles incluent des champs calculés que vous créez et sont restreintes par des filtres définis sur le dataset, la région de données, et le groupe de détails. Vous affichez les données de détail sur une ligne de détail en utilisant une expression simple telle que [Quantity]. Lorsque le rapport s'exécute, la ligne de détail se répète une fois pour chaque ligne dans les résultats de la requête au moment de l'exécution.  
  
 Les données groupées sont des données de détail organisées par une valeur que vous spécifiez dans la définition de groupe, telles que [SalesOrder]. Vous affichez les données groupées dans des lignes et des colonnes de groupe en utilisant des expressions simples qui agrègent les données groupées, par exemple [Sum(Quantity)]. Pour plus d’informations, consultez [Fonctionnement des groupes&#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
## <a name="understanding-group-hierarchies"></a>Fonctionnement des hiérarchies de groupes  
 Les groupes sont organisés comme des membres de hiérarchies de groupes. Les hiérarchies de groupes de lignes et de groupes de colonnes sont des structures identiques sur des axes différents. Pensez aux groupes de lignes comme à des groupes qui se développent vers le bas de la page et aux groupes de colonnes comme à des groupes qui se développent à travers la page.  
  
 Une arborescence représente des groupes de lignes et de colonnes imbriqués qui ont une relation de parent à enfant, comme par exemple une catégorie avec des sous-catégories. Le groupe parent est la racine de l'arborescence et les groupes enfants sont ses branches. Les groupes peuvent également avoir une relation adjacente indépendante, comme par exemple les ventes par secteur et les ventes par année. On appelle « forêt » plusieurs hiérarchies d'arborescence sans relation. Dans une région de données de tableau matriciel, les groupes de lignes et les groupes de colonnes sont représentés comme une forêt indépendante. Pour plus d’informations, consultez [Fonctionnement des groupes&#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
## <a name="understanding-tablix-data-region-areas"></a>Présentation des zones de régions de données de tableau matriciel  
 Une région de données de tableau matriciel peut comprendre quatre zones de cellules : l'angle, la hiérarchie de groupes de lignes, la hiérarchie de groupes de colonnes et le corps. Le corps de tableau matriciel existe toujours. Les autres zones sont facultatives.  
  
 Les cellules de la zone du corps de tableau matriciel affichent des données de détail et de groupe.  
  
 Les cellules dans la zone Groupes de lignes sont créées automatiquement lorsque vous créez un groupe de lignes. Ce sont par défaut des cellules d'en-tête de groupe de lignes et elles affichent des valeurs d'instance de groupe de lignes par défaut. Par exemple, lorsque vous faites un regroupement par [SalesOrder], les valeurs d'instance de groupe sont les commandes individuelles en fonction desquelles vous effectuez le regroupement.  
  
 Les cellules de la zone Groupes de colonnes sont créées automatiquement lorsque vous créez un groupe de colonnes. Ce sont par défaut des cellules d'en-tête de groupe de colonnes et elles affichent des valeurs d'instance de groupe de colonnes par défaut. Par exemple, lorsque vous faites un regroupement par [Year], les valeurs d'instance de groupe sont les années individuelles en fonction desquelles vous effectuez le regroupement.  
  
 Les cellules de la zone d’angle de tableau matriciel sont créées automatiquement quand vous définissez des groupes de lignes et des groupes de colonnes. Les cellules de cette zone peuvent afficher des étiquettes, ou vous pouvez fusionner les cellules et créer un titre.  
  
 Pour plus d’informations, consultez [Zones de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-areas-report-builder-and-ssrs.md).  
  
## <a name="understanding-static-and-dynamic-rows-and-columns"></a>Fonctionnement des lignes et des colonnes statiques et dynamiques  
 Une région de données de tableau matriciel organise les cellules en lignes et colonnes associées à des groupes. Les structures de groupes pour les groupes de lignes et les colonnes sont identiques. Cet exemple utilise des groupes de lignes, mais vous pouvez appliquer les mêmes concepts aux groupes de colonnes.  
  
 Une ligne est soit statique, soit dynamique. Une ligne statique n'est pas associée à un groupe. Lorsque le rapport s'exécute, une ligne statique est utilisée une fois. Les en-têtes et les pieds de page de table sont des lignes statiques. Les lignes statiques affichent des étiquettes et des totaux. Les cellules d'une ligne statique sont étendues à la région de données.  
  
 Une ligne dynamique est associée à un ou plusieurs groupes. Une ligne dynamique est utilisée une fois pour chaque valeur unique dans le groupe le plus interne. La portée des cellules d'une ligne dynamique s'étend au groupe de lignes et au groupe de colonnes les plus internes auxquels la cellule appartient.  
  
 Les lignes de détail dynamiques sont associées au groupe de détails créé automatiquement lorsque vous ajoutez une table ou une liste à l'aire de conception. Par définition, le groupe de détails est le groupe le plus interne dans une région de données de tableau matriciel. Les cellules des lignes de détail affichent des données de détail.  
  
 Des lignes de groupe dynamiques sont créées automatiquement lorsque vous ajoutez un groupe de lignes ou un groupe de colonnes à une région de données de tableau matriciel existante. Les cellules des lignes de groupe dynamiques affichent des valeurs agrégées pour l'étendue par défaut.  
  
 La fonction Ajouter un total crée automatiquement une ligne à l'extérieur du groupe actuel dans laquelle afficher les valeurs dont la portée est étendue au groupe. Vous pouvez également ajouter manuellement des lignes statiques et dynamiques. Des indicateurs visuels vous aident à comprendre quelles lignes sont statiques et quelles lignes sont dynamiques. Pour plus d’informations, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Liaison de plusieurs régions de données à un même dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)   
 [Contrôle de l’affichage de la région de données de tableau matriciel sur une page de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md)   
 [Exploration de la souplesse d’une région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/exploring-the-flexibility-of-a-tablix-data-region-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
