---
title: Déploiement de Solutions d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b42d01b097483d9088bd76257cd30ac37158f889
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deployment-of-data-mining-solutions"></a>Déploiement de solutions d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La dernière étape du processus d'exploration de données consiste à déployer les modèles dans un environnement de production. Le déploiement est important car il met les modèles à la disposition des utilisateurs afin de vous permettre d'effectuer n'importe laquelle des tâches suivantes :  
  
-   Utiliser les modèles pour créer des prédictions et prendre des décisions d'entreprise. Pour plus d’informations sur les outils que vous pouvez utiliser pour créer des requêtes, consultez [Outils de requête d’exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md).  
  
-   Incorporer la fonctionnalité d'exploration de données directement dans une application. Vous pouvez inclure des objets AMO (Analysis Management Objects) ou un assembly contenant un ensemble d'objets qui peuvent être utilisés par votre application pour créer, modifier, traiter et supprimer des structures d'exploration de données et des modèles d'exploration de données.  
  
-   Créer des rapports qui permettent aux utilisateurs de demander des prédictions, d'afficher des tendances ou de comparer des modèles.  
  
 Cette section fournit des informations détaillées sur les options de déploiement.  
  
 [Conditions requises pour le déploiement de solutions d'exploration de données](#bkmk_Reqs)  
  
 [Déploiement d'une solution relationnelle](#bkmk_RelationalSltn)  
  
 [Déploiement d'une solution multidimensionnelle](#bkmk_MDSltn)  
  
 [Ressources connexes](#bkmk_Resources)  
  
## <a name="in-this-section"></a>Dans cette section  
 [Déployer une Solution d’exploration de données pour les Versions précédentes de SQL Server](../../analysis-services/data-mining/deploy-a-data-mining-solution-to-previous-versions-of-sql-server.md)  
  
 [Exporter et importer des objets d’exploration de données](../../analysis-services/data-mining/export-and-import-data-mining-objects.md)  
  
##  <a name="bkmk_Reqs"></a> Conditions requises pour le déploiement de solutions d'exploration de données  
 L’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans laquelle vous déployez la solution doit s’exécuter dans un mode qui prend en charge les objets multidimensionnels et les objets d’exploration de données ; autrement dit, vous ne pouvez pas déployer des objets d’exploration de données dans une instance qui héberge des modèles tabulaires ou des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Par conséquent, lorsque vous créez une solution d'exploration de données dans Visual Studio, veillez à utiliser le modèle **Projet multidimensionnel et d'exploration de données Analysis Services**.  
  
 Lorsque vous déployez la solution, les objets utilisés pour l'exploration de données sont créés dans l'instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] spécifiée, dans une base de données portant le même nom que le fichier solution.  
  
###  <a name="bkmk_RelationalSltn"></a> Déploiement d'une solution relationnelle  
 Lorsque vous déployez une solution d'exploration de données relationnelle, les objets d'exploration de données requis sont créés dans une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , et les objets sont traités par défaut. Vous pouvez modifier les options de traitement à l'aide de la propriété de configuration **Option de traitement**. Pour plus d’informations, consultez [Configurer les propriétés d’un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/configure-analysis-services-project-properties-ssdt.md).  
  
 Par défaut, seules les modifications incrémentielles sont déployées à chaque fois. En d'autres termes, vous pouvez modifier un modèle d'exploration de données, et lorsque vous redéployez le projet, seul ce modèle d'exploration de données est mis à jour. Toutefois, si vous avez plusieurs clients qui modifient la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , cela peut générer des erreurs. Pour modifier le mode de déploiement par défaut afin que la totalité de la base de données soit actualisée lorsque vous déployez la solution, modifiez la propriété **Mode de déploiement** .  
  
 Dans une solution d'exploration de données relationnelles, les seuls objets qui doivent être déployés sont la définition des sources de données, toute vue de sources de données, les structures d'exploration de données et tous les modèles d'exploration de données dépendants.  
  
###  <a name="bkmk_MDSltn"></a> Déploiement d'une solution multidimensionnelle  
 Lorsque vous déployez une solution d'exploration de données multidimensionnelle, cette solution crée vos objets d'exploration de données dans la même base de données que le cube source.  
  
 Lorsque vous traitez la structure d'exploration de données ou le modèle d'exploration de données, vous devez également traiter le cube source. Par conséquent, le déploiement d'une solution qui utilise des modèles d'exploration de données OLAP peut prendre plus de temps que les solutions d'exploration de données relationnelles.  
  
 En général, les objets d'exploration de données utilisent également les mêmes sources de données et vues de sources de données que celles utilisées pour le cube. Toutefois, vous pouvez ajouter des sources de données et des vues de sources de données qui sont spécifiquement destinées à l'exploration de données. Par exemple, un cube ne contient généralement pas de données sur les clients potentiels, ou sur les données externes non utilisées dans les objets multidimensionnels.  
  
##  <a name="bkmk_Resources"></a> Ressources connexes  
 [Déplacement d’objets d’exploration de données](../../analysis-services/data-mining/moving-data-mining-objects.md)  
  
 Si votre modèle est basé uniquement sur des données relationnelles, l'exportation et l'importation d'objets à l'aide de DMX est la façon la plus simple de déplacer des modèles.  
  
 [Déplacer une base de données Analysis Services](../../analysis-services/multidimensional-models/move-an-analysis-services-database.md)  
  
 Lorsque des modèles utilisent un cube comme source de données, reportez-vous à cette rubrique pour plus d'informations sur le déplacement de modèles et de leurs données de cube de prise en charge.  
  
 [Déployer des projets Analysis Services & #40 ; SSDT & #41 ;](../../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md)  
  
 Fournit des informations générales sur le déploiement de projets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , et décrit les propriétés que vous pouvez définir dans le cadre de la configuration du projet.  
  
## <a name="see-also"></a>Voir aussi  
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)   
 [Outils de requête d'exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md)   
 [Le traitement de la configuration requise et considérations relatives à la & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md)  
  
  
