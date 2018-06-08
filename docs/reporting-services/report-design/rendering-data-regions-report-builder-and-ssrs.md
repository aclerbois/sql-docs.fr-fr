---
title: Rendu des régions de données (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 4f3b2c7d-3669-457f-899b-b758d1db3426
caps.latest.revision: 6
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 567bb447bec9e6ff59269aa23d7ab7fc88fc89e1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rendering-data-regions-report-builder-and-ssrs"></a>Rendu des régions de données (Générateur de rapports et SSRS)
  En plus des comportements de rendu généraux qui s'appliquent à tous les éléments de rapport, les régions de données suivent des comportements de pagination et de rendu supplémentaires. Les règles de rendu spécifiques aux régions de données comprennent la croissance d'une région de données, le rendu de cellules spéciales, telles que la cellule d'angle ou les cellules d'en-tête, le rendu de la lecture de droite à gauche d'une région de données. Cette rubrique présente le rendu des différents composants d'une région de données.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="tablix-data-regions"></a>Régions de données de tableau matriciel  
 La région de données de tableau matriciel, qui vous permet de créer des tables, des matrices et des listes, est rendue sous forme d'une grille composée de colonnes et de lignes. L'intersection d'une ligne et d'une colonne est une cellule. Lors du rendu, cette cellule peut contenir des données ou d'autres éléments de rapport, tels que des images, des rectangles, des zones de texte ou des sous-rapports. Une région de données de tableau matriciel peut être agrandie verticalement et/ou horizontalement. De plus, la cellule d'angle, les cellules d'en-tête de région de données et les cellules de corps de la région de données peut être agrandies en fonction de leur contenu. Si la région de données s'étend sur plusieurs pages, les éléments de rapport configurés pour être répétés avec la région de données sont rendus sur chaque page sur laquelle la région de données est affichée. Pour plus d’informations, consultez [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md).  
  
### <a name="right-to-left"></a>De droite à gauche  
 Une région de données de tableau matriciel configurée pour un affichage de droite à gauche est rendue avec sa structure sous la forme d’une image miroir de la région de données si elle était rendue de gauche à droite. L'angle de la région de données apparaît dans l'angle supérieur droit. Si des colonnes dynamiques existent dans le rapport, elles se développent à gauche. Les paramètres de droite à gauche n'affectent pas l'ordre des données dans la région de données ; vos colonnes sont simplement organisées différemment.  
  
### <a name="tablix-headers"></a>En-têtes de tableau matriciel  
 Les en-têtes de tableau matriciel sont rendus sous la forme d'un en-tête de ligne ou de colonne selon que la cellule d'en-tête s'affiche dans la hiérarchie de groupe de ligne ou la hiérarchie de groupe de colonne. Si un saut de page logique existe dans le contenu de la cellule d'un en-tête, il est ignoré. Les sauts de page logiques sur les groupes de colonnes sont ignorés.  
  
 Les sauts de page logiques sur les groupes ne provoquent pas la coupure des en-têtes de groupe externes. Par exemple, votre rapport a un groupe externe de pays et un groupe interne de région de pays. Si un saut de page logique existe entre les instances du groupe de région de pays, le groupe externe, le pays, apparaîtra sur les deux pages du rapport.  
  
#### <a name="repeated-tablix-headers"></a>En-têtes de tableau matriciel répétés  
 Quand la propriété RepeatWith est définie dans le volet **Propriétés** , les éléments qui ne changent pas au sein de la région de données, tels que les en-têtes de colonne, sont répétés sur chaque page où cette partie de la région de données est rendue. Par exemple, si une ligne de données apparaît sur la page suivante et que la propriété RepeatWith est définie, les en-têtes de colonnes apparaissent également sur la page rendue.  
  
### <a name="tablix-corner"></a>Angle de tableau matriciel  
 L'angle supérieur gauche est appelé angle de tableau matriciel. L'angle de tableau matriciel peut contenir d'autres éléments de rapport mais, si les sauts de page logiques sont insérés dans l'angle, ils sont ignorés lorsque la région de données de tableau matriciel est rendue.  
  
### <a name="tablix-body"></a>Corps de tableau matriciel  
 Le corps de tableau matriciel est composé de cellules de tableau matriciel. Le corps de tableau matriciel est rendu en fonction des règles de pagination et des comportements de rendu d'éléments de rapport. Pour plus d’informations, consultez [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md).  
  
## <a name="chart-gauge-and-map-data-regions"></a>Régions de données de graphique, jauge et plan  
 Les régions de données de graphique, jauge et plan se comportent comme des images lorsqu'elles sont rendues et affichées dans le corps du rapport. Les valeurs au sein de la région de données peuvent avoir des actions associées, telles que la liaison à un autre rapport ou l'ouverture d'un signet, et ces actions peuvent également être rendues, si le convertisseur le prend en charge.  
  
## <a name="see-also"></a> Voir aussi  
 [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Fonctionnalités interactives des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)  
  
  
