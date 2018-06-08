---
title: Traitement d’un modèle multidimensionnel (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a09e1c4118576535371e16aa5e1a39a169b406fe
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="processing-a-multidimensional-model-analysis-services"></a>Traitement d’un modèle multidimensionnel (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le traitement est l'étape, ou la série d'étapes, dans lesquelles Analysis Services charge des données d'une source de données relationnelle dans un modèle multidimensionnel. Pour les objets qui utilisent le mode de stockage MOLAP, les données sont enregistrées sur le disque le dossier des fichiers de la base de données. Pour le mode de stockage ROLAP, le traitement s'effectue à la demande, en réponse à une requête MDX sur un objet. Pour les objets qui utilisent le stockage ROLAP, le traitement fait référence à la mise à jour du cache avant de retourner des résultats de la requête.  
  
 Par défaut, le traitement s'effectue lorsque vous déployez une solution sur le serveur. Vous pouvez également traiter une partie d'une solution, ad hoc ou à l'aide d'outils tels que [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ou selon une planification à l'aide de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et de SQL Server Agent. Lorsque vous apportez une modification structurelle au modèle, telle que la suppression d'une dimension ou la modification de son niveau de compatibilité, vous devez réeffectuer le traitement afin de synchroniser les aspects physiques et logiques du modèle.  
  
 Cette rubrique comprend les sections suivantes :  
  
 [Configuration requise](#bkmk_prereq)  
  
 [Choix d'un outil ou d'une approche](#bkmk_tool)  
  
 [Traitement d'objets](#bkmk_proc)  
  
 [Reprocessing Objects](#bkmk_reproc)  
  
##  <a name="bkmk_prereq"></a> Configuration requise  
  
-   Le traitement nécessite des autorisations d'administration sur l'instance Analysis Services. Si effectuez le traitement de manière interactive dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ou [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vous devez être membre du rôle administrateur de serveur sur l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . En ce qui concerne le traitement sans assistance, par exemple à l'aide d'un package SSIS que vous planifiez via SQL Server Agent, le compte utilisé pour exécuter le package doit être membre du rôle administrateur de serveur. Pour plus d’informations sur la définition des autorisations d’administrateur, consultez [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md).  
  
-   Le compte utilisé pour récupérer des données est spécifié dans l'objet source de données, en tant qu'option d'emprunt d'identité si vous utilisez l'authentification Windows, ou en tant que nom d'utilisateur dans la chaîne de connexion si vous utilisez l'authentification de base de données. Le compte doit disposer d'autorisations en lecture sur les sources de données relationnelles utilisées par le modèle.  
  
-   Le projet ou la solution doit être déployé pour que vous puissiez traiter les objets.  
  
     Initialement, au cours des premières phases de développement du modèle, le déploiement et le traitement se produisent simultanément. Toutefois, vous pouvez définir des options pour traiter le modèle ultérieurement, après que vous avez déployé la solution. Pour plus d’informations sur le déploiement, consultez [Déployer des projets Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md).  
  
##  <a name="bkmk_tool"></a> Choix d'un outil ou d'une approche  
 Vous pouvez traiter des objets de manière interactive à l'aide d'une application cliente telle que [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ou [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ou d'une opération de script qui s'exécute en tant que travail SQL Server Agent ou package [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Le mode de traitement d'une base de données varie considérablement selon que le modèle est en phase de développement actif ou de production. Une fois qu'un modèle est déployé sur un serveur de production, le traitement doit être étroitement contrôlé pour garantir l'intégrité et la disponibilité des données multidimensionnelles. Les objets étant interdépendants, le traitement a en général un effet en cascade sur le modèle lorsque d'autres objets sont également traités ou non en tandem. Si certains objets sont conservés dans un état non traité, les requêtes pour ces données ne seront pas résolues et interrompront les rapports ou applications qui les utilisent. Lors du développement d'une stratégie pour traiter une base de données de production, envisagez d'utiliser un script ou des packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] que vous avez débogués et testés pour éviter une erreur d'opérateur ou des étapes négligées.  
  
 Pour plus d’informations, consultez [Outils et approches de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/tools-and-approaches-for-processing-analysis-services.md).  
  
##  <a name="bkmk_proc"></a> Traitement d'objets  
 Le traitement affecte les objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] suivants : groupes de mesures, partitions, dimensions, cubes, modèles d'exploration de données, structures d'exploration et bases de données. Lorsqu'un objet contient un ou plusieurs objets, le traitement de l'objet de niveau supérieur entraîne le traitement en cascade de tous les objets de niveau inférieur. Par exemple, un cube contient généralement un ou plusieurs groupes de mesures (chacun d'entre eux contenant une ou plusieurs partitions) et des dimensions. Le traitement d'un cube entraîne le traitement de tous les groupes de mesures qu'il contient et des dimensions constituantes qui sont actuellement dans un état non traité. Pour plus d’informations sur le traitement des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , consultez [Traitement des objets Analysis Services](../../analysis-services/multidimensional-models/processing-analysis-services-objects.md).  
  
 Durant l'exécution du travail de traitement, les objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] affectés sont accessibles pour l'interrogation. Le travail de traitement fonctionne à l'intérieur d'une transaction et celle-ci peut être validée ou annulée. Si le travail de traitement échoue, la transaction est restaurée. Si le travail de traitement réussit, un verrou exclusif est placé sur l'objet lorsque des modifications sont validées, ce qui signifie que l'objet est momentanément indisponible pour l'interrogation ou le traitement. Pendant la phase de validation de la transaction, il est toujours possible d'envoyer des requêtes à l'objet, mais celles-ci seront mises en file d'attente jusqu'à la fin de la validation.  
  
 Durant un travail de traitement, le traitement éventuel d'un objet et la façon dont il sera traité dépendent de l'option de traitement définie pour cet objet. Pour plus d’informations sur les options de traitement spécifiques applicables à chaque objet, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
##  <a name="bkmk_reproc"></a> Reprocessing Objects  
 Les cubes contenant des éléments non traités doivent être retraités avant de pouvoir être explorés. Les cubes dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contiennent des groupes de mesures et des partitions doivent être traitées avant de pouvoir interroger le cube. Le traitement d'un cube fait en sorte que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] traite les dimensions constituantes du cube si elles sont dans un état non traité. Une fois qu'un objet a déjà été traité une fois, il doit être retraité, partiellement ou intégralement, à chaque fois que l'une des situations suivantes se produit :  
  
-   la structure de l'objet change, par exemple en cas de suppression d'une colonne dans une table de faits ;  
  
-   la conception d'agrégation de l'objet change ;  
  
-   les données contenues dans l'objet doivent être mises à jour.  
  
 Lorsque vous traitez des objets dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous pouvez sélectionner une option de traitement ou vous pouvez demander à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de déterminer le type de traitement approprié. Les méthodes de traitement disponibles varient d'un objet à l'autre et sont basées sur le type d'objet. En outre, les méthodes disponibles dépendent des modifications qui ont été apportées à l'objet depuis son dernier traitement. Si vous autorisez [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à sélectionner automatiquement une méthode de traitement, la méthode utilisée sera celle qui permet de traiter complètement l’objet le plus rapidement possible. Pour plus d’informations, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Architecture logique &#40;Analysis Services - Données multidimensionnelles&#41;](../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
