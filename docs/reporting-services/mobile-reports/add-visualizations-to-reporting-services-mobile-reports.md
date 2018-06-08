---
title: Ajouter des visualisations aux rapports mobiles Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: mobile-reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3b220b74-9ecd-4084-93fb-545208d5d7a2
caps.latest.revision: 15
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: be7412693c221c8436e0751414c5c615b6d23448
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-visualizations-to-reporting-services-mobile-reports"></a>Ajouter des visualisations aux rapports mobiles Reporting Services
Les graphiques sont une partie essentielle de la visualisation des données. Découvrez les graphiques que vous pouvez utiliser dans les rapports mobiles [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] pour couvrir un large éventail de scénarios. 

[!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-long.md)] propose trois types de graphiques de base : temps, catégorie et totaux. Ces trois types de graphiques ont des graphiques de comparaison correspondants, qui sont utiles pour comparer deux ensembles de séries distincts.  

## <a name="shared-chart-properties"></a>Propriétés partagées des graphiques

Certaines propriétés s’appliquent à tous les graphiques, tandis que d’autres ne concernent que des graphiques spécifiques. Voici quelques-unes des propriétés partagées.

### <a name="number-format"></a>Format des nombres
Vous pouvez affecter différents formats de nombres dans un graphique dans [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] , par exemple : général, devise avec ou sans décimales, pourcentages avec ou sans décimales, et ainsi de suite. Dans un graphique, la mise en forme des nombres s’applique aux annotations d’axe, ainsi qu’aux fenêtres contextuelles de points de données. Vous définissez la mise en forme des nombres individuellement dans chaque graphique, et non dans le rapport mobile global. 

* Pour définir le format des nombres, sélectionnez l’onglet **Disposition** , sélectionnez un graphique sur l’aire de conception puis, dans le panneau **Propriétés visuelles** , sélectionnez un **Format de nombre**. 
  
### <a name="legend"></a>Légende
* Pour afficher la légende d’un graphique, sélectionnez l’onglet **Disposition** , sélectionnez un graphique sur l’aire de conception puis, dans le panneau **Propriétés visuelles** , affectez la valeur **Activé** à **Afficher la légende**.
  
### <a name="series"></a>Série
Chaque métrique ou valeur individuelle affichée sur un graphique est appelée série. Plusieurs séries peuvent partager un axe X et un axe Y. Les séries sont définies dans le volet Propriétés des données de la vue Données en sélectionnant une ou plusieurs tables de données et champs. Chaque champ se traduit sur la visualisation du graphique par une série de points de données spécifique ayant sa propre couleur.  

### <a name="change-aggregation"></a>Modifier l’agrégation 
Pour les champs numériques dans les graphiques, l’agrégation par défaut est la somme. Vous pouvez la modifier et choisir Moyenne, Nombre, Minimum, Maximum, Premier ou Dernier.

* Sélectionnez l’onglet **Données** puis, dans **Propriétés des données**, sélectionnez **Options** en regard du champ numérique > sélectionnez une agrégation différente.

### <a name="set-or-clear-filters"></a>Définir ou effacer des filtres

Si vous ajoutez un navigateur pour filtrer votre rapport mobile, vous pouvez identifier les graphiques que vous souhaitez filtrer.

1. Sélectionnez l’onglet **Données** et, dans **Propriétés des données**, sélectionnez **Options**.

2. Sous **Filtré par**sont affichés les navigateurs que vous pouvez sélectionner ou effacer.

En savoir plus sur [l’ajout de navigateurs pour filtrer un rapport mobile](../../reporting-services/mobile-reports/add-navigators-to-reporting-services-mobile-reports.md).
   
## <a name="time-charts"></a>Graphiques de temps  
  
Le graphique de temps est le graphique le plus basique dans [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]. L’axe des heures (et dates) du graphique est défini automatiquement sur le premier champ de date/heure valide dans la table de données.  

![mobile-report-time-chart](../../reporting-services/mobile-reports/media/mobile-report-time-chart.png)

1. Faites glisser un **Graphique de temps** de l’onglet **Disposition** vers l’aire de conception et redimensionnez-le.

2. Par défaut, il s’agit d’un graphique à barres empilées. Vous pouvez changer le type dans **Visualisation des séries**.

3. Si le graphique a besoin de données qui ne sont pas encore dans le rapport, sélectionnez l’onglet **Données** > **Ajouter des données** pour [obtenir des données à partir d’Excel ou d’un jeu de données partagé](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md).

3. Dans le volet **Propriétés des données** , la **Série principale** est **SimulatedTable**. Cliquez sur la flèche dans la zone > sélectionnez votre table.

5. Si vous affectez la valeur **Par colonnes** à **Structure des données** (sous l’onglet **Disposition** > volet **Propriétés visuelles**), ici dans le volet **Propriétés des données** vous pouvez sélectionner plusieurs colonnes de valeurs numériques.

   Si vous affectez la valeur **Par lignes** à **Structure des données**, ici dans le volet **Propriétés des données** vous pouvez sélectionner un **Champ de nom de la série** et une colonne de valeurs numériques.
   
En savoir plus sur le [regroupement des données par colonnes ou par lignes](../../reporting-services/mobile-reports/group-data-by-columns-or-rows-in-a-mobile-report-reporting-services.md).
  
## <a name="category-charts"></a>Graphiques à catégories  
  
Contrairement aux graphiques de temps, dans un graphique à catégories vous regroupez sur un champ autre qu’un champ de date/heure sur l’axe X. Ce regroupement, appelé *coordonnée de catégorie*, doit être sur un champ de type chaîne, pas de type numérique.

![mobile-report-category-chart](../../reporting-services/mobile-reports/media/mobile-report-category-chart.png)   

1. Faites glisser un **Graphique à catégories** de l’onglet **Disposition** vers l’aire de conception, redimensionnez-le et [obtenez des données](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)si nécessaire.

2. Sélectionnez l’onglet **Données** et, dans le volet **Propriétés des données** , sous **Coordonnée de catégorie**, sélectionnez une table et un champ pour le regroupement. Ce champ figurera sur l’axe X du graphique résultant.

3. Sous **Série principale**, sélectionnez la table et les champs numériques à agréger pour chaque catégorie. 
  
## <a name="totals-charts"></a>Graphiques de totaux  

![mobile-report-totals-chart](../../reporting-services/mobile-reports/media/mobile-report-totals-chart.png)
  
Le graphique Totaux remplit deux fonctions distinctes : 
* Il ne présente pas plusieurs séries, mais uniquement la somme (ou total) de la série principale définie. 
* Il offre la possibilité de regrouper les données par colonnes ou par lignes. Le regroupement par colonnes peut être utile quand vous traitez des données aplaties. Lors du regroupement par colonnes, seule la propriété de série principale est disponible, car la colonne de catégorie est déterminée automatiquement d’après le nombre de champs sélectionnés pour la propriété de série principale.  

En savoir plus sur le [regroupement des données par colonnes ou par lignes](../../reporting-services/mobile-reports/group-data-by-columns-or-rows-in-a-mobile-report-reporting-services.md). 
  
## <a name="comparison-charts"></a>Graphiques de comparaison  
  
Les graphiques de temps, de catégorie et de totaux sont également disponibles en tant que *graphiques de comparaison*. Dans un graphique de comparaison, vous pouvez spécifier non seulement une série principale, mais également une deuxième série de comparaison. Vous pouvez afficher les séries principales et de comparaison de trois façons différentes.

![mobile-report-comparison-time-chart](../../reporting-services/mobile-reports/media/mobile-report-comparison-time-chart.png)

1. Faites glisser un **Graphique de comparaison** (temps, catégorie ou totaux) de l’onglet **Disposition** vers l’aire de conception, redimensionnez-le et [obtenez des données](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)si nécessaire.

2. Dans le volet **Propriétés visuelles** , dans **Visualisation des séries**, sélectionnez l’un des éléments suivants : 
   * Barres et barres fines
   * Courbes et barres
   * Barres et aires en escalier 

Dans les graphiques de comparaison, vous pouvez choisir d’avoir les mêmes couleurs de graphique sur les valeurs principales et les valeurs de comparaison dans une série.

* Dans le volet **Propriétés visuelles** , affectez la valeur **Activé** à l’option **Réutiliser les couleurs sur les séries de comparaison**.

   Si vous affectez la valeur **Activé**, la palette de couleurs est réinitialisée entre le dessin de la série principale et le dessin de la série de comparaison. Les valeurs associées dans les deux séries sont donc identiques. 

   Si vous affectez la valeur **Désactivé**, la palette de couleurs continue sa rotation normale pendant le dessin de la série principale après la série de comparaison, empêchant la coordination de couleurs potentiellement trompeuse entre les deux ensembles de séries.  
  
## <a name="pie-and-funnel-charts"></a>Graphiques en secteurs et graphiques en entonnoir  
  
Les graphiques en secteurs et les graphiques en entonnoir font partie des visualisations les plus simples. Vous pouvez structurer les données par lignes ou par colonnes. 
* Les **graphiques en secteurs** dans les rapports mobiles [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] peuvent être des secteurs, des anneaux ou des anneaux avec un total au centre. Les graphiques en secteurs conviennent pour l’affichage de la taille relative de différentes parties d’un entier. Trop de secteurs rend la lecture difficile.
* Les**graphiques en entonnoir** sont souvent utilisés pour afficher les étapes d’un processus, tel que les ventes.

![mobile-report-funnel-chart](../../reporting-services/mobile-reports/media/mobile-report-funnel-chart.png)

### <a name="structure-pie-and-funnel-chart-data-by-rows-or-by-columns"></a>Structurer les données des graphiques en secteurs et des graphiques en entonnoir par lignes ou par colonnes
1. Faites glisser un **Graphique en secteurs** ou un **Graphique en entonnoir** de l’onglet **Disposition** vers l’aire de conception, redimensionnez-le et [obtenez des données](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)si nécessaire.
2. Dans le volet **Propriétés visuelles** sous **Structure des données**, sélectionnez l’une des options suivantes :
   * **Structure des données**
   * **Structure des données**
3. Si vous avez sélectionné **Par colonnes**, sélectionnez l’onglet **Données** et, dans le volet **Propriétés des données** , sous **Série principale**, sélectionnez la table et tous les champs que vous souhaitez agréger dans le graphique à secteurs ou en entonnoir. Les noms de champ servent à étiqueter chaque zone du graphique obtenu.

   Si vous avez sélectionné **Par lignes**, sélectionnez l’onglet **Données** et, dans le volet **Propriétés des données** , sous **Colonne de catégorie**, sélectionnez la table et la colonne contenant les valeurs à utiliser pour le regroupement et les étiquettes dans le graphique à secteurs. Sous Colonne de la série principale, sélectionnez un champ numérique pour les valeurs du graphique.

En savoir plus sur le [regroupement des données par colonnes ou par lignes](../../reporting-services/mobile-reports/group-data-by-columns-or-rows-in-a-mobile-report-reporting-services.md). 

## <a name="treemaps"></a>Treemaps  
  
Les treemaps affichent des métriques en appliquant leurs valeurs à la taille et à la couleur des mosaïques dans une grille rectangulaire. 

![mobile-report-group-treemap](../../reporting-services/mobile-reports/media/mobile-report-group-treemap.png)

1. Faites glisser un **Tree Map** de l’onglet **Disposition** vers l’aire de conception, redimensionnez-le et [obtenez des données](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)si nécessaire.
2.  Sélectionnez l’onglet **Données** et, dans **Propriétés des données** : 

     * Sous **La taille représente** , sélectionnez un champ numérique pour la taille des mosaïques.
     * Sous **La couleur représente** , sélectionnez un champ numérique pour la couleur des mosaïques. 
     * [facultatif] **Valeur centrale personnalisée**: Vous ne pouvez utiliser **Valeur centrale personnalisée** que quand le type de visualisation est HeatMapWithCustomCenterValue.
     
         La valeur centrale détermine la couleur d’une zone. Plus la métrique se rapproche de la valeur centrale, plus la zone est verte. Plus elle s’en éloigne, plus la zone est rouge.
     
     * [Facultatif] Pour afficher une fenêtre contextuelle quand les utilisateurs sélectionnent une mosaïque dans la grille, sous **Étiquettes de fenêtre contextuelle** , sélectionnez un ou plusieurs champs. Les fenêtres contextuelles de treemap peuvent afficher des champs de texte et des champs numériques.  

Par défaut, les treemaps sont hiérarchiques et regroupent les mosaïques tout d’abord par catégorie, puis par taille et par couleur.
* Toujours sous l’onglet **Données** , sous **Regrouper par** , sélectionnez une table et un champ.

Vous pouvez désactiver le regroupement pour que les mosaïques soient disposées uniquement par taille et par couleur. 

* Sélectionnez l’onglet **Disposition** et affectez la valeur **Activé** à **Arborescence à deux niveaux**.   

## <a name="waterfall-charts"></a>Graphiques en cascade

Un graphique en cascade affiche le total cumulé alors que les valeurs sont ajoutées ou soustraites. Il est utile pour comprendre comment une valeur initiale (par exemple, le revenu net) est affectée par une série de modifications positives et négatives.

Pour vous permettre de vous repérer facilement, les colonnes en vert indiquent les augmentations et les colonnes en rouge indiquent les diminutions. Les colonnes de la valeur initiale et de la valeur finale commencent souvent à zéro, tandis que les valeurs intermédiaires sont représentées par des colonnes flottantes. En raison de cette « apparence », les graphiques en cascade sont également appelés graphiques en pont.

### <a name="when-to-use-a-waterfall-chart"></a>Quand utiliser un graphique en cascade ?

Les graphiques en cascade constituent un bon choix :
* Lorsque la mesure subit des modifications entre des séries chronologiques ou différentes catégories pour auditer les modifications majeures qui contribuent à la valeur totale.
* Pour tracer le bénéfice annuel de votre société en affichant différentes sources de revenus et obtenir le total des bénéfices (ou pertes).
* Pour illustrer les effectifs de début et de fin de votre société au cours d’une année.
* Pour visualiser l’argent que vous gagnez et que vous dépensez chaque mois, ainsi que le solde courant de votre compte. 

### <a name="create-a-waterfall-chart"></a>Créer un graphique en cascade

1. Faites glisser un **Graphique en cascade** de l’onglet **Disposition** vers l’aire de conception, redimensionnez-le et [obtenez des données](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)si nécessaire.

    ![mobile-report-waterfall-chart-icon](../../reporting-services/mobile-reports/media/mobile-report-waterfall-chart-icon.png)
    
2.  Sélectionnez l’onglet **Données** et dans le panneau **Propriétés des données** , sélectionnez un champ de catégorie dans vos données pour **Category Coordinate**(Coordonnées de catégorie) et un champ numérique pour **Série principale**: 

    ![mobile-report-waterfall-data](../../reporting-services/mobile-reports/media/mobile-report-waterfall-data.png)
    
3. Sélectionnez l’onglet **Disposition** pour afficher le graphique en cascade dans l’aperçu.

   ![mobile-report-waterfall-chart](../../reporting-services/mobile-reports/media/mobile-report-waterfall-chart.png)
   
   Les mois à perte, comme février, juin et juillet, sont indiqués en rouge. 
   Les mois avec gain, comme septembre, octobre et novembre, sont indiqués en vert. 

## <a name="see-also"></a>Voir aussi 
* [Cartes dans les rapports pour mobiles Reporting Services](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)
* [Navigateurs dans les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/add-navigators-to-reporting-services-mobile-reports.md)
* [Grilles de données dans les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/add-data-grids-to-mobile-reports-reporting-services.md)
* [Jauges dans les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)
  

