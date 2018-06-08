---
title: Charger des données SQL Server dans Azure SQL Data Warehouse (SSIS) | Microsoft Docs
description: Indique comment créer un package SQL Server Integration Services (SSIS) pour déplacer des données vers SQL Data Warehouse à partir d’un large éventail de sources de données.
services: sql-data-warehouse
documentationcenter: NA
author: douglaslMS
manager: craigg-msft
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.custom: loading
ms.date: 04/04/2018
ms.author: douglasl
ms.openlocfilehash: e627fdad03bf3159a0ed9c730381fde53c86ee9f
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="load-data-from-sql-server-to-azure-sql-data-warehouse-with-sql-server-integration-services-ssis"></a>Charger des données SQL Server dans Azure SQL Data Warehouse à l’aide de SQL Server Integration Services (SSIS)

Créez un package SQL Server Integration Services (SSIS) pour charger des données SQL Server dans [Azure SQL Data Warehouse](/azure/sql-data-warehouse/index.md). Vous pouvez éventuellement restructurer, transformer et nettoyer les données qui traversent le flux de données SSIS.

Dans ce didacticiel, vous apprenez à :

* Créer un projet Integration Services dans Visual Studio.
* Se connecter aux sources de données, y compris SQL Server (en tant que source) et SQL Data Warehouse (en tant que destination).
* Concevoir un package SSIS qui charge des données de la source vers la destination.
* Exécuter le package SSIS pour charger les données.

Ce didacticiel utilise SQL Server comme source de données. SQL Server peut s’exécuter localement ou sur une machine virtuelle Azure.

## <a name="basic-concepts"></a>Concepts de base
Le package est l'unité de travail dans SSIS. Les packages associés sont regroupés en projets. Vous créez des projets et concevez des packages dans Visual Studio avec SQL Server Data Tools. Le processus de conception est un processus visuel au cours duquel vous faites glisser et déposez des composants issus de la boîte à outils dans l’aire de conception, les connectez et définissez leurs propriétés. Une fois que vous avez terminé votre package, vous pouvez éventuellement le déployer dans SQL Server pour le gérer, le surveiller et le sécuriser complètement.

## <a name="options-for-loading-data-with-ssis"></a>Options de chargement des données avec SSIS
SQL Server Integration Services (SSIS) est un ensemble d’outils flexible qui fournit diverses options pour la connexion et le chargement de données dans SQL Data Warehouse.

1. Utilisez une destination ADO NET pour vous connecter à SQL Data Warehouse. Ce didacticiel utilise une destination ADO NET, car elle présente le moins d’options de configuration.
2. Utilisez une destination OLE DB pour vous connecter à SQL Data Warehouse. Cette option peut fournir des performances légèrement meilleures que la destination ADO NET.
3. Utilisez la tâche de chargement d'objet blob Azure pour effectuer une copie intermédiaire des données dans Stockage Blob Azure. Ensuite, utilisez la tâche SSIS d'exécution de requêtes SQL pour lancer un script Polybase qui charge les données dans SQL Data Warehouse. Cette option offre les meilleures performances parmi les trois options répertoriées ici. Pour obtenir la tâche de chargement d’objets blob Azure, téléchargez le [Feature Pack Microsoft SQL Server 2016 Integration Services pour Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]. Pour en savoir plus sur Polybase, consultez [Guide de PolyBase][PolyBase Guide].

## <a name="before-you-start"></a>Avant de commencer
Pour exécuter pas à pas ce didacticiel, vous avez besoin des éléments suivants :

1. **SQL Server Integration Services (SSIS)**. SSIS est un composant de SQL Server et requiert une version d’évaluation ou une version sous licence de SQL Server. Pour obtenir une version d’évaluation de SQL Server 2016 Preview, consultez [Évaluations SQL Server][SQL Server Evaluations].
2. **Visual Studio**. Pour obtenir l’édition gratuite Visual Studio Community Edition, consultez [Visual Studio Community][Visual Studio Community].
3. **SQL Server Data Tools pour Visual Studio (SSDT)**. Pour obtenir SQL Server Data Tools pour Visual Studio, consultez [Télécharger SSDT (SQL Server Data Tools)][Download SQL Server Data Tools (SSDT)].
4. **Exemples de données**. Ce didacticiel utilise des exemples de données stockées dans SQL Server, dans la base de données AdventureWorks, en tant que données sources à charger dans SQL Data Warehouse. Pour obtenir l’exemple de base de données AdventureWorks, consultez [Exemples de bases de données AdventureWorks 2014][AdventureWorks 2014 Sample Databases].
5. **Base de données SQL Data Warehouse et autorisations**. Dans ce didacticiel, vous vous connectez à une instance SQL Data Warehouse et y chargez des données. Vous devez disposer des autorisations pour créer une table et charger des données.
6. **Règle de pare-feu**. Vous devez créer une règle de pare-feu sur SQL Data Warehouse avec l’adresse IP de votre ordinateur local afin de pouvoir charger des données dans SQL Data Warehouse.

## <a name="step-1-create-a-new-integration-services-project"></a>Étape 1 : Créer un projet Integration Services
1. Lancez Visual Studio.
2. Dans le menu **Fichier**, sélectionnez **Nouveau | Projet**.
3. Accédez aux types de projet **Installé | Modèles | Business Intelligence | Integration Services**.
4. Sélectionnez **Projet Integration Services**. Fournissez des valeurs pour **Nom** et **Emplacement**, puis sélectionnez **OK**.

Visual Studio s’ouvre et crée un nouveau projet Integration Services (SSIS). Ensuite, Visual Studio ouvre le concepteur pour le nouveau package SSIS individuel (Package.dtsx) dans le projet. Vous voyez les zones d’écran suivantes :

* À gauche, la **boîte à outils** des composants SSIS.
* Au milieu, l’aire de conception avec plusieurs onglets. Vous utilisez généralement au moins les onglets **Flux de contrôle** et **Flux de données**.
* À droite, les volets **Explorateur de solutions** et **Propriétés**.
  
    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Étape 2 : Créer le flux de données de base
1. Faites glisser une tâche de flux de données de la boîte à outils jusqu’au centre de l’aire de conception (dans l’onglet **Flux de contrôle**).
   
    ![][02]
2. Double-cliquez sur la tâche de flux de données pour basculer vers l'onglet Flux de données.
3. Dans la liste Autres sources, dans la boîte à outils, faites glisser une source ADO.NET jusqu’à l’aire de conception. Lorsque l’adaptateur de source est encore sélectionné, remplacez son nom par **Source SQL Server** dans le volet **Propriétés**.
4. Dans la liste Autres destinations, dans la boîte à outils, faites glisser une destination ADO.NET jusqu’à l’aire de conception sous la source ADO.NET. Lorsque l’adaptateur de destination est encore sélectionné, remplacez son nom par **Destination SQL DW** dans le volet **Propriétés**.
   
    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Étape 3 : Configurer l’adaptateur de source
1. Double-cliquez sur l’adaptateur de source pour ouvrir l’**Éditeur de source ADO.NET**.
   
    ![][03]
2. Dans l’onglet **Gestionnaire de connexions** de l’**Éditeur de source ADO.NET**, cliquez sur le bouton **Nouveau** situé en regard de la liste **Gestionnaire de connexions ADO.NET** pour afficher la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET** et créer des paramètres de connexion pour la base de données SQL Server à partir de laquelle ce didacticiel charge les données.
   
    ![][04]
3. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur le bouton **Nouveau** pour ouvrir la boîte de dialogue **Gestionnaire de connexions** et créer une nouvelle connexion de données.
   
    ![][05]
4. Dans la boîte de dialogue **Gestionnaire de connexions**, effectuez les actions suivantes.
   
   1. Pour **Fournisseur**, sélectionnez le fournisseur de données SqlClient.
   2. Pour **Nom du serveur**, entrez le nom du serveur SQL Server.
   3. Dans la section **Connexion au serveur**, sélectionnez ou entrez les informations d’authentification.
   4. Dans la section **Se connecter à une base de données**, sélectionnez l’exemple de base de données AdventureWorks.
   5. Cliquez sur **Tester la connexion**.
      
       ![][06]
   6. Dans la boîte de dialogue indiquant les résultats du test de connexion, cliquez sur **OK** pour revenir à la boîte de dialogue **Gestionnaire de connexions**.
   7. Dans la boîte de dialogue **Gestionnaire de connexions**, cliquez sur **OK** pour revenir à la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**.
5. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur **OK** pour revenir à l’**Éditeur de source ADO.NET**.
6. Dans l’**Éditeur de source ADO.NET**, dans la liste **Nom de la table ou de la vue**, sélectionnez la table **Sales.SalesOrderDetail**.
   
    ![][07]
7. Cliquez sur **Aperçu** pour afficher les 200 premières lignes de données dans la table source de la boîte de dialogue **Visualiser les résultats de la requête**.
   
    ![][08]
8. Dans la boîte de dialogue **Visualiser les résultats de la requête**, cliquez sur **Fermer** pour revenir à l’**Éditeur de source ADO.NET**.
9. Dans l’**Éditeur de source ADO.NET**, cliquez sur **OK** pour terminer la configuration de la source de données.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Étape 4 : Connecter l’adaptateur de source à l’adaptateur de destination
1. Sélectionnez l’adaptateur de source dans l’aire de conception.
2. Sélectionnez la flèche bleue qui s’étend de l’adaptateur de source et faites-la glisser vers l’éditeur de destination jusqu'à ce qu’il s’enclenche.
   
    ![][10]
   
    Dans un package SSIS standard, vous utilisez plusieurs autres composants de la boîte à outils SSIS entre la source et la destination pour restructurer, transformer et nettoyer vos données lorsqu’elles traversent le flux de données SSIS. Pour que cet exemple reste aussi simple que possible, nous connectons directement la source à la destination.

## <a name="step-5-configure-the-destination-adapter"></a>Étape 5 : Configurer l’adaptateur de destination
1. Double-cliquez sur l’adaptateur de destination pour ouvrir l’**Éditeur de destination ADO.NET**.
   
    ![][11]
2. Dans l’onglet **Gestionnaire de connexions** de l’**Éditeur de destination ADO.NET**, cliquez sur le bouton **Nouveau** situé en regard de la liste **Gestionnaire de connexions** pour afficher la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET** et créer des paramètres de connexion pour la base de données Azure SQL Data Warehouse dans laquelle ce didacticiel charge des données.
3. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur le bouton **Nouveau** pour ouvrir la boîte de dialogue **Gestionnaire de connexions** et créer une nouvelle connexion de données.
4. Dans la boîte de dialogue **Gestionnaire de connexions**, effectuez les actions suivantes.
   1. Pour **Fournisseur**, sélectionnez le fournisseur de données SqlClient.
   2. Pour **Nom du serveur**, entrez le nom de l’entrepôt de données SQL Data Warehouse.
   3. Dans la section **Connexion au serveur**, sélectionnez **Utiliser l'authentification SQL Server** et entrez les informations d’authentification.
   4. Dans la section **Se connecter à une base de données**, sélectionnez une base de données SQL Data Warehouse existante.
   5. Cliquez sur **Tester la connexion**.
   6. Dans la boîte de dialogue indiquant les résultats du test de connexion, cliquez sur **OK** pour revenir à la boîte de dialogue **Gestionnaire de connexions**.
   7. Dans la boîte de dialogue **Gestionnaire de connexions**, cliquez sur **OK** pour revenir à la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**.
5. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur **OK** pour revenir à l’**Éditeur de destination ADO.NET**.
6. Dans l’**Éditeur de destination ADO.NET**, cliquez sur **Nouveau** en regard de la liste **Utiliser une table ou une vue** pour ouvrir la boîte de dialogue **Créer une table** afin de créer une nouvelle table de destination avec une liste de colonnes qui correspond à la table source.
   
    ![][12a]
7. Dans la boîte de dialogue **Créer une table**, effectuez les actions suivantes.
   
   1. Remplacez le nom de la table de destination par **SalesOrderDetail**.
   2. Supprimez la colonne **rowguid**. Le type de données **uniqueidentifier** n’est pas pris en charge dans SQL Data Warehouse.
   3. Changez le type de données de la colonne **LineTotal** en spécifiant **money**. Le type de données **decimal** n’est pas pris en charge dans SQL Data Warehouse. Pour obtenir des informations sur les types de données pris en charge, consultez [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].
      
       ![][12b]
   4. Cliquez sur **OK** pour créer la table et revenir à l’**Éditeur de destination ADO.NET**.
8. Dans l’**Éditeur de destination ADO.NET**, sélectionnez l’onglet **Mappages** pour voir comment les colonnes de la source sont mappées aux colonnes de la destination.
   
    ![][13]
9. Cliquez sur **OK** pour terminer la configuration de la source de données.

## <a name="step-6-run-the-package-to-load-the-data"></a>Étape 6 : Exécuter le package pour charger les données
Exécutez le package en cliquant sur le bouton **Démarrer** dans la barre d’outils ou en sélectionnant l’une des options **Exécuter** dans le menu **Débogage**.

Lorsque le package commence à s’exécuter, des roues dentées jaunes tournent pour indiquer l’activité et vous voyez le nombre de lignes traitées jusque là.

![][14]

Quand le package a fini de s’exécuter, vous voyez des coches vertes qui indiquent la réussite de l’opération, ainsi que le nombre total de lignes de données chargées de la source vers la destination.

![][15]

Félicitations ! Vous avez utilisé avec succès SQL Server Integration Services pour charger des données dans Azure SQL Data Warehouse.

## <a name="next-steps"></a>Étapes suivantes
* Découvrez plus en détail le flux de données SSIS. Commencez ici : [Flux de données][Data Flow].
* Découvrez comment déboguer et dépanner vos packages directement dans l’environnement de conception. Commencez ici : [Outils de dépannage pour le développement des packages][Troubleshooting Tools for Package Development].
* Découvrez comment déployer vos projets SSIS et vos packages sur le serveur Integration Services ou dans un autre emplacement de stockage. Commencez ici : [Déploiement de projets et de packages][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/load-data-to-sql-data-warehouse/ssis-designer-01.png
[02]:  ./media/load-data-to-sql-data-warehouse/ssis-data-flow-task-02.png
[03]:  ./media/load-data-to-sql-data-warehouse/ado-net-source-03.png
[04]:  ./media/load-data-to-sql-data-warehouse/ado-net-connection-manager-04.png
[05]:  ./media/load-data-to-sql-data-warehouse/ado-net-connection-05.png
[06]:  ./media/load-data-to-sql-data-warehouse/test-connection-06.png
[07]:  ./media/load-data-to-sql-data-warehouse/ado-net-source-07.png
[08]:  ./media/load-data-to-sql-data-warehouse/preview-data-08.png
[09]:  ./media/load-data-to-sql-data-warehouse/source-destination-09.png
[10]:  ./media/load-data-to-sql-data-warehouse/connect-source-destination-10.png
[11]:  ./media/load-data-to-sql-data-warehouse/ado-net-destination-11.png
[12a]: ./media/load-data-to-sql-data-warehouse/destination-query-before-12a.png
[12b]: ./media/load-data-to-sql-data-warehouse/destination-query-after-12b.png
[13]:  ./media/load-data-to-sql-data-warehouse/column-mapping-13.png
[14]:  ./media/load-data-to-sql-data-warehouse/package-running-14.png
[15]:  ./media/load-data-to-sql-data-warehouse/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: ../relational-databases/polybase/polybase-guide.md
[Download SQL Server Data Tools (SSDT)]: ../ssdt/download-sql-server-data-tools-ssdt.md
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: ../t-sql/statements/create-table-azure-sql-data-warehouse.md
[Data Flow]: ./data-flow/data-flow.md
[Troubleshooting Tools for Package Development]: ./troubleshooting/troubleshooting-tools-for-package-development.md
[Deployment of Projects and Packages]: ./packages/deploy-integration-services-ssis-projects-and-packages.md

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
