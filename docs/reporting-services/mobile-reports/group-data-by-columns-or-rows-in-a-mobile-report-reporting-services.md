---
title: Regrouper des données par colonnes ou par lignes dans un rapport mobile | Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: mobile-reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b9ebd36c-a337-47ae-83e5-6c2f2144eb52
caps.latest.revision: 6
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 17f93e01d074b243e7b40c708ac65dd1609d8841
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="group-data-by-columns-or-rows-in-a-mobile-report--reporting-services"></a>Regrouper des données par colonnes ou par lignes dans un rapport mobile | Reporting Services
L’ [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]vous permet d’organiser vos données par colonnes ou par lignes dans de nombreux types de graphiques. La procédure détaillée qui suit vous indique la marche à suivre.

Dans les graphiques de temps, de totaux, en secteurs et en entonnoir, vous pouvez organiser les données par lignes ou par colonnes. 
* L’organisation par colonnes est utile si une table comprend plusieurs colonnes de données à comparer. 
* L’organisation par lignes est mieux adaptée si une colonne de la table contient les noms des différentes catégories. 

Les étapes suivantes utilisent une table de totaux de comparaison avec les données simulées de l’ [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] pour illustrer la différence entre une organisation des données par lignes ou par colonnes dans un graphique.  

1. Faites glisser un **Graphique de totaux de comparaison** de l’onglet **Disposition** jusqu’à l’aire de conception et agrandissez-le.

2. Sélectionnez l’onglet **Données** . La table SimulatedTable contient une série de colonnes : des métriques (de **Metric1** à **Metric5**) et des **comparaisons** (de **Comparison1 à Comparison5**). 

   ![mobile-report-data-group-column](../../reporting-services/mobile-reports/media/mobile-report-data-group-column.png)

3. Dans le volet **Propriétés des données** , la **Série principale** est **SimulatedTable**. Si vous cliquez sur la flèche à côté de **Série principale**, vous pouvez voir que les métriques (de **Metric1** à **Metric5** ) sont sélectionnées.

   ![mobile-report-properties-columns](../../reporting-services/mobile-reports/media/mobile-report-properties-columns.png)

   De même, pour **Série de comparaison** -- , les comparaisons (de **Comparison1** à **Comparison5**) sont sélectionnées.
   
4. Sélectionnez **Aperçu**.

   ![mobile-report-chart-by-columns](../../reporting-services/mobile-reports/media/mobile-report-chart-by-columns.png)

   Chaque barre du graphique représente une colonne dans la table. Les barres épaisses correspondent aux colonnes de métriques, tandis que les barres plus fines correspondent aux colonnes de comparaison.

5. Sélectionnez la flèche Précédent dans le coin supérieur gauche pour quitter le mode Aperçu.

6. Sous l’onglet **Disposition** , dans le volet **Propriétés visuelles** , faites passer la **Structure des données** de **Par colonnes** à **Par lignes**.  

7. Sélectionnez l’onglet **Données** . La table SimulatedTable comprend désormais une colonne **Catégorie** (catégories A à E), suivie des colonnes **Métrique** et **Comparaison**. 

   ![mobile-report-data-group-rows](../../reporting-services/mobile-reports/media/mobile-report-data-group-rows.png)

8.  Le volet **Propriétés des données** propose à présent une zone Colonne Catégorie qui répertorie les éléments de la colonne Catégorie de SimulatedTable. Dans Série principale, vous pouvez choisir la colonne à utiliser pour les valeurs. Par défaut, l’ [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] sélectionne Metric1 à Metric5 comme série principale et Comparison1 à Comparison5 comme série de comparaison. 

    ![mobile-report-properties-rows](../../reporting-services/mobile-reports/media/mobile-report-properties-rows.png)

9. Sélectionnez **Aperçu**.

   ![mobile-report-chart-by-rows](../../reporting-services/mobile-reports/media/mobile-report-chart-by-rows.png)

   Chaque barre du graphique représente les valeurs de chaque catégorie dans la colonne Catégorie.

### <a name="see-also"></a>Voir aussi
* [Visualisations dans les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
