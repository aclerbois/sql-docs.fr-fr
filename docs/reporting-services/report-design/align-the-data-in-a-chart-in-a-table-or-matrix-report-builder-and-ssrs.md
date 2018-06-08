---
title: Aligner les données d’un graphique dans une table ou une matrice (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 75137575-d1bf-46d6-8527-5afc85eea5e1
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c67f71e74dc88fe501efe81b498ee571fd36c41f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="align-the-data-in-a-chart-in-a-table-or-matrix-report-builder-and-ssrs"></a>Aligner les données d'un graphique dans une table ou une matrice (Générateur de rapports et SSRS)
  Les graphiques sparkline et les barres de données sont des graphiques simples de petite taille qui communiquent beaucoup d'informations avec peu de détails superflus. Quand vous activez cette option dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , les valeurs de vos graphiques sparkline et barres de données sont alignées dans les différentes cellules de la table ou de la matrice, même s’il manque des valeurs dans les données sur lesquelles ils sont basés.  
  
 ![rs_SparklineAlignData](../../reporting-services/report-design/media/rs-sparklinealigndata.gif "rs_SparklineAlignData")  
  
 Dans cette image, l'histogramme affiche les ventes quotidiennes pour chaque employé. Notez que les jours pendant lesquels un employé ne génère aucun chiffre d'affaires, le graphique laisse un espace et aligne les jours suivants horizontalement. Il aligne également les graphiques verticalement en définissant les tailles des différents graphiques les unes par rapport aux autres. Pour plus d’informations, consultez [Graphiques sparkline et barres de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md).  
  
## <a name="align-the-data-in-a-sparkline-or-data-bar"></a>Aligner les données dans un graphique sparkline ou une barre de données  
  
1.  [Ajoutez un graphique sparkline ou une barre de données](../../reporting-services/report-design/add-sparklines-and-data-bars-report-builder-and-ssrs.md) à une table ou une matrice.  
  
2. Cliquez dans le graphique sparkline ou la barre de données, puis choisissez **Propriétés de l’axe horizontal** ou **Propriétés de l’axe vertical**.  
  
2.  Sous l’onglet **Options de l’axe** , cochez la case **Aligner les axes dans** puis, dans la zone déroulante, sélectionnez le groupe sur lequel aligner l’axe.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Ajouter des graphiques sparkline et des barres de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-sparklines-and-data-bars-report-builder-and-ssrs.md)  
  
  
