---
title: Ajouter des navigateurs aux rapports mobiles Reporting Services | Microsoft Docs
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
ms.assetid: e141f50e-49a9-46c6-983c-f656013aa07c
caps.latest.revision: 12
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: a4a4b4c717ae7a8d7fffa772738c6bdb4aeb2fee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-navigators-to-reporting-services-mobile-reports"></a>Add navigators to Reporting Services mobile reports
Dans [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)], vous ajoutez des *navigateurs* pour filtrer les données dans les visualisations en fonction d’une période ou d’une sélection. 

Les navigateurs sont semblables aux segments dans Power BI et les tableaux croisés dynamiques Excel, mais présentent également certaines caractéristiques.

Les**navigateurs de temps** filtrent les tables en sélectionnant les lignes qui appartiennent à une période spécifique. 

Les**navigateurs par sélection** filtrent les tables en sélectionnant les lignes pour lesquelles une certaine valeur de colonne correspond à la valeur de clé sélectionnée ; ou, dans le cas des arborescences hiérarchiques, pour lesquelles une certaine valeur de colonne appartient à la sous-arborescence de la valeur de clé sélectionnée. Il existe deux types de navigateurs par sélection :
* Les listes de sélection sont des tables d’une seule colonne que vous pouvez utiliser pour filtrer votre rapport mobile, à l’image des segments dans Power BI et Excel.
* Les grilles de tableau de bord filtrent également le rapport mobile et peuvent également contenir 
  
## <a name="time-navigators"></a>Navigateurs de temps   
  
Comme son nom l’indique, vous utilisez le navigateur de temps pour filtrer une plage de données délimitée par une plage de temps.   
  
![SSMRP_TimeNav](../../reporting-services/mobile-reports/media/ssmrp-timenav.png)  
*Les quatre graphiques en courbes sur la gauche sont définis dans les présélections d’intervalles de temps. Le graphique en courbes à droite est le filtre.*  
  
Quand vous affichez le rapport en mode aperçu ou dans le portail web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , vous déplacez les flèches dans le navigateur de temps pour filtrer le reste du rapport.  
  
![SSMRP_TimeNavPreview](../../reporting-services/mobile-reports/media/ssmrp-timenavpreview.png)  
  
Par défaut, le navigateur de temps filtre tous les éléments visuels dans le rapport qui sont connectés à des données de temps. Si une table contient plusieurs colonnes temporelles, seule la première est utilisée pour le filtrage. La table de la série des lecteurs lance la visualisation incorporée et détermine la plage de dates générale du rapport mobile.  
  
Vous pouvez déconnecter une visualisation du navigateur de temps.   
1. Sélectionnez la visualisation, puis sélectionnez l’onglet **Données** .  
2. Dans **Propriétés des données**, sélectionnez **Options**.  
3. Décochez la case **filtré par** .  
  
   ![SSMRP_ClearTimeFilter](../../reporting-services/mobile-reports/media/ssmrp-cleartimefilter.png)  
  
## <a name="selection-lists"></a>Listes de sélection   
  
La liste de sélection filtre les données d’un rapport mobile en faisant correspondre la valeur que vous sélectionnez dans la liste à la valeur d’une colonne spécifiée pour chaque ligne d’une table filtrée. 

1. À partir de l’onglet **Disposition** , faites glisser **Liste de sélection** vers l’aire de conception et redimensionnez-la comme vous le souhaitez.

2. Sélectionnez l’onglet **Données** , puis dans le volet **Propriétés des données** sous **Clés**, sélectionnez la table et la colonne à utiliser comme filtre. 

3. Sous **Étiquettes**, sélectionnez la colonne portant l’étiquette à afficher. La colonne clé et la colonne d'étiquette peuvent être les mêmes.  
  
   Dans le cas de données d’arborescence hiérarchique, sélectionnez une colonne clé parente.  
  
4. Après avoir défini les propriétés de données, sous **Tables filtrées par liste de sélection**, sélectionnez les tables à filtrer et la colonne à utiliser comme filtre. Cette colonne doit établir une correspondance avec les valeurs dans la colonne clé de la liste de sélection. 

Pour chaque visualisation du rapport mobile que la liste de sélection doit filtrer :

1. Sélectionnez la visualisation, sélectionnez l’onglet **Données** et dans le volet **Propriétés des données** , sélectionnez **Options** en regard du nom de champ.

   ![mobile-report-set-selection-list](../../reporting-services/mobile-reports/media/mobile-report-set-selection-list.png)

2. Sous **filtré par**, sélectionnez la liste de sélection.

Quand vous affichez le rapport mobile en mode aperçu ou dans le portail web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] et que vous sélectionnez une valeur dans la liste de sélection, les autres visualisations du rapport mobile sont filtrées.

![mobile-report-selection-list-filtering](../../reporting-services/mobile-reports/media/mobile-report-selection-list-filtering.png) 
     
## <a name="scorecard-grid"></a>Grille de tableau de bord  
  
Le filtre de grille de tableau de bord fonctionne de façon semblable au filtre de liste de sélection, mais il affiche également des colonnes de valeur et des indicateurs de score, identiques aux indicateurs dans une grille de données d’indicateur. Après avoir sélectionné la clé, l’étiquette et les éventuelles colonnes clés parentes, vous sélectionnez une colonne et une table d’entrée pour fournir des données au tableau de bord. La colonne de données du tableau de bord doit pouvoir être filtrée par la colonne clé.  

1. À partir de l’onglet **Disposition** , faites glisser **Grille de tableau de bord** vers l’aire de conception et redimensionnez-la comme vous le souhaitez.

2. Sélectionnez l’onglet **Données** , puis dans le volet **Propriétés des données** sous **Clés**, sélectionnez la table et la colonne à utiliser comme filtre. 

3. Sous **Étiquettes**, sélectionnez la colonne portant l’étiquette à afficher. La colonne clé et la colonne d'étiquette peuvent être les mêmes.  
  
4. Pour ajouter un indicateur de score, dans le volet **Colonnes de données** , sélectionnez **Ajouter un score**.   
  
5. Attribuez un nom à l’indicateur de score, puis sélectionnez **Options** pour définir les mêmes propriétés que pour un indicateur dans une grille de données :  
  
   * Type de jauge
   * Champ de valeur
   * Champ de comparaison
   * Sens des valeurs
  
6. Pour ajouter un indicateur de valeur, dans le volet **Colonnes de données** , sélectionnez **Ajouter une valeur**.

7. Nommez l'indicateur de valeur comme vous le souhaitez, choisissez sa colonne source dans la table et sélectionnez sa mise en forme.  

   ![mobile-report-scorecard-grid-data-properties](../../reporting-services/mobile-reports/media/mobile-report-scorecard-grid-data-properties.png)

8. Après avoir défini les propriétés de données, sous **Tables filtrées par liste de sélection**, sélectionnez les tables à filtrer et la colonne à utiliser comme filtre. Cette colonne doit établir une correspondance avec les valeurs dans la colonne clé de la liste de sélection. 

Quand vous affichez le rapport mobile en mode aperçu ou dans le portail web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] et que vous sélectionnez une valeur dans la grille de tableau de bord, les autres visualisations du rapport mobile sont filtrées.

![mobile-report-scorecard-grid](../../reporting-services/mobile-reports/media/mobile-report-scorecard-grid.png)
    
## <a name="set-which-visualizations-are-filtered"></a>Définir les visualisations à filtrer  
  
Les éléments de la galerie sont configurés pour utiliser des filtres en cliquant sur le bouton Options pour une entrée en particulier dans l’affichage des données. Les filtres sont activés en sélectionnant la case appropriée.  

Vous pouvez choisir les visualisations qu’un navigateur doit filtrer dans le rapport mobile :

1. Sélectionnez la visualisation, sélectionnez l’onglet **Données** et dans le volet **Propriétés des données** , sélectionnez **Options** en regard du nom de champ.

   ![mobile-report-set-selection-list](../../reporting-services/mobile-reports/media/mobile-report-set-selection-list.png)

2. Sous **filtré par**, sélectionnez le navigateur. Chaque visualisation peut être filtrée par plusieurs navigateurs.
  
## <a name="cascading-filters"></a>Filtres en cascade   
  
Les filtres peuvent également être mis en cascade les uns avec les autres de façon que la valeur sélectionnée de l'un filtre les valeurs disponibles du second. Pour mettre des filtres en cascade, appliquez les filtres à la colonne clé comme vous le feriez pour un élément de galerie standard.  

### <a name="see-also"></a>Voir aussi 
  
* [Cartes dans les rapports pour mobiles Reporting Services](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)
* [Visualisations dans les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
* [Jauges dans les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)
* [Grilles de données dans les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/add-data-grids-to-mobile-reports-reporting-services.md)  
