---
title: Outils de dépannage pour l’exécution des packages | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Integration Services packages, troubleshooting
- SSIS packages, troubleshooting
- Integration Services, troubleshooting
- errors [Integration Services], troubleshooting
- packages [Integration Services], troubleshooting
ms.assetid: f18d6ff6-e881-444c-a399-730b52130e7c
caps.latest.revision: 59
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: eba1b10c2ebd776ddd03174e33529b03149605e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshooting-tools-for-package-execution"></a>Outils de dépannage pour l'exécution des packages
  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] intègre des fonctionnalités et des outils que vous pouvez utiliser pour résoudre des problèmes liés aux packages que vous exécutez après les avoir menés à terme et les avoir déployés.  
  
 Au moment de la conception, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] fournit des points d'arrêt permettant d'arrêter l'exécution des packages, une fenêtre Progression et des visionneuses de données à l'aide desquelles vous pouvez observer les données qui traversent le flux de données. Néanmoins, ces fonctionnalités ne sont pas disponibles lorsque vous exécutez des packages que vous avez déployés. Les principales techniques de dépannage des packages déployés sont les suivantes :  
  
-   Détection et traitement des erreurs liées aux packages par le biais de gestionnaires d'événements.  
  
-   Capture de données incorrectes à l'aide de sorties d'erreur.  
  
-   Suivi des étapes d'exécution des packages à l'aide de la fonction de journalisation.  
  
 Vous pouvez également suivre les conseils et les techniques suivantes pour éviter tout problème lors de l'exécution des packages :  
  
-   **Renforcez l'intégrité des données à l'aide de transactions**. Pour plus d’informations, consultez [Transactions Integration Services](../../integration-services/integration-services-transactions.md).  
  
-   **Utilisez des points de contrôle pour redémarrer les packages à compter du point de défaillance**. Pour plus d'informations, consultez [Redémarrer des packages à l'aide de points de contrôle](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
## <a name="catch-and-handle-package-errors-by-using-event-handlers"></a>Détecter et traiter les erreurs liées aux packages par le biais de gestionnaires d'événements  
 Vous pouvez répondre aux nombreux événements engagés par le package et les objets de ce dernier par le biais de gestionnaires d'événements.  
  
-   **Créez un gestionnaire d'événements pour l'événement OnError**. Dans le gestionnaire d'événements, vous pouvez recourir à une tâche Envoyer un message pour signaler l'échec à un administrateur, utiliser une tâche de script et une logique personnalisée pour recueillir des informations système à des fins de dépannage, ou bien nettoyer les ressources temporaires ou des résultats incomplets. Pour plus d’informations, consultez [Gestionnaires d’événements Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-event-handlers.md).  
  
## <a name="troubleshoot-bad-data-by-using-error-outputs"></a>Résoudre les problèmes liés à des données incorrectes à l'aide de sorties d'erreur  
 Vous pouvez exploiter la sortie d'erreur disponible dans nombre de composants de flux de données pour orienter les lignes contenant des erreurs vers une destination distincte en vue d'une analyse ultérieure. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).  
  
-   **Capturez les données incorrectes à l'aide de sorties d'erreur**. Transmettez les lignes qui contiennent des erreurs à une destination distincte, telle qu'une table d'erreurs ou un fichier texte. La sortie d'erreur ajoute automatiquement deux colonnes numériques renfermant le numéro de l'erreur responsable du rejet de la ligne, ainsi que l'ID de la colonne dans laquelle l'erreur est survenue.  
  
-   **Dotez les sorties d'erreur d'informations conviviales**. Vous pouvez faciliter le travail d’analyse de la sortie d’erreur en ajoutant le message d’erreur ainsi que le nom de la colonne, en plus des deux identificateurs numériques fournis par la sortie d’erreur. Pour découvrir un exemple montrant comment ajouter ces deux colonnes à l’aide de scripts, voir [Enhancing an Error Output with the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/enhancing-an-error-output-with-the-script-component.md).  
  
-   **Ou obtenez le nom des colonnes en enregistrant l’événement DiagnosticEx**. Cet événement consigne un mappage de lignage de flux de données dans le journal. Vous pouvez alors rechercher le nom de colonne dans ce mappage de lignage à l’aide de l’identificateur de colonne capturé par une sortie d’erreur.  Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).  
  
     La valeur de la colonne de message pour **DiagnosticEx** est du texte XML. Pour afficher le texte du message pour une exécution de package, interrogez la vue [catalog.operation_messages &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md). Notez que l’événement **DiagnosticEx** ne conserve pas l’espace blanc dans sa sortie XML afin réduire la taille du journal. Pour améliorer la lisibilité, copiez le journal dans un éditeur XML (dans Visual Studio, par exemple) prenant en charge la mise en forme XML et la mise en surbrillance de la syntaxe.  
  
## <a name="troubleshoot-package-execution-by-using-operations-reports"></a>Résoudre les problèmes liés à l'exécution des packages à l'aide de rapports d'opérations  
 Des rapports d'opérations standard sont disponibles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour vous aider à contrôler les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployés dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ces rapports de package vous aident à consulter l'état et l'historique du package et, si nécessaire, à identifier la cause des erreurs.  
  
 Pour plus d’informations, voir [Troubleshooting Reports for Package Execution](../../integration-services/troubleshooting/troubleshooting-reports-for-package-execution.md).  
  
## <a name="troubleshoot-package-execution-by-using-ssisdb-views"></a>Résoudre les problèmes liés à l'exécution des packages à l'aide de vues SSISDB  
 Vous pouvez interroger plusieurs vues de base de données SSISDB pour contrôler les informations relatives à l'exécution des packages et à d'autres opérations. Pour plus d’informations, consultez [Surveiller les packages en cours d’exécution et autres opérations](../../integration-services/performance/monitor-running-packages-and-other-operations.md).  
  
## <a name="troubleshoot-package-execution-by-using-logging"></a>Résoudre les problèmes liés à l'exécution des packages à l'aide de la journalisation  
 Vous pouvez contrôler la plupart des opérations réalisées dans vos packages en cours d'exécution en activant la fonction de journalisation. Les modules fournisseur d'informations permettent de capturer des informations sur des événements spécifiques à des fins d'analyse ultérieure, puis d'enregistrer ces informations dans une table de base de données, un fichier plat, un fichier XML ou un autre format de sortie pris en charge.  
  
-   **Activez la journalisation**. Vous pouvez affiner la sortie de journalisation en choisissant uniquement les événements et les éléments d'information que vous souhaitez capturer. Pour plus d’informations, consultez [Journalisation d’Integration Services (SSIS)](../performance/integration-services-ssis-logging.md).  
  
-   **Sélectionnez l'événement Diagnostic du package pour résoudre les problèmes inhérents au fournisseur.** Il existe des messages de journalisation qui vous permettent de résoudre les problèmes d'interaction d'un package avec des sources de données externes. Pour plus d’informations, voir [Troubleshooting Tools Package Connectivity](troubleshooting-tools-for-package-connectivity.md).  
  
-   **Améliorez la sortie de journalisation par défaut**. La journalisation ajoute généralement des lignes à la destination de journalisation à chaque exécution d'un package. Bien que chaque ligne de la sortie de journalisation identifie le package par son nom et son identificateur unique, ainsi que l'exécution du package par un ExecutionID unique, une grande partie de la sortie de journalisation au sein d'une seule liste peut s'avérer difficile à analyser.  
  
     L'approche suivante est une suggestion possible pour améliorer la sortie de journalisation par défaut et la rendre plus facile pour la génération de rapports :  
  
    1.  **Créez une table parent chargée de consigner chaque exécution d'un package**. Cette table parent dispose d'une seule ligne pour chaque exécution d'un package et utilise l'ExecutionID pour établir un lien avec les enregistrements enfants de la table de journalisation [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Vous pouvez exécuter une tâche d'exécution SQL au début de chaque package pour créer cette nouvelle ligne et enregistrer l'heure de début. Vous pouvez ensuite utiliser une autre tâche d'exécution SQL à la fin du package pour mettre à jour la ligne avec l'heure de fin, la durée et l'état.  
  
    2.  **Ajoutez des informations d'audit au flux de données**. Vous pouvez utiliser la transformation d'audit pour ajouter aux lignes du flux de données des informations sur l'exécution de package ayant entraîné la création ou la modification de chaque ligne. La transformation d'audit met neuf éléments d'information à disposition, notamment les variables PackageName et ExecutionInstanceGUID. Pour plus d’informations, voir [Audit Transformation](../../integration-services/data-flow/transformations/audit-transformation.md). Si vous disposez d'informations personnalisées que vous aimeriez inclure dans chaque ligne à des fins d'audit, vous pouvez les ajouter aux lignes dans le flux de données à l'aide d'une transformation de colonne dérivée. Pour plus d'informations, consultez [Transformation de colonne dérivée](../../integration-services/data-flow/transformations/derived-column-transformation.md).  
  
    3.  **Si possible, capturez les données sur le nombre de lignes**. Si possible, créez une table séparée pour les informations concernant le nombre de lignes dans laquelle chaque instance d'exécution de package est identifiée par son ExecutionID. Utilisez la transformation de calcul du nombre de lignes pour enregistrer le nombre de lignes dans une série de variables à des étapes critiques du flux de données. À la fin du flux de données, utilisez une tâche d'exécution SQL pour insérer la série de valeurs dans une ligne de la table en vue d'une analyse et d'un rapport ultérieurs.  
  
     Pour plus d'informations sur cette approche, consultez la section « ETL Auditing and Logging » dans le livre blanc [!INCLUDE[msCoName](../../includes/msconame-md.md)] [Project REAL: Business Intelligence ETL Design Practices](http://go.microsoft.com/fwlink/?LinkId=96602)(en anglais).  
  
## <a name="troubleshoot-package-execution-by-using-debug-dump-files"></a>Résoudre les problèmes liés à l'exécution des packages à l'aide de fichiers de vidage du débogage  
 Dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous pouvez créer des fichiers de vidage du débogage qui fourniront des informations sur l'exécution d'un package. Pour plus d’informations, voir [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
## <a name="troubleshoot-run-time-validation-issues"></a>Résoudre les problèmes de validation au moment de l'exécution  
 Il est possible, parfois, que vous ne parveniez pas à vous connecter à vos sources de données ou que des parties de votre package ne puissent pas être validées jusqu'à ce que les précédentes tâches du package aient été exécutées. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vous permettent d'éviter des erreurs de validation susceptibles de survenir dans ces conditions :  
  
-   **Configurez la propriété DelayValidation dans les éléments de package non valides lors du chargement du package**. Pour éviter des erreurs de validation lors du chargement du package, vous pouvez affecter à **DelayValidation** la valeur **True** dans des éléments de package dont la configuration n'est pas valide. Par exemple, vous pouvez disposer d'une tâche Flux de données qui utilise une table de destination qui n'existe pas jusqu'à ce qu'une tâche d'exécution SQL crée la table au moment de l'exécution. La propriété **DelayValidation** peut être activée au niveau du package ou au niveau des tâches individuelles et des conteneurs inclus dans le package.  
  
     La propriété **DelayValidation** peut être définie sur une tâche Flux de données mais pas sur des composants de flux de données individuels. Vous pouvez obtenir un résultat similaire en affectant la valeur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> à la propriété **ValidateExternalMetadata**. Néanmoins, si cette propriété affiche la valeur **false**, le composant n'a pas connaissance des modifications apportées aux métadonnées des sources de données externes. Quand la valeur **true**est définie, la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> peut permettre d’éviter des problèmes de blocage provoqués par un verrouillage dans la base de données, surtout quand le package utilise des transactions.  
  
## <a name="troubleshoot-run-time-permissions-issues"></a>Résoudre les problèmes d'autorisations au moment de l'exécution  
 Si vous rencontrez des erreurs lorsque vous tentez d'exécuter des packages déployés à l'aide de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il est possible que les comptes employés par ce dernier ne disposent pas des autorisations nécessaires. Pour plus d'informations sur la résolution des problèmes liés aux packages que vous exécutez à partir des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Un package SSIS n'est pas exécuté lorsque vous appelez le package SSIS à partir d'une étape de travail de SQL Server Agent](http://support.microsoft.com/kb/918760). Pour plus d’informations sur l’exécution de packages à partir des travaux de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Travaux de l’Agent SQL Server pour les packages](../../integration-services/packages/sql-server-agent-jobs-for-packages.md).  
  
 Pour se connecter à des sources de données Excel ou Access, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert un compte doté de l'autorisation de lire, écrire, créer et supprimer des fichiers temporaires dans le dossier spécifié par les variables d'environnement TEMP et TMP.  
  
## <a name="troubleshoot-64-bit-issues"></a>Résoudre les problèmes liés à la version 64 bits  
  
-   **Certains fournisseurs de données ne sont pas disponibles sur la plateforme 64 bits**. C’est notamment le cas du fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet OLE DB, qui est nécessaire pour se connecter à des sources de données Excel ou Access : il n’est pas disponible dans une version 64 bits.  
  
## <a name="troubleshoot-errors-without-a-description"></a>Résoudre les erreurs sans description  
 Si vous rencontrez une erreur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sans description qui l'accompagne, vous pouvez localiser cette dernière dans le [Guide de référence des erreurs et des messages propres à Integration Services](../../integration-services/integration-services-error-and-message-reference.md) en recherchant l'erreur d'après son numéro. La liste ne comporte actuellement aucune information de dépannage.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Débogage d’un flux de données](../../integration-services/troubleshooting/debugging-data-flow.md)  
  
## <a name="related-content"></a>Contenu associé  
 Entrée de blog, [Ajout du nom de la colonne d'erreur à une sortie d'erreur](http://go.microsoft.com/fwlink/?LinkId=261546)sur dougbert.com.  
