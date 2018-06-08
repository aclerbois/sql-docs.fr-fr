---
title: Vue de Source de définition de données | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d393ab0384e4866ec0a4f82f91d1b73aa88116d8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-1-3---defining-a-data-source-view"></a>Leçon 1-3-définition d’une vue de Source de données
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

L'étape qui suit la définition des sources de données que vous utiliserez dans un projet [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] consiste généralement à définir une vue de source de données pour le projet. Une vue de source de données est une vue unique et unifiée des métadonnées des tables et des vues spécifiées que la source de données définit dans le projet. Le stockage des métadonnées dans la vue de source de données permet d'utiliser ces métadonnées au cours de la phase de développement sans avoir besoin de disposer d'une connexion ouverte à une source de données sous-jacente. Pour plus d’informations, consultez [Vues de sources de données dans les modèles multidimensionnels](../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md).  
  
Au cours de la tâche suivante, vous définissez une vue de source de données qui contient cinq tables de la source de données **AdventureWorksDW2012** .  
  
### <a name="to-define-a-new-data-source-view"></a>Pour définir une nouvelle vue de source de données  
  
1.  Dans l’Explorateur de solutions (à droite de la fenêtre de Microsoft Visual Studio), cliquez avec le bouton droit sur **Vues des sources de données**, puis cliquez sur **Nouvelle vue de source de données**.  
  
2.  Dans la page **Assistant Vue de source de données** , cliquez sur **Suivant**. La page **Sélectionner une source de données** s’affiche.  
  
3.  Sous **Sources de données relationnelles**, la source de données **Adventure Works DW 2012** est sélectionnée. Cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Pour créer une vue de source de données basée sur plusieurs sources de données, vous devez d’abord définir une vue de source de données basée sur une seule source de données. Cette source de données est alors appelée la source de données principale. Vous pouvez ensuite ajouter des tables et des vues à partir d'une source de données secondaire. Lors de la conception des dimensions qui contiennent des attributs basés sur des tables associées dans plusieurs sources de données, vous devrez peut-être définir un [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] source de données en tant que source de données principale à utiliser ses fonctions de moteur de requête distribuée.  
  
4.  Dans la page **Sélectionner des tables et des vues** , sélectionnez des tables et des vues dans la liste des objets disponibles de la source de données sélectionnée. Vous pouvez filtrer cette liste pour sélectionner plus facilement les tables et les vues.  
  
    > [!NOTE]  
    > Cliquez sur le bouton agrandissement dans l'angle supérieur droit afin que la fenêtre couvre le plein écran. Vous pouvez alors consulter plus facilement la liste complète des objets disponibles.  
  
    Dans la liste **Objets disponibles** , sélectionnez les objets suivants. Vous pouvez sélectionner plusieurs tables en maintenant enfoncée la touche Ctrl :  
  
    -   **DimCustomer (dbo)**  
  
    -   **DimDate (dbo)**  
  
    -   **DimGeography (dbo)**  
  
    -   **DimProduct (dbo)**  
  
    -   **FactInternetSales (dbo)**  
  
5.  Cliquez sur **>** pour ajouter les tables sélectionnées à la liste **Objets inclus** .  
  
6.  Cliquez sur **Suivant.**  
  
7.  Dans le champ Nom, vérifiez que **Adventure Works DW 2012** s’affiche, puis cliquez sur **Terminer**.  
  
    La vue de source de données **Adventure Works DW 2012** apparaît dans le dossier **Vues des sources de données** dans l’Explorateur de solutions. Le contenu de la vue de source de données s'affiche également dans le Concepteur de vues de source de données dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. Ce Concepteur contient les éléments suivants :  
  
    -   Un volet **Diagramme** dans lequel les tables et leurs relations sont représentées graphiquement.  
  
    -   Un volet **Tables** dans lequel les tables et leurs éléments de schéma sont affichés dans une arborescence.  
  
    -   Un volet **Bibliothèque de diagrammes** dans lequel vous pouvez créer des sous-diagrammes pour afficher des sous-ensembles de la vue de source de données.  
  
    -   Une barre d'outils qui est spécifique au Concepteur de vues de source de données.  
  
8.  Cliquez sur le bouton [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] pour agrandir l’environnement de **développement** .  
  
9. Pour afficher à 50 % les tables dans le volet **Diagramme** , cliquez sur l’icône **Zoom** de la barre d’outils du Concepteur de vues de sources de données. Cela permet de masquer les détails des colonnes de chaque table.  
  
10. Pour masquer l’Explorateur de solutions, cliquez sur le bouton **Masquer automatiquement** , qui correspond à l’icône en forme de punaise sur la barre de titre. Pour afficher à nouveau l'Explorateur de solutions, positionnez votre pointeur sur l'onglet Explorateur de solutions le long du côté droit de l'environnement de développement. Pour afficher l’Explorateur de solutions, recliquez sur le bouton **Masquer automatiquement** .  
  
11. Si les fenêtres ne sont pas masquées par défaut, cliquez sur **Masquer automatiquement** dans la barre de titre des fenêtres Propriétés et Explorateur de solutions.  
  
    Vous pouvez maintenant consulter toutes les tables et leurs relations dans le volet **Diagramme** . Notez qu'il existe trois relations entre la table FactInternetSales et la table DimDate. Chaque vente est associée à trois dates : une date de commande, une date d'échéance et une date de livraison. Pour afficher les détails d’une relation, double-cliquez sur la flèche de la relation dans le volet **Diagramme** .  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Modification des noms de Table par défaut](../analysis-services/lesson-1-4-modifying-default-table-names.md)  
  
## <a name="see-also"></a>Voir aussi  
[Vues de sources de données dans les modèles multidimensionnels](../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md)  
  
  
  
