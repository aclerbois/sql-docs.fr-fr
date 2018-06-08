---
title: 'Didacticiel : mettre en forme du texte (Générateur de rapports) | Microsoft Docs'
ms.custom: ''
ms.date: 05/30/2017
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
ms.assetid: 67d8513e-8a70-464b-b87f-e91d010cfd82
caps.latest.revision: 16
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c4135b411028d38a8c3ac91ed381e8c1708a8a66
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-format-text-report-builder"></a>Didacticiel : mettre en forme du texte (Générateur de rapports)

Dans ce didacticiel, vous allez vous entraîner à mettre en forme le texte de plusieurs façons dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] . Vous pouvez expérimenter avec différents formats. 

Après avoir configuré le rapport vierge avec la source de données et le dataset, vous pouvez choisir les formats que vous souhaitez explorer. L'illustration suivante montre un rapport similaire à celui que vous allez créer.  
  
![report-build-format-report](../reporting-services/media/report-build-format-report.png) 
  
Dans une étape, vous allez sciemment générer une erreur afin de voir pourquoi il s'agit d'une erreur. Vous corrigerez ensuite l'erreur pour obtenir l'effet souhaité.  
    
Durée estimée pour effectuer le didacticiel : 20 minutes.  
  
## <a name="requirements"></a>Spécifications  
Pour plus d’informations sur les spécifications, consultez [Éléments requis pour les didacticiels &#40;Générateur de rapports&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="CreateReport"></a>Créer un rapport vierge avec une source de données et un dataset  
  
### <a name="to-create-a-blank-report"></a>Pour créer un rapport vierge  
  
1.  [Démarrez le Générateur de rapports](../reporting-services/report-builder/start-report-builder.md) à partir de votre ordinateur, du portail web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] ou du mode intégré SharePoint.  
  
    La boîte de dialogue **Nouveau rapport ou dataset** s’ouvre.  
  
    Si vous ne voyez pas la boîte de dialogue **Nouveau rapport ou dataset**, dans le menu **Fichier**, choisissez **Nouveau**.  
 
2.  Dans le volet gauche de la boîte de dialogue **Mise en route** , assurez-vous que **Nouveau rapport** est sélectionné.  
  
3.  Dans le volet droit, cliquez sur **Rapport vierge**.  
  
### <a name="to-create-a-data-source"></a>Pour créer une source de données  
  
1.  Dans le volet Données du rapport, cliquez sur **Nouveau** > **Source de données**.  

    Si le volet **Données du rapport** n’est pas visible, cochez **Données du rapport** sous l’onglet **Affichage**.
  
2.  Dans la zone **Nom** , tapez **TextDataSource**  
  
3.  Cliquez sur **Utiliser une connexion incorporée dans mon rapport**.  
  
4.  Vérifiez que le type de connexion est bien Microsoft SQL Server puis, dans la zone **Chaîne de connexion** , tapez : `Data Source = <servername>`  
  
    > [!NOTE]  
    > L’expression `<servername>`, par exemple Rapport001, spécifie un ordinateur sur lequel une instance du moteur de base de données SQL Server est installée. Ce didacticiel n’a pas besoin de données spécifiques. Il a juste besoin d’une connexion à une base de données SQL Server. Si une connexion à une source de données est déjà répertoriée sous **Connexions à la source de données**, vous pouvez la sélectionner et passer à la procédure suivante, « Pour créer un dataset ». Pour plus d’informations, consultez [Autres manières d’obtenir une connexion de données &#40;Générateur de rapports&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-create-a-dataset"></a>Pour créer un dataset  
  
1.  Dans le volet Données du rapport, cliquez sur **Nouveau** > **Dataset**.  
  
2.  Vérifiez que la source de données est **TextDataSource**.  
  
3.  Dans la zone **Nom** , tapez **TextDataset**.  
  
4.  Vérifiez que le type de requête **Texte** est sélectionné, puis cliquez sur **Concepteur de requêtes**.  
  
5.  Cliquez sur **Modifier en tant que texte**.  
  
6.  Collez la requête suivante dans le volet de requête :  

    > [!NOTE]  
    > Dans ce didacticiel, la requête contient déjà les valeurs de données, afin qu'il ne soit pas nécessaire de disposer d'une source de données externe. Cela rend la requête assez longue. Dans un environnement métier, une requête ne contient pas les données. Ceci est nécessaire à des fins de formation uniquement.  
  
    ```  
    SELECT CAST('2015-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Carrying Case' as Product, CAST(16996.60 AS money) AS Sales, 68 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(13747.25 AS money) AS Sales, 55 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Carrying Case' as Product, CAST(9248.15 AS money) As Sales, 37 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1800.00 AS money) AS Sales, 24 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1125.00 AS money) AS Sales, 15 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory,  'Lens Adapter' as Product, CAST(742.50 AS money) AS Sales, 11 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1417.50 AS money) AS Sales, 21 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(13497.30 AS money) AS Sales, 54 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(11997.60 AS money) AS Sales, 48 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory, 'Carrying Case' as Product, CAST(10247.95 AS money) As Sales, 41 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory, 'Tripod' as Product, CAST(1200.00 AS money) AS Sales, 16 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(2025.00 AS money) AS Sales, 27 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Tripod' as Product, CAST(1425.00 AS money) AS Sales, 19 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(887.50 AS money) AS Sales, 13 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Accessories' as Subcategory, 'Lens Adapter' as Product, CAST(607.50 AS money) AS Sales, 9 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Accessories' as Subcategory,'Lens Adapter' as Product, CAST(1215.00 AS money) AS Sales, 18 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,  'Lauren Johnson' as FullName,'Central' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(10191.00 AS money) AS Sales, 79 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,  'Warren Pal' as FullName,'North' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(8772.00 AS money) AS Sales, 68 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate,  'Fernando Ross' as FullName,'South' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(10578.00 AS money) AS Sales, 82 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory,'Digital' as Subcategory, 'Slim Digital' as Product, CAST(7218.10 AS money) AS Sales, 38 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory,'Digital' as Subcategory, 'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory,'Digital' as Subcategory,'Slim Digital' as Product, CAST(9307.55 AS money) AS Sales, 49 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Lauren Johnson' as FullName,'Central' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(3870.00 AS money) AS Sales, 30 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Warren Pal' as FullName,'North' as Territory, 'Digital' as Subcategory,'Compact Digital' as Product, CAST(5805.00 AS money) AS Sales, 45 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Fernando Ross' as FullName,'South' as Territory, 'Digital' as Subcategory, 'Compact Digital' as Product, CAST(8643.00 AS money) AS Sales, 67 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Lauren Johnson' as FullName,'Central' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(9877.40 AS money) AS Sales, 52 as Quantity, 'Install Report Builder' as LinkText, 'http://go.microsoft.com/fwlink/?LinkId=154882' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Warren Pal' as FullName,'North' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(12536.70 AS money) AS Sales, 66 as Quantity, 'Report Builder in SQL Server' as Link, 'http://go.microsoft.com/fwlink/?LinkId=160556' AS URL  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Fernando Ross' as FullName,'South' as Territory, 'Digital' as Subcategory, 'Slim Digital' as Product, CAST(6648.25 AS money) AS Sales, 35 as Quantity, 'What is New in Reporting Services (SSRS)' as Link, 'http://go.microsoft.com/fwlink/?LinkId=165064' AS URL  
    ```  
  
7.  Cliquez sur Exécuter (**!**) pour exécuter la requête.  
  
    Les résultats de la requête sont les données qui peuvent être affichées dans votre rapport.  
  
8.  [!INCLUDE[clickOK](../includes/clickok-md.md)]

9.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="AddField"></a>Ajouter un champ à l'aire de conception du rapport  
Si vous souhaitez qu'un champ de votre dataset apparaisse dans un rapport, votre premier réflexe peut être de le faire glisser directement sur l'aire de conception. Cet exercice montre pourquoi cela ne fonctionne pas et ce que vous devez faire à la place.  
  
### <a name="to-add-a-field-to-the-report-and-get-the-wrong-result"></a>Pour ajouter un champ au rapport (et obtenir le résultat incorrect)  
  
1.  Faites glisser le champ **FullName** du volet Données du rapport vers l’aire de conception.  
  
    Le Générateur de rapports crée une zone de texte contenant une expression, représentée sous la forme `<Expr>`.  
  
2.  Cliquez sur **Exécuter**.  
  
    Un seul enregistrement, **Fernando Ross**, est visible. Il correspond au premier enregistrement de la requête dans l’ordre alphabétique. Le champ ne se répète pas pour afficher les autres enregistrements.  
  
3.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
4.  Sélectionnez l’expression `<Expr>` dans la zone de texte.  
  
5.  Dans le volet Propriétés, les éléments suivants sont affichés pour la propriété **Valeur** (si le volet Propriétés n’est pas affiché, sous l’onglet **Affichage** , cochez **Propriétés**) :  
  
    ```  
    =First(Fields!FullName.Value, "TextDataSet")  
    ```  
  
    La fonction `First` permet de récupérer uniquement la première valeur d'un champ, et c'est ce qu'elle a fait.  
  
    Le fait de faire glisser le champ directement sur l'aire de conception a créé une zone de texte. Les zones de texte par elles-mêmes ne sont pas des régions de données et n'affichent donc pas les données d'un dataset de rapport. Les zones de texte dans les régions de données, comme les tableaux, les matrices et les listes, affichent des données.  
  
6.  Sélectionnez la zone de texte (si l'expression est sélectionnée, appuyez sur Échap pour sélectionner la zone de texte) et appuyez sur la touche Suppr.  
  
### <a name="to-add-a-field-to-the-report-and-get-the-right-result"></a>Pour ajouter un champ au rapport (et obtenir le résultat correct)  
  
1.  Sous l’onglet **Insertion** du Ruban, dans la zone **Régions de données** , cliquez sur **Liste**. Cliquez sur l'aire de conception, puis faites glisser la souris pour créer une zone d'environ 5 cm de large sur 2,5 cm de haut.  
  
2.  Faites glisser le champ **FullName** du volet Données du rapport vers la zone de liste.  
  
    Cette fois, le Générateur de rapports crée une zone de texte contenant l’expression `[FullName]` .  
  
3.  Cliquez sur **Exécuter**.  
  
    Notez que cette fois, la zone se répète de manière à afficher tous les enregistrements de la requête.  
  
4.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
5.  Sélectionnez l'expression dans la zone de texte.  
  
6.  Dans le volet Propriétés, les éléments suivants sont affichés pour la propriété **Valeur** :  
  
    ```  
    =Fields!FullName.Value  
    ```  
  
    En faisant glisser la zone de texte vers la région de données de liste, vous affichez les données de ce champ dans le dataset.  
  
7.  Sélectionnez la zone de liste et appuyez sur la touche Suppr.  
  
## <a name="AddTable"></a>Ajouter un tableau à l'aire de conception du rapport  
Créez ce tableau dans lequel vous pourrez placer les liens hypertexte et le texte pivoté.   
  
1.  Sous l’onglet **Insertion** > **Table** > **Assistant Tableau**.  
  
2.  Dans la page **Choisir un dataset** de l’Assistant Nouveau tableau ou nouvelle matrice, cliquez sur **Choisir un dataset existant dans ce rapport ou un dataset partagé** > **TextDataset (dans ce rapport)** > **Suivant**.  
  
3.  Dans la page **Organiser les champs** , faites glisser les champs **Territory**, **LinkText**et **Product** vers **Groupes de lignes**, faites glisser le champ **Sales** vers **Valeurs**, puis cliquez sur **Suivant**.  

    ![report-builder-text-arrange-fields](../reporting-services/media/report-builder-text-arrange-fields.png)
  
4.  Dans la page **Choisir la disposition** , décochez la case **Développer/réduire les groupes** pour voir le tableau entier, puis cliquez sur **Suivant**. 
  
5.  Cliquez sur **Terminer**.  
  
6.  Cliquez sur **Exécuter**.  
  
    Le tableau semble parfait, mais il contient deux lignes de total. La colonne **LinkText** n’a pas besoin de ligne Total.  
    
    ![report-builder-format-2-totals](../reporting-services/media/report-builder-format-2-totals.png)
  
8.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
9. Sélectionnez la cellule **Total** dans la colonne **LinkText** , puis maintenez la touche Maj enfoncée et sélectionnez les deux cellules à sa droite : la cellule vide dans la colonne **Product** et la cellule `[Sum(Sales)]` dans la colonne **Sales** .  
  
11. Ces trois cellules étant sélectionnées, cliquez avec le bouton droit sur l’une d’elles et cliquez sur **Supprimer la ligne**.  

    ![report-builder-format-delete-rows](../reporting-services/media/report-builder-format-delete-rows.png)
  
12. Cliquez sur **Exécuter**.  

    Il n’y a maintenant qu’une seule ligne Total.
    
    ![report-builder-format-one-total](../reporting-services/media/report-builder-format-one-total.png)
  
## <a name="AddHyperlink"></a>Ajouter un lien hypertexte au rapport  
Dans cette section, vous allez ajouter un lien hypertexte au texte du tableau de la section précédente.  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Cliquez avec le bouton droit dans la cellule qui contient `[LinkText]`, puis cliquez sur **Propriétés de la zone de texte**.  
  
3.  Sous l’onglet **Action** , cliquez sur **Atteindre l’URL**.  
  
5.  Dans la zone **Sélectionner une URL** , cliquez sur **[URL]**, puis sur **OK**.  
  
6.  Notez que le texte n'est en aucune manière différent. Vous devez lui donner l'apparence de texte de lien.  
  
7.  Sélectionnez `[LinkText]`.  
  
8.  Sous l’onglet **Accueil** > **Police**, sélectionnez **Souligné**, puis affectez la valeur **Bleu** à **Couleur**.  
  
9. Cliquez sur **Exécuter**.  
  
    Le texte a maintenant l'apparence d'un lien.  
    
    ![report-builder-format-hyperlink](../reporting-services/media/report-builder-format-hyperlink.png)
  
10. Cliquez sur un lien. Si l'ordinateur est connecté à Internet, un navigateur s'ouvre sur une rubrique d'aide du Générateur de rapports.  
  
## <a name="RotateText"></a>Faire pivoter le texte dans le rapport  
Dans cette section, vous allez faire pivoter une partie du texte du tableau des sections précédentes.  
 
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Cliquez dans la cellule qui contient `[Territory].`  
  
3.  Sous l’onglet **Accueil** , dans la section **Police** , cliquez sur le bouton **Gras** .  
  
4.  Si le volet Propriétés n’est pas ouvert, sous l’onglet **Affichage** , cochez la case **Propriétés** .  
  
5.  Recherchez la propriété WritingMode dans le volet Propriétés, et remplacez **Par défaut** par **Rotate270**.  
 
    > [!NOTE]  
    > Quand les propriétés du volet Propriétés sont organisées en catégories, WritingMode figure dans la catégorie **Localisation** . Assurez-vous que vous avez sélectionné la cellule et non le texte. WritingMode est une propriété de la zone de texte, pas du texte.  

    ![report-builder-select-territory-cell](../reporting-services/media/report-builder-select-territory-cell.png)
   
6.  Sous l’onglet **Accueil** > section **Paragraphe**, sélectionnez **Milieu** et **Centre** pour centrer verticalement et horizontalement le texte dans la cellule.  
  
8.  Cliquez sur Exécuter (**!**).  
  
Le texte de la cellule `[Territory]` s'exécute maintenant verticalement de bas en haut des cellules.  

![report-builder-format-rotate-270](../reporting-services/media/report-builder-format-rotate-270.png)

## <a name="FormatCurrency"></a>Mettre en forme la devise  
  
1.  Cliquez sur **Conception** pour basculer en mode Conception.  
  
2.  Cliquez sur la cellule supérieure du tableau contenant `[Sum(Sales)]`, maintenez la touche Maj enfoncée et cliquez sur la cellule inférieure du tableau contenant `[Sum(Sales)]`.  
  
3.  Sous l’onglet **Accueil** > groupe **Nombre** > bouton **Devise**.  
  
4.  (Facultatif)     Si votre paramètre régional est Anglais (États-Unis), l’exemple de texte par défaut est [**$12,345.00**]. Si vous ne voyez pas s’afficher d’exemple de valeur monétaire, dans le groupe **Nombres** , cliquez sur **Styles des espaces réservés** > **Valeurs d’aperçu**.  

    ![report-builder-placeholder-value-button](../reporting-services/media/report-builder-placeholder-value-button.png)
  
5.  (Facultatif) Sous l’onglet **Accueil** , dans le groupe **Nombre** , cliquez sur le bouton **Réduire les décimales** à deux reprises, pour afficher les valeurs en dollars sans indication de centimes.  
  
6.  Cliquez sur Exécuter (**!**) pour afficher un aperçu du rapport.  
  
Le rapport affiche maintenant les données mises en forme et est plus facile à lire.  

![report-build-format-report](../reporting-services/media/report-build-format-report.png)
    
## <a name="FormatHTML"></a>Afficher le texte avec la mise en forme HTML  
  
1.  Cliquez sur **Conception** pour basculer en mode Conception.  
  
2.  Sous l’onglet **Insertion** , cliquez sur **Zone de texte**, puis sur l’aire de conception, cliquez et faites glisser la souris pour créer une zone de texte sous le tableau d’environ 10 cm de large sur 8 cm de haut.  
  
3.  Copiez le texte suivant et collez-le dans la zone de texte :  
  
    ```  
    <h4>Limitations of cascading style sheet attributes</h4>  
          <p>Only a basic set of <b>cascading style sheet (CSS)</b> attributes are defined:</p>  
          <ul><li>  
              text-align, text-indent  
            </li><li>  
              font-family, font-size  
            </li><li>  
              color  
            </li><li>  
              padding, padding-bottom, padding-top, padding-right, padding-left  
            </li><li>  
              font-weight  
            </li></ul>  
    ```  
  
4.  Faites glisser le bord inférieur de la zone de texte pour qu’elle contienne tout le texte. Notez que l’aire de conception s’agrandit quand vous faites glisser.

5. Sélectionnez tout le texte de la zone de texte.  
  
5.  Cliquez avec le bouton droit sur l’ensemble du texte sélectionné, puis cliquez sur **Propriétés du texte**.  
  
    Il s'agit d'une propriété du texte, pas de la zone de texte. Ainsi, une zone de texte peut contenir à la fois du texte brut et du texte qui utilise des balises HTML comme styles.  
  
6.  Sous l’onglet **Général** , sous **Type de balise**, cliquez sur **HTML - Interpréter les balises HTML comme des styles**.  
  
7.  Cliquez sur **OK**.  
  
8.  Cliquez sur Exécuter (**!**) pour afficher un aperçu du rapport.  
  
Le texte de la zone de texte est affiché sous forme de titre, paragraphe et liste à puce.  
  
![report-builder-format-html](../reporting-services/media/report-builder-format-html.png)

## <a name="Save"></a>Enregistrer le rapport  
Vous pouvez enregistrer les rapports sur un serveur de rapports, dans une bibliothèque SharePoint ou sur l'ordinateur.  
  
Dans ce didacticiel, enregistrez le rapport sur un serveur de rapports. Si vous n'avez pas accès à un serveur de rapports, enregistrez le rapport sur votre ordinateur.  
  
### <a name="to-save-the-report-on-a-report-server"></a>Pour enregistrer le rapport sur un serveur de rapports  
  
1.  À partir du bouton **Générateur de rapports** , cliquez sur **Enregistrer sous**.  
  
2.  Cliquez sur **Sites et serveurs récents**.  
  
3.  Sélectionnez ou tapez le nom du serveur de rapports sur lequel vous êtes autorisé à enregistrer des rapports.  
  
    Le message « Connexion au serveur de rapports » s'affiche. Une fois la connexion établie, le contenu du dossier de rapports spécifié par l'administrateur du serveur de rapports s'affiche comme emplacement par défaut des rapports.  
  
4.  Dans **Nom**, remplacez le nom par défaut par un nom de votre choix.

5.  Cliquez sur **Enregistrer**.  
  
Le rapport est enregistré sur le serveur de rapports. Le nom du serveur de rapports auquel vous êtes connecté est indiqué dans la barre d'état située au bas de la fenêtre.  
  
### <a name="to-save-the-report-on-your-computer"></a>Pour enregistrer le rapport sur votre ordinateur  
  
1.  À partir du bouton **Générateur de rapports** , cliquez sur **Enregistrer sous**.  
  
2.  Cliquez sur **Bureau**, **Mes documents**ou **Poste de travail**, puis naviguez jusqu'au dossier où vous souhaitez enregistrer le rapport.  
  
3.  Dans **Nom**, remplacez le nom par défaut par un nom de votre choix. 
  
4.  Cliquez sur **Enregistrer**.  

## <a name="next-steps"></a>Next Steps

Il existe de nombreuses méthodes pour mettre en forme du texte dans le Générateur de rapports. Le [didacticiel : création d’un rapport de forme libre](../reporting-services/tutorial-creating-a-free-form-report-report-builder.md) contient d’autres exemples.  

[Didacticiels du Générateur de rapports ](../reporting-services/report-builder-tutorials.md) 
[Mise en forme des éléments de rapport](../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)  
[Générateur de rapports dans SQL Server 2016](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
