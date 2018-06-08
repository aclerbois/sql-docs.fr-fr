---
title: Traitement d’Analysis Services, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.asprocessingtask.f1
- sql13.dts.designer.asprocessingtask.general.f1
- sql13.dts.designer.asprocessingtask.as.f1
helpviewer_keywords:
- Analysis Services Processing task
- processing objects [Integration Services]
ms.assetid: e5748836-b4ce-4e17-ab6b-617a336f02f4
caps.latest.revision: 52
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 86a4674fff0795918fe89a26b62b5b15524d543e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="analysis-services-processing-task"></a>tâche de traitement d'Analysis Services
  La tâche de traitement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] traite les objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tels que les modèles tabulaires, les cubes, les dimensions et les modèles d'exploration de données.  
  
 Lors du traitement des modèles tabulaires, gardez à l'esprit les points suivants :  
  
-   Vous ne pouvez pas effectuer d'analyse d'impact sur les modèles tabulaires.  
  
-   Certaines options de traitement du mode tabulaire ne sont pas exposées, par exemple Traiter la défragmentation et Traiter le recalcul. Vous pouvez exécuter ces fonctions à l'aide de la tâche DDL d'exécution.  
  
-   Les options Traiter l'index et Traiter la mise à jour ne conviennent pas aux modèles tabulaires et ne doivent pas être utilisées.  
  
-   Les paramètres de lot sont ignorés pour les modèles tabulaires.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend une série de tâches qui effectuent des opérations de Business Intelligence, telles que l’exécution d’instructions DDL (Data Definition Language) et de requêtes de prédiction d’exploration de données. Pour plus d'informations sur les tâches Business Intelligence associées, cliquez sur l'une des rubriques suivantes :  
  
-   [Tâche DDL d'exécution de SQL Server Analysis Services](../../integration-services/control-flow/analysis-services-execute-ddl-task.md)  
  
-   [Tâche de requête d’exploration de données](../../integration-services/control-flow/data-mining-query-task.md)  
  
## <a name="object-processing"></a>Traitement des objets  
 Plusieurs objets peuvent être traités simultanément. Pour traiter plusieurs objets, vous définissez des paramètres qui s'appliquent au traitement de tous les objets du traitement.  
  
 Les objets d'un traitement peuvent être traités de façon séquentielle ou parallèle. Si le traitement ne contient pas d'objets nécessitant un traitement séquentiel, un traitement parallèle peut accélérer la procédure. Si des objets du traitement sont traités en parallèle, vous pouvez configurer la tâche afin qu'elle détermine le nombre d'objets à traiter de la sorte ou bien spécifier manuellement le nombre d'objets à traiter simultanément. Si des objets sont traités de façon séquentielle, vous pouvez définir un attribut de transaction sur le traitement en inscrivant tous les objets dans une même transaction ou en utilisant une transaction distincte pour chaque objet du traitement.  
  
 Lorsque vous traitez des objets analytiques, vous pouvez également traiter les objets qui dépendent de ceux-ci. La tâche de traitement d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dispose d'une option qui, outre les objets sélectionnés, permet de traiter tous les objets dépendants.  
  
 En principe, vous traitez les tables de dimension avant de traiter les tables de faits. Des erreurs peuvent se produire si vous essayez de traiter les tables de faits avant de traiter les tables de dimension.  
  
 Cette tâche vous permet également de configurer la gestion des erreurs de clés de dimension. Par exemple, la tâche peut ignorer les erreurs ou s'arrêter dès qu'un certain nombre d'erreurs se sont produites. La tâche peut utiliser la configuration d'erreur par défaut ou vous pouvez construire une configuration d'erreur personnalisée. Dans la configuration d'erreur personnalisée, vous indiquez les conditions d'erreur et comment la tâche gère les erreurs. Par exemple, vous pouvez spécifier que l'exécution de la tâche doit s'arrêter dès la quatrième erreur ou indiquer comment la tâche doit gérer les valeurs de clé **NULL** . La configuration d'erreur personnalisée peut également comprendre le chemin d'accès d'un journal d'erreurs.  
  
> [!NOTE]  
>  La tâche de traitement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne peut traiter que les objets analytiques créés à l'aide des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Cette tâche est fréquemment utilisée avec une tâche d'insertion en bloc qui charge des données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou avec une tâche de flux qui met en œuvre un flux chargeant des données dans une table. Par exemple, la tâche de flux peut avoir un flux qui extrait des données d’une base de données OLTP (Online Transaction Processing) et les charge dans une table de faits d’un entrepôt de données, à la suite de quoi la tâche de traitement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est appelée pour traiter le cube basé sur l’entrepôt de données.  
  
 La tâche de traitement d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise un gestionnaire de connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour se connecter à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d'informations, consultez [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md).  
  
## <a name="error-handling"></a>Gestion des erreurs  
  
## <a name="configuration-of-the-analysis-services-processing-task"></a>Configuration de la tâche de traitement Analysis Services  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="programmatic-configuration-of-the-analysis-services-processing-task"></a>Configuration par programmation de la tâche de traitement Analysis Services  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.DataTransformationServices.Tasks.DTSProcessingTask.DTSProcessingTask>  
  
## <a name="analysis-services-processing-task-editor-general-page"></a>Éditeur de tâche de traitement d'Analysis Services (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de traitement d’Analysis Services** pour nommer et décrire la tâche de traitement d’Analysis Services.  
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique à la tâche de traitement Analysis Services. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Tapez une description de la tâche de traitement Analysis Services.  
  
## <a name="analysis-services-processing-task-editor-analysis-services-page"></a>Éditeur de tâche de traitement d'Analysis Services (page Analysis Services)
  Utilisez la page **Analysis Services** de la boîte de dialogue **Éditeur de tâche de traitement d’Analysis Services** pour définir un gestionnaire de connexions Analysis Services, sélectionner les objets analytiques à traiter et définir les options de traitement et de gestion des erreurs.  
  
 Lors du traitement des modèles tabulaires, gardez à l'esprit les points suivants :  
  
1.  Vous ne pouvez pas effectuer d'analyse d'impact sur les modèles tabulaires.  
  
2.  Certaines options de traitement du mode tabulaire ne sont pas exposées, par exemple Traiter la défragmentation et Traiter le recalcul. Vous pouvez exécuter ces fonctions à l'aide de la tâche DDL d'exécution.  
  
3.  Certaines options de traitement fournies, par exemple le traitement des index, ne conviennent pas aux modèles tabulaires et ne doivent pas être utilisées.  
  
4.  Les paramètres de lot sont ignorés pour les modèles tabulaires.  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions Analysis Services**  
 Sélectionnez un gestionnaire de connexions Analysis Services existant dans la liste ou cliquez sur **Nouveau** pour créer un nouveau gestionnaire de connexions.  
  
 **Nouveau**  
 Créez un nouveau gestionnaire de connexions Analysis Services.  
  
 **Rubriques connexes :** [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md), [Référence de l’interface utilisateur de la boîte de dialogue Ajout d’un gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)  
  
 **Liste d'objets**  
 |Propriété|Description|  
|--------------|-----------------|  
|**Nom de l'objet**|Affiche la liste des noms d'objets définis.|  
|**Type**|Affiche la liste des types des objets définis.|  
|**Options de traitement**|Sélectionnez une option de traitement dans la liste.<br /><br /> **Rubriques connexes** : [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)|  
|**Paramètres**|Affiche la liste des paramètres de traitement des objets définis.|  
  
 **Ajouter**  
 Ajoutez un objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à la liste.  
  
 **Supprimer**  
 Sélectionnez un objet et cliquez sur **Supprimer**.  
  
 **Analyse d'impact**  
 Analyse l'impact sur l'objet sélectionné.  
  
 **Rubriques connexes :** [Boîte de dialogue Analyse d’impact &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/208268eb-4e14-44db-9c64-6f74b776adb6)  
  
 **Résumé des paramètres du traitement**  
 |Propriété|Description|  
|--------------|-----------------|  
|**Ordre de traitement**|Indique si les objets sont traités séquentiellement ou dans un traitement. Si le traitement parallèle est utilisé, indique le nombre d'objets à traiter simultanément.|  
|**Mode de transaction**|Indique le mode de transaction du traitement séquentiel.|  
|**Erreurs de la dimension**|Indique le comportement de la tâche lorsqu'une erreur se produit.|  
|**Chemin d'accès du journal des erreurs de clé de dimension**|Définit le chemin d'accès du fichier de consignation des erreurs.|  
|**Traiter les objets affectés**|Indique si les objets dépendants ou affectés sont également traités.|  
  
 **Modifier les paramètres**  
 Change les options de traitement et la gestion des erreurs dans les clés de dimension.  
  
 **Rubriques connexes :** [Boîte de dialogue Modifier les paramètres &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/0041e042-d7ce-48f9-a690-a6dc65471ff3)  
  
