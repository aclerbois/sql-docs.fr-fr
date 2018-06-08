---
title: 'Didacticiel : ajouter un histogramme à un rapport (Générateur de rapports) | Microsoft Docs'
ms.custom: ''
ms.date: 09/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 63480059-b7b9-44b5-9d7f-91780db708b6
caps.latest.revision: 17
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c20091938d699da4dfc8e00b242509637e25e8bf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-add-a-column-chart-to-your-report-report-builder"></a>Didacticiel : ajouter un histogramme à un rapport (Générateur de rapports)
Dans ce didacticiel, vous allez créer un rapport paginé [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] avec un histogramme affichant une série sous la forme d’un ensemble de barres verticales regroupées par catégorie. 

Les histogrammes sont utiles pour :  
  
-   montrer l'évolution des données sur une période ;  
-   comparer la valeur relative de plusieurs séries.  
-   afficher une moyenne mobile pour montrer les tendances.  
  
L'illustration suivante montre l'histogramme que vous allez créer, avec une moyenne mobile.  
  
![didacticiel-histogramme-générateur-de-rapports](../reporting-services/media/report-builder-column-chart-tutorial.png)    
> [!NOTE]  
> Dans ce didacticiel, les étapes de l'Assistant sont consolidées en une seule procédure. Pour obtenir des instructions pas à pas sur l’accès à un serveur de rapports, le choix d’une source de données et la création d’un dataset, consultez le premier didacticiel de cette série : [Didacticiel : création d’un rapport de tableau de base &#40;Générateur de rapports&#41;](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md).  
  
Durée estimée pour effectuer ce didacticiel : 15 minutes.  
  
## <a name="requirements"></a>Spécifications  
Pour plus d’informations sur les spécifications, consultez [Éléments requis pour les didacticiels &#40;Générateur de rapports&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="Chart"></a>1. Créer un rapport de graphique à partir de l'Assistant Graphique  
Dans cette section, vous utilisez l’Assistant Graphique pour créer un dataset incorporé, choisir une source de données partagée et créer un histogramme.  
  
> [!NOTE]  
> La requête dans ce didacticiel contenant les valeurs de données, aucune source de données externe n’est nécessaire. Cela rend la requête assez longue. Dans un environnement métier, une requête ne contient pas les données. Ceci est nécessaire à des fins de formation uniquement.  
  
### <a name="to-create-a-chart-report"></a>Pour créer un rapport de graphique  
  
1.  [Démarrez le Générateur de rapports](../reporting-services/report-builder/start-report-builder.md) à partir de votre ordinateur, du portail web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] ou du mode intégré SharePoint.  
  
    La boîte de dialogue **Nouveau rapport ou dataset** s’ouvre.  
  
    Si vous ne voyez pas la boîte de dialogue **Nouveau rapport ou dataset**, dans le menu **Fichier**, choisissez **Nouveau**.  
  
2.  Dans le volet gauche, assurez-vous que **Nouveau rapport** est sélectionné.  
  
3.  Dans le volet droit, cliquez sur **Assistant Graphique**.  
  
4.  Dans la page **Choisir un dataset**, cliquez sur **Créer un dataset**, puis sur **Suivant**.  
  
5.  Dans la page **Choisir une connexion à une source de données** , sélectionnez une source de données existante ou naviguez jusqu’au serveur de rapports, sélectionnez une source de données, puis cliquez sur **Suivant**. Vous devrez peut-être entrer un nom d'utilisateur et un mot de passe.  
  
    > [!NOTE]  
    > La source de données que vous choisissez n'a pas d'importance, tant que vous disposez des autorisations appropriées. Vous n'allez pas récupérer de données à partir de la source de données. Pour plus d’informations, consultez [Autres manières d’obtenir une connexion de données &#40;Générateur de rapports&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
6.  Dans la page **Créer une requête** , cliquez sur **Modifier en tant que texte**.  
  
7.  Collez la requête suivante dans le volet de requête :  
  
    ```  
    SELECT CAST('2015-01-01' AS date) AS SalesDate, CAST(54995.21 AS money) AS Sales  
    UNION SELECT CAST('2015-01-05' AS date) AS SalesDate, CAST(64499.04 AS money) AS Sales  
    UNION SELECT CAST('2015-02-11' AS date) AS SalesDate, CAST(37821.79 AS money) AS Sales  
    UNION SELECT CAST('2015-03-18' AS date) AS SalesDate, CAST(53633.08 AS money) AS Sales  
    UNION SELECT CAST('2015-04-23' AS date) AS SalesDate, CAST(24019.3 AS money) AS Sales  
    UNION SELECT CAST('2015-05-01' AS date) AS SalesDate, CAST(93245.5 AS money) AS Sales  
    UNION SELECT CAST('2015-06-06' AS date) AS SalesDate, CAST(55288.0 AS money) AS Sales  
    UNION SELECT CAST('2015-06-16' AS date) AS SalesDate, CAST(68733.5 AS money) AS Sales  
    UNION SELECT CAST('2015-07-16' AS date) AS SalesDate, CAST(24750.85 AS money) AS Sales  
    UNION SELECT CAST('2015-08-23' AS date) AS SalesDate, CAST(43452.3 AS money) AS Sales  
    UNION SELECT CAST('2015-09-24' AS date) AS SalesDate, CAST(58656. AS money) AS Sales  
    UNION SELECT CAST('2015-10-15' AS date) AS SalesDate, CAST(44583. AS money) AS Sales  
    UNION SELECT CAST('2015-11-21' AS date) AS SalesDate, CAST(81568. AS money) AS Sales  
    UNION SELECT CAST('2015-12-15' AS date) AS SalesDate, CAST(45973. AS money) AS Sales  
    UNION SELECT CAST('2015-12-26' AS date) AS SalesDate, CAST(96357. AS money) AS Sales  
    UNION SELECT CAST('2015-12-31' AS date) AS SalesDate, CAST(81946. AS money) AS Sales  
    ```  
  
8.  (Facultatif) Cliquez sur le bouton Exécuter (**!**) pour voir les données sur lesquelles votre graphique sera basé.  
  
9. Cliquez sur **Suivant**.  
  
## <a name="ChartType"></a>2. Choisir le type de graphique  
Vous pouvez choisir parmi plusieurs types de graphiques prédéfinis, puis modifier le graphique après avoir terminé l’Assistant.  
  
### <a name="to-add-a-column-chart"></a>Pour ajouter un histogramme  
  
1.  Dans la page **Choisir un type de graphique** , l’histogramme est le type de graphique par défaut. Cliquez sur **Suivant**.  
  
2.  Dans la page **Organiser les champs du graphique** , faites glisser le champ SalesDate vers **Catégories**. Les catégories s'affichent sur l'axe horizontal.  
  
3.  Faites glisser le champ Sales vers **Valeurs**. La zone **Valeurs** affiche Sum(Sales), car la somme de la valeur totale des ventes est agrégée pour chaque date. Les valeurs s'affichent sur l'axe vertical.  
  
4.  Cliquez sur **Suivant**.  
 
6.  Cliquez sur **Terminer**.  
  
    Le graphique est ajouté à l'aire de conception. Notez que le nouvel histogramme ne fait que représenter les données. La légende indique Sales Date A, Sales Date B, et ainsi de suite, simplement pour donner une idée de l’apparence du rapport. 
    
    ![report-builder-column-chart-1-design-view](../reporting-services/media/report-builder-column-chart-1-design-view.png)
  
7.  Cliquez sur le graphique pour afficher ses poignées. Faites glisser l'angle inférieur droit du graphique pour augmenter sa taille. Notez que la taille de l'aire de conception du rapport augmente pour s'adapter à la taille du graphique.  
  
8.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  

    ![report-builder-column-chart-1-preview](../reporting-services/media/report-builder-column-chart-1-preview.png)

Notez que le graphique n’étiquette pas chaque catégorie sur l’axe horizontal. Par défaut, seules sont incluses les étiquettes qui n'empiètent pas sur l'axe. 
  
## <a name="Horizontal"></a>3. Mettre en forme une date sur l’axe horizontal  
Par défaut, l'axe horizontal affiche les valeurs dans un format général qui est mis à l'échelle automatiquement pour s'ajuster à la taille du graphique.  
  
1.  Basculez en mode création de rapport.  
  
2.  Cliquez avec le bouton droit sur l’axe horizontal > **Propriétés de l’axe horizontal**.  
  
3.  Sous l’onglet **Nombre** , dans **Catégorie**, sélectionnez **Date**.  
  
5.  Dans la zone **Type** , sélectionnez **31 Jan 2000**.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  Sous l’onglet Accueil, cliquez sur **Exécuter** pour afficher l’aperçu du rapport.  
  
La date s'affiche dans le format de date que vous avez sélectionné. Le graphique n’étiquette toujours pas chaque catégorie sur l’axe horizontal. 

![report-builder-column-chart-2-preview](../reporting-services/media/report-builder-column-chart-2-preview.png)
  
Vous pouvez personnaliser l'affichage des étiquettes en les faisant pivoter et en spécifiant l'intervalle.  
  
## <a name="4-rotate-the-axis-labels-on-the-horizontal-axis"></a>4. Faire pivoter les étiquettes d’axes sur l’axe horizontal  
  
1.  Basculez en mode création de rapport.  
  
2.  Cliquez avec le bouton droit sur le titre de l’axe horizontal, puis cliquez sur **Afficher le titre de l’axe** pour supprimer le titre. Étant donné que l'axe horizontal indique les dates, le titre n'est pas nécessaire.  
  
3.  Cliquez avec le bouton droit sur l’axe horizontal > **Propriétés de l’axe horizontal**.  
  
5.  Sous l’onglet **Étiquettes** , sous **Modifier les options d’ajustement automatique des étiquettes d’axe**, sélectionnez **Désactiver l’ajustement automatique**.  
  
7.  Dans **Angle de rotation des étiquettes**, sélectionnez **-90**.  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    L'exemple de texte de l'axe horizontal pivote de 90 degrés.  
    
    ![report-builder-column-chart-rotate-x-axis](../reporting-services/media/report-builder-column-chart-rotate-x-axis.png)
  
9. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Sur le graphique, les étiquettes pivotent.  

![report-builder-column-chart-rotate-x-axis-preview](../reporting-services/media/report-builder-column-chart-rotate-x-axis-preview.png)
  
## <a name="Legend"></a>5. Déplacer la légende  
La légende est créée automatiquement à partir des données de catégories et de séries. Vous pouvez déplacer la légende sous la zone de graphique d’un histogramme.  
  
1.  Basculez en mode création de rapport.  
  
2.  Cliquez avec le bouton droit sur la légende du graphique > **Propriétés de la légende**.  
  
3.  Sous **Mise en page et position**, sélectionnez une autre position. Par exemple, sélectionnez l’option en bas au milieu.  
  
    Lorsque la légende est placée en haut ou en bas d'un graphique, la disposition de la légende change de vertical à horizontal. Vous pouvez sélectionner une disposition différente dans la zone **Disposition** .  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  (Facultatif) Ce didacticiel ne comprenant qu’une seule catégorie, le graphique n’a pas besoin de légende. Pour la supprimer, cliquez sur la légende > **Supprimer la légende**.  
  
6.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="ChartTitle"></a>6. Intituler le graphique  
    
1.  Basculez en mode création de rapport.  
  
2.  Sélectionnez les mots **Titre du graphique** en haut du graphique, puis tapez **Total des commandes client par magasin**.  
  
3.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="Vertical"></a>7. Mettre en forme et étiqueter l'axe vertical  
Par défaut, l'axe vertical affiche les valeurs dans un format général qui est mis à l'échelle automatiquement pour s'ajuster à la taille du graphique.   
  
1.  Basculez en mode création de rapport.  
  
2. Cliquez sur les étiquettes figurant sur l’axe vertical, sur le côté gauche du graphique, pour les sélectionner.  
  
3.  Sous l’onglet **Accueil** > groupe **Nombre**, cliquez sur le bouton **Devise**. Les étiquettes de l'axe changent pour afficher le format monétaire.  
  
4.  Cliquez sur le bouton **Réduire les décimales** à deux reprises, pour afficher le nombre arrondi au dollar le plus proche.  
  
5.  Cliquez avec le bouton droit sur l’axe vertical > **Propriétés de l’axe vertical**.  
  
6.  Sous l’onglet **Nombre** , notez que **Devise** est déjà sélectionné dans la zone **Catégorie** et que **Nombre de décimales** est déjà **0** (zéro).  
  
7.  Sélectionnez l’option **Afficher les valeurs en**. **Milliers** est déjà sélectionné.  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
9. Cliquez avec le bouton droit sur l’axe vertical > **Afficher le titre de l’axe**. 

10. Cliquez avec le bouton droit sur le titre de l’axe vertical > **Propriétés du titre de l’axe**.  
  
10. Remplacez le texte du champ **Texte du titre** par **Total des ventes (en milliers)**. Vous pouvez également spécifier diverses options de mise en forme du titre.  
  
11. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
12. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  

    ![report-builder-column-chart-format-y-axis](../reporting-services/media/report-builder-column-chart-format-y-axis.png)
    
## <a name="8-show-all-the-labels-on-the-horizontal-x-axis"></a>8. Afficher toutes les étiquettes sur l’axe horizontal (x)

Vous remarquerez que seuls certaines des étiquettes sur l’axe x sont affichées. Dans cette section, vous allez définir une propriété dans le volet Propriétés pour les afficher toutes.

1.  Basculez en mode création de rapport.  
  
2.  Cliquez sur le graphique, puis sélectionnez les étiquettes de l’axe horizontal.

3. Dans le volet Propriétés, affectez la valeur 1 à LabelInterval.

    ![report-builder-column-chart-set-label-interval](../reporting-services/media/report-builder-column-chart-set-label-interval.png)

    Le graphique a la même apparence en mode Conception. 
    
5.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.

    ![report-builder-column-chart-label-interval-one-preview](../reporting-services/media/report-builder-column-chart-label-interval-one-preview.png)
    
    Le graphique affiche maintenant toutes ses étiquettes.
  
## <a name="Average"></a>9. Ajouter une moyenne mobile avec une série calculée  

Une moyenne mobile est une moyenne des données de votre série, calculée au fil du temps. La moyenne mobile peut identifier des tendances.
  
1.  Basculez en mode création de rapport.  
  
2.  Double-cliquez sur le graphique pour afficher le volet **Données du graphique** .  
  
3.  Cliquez avec le bouton droit sur le champ **[Sum(Sales)]** dans la zone **Valeurs** , puis cliquez sur **Ajouter une série calculée**.  

     ![report-builder-column-chart-add-calculated-series](../reporting-services/media/report-builder-column-chart-add-calculated-series.png)
  
4.  Dans **Formule**, vérifiez que **Moyenne mobile** est sélectionné.  
  
5.  Dans **Définir les paramètres de la formule**, pour **Période**, sélectionnez **4**.  
  
6.  Sous l’onglet **Bordure** , dans **Largeur de ligne**, sélectionnez **3pt**.  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
9. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Le graphique présente une ligne qui indique la moyenne mobile pour le total des ventes par date. La moyenne est ici calculée toutes les quatre dates. En savoir plus sur [l’ajout d’une moyenne mobile à un graphique](../reporting-services/report-design/add-a-moving-average-to-a-chart-report-builder-and-ssrs.md). 

![report-builder-column-chart-moving-average](../reporting-services/media/report-builder-column-chart-moving-average.png)
  
## <a name="Title"></a>10. Ajouter un titre de rapport  
  
1.  Basculez en mode création de rapport.  
  
2.  Dans l'aire de conception, cliquez sur **Cliquez pour ajouter un titre**.  
  
3.  Tapez **Graphique sur les ventes**, appuyez sur Entrée, puis tapez **Janvier à décembre 2015**pour obtenir ce qui suit :  
  
    **Graphique sur les ventes**  
  
    **Janvier à décembre 2015**  
  
4.  Sélectionnez **Graphique sur les ventes** et, sous l’onglet **Accueil** > section **Police** > **Gras**.  
  
5.  Sélectionnez **Janvier à décembre 2015** et, sous l’onglet **Accueil** > section **Police** > affectez la valeur **10** à la taille de police.  
  
6.  (Facultatif) Vous devrez peut-être agrandir la zone de texte **Titre** pour contenir les deux lignes de texte. Tirez sur les flèches gauche-droite quand vous cliquez au milieu du bord inférieur. Et vous devrez peut-être faire glisser le haut du graphique pour que le titre soit visible.  
  
    Ce titre s’affiche dans la partie supérieure du rapport. Quand aucun en-tête de page n’est défini, les éléments situés en haut du corps du rapport font office d’en-tête de rapport.  
  
7.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="Save"></a>11. Enregistrer le rapport  
  
### <a name="to-save-the-report"></a>Pour enregistrer le rapport  
  
1.  Basculez en mode création de rapport.  
  
2.  À partir du bouton Générateur de rapports, cliquez sur **Enregistrer sous**.  

    Vous pouvez l’enregistrer sur votre ordinateur ou sur le serveur de rapports.
  
3.  Dans **Nom**, tapez **Histogramme des commandes client**.  
  
4.  Cliquez sur **Enregistrer**.  
  
## <a name="next-steps"></a>Next Steps  
Vous avez terminé le didacticiel Ajout d'un histogramme à un rapport. Pour en savoir plus sur les graphiques, consultez [Graphiques &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/charts-report-builder-and-ssrs.md) et [Graphiques sparkline et barres de données &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
-    [Didacticiels du Générateur de rapports](../reporting-services/report-builder-tutorials.md) 
-    [Générateur de rapports dans SQL Server 2016](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

