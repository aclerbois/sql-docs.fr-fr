---
title: 'Didacticiel : ajout d’un indicateur de performance clé à un rapport (Générateur de rapports) | Microsoft Docs'
ms.custom: ''
ms.date: 06/15/2016
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
ms.assetid: 1bf77859-0b33-4f40-abaf-ebeeb6ebb1f8
caps.latest.revision: 13
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 9c6c7edf99179ba576fbb2a30668768eeca222e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-adding-a-kpi-to-your-report-report-builder"></a>Didacticiel : ajout d'un indicateur de performance clé à un rapport (Générateur de rapports)
Dans ce didacticiel [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion-md.md)] , vous ajoutez un indicateur de performance clé (KPI) à un rapport paginé [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] .  

Les indicateurs de performance clés sont des valeurs mesurables qui revêtent une importance significative pour l’entreprise. Dans ce scénario, le récapitulatif des ventes par sous-catégories de produits est l'indicateur de performance clé. L’état actuel de l’indicateur de performance clé est indiqué avec des couleurs, des jauges et des indicateurs.
  
L’illustration suivante est similaire au rapport que vous allez créer.  
  
![report-builder-kpi-report](../reporting-services/media/report-builder-kpi-report.png)
    
> [!NOTE]  
> Dans ce didacticiel, les étapes de l'Assistant sont consolidées sous forme de deux procédures : l'une pour créer le dataset, et l'autre pour créer une table. Pour obtenir des instructions pas à pas sur l’accès à un serveur de rapports, le choix d’une source de données, la création d’un dataset et l’exécution de l’Assistant, consultez le premier didacticiel de cette série : [Didacticiel : création d’un rapport de tableau de base &#40;Générateur de rapports&#41;](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md).  
  
Durée estimée pour effectuer ce didacticiel : 15 minutes.  
  
## <a name="requirements"></a>Spécifications  
Pour plus d’informations sur les spécifications, consultez [Éléments requis pour les didacticiels &#40;Générateur de rapports&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="Table"></a>1. Créer un rapport de tableau et un dataset à partir de l'Assistant Tableau ou matrice  
Dans cette section, vous choisissez une source de données partagée, créez un dataset incorporé et affichez les données dans un tableau.  
 
### <a name="to-create-a-table-with-an-embedded-dataset"></a>Pour créer un tableau avec un dataset incorporé  
  
1.  [Démarrez le Générateur de rapports](../reporting-services/report-builder/start-report-builder.md) à partir de votre ordinateur, du portail web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] ou du mode intégré SharePoint.  
  
    La boîte de dialogue **Nouveau rapport ou dataset** s’ouvre.  
  
    Si vous ne voyez pas la boîte de dialogue **Nouveau rapport ou dataset**, dans le menu **Fichier**, choisissez **Nouveau**.  
  
2.  Dans le volet gauche, assurez-vous que **Nouveau rapport** est sélectionné.  
  
3.  Dans le volet droit, cliquez sur **Assistant Tableau ou matrice**.  
  
4.  Dans la page **Choisir un dataset** , cliquez sur **Créer un dataset**.  
  
5.  Cliquez sur **Suivant**.  
  
6.  Dans la page **Choisir une connexion à une source de données** , sélectionnez une source de données existante ou naviguez jusqu’au serveur de rapports, puis sélectionnez une source de données. Si aucune source de données n’est disponible ou que vous n’avez pas accès à un serveur de rapports, vous pouvez utiliser une source de données incorporée à la place. Pour plus d’informations, consultez [Didacticiel : création d’un rapport de tableau de base &#40;Générateur de rapports&#41;](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md).  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page **Créer une requête** , cliquez sur **Modifier en tant que texte**.  
  
9. Copiez et collez la requête suivante dans le volet de requête :  

    > [!NOTE]  
    > Dans ce didacticiel, la requête contient les valeurs de données, afin qu'il ne soit pas nécessaire de disposer d'une source de données externe. Cela rend la requête assez longue. Dans un environnement métier, une requête ne contient pas les données. Ceci est nécessaire à des fins de formation uniquement.  
  
    ```  
    SELECT CAST('2015-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Carrying Case' as Product, CAST(16996.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-11' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Mini Battery Charger' as Product, CAST(1056.00 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Accessories' as Subcategory,  
       'Telephoto Conversion Lens' as Product, CAST(1380.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,'Accessories' as Subcategory,    
       'USB Cable' as Product, CAST(780.00 AS money) AS Sales, 26 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(3798.00 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2015-01-09' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Business Videographer' as Product, CAST(10400.00 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2015-01-10' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Social Videographer' as Product, CAST(3000.00 AS money) AS Sales, 60 as Quantity  
    UNION SELECT CAST('2015-01-11' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Advanced Digital' as Product, CAST(7234.50 AS money) AS Sales, 39 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Compact Digital' as Product, CAST(10836.00 AS money) AS Sales, 84 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Consumer Digital' as Product, CAST(2550.00 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Digital' as Subcategory,   
       'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-09' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera 35mm' as Product, CAST(18530.00 AS money) AS Sales, 34 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera' as Product, CAST(26576.00 AS money) AS Sales, 88 as Quantity  
    ```  
  
10. Dans la barre d’outils du Concepteur de requêtes, cliquez sur Exécuter (**!**).

11. Cliquez sur **Suivant**.  
  
## <a name="CompleteWizard"></a>2. Organiser les données et choisir la disposition dans l’Assistant  
L’Assistant Tableau ou matrice propose une conception initiale pour l’affichage les données. Le volet de visualisation de l'Assistant vous aide à visualiser le résultat du regroupement des données avant de terminer la conception de la table ou de la matrice.  
  
### <a name="to-organize-data-into-groups-and-choose-a-layout"></a>Pour organiser les données en groupes et choisir une disposition 
  
1.  Dans la page Organiser les champs, faites glisser Product vers **Valeurs**.  
  
2.  Faites glisser Quantity vers **Valeurs** et placez-le sous Product.  
  
    Quantity est synthétisé à l'aide de la fonction Sum, la fonction de synthèse par défaut des champs numériques.  
  
3.  Faites glisser Sales vers **Valeurs** et placez-le sous Quantity.  
  
    Les étapes 1, 2 et 3 spécifient les données à afficher dans le tableau.  
  
4.  Faites glisser SalesDate vers **Groupes de lignes**.  
  
5.  Faites glisser Subcategory vers **Groupes de lignes** et placez-le sous SalesDate.  
  
    Les étapes 4 et 5 organisent les valeurs des champs par date, puis par l'ensemble des ventes pour chaque date.  
  
6.  Cliquez sur **Suivant**.  
  
    Lorsque vous exécutez le rapport, le tableau affiche chaque date, toutes les commandes pour chaque date et tous les produits, quantités et totaux de ventes pour chaque commande.  
  
7.  Dans la page Choisir la disposition, sous **Options**, vérifiez que **Afficher les sous-totaux et les totaux généraux** est sélectionné.  
  
8.  Vérifiez que **Bloqué, sous-total ci-dessous** est sélectionné.  
  
9. Désactivez l’option **Développer/Réduire les groupes**.  
  
    Dans ce didacticiel, le rapport que vous créez n'utilise pas la fonctionnalité d'exploration vers le bas qui permet à un utilisateur de développer une hiérarchie de groupe parente afin d'afficher les lignes de groupes enfants et les lignes de détails.  
  
10. Cliquez sur **Suivant**.  
  
11. Cliquez sur **Terminer**.  
  
      Le tableau est ajouté à l'aire de conception. Le tableau possède cinq colonnes et cinq lignes. Le volet Groupes de lignes affiche trois lignes : SalesDate, Subcategory et Details. Les données de détail sont toutes les données récupérées par la requête de dataset. Le volet Groupes de colonnes est vide.  
      
      ![report-builder-kpi-row-groups](../reporting-services/media/report-builder-kpi-row-groups.png)
  
12. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Pour chaque produit vendu à une date spécifique, le tableau affiche le nom du produit, la quantité vendue et le total des ventes. Les données sont organisées par date de vente, puis par sous-catégorie. 

![report-builder-kpi-basic-table](../reporting-services/media/report-builder-kpi-basic-table.png)
    
### <a name="format-dates-and-currency"></a>Mettre en forme les dates et des devises
Élargissons les colonnes et définissons le format des dates et des devises.

1. Cliquez sur **Conception** pour rebasculer en mode Conception.

2. Les noms de produits pourraient occuper davantage d’espace. Pour élargir la colonne Product, sélectionnez tout le tableau et faites glisser le bord droit de la poignée de colonne en haut de la colonne Product.

3. Appuyez sur la touche Ctrl, puis sélectionnez les quatre cellules contenant [Sum(Sales)].

4. Sous l’onglet **Accueil** > **Nombre** > **Devise**. Les cellules changent pour afficher le format de devise.

   Si votre paramètre régional est Anglais (États-Unis), le texte d’exemple par défaut est [$12,345.00]. Si vous ne voyez pas s’afficher d’exemple de valeur monétaire, dans le groupe **Nombres** , cliquez sur **Styles des espaces réservés** > **Valeurs d’aperçu**.
    
    ![report-builder-placeholder-value-button](../reporting-services/media/report-builder-placeholder-value-button.png)

5. (Facultatif) Sous l’onglet **Accueil** , dans le groupe **Nombre** , cliquez sur le bouton **Réduire les décimales** à deux reprises, pour afficher les valeurs en dollars sans indication de centimes.

6. Cliquez sur la cellule qui contient [SalesDate].

6. Dans le groupe **Nombre** > **Date**.

   La cellule affiche la date d'exemple [1/31/2000]. 

12. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
 
![report-builder-kpi-format-numbers](../reporting-services/media/report-builder-kpi-format-numbers.png)

## <a name="BackgroundColors"></a>3. Utiliser les couleurs d'arrière-plan pour afficher un indicateur de performance clé  
Les couleurs d'arrière-plan peuvent avoir la valeur d'une expression qui est évaluée lorsque vous exécutez le rapport.  
  
### <a name="to-display-the-present-state-of-a-kpi-by-using-background-colors"></a>Pour afficher l'état actuel d'un KPI en utilisant des couleurs d'arrière-plan  
  
1.  Dans le tableau, cliquez avec le bouton droit sur la deuxième cellule `[Sum(Sales)]` (ligne de sous-total qui affiche les ventes d’une sous-catégorie), puis cliquez sur **Propriétés de la zone de texte**. 

    Vous devez sélectionner la cellule, pas le texte qu’elle contient, pour afficher **Propriétés de la zone de texte**. 
    
    ![report-builder-text-box-properties](../reporting-services/media/report-builder-text-box-properties.png)
  
2.  Sous l’onglet **Remplissage** , cliquez sur le bouton **fx** en regard de **Couleur de remplissage** , puis entrez l’expression suivante dans le champ **Définir l’expression pour : BackgroundColor** :  
  
    `=IIF(Sum(Fields!Sales.Value) >= 5000 ,"Lime", IIF(Sum(Fields!Sales.Value) < 2500, "Red","Yellow"))`  
  
     Cette expression fait passer la couleur d’arrière-plan au tilleul foncé pour chaque cellule qui contient une somme agrégée supérieure ou égale à 5000 pour l’élément `[Sum(Sales)]` . Les valeurs de `[Sum(Sales)]` entre 2500 et 5000 apparaissent sur fond jaune. Les valeurs inférieures à 2500 apparaissent sur fond rouge.  
  
1.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
2.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Dans la ligne de sous-total qui affiche les ventes d'une sous-catégorie, la couleur d'arrière-plan de la cellule est le rouge, le jaune ou le vert, selon la valeur de la somme des ventes.  

![report-builder-kpi-colors](../reporting-services/media/report-builder-kpi-colors.png)
  
## <a name="Gauge"></a>4. Afficher un indicateur de performance clé à l'aide d'une jauge  
Une jauge représente une valeur unique dans un dataset. Ce didacticiel utilise une jauge linéaire horizontale, car sa forme et sa simplicité la rendent facile à lire, même quand elle est petite et qu’elle se trouve utilisée dans une cellule de tableau. Pour plus d’informations, consultez [Jauges &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/gauges-report-builder-and-ssrs.md).  
  
### <a name="to-display-the-present-state-of-a-kpi-using-a-gauge"></a>Pour afficher l'état présent d'un KPI à l'aide d'une jauge  
  
1.  Rebasculez en mode Conception.  
  
2.  Dans le tableau, cliquez avec le bouton droit sur la poignée de la colonne Sales > **Insérer une colonne** > **Droite**. Une nouvelle colonne est ajoutée au tableau.  

    ![report-builder-kpi-insert-column](../reporting-services/media/report-builder-kpi-insert-column.png)
  
3.  Tapez **Linear KPI** dans l’en-tête de colonne.  
  
4.  Sous l’onglet **Insérer** > **Visualisations des données** > **Jauge**, puis cliquez sur l’aire de conception située en dehors du tableau.   
  
5.  Dans la boîte de dialogue **Sélectionner le type de jauge** , sélectionnez le premier type de jauge linéaire, **Horizontal**.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    Une jauge est ajoutée à l'aire de conception.  
  
7.  À partir du dataset dans le volet Données du rapport, faites glisser le champ `Sales` vers la jauge. Le volet **Données de la jauge** s’ouvre.  
  
    Une fois le champ `Sales` déposé sur la jauge, il rejoint la liste **Valeurs** et ses valeurs sont agrégées à l’aide de la fonction intégrée Sum.  
   
    ![report-builder-kpi-drag-sales-field](../reporting-services/media/report-builder-kpi-drag-sales-field.png)
   
9. Dans le volet **Données de la jauge** , cliquez sur la flèche en regard **LinearPointer1** > **Propriétés du pointeur**.  
  
10. Dans la boîte de dialogue **Propriétés du pointeur linéaire** > onglet **Options du pointeur** > **Type de pointeur**, vérifiez que **Barre** est sélectionné. 
 
11. Cliquez sur **OK**.  
  
12. Cliquez avec le bouton droit sur l’échelle de la jauge, puis cliquez sur **Propriétés de l’échelle**.  
  
13. Dans la boîte de dialogue **Propriétés de l’échelle linéaire** > onglet **Général**, définissez **Maximum** sur 25000.  

    > [!NOTE]  
    > Au lieu d’une constante comme 25000, vous pouvez utiliser une expression pour calculer dynamiquement la valeur de l’option **Maximum** . L'expression utilise alors la fonctionnalité d'agrégation et est semblable à l'expression `=Max(Sum(Fields!Sales.value), "Tablix1")`.  

14. Sous onglet, **Étiquettes** , cochez **Masquer les étiquettes de l’échelle**.

15. Cliquez sur **OK**.
  
14. Faites glisser la jauge dans le tableau jusqu’à la deuxième cellule vide de la colonne Linear KPI, dans la ligne qui affiche le sous-total des ventes pour le champ `Subcategory` , en regard du champ où vous avez ajouté la formule de couleur d’arrière-plan.  
  
    > [!NOTE]  
    > Vous devrez peut-être redimensionner la colonne afin que la jauge linéaire horizontale s’ajuste à la taille des cellules. Pour redimensionner la colonne, sélectionnez le tableau et faites glisser les poignées de colonne. L’aire de conception de rapport est redimensionnée pour s’ajuster au tableau.  
  
15. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
    La longueur horizontale de la barre verte de la jauge varie en fonction de la valeur du KPI.  
  
![report-builder-linear-kpi](../reporting-services/media/report-builder-linear-kpi.png) 
  
## <a name="Indicator"></a>5. Afficher un indicateur de performance clé à l'aide d'un indicateur  
Les indicateurs sont de petites jauges simples qui permettent d'obtenir en un coup d'œil des valeurs de données. En raison de leur taille et de leur simplicité, les indicateurs sont souvent utilisés dans les tableaux et les matrices. Pour plus d’informations, consultez [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/indicators-report-builder-and-ssrs.md).  
  
### <a name="to-display-the-present-state-of-a-kpi-using-an-indicator"></a>Pour afficher l'état présent d'un KPI à l'aide d'un indicateur  
  
1.  Basculez en mode Conception.  
  
2.  Dans le tableau, cliquez avec le bouton droit sur la poignée de la colonne Linear KPI que vous avez ajoutée au cours de la dernière procédure > **Insérer une colonne** > **Droite**. Une nouvelle colonne est ajoutée au tableau.  
  
3.  Tapez **Stoplight KPI** dans l’en-tête de colonne.  
  
4.  Cliquez sur la cellule qui contient le sous-total de la sous-catégorie, en regard de la jauge linéaire que vous avez ajoutée au cours de la dernière procédure.  
  
5.  Sous l’onglet **Insérer** > **Visualisations des données** > double-cliquez sur **Indicateur**.  
  
6.  Dans la boîte de dialogue **Sélectionner un type d’indicateur** , sous **Formes**, sélectionnez le premier type de forme, **3 Indicateurs (sans bordure)**.  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    L’indicateur est ajouté à la cellule de la nouvelle colonne Stoplight KPI.  
  
8.  Cliquez avec le bouton droit sur l’indicateur, puis cliquez sur **Propriétés de l’indicateur**.  
  
9. Sous l’onglet **Valeur et états** , dans la zone **Valeur** , sélectionnez **[SUM (Sales)]**. Ne modifiez pas les autres options.  
  
    Par défaut, la synchronisation des données se produit au niveau de la région de données et vous voyez s’afficher la valeur **Tablix1**, le nom de la région de données de table dans le rapport, dans la zone **Étendue de synchronisation** .  
  
    Dans ce rapport, vous pouvez également modifier l'étendue d'un indicateur placé dans la cellule du sous-total de la sous-catégorie pour synchroniser le champ SalesDate.  
  
11. Cliquez sur **OK**.

11. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  

![report-builder-kpi-stoplight](../reporting-services/media/report-builder-kpi-stoplight.png)
  
## <a name="Title"></a>6. Ajouter un titre de rapport  
Un titre de rapport s'affiche dans la partie supérieure du rapport. Vous pouvez placer le titre du rapport dans un en-tête de rapport, ou si le rapport n'en utilise pas, dans une zone de texte située en haut du corps du rapport. Dans cette section, vous utilisez la zone de texte placée automatiquement en haut du corps du rapport.  
  
Vous pouvez améliorer le texte en appliquant différents types de styles de police, de tailles et de couleurs à des expressions et des caractères spécifiques. Pour plus d’informations, consultez [Mettre en forme du texte dans une zone de texte &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/format-text-in-a-text-box-report-builder-and-ssrs.md).  
  
### <a name="to-add-a-report-title"></a>Pour ajouter un titre de rapport  
  
1.  Dans l'aire de conception, cliquez sur **Cliquez pour ajouter un titre**.  
  
2.  Tapez **Product Sales KPIs**, puis cliquez à l’extérieur de la zone de texte.  
  
3.  Si vous le souhaitez, cliquez avec le bouton droit sur la zone de texte qui contient **Product Sales KPI**, cliquez sur **Propriétés de la zone de texte**, puis sous l’onglet Police, sélectionnez différents types de styles de police, tailles et couleurs.  
  
4.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="Save"></a>7. Enregistrer le rapport  
Enregistrez le rapport sur un serveur de rapports ou sur votre ordinateur. Si vous n'enregistrez pas le rapport sur le serveur de rapports, plusieurs fonctionnalités de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] telles que les parties de rapports et les sous-rapports ne sont pas disponibles.  
  
### <a name="to-save-the-report-on-a-report-server"></a>Pour enregistrer le rapport sur un serveur de rapports  
  
1.  À partir du bouton **Générateur de rapports** , cliquez sur **Enregistrer sous**.  
  
2.  Cliquez sur **Sites et serveurs récents**.  
  
3.  Sélectionnez ou tapez le nom du serveur de rapports sur lequel vous êtes autorisé à enregistrer des rapports.  
  
    Le message « Connexion au serveur de rapports » s'affiche. Une fois la connexion établie, le contenu du dossier de rapports spécifié par l'administrateur du serveur de rapports s'affiche comme emplacement par défaut des rapports.  
  
4.  Dans **Nom**, remplacez le nom par défaut par **Product Sales KPI**.  
  
5.  Cliquez sur **Enregistrer**.  
  
Le rapport est enregistré sur le serveur de rapports. Le nom du serveur de rapports auquel vous êtes connecté est indiqué dans la barre d'état située au bas de la fenêtre.  
  
### <a name="to-save-the-report-on-your-computer"></a>Pour enregistrer le rapport sur votre ordinateur  
  
1.  À partir du bouton **Générateur de rapports** , cliquez sur **Enregistrer sous**.  
  
2.  Cliquez sur **Bureau**, **Mes documents**ou **Poste de travail**, puis naviguez jusqu’au dossier où vous souhaitez enregistrer le rapport.  
  
> [!NOTE]  
> Si vous n’avez pas accès à un serveur de rapports, cliquez sur **Bureau**, **Mes documents**ou **Poste de travail** et enregistrez le rapport sur votre ordinateur.  
  
1.  Dans **Nom**, remplacez le nom par défaut par **Product Sales KPI**.  
  
2.  Cliquez sur **Enregistrer**.  
  
## <a name="next-steps"></a>Next Steps  
Vous avez terminé le didacticiel d'ajout d'un indicateur de performance clé à votre rapport. Pour plus d'informations, consultez :
*  [Jauges](../reporting-services/report-design/gauges-report-builder-and-ssrs.md)
* [Indicateurs](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a> Voir aussi  
* [Didacticiels du Générateur de rapports](../reporting-services/report-builder-tutorials.md)
* [Générateur de rapports dans SQL Server 2016](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

