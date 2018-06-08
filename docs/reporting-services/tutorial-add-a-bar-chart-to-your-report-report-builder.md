---
title: 'Didacticiel : ajouter un graphique à barres à un rapport (Générateur de rapports) | Microsoft Docs'
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
ms.assetid: 6956ebd6-0217-4087-a4fa-5cc1c3804691
caps.latest.revision: 14
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 8ef72373f91b9104c8ff83f6d1d9b012b283df3d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-add-a-bar-chart-to-your-report-report-builder"></a>Didacticiel : ajouter un graphique à barres à un rapport (Générateur de rapports)
Dans ce didacticiel, vous allez utiliser un Assistant dans [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion-md.md)] pour créer un graphique à barres dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] . Ensuite, vous ajouterez un filtre et améliorerez le graphique. 

Un graphique à barres représente les données de catégorie horizontalement. Cela peut aider à :  
  
-   améliorer la lisibilité des noms de catégorie longs ;  
-   améliorer la compréhension des heures représentées sous forme de valeurs ;   
-   comparer la valeur relative de plusieurs séries.  
  
L’illustration suivante montre le graphique à barres que vous allez créer, avec les ventes de 2014 et 2015 pour les cinq meilleurs commerciaux, par ordre décroissant de montant des ventes en 2015.  
  
![report-builder-bar-chart](../reporting-services/media/report-builder-bar-chart.png) 
  
 
> [!NOTE]  
> Dans ce didacticiel, les étapes de l'Assistant sont consolidées en une seule procédure. Pour obtenir des instructions pas à pas sur l’accès à un serveur de rapports, la création d’un dataset et le choix d’une source de données, consultez le premier didacticiel de cette série : [Didacticiel : création d’un rapport de tableau de base &#40;Générateur de rapports&#41;](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md).  
  
Durée estimée pour effectuer ce didacticiel : 15 minutes.  
  
## <a name="requirements"></a>Spécifications  
Pour plus d’informations sur les spécifications, consultez [Éléments requis pour les didacticiels &#40;Générateur de rapports&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="Chart"></a>1. Créer un rapport de graphique à partir de l'Assistant Graphique  
Vous allez créer un dataset incorporé, choisir une source de données partagée et créer un graphique à barres à l’aide de l’Assistant Graphique.  
  
> [!NOTE]  
> Dans ce didacticiel, la requête contient les valeurs de données. Ainsi, aucune source de données externe n’est nécessaire. Cela rend la requête assez longue. Dans un environnement métier, une requête ne contient pas les données. Ceci est nécessaire à des fins de formation uniquement.  
  
1.  [Démarrez le Générateur de rapport](../reporting-services/report-builder/start-report-builder.md) à partir du portail web [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] , du serveur de rapports en mode intégré SharePoint, ou de votre ordinateur.  
  
     La boîte de dialogue **Mise en route** s'affiche.  
  
     ![Générateur de rapports - Mise en route](../reporting-services/media/rb-getstarted.png "Générateur de rapports - Mise en route")  
  
     Si vous ne voyez pas la boîte de dialogue **Mise en route** , cliquez sur **Fichier** >**Nouveau**. La boîte de dialogue **Nouveau rapport ou dataset** contient une grande partie des contenus de la boîte de dialogue **Mise en route** . 
      
2.  Dans le volet gauche, assurez-vous que **Nouveau rapport** est sélectionné.  
  
3.  Dans le volet droit, cliquez sur **Assistant Graphique**.  
  
4.  Dans la page **Choisir un dataset** , cliquez sur **Créer un dataset**, puis sur **Suivant**.  
  
5.  Dans la page **Choisir une connexion à une source de données** , sélectionnez une source de données existante ou naviguez jusqu’au serveur de rapports, sélectionnez une source de données, puis cliquez sur **Suivant**. Vous devrez peut-être entrer un nom d'utilisateur et un mot de passe.  
  
    > [!NOTE]  
    > La source de données que vous choisissez n'a pas d'importance, tant que vous disposez des autorisations appropriées. Vous n'allez pas récupérer de données à partir de la source de données. Pour plus d’informations, consultez [Autres manières d’obtenir une connexion de données &#40;Générateur de rapports&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
6.  Dans la page **Créer une requête** , cliquez sur **Modifier en tant que texte**.  
  
7.  Collez la requête suivante dans le volet de requête :  
  
    ```  
    SELECT 'Luis' as FirstName, 'Alverca' as LastName, CAST(170000.00 AS money) AS SalesYear2015, CAST(150000. AS money) AS SalesYear2014  
    UNION SELECT 'Jeffrey' as FirstName, 'Zeng' as LastName, CAST(210000. AS money) AS SalesYear2015, CAST(190000. AS money) AS SalesYear2014  
    UNION SELECT 'Houman' as FirstName, 'Pournasseh' as LastName, CAST(150000. AS money) AS SalesYear2015, CAST(180000. AS money) AS SalesYear2014  
    UNION SELECT 'Robin' as FirstName, 'Wood' as LastName, CAST(75000. AS money) AS SalesYear2015, CAST(175000. AS money) AS SalesYear2014  
    UNION SELECT 'Daniela' as FirstName, 'Guaita' as LastName,  CAST(170000. AS money) AS SalesYear2015, CAST(175000. AS money) AS SalesYear2014  
    UNION SELECT 'John' as FirstName, 'Yokim' as LastName, CAST(160000. AS money) AS SalesYear2015, CAST(195000. AS money) AS SalesYear2014  
    UNION SELECT 'Delphine' as FirstName, 'Ribaute' as LastName, CAST(180000. AS money) AS SalesYear2015, CAST(205000. AS money) AS SalesYear2014  
    UNION SELECT 'Robert' as FirstName, 'Hernady' as LastName, CAST(140000. AS money) AS SalesYear2015, CAST(180000. AS money) AS SalesYear2014  
    UNION SELECT 'Tanja' as FirstName, 'Plate' as LastName, CAST(150000. AS money) AS SalesYear2015, CAST(160000. AS money) AS SalesYear2014  
    UNION SELECT 'David' as FirstName, 'Bradley' as LastName, CAST(210000. AS money) AS SalesYear2015, CAST(180000. AS money) AS SalesYear2014  
    UNION SELECT 'Michal' as FirstName, 'Jaworski' as LastName, CAST(175000. AS money) AS SalesYear2015, CAST(220000. AS money) AS SalesYear2014  
    UNION SELECT 'Chris' as FirstName, 'Ashton' as LastName, CAST(195000. AS money) AS SalesYear2015, CAST(205000. AS money) AS SalesYear2014  
    UNION SELECT 'Pongsiri' as FirstName, 'Hirunyanitiwatna' as LastName, CAST(175000. AS money) AS SalesYear2015, CAST(215000. AS money) AS SalesYear2014  
    UNION SELECT 'Brian' as FirstName, 'Burke' as LastName, CAST(187000. AS money) AS SalesYear2015, CAST(207000. AS money) AS SalesYear2014  
    ```  
  
8.  (Facultatif) Cliquez sur le bouton Exécuter (**!**) pour voir les données sur lesquelles votre graphique sera basé.  
  
9. Cliquez sur **Suivant**.  
  
## <a name="ChartType"></a>2. Créer un graphique à barres  
 
1.  Dans la page **Choisir un type de graphique** , l’histogramme est le type de graphique par défaut.  
  
2.  Cliquez sur **Barre**, puis sur **Suivant**.  
  
    Dans la page **Organiser les champs du graphique** , il y a quatre champs dans le volet **Champs disponibles** : FirstName, LastName, SalesYear2015 et SalesYear2014.  
  
3.  Faites glisser LastName vers le volet Catégories.  
  
4.  Faites glisser SalesYear2015 vers le volet Valeurs. SalesYear2015 représente le montant des ventes de chaque commercial pour l’année 2015. Le volet Valeurs affiche `[Sum(SalesYear2015)]` , car le graphique affiche l'agrégat pour chaque produit.  
  
5.  Faites glisser SalesYear2014 vers le volet Valeurs sous SalesYear2015. SalesYear2014 représente le montant des ventes de chaque commercial pour l’année 2014.  
  
6.  Cliquez sur **Suivant**.  
  
7.  Cliquez sur **Terminer**.  
  
    Le graphique est ajouté à l'aire de conception. Notez que le nouveau graphique à barres ne fait que représenter les données. La légende indique Last Name A, Last Name B, etc., plutôt que les noms des personnes, juste pour donner une idée de l’apparence du rapport. 
  
9. Cliquez sur le graphique pour afficher ses poignées. Faites glisser l'angle inférieur droit du graphique pour augmenter sa taille. Notez que l’aire de conception s’agrandit lorsque vous faites glisser. 
  
10. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Le graphique à barres affiche les ventes de chaque commercial pour les années 2014 et 2015. La longueur de la barre correspond au total des ventes.  
  
## <a name="AllValues"></a>3. Afficher tous les noms sur l’axe vertical  
Par défaut, seules quelques-unes des valeurs de l'axe vertical s'affichent. Vous pouvez modifier le graphique pour afficher toutes les catégories.  
  
1.  Basculez en mode création de rapport.  
  
2.  Cliquez avec le bouton droit sur l’axe vertical, puis cliquez sur **Propriétés de l’axe vertical**.  
  
3.  Sous **Plage et intervalle de l’axe**, dans la zone **Intervalle** , tapez **1**.  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
> [!NOTE]  
> Si vous ne parvenez pas à lire les noms des commerciaux sur l'axe vertical, vous pouvez augmenter la taille de votre graphique ou modifier les options de mise en forme des étiquettes d'axe.  
  
### <a name="CategoryExpression"></a>Afficher le nom et le prénom sur l'axe vertical  
Vous pouvez modifier l'expression de catégorie pour inclure le nom suivi du prénom de chaque commercial.  
  
1.  Basculez en mode création de rapport.  
  
2.  Double-cliquez sur le graphique pour afficher le volet **Données du graphique** .  
  
3.  Dans la zone **Groupes de catégories** , cliquez avec le bouton droit sur le champ [LastName], puis cliquez sur **Propriétés du groupe de catégories**.  
  
4.  Dans Étiquette, cliquez sur le bouton Expression (Fx).  
  
5.  Tapez l’expression suivante : `=Fields!LastName.Value & ", " & Fields!FirstName.Value`  
  
    Cette expression concatène le nom, une virgule et le prénom.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Si les prénoms n'apparaissent pas lorsque vous exécutez le rapport, vous pouvez actualiser les données manuellement. Toujours en mode Aperçu, sous l’onglet **Exécuter** du groupe **Navigation** , cliquez sur **Actualiser**.  
  
> [!NOTE]  
> Si vous ne parvenez pas à lire les noms des commerciaux sur l'axe vertical, vous pouvez augmenter la taille de votre graphique ou modifier les options de mise en forme des étiquettes d'axe.  
  
## <a name="Sort"></a>4. Modifier l’ordre de tri sur l’axe vertical  
Lorsque vous triez les données d'un graphique, vous modifiez l'ordre des valeurs sur l'axe des abscisses.  
  
1.  Basculez en mode création de rapport.  
  
2.  Double-cliquez sur le graphique pour afficher le volet **Données du graphique** .  
  
3.  Dans la zone **Groupes de catégories** , cliquez avec le bouton droit sur le champ [LastName], puis cliquez sur **Propriétés du groupe de catégories**.  
  
4.  Cliquez sur **Tri**. La page **Modifiez les options de tri** affiche une liste d’expressions de tri. Par défaut, cette liste a une expression de tri identique à l'expression de groupe de la catégorie d'origine.  
  
5.  Dans **Trier par**, cliquez sur **[SalesYear2015]**.  
  
6.  Dans la liste **Tri** , sélectionnez **A-Z** pour que les noms apparaissent dans l’ordre décroissant du montant des ventes 2015.
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
10. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Les noms sur l’axe horizontal sont triés par ordre décroissant du montant des ventes 2015, avec **Zeng** en haut.  
  
## <a name="Legend"></a>5. Déplacer la légende  
Pour améliorer la lisibilité des valeurs du graphique, vous pouvez déplacer la légende du graphique. Par exemple, dans un graphique à barres horizontales, vous pouvez modifier la position de la légende de manière à l'afficher au-dessus ou en dessous de la zone de graphique. Cela permet d'augmenter l'espace horizontal entre les barres.  
  
#### <a name="to-display-the-legend-below-the-chart-area-of-a-bar-chart"></a>Pour afficher la légende sous la zone de graphique d'un graphique à barres  
  
1.  Basculez en mode création de rapport.  
  
2.  Cliquez avec le bouton droit sur la légende du graphique.  
  
3.  Sélectionnez **Propriétés de la légende**.  
  
4.  Pour **Position de la légende**, sélectionnez une position différente. Par exemple, choisissez de positionner le graphique en bas au centre.  
  
    Lorsque la légende est placée en haut ou en bas d'un graphique, la disposition de la légende change de vertical à horizontal. Vous pouvez sélectionner une autre disposition dans la liste déroulante **Disposition** .  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
6.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="ChartTitle"></a>6. Intituler le graphique  
  
1.  Basculez en mode création de rapport.  
  
2.  Sélectionnez les mots **Titre du graphique** en haut du graphique, puis tapez **Ventes pour 2014 et 2015**.  
  
3.  Dans le volet Propriétés, le titre étant sélectionné, affectez la valeur **Noir** au paramètre **Couleur** et la valeur **12 pt** au paramètre **Police**. 
  
4.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="Horizontal"></a>7. Mettre en forme et étiqueter l'axe horizontal  
Par défaut, l'axe horizontal affiche les valeurs dans un format général qui est mis à l'échelle automatiquement pour s'ajuster à la taille du graphique. Vous pouvez le modifier et lui affecter le format monétaire.  
   
1.  Basculez en mode création de rapport.  
  
2.  Cliquez sur l'axe horizontal en bas du graphique pour le sélectionner.  
  
3.  Sous l’onglet **Accueil** > groupe **Nombre** > **Devise**. Les étiquettes de l'axe horizontal changent et utilisent une devise.  
  
3.  (Facultatif) Supprimez les chiffres décimaux. Près du bouton **Devise** , cliquez deux fois sur le bouton **Réduire les décimales** .  
  
4.  Cliquez avec le bouton droit sur l’axe horizontal, puis cliquez sur **Propriétés de l’axe horizontal**.  
  
5.  Sous l’onglet **Nombre** , sélectionnez **Afficher les valeurs en milliers**.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  

8.  Cliquez avec le bouton droit sur l’axe horizontal, puis sélectionnez **Afficher le titre de l’axe**.
  
7.  Dans la zone **Titre de l’axe** , tapez **Ventes en milliers** et appuyez sur Entrée.  

    >**Remarque :** Pendant que vous tapez, la zone Titre de l’axe apparaît sur l’axe vertical. Quand vous appuyez sur Entrée, elle passe sur l’axe horizontal.
  
9. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Le rapport affiche les montants des ventes sur l’axe horizontal sous forme de devises en milliers, sans chiffres décimaux.  
  
## <a name="Filter"></a>8. Ajouter un filtre pour afficher les cinq valeurs supérieures  
Vous pouvez ajouter un filtre au graphique pour spécifier les données du dataset à inclure ou exclure dans le graphique.   
  
1.  Basculez en mode création de rapport.  
  
2.  Double-cliquez sur le graphique pour afficher le volet **Données du graphique** .  
  
3.  Dans la zone **Groupes de catégories** , cliquez avec le bouton droit sur le champ [LastName], puis cliquez sur **Propriétés du groupe de catégories**.  
  
4.  Cliquez sur **Filtres**. La page **Modifiez les filtres** peut afficher une liste d’expressions de filtre. Par défaut, cette liste est vide.  
  
5.  Cliquez sur **Ajouter**. Un nouveau filtre vide apparaît.  
  
6.  Dans **Expression**, tapez **[Sum(SalesYear2015)]**. Cela crée l’expression sous-jacente `=Sum(Fields!SalesYear2015.Value)`, que vous pouvez afficher en cliquant sur le bouton **fx** .  
  
7.  Vérifiez que le type de données est **Text**.  
  
8.  Dans **Opérateur**, sélectionnez **N supérieurs** dans la liste déroulante.  
  
9. Dans **Valeur**, tapez l’expression suivante : **=5**  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
Si les résultats ne sont pas filtrés lorsque vous exécutez le rapport, vous pouvez actualiser les données manuellement. Sous l’onglet **Exécuter** , dans le groupe **Navigation** , cliquez sur **Actualiser**.  
  
Le graphique affiche les noms des cinq meilleurs commerciaux issus des données de ventes 2015.  
  
## <a name="Title"></a>9. Ajouter un titre de rapport  
  
1.  Dans l'aire de conception, cliquez sur **Cliquez pour ajouter un titre**.  
  
2.  Tapez **Graphique à barres des ventes**, appuyez sur Entrée, puis tapez **Cinq meilleurs vendeurs pour 2015**, afin d’obtenir ce qui suit :  
  
    **Graphique à barres des ventes**  
  
    **Cinq meilleurs vendeurs pour 2015**  
  
3.  Sélectionnez **Graphique à barres des ventes**, puis cliquez sur le bouton **Gras** .  
  
4.  Sélectionnez **Cinq meilleurs vendeurs pour 2015**puis, dans la section **Police** de l’onglet **Accueil** , affectez la valeur **10**à la taille de la police.  
  
5.  (Facultatif) Vous devrez peut-être agrandir la zone de texte Titre et descendre le haut du graphique à barres pour que les deux lignes de texte soient visibles.  
  
    Ce titre s'affiche alors dans la partie supérieure du rapport. En l’absence d’en-tête de page défini, les éléments situés au-dessus du corps du rapport font office d’en-tête de rapport.  
  
6.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="Save"></a>10. Enregistrer le rapport  
  
1.  Basculez en mode création de rapport.  
  
2.  Cliquez sur **Fichier** > **Enregistrer sous**.  
  
3.  Dans **Nom**, tapez **Graphique à barres des ventes**.  

    Vous pouvez l’enregistrer sur votre ordinateur ou sur le serveur de rapports.
  
4.  Cliquez sur **Enregistrer**.   
  
## <a name="next-steps"></a>Next Steps  
Vous avez réalisé le didacticiel d'ajout d'un graphique à barres à votre rapport. Pour en savoir plus sur les graphiques, consultez [Graphiques](../reporting-services/report-design/charts-report-builder-and-ssrs.md) et [Graphiques à barres](../reporting-services/report-design/bar-charts-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
[Didacticiels du Générateur de rapports](../reporting-services/report-builder-tutorials.md)  
[Générateur de rapports dans SQL Server 2016](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

