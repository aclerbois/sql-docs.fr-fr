---
title: Générateur de rapports dans SQL Server 2016 | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
f1_keywords:
- "10428"
helpviewer_keywords:
- overview of Report Builder
- getting started
ms.assetid: 55bf4f9c-d037-412f-ae57-3fc39ce32fa5
caps.latest.revision: 35
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 7b0e65fda3c8aab492d45546428db43b8af7fc1a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-builder-in-sql-server-2016"></a>Générateur de rapports dans SQL Server 2016
  [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] est un outil de création de rapports paginés pour les utilisateurs professionnels qui préfèrent travailler dans un environnement autonome plutôt que d’utiliser le Concepteur de rapports dans Visual Studio.  Lorsque vous concevez un rapport paginé, vous créez une définition de rapport qui spécifie l’emplacement des données à obtenir, leur nature et leur mode d’affichage. Au moment de l’exécution du rapport, le processeur de rapports assimile la définition de rapport que vous avez spécifiée, puis il récupère les données et les combine à la mise en page du rapport pour générer le rapport. Vous pouvez afficher un aperçu de votre rapport dans [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] et le publier sur un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif ou intégré SharePoint, pour permettre à d’autres utilisateurs de l’exécuter.  
  
 ![rs_GettingStartedReport](../../reporting-services/report-builder/media/rs-gettingstartedreport.png "rs_GettingStartedReport")  
  
 Ce rapport paginé comprend une matrice avec des groupes de lignes et de colonnes, des graphiques sparkline, des indicateurs et un graphique à secteurs de synthèse dans la cellule d’angle, ainsi qu’une carte avec deux ensembles de données géographiques représentées par couleur et par taille de cercle.  
  
##  <a name="JumpStartReptCreation"></a> Accélérer la création de rapports  
  
-   **Commencez avec un jeu de données partagé**. Les jeux de données partagés sont des requêtes basées sur une source de données partagée, qui sont enregistrées sur un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif ou intégré SharePoint.  
  
-   **Démarrez avec l'Assistant Tableau, Matrice ou Graphique**. Sélectionnez une connexion à une source de données, effectuez un glisser-déplacer de champs pour créer une requête de dataset, sélectionnez une disposition et un style, puis personnalisez votre rapport.  
  
-   **Démarrez avec l'Assistant Carte** pour créer des rapports qui affichent des données agrégées sur un arrière-plan géographique ou géométrique. Les données cartographiques peuvent être des données spatiales provenant d'une requête [!INCLUDE[tsql](../../includes/tsql-md.md)] ou d'un fichier de forme ESRI (Environmental Systems Research Institute, Inc.). Vous pouvez également ajouter un arrière-plan de mosaïque [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing.  
  
-   **Commencez votre rapport avec des parties de rapport**. Les parties de rapports sont des éléments de rapport qui ont été publiés séparément sur un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif ou intégré SharePoint. Elles peuvent être réutilisées dans d’autres rapports. Les éléments de rapport tels que les tableaux, matrices, graphiques et images peuvent être publiés en tant que parties de rapports.  
  
##  <a name="DesignRept"></a> Concevoir votre rapport  
  
-   **Créez des rapports paginés à l’aide de mises en page de rapports de tableau, de matrice, de graphique et au format libre.** Créez des rapports de tableau pour des données en colonnes, des rapports de matrice (tels que des rapports d'analyse croisée ou de tableau croisé dynamique) pour des données synthétisées, des rapports de graphique pour des données graphiques et enfin, des rapports au format libre pour tout autre type de données. Les rapports peuvent incorporer d'autres rapports et graphiques, avec des listes, des graphiques et des contrôles pour les applications web dynamiques.  
  
-   **Créez des rapports à partir de diverses sources de données.** Générez des rapports à l’aide de données provenant de tout type de source de données ayant un fournisseur de données managées [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], un fournisseur OLE DB ou une source de données ODBC. Vous pouvez créer des rapports qui utilisent des données relationnelles et multidimensionnelles provenant de bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], Oracle, Hypérion, entre autres. Utilisez une extension pour le traitement des données XML et récupérez des données de n'importe quelle source XML. Vous pouvez utiliser des fonctions table pour concevoir des sources de données personnalisées.  
  
-   **Modifiez des rapports existants.** Grâce au [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)], vous pouvez personnaliser et mettre à jour des rapports qui ont été créés dans le [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]Concepteur de rapports.  
  
-   **Modifiez vos données** en filtrant, en regroupant et en triant ces dernières, ou en ajoutant des formules ou des expressions.  
  
-   **Ajoutez des graphiques, des jauges, des graphiques sparkline et des indicateurs** pour synthétiser les données dans un format visuel et présenter d'un coup d'œil de grandes quantités d'informations agrégées.  
  
-   **Ajoutez des fonctionnalités interactives** telles que des explorateurs de documents, des boutons d’affichage/de masquage et des liens d’extraction dans des sous-rapports et des rapports d’extraction. Utilisez des paramètres et des filtres pour filtrer les données pour des vues personnalisées.  
  
-   **Incorporez des images ou faites référence à ces dernières** et à d'autres ressources, y compris du contenu externe.  
  
##  <a name="ManageRpt"></a> Gérer votre rapport  
  
-   **Enregistrez la définition du rapport** sur votre ordinateur ou sur le serveur de rapports, d'où vous pourrez le gérer et le partager avec d'autres utilisateurs.  
  
-   **Choisissez un format de présentation** au moment de l'ouverture du rapport ou après son ouverture. Vous pouvez sélectionner un format adapté au Web, à l'impression ou à une application bureautique. Les formats disponibles sont les suivants : HTML, MHTML, PDF, XML, CSV, TIFF, Word et Excel.  
  
-   **Configurez des abonnements.** Après avoir publié le rapport sur le serveur de rapports ou un serveur de rapports en mode intégré SharePoint, vous pouvez configurer votre rapport pour une exécution à un moment spécifique, créer un historique de rapport et configurer des abonnements par messagerie électronique.  
  
-   **Générez des flux** à partir de votre rapport à l'aide de l'extension de rendu Atom Reporting Services.  
  
> [!NOTE]  
>  Les rapports publiés sont gérés sur un serveur de rapports ou un serveur de rapports en mode intégré SharePoint par un administrateur de serveur de rapports. Les administrateurs de serveur de rapports peuvent choisir le mode de sécurité, définir des propriétés et planifier des opérations, telles que l'historique de rapport et la remise des rapports par courrier électronique. Ils peuvent créer des planifications partagées et des sources de données partagées afin d'en permettre une utilisation générale. Les administrateurs peuvent également gérer tous les dossiers du serveur de rapports. La possibilité de réaliser des tâches de gestion dépend des autorisations délivrées aux utilisateurs.  
  
## <a name="see-also"></a> Voir aussi  
  [Démarrer le Générateur de rapports](../../reporting-services/report-builder/start-report-builder.md)  
  
  [Installer le générateur de rapports](../../reporting-services/install-windows/install-report-builder.md)

  [Nouveautés dans Reporting Services et le Générateur de rapports pour SQL Server 2016](~/reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)  
  Décrit les nouvelles fonctionnalités de cette version de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] et du [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)].   
  [Didacticiel : création d'un rapport de graphique rapide en mode hors connexion](../../reporting-services/report-builder/tutorial-create-a-quick-chart-report-offline-report-builder.md)  
 Présente le [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] et les Assistants disponibles pour vous permettre de créer des rapports. Il fournit un ensemble de données que vous pouvez utiliser pour vous éviter d'avoir à vous connecter à une source de données pour la mise en route.  
  
 [Planification d’un rapport &#40;Générateur de rapports&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md)  
 Fournit des informations sur les éléments à prendre en considération avant de commencer à créer un rapport.  
  
 [Concepts de création de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-authoring-concepts-report-builder-and-ssrs.md)  
 Définit les concepts clés utilisés dans la documentation du [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] .  
  
 [Mode Conception de rapport &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/report-design-view-report-builder.md)  
 Explique les différents volets et régions du mode création de rapport.  
  
 [Mode création de dataset partagé &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/shared-dataset-design-view-report-builder.md)  
 Explique les différents volets et régions du mode création de dataset partagé.  
  
 [Raccourcis clavier &#40;Générateur de rapports &#41;](../../reporting-services/report-builder/keyboard-shortcuts-report-builder.md)  
 Présente les touches de raccourci disponibles pour la navigation et la conception de rapports dans le [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)].  
  

