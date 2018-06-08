---
title: 'Didacticiel : création d’un rapport de tableau de base (Générateur de rapports) | Microsoft Docs'
ms.custom: ''
ms.date: 06/23/2016
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
ms.assetid: d9e30521-f8ae-4c45-89c3-d40727f622f7
caps.latest.revision: 16
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: e03e955d70e46dea954dcc4cce83faece6c2e2ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-creating-a-basic-table-report-report-builder"></a>Didacticiel : création d'un rapport de tableau de base (Générateur de rapports)
Ce didacticiel vous apprend à créer un rapport de tableau de base à partir des exemples de données de ventes. L’illustration suivante montre le rapport que vous allez créer.  
  
![SSRS_Tutorial_Basic_Table_Report](../reporting-services/media/ssrs-tutorial-basic-table-report.png)  
  

Durée estimée pour effectuer le didacticiel : 20 minutes.  
  
## <a name="requirements"></a>Spécifications  
Pour plus d’informations sur les spécifications, consultez [Éléments requis pour les didacticiels &#40;Générateur de rapports&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="CreateTable"></a>1. Créer un rapport à l’aide d’un Assistant  
Créez un rapport de tableau avec l’Assistant Tableau ou matrice. Il existe deux modes : création de rapport et création de dataset partagé. En mode création de rapport, vous pouvez spécifier les données dans le volet des données de rapport et la disposition du rapport dans l'aire de conception. En mode création de dataset partagé, vous créez des requêtes de dataset à partager avec d'autres utilisateurs. Dans ce didacticiel, vous allez utiliser le mode création de rapport.  
  
### <a name="to-create-a-report"></a>Pour créer un rapport  
  
1.  [Démarrez le Générateur de rapports](../reporting-services/report-builder/start-report-builder.md) à partir de votre ordinateur, du portail web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] ou du mode intégré SharePoint.  
  
    La boîte de dialogue **Nouveau rapport ou dataset** s’ouvre.  
  
    Si vous ne voyez pas la boîte de dialogue **Nouveau rapport ou dataset**, dans le menu **Fichier**, choisissez **Nouveau**.  
  
2.  Dans le volet gauche, assurez-vous que **Nouveau rapport** est sélectionné.  
  
3.  Dans le volet droit, cliquez sur **Assistant Tableau ou matrice**.  
  
## <a name="DataConnection"></a>1a. Spécifier une connexion de données dans l'Assistant Tableau  
Une connexion de données contient les informations nécessaires pour se connecter à une source de données externe telle qu'une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . En règle générale, vous obtenez les informations de connexion et le type d'informations d'identification à utiliser auprès du propriétaire de la source de données. Pour spécifier une connexion de données, vous pouvez utiliser une source de données partagée sur le serveur de rapports ou créer une source de données incorporée utilisée uniquement dans ce rapport.  
  
Dans ce didacticiel, vous allez utiliser une source de données incorporée. Pour en savoir plus sur l’utilisation des sources de données partagées, consultez [Autres procédures pour l’obtention d’une connexion de données &#40;Générateur de rapports&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
### <a name="to-create-an-embedded-data-source"></a>Pour créer une source de données incorporée  
  
1.  Dans la page **Choisir un dataset** , sélectionnez **Créer un dataset**, puis cliquez sur **Suivant**. La page **Choisir une connexion à une source de données** s’ouvre.  
  
2.  Cliquez sur **Nouveau**. La boîte de dialogue **Propriétés de la source de données** s'ouvre.  
  
3.  Dans **Nom**, tapez **Product_Sales** comme nom de la source de données.  
  
4.  Dans **Sélectionner un type de connexion**, assurez-vous que **Microsoft SQL Server** est sélectionné.  
  
5.  Dans **Chaîne de connexion**, tapez le texte suivant, où \<servername> est le nom d’une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :  
  
    ```  
    Data Source=<servername>  
    ```  
  
    Dans la mesure où vous utilisez une requête qui contient les données au lieu de récupérer ces dernières à partir d'une base de données, la chaîne de connexion n'inclut pas le nom de la base de données. Pour plus d’informations, voir [Éléments requis pour les didacticiels (Générateur de rapports)](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
6.  Cliquez sur l’onglet **Informations d’identification** . Entrez les informations d'identification nécessaires pour accéder à la source de données externe.  
  
7. Recliquez sur l’onglet Général. Pour vérifier vous pouvez vous connecter à la source de données, cliquez sur **Tester la connexion**.  
  
    Le message « La connexion a été correctement créée » s'affiche.  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    Vous revenez à la page **Choisir une connexion à une source de données** , avec votre nouvelle source de données sélectionnée.  
  
9. Cliquez sur **Suivant**.  
  
## <a name="Query"></a>1b. Créer une requête dans l'Assistant Tableau  
Dans un rapport, vous pouvez utiliser un dataset partagé qui comprend une requête prédéfinie ou vous pouvez créer un dataset incorporé utilisable uniquement dans votre rapport. Dans ce didacticiel, vous allez créer un dataset incorporé.  
  
> [!NOTE]  
> Dans ce didacticiel, la requête contient les valeurs de données, afin qu'il ne soit pas nécessaire de disposer d'une source de données externe. Cela rend la requête assez longue. Dans un environnement métier, une requête ne contient pas les données. Ceci est nécessaire à des fins de formation uniquement.  
  
### <a name="to-create-a-query"></a>Pour créer une requête  
  
1.  Dans la page **Créer une requête** , le concepteur de requêtes relationnelles est ouvert. Pour ce didacticiel, vous utiliserez le Concepteur de requêtes textuel.  
  
    Cliquez sur **Modifier en tant que texte**. Le Concepteur de requêtes textuel affiche un volet de requête et un volet de résultats.  
  
2.  Collez la requête [!INCLUDE[tsql](../includes/tsql-md.md)] suivante dans la zone vide en haut.  
  
    ```  
    SELECT CAST('2009-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Carrying Case' as Product, CAST(9924.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2009-01-11' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Mini Battery Charger' as Product, CAST(1056.00 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate,  'Accessories' as Subcategory,  
       'Telephoto Conversion Lens' as Product, CAST(1380.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2009-01-06' AS date) as SalesDate,'Accessories' as Subcategory,    
       'USB Cable' as Product, CAST(780.00 AS money) AS Sales, 26 as Quantity  
    UNION SELECT CAST('2009-01-08' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(3798.00 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2009-01-09' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Business Videographer' as Product, CAST(10400.00 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2009-01-10' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Social Videographer' as Product, CAST(3000.00 AS money) AS Sales, 60 as Quantity  
    UNION SELECT CAST('2009-01-11' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Advanced Digital' as Product, CAST(7234.50 AS money) AS Sales, 39 as Quantity  
    UNION SELECT CAST('2009-01-07' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Compact Digital' as Product, CAST(10836.00 AS money) AS Sales, 84 as Quantity  
    UNION SELECT CAST('2009-01-08' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Consumer Digital' as Product, CAST(2550.00 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2009-01-05' AS date) as SalesDate, 'Digital' as Subcategory,   
       'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2009-01-09' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera 35mm' as Product, CAST(18530.00 AS money) AS Sales, 34 as Quantity  
    UNION SELECT CAST('2009-01-07' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera' as Product, CAST(26576.00 AS money) AS Sales, 88 as Quantity  
  
    ```  
  
3.  Dans la barre d’outils du Concepteur de requêtes, cliquez sur **Exécuter** (**!**).  
  
    La requête s'exécute et affiche le jeu de résultats pour les champs SalesDate, Subcategory, Product, Sales et Quantity.  
  
    Dans le jeu de résultats, les en-têtes de colonnes sont basés sur les noms présents dans la requête. Dans le dataset, les en-têtes de colonnes deviennent les noms de champs et sont enregistrés dans le rapport. Après avoir terminé l'Assistant, vous pouvez utiliser le volet des données de rapport pour afficher la collection de champs de dataset.  
  
4.  Cliquez sur **Suivant**.  
  
## <a name="Groups"></a>1c. Organiser les données en groupes dans l'Assistant Tableau  
Lorsque vous sélectionnez des champs à regrouper, vous concevez un tableau dont les lignes et les colonnes affichent des données de détail et des données agrégées.  
  
### <a name="to-organize-data-into-groups"></a>Pour organiser les données en groupes  
  
1.  Dans la page **Organiser les champs** , faites glisser Product vers **Valeurs**.  
  
2.  Faites glisser Quantity vers **Valeurs** et placez-le sous Product.  
  
    Quantity est automatiquement agrégé par la fonction Sum, l'agrégat par défaut des champs numériques. La valeur est [Sum(Quantity)].  
  
    Cliquez sur la flèche à côté de [Sum(Quantity)] pour afficher les autres fonctions d’agrégation disponibles. Ne modifiez pas la fonction d'agrégation.  
  
3.  Faites glisser Sales vers **Valeurs** et placez-le sous [Sum(Quantity)].  
  
    Sales est agrégé par la fonction Sum. La valeur est [Sum(Sales)].  
  
    Les étapes 1, 2 et 3 spécifient les données à afficher dans le tableau.  
  
4.  Faites glisser SalesDate vers **Groupes de lignes**.  
  
5.  Faites glisser Subcategory vers **Groupes de lignes** et placez-le sous SalesDate.  
  
    Les étapes 4 et 5 organisent les valeurs des champs par date, puis par sous-catégorie de produits pour chaque date.  
  
6.  Cliquez sur **Suivant**.  
  
## <a name="Subtotals"></a>1d. Ajouter des lignes de sous-total et de total dans l'Assistant Tableau  
Après avoir créé des groupes, vous pouvez ajouter et mettre en forme les lignes dans lesquelles afficher les valeurs agrégées des champs. Vous pouvez afficher toutes les données ou laisser l'utilisateur développer/réduire les données regroupées de manière interactive.  
  
### <a name="to-add-subtotals-and-totals"></a>Pour ajouter des sous-totaux et des totaux  
  
1.  Dans la page **Choisir la disposition** , sous **Options**, vérifiez que **Afficher les sous-totaux et les totaux généraux** est sélectionné.  
  
2.  Vérifiez que **Bloqué, sous-total ci-dessous** est sélectionné.  
  
    Le volet Aperçu de l'Assistant affiche un tableau avec cinq lignes. Lorsque vous exécutez le rapport, chaque ligne est affichée de la manière suivante :  
  
    1.  La première ligne est répétée une fois pour le tableau afin d'afficher les en-têtes de colonnes.  
  
    2.  La deuxième ligne est répétée une fois pour chaque élément de ligne dans la commande client et affiche le nom du produit, la quantité commandée et le total de ligne.  
  
    3.  La troisième ligne est répétée une fois pour chaque commande client afin d'afficher les sous-totaux par commande.  
  
    4.  La quatrième ligne est répétée une fois pour chaque date de commande afin d'afficher les sous-totaux par jour.  
  
    5.  La cinquième ligne est répétée une fois pour le tableau afin d'afficher les totaux généraux.  
  
3.  Décochez l’option **Développer/Réduire les groupes**. Dans ce didacticiel, le rapport que vous créez n'utilise pas la fonctionnalité d'exploration vers le bas qui permet à un utilisateur de développer une hiérarchie de groupe parente afin d'afficher les lignes de groupes enfants et les lignes de détails.  
  
4.  Cliquez sur **Suivant** pour afficher un aperçu de la table, puis sur **Terminer**.  
  
Le tableau est ajouté à l'aire de conception. Le tableau possède 5 colonnes et 5 lignes. Le volet Groupes de lignes affiche trois lignes : SalesDate, Subcategory et Details. Les données de détail sont toutes les données récupérées par la requête de dataset.  
  
## <a name="FormatCurrency"></a>2. Mettre en forme les données en tant que devises  
Par défaut, les données de synthèse du champ Sales affichent un nombre général. Appliquez une mise en forme pour afficher ce nombre dans un format monétaire.   
  
### <a name="to-format-a-currency-field"></a>Pour mettre en forme un champ monétaire  
  
1.  Pour afficher les zones de texte mis en forme et le texte d’espace réservé en tant qu’exemples de valeurs dans la vue Design, sous l’onglet **Accueil**, dans le groupe **Nombre**, cliquez sur la flèche à côté de l’icône **Styles des espaces réservés** > **Valeurs d’aperçu**.  
  
2.   Cliquez sur la cellule de la deuxième ligne (sous la ligne des en-têtes de colonnes) de la colonne Sales et faites glisser la souris vers le bas de façon à sélectionner toutes les cellules qui contiennent `[Sum(Sales)]`.  
  
3.  Sous l’onglet **Accueil** , dans le groupe **Nombre** , cliquez sur le bouton **Devise** . Les cellules changent pour afficher le format de devise.  
  
    Si votre paramètre régional est Anglais (États-Unis), le texte d’exemple par défaut est [**$12,345.00**]. Si vous ne voyez pas d’exemple de valeur monétaire sous l’onglet **Accueil**, dans le groupe **Nombre**, cliquez sur la flèche à côté de l’icône **Styles des espaces réservés** > **Valeurs d’aperçu**.  
  
4.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Les valeurs de synthèse de Sales s'affichent sous forme de devises.  
  
## <a name="FormatDate"></a>3. Mettre en forme les données de date  
Par défaut, le champ SalesDate affiche les informations de date et d’heure. Vous pouvez le mettre en forme de sorte qu'il n'affiche que la date.  
  
### <a name="to-format-a-date-field-as-the-default-format"></a>Pour appliquer à un champ de date le format par défaut  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Cliquez sur la cellule qui contient `[SalesDate]`.  
  
3.  Dans le ruban, sous l’onglet **Accueil** , dans le groupe **Nombre** cliquez sur la flèche et sélectionnez **Date**.  
  
    La cellule affiche la date d’exemple **[1/31/2000]**. Si vous ne voyez pas d’exemple de date, sous l’onglet **Accueil**, dans le groupe **Nombre**, cliquez sur la flèche à côté de l’icône **Styles des espaces réservés** > **Valeurs d’aperçu**.  
  
4.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Les valeurs de SalesDate s'affichent dans le format de date par défaut.  
  
### <a name="to-change-the-date-format-to-a-custom-format"></a>Pour appliquer à une date un format personnalisé  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Sélectionnez la cellule qui contient `[SalesDate]`.  
  
3.  Sous l’onglet **Accueil** , dans le groupe **Nombre** cliquez sur la flèche en bas à droite pour ouvrir la boîte de dialogue.  
  
    La boîte de dialogue **Propriétés de la zone de texte** s’ouvre.  
  
4.  Dans le volet Catégorie, vérifiez que **Date** est sélectionné.  
  
5.  Dans le volet **Type** , sélectionnez **January 31, 2000**.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    La cellule affiche l’exemple de date **[January 31, 2000]**.  
  
7.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
La valeur de SalesDate affiche le nom du mois et non son chiffre.  
  
## <a name="Width"></a>4. Modifier la largeur des colonnes  
Par défaut, chaque cellule d'un tableau contient une zone de texte. Une zone de texte s'étend verticalement pour accueillir le texte lors du rendu de la page. Dans le rapport rendu, chaque ligne s'étend en fonction de la hauteur de la plus grande zone de texte rendue dans la ligne. La hauteur de la ligne dans l'aire de conception n'a aucun impact sur la hauteur de la ligne dans le rapport rendu.  
  
Pour réduire l'espace vertical occupé par chaque ligne, augmentez la largeur de colonne afin d'accueillir le contenu attendu des zones de texte dans la colonne sur une seule ligne.  
  
### <a name="to-change-the-width-of-table-columns"></a>Pour modifier la largeur des colonnes d'un tableau  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Cliquez sur la table pour que les poignées de ligne et de colonne apparaissent au-dessus et à côté de la table.  
  
    Les barres grises le long du haut et du côté de la table sont les poignées de colonne et de ligne.  
  
3.  Placez le curseur entre les séparateurs de colonne pour qu'il se transforme en flèche à deux pointes. Faites glisser les colonnes pour les redimensionner. Par exemple, agrandissez la colonne Product de sorte que le nom des produits tienne sur une seule ligne.  
  
4.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="Title"></a>5. Ajouter un titre de rapport  
Un titre de rapport s'affiche dans la partie supérieure du rapport. Vous pouvez placer le titre du rapport dans un en-tête de rapport, ou si le rapport n'en utilise pas, dans une zone de texte située en haut du corps du rapport. Dans ce didacticiel, vous allez utiliser la zone de texte placée automatiquement en haut du corps du rapport.  
  
Vous pouvez améliorer le texte en appliquant différents types de styles de police, de tailles et de couleurs à des expressions et des caractères spécifiques. Pour plus d’informations, consultez [Mettre en forme du texte dans une zone de texte &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/format-text-in-a-text-box-report-builder-and-ssrs.md).  
  
### <a name="to-add-a-report-title"></a>Pour ajouter un titre de rapport  
  
1.  Dans l'aire de conception, cliquez sur **Cliquez pour ajouter un titre**.  
  
2.  Tapez **Product Sales**, puis cliquez à l’extérieur de la zone de texte.  
  
3.  Cliquez avec le bouton droit sur la zone de texte qui contient **Product Sales** , puis cliquez sur **Propriétés de la zone de texte**.  
  
4.  Dans la boîte de dialogue **Propriétés de la zone de texte** , cliquez sur **Police**.  
  
5.  Dans la liste **Taille** , sélectionnez **18pt**.  
  
6.  Dans la liste **Couleur** , sélectionnez **Bleuet**.  
  
7.  Sélectionnez **Gras**.  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="Save"></a>6. Enregistrer le rapport  
Enregistrez le rapport sur un serveur de rapports ou sur votre ordinateur. Si vous n'enregistrez pas le rapport sur le serveur de rapports, plusieurs fonctionnalités de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] telles que les parties de rapports et les sous-rapports ne sont pas disponibles.  
  
### <a name="to-save-the-report-on-a-report-server"></a>Pour enregistrer le rapport sur un serveur de rapports  
  
1.  Cliquez sur **Fichier** > **Enregistrer sous**.  
  
2.  Cliquez sur **Sites et serveurs récents**.  
  
3.  Sélectionnez ou tapez le nom du serveur de rapports sur lequel vous êtes autorisé à enregistrer des rapports.  
  
    Le message « Connexion au serveur de rapports » s'affiche. Une fois la connexion établie, le contenu du dossier de rapports spécifié par l'administrateur du serveur de rapports s'affiche comme emplacement par défaut des rapports.  
  
4.  Dans **Nom**, remplacez **Sans titre** par **Product_Sales**.  
  
5.  Cliquez sur **Enregistrer**.  
  
Le rapport est enregistré sur le serveur de rapports. Le nom du serveur de rapports auquel vous êtes connecté est indiqué dans la barre d'état située au bas de la fenêtre.  
  
### <a name="to-save-the-report-on-your-computer"></a>Pour enregistrer le rapport sur votre ordinateur  
  
1.  Cliquez sur **Fichier** > **Enregistrer sous**.  
  
2.  Cliquez sur **Bureau**, **Mes documents**ou **Poste de travail**, puis naviguez jusqu’au dossier où vous souhaitez enregistrer le rapport.  
  
3.  Dans **Nom**, remplacez **Sans titre** par **Product Sales**.  
  
4.  Cliquez sur **Enregistrer**.  
  
## <a name="Export"></a>7. Exporter le rapport  
Les rapports peuvent être exportés dans différents formats, par exemple, Microsoft Excel et les fichiers de valeurs séparées par des virgules (CSV). Pour plus d’informations, consultez [Export Reports &#40;Report Builder and SSRS&#41;](../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
Dans ce didacticiel, vous allez exporter le rapport vers Excel et définir une propriété du rapport afin de fournir un nom personnalisé pour l'onglet de classeur.  
  
### <a name="to-specify-the-workbook-tab-name"></a>Pour spécifier le nom de l'onglet de classeur  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Cliquez n’importe où dans l’aire de conception, en dehors du rapport.  
  
3.  Dans le volet Propriétés, recherchez la propriété InitialPageName et tapez **Product Sales Excel**.  
  
    > [!NOTE]  
    > Si le volet Propriétés n’est pas visible, sélectionnez **Propriétés** sous l’onglet **Affichage**.  
    > Si vous ne voyez pas de propriété dans le volet Propriétés, essayez de sélectionner le bouton **Alphabétique** en haut du volet pour trier toutes les propriétés par ordre alphabétique.   
  
### <a name="to-export-a-report-to-excel"></a>Pour exporter un rapport vers Excel  
  
1.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
2.  Dans le ruban, cliquez sur **Exporter** > **Excel**.  
  
    Le rapport s’ouvre.  
  
3.  Dans la boîte de dialogue **Enregistrer sous** , naviguez jusqu’à l’emplacement où vous voulez enregistrer le fichier.  
  
4.  Dans la zone **Nom de fichier** , tapez **Product_Sales_Excel**.  
  
5.  Vérifiez que le type de fichier est **Excel (\*.xlsx)**.  
  
6.  Cliquez sur **Enregistrer**.  
  
### <a name="to-view-the-report-in-excel"></a>Pour afficher le rapport dans Excel  
  
1.  Ouvrez le dossier dans lequel vous enregistrez le classeur et double-cliquez sur **Product_Sales_Excel.xlsx**.  
  
2.  Vérifiez que le nom de l’onglet de classeur est **Product Sales Excel**.  
  
## <a name="next-steps"></a>Next Steps  
Ceci conclut la procédure pas à pas décrivant comment créer un rapport de tableau de base. Pour plus d’informations sur les tables, consultez [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
[Didacticiels du Générateur de rapports](../reporting-services/report-builder-tutorials.md)  
[Générateur de rapports dans SQL Server 2016](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

