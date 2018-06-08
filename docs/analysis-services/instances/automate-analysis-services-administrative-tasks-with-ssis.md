---
title: Automatiser les tâches d’administration Analysis Services avec SSIS | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c750c0e8ee9f13c4b4751af872b02f4ed9ee419a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="automate-analysis-services-administrative-tasks-with-ssis"></a>Automatiser les tâches d'administration Analysis Services avec SSIS
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vous permet d’automatiser l’exécution de scripts DDL, cube et de modèle d’exploration de données de traitement des tâches et les tâches de requête d’exploration de données. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peut être considéré comme une collection de tâches de flux de contrôle et de maintenance, qui peuvent être liées pour former des tâches de traitement de données séquentielles et parallèles.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] a été conçu pour effectuer des opérations de nettoyage de données au cours des tâches de traitement de données et pour rassembler des données de sources de données différentes. En cas d’utilisation de cubes et de modèles d’exploration de données, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peut transformer des données non numériques en données numériques et peut garantir que les valeurs appartiennent aux intervalles prévus, créant ainsi des données propres qui permettront de remplir les tables de faits et les dimensions.  
  
## <a name="integration-services-tasks"></a>Tâches Integration Services  
 Il existe deux éléments principaux dans toute tâche ou travail [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] : les éléments de flux de contrôle et les éléments de flux de données. Les éléments de flux de contrôle définissent l'ordre logique de la progression des travaux en appliquant des contraintes de précédence. Les éléments de flux de données se rapportent à la connexion entre la sortie d'un composant et l'entrée du composant suivant, ainsi qu'à toutes les transformations des données qui peuvent avoir lieu entre les deux. Pour ce qui est du choix de l'emplacement de destination des données, les contraintes de précédence incorporent une logique permettant de spécifier le composant qui reçoit la sortie. Les tâches de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] les plus pertinentes par rapport à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] incluent la tâche DDL d'exécution, la tâche de traitement Analysis Services et la tâche de requête d'exploration de données. Pour chacune de ces tâches, la tâche Envoyer un message peut être utilisée pour envoyer à l'administrateur un message électronique contenant les résultats de la tâche.  
  
## <a name="the-execute-ddl-task"></a>Tâche DDL d'exécution  
 La tâche DDL d'exécution dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vous permet d'envoyer des scripts DDL directement vers le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et de les exécuter automatiquement. Cela permet à l'administrateur de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] d'effectuer des opérations de sauvegarde, restauration et synchronisation à partir de l'intérieur d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Un package se compose des éléments de flux de contrôle et de données décrits précédemment, qui doivent tous être **run regularly**, de même que les autres instructions DDL qui peuvent être ajoutées aux tâches. Comme les tâches décrites ici sont souvent exécutées pendant la nuit, il est particulièrement utile de disposer de packages qui peuvent être exécutés facilement à partir de toute application de planification. Vous pouvez planifier un package pour l'exécuter à tout moment à l'aide de l'Agent [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations sur la manière d’implémenter cette tâche, consultez [Tâche DDL d’exécution de SQL Server Analysis Services](../../integration-services/control-flow/analysis-services-execute-ddl-task.md).  
  
## <a name="analysis-services-processing-task"></a>tâche de traitement d'Analysis Services  
 La tâche de traitement Analysis Services dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vous permet de remplir automatiquement des cubes avec de nouvelles informations lorsque vous effectuez des mises à jour régulières de votre base de données relationnelle source. Vous pouvez effectuer le traitement au niveau d'une dimension, d'un cube ou d'une partition à l'aide de la tâche de traitement Analysis Services. Le traitement lui-même peut être de type **incremental** ou **full**, tel que vous le sélectionnez dans vos exigences de travail. Le traitement incrémentiel ajoute de nouvelles données et renouvelle suffisamment les calculs pour maintenir à jour la cible, tandis que le traitement complet supprime les données existantes pour effectuer un rechargement complet et renouveler tous les calculs. Le traitement complet prend plus de temps, mais il est plus complet. Pour plus d'informations sur la manière d'implémenter cette tâche, consultez [Analysis Services Processing Task](../../integration-services/control-flow/analysis-services-processing-task.md).  
  
## <a name="data-mining-query-task"></a>Tâche de requête d'exploration de données  
 La tâche de requête d'exploration de données dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vous permet d'extraire et de stocker des informations à partir de modèles d'exploration de données. Les informations sont souvent stockées dans une base de données relationnelle et, par exemple, peuvent être utilisées pour isoler une liste de clients potentiels pour une campagne de marketing ciblée. L'exploration de données peut identifier la valeur d'un client et la probabilité que ce client réponde à une sollicitation marketing particulière. Vous pouvez utiliser la tâche de requête d'exploration de données pour extraire des données et les convertir dans un format préféré. Pour plus d'informations sur la manière d'implémenter cette tâche, consultez [Data Mining Query Task](../../integration-services/control-flow/data-mining-query-task.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Destination de traitement de partition](../../integration-services/data-flow/partition-processing-destination.md)   
 [Destination de traitement de dimension](../../integration-services/data-flow/dimension-processing-destination.md)   
 [Transformation de requête d’exploration de données](../../integration-services/data-flow/transformations/data-mining-query-transformation.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)   
  
  
