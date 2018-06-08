---
title: À l’aide d’une Version modifiée de l’analyse des Services de projet du didacticiel | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 93847b7e6cade7d77774603ba1852c16a5a783b7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-4-1---using-a-modified-version-of-the-analysis-services-tutorial-project"></a>Leçon 4-1-à l’aide d’une Version modifiée du projet didacticiel Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

Les sept dernières leçons de ce didacticiel sont basées sur une version évoluée du projet du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] que vous avez créé au cours des trois premières leçons. D’autres tables et calculs nommés ont été ajoutés à la vue de source de données **Adventure Works DW 2012** , des dimensions supplémentaires ont été ajoutées au projet et ces nouvelles dimensions ajoutées au cube [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] Tutorial. De plus, un deuxième groupe de mesures a été ajouté, qui contient des mesures d'une deuxième table de faits. Ce projet évolué va vous permettre d'apprendre à enrichir en fonctionnalités votre application Business Intelligence sans avoir à répéter ce que vous avez déjà appris.  
  
Avant de poursuivre votre apprentissage dans le didacticiel, vous devez télécharger, extraire, charger et traiter la version améliorée du projet du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] .  Suivez les instructions de cette leçon pour vous assurer d'effectuer toutes les étapes.  
  
## <a name="downloading-and-extracting-the-project-file"></a>Téléchargement et extraction du fichier projet  
  
1.  [Cliquez ici](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks-analysis-services) pour accéder à la page de téléchargement qui fournit les exemples de projets de ce didacticiel. Les projets de didacticiel sont inclus dans le **adventure-works-multidimensionnel-didacticiel-projects.zip** télécharger.  
  
2.  Cliquez sur **adventure-works-multidimensionnel-didacticiel-projects.zip** pour télécharger le package qui contient les projets de ce didacticiel.  
  
    Par défaut, le fichier .zip est enregistré dans le dossier Téléchargements. Vous devez déplacer le fichier .zip vers un emplacement dont le chemin d'accès est plus court (par exemple, créez un dossier C:\Tutorials pour stocker les fichiers).  Vous pouvez ensuite extraire les fichiers contenus dans le fichier .zip. Si vous essayez de décompresser les fichiers dans le dossier Téléchargements, dont le chemin d'accès est long, vous n'obtiendrez que la leçon 1.  
  
3.  Créez un sous-dossier au niveau ou proche du lecteur racine, par exemple, C:\Tutorial.  
  
4.  Déplacer le **adventure-works-multidimensionnel-didacticiel-projects.zip** fichier dans le sous-dossier.  
  
5.  Cliquez avec le bouton droit sur le fichier et sélectionnez **Extraire tout**.  
  
6.  Accédez au dossier **Début de la leçon 4** et recherchez le fichier **Analysis Services Tutorial.sln** .  
  
## <a name="loading-and-processing-the-enhanced-project"></a>Chargement et traitement du projet amélioré  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], dans le menu **Fichier** , cliquez sur **Fermer la solution** pour fermer les fichiers que vous n’utiliserez pas.  
  
2.  Dans le menu **Fichier**, pointez sur **Ouvrir**, puis cliquez sur **Projet/Solution**.  
  
3.  Accédez à l'emplacement dans lequel vous avez extrait les fichiers de projet du didacticiel.  
  
    Recherchez le dossier nommé **Début de la leçon 4**, puis double-cliquez sur Analysis Services Tutorial.sln.  
  
4.  Déployez la version améliorée du projet du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] sur l'instance locale de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]ou sur une autre instance et vérifiez si le traitement s'est terminé sans problème.  
  
## <a name="understanding-the-enhancements-to-the-project"></a>Améliorations apportées au projet  
La version améliorée du projet est différente de la version du projet du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] que vous avez utilisée au cours des trois premières leçons. Les différences entre ces versions sont décrites dans les sections qui suivent. Consultez ces informations avant de poursuivre les leçons restantes du didacticiel.  
  
### <a name="data-source-view"></a>Vue de source de données  
Dans le projet amélioré, la vue de source de données contient une table de faits supplémentaire et quatre tables de dimension supplémentaires dans la base de données [!INCLUDE[ssSampleDBDWobject](../includes/sssampledbdwobject-md.md)] .  
  
Remarquez qu’avec dix tables dans la vue de source de données, le diagramme <All Tables> est presque plein. Il devient dès lors difficile de comprendre les relations entre les tables et de localiser des tables spécifiques. Pour résoudre ce problème, les tables sont organisées en deux diagrammes logiques, le diagramme **Internet Sales** et le diagramme **Reseller Sales** . Ces diagrammes sont tous deux organisés autour d'une seule table de faits. La création de diagrammes logiques permet d'afficher et d'utiliser uniquement des sous-ensembles spécifiques des tables dans la vue de source de données au lieu de toujours afficher la totalité des tables et de leurs relations dans un même diagramme.  
  
#### <a name="internet-sales-diagram"></a>Diagramme Internet Sales  
Le diagramme **Internet Sales** contient les tables qui sont liées à la vente des produits [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] effectuée directement auprès des clients par le biais d’Internet. Les tables que ce diagramme contient sont les quatre tables de dimension et la table de faits que vous avez ajoutées à la vue de source de données **Adventure Works DW 2012** de la leçon 1. Il s’agit des tables suivantes :  
  
-   **Geography**  
  
-   **Customer**  
  
-   **Date**  
  
-   **Product**  
  
-   **InternetSales**  
  
#### <a name="reseller-sales-diagram"></a>Diagramme Reseller Sales  
Le diagramme **Reseller Sales** contient les tables qui sont liées à la vente des produits [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] par le biais de revendeurs. Ce diagramme contient les sept tables de dimension et la table de faits suivantes de la base de données [!INCLUDE[ssSampleDBDWobject](../includes/sssampledbdwobject-md.md)] :  
  
-   **Reseller**  
  
-   **Promotion**  
  
-   **SalesTerritory**  
  
-   **Geography**  
  
-   **Date**  
  
-   **Product**  
  
-   **Employee**  
  
-   **ResellerSales**  
  
Notez que les tables **DimGeography**, **DimDate**et **DimProduct** sont utilisées à la fois dans le diagramme **Internet Sales** et le diagramme **Reseller Sales** . Les tables de dimension peuvent être liées à plusieurs tables de faits.  
  
### <a name="database-and-cube-dimensions"></a>Dimensions de base de données et de cube  
Le projet du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] contient cinq nouvelles dimensions de base de données et le cube [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] Tutorial contient ces cinq mêmes dimensions mais en tant que dimensions de cube. Ces dimensions ont été définies pour avoir des hiérarchies utilisateur et des attributs modifiés par des calculs nommés, des clés de membre de composition et des dossiers d'affichage. Ces nouvelles dimensions sont décrites ci-après.  
  
Dimension Reseller  
La dimension Reseller est basée sur la table **Reseller** de la vue de source de données **Adventure Works DW 2012** .  
  
Dimension Promotion  
La dimension Promotion est basée sur la table **Promotion** de la vue de source de données **Adventure Works DW 2012** .  
  
Dimension Sales Territory  
La dimension Sales Territory est basée sur la table **SalesTerritory** de la vue de source de données **Adventure Works DW 2012** .  
  
Dimension Employee  
La dimension Employee est basée sur la table **Employee** de la vue de source de données **Adventure Works DW 2012** .  
  
Dimension Geography  
La dimension Geography est basée sur la table **Geography** de la vue de source de données **Adventure Works DW 2012** .  
  
#### <a name="analysis-services-cube"></a>Cube Analysis Services  
Le cube **Analysis Services Tutorial** contient maintenant deux groupes de mesures, le groupe de mesure d’origine, basé sur la table **InternetSales** , et un deuxième groupe de mesures basé sur la table **ResellerSales** dans la vue de source de données **Adventure Works DW 2012** .  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Définition des propriétés d'attribut parent dans une hiérarchie parent-enfant](../analysis-services/lesson-4-2-defining-parent-attribute-properties-in-a-parent-child-hierarchy.md)  
  
## <a name="see-also"></a>Voir aussi  
[Déploiement d’un projet Analysis Services](../analysis-services/lesson-2-5-deploying-an-analysis-services-project.md)  
  
