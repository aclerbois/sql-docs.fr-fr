---
title: Exécuter des packages Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: packages
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.ispackageexecute.f1
- sql13.ssis.ssms.executepackage.f1
helpviewer_keywords:
- Integration Services packages, running
- SSIS packages, running
- packages [Integration Services], running
- SQL Server Integration Services packages, running
- executing packages [Integration Services]
- running packages [Integration Services]
- Integration Services, (See also Integration Services packages)
ms.assetid: c5fecc23-6f04-4fb2-9a29-01492ea41404
caps.latest.revision: 65
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fe9d96b588130d1b4bab8e611dc2e0143ab457c9
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="run-integration-services-ssis-packages"></a>Exécuter des packages Integration Services (SSIS)
  Pour exécuter un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous pouvez utiliser un outil parmi plusieurs, en fonction de l’endroit où ces packages sont stockés. Ces outils sont répertoriées dans le tableau ci-dessous.  

> [!NOTE]
> Avec la publication de SQL Server pour Linux, vous pouvez exécuter des packages SSIS sur Linux. Pour plus d’informations, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../../linux/sql-server-linux-migrate-ssis.md).
  
 Pour stocker un package sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous utilisez le modèle de déploiement de projet pour déployer le projet sur le serveur. Pour plus d’informations, consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 Pour stocker un package dans le magasin de packages SSIS, la base de données msdb ou dans le système de fichiers, vous utilisez le modèle de déploiement de package. Pour plus d’informations, consultez [Déploiement de packages hérités &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
|Outil|Packages stockés sur le serveur Integration Services|Packages stockés dans le magasin de packages SSIS ou dans la base de données msdb|Packages stockés dans le système de fichiers, hors de l'emplacement qui fait partie du magasin de packages SSIS|  
|----------|-----------------------------------------------------------------|--------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|  
|**Outils de données SQL Server**|non|non<br /><br /> Vous pouvez cependant ajouter un package existant à un projet du magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] , qui inclut la base de données msdb. L'ajout d'un package existant au projet de cette manière effectue une copie locale du package dans le système de fichiers.|Oui|  
|**SQL Server Management Studio, quand vous êtes connecté à une instance du moteur de base de données qui héberge le serveur Integration Services**<br /><br /> Pour plus d’informations, consultez [Boîte de dialogue d’exécution de package](#execute_package_dialog)|Oui|non<br /><br /> Vous pouvez toutefois importer un package vers le serveur à partir de ces emplacements.|non<br /><br /> Vous pouvez toutefois importer un package vers le serveur à partir du système de fichiers.|
|**SQL Server Management Studio, quand vous êtes connecté à une instance du moteur de base de données qui héberge le serveur Integration Services**<br /><br /> Pour plus d’informations, consultez [Exécuter des packages dans Scale Out](../../integration-services/scale-out/run-packages-in-integration-services-ssis-scale-out.md)|Oui|non|non|
|**SQL Server Management Studio, quand il est connecté au service Integration Services qui gère le magasin de packages SSIS**|non|Oui|non<br /><br /> Vous pouvez cependant importer un package dans le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] à partir du système de fichiers.|  
|**dtexec**<br /><br /> Pour plus d’informations, voir [dtexec Utility](../../integration-services/packages/dtexec-utility.md).|Oui|Oui|Oui|  
|**dtexecui**<br /><br /> Pour plus d’informations, consultez [Référence de l’interface utilisateur de l’utilitaire d’exécution de package &#40;DtExecUI&#41;](../../integration-services/packages/execute-package-utility-dtexecui-ui-reference.md)|non|Oui|Oui|  
|**SQL Server Agent**<br /><br /> Vous pouvez utiliser un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour planifier un package.<br /><br /> Pour plus d’informations, consultez [Travaux de l’Agent SQL Server pour les packages](../../integration-services/packages/sql-server-agent-jobs-for-packages.md).|Oui|Oui|Oui|  
|**Procédure stockée intégrée**<br /><br /> Pour plus d’informations, consultez [catalog.start_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)|Oui|non|non|  
|**API managée à l’aide des types et membres dans l’espace de noms** <xref:Microsoft.SqlServer.Management.IntegrationServices>|Oui|non|non|  
|**API managée à l’aide des types et membres dans l’espace de noms** <xref:Microsoft.SqlServer.Dts.Runtime>|Actuellement impossible|Oui|Oui|  

## <a name="execution-and-logging"></a>Exécution et journalisation  
 Les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peuvent être activés pour la journalisation et vous pouvez capturer des informations sur l'exécution dans des fichiers journaux. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
 Vous pouvez surveiller des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployés et exécutés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l’aide des rapports de fonctionnement. Les rapports sont disponibles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations, consultez [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).  
  
## <a name="run-a-package-in-sql-server-data-tools"></a>Exécuter un package dans les outils de données SQL Server
  Les packages sont exécutés le plus souvent dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pendant le développement, le débogage et le test des packages. Quand vous exécutez un package à partir du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , il est exécuté immédiatement.  
  
 Pendant qu’un package s’exécute, le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] affiche la progression de l’exécution sous l’onglet **Progression** . Vous pouvez afficher l'heure de début et de fin du package et de ses tâches et conteneurs, ainsi que des informations sur les tâches et les conteneurs du package ayant échoué. Quand un package a terminé son exécution, les informations sur l’exécution restent disponibles sous l’onglet **Résultats d’exécution** . Pour plus d’informations, consultez la section « Rapport de progression » dans la rubrique [Débogage du flux de contrôle](../../integration-services/troubleshooting/debugging-control-flow.md).  
  
 **Déploiement au moment du design**. Lorsqu'un package est exécuté dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], il est créé, puis déployé dans un dossier. Avant d'exécuter le package, vous pouvez spécifier le dossier dans lequel il est déployé. Si vous ne spécifiez aucun dossier, le dossier **bin** est utilisé par défaut. Ce type de déploiement est appelé déploiement au moment de la conception.  
  
### <a name="to-run-a-package-in-sql-server-data-tools"></a>Pour exécuter un package dans les outils de données SQL Server  
  
1.  Dans l’Explorateur de solutions, si votre solution contient plusieurs projets, cliquez avec le bouton droit sur le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient le package, puis cliquez sur **Définir en tant qu’objet de démarrage** pour définir le projet de démarrage.  
  
2.  Dans l’Explorateur de solutions, si votre projet contient plusieurs packages, cliquez avec le bouton droit sur un package, puis cliquez sur **Définir en tant qu’objet de démarrage** pour définir le package de démarrage.  
  
3.  Pour exécuter un package, utilisez l'une des procédures suivantes :  
  
    -   Ouvrez le package à exécuter, puis cliquez sur **Démarrer le débogage** dans la barre de menus ou appuyez sur F5. Une fois l'exécution du package terminée, appuyez sur Maj+F5 pour revenir au mode Création.  
  
    -   Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le package, puis cliquez sur **Exécuter le package**.  
  
### <a name="to-specify-a-different-folder-for-design-time-deployment"></a>Pour spécifier un dossier différent pour le déploiement au moment du design  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier de projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package à exécuter, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **\<Pages de propriétés de <nom_projet**, cliquez sur **Générer**.  
  
3.  Mettez à jour la valeur de la propriété OutputPath pour indiquer le dossier que vous souhaitez utiliser pour le déploiement au moment du design, puis cliquez sur **OK**.  


## <a name="run-a-package-on-the-ssis-server-using-sql-server-management-studio"></a>Exécuter un package sur le serveur SSIS à l’aide de SQL Server Management Studio
  Après avoir déployé votre projet sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous pouvez exécuter le package sur le serveur.  
  
 Vous pouvez utiliser des rapports d'opérations pour afficher des informations sur les packages qui ont été exécutés, ou qui sont actuellement exécutés, sur le serveur. Pour plus d'informations, consultez [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).  
  
### <a name="to-run-a-package-on-the-server-using-sql-server-management-studio"></a>Pour exécuter un package sur le serveur à l'aide de SQL Server Management Studio  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Dans l’Explorateur d’objets, développez le nœud **Catalogues Integration Services** , développez le nœud **SSISDB** , puis accédez au package contenu dans le projet que vous avez déployé.  
  
3.  Cliquez avec le bouton droit sur le nom du package, puis sélectionnez **Exécuter**.  
  
4.  Configurez l’exécution du package à l’aide des paramètres sous les onglets **Paramètres**, **Gestionnaires de connexions**et **Avancé** de la boîte de dialogue **Exécuter le package** .  
  
5.  Cliquez sur **OK** pour exécuter le package.  
  
     -ou-  
  
     Utilisez des procédures stockées pour exécuter le package. Cliquez sur **Script** pour générer l’instruction Transact-SQL qui crée et démarre une instance de l’exécution. L'instruction inclut un appel aux procédures stockées catalog.create_execution, catalog.set_execution_parameter_value et catalog.start_execution. Pour plus d’informations sur ces procédures stockées, consultez [catalog.create_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md), [catalog.set_execution_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) [catalog.start_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md).  

## <a name="execute_package_dialog"></a> Boîte de dialogue d’exécution de package
  Utilisez la boîte de dialogue **Exécuter le package** pour exécuter un package stocké sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peut contenir des paramètres qui référencent les valeurs stockées dans les variables d'environnement. Avant d'exécuter un tel package, vous devez spécifier quel environnement sera utilisé pour fournir les valeurs de variable d'environnement. Un projet peut contenir plusieurs environnements, mais un seul environnement peut être utilisé pour la liaison de valeurs de variable d'environnement au moment de l'exécution. Si aucune variable d'environnement n'est utilisée dans le package, un environnement n'est pas obligatoire.  
  
 Que voulez-vous faire ?  
  
-   [Ouvrir la boîte de dialogue Exécuter le package](#open_dialog)  
  
-   [Définir les options sur la page Général](#general)  
  
-   [Définir les options de l'onglet Paramètres](#parameters)  
  
-   [Définir les options de l'onglet Gestionnaires de connexions](#connection)  
  
-   [Définir les options de l'onglet Avancé](#advanced)  
  
-   [Création de script avec les options de la boîte de dialogue Exécuter le package](#script)  
  
###  <a name="open_dialog"></a> Ouvrir la boîte de dialogue Exécuter le package  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous vous connectez à l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données SSISDB.  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **SSISDB** .  
  
4.  Développez le dossier contenant le package à exécuter.  
  
5.  Cliquez avec le bouton droit sur le package, puis cliquez sur **Exécuter**.  
  
###  <a name="general"></a> Définir les options sur la page Général  
 Sélectionnez **Environnement** pour spécifier l'environnement qui est appliqué avec le package.  
  
###  <a name="parameters"></a> Définir les options de l'onglet Paramètres  
 Utilisez l'onglet **Paramètres** pour modifier les valeurs de paramètre utilisées lors de l'exécution du package.  
  
###  <a name="connection"></a> Définir les options de l'onglet Gestionnaires de connexions  
 Utilisez l'onglet Gestionnaires de connexions pour définir les propriétés du ou des gestionnaires de connexions du package.  
  
###  <a name="advanced"></a> Définir les options de l'onglet Avancé  
 Utilisez l'onglet Avancé pour gérer des propriétés et d'autres paramètres du package.  
  
 **Ajouter**, **Modifier**, **Supprimer**  
 Cliquez pour ajouter, modifier ou supprimer une propriété.  
  
 **Niveau de journalisation**  
 Sélectionnez le niveau de journalisation pour l'exécution du package. Pour plus d’informations, consultez [catalog.set_execution_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md).  
  
 **Vider en cas d'erreurs**  
 Spécifiez si un fichier de vidage est créé lorsque des erreurs se produisent pendant l'exécution du package. Pour plus d’informations, voir [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
 **Runtime 32 bits**  
 Indiquez que le package doit s'exécuter sur un système 32 bits.  
  
###  <a name="script"></a> Création de script avec les options de la boîte de dialogue Exécuter le package  
 Lorsque vous vous trouvez dans la boîte de dialogue **Exécuter le package** , vous pouvez également utiliser le bouton **Script** de la barre d'outils pour écrire du code [!INCLUDE[tsql](../../includes/tsql-md.md)] . Le script généré appelle les procédures stockées [catalog.start_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) avec les options que vous avez sélectionnées dans la boîte de dialogue **Exécuter le package**. Le script s'affiche dans une nouvelle fenêtre de script dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  

## <a name="see-also"></a> Voir aussi  
 [Utilitaire dtexec](../../integration-services/packages/dtexec-utility.md)   
[Démarrer l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md)
  
  
