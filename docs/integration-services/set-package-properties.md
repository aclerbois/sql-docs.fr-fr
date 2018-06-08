---
title: Définir les propriétés d’un package | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Integration Services packages, properties
- properties [Integration Services]
- checkpoints [Integration Services]
- execution properties [Integration Services]
- packages [Integration Services], properties
- identification properties [Integration Services]
- passwords [Integration Services]
- SSIS packages, properties
- transaction properties [Integration Services]
- updating package properties
- forced execution value properties [Integration Services]
- security properties [Integration Services]
- version properties [Integration Services]
- SQL Server Integration Services packages, properties
ms.assetid: 13f81c3e-2b18-4f83-b445-a2f4a2c560aa
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 841f4d7bf5b77ccc91731276510b1b3186c1bb90
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-package-properties"></a>Définir les propriétés d'un package
  Lorsque vous créez un package dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] à l'aide de l'interface graphique fournie par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , vous définissez les propriétés de l'objet de package dans la fenêtre Propriétés.  
  
 La fenêtre **Propriétés** affiche une liste de propriétés triées par catégorie et par ordre alphabétique. Pour réorganiser la fenêtre **Propriétés** par catégorie, cliquez sur l'icône Catégories.  
  
 Dans cette organisation, la fenêtre **Propriétés** regroupe les propriétés dans les catégories suivantes :  
  
-   [Points de contrôle](#Checkpoints)  
  
-   [Exécution](#Execution)  
  
-   [Valeur d'exécution forcée](#ForcedExecutionValue)  
  
-   [Identification](#Identification)  
  
-   [Divers](#Misc)  
  
-   [Sécurité](#Security)  
  
-   [Transactions](#Transactions)  
  
-   [Version](#Version)  
  
 Pour plus d’informations sur les propriétés d’un package que vous ne pouvez pas définir dans la fenêtre **Propriétés** , consultez <xref:Microsoft.SqlServer.Dts.Runtime.Package>.  
  
### <a name="to-set-package-properties-in-the-properties-window"></a>Pour définir les propriétés d'un package dans la fenêtre Propriétés  
  
-   [Définir les propriétés d’un package](http://msdn.microsoft.com/library/0d20346e-475c-412f-b3ff-7bce25242b7a)  
  
## <a name="properties-by-category"></a>Propriétés par catégorie  
 Les tableaux qui suivent énumèrent les propriétés d'un package par catégorie.  
  
###  <a name="Checkpoints"></a> Points de contrôle  
 Vous pouvez utiliser les propriétés de cette catégorie pour redémarrer le package à partir d'un point d'échec dans le flux de contrôle du package, au lieu de réexécuter le package depuis le début de son flux de contrôle. Pour plus d'informations, consultez [Redémarrer des packages à l'aide de points de contrôle](../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
|Propriété|Description|  
|--------------|-----------------|  
|**CheckpointFileName**|Nom du fichier qui capture les informations de point de contrôle permettant à un package de redémarrer. Une fois le package terminé avec succès, le fichier est supprimé.|  
|**CheckpointUsage**|Indique à quel moment un package peut être redémarré. Ces valeurs sont **Never**, **IfExists**et **Always**. La valeur par défaut est **Never**pour indiquer que le package ne peut pas être redémarré. Pour plus d'informations, consultez <xref:Microsoft.SqlServer.Dts.Runtime.DTSCheckpointUsage>.|  
|**SaveCheckpoints**|Indique si les points de contrôle sont écrits dans le fichier de point de contrôle lors de l'exécution du package. La valeur par défaut de cette propriété est **False**.|  
  
> [!NOTE]  
>  L’option **/CheckPointing on** de dtexec revient à définir la propriété **SaveCheckpoints** du package sur True, et la propriété **CheckpointUsage** sur Always. Pour plus d’informations, voir [dtexec Utility](../integration-services/packages/dtexec-utility.md).  
  
###  <a name="Execution"></a> Exécution  
 Les propriétés de cette catégorie permettent de configurer le comportement de l'objet de package au moment de l'exécution.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**DelayValidation**|Indique si la validation du package est retardée jusqu'à l'exécution du package. La valeur par défaut de cette propriété est **False**.|  
|**Désactiver**|Indique si le package est désactivé. La valeur par défaut de cette propriété est **False**.|  
|**DisableEventHandlers**|Indique si les gestionnaires d'événements du package sont exécutés. La valeur par défaut de cette propriété est **False**.|  
|**FailPackageOnFailure**|Indique si le package échoue en cas d'erreur dans un composant du package. La seule valeur possible pour cette propriété est **False**.|  
|**FailParentOnError**|Indique si le conteneur parent échoue en cas d'erreur dans un conteneur enfant. La valeur par défaut de cette propriété est **False**.|  
|**MaxConcurrentExecutables**|Nombre de fichiers exécutables pouvant être exécutés simultanément par le package. La valeur par défaut de cette propriété est **-1**, ce qui indique qu’aucune limite n’est appliquée.|  
|**MaximumErrorCount**|Nombre maximal d'erreurs pouvant se produire avant arrêt de l'exécution d'un package. La valeur par défaut de cette propriété est **1**.|  
|**PackagePriorityClass**|Classe de priorité du thread Win32 du package. Cette propriété peut prendre les valeurs **Default**, **AboveNormal**, **Normal**, **BelowNormal**et **Idle**. La valeur par défaut de cette propriété est **Default**. Pour plus d'informations, consultez <xref:Microsoft.SqlServer.Dts.Runtime.DTSPriorityClass>.|  
  
###  <a name="ForcedExecutionValue"></a> Valeur d'exécution forcée  
 Les propriétés de cette catégorie permettent de configurer une valeur d'exécution facultative pour le package.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**ForcedExecutionValue**|Si ForceExecutionValue a la valeur **True**, valeur indiquant la valeur d’exécution facultative retournée par le package. La valeur par défaut de cette propriété est **0**.|  
|**ForcedExecutionValueType**|Type de données de ForcedExecutionValue. La valeur par défaut de cette propriété est **Int32**.|  
|**ForceExecutionValue**|Valeur booléenne qui indique si la valeur d'exécution facultative du conteneur doit être forcée de contenir une valeur particulière. La valeur par défaut de cette propriété est **False**.|  
  
###  <a name="Identification"></a> Identification  
 Les propriétés de cette catégorie fournissent des informations telles que l'identificateur unique et le nom du package.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**CreationDate**|Date de création du package.|  
|**CreatorComputerName**|Nom de l'ordinateur sur lequel le package a été créé.|  
|**CreatorName**|Nom de la personne qui a créé le package.|  
|**Description**|Description des fonctionnalités du package.|  
|**ID**|Identificateur global unique du package, affecté lors de la création du package. Cette propriété est en lecture seule. Pour générer une nouvelle valeur aléatoire pour la propriété **ID**, sélectionnez **\<Générer un nouvel ID\>** dans la liste déroulante.|  
|**Nom**|Nom du package.|  
|**PackageType**|Type de package. Les valeurs possibles sont **Default**, **DTSDesigner**, **DTSDesigner100**, **DTSWizard**, **SQLDBMaint**et **SQLReplication**. La valeur par défaut de cette propriété est **Default**. Pour plus d'informations, consultez <xref:Microsoft.SqlServer.Dts.Runtime.DTSPackageType>.|  
  
###  <a name="Misc"></a> Divers  
 Les propriétés de cette catégorie sont utilisées pour accéder aux configurations et aux expressions utilisées par un package et pour fournir des informations sur les paramètres régionaux et le mode de journalisation du package. Pour plus d’informations, consultez [Expressions de propriété dans des packages](../integration-services/expressions/use-property-expressions-in-packages.md).  
  
|Propriété|Description|  
|--------------|-----------------|  
|**Configurations**|Ensemble de configurations utilisées par le package. Cliquez sur le bouton Parcourir **(…)** pour afficher et configurer les configurations du package.|  
|**Expressions**|Cliquez sur le bouton Parcourir **(…)** pour créer des expressions pour les propriétés du package.<br /><br /> Remarque : vous pouvez créer des expressions pour toutes les propriétés du package incluses dans le modèle d’objet, et non seulement pour les propriétés énumérées dans la fenêtre Propriétés.<br /><br /> Pour plus d’informations, consultez [Expressions de propriété dans des packages](../integration-services/expressions/use-property-expressions-in-packages.md).<br /><br /> Pour afficher les expressions de propriétés existantes, développez **Expressions**. Cliquez sur le bouton **(…)** dans la zone de texte d’une expression pour modifier et évaluer une expression.|  
|**ForceExecutionResult**|Résultat d'exécution du package. Cette propriété peut prendre les valeurs **None**, **Success**, **Failure**et **Completion**. La valeur par défaut de cette propriété est **None**. Pour plus d’informations, consultez T:Microsoft.SqlServer.Dts.Runtime.DTSForcedExecResult.|  
|**LocaleId**|Paramètre régional Microsoft Win32. La valeur par défaut de cette propriété est le paramètre régional du système d'exploitation sur l'ordinateur local.|  
|**LoggingMode**|Valeur qui indique le comportement de journalisation du package. Ces valeurs sont **Disabled**, **Enabled**et **UseParentSetting**. La valeur par défaut de cette propriété est **UseParentSetting**. Pour plus d'informations, consultez <xref:Microsoft.SqlServer.Dts.Runtime.DTSLoggingMode>.|  
|**OfflineMode**|Indique si le package est en mode hors connexion. Cette propriété est en lecture seule. Elle est définie au niveau du projet. Normalement, le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] tente de se connecter à chaque source de données utilisée par votre package pour valider les métadonnées associées aux sources et aux destinations. Vous pouvez activer l'option **Travailler hors connexion** du menu **SSIS** , avant même d'ouvrir un package, pour empêcher ces tentatives de connexion et les erreurs de validation qui en résultent lorsque les sources de données ne sont pas disponibles. Vous pouvez également activer l’option **Travailler hors connexion** pour accélérer les opérations exécutées dans le concepteur, puis la désactiver dès que vous souhaitez valider votre package.|  
|**SuppressConfigurationWarnings**|Indique sur les avertissements générés par les configurations sont supprimés. La valeur par défaut de cette propriété est **False**.|  
|**UpdateObjects**|Indique si le package est mis à jour pour utiliser des versions plus récentes des objets qu'il contient, si des versions plus récentes sont disponibles. Par exemple, si cette propriété a la valeur **True**, un package incluant une tâche d’insertion en bloc est mis à jour pour utiliser la version plus récente de cette tâche fournie par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . La valeur par défaut de cette propriété est **False**.|  
  
###  <a name="Security"></a> Sécurité  
 Les propriétés de cette catégorie sont utilisées pour définir le niveau de protection du package. Pour plus d'informations, consultez [Access Control for Sensitive Data in Packages](../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
|Propriété|Description|  
|--------------|-----------------|  
|**PackagePassword**|Mot de passe pour les niveaux de protection de package (**EncryptSensitiveWithPassword** et **EncryptAllWithPassword**) qui nécessitent des mots de passe.|  
|**ProtectionLevel**|Niveau de protection du package. Cette propriété peut prendre les valeurs **DontSaveSensitive**, **EncryptSensitiveWithUserKey**, **EncryptSensitiveWithPassword**, **EncryptAllWithPassword**et **ServerStorage**. La valeur par défaut de cette propriété est **EncryptSensitiveWithUserKey**. Pour plus d'informations, consultez <xref:Microsoft.SqlServer.Dts.Runtime.DTSProtectionLevel>.|  
  
###  <a name="Transactions"></a> Transactions  
 Les propriétés de cette catégorie permettent de configurer le niveau d'isolement et l'option de transaction du package. Pour plus d’informations, consultez [Transactions Integration Services](../integration-services/integration-services-transactions.md).  
  
|Propriété|Description|  
|--------------|-----------------|  
|**IsolationLevel**|Niveau d'isolement de la transaction sur package. Cette propriété peut prendre les valeurs **Unspecified**, **Chaos**, **ReadUncommitted**, **ReadCommitted**, **RepeatableRead**, **Serializable**et **Snapshot**. La valeur par défaut de cette propriété est **Serializable**.<br /><br /> Remarque : la valeur **Snapshot** de la propriété **IsolationLevel** est incompatible avec les transactions de package. Cependant, vous ne pouvez pas utiliser la propriété **IsolationLevel** pour définir le niveau d'isolation des transactions de package pour **Shapshot**. Utilisez une requête SQL pour définir les transactions de package à **Snapshot**. Pour plus d’informations, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).<br /><br /> Le système applique la propriété **IsolationLevel** aux transactions de package uniquement lorsque la propriété **TransactionOption** a la valeur **Required**.<br /><br /> La valeur de la propriété **IsolationLevel** demandée par un conteneur enfant est ignorée lorsque les conditions suivantes sont remplies :<br />La valeur de la propriété **TransactionOption** du conteneur enfant est **Supported**.<br />Le conteneur enfant rejoint la transaction d'un conteneur parent.<br /><br /> La valeur de la propriété **IsolationLevel** demandée par le conteneur est respectée uniquement lorsque le conteneur lance une nouvelle transaction. Un conteneur lance une nouvelle transaction lorsque les conditions suivantes sont remplies :<br />La valeur de la propriété **TransactionOption** du conteneur est **Required**.<br />Le parent n'a pas déjà démarré de transaction.<br /><br /> <br /><br /> Pour plus d'informations, consultez <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.IsolationLevel%2A>.|  
|**TransactionOption**|Participation transactionnelle du package. Cette propriété peut prendre les valeurs **NotSupported**, **Supported**et **Required**. La valeur par défaut de cette propriété est **Supported**. Pour plus d'informations, consultez <xref:Microsoft.SqlServer.Dts.Runtime.DTSTransactionOption>.|  
  
###  <a name="Version"></a> Version  
 Les propriétés de cette catégorie fournissent des informations sur la version de l'objet de package.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**VersionBuild**|Numéro de version de la build du package.|  
|**VersionComments**|Commentaires sur la version du package.|  
|**VersionGUID**|Identificateur global unique de la version du package. Cette propriété est en lecture seule.|  
|**VersionMajor**|Dernière version majeure du package.|  
|**VersionMinor**|Dernière version mineure du package.|  

## <a name="set-package-properties-in-the-properties-window"></a>Définir les propriétés d’un package dans la fenêtre Propriétés 
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package à configurer.  
  
2.  Dans **l’Explorateur de solutions**, double-cliquez sur le package pour l’ouvrir dans le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , ou cliquez avec le bouton droit et sélectionnez **Concepteur de vues**.  
  
3.  Cliquez sur l’onglet **Flux de contrôle** , puis effectuez l’une des opérations suivantes :  
  
    -   Cliquez avec le bouton droit n’importe où dans l’arrière-plan de la surface de dessin du flux de contrôle, puis cliquez sur **Propriétés**.  
  
    -   Dans le menu **Affichage** , cliquez sur **Fenêtre Propriétés**.  
  
4.  Modifiez les propriétés du package dans la fenêtre **Propriétés** .  
  
5.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés** .  
  
