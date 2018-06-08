---
title: Déploiement de solutions de modèle multidimensionnel | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4e5f4b4f7f30ec9f94993e34596d348ae22842fd
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="multidimensional-model-solution-deployment"></a>Déploiement d'une solution de modèle multidimensionnel
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une fois le développement d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] terminé, vous pouvez déployer la base de données sur un serveur Analysis Services. Analysis Services fournit six méthodes de déploiement possibles qui peuvent être utilisées pour déplacer la base de données vers un serveur de production ou de test. Les méthodes sont énumérées ici dans l'ordre de leur avantage : automatisation AMO (Analysis Management Objects), XMLA, Assistant Déploiement, Utilitaire de déploiement, Assistant Synchronisation, Sauvegarde et Restauration.  
  
 Cette rubrique comprend les sections suivantes :  
  
 [Méthodes de déploiement](#bkmk_meth)  
  
 [Points à prendre en considération pour le déploiement](#bkmk_considerations)  
  
 [Tâches associées](#bkmk_rel)  
  
##  <a name="bkmk_meth"></a> Méthodes de déploiement  
  
|Méthode|Description|Lien|  
|------------|-----------------|----------|  
|**Automatisation AMO (Analysis Management Objects)**|AMO fournit une interface de programmation à l'ensemble de commandes complet pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], y compris les commandes qui peuvent être utilisées pour le déploiement de solutions. En tant qu'approche au déploiement de solutions, l'automatisation AMO constitue la méthode la plus souple, mais elle nécessite également un effort de programmation.  Le principal avantage de l'utilisation d'AMO est que vous pouvez utiliser l'Agent SQL Server avec votre application AMO pour exécuter le déploiement selon une planification prédéfinie.|[Développement avec Analysis Management Objects & #40 ; AMO & #41 ;](../../analysis-services/multidimensional-models/analysis-management-objects/developing-with-analysis-management-objects-amo.md)|  
|**XMLA**|Utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour générer un script XMLA des métadonnées d'une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante, puis exécutez le script sur un autre serveur pour recréer la base de données initiale. Les scripts XMLA sont aisément formés dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en définissant le processus de déploiement, puis en le codifiant et en l'enregistrant dans un script XMLA. Une fois que le script XMLA est dans un fichier sauvegardé, vous pouvez aisément l'exécuter le script conformément au calendrier ou l'incorporer dans une application qui se connecte directement à une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].<br /><br /> Vous pouvez également exécuter des scripts XMLA sur une base prédéfinie à l'aide de l'Agent SQL Server, mais la flexibilité n'est pas aussi grande que celle offerte par AMO. AMO fournit un large éventail de fonctionnalités en hébergeant tout le spectre des commandes administratives.|[Déployer des Solutions de modèle à l’aide de XMLA](../../analysis-services/multidimensional-models/deploy-model-solutions-using-xmla.md)|  
|**Assistant Déploiement**|Utilisez l'Assistant Déploiement pour utiliser les fichiers de sortie XMLA générés par un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour déployer les métadonnées du projet sur un serveur de destination. Avec l'Assistant Déploiement, vous pouvez effectuer directement le déploiement à partir du fichier [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , tel que créé dans le répertoire de sortie par la génération du projet.<br /><br /> Le principal avantage de l'utilisation de l'assistant Déploiement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est son aspect pratique. Tout comme vous pouvez enregistrer un script XMLA en vue d'une utilisation ultérieure dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez enregistrer les scripts de l'assistant Déploiement. L'Assistant Déploiement peut être exécuté de façon interactive et à partir de l'invite de commandes via l'Utilitaire de déploiement.|[Déployer des Solutions de modèle à l’aide de l’Assistant de déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-using-the-deployment-wizard.md)|  
|**Utilitaire de déploiement**|L'utilitaire de déploiement permet de démarrer le moteur de déploiement Analysis Services à partir d'une invite de commandes.|[Déployer des Solutions de modèle avec l’utilitaire de déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility.md)|  
|**Assistant Synchronisation de base de données**|Utilisez l'Assistant Synchronisation de base de données pour synchroniser les métadonnées et les données entre deux bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .<br /><br /> L'Assistant Synchronisation peut être utilisé pour copier des données et des métadonnées d'un serveur source vers un serveur de destination. Si le serveur de destination n'a pas de copie de la base de données que vous souhaitez déployer, une nouvelle base de données est copiée sur le serveur de destination. Si le serveur de destination dispose déjà d'une copie de la même base de données, la base de données sur le serveur de destination est mise à jour afin d'utiliser les métadonnées et les données de la base de données source.|[Synchroniser les bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md)|  
|**Sauvegarde et restauration**|La sauvegarde offre la méthode la plus simple permettant de transférer des bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Dans la boîte de dialogue **Sauvegarde** , vous pouvez définir la configuration des options, puis exécuter la sauvegarde à partir de cette même boîte de dialogue. Vous pouvez également créer un script qui peut être enregistré et exécuté aussi souvent que nécessaire.<br /><br /> La sauvegarde et la restauration ne sont pas utilisées aussi souvent que les autres méthodes de déploiement, mais elles permettent de terminer rapidement un déploiement avec une infrastructure minimale.|[Sauvegarde et restauration de bases de données Analysis Services](../../analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases.md)|  
  
##  <a name="bkmk_considerations"></a> Points à prendre en considération pour le déploiement  
 Avant de déployer un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , prenez en considération les questions qui s’appliquent à votre solution, puis passez en revue le lien associé pour savoir comment résoudre le problème :  
  
|Considération|Lien vers des informations  supplémentaires|  
|-------------------|------------------------------|  
|Quelles ressources matérielles et logicielles sont nécessaires pour cette solution ?|[Exigences et les considérations de déploiement d’Analysis Services](../../analysis-services/multidimensional-models/requirements-and-considerations-for-analysis-services-deployment.md)|  
|Comment allez-vous déployer les objets connexes qui se trouvent en dehors de l’étendue du projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , par exemple les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , les rapports ou les schémas de base de données relationnelle ?||  
|Comment allez-vous charger et mettre à jour les données dans la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] déployée ?<br /><br /> Comment allez-vous mettre à jour les métadonnées (par exemple les calculs) dans la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] déployée ?|[Méthodes de déploiement](#bkmk_meth) dans cette rubrique.|  
|Souhaitez-vous autoriser des utilisateurs à accéder aux données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à travers Internet ?|[Configurer l’accès HTTP à Analysis Services sur Internet Information Services & #40 ; IIS & #41 ; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md)|  
|Souhaitez-vous fournir un accès par requête continu aux données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ?|[Exigences et les considérations de déploiement d’Analysis Services](../../analysis-services/multidimensional-models/requirements-and-considerations-for-analysis-services-deployment.md)|  
|Souhaitez-vous déployer des objets dans un environnement distribué en utilisant des objets liés ou des partitions distantes ?|[Créer et gérer une partition locale &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/create-and-manage-a-local-partition-analysis-services.md), [Créer et gérer une partition distante &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/create-and-manage-a-remote-partition-analysis-services.md) et [Groupes de mesures liés](../../analysis-services/multidimensional-models/linked-measure-groups.md).|  
|Comment allez-vous sécuriser les données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ?|[Autorisation d’accès aux objets et les opérations de & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/authorizing-access-to-objects-and-operations-analysis-services.md)|  
  
##  <a name="bkmk_rel"></a> Tâches associées  
 [Exigences et les considérations de déploiement d’Analysis Services](../../analysis-services/multidimensional-models/requirements-and-considerations-for-analysis-services-deployment.md)  
  
 [Déployer des Solutions de modèle à l’aide de XMLA](../../analysis-services/multidimensional-models/deploy-model-solutions-using-xmla.md)  
  
 [Déployer des Solutions de modèle à l’aide de l’Assistant de déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-using-the-deployment-wizard.md)  
  
 [Déployer des Solutions de modèle avec l’utilitaire de déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility.md)  
  
  
