---
title: Projet de Scripts dans SQL Server Management Studio Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e8ee5d2958b6ba7f180472e4d91ce389159e0438
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-scripts-project-in-sql-server-management-studio"></a>Projet de script Analysis Services dans SQL Server Management Studio
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous pouvez créer un projet Scripts Analysis Server dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour regrouper des scripts liés destinés au développement, à la gestion et au contrôle de code source. Si aucune solution n’est chargée dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], la création d’un nouveau projet Scripts Analysis Server crée automatiquement une solution. Vous pouvez également ajouter le projet Scripts du serveur d'analyse à la solution existante ou le créer dans une nouvelle solution.  
  
 Vous pouvez utiliser les étapes de base suivantes pour créer un projet Scripts du serveur d'analyse dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  
  
1.  Dans le menu Fichier, pointez sur **Nouveau**, puis cliquez sur **Projet**.  
  
     Sélectionnez le modèle de projet **Scripts Analysis Server** et spécifiez un nom et un emplacement pour le nouveau projet.  
  
2.  Cliquez avec le bouton droit sur **Connexions** pour créer une connexion dans le dossier Connexions du projet Scripts Analysis Server dans l’Explorateur de solutions.  
  
     Ce dossier contient les chaînes de connexion aux instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , sur lesquelles les scripts contenus dans le projet Scripts du serveur d'analyse peuvent être exécutés. Vous pouvez posséder plusieurs connexions dans un projet Scripts du serveur d'analyse et vous pouvez choisir une connexion pour laquelle exécuter un script contenu dans le projet au moment de l'exécution.  
  
3.  Cliquez avec le bouton droit sur **Requêtes** pour créer des scripts MDX (Multidimensional Expressions), DMX (Data Mining Extensions) et XMLA (XML for Analysis) dans le dossier Scripts du projet Scripts Analysis Server, dans l’Explorateur de solutions.
  
4.  Cliquez avec le bouton droit sur le projet, pointez sur **Ajouter**, puis sélectionnez **Élément existant** pour ajouter différents fichiers, comme des fichiers texte contenant des notes sur le projet, dans le dossier **Divers** du projet Scripts Analysis Server dans l’Explorateur de solutions. Ces fichiers sont ignorés par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="file-types"></a>Types de fichiers  
 Une solution [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] peut contenir plusieurs types de fichiers, en fonction des projets que vous avez inclus dans la solution et des éléments que vous avez inclus dans chaque projet pour cette solution. Pour plus d’informations sur les types de fichiers pour les solutions dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], consultez [Fichiers gérant les solutions et les projets](http://msdn.microsoft.com/library/e19d2859-0b97-4727-ac27-c4c226d86b2f). En règle générale, les fichiers de chaque projet dans une solution [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sont stockés dans le dossier de la solution, dans un dossier distinct pour chaque projet.  
  
 Le dossier d'un projet Scripts du serveur d'analyse peut contenir les types de fichiers répertoriés dans le tableau ci-dessous.  
  
|Type de fichier| Description|  
|---------------|-----------------|  
|Fichier de définition de projet Scripts du serveur d'analyse (.ssmsasproj)|Contient des métadonnées sur les dossiers affichés dans l'Explorateur de solutions, ainsi que des informations qui indiquent quels dossiers doivent afficher les fichiers inclus dans le projet.<br /><br /> Le fichier de définition de projet contient également les métadonnées pour les connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contenues dans le projet, ainsi que les métadonnées qui associent les connexions aux fichiers de script inclus dans le projet.|  
|Fichier de script DMX (.dmx)|Contient un script DMX inclus dans le projet.|  
|Fichier de script MDX (.mdx)|Contient un script MDX inclus dans le projet.|  
|Fichier de script XMLA (.xmla)|Contient un script XMLA inclus dans le projet.|  
  
## <a name="analysis-services-templates"></a>Modèles Analysis Services  
 Si vous ajoutez des scripts MDX, DMX ou XMLA à un projet Scripts Analysis Server, vous pouvez utiliser l’Explorateur de modèles pour rechercher les modèles [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , qui sont constitués d’un ensemble de scripts ou d’instructions prédéfinis qui montrent comment effectuer une action spécifique. L’Explorateur de modèles est accessible à partir du menu **Affichage** et intègre des modèles pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]et [!INCLUDE[ssEW](../../includes/ssew-md.md)]. Pour plus d’informations, consultez [Utiliser des modèles Analysis Services dans SQL Server Management Studio](../../analysis-services/instances/use-analysis-services-templates-in-sql-server-management-studio.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Création de modèles multidimensionnels à l’aide des Outils de données SQL Server &#40;SSDT&#41;](../../analysis-services/multidimensional-models/creating-multidimensional-models-using-sql-server-data-tools-ssdt.md)   
 [Expressions multidimensionnelles & #40 ; MDX & #41 ; Référence](../../mdx/multidimensional-expressions-mdx-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../../dmx/data-mining-extensions-dmx-reference.md)   
 [Langage de script Analysis Services &#40;ASSL pour XMLA&#41;](../../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md)   
 [Analysis Services Scripting Language &#40;ASSL de XMLA&#41;](../../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md)  
  
  
