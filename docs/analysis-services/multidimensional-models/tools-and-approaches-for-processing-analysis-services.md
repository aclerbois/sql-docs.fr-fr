---
title: Outils et approches de traitement (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4ecb64ddf6caedc2353541ab5d4aa7229b9a120f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tools-and-approaches-for-processing-analysis-services"></a>Outils et approches de traitement (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le traitement est une opération selon laquelle Analysis Services interroge les données provenant d'une source de données relationnelle et remplit des objets Analysis Services à l'aide de ces données.  
  
 En tant qu'administrateur système Analysis Services, vous pouvez exécuter et contrôler le traitement d'objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide des ces approches.  
  
-   Exécuter une analyse d'impact pour comprendre les dépendances entre les objets et l'étendue des opérations  
  
-   Traiter un objet dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
-   Traiter un ou plusieurs objets dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]  
  
-   Exécutez une analyse d'impact pour examiner la liste des objets connexes non traités à la suite de l'action actuelle.  
  
-   Générer et exécuter un script dans une fenêtre de requête XMLA [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour traiter un ou plusieurs objets  
  
-   Utiliser des applets de commande [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] PowerShell  
  
-   Utiliser des flux de contrôle et des tâches dans des packages [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
-   Surveiller le traitement avec SQL Server Profiler  
  
-   Programmez une solution personnalisée à l'aide d'AMO. Pour plus d’informations, consultez [Programmation d’objets de base OLAP AMO](../../analysis-services/multidimensional-models/analysis-management-objects/programming-amo-olap-basic-objects.md).  
  
 Le traitement est une opération hautement configurable, contrôlée par un ensemble d'options de traitement qui déterminent si un traitement complet ou incrémentiel se produit au niveau de l'objet. Pour plus d’informations sur les options de traitement et sur le traitement des objets, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md) et [Traitement des objets Analysis Services](../../analysis-services/multidimensional-models/processing-analysis-services-objects.md).  
  
> [!NOTE]  
>  Cette rubrique décrit les outils et approches pour traiter des modèles multidimensionnels. Pour plus d’informations sur le traitement des modèles tabulaires, consultez [processus de base de données, la Table ou Partition &#40;Analysis Services&#41; ](../../analysis-services/tabular-models/process-database-table-or-partition-analysis-services.md) et [traiter les données](../../analysis-services/tabular-models/process-data-ssas-tabular.md).  
  
### <a name="processing-objects-in-sql-server-management-studio"></a>Objets de traitement dans SQL Server Management Studio  
  
1.  Démarrez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et connectez-vous à Analysis Services.  
  
2.  Cliquez avec le bouton droit sur l’objet Analysis Services à traiter, puis cliquez sur **Traiter**. Vous pouvez traiter les données à tous les niveaux :  
  
    -   Bases de données  
  
    -   Cubes  
  
    -   Groupes de mesures ou partitions individuelles dans le groupe de mesures  
  
    -   Dimensions  
  
    -   Modèles d'exploration de données  
  
    -   Structures d'exploration de données  
  
     Les objets Analysis Services sont hiérarchiques. Si vous choisissez la base de données, le traitement concernera tous les objets contenus dans la base de données. En fonction de l'option de traitement sélectionnée et de l'état de l'objet, le traitement va avoir lieu ou ne pas avoir lieu. Notamment, si un objet n'est pas traité, le traitement de son objet parent entraînera le traitement de l'objet d'origine. Pour plus d’informations sur les dépendances entre les objets, consultez [Traitement des objets Analysis Services](../../analysis-services/multidimensional-models/processing-analysis-services-objects.md).  
  
3.  Dans la boîte de dialogue **Traitement** dans **Options de traitement**, utilisez la valeur par défaut fournie ou sélectionnez une option différente dans la liste. Pour plus d’informations sur chaque option, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
4.  Cliquez sur **Analyse d’impact** pour identifier et éventuellement traiter les objets dépendants affectés si les objets figurant dans la boîte de dialogue Traiter sont traités.  
  
5.  Vous pouvez également cliquer sur **Modifier les paramètres** pour modifier l’ordre de traitement, le comportement de traitement relatif aux types d’erreurs spécifiques et d’autres paramètres.  
  
6.  Cliquez sur **OK**.  
  
     La boîte de dialogue État d'avancement du traitement fournit l'état de chaque commande en cours. Si un message d’état est tronqué, vous pouvez cliquer sur **Afficher les détails** pour lire le message en entier.  
  
### <a name="processing-objects-in-sql-server-data-tools"></a>Traitement d'objets dans les outils de données SQL Server  
  
1.  Démarrez [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et ouvrez un projet qui a été déployé.  
  
2.  Dans l'Explorateur de solutions, sous le projet déployé, développez le dossier **Dimensions** .  
  
3.  Cliquez avec le bouton droit sur une dimension, puis cliquez sur **Traiter**. Vous pouvez cliquer avec le bouton droit sur plusieurs dimensions pour traiter plusieurs objets à la fois. Pour plus d’informations, consultez [Traitement par lots &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/batch-processing-analysis-services.md).  
  
4.  Dans la boîte de dialogue **Traiter la dimension** , dans la colonne **Options de traitement** sous **Liste d'objets**, vérifiez que l'option pour cette colonne est **Traiter entièrement**. Si ce n’est pas le cas, sous **Options de traitement**, cliquez sur l’option, puis sélectionnez **Traiter entièrement** dans la liste déroulante.  
  
5.  Cliquez sur **Exécuter**.  
  
6.  Une fois le traitement terminé, cliquez sur **Fermer**.  
  
##  <a name="bkmk_impactanalysis"></a> Exécuter une analyse d'impact pour identifier les dépendances entre les objets et l'étendue des opérations  
  
1.  Avant de traiter un objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ou [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vous pouvez analyser l’impact du traitement sur les objets associés en cliquant sur **Analyse d’impact** dans l’une des boîtes de dialogue **Traiter les objets** .  
  
2.  Cliquez avec le bouton droit sur une dimension, un cube, un groupe de mesures ou une partition pour ouvrir une boîte de dialogue **Traiter les objets** .  
  
3.  Cliquez sur **Analyse d’impact**. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] analyse le modèle et génère un rapport sur les conditions requises pour les objets liés à celui qui est sélectionné en vue du traitement.  
  
### <a name="processing-objects-using-xmla"></a>Traitement d'objets à l'aide de XMLA  
  
1.  Démarrez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et connectez-vous à Analysis Services.  
  
2.  Cliquez avec le bouton droit sur l’objet à traiter, puis cliquez sur **Traiter**.  
  
3.  Dans la boîte de dialogue **Traitement** , sélectionnez l’option de traitement à utiliser. Modifiez tout autre paramètre. Exécutez une analyse d'impact pour déterminer si vous devez apporter des modifications.  
  
4.  Cliquez sur **Script** sur l’écran **Traiter les objets** .  
  
     Cela génère un script XMLA et ouvre la fenêtre Requête XMLA [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
5.  Fermez la boîte de dialogue. Le script contient la commande et les options de traitement spécifiées dans la boîte de dialogue.  
  
6.  Éventuellement, vous pouvez continuer à ajouter des objets au script si vous souhaitez traiter des objets supplémentaires dans le même traitement. Pour continuer, répétez les étapes précédentes, en ajoutant des objets au script généré afin de disposer d'un seul script pour toutes les opérations de traitement. Pour afficher un exemple, consultez [Planifier des tâches administratives SSAS avec SQL Server Agent](../../analysis-services/instances/schedule-ssas-administrative-tasks-with-sql-server-agent.md).  
  
7.  Dans la barre de menus, cliquez sur **Requête**, puis sur **Exécuter**.  
  
### <a name="processing-objects-using-powershell"></a>Traitement d'objets à l'aide de PowerShell  
  
1.  À partir de cette version de SQL Server, vous pouvez utiliser les applets de commande Analysis Services PowerShell pour traiter les objets. Les applets de commande suivants peuvent être exécutées de façon interactive ou dans un script :  
  
    -   [Invoke-ProcessCube, applet de commande](../../analysis-services/powershell/invoke-processcube-cmdlet.md)  
  
    -   [Invoke-ProcessDimension, applet de commande](../../analysis-services/powershell/invoke-processdimension-cmdlet.md)  
  
    -   [Invoke-ProcessPartition, applet de commande](../../analysis-services/powershell/invoke-processpartition-cmdlet.md)  
  
    -   [Applet de commande Invoke-ASCmd](../../analysis-services/powershell/invoke-ascmd-cmdlet.md), qui peut être utilisée pour exécuter un script XMLA, MDX ou DMX qui inclut les commandes de traitement.  
  
### <a name="monitoring-object-processing-using-sql-server-profiler"></a>Surveillance du traitement des objets à l'aide de SQL Server Profiler  
  
1.  Connectez-vous à une instance Analysis Services dans SQL Server Profiler.  
  
2.  Dans Sélection des événements, cliquez sur **Afficher tous les événements** pour ajouter tous les événements à la liste.  
  
3.  Choisissez l'un des événements suivants :  
  
    -   **Début de la commande** et **Fin de la commande** pour afficher le démarrage et l’arrêt du traitement  
  
    -   **Erreur** pour capturer les erreurs  
  
    -   **Début du rapport de progression**, **Rapport de progression actuel**et **Fin du rapport de progression** pour créer un rapport sur l’état de processus et afficher les requêtes SQL utilisées pour récupérer les données  
  
    -   **Exécuter script MDX Début** et **Exécuter script MDX Fin** pour afficher les calculs de cube  
  
    -   Éventuellement, ajoutez des événements de verrou si vous analysez des problèmes de performances liés au traitement.  
  
### <a name="process-analysis-services-objects-using-integration-services"></a>Traiter des objets Analysis Services à l'aide d'Integration Services  
  
1.  Dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], créez un package qui utilise la tâche de traitement Analysis Services pour remplir automatiquement des objets avec de nouvelles données quand vous effectuez des mises à jour régulières de votre base de données relationnelle source.  
  
2.  Dans **Boîte à outils SSIS**, double-cliquez sur **Traitement Analysis Services** pour l’ajouter au package.  
  
3.  Modifiez la tâche pour spécifier une connexion à la base de données, les objets à traiter et l'option de traitement. Pour plus d'informations sur la manière d'implémenter cette tâche, consultez [Analysis Services Processing Task](../../integration-services/control-flow/analysis-services-processing-task.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)  
  
  
