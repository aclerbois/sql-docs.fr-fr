---
title: Constantes énumérées dans des expressions de propriété | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: expressions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- enumerators [Integration Services]
- packages [Integration Services], expressions
- dynamic properties
- updating package properties
- enumerated constants [Integration Services]
- property expressions [Integration Services]
ms.assetid: a4418315-38e2-4ad3-8784-576163b25d6f
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8fbdbef76153fa24bdf8b5eb6bd3d2999bb297ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enumerated-constants-in-property-expressions"></a>Constantes énumérées dans des expressions de propriété
  Si des expressions de propriété incluent des valeurs d'une liste de membres d'énumérateur, l'expression doit utiliser la valeur numérique du membre énumérateur et non le nom convivial du membre. Par exemple, si une expression définit la propriété **LoggingMode** , vous devez utiliser la valeur numérique 2 à la place du nom convivial Disabled.  
  
 Cette rubrique répertorie uniquement les valeurs numériques équivalant aux noms conviviaux d'énumérateurs dont les membres sont fréquemment utilisés dans des expressions de propriété. Le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclut de nombreux énumérateurs supplémentaires que vous utilisez quand vous programmez le modèle objet pour générer des packages par programmation ou des éléments de package de code personnalisé, tels que des tâches et des composants de flux de données.  
  
 En complément des propriétés personnalisées pour les packages et les objets package, la fenêtre Propriétés de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] inclut un jeu de propriétés accessibles aux packages, aux tâches, ainsi qu'aux conteneurs de boucles Foreach, de boucles For et de séquences. Les propriétés communes qui sont définies par des valeurs provenant d’énumérateurs (**ForceExecutionResult**, **LoggingMode**, **IsolationLevel**et **TransactionOption**) sont répertoriées dans la section Propriétés communes.  
  
 Les sections suivantes fournissent des informations sur les constantes énumérées :  
  
 [Package](#Package)  
  
 [Énumérateurs de boucles Foreach](#Foreach)  
  
 [Tâches](#Tasks)  
  
 [Tâches du plan de maintenance](#MaintenancePlanTasks)  
  
 [Propriétés communes](#CommonProperties)  
  
##  <a name="Package"></a> Package  
 Les tableaux suivants répertorient les noms conviviaux et les équivalents en valeur numérique pour des propriétés de packages que vous définissez à l'aide de valeurs provenant d'un énumérateur.  
  
 Propriété**PackageType** : définie à l’aide de valeurs provenant de l’énumération **DTSPackageType** .  
  
|Nom convivial dans DTSPackageType|Valeur numérique|  
|-------------------------------------|-------------------|  
|Par défaut|0|  
|DTSWizard| 1|  
|DTSDesigner|2|  
|SQLReplication|3|  
|DTSDesigner100|5|  
|SQLDBMaint|6|  
  
 Propriété**CheckpointUsage** : définie à l’aide de valeurs provenant de l’énumération **DTSCheckpointUsage** .  
  
|Nom convivial dans DTSCheckpointUsage|Valeur numérique|  
|-----------------------------------------|-------------------|  
|Never|0|  
|IfExists| 1|  
|Always|2|  
  
 Propriété**PackagePriorityClass** : définie à l’aide de valeurs provenant de l’énumération **DTSPriorityClass** .  
  
|Nom convivial dans DTSPriorityClass|Valeur numérique|  
|---------------------------------------|-------------------|  
|Par défaut|0|  
|AboveNormal| 1|  
|Normale|2|  
|BelowNormal|3|  
|Idle|4|  
  
 Propriété**ProtectionLevel** : définie à l’aide de valeurs provenant de l’énumération **DTSProtectionLevel** .  
  
|Nom convivial dans DTSProtectionLevel|Valeur numérique|  
|-----------------------------------------|-------------------|  
|DontSaveSensitive|0|  
|EncryptSensitiveWithUserKey| 1|  
|EncryptSensitiveWithPassword|2|  
|EncryptAllWithPassword|3|  
|EncryptAllWithUserKey|4|  
|ServerStorage|5|  
  
##  <a name="PrecedenceConstraints"></a> Contraintes de précédence  
 Propriété**EvalOp** : définie à l’aide de valeurs provenant de l’énumération **DTSPrecedenceEvalOp** .  
  
|Nom convivial dans DTSPrecedenceEvalOp|Valeur numérique|  
|------------------------------------------|-------------------|  
|Expression| 1|  
|Contrainte|2|  
|ExpressionAndConstraint|3|  
|ExpressionOrConstraint|4|  
  
 Propriété**Value** : définie à l’aide de valeurs provenant de l’énumération **DTSExecResult** .  
  
|Nom convivial|Valeur numérique|  
|-------------------|-------------------|  
|Réussi|0|  
|Failure| 1|  
|Completion|2|  
|Opération annulée|3|  
  
##  <a name="Foreach"></a> Énumérateurs de boucles Foreach  
 La boucle Foreach inclut un jeu d'énumérateurs comportant des propriétés pouvant être définies par des expressions de la propriété.  
  
### <a name="foreach-ado-enumerator"></a>Énumérateur Foreach ADO  
 Propriété**Type** : définie à l’aide de valeurs provenant de l’énumération **ADOEnumerationType** .  
  
|Nom convivial dans ADOEnumerationType|Valeur numérique|  
|-----------------------------------------|-------------------|  
|EnumerateTables|0|  
|EnumerateAllRows| 1|  
|EnumerateRowsInFirstTable|2|  
  
### <a name="foreach-nodelist-enumerator"></a>Énumérateur Foreach Nodelist  
 Propriétés**SourceDocumentType**, **InnerXPathStringSourceType**et **OuterXPathStringSourceType** : définies à l’aide de valeurs provenant de l’énumération **SourceType** .  
  
|Nom convivial dans SourceType|Valeur numérique|  
|---------------------------------|-------------------|  
|FileConnection|0|  
|Variable| 1|  
|DirectInput|2|  
  
 Propriété**EnumerationType** : définie à l’aide de valeurs provenant de l’énumération **EnumerationType** .  
  
|Nom convivial dans EnumerationType|Valeur numérique|  
|--------------------------------------|-------------------|  
|Navigator|0|  
|Nœud| 1|  
|NodeText|2|  
|ElementCollection|3|  
  
 Propriété**InnerElementType** : définie à l’aide de valeurs provenant de l’énumération **InnerElementType** .  
  
|Nom convivial dans InnerElementType|Valeur numérique|  
|---------------------------------------|-------------------|  
|Navigator|0|  
|Nœud| 1|  
|NodeText|2|  
  
##  <a name="Tasks"></a> Tâches  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclut de nombreuses tâches avec des propriétés pouvant être définies par des expressions de la propriété.  
  
### <a name="analysis-services-execute-ddl-task"></a>Tâche DDL d'exécution de SQL Server Analysis Services  
 Propriété**SourceType** : définie à l’aide de valeurs provenant de l’énumération **DDLSourceType** .  
  
|Nom convivial dans DDLSourceType|Valeur numérique|  
|------------------------------------|-------------------|  
|DirectInput|0|  
|FileConnection| 1|  
|Variable|2|  
  
### <a name="bulk-insert-task"></a>tâche d'insertion en bloc  
 Propriété**DataFileType** : définie à l’aide de valeurs provenant de l’énumération **DTSBulkInsert_DataFileType** .  
  
|Nom convivial dans DTSBulkInsert_DataFileType|Valeur numérique|  
|--------------------------------------------------|-------------------|  
|DTSBulkInsert_DataFileType_Char|0|  
|DTSBulkInsert_DataFileType_Native| 1|  
|DTSBulkInsert_DataFileType_WideChar|2|  
|DTSBulkInsert_DataFileType_WideNative|3|  
  
### <a name="execute-sql-task"></a>Tâche d’exécution de requêtes SQL  
 Propriété**ResultSetType** : définie à l’aide de valeurs provenant de l’énumération **ResultSetType** .  
  
|Nom convivial dans ResultSetType|Valeur numérique|  
|------------------------------------|-------------------|  
|ResultSetType_None| 1|  
|ResultSetType_SingleRow|2|  
|ResultSetType_Rowset|3|  
|ResultSetType_XML|4|  
  
 Propriété**SqlStatementSourceType** : définie à l’aide de valeurs provenant de l’énumération **SqlStatementSourceType** .  
  
|Nom convivial dans SqlStatementSourceType|Valeur numérique|  
|---------------------------------------------|-------------------|  
|DirectInput| 1|  
|FileConnection|2|  
|Variable|3|  
  
### <a name="file-system-task"></a>Tâches du système de fichiers  
 Propriété**Opération** : définie à l’aide de valeurs provenant de l’énumération **DTSFileSystemOpération** .  
  
|Nom convivial dans DTSFileSystemOperation|Valeur numérique|  
|---------------------------------------------|-------------------|  
|CopyFile|0|  
|MoveFile| 1|  
|DeleteFile|2|  
|RenameFile|3|  
|SetAttributes|4|  
|CreateDirectory|5|  
|CopyDirectory|6|  
|MoveDirectory|7|  
|DeleteDirectory|8|  
|DeleteDirectoryContent|9|  
  
 Propriété**Attributes** : définie à l’aide de valeurs provenant de l’énumération **DTSFileSystemAttributes** .  
  
|Nom convivial dans DTSFileSystemAttributes|Valeur numérique|  
|----------------------------------------------|-------------------|  
|Normale|0|  
|Archive| 1|  
|Hidden|2|  
|En lecture seule|4|  
|Système|8|  
  
### <a name="ftp-task"></a>Tâche FTP  
 Propriété**Operation** : définie à l’aide de valeurs provenant de l’énumération **DTSFTPOp** .  
  
|Nom convivial dans DTSFTPOp|Valeur numérique|  
|-------------------------------|-------------------|  
|Send|0|  
|Recevoir| 1|  
|DeleteLocal|2|  
|DeleteRemote|3|  
|MakeDirLocal|4|  
|MakeDirRemote|5|  
|RemoveDirLocal|6|  
|RemoveDirRemote|7|  
  
### <a name="message-queue-task"></a>Message Queue Task  
 Propriété**MessageType** : définie à l’aide de valeurs provenant de l’énumération **MQMessageType** .  
  
|Nom convivial dans MQMessageType|Valeur numérique|  
|------------------------------------|-------------------|  
|DTSMQMessageType_String|0|  
|DTSMQMessageType_DataFile| 1|  
|DTSMQMessageType_Variables|2|  
|DTSMQMessagType_StringMessageToVariable|3|  
  
 Propriété**StringCompareType** : définie à l’aide de valeurs provenant de l’énumération **MQStringMessageCompare** .  
  
|Nom convivial dans MQStringMessageCompare|Valeur numérique|  
|---------------------------------------------|-------------------|  
|DTSMQStringMessageCompare_None|0|  
|DTSMQStringMessageCompare_Exact| 1|  
|DTSMQStringMessageCompare_IgnoreCase|2|  
|DTSMQStringMessageCompare_Contains|3|  
  
 Propriété**TaskType** : définie à l’aide de valeurs provenant de l’énumération **MQType** .  
  
|Nom convivial dans MQType|Valeur numérique|  
|-----------------------------|-------------------|  
|DTSMQType_Sender|0|  
|DTSMQType_Receiver| 1|  
  
### <a name="send-mail-task"></a>tache Envoyer un message  
 Propriété**MessageSourceType** : définie à l’aide de valeurs provenant de l’énumération **SendMailMessageSourceType** .  
  
|Nom convivial dans SendMailMessageSourceType|Valeur numérique|  
|------------------------------------------------|-------------------|  
|DirectInput|0|  
|FileConnection| 1|  
|Variable|2|  
  
 Propriété**Priority** : définie à l’aide de valeurs provenant de l’énumération **MailPriority** .  
  
|Nom convivial dans MailPriority|Valeur numérique|  
|-----------------------------------|-------------------|  
|Élevée| 1|  
|Normale|3|  
|Faible|5|  
  
### <a name="transfer-database-task"></a>Tâche de transfert de bases de données  
 Propriété**Action** : définie à l’aide de valeurs provenant de l’énumération **TransferAction** .  
  
|Nom convivial dans TransferAction|Valeur numérique|  
|-------------------------------------|-------------------|  
|Copier|0|  
|Déplacer| 1|  
  
 Propriété**Method** : définie à l’aide de valeurs provenant de l’énumération **TransferMethod** .  
  
|Nom convivial dans TransferMethod|Valeur numérique|  
|-------------------------------------|-------------------|  
|DatabaseOffline|0|  
|DatabaseOnline| 1|  
  
### <a name="transfer-error-messages-task"></a>Tâche de transfert de messages d'erreur  
 Propriété**IfObjectExists** : définie à l’aide de valeurs provenant de l’énumération **IfObjectExists** .  
  
|Nom convivial dans IfObjectExists|Valeur numérique|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Remplacer| 1|  
|Ignorer|2|  
  
### <a name="transfer-jobs-task"></a>Tâche de transfert de travaux  
 Propriété**IfObjectExists** : définie à l’aide de valeurs provenant de l’énumération **IfObjectExists** .  
  
|Nom convivial dans IfObjectExists|Valeur numérique|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Remplacer| 1|  
|Ignorer|2|  
  
### <a name="transfer-logins-task"></a>Tâche de transfert de connexions  
 Propriété**IfObjectExists** : définie à l’aide de valeurs provenant de l’énumération **IfObjectExists** .  
  
|Nom convivial dans IfObjectExists|Valeur numérique|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Remplacer| 1|  
|Ignorer|2|  
  
 Propriété**LoginsToTransfer** : définie à l’aide de valeurs provenant de l’énumération **LoginsToTransfer** .  
  
|Nom convivial dans LoginsToTransfer|Valeur numérique|  
|---------------------------------------|-------------------|  
|AllLogins|0|  
|SelectedLogins| 1|  
|AllLoginsFromSelectedDatabases|2|  
  
### <a name="transfer-master-stored-procedures-task"></a>Tâche de transfert de procédures stockées de master  
 Propriété**IfObjectExists** : définie à l’aide de valeurs provenant de l’énumération **IfObjectExists** .  
  
|Nom convivial dans IfObjectExists|Valeur numérique|  
|-------------------------------------|-------------------|  
|FailTask|0|  
|Remplacer| 1|  
|Ignorer|2|  
  
### <a name="transfer-sql-server-objects-task"></a>Tâche de transfert d'objets SQL Server  
 Propriété**ExistingData** : définie à l’aide de valeurs provenant de l’énumération **ExistingData** .  
  
|Nom convivial dans ExistingData|Valeur numérique|  
|-----------------------------------|-------------------|  
|Remplacer|0|  
|Append| 1|  
  
### <a name="web-service-task"></a>Tâche de service Web  
 Propriété**OutputType** : définie à l’aide de valeurs provenant de l’énumération **DTSOutputType** .  
  
|Nom convivial dans DTSOutputType|Valeur numérique|  
|------------------------------------|-------------------|  
|Fichier|0|  
|Variable| 1|  
  
### <a name="wmi-data-reader-task"></a>Tâche Lecteur de données WMI  
 Propriété**OverwriteDestination** : définie à l’aide de valeurs provenant de l’énumération **OverwriteDestination** .  
  
|Nom convivial dans OverwriteDestination|Valeur numérique|  
|-------------------------------------------|-------------------|  
|OverwriteDestination|0|  
|AppendToDestination| 1|  
|KeepOriginal|2|  
  
 Propriété**OutputType** : définie à l’aide de valeurs provenant de l’énumération **OutputType** .  
  
|Nom convivial dans OutputType|Valeur numérique|  
|---------------------------------|-------------------|  
|DataTable|0|  
|PropertyValue| 1|  
|PropertyNameAndValue|2|  
  
 Propriété**DestinationType** : définie à l’aide de valeurs provenant de l’énumération **DestinationType** .  
  
|Nom convivial dans DestinationType|Valeur numérique|  
|--------------------------------------|-------------------|  
|FileConnection|0|  
|Variable| 1|  
  
 Propriété**WqlQuerySourceType** : définie à l’aide de valeurs provenant de l’énumération **QuerySourceType** .  
  
|Nom convivial dans QuerySourceType|Valeur numérique|  
|--------------------------------------|-------------------|  
|FileConnection|0|  
|DirectInput| 1|  
|Variable|2|  
  
 Propriété **ActionAtEvent** de l’Observateur d’événement WMI : définie à l’aide de valeurs provenant de l’énumération **ActionAtEvent** .  
  
|Nom convivial dans ActionAtEvent|Valeur numérique|  
|------------------------------------|-------------------|  
|LogTheEventAndFireDTSEvent|0|  
|LogTheEvent| 1|  
  
 Propriété**ActionAtTimeout** : définie à l’aide de valeurs provenant de l’énumération **ActionAtTimeout** .  
  
|Nom convivial dans ActionAtTimeout|Valeur numérique|  
|--------------------------------------|-------------------|  
|LogTimeoutAndFireDTSEvent|0|  
|LogTimeout| 1|  
  
 Propriété**AfterEvent** : définie à l’aide de valeurs provenant de l’énumération **AfterEvent** .  
  
|Nom convivial dans AfterEvent|Valeur numérique|  
|---------------------------------|-------------------|  
|ReturnWithSuccess|0|  
|ReturnWithFailure| 1|  
|WatchfortheEventAgain|2|  
  
 Propriété**AfterTimeout** : définie à l’aide de valeurs provenant de l’énumération **AfterTimeout** .  
  
|Nom convivial dans AfterTimeout|Valeur numérique|  
|-----------------------------------|-------------------|  
|ReturnWithSuccess|0|  
|ReturnWithFailure| 1|  
|WatchfortheEventAgain|2|  
  
 Propriété**WqlQuerySourceType** : définie à l’aide de valeurs provenant de l’énumération **QuerySourceType** .  
  
|Nom convivial dans QuerySourceType|Valeur numérique|  
|--------------------------------------|-------------------|  
|FileConnection|0|  
|DirectInput| 1|  
|Variable|2|  
  
### <a name="xml-task"></a>Tâche XML  
 Propriété**OperationType** : définie à l’aide de valeurs provenant de l’énumération **DTSXMLOperation** .  
  
|Nom convivial dans DTSXMLOperation|Valeur numérique|  
|--------------------------------------|-------------------|  
|Valider|0|  
|XSLT| 1|  
|XPATH|2|  
|Fusion|3|  
|Diff|4|  
|Patch|5|  
  
 Propriétés**SourceType**, **SecondOperandType**et **XPathSourceType** : définies à l’aide de valeurs provenant de l’énumération **DTSXMLSourceType** .  
  
|Nom convivial dans DTSXMLSourceType|Valeur numérique|  
|---------------------------------------|-------------------|  
|FileConnection|0|  
|Variable| 1|  
|DirectInput|2|  
  
 Propriétés**DestinationType** et **DiffGramDestinationType** : définies à l’aide de valeurs provenant de l’énumération **DTSXMLSaveResultTo** .  
  
|Nom convivial dans DTSXMLSaveResultTo|Valeur numérique|  
|-----------------------------------------|-------------------|  
|FileConnection|0|  
|Variable| 1|  
  
 Propriété**ValidationType** : définie à l’aide de valeurs provenant de l’énumération **DTSXMLValidationType** .  
  
|Nom convivial dans DTSXMLValidationType|Valeur numérique|  
|-------------------------------------------|-------------------|  
|DTD|0|  
|XSD| 1|  
  
 Propriété**XPathOperation** : définie à l’aide de valeurs provenant de l’énumération **DTSXMLXPathOperation** .  
  
|Nom convivial dans DTSXMLXPathOperation|Valeur numérique|  
|-------------------------------------------|-------------------|  
|Evaluation|0|  
|Valeurs| 1|  
|NodeList|2|  
  
 Propriété**DiffOptions** : définie à l’aide de valeurs provenant de l’énumération **DTSXMLDiffOptions** . Les options dans cet énumérateur ne sont pas mutuellement exclusives. Pour utiliser plusieurs options, fournissez une liste des options à appliquer, séparées par des virgules.  
  
|Nom convivial dans DTSXMLDiffOptions|Valeur numérique|  
|----------------------------------------|-------------------|  
|None|0|  
|IgnoreChildOrder| 1|  
|IgnoreComments|2|  
|IgnorePI|4|  
|IgnoreWhitespace|8|  
|IgnoreNamespaces|16|  
|IgnorePrefixes|32|  
|IgnoreXmlDecl|64|  
|IgnoreDtd|128|  
  
 Propriété**DiffAlgorithm** : définie à l’aide de valeurs provenant de l’énumération **DTSXMLDiffAlgorithm** .  
  
|Nom convivial dans DTSXMLDiffAlgorithm|Valeur numérique|  
|------------------------------------------|-------------------|  
|Auto|0|  
|Rapide| 1|  
|Précise|2|  
  
##  <a name="MaintenancePlanTasks"></a> Tâches du plan de maintenance  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclut un ensemble de tâches qui effectuent des tâches SQL Server à utiliser dans des plans de maintenance et dans des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge la gestion de ces tâches par programmation et la documentation de référence de programmation n’inclut pas la documentation API de ces tâches et de leurs énumérateurs.  
  
### <a name="all-maintenance-tasks"></a>Toutes les tâches de maintenance  
 Toutes les tâches de maintenance utilisent les énumérations suivantes pour définir les propriétés spécifiées.  
  
 Propriété**DatabaseSelectionType** : définie à l’aide de valeurs provenant de l’énumération **DatabaseSelection** .  
  
|Nom convivial dans DatabaseSelection|Valeur numérique|  
|----------------------------------------|-------------------|  
|None|0|  
|All| 1|  
|Système|2|  
|Utilisateur|3|  
|Specific|4|  
  
 Propriété**TableSelectionType** : définie à l’aide de valeurs provenant de l’énumération **TableSelection** .  
  
|Nom convivial dans TableSelection|Valeur numérique|  
|-------------------------------------|-------------------|  
|None|0|  
|All| 1|  
|Specific|2|  
  
 Propriété**ObjectTypeSelection** : définie à l’aide de valeurs provenant de l’énumération **ObjectType** .  
  
|Nom convivial dans ObjectType|Valeur numérique|  
|---------------------------------|-------------------|  
|Table de charge de travail|0|  
|Affichage| 1|  
|TableView|2|  
  
### <a name="back-up-database-task"></a>Tâche Sauvegarder la base de données  
 Propriété**DestinationCreationType** : définie à l’aide de valeurs provenant de l’énumération **DestinationType** .  
  
|Nom convivial dans DestinationType|Valeur numérique|  
|--------------------------------------|-------------------|  
|Auto|0|  
|Manuel| 1|  
  
 Propriété**ExistingBackupsAction** : définie à l’aide de valeurs provenant de l’énumération **ActionForExistingBackups** .  
  
|Nom convivial dans ActionForExistingBackups|Valeur numérique|  
|-----------------------------------------------|-------------------|  
|Ajouter|0|  
|Remplacer| 1|  
  
 Propriété**BackupAction** : définie à l’aide de valeurs provenant de l’énumération **BackupTaskType** . Cette propriété définit avec la propriété **BackupIsIncremental** le type de sauvegarde que la tâche effectue.  
  
|Nom convivial dans BackupTaskType|Valeur numérique|  
|-------------------------------------|-------------------|  
|Base de données|0|  
|Fichiers| 1|  
|Journal|2|  
  
 Propriété**BackupDevice** : définie à l’aide de valeurs provenant de l’énumération SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects) **DeviceType** .  
  
|Nom convivial dans DeviceType|Valeur numérique|  
|---------------------------------|-------------------|  
|LogicalDevice|0|  
|Bande| 1|  
|Fichier|2|  
|Pipe|3|  
|VirtualDevice|4|  
  
### <a name="maintenance-cleanup-task"></a>Tâche de nettoyage de maintenance  
 Propriété**FileTypeSelected** : définie à l’aide de valeurs provenant de l’énumération **FileType** .  
  
|Nom convivial dans FileType|Valeur numérique|  
|-------------------------------|-------------------|  
|FileBackup|0|  
|FileReport| 1|  
  
 Propriété**OlderThanTimeUnitType** : définie à l’aide de valeurs provenant de l’énumération **TimeUnitType** .  
  
|Nom convivial dans TimeUnitType|Valeur numérique|  
|-----------------------------------|-------------------|  
|Jour|0|  
|Week| 1|  
|Month|2|  
|Année|3|  
  
### <a name="update-statistics-task"></a>Tâche Mettre à jour les statistiques  
 Propriété**UpdateType** : définie à l’aide de valeurs provenant de l’énumération SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects) **StatisticsTarget** .  
  
|Nom convivial dans StatisticsTarget|Valeur numérique|  
|---------------------------------------|-------------------|  
|colonne| 1|  
|Index|2|  
|All|3|  
  
##  <a name="CommonProperties"></a> Propriétés communes  
 Les packages, les tâches et les conteneurs de boucles Foreach, de boucles For et de séquences peuvent utiliser les énumérations suivantes pour définir les propriétés spécifiées.  
  
 Propriété**ForceExecutionResult** : définie à l’aide de valeurs provenant de l’énumération **DTSForcedExecResult** .  
  
|Nom convivial dans DTSForcedExecResult|Valeur numérique|  
|------------------------------------------|-------------------|  
|None|-1|  
|Réussi|0|  
|Failure| 1|  
|Completion|2|  
  
 Propriété**IsolationLevel** : définie à l’aide de valeurs provenant de l’énumération **IsolationLevel** . Pour plus d’informations, consultez la bibliothèque de classes .NET Framework dans [MSDN Library](http://go.microsoft.com/fwlink?LinkId=17313).  
  
 Propriété**LoggingMode** : définie à l’aide de valeurs provenant de l’énumération **DTSLoggingMode** .  
  
|Nom convivial dans DTSLoggingMode|Valeur numérique|  
|-------------------------------------|-------------------|  
|UseParentSetting|0|  
|Activé| 1|  
|Désactivé|2|  
  
 Propriété**TransactionOption** : définie à l’aide de valeurs provenant de l’énumération **DTSTransactionOption** .  
  
|Nom convivial dans DTSTransactionOption|Valeur numérique|  
|-------------------------------------------|-------------------|  
|NotSupported|0|  
|Pris en charge| 1|  
|Requis|2|  
  
## <a name="related-tasks"></a>Related Tasks  
 [Ajouter ou modifier une expression de propriété](../../integration-services/expressions/add-or-change-a-property-expression.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md)   
 [Packages Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Conteneurs Integration Services](../../integration-services/control-flow/integration-services-containers.md)   
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Contraintes de précédence](../../integration-services/control-flow/precedence-constraints.md)  
  
  
