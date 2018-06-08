---
title: 'Leçon 2 : modification des propriétés d’une source de données de rapport | Microsoft Docs'
ms.custom: ''
ms.date: 05/23/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
applies_to:
- SQL Server 2016
ms.assetid: c962b0ff-ce8a-4742-8262-dc730901afcf
caps.latest.revision: 43
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 7ec0c1cc3ce7e2be9b058fc881d0971922f5994f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-modifying-the-report-data-source-properties"></a>Lesson 2: Modifying the Report Data Source Properties
Dans cette leçon du didacticiel sur [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] , vous utilisez le portail web pour sélectionner un rapport qui doit être remis à des destinataires. L’abonnement piloté par les données que vous allez définir distribue le rapport **Sales Order** créé dans le didacticiel [Créer un rapport de tableau de base &amp;#40;Didacticiel SSRS&amp;#41;](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md).  Au cours des étapes qui suivent, vous allez modifier les informations de connexion à la source de données utilisée par le rapport pour extraire les données. Seuls les rapports qui utilisent des **informations d’identification stockées** pour accéder à une source de données de rapport peuvent être distribués par le biais d’un abonnement piloté par les données. Les informations d'identification stockées sont nécessaires pour traiter les rapports de façon autonome.  
  
Vous allez également modifier le dataset et le rapport pour qu'ils utilisent un paramètre permettant de filtrer le rapport sur `[Order]` de sorte que l'abonnement puisse générer plusieurs instances différentes du rapport pour des commandes et des formats de rendu spécifiques.  
  
## <a name="bkmk_modify_datasource"></a>Pour modifier une source de données pour qu’elle utilise les informations d’identification stockées  
  
1.  Accédez au portail web [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] avec des privilèges d’administrateur : par exemple, cliquez avec le bouton droit sur l’icône d’Internet Explorer et cliquez sur **Exécuter en tant qu’administrateur**.  
 
2.    Accédez à l’URL du portail web.  Exemple :   
    `http://<server name>/reports`.  
    `http://localhost/reports`
 **Remarque :** L’URL du *portail* est « Reports », et non « Reportserver », l’URL du *serveur* de rapports.  
3.  Accédez au dossier contenant le rapport **Sales Orders** et, dans le menu contextuel du rapport, cliquez sur **Gérer**.  
 
 ![ssrs_tutorial_datadriven_manage_report](../reporting-services/media/ssrs-tutorial-datadriven-manage-report.png)
  
3.  Cliquez sur **Sources de données** dans le volet gauche.  
  
4.  Vérifiez que le **Type de connexion** est **Microsoft SQL Server**.  
  
5.  Vérifiez que la chaîne de connexion est la suivante et qu’elle suppose que l’exemple de base de données se trouve sur un serveur de base de données local :  
  
    ```  
    Data source=localhost; initial catalog=AdventureWorks2014  
    ```  
  
6.  Cliquez sur **Utiliser les informations d’identification suivantes**.  
  
7. Dans la zone **Type d’informations d’identification**, sélectionnez **Nom d’utilisateur et mot de passe Windows**
8. Tapez votre nom d’utilisateur (utilisez le format *domaine\utilisateur*) et votre mot de passe. Si vous n’êtes pas autorisé à accéder à la base de données AdventureWorks2014, spécifiez une connexion qui rendra cet accès possible.  
    
9. Cliquez sur **Tester la connexion** pour vous assurer que vous pouvez vous connecter à la source de données.  
  
10. Cliquez sur **Enregistrer**.
11. Cliquez sur **Annuler**  
  
11. Affichez le rapport pour vérifier qu'il s'exécute avec les informations d'identification que vous avez spécifiées. .  
  
## <a name="bkmk_modify_dataset"></a>Pour modifier AdventureWorksDataset  
 Dans les étapes suivantes, vous modifiez le dataset pour utiliser un paramètre permettant de filtrer le jeu de données en fonction d’un numéro de commande.
1.  Ouvrez le rapport **Sales Orders** dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]  
  
2.  Cliquez avec le bouton droit sur le dataset `AdventureWorksDataset` et cliquez sur **Propriétés du dataset**.  
    ![ssrs_tutorial_datadriven_datasetproperties](../reporting-services/media/ssrs-tutorial-datadriven-datasetproperties.png)  
3.  Ajoutez l'instruction `WHERE (UPPER(SalesOrderNumber) =UPPER(@OrderNumber) or  @OrderNumber IS NULL)` avant l'instruction `Group By` . La syntaxe de requête complète est la suivante :  
  
    ```  
    SELECT soh.OrderDate AS Date, soh.SalesOrderNumber AS [Order], pps.Name AS Subcat, pp.Name AS Product, SUM(sd.OrderQty) AS Qty, SUM(sd.LineTotal)  AS LineTotal  
    FROM Sales.SalesPerson AS sp INNER JOIN  
      Sales.SalesOrderHeader AS soh ON sp.BusinessEntityID = soh.SalesPersonID INNER JOIN  
       Sales.SalesOrderDetail AS sd ON sd.SalesOrderID = soh.SalesOrderID INNER JOIN  
       Production.Product AS pp ON sd.ProductID = pp.ProductID  
    INNER JOIN  
       Production.ProductSubcategory AS pps ON pp.ProductSubcategoryID = pps.ProductSubcategoryID   
    INNER JOIN  
        Production.ProductCategory AS ppc ON ppc.ProductCategoryID = pps.ProductCategoryID  
  
    WHERE (UPPER(SalesOrderNumber) =UPPER(@OrderNumber) or  @OrderNumber IS NULL)  
  
    GROUP BY ppc.Name, soh.OrderDate, soh.SalesOrderNumber, pps.Name, pp.Name, soh.SalesPersonID  
    HAVING (ppc.Name = 'Clothing')  
    ```  
  
4.  Cliquez sur **OK**  
 Dans les étapes suivantes, vous ajoutez un paramètre au rapport.  Le paramètre de rapport alimente le paramètre de dataset. 
## <a name="bkmk_add_reportparameter"></a>Pour ajouter un paramètre de rapport et republier le rapport  
  
1.  Dans le volet **Données du rapport** , développez le dossier de paramètres et double-cliquez sur le paramètre **Ordernumber** .  Il a été créé automatiquement dans les étapes précédentes quand vous avez ajouté le paramètre au dataset. Cliquez sur **Nouveau** , puis sur **Paramètre...**  
 ![ssrs_tutorial_datadriven_parameter](../reporting-services/media/ssrs-tutorial-datadriven-parameter.png) 
2.  Vérifiez que le **Nom** est `OrderNumber`.  
  
3.  Vérifiez que **l’Invite** est `OrderNumber`.  
  
4.  Sélectionnez **Autoriser une valeur vide ("")**.  
  
5.  Sélectionnez **Autoriser les valeurs de type Null**.  
  
6.  Cliquez sur **OK**.  
  
7.  Cliquez sur l’onglet **Aperçu** pour exécuter le rapport. Notez que la zone d’entrée de paramètres apparaît en haut du rapport. Vous pouvez effectuer l'une des opérations suivantes :  
  
    -   Cliquez sur Afficher le rapport pour afficher le rapport dans son intégralité sans utiliser de paramètre.  
  
    -   Désélectionnez l’option **Null** et tapez un numéro de commande, par exemple, *so71949*, puis cliquez sur **Afficher le rapport** pour afficher uniquement cette commande dans le rapport.  
    ![ssrs_tutorial_datadriven_reportviewer_parameter](../reporting-services/media/ssrs-tutorial-datadriven-reportviewer-parameter.png) 
 
  
## <a name="bkmk_redeploy"></a>Redéployer le rapport  
  
1.  Redéployez le rapport afin que la configuration de l'abonnement dans la leçon suivante puisse utiliser les modifications que vous avez apportées dans cette leçon. Pour plus d’informations sur les propriétés de projet utilisées dans le didacticiel de création d’une table, consultez la section « Pour publier le rapport sur le serveur de rapports (facultatif) » de la [Leçon 6 : ajout d’un regroupement et de totaux &#40;Reporting Services&#41;](../reporting-services/lesson-6-adding-grouping-and-totals-reporting-services.md).  
  
2.  Dans la barre d'outils, cliquez sur **Générer** , puis sur **Déployer le didacticiel**.  
  
## <a name="next-steps"></a>Next Steps  
+ Vous avez correctement configuré le rapport pour extraire les données au moyen des informations d’identification stockées et les données peuvent être filtrées avec un paramètre. 
+ Dans la leçon suivante, vous configurez l’abonnement à l’aide des pages Abonnement piloté par les données du portail web. Consultez la [Leçon 3 : Définition d’un abonnement piloté par les données](../reporting-services/lesson-3-defining-a-data-driven-subscription.md).  
  
## <a name="see-also"></a> Voir aussi  
[Gérer des sources de données de rapports](../reporting-services/report-data/manage-report-data-sources.md)  
[Spécifier des informations d'identification et de connexion pour les sources de données de rapports](../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)  
[Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md)  
[Créer un rapport de tableau de base &#40;Didacticiel SSRS&#41;](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)  
  
  
  

