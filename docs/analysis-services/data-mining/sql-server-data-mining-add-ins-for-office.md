---
title: SQL Server Data Mining Add-Ins for Office | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2220bb48704fb29aa00236ebf1ec4ad46ecb4007
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="sql-server-data-mining-add-ins-for-office"></a>Compléments d'exploration de données SQL Server pour Office

  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Compléments d'exploration de données est un ensemble d'outils légers pour l'analyse prédictive qui vous permet d'utiliser les données dans Excel pour générer des modèles d'analyse pour la prédiction, les recommandations ou l'exploration.  
  
> [!IMPORTANT]
> Le complément d’exploration de données pour Office n'est pas pris en charge dans Office 2016 ou version ultérieure.
  
 Les Assistants et les outils de gestion de données des compléments fournissent des instructions pas à pas pour les tâches d'exploration de données communes suivantes :  
  
-   **Organiser et nettoyer vos données avant la modélisation.** Utiliser des données stockées dans Excel ou dans une source de données Excel. Créez et enregistrez les connexions de façon à réutiliser les sources de données, répéter les expériences ou réapprendre les modèles.  
  
-   **Profiler, échantillonner et préparer.** Nombre de mineurs d'exploration de données expérimentés déclarent que 70 à 90 % du temps d'un projet d'exploration de données est consacré à la préparation des données. Les compléments permettent d'accélérer cette tâche. De fait, ils fournissent des visualisations dans Excel et des Assistants qui vous aident à exécuter les tâches courantes suivantes :  
  
    -   Profiler les données et comprendre leurs distribution et caractéristiques.  
  
    -   Créer des jeux d'apprentissage et de test via un échantillonnage aléatoire ou un suréchantillonnage.  
  
    -   Rechercher et supprimer ou remplacer les valeurs hors norme.  
  
    -   Réétiqueter les données pour améliorer la qualité d'analyse.  
  
-   **Analyser des modèles au moyen d'un apprentissage supervisé ou non.** Cliquez dans les Assistants conviviaux pour effectuer certaines tâches les plus fréquentes d'exploration de données, y compris l'analyse de clustering, l'analyse du panier d'achat et des prédictions.  
  
     Parmi les algorithmes d'apprentissage automatique connus inclus dans les compléments figurent Naïve Bayes, régression logistique, clustering, série chronologique et réseaux neuronaux.  
  
     Si vous découvrez l'exploration de données, obtenez de l'aide pour créer des requêtes de prédiction dans l'Assistant **Requête** .  
  
     Les utilisateurs expérimentés peuvent créer des requêtes personnalisées DMX à l’aide de l’ **Éditeur de requêtes avancé**par glisser-déplacer, ou automatiser les prédictions à l’aide d’Excel VBA.  
  
-   **Documenter et gérer.** Après avoir créé un jeu de données et généré des modèles, documentez votre travail et vos analyses en générant un résumé statistique des paramètres de données et du modèle.  
  
-   **Explorer et visualiser.** L'exploration de données n'est pas une activité qui peut être entièrement automatisée. Vous devez explorer et comprendre les résultats pour prendre des mesures explicites. Les compléments facilitent l'exploration en fournissant des visionneuses interactives dans Excel, des modèles Visio qui vous laissent personnaliser les diagrammes de modèle, et la possibilité d'exporter des graphiques et des tables pour Excel à des fins de filtrage ou de modification supplémentaire.  
  
-   **Déployer et intégrer.** Lorsque vous avez créé un modèle utile, mettez votre modèle en production, en utilisant les outils de gestion pour exporter le modèle de votre serveur expérimental vers une autre instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
     Vous pouvez également laisser le modèle sur le serveur où vous l'avez créé, mais actualisez les données d'apprentissage et exécutez des prédictions à l'aide d'Integration Services ou de scripts DMX.  
  
     Les utilisateurs avancés apprécieront la fonctionnalité de **trace** , qui permet d'afficher les instructions XMLA et DMX envoyées au serveur.  
  
## <a name="getting-started"></a>Mise en route  
 Pour plus d’informations, consultez [Éléments inclus dans les compléments d’exploration de données pour Office](http://go.microsoft.com/fwlink/p/?LinkId=616849).  
  
## <a name="support-and-requirements"></a>Prise en charge et configuration requise  
 Les compléments d'exploration de données SQL Server pour Office peuvent être téléchargés gratuitement. Vous devez disposer de l'une des versions suivantes d'Office déjà installée pour utiliser ces outils :  
  
-   Office 2010, version 32 bits ou 64 bits  
  
-   Office 2013, 32 bits ou 64 bits  
  
> [!WARNING]  
>  Veillez à installer la version des compléments qui correspond à votre version d'Excel.  
  
 Les compléments d'exploration de données nécessitent une connexion à l'une des versions suivantes de SQL Server Analysis Services :  
  
-   Enterprise  
  
-   Business Intelligence  
  
-   Standard  
  
 Selon l'édition de SQL Server Analysis Services à laquelle vous vous connectez, certains algorithmes avancés peuvent ne pas être disponibles. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).  
  
 Pour plus d’informations d’installation, consultez cette page du centre de téléchargement : [http://www.microsoft.com/download/details.aspx?id=29061](http://www.microsoft.com/download/details.aspx?id=29061)  
  
  
