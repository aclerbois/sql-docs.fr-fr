---
title: Transfert de procédures stockées de master, tâche | Microsoft Docs
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
- sql13.dts.designer.transfermasterspstask.f1
- sql13.dts.designer.transferstoredprocedurestask.general.f1
- sql13.dts.designer.transferstoredprocedurestask.storedprocedures.f1
helpviewer_keywords:
- Transfer Master Stored Procedures task [Integration Services]
ms.assetid: 81702560-48a3-46d1-a469-e41304c7af8e
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 87639875b3ae15a514cb44ac5fd8d5fe8997188c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-master-stored-procedures-task"></a>Tâche de transfert de procédures stockées de master
  La tâche de transfert de procédures stockées de master transfère une ou plusieurs procédures stockées définies par l’utilisateur entre les bases de données **master** sur des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour transférer une procédure stockée à partir de la base de données **master** , le propriétaire de la procédure doit être dbo.  
  
 La tâche de transfert de procédures stockées de master peut être configurée pour transférer toutes les procédures stockées ou seulement des procédures spécifiées. Cette tâche ne copie pas les procédures stockées système.  
  
 Les procédures stockées de master à transférer peuvent déjà exister à l'emplacement de destination. La tâche de transfert de procédures stockées de master peut être configurée pour traiter les procédures stockées existantes de différentes façons :  
  
-   Remplacer les procédures stockées existantes.  
  
-   Provoquer l'échec de la tâche lorsque des procédures stockées dupliquées existent.  
  
-   Ignorer les procédures stockées dupliquées.  
  
 À l'exécution, la tâche de transfert des procédures stockées de master se connecte aux serveurs source et destination en utilisant deux gestionnaires de connexions SMO. Les gestionnaires de connexions SMO sont configurés indépendamment de la tâche de transfert des procédures stockées de master, puis référencés dans celle-ci. Les gestionnaires de connexions SMO spécifient le serveur et le mode d'authentification à utiliser lors de l'accès au serveur. Pour plus d'informations, consultez [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
## <a name="transferring-stored-procedures-between-instances-of-sql-server"></a>Transfert de procédures stockées entre des instances de SQL Server  
 La tâche de transfert de procédures stockées de master prend en charge une source et une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="events"></a>Événements  
 La tâche génère un événement d'information qui indique le nombre de procédures stockées transférées et un événement d'avertissement lorsque qu'une procédure stockée est remplacée.  
  
 La tâche de transfert des procédures stockées de master n'indique pas les stades intermédiaires de l'avancement du transfert des connexions : elle signale la tâche comme réalisée à 0 % ou à 100 %.  
  
## <a name="execution-value"></a>Valeur d'exécution  
 La valeur d’exécution, définie dans la propriété **ExecutionValue** de la tâche, renvoie le nombre de procédures stockées transférées. En affectant une variable définie par l’utilisateur à la propriété **ExecValueVariable** de la tâche de transfert des procédures stockées de master, les informations sur le transfert des procédures stockées peuvent être rendues disponibles aux autres objets du package. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
## <a name="log-entries"></a>Entrées du journal  
 La tâche de transfert des procédures stockées comporte les entrées de journal personnalisées suivantes :  
  
-   TransferStoredProceduresTaskStartTransferringObjects Cette entrée du journal indique que le transfert a commencé. L'entrée du journal inclut l'heure de début.  
  
-   TransferSStoredProceduresTaskFinishedTransferringObjects Cette entrée du journal indique que le transfert est terminé. L'entrée du journal inclut l'heure de fin.  
  
 En outre, une entrée de journal pour l’événement **OnInformation** indique le nombre de procédures stockées qui ont été transférées et une entrée de journal pour l’événement **OnWarning** est générée pour chaque procédure stockée remplacée à l’emplacement de destination.  
  
## <a name="security-and-permissions"></a>Sécurité et autorisations  
 L’utilisateur doit avoir l’autorisation d’afficher la liste des procédures stockées dans la base de données **master** sur la source et doit être un membre du rôle serveur sysadmin ou disposer de l’autorisation de créer des procédures stockées dans la base de données **master** sur le serveur de destination.  
  
## <a name="configuration-of-the-transfer-master-stored-procedures-task"></a>Configuration de la tâche de transfert de procédures stockées de master  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition par programmation de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.TransferStoredProceduresTask.TransferStoredProceduresTask>  
  
### <a name="configuring-the-transfer-master-stored-procedures-task-programmatically"></a>Configuration par programmation de la tâche de transfert de procédures stockées de master  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="transfer-master-stored-procedures-task-editor-general-page"></a>Éditeur de tâche de transfert de procédures stockées de master (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de transfert de procédures stockées de master** pour nommer et décrire la tâche de transfert de procédures stockées de master.  
  
> [!NOTE]  
>  Cette tâche transfère seulement les procédures stockées définies par l’utilisateur appartenant à **dbo** d’une base de données **MASTER** sur le serveur source vers une base de données **MASTER** sur le serveur de destination. Les utilisateurs doivent disposer de l’autorisation Créer une procédure dans la base de données **MASTER** du serveur de destination ou être membres du rôle serveur fixe **sysadmin** sur le serveur de destination pour y créer des procédures stockées.  
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique à la tâche de transfert de procédures stockées de master. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche de transfert de procédures stockées de master.  
  
## <a name="transfer-master-stored-procedures-task-editor-stored-procedures-page"></a>Éditeur de tâche de transfert de procédures stockées de master (page Procédures stockées)
  La page **Procédures stockées** de la boîte de dialogue **Éditeur de tâche de transfert de procédures stockées de master** permet de spécifier les propriétés de copie d’une ou plusieurs procédures stockées définies par l’utilisateur de la base de données **MASTER** d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers une base de données **MASTER** d’une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Cette tâche transfère seulement les procédures stockées définies par l’utilisateur appartenant à **dbo** d’une base de données **MASTER** sur le serveur source vers une base de données **MASTER** sur le serveur de destination. Les utilisateurs doivent disposer de l’autorisation Créer une procédure dans la base de données **MASTER** du serveur de destination ou être membres du rôle serveur fixe **sysadmin** sur le serveur de destination pour y créer des procédures stockées.  
  
### <a name="options"></a>Options  
 **SourceConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur source.  
  
 **DestinationConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste, ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur de destination.  
  
 **IfObjectExists**  
 Sélectionnez la façon dont la tâche doit traiter les procédures stockées définies par l’utilisateur qui existent déjà sous le même nom dans la base de données **MASTER** du serveur de destination.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**FailTask**|La tâche échoue si des procédures stockées du même nom existent déjà dans la base de données **MASTER** du serveur de destination.|  
|**Remplacer**|La tâche remplace les procédures stockées du même nom dans la base de données **MASTER** du serveur de destination.|  
|**Ignorer**|La tâche ignore les procédures stockées qui existent sous le même nom dans la base de données **MASTER** du serveur de destination.|  
  
 **TransferAllStoredProcedures**  
 Indiquez si toutes les procédures stockées définies par l’utilisateur dans la base de données **MASTER** sur le serveur source doivent être copiées sur le serveur de destination.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Copie toutes les procédures stockées définies par l’utilisateur dans la base de données **MASTER** .|  
|**False**|Copie uniquement les procédures stockées spécifiées.|  
  
 **StoredProceduresList**  
 Sélectionnez les procédures stockées définies par l’utilisateur dans la base de données **MASTER** du serveur source qui doivent être copiées dans la base de données **MASTER** de destination. Cette option est disponible uniquement lorsque **TransferAllStoredProcedures** a la valeur **False**.  
  
## <a name="see-also"></a> Voir aussi  
 [Tâche de transfert d'objets SQL Server](../../integration-services/control-flow/transfer-sql-server-objects-task.md)   
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
  
