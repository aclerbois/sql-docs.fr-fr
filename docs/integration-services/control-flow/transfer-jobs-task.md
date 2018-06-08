---
title: Transfert de travaux, tâche | Microsoft Docs
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
- sql13.dts.designer.transferjobstask.f1
- sql13.dts.designer.transferjobstask.general.f1
- sql13.dts.designer.transferjobstask.jobs.f1
helpviewer_keywords:
- Transfer Jobs task [Integration Services]
ms.assetid: 1bf33885-9c5b-47e4-a549-f5920b66a1de
caps.latest.revision: 28
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d86f9b5ddd0456faa92348f6bce99edc37bb029b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-jobs-task"></a>Tâche de transfert de travaux
  La tâche de transfert de travaux transfère un ou plusieurs travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entre des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La tâche de transfert de travaux peut être configurée pour transférer tous les travaux ou seulement certains travaux spécifiés. Vous pouvez également spécifier si les travaux transférés sont activés lorsqu'ils arrivent à destination.  
  
 Les travaux à transférer peuvent déjà exister à l'emplacement de destination. La tâche de transfert de travaux peut être configurée pour traiter les travaux existants de différentes manières :  
  
-   Remplacer les travaux existants.  
  
-   Provoquer l'échec de la tâche lorsque des travaux dupliqués existent.  
  
-   Ignorer les travaux dupliqués.  
  
 À l'exécution, la tâche de transfert de travaux se connecte aux serveurs source et destination en utilisant un ou deux gestionnaires de connexions SMO. Le gestionnaire de connexions SMO est configuré indépendamment de la tâche de transfert de travaux, puis il est référencé dans celle-ci. Le gestionnaire de connexions SMO spécifie le serveur et le mode d'authentification à utiliser lors de l'accès au serveur. Pour plus d'informations, consultez [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
## <a name="transferring-jobs-between-instances-of-sql-server"></a>Transfert de travaux entre des instances de SQL Server  
 La tâche de transfert de travaux prend en charge une source et une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Chacune des versions peut être utilisée indifféremment comme source ou comme destination.  
  
## <a name="events"></a>Événements  
 La tâche de transfert de travaux génère un événement d'information qui indique le nombre de travaux transférés et un événement d'avertissement quand un travail est remplacé. La tâche n'indique pas les stades intermédiaires de l'avancement du transfert de travaux ; elle ne signale qu'une réalisation à 0 % ou à 100 %.  
  
## <a name="execution-value"></a>Valeur d'exécution  
 La valeur d'exécution, définie dans la propriété **ExecutionValue** de la tâche, renvoie le nombre de travaux transférés. En affectant une variable définie par l’utilisateur à la propriété **ExecValueVariable** de la tâche de transfert de travaux, les informations sur le transfert de travaux deviennent accessibles aux autres objets du package. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
## <a name="log-entries"></a>Entrées du journal  
 La tâche de transfert de travaux comporte les entrées du journal personnalisées suivantes :  
  
-   TransferJobsTaskStarTransferringObjects   Cette entrée du journal indique que le transfert a commencé. L'entrée du journal inclut l'heure de début.  
  
-   TransferJobsTaskFinishedTransferringObjects   Cette entrée du journal indique que le transfert est terminé. L'entrée du journal inclut l'heure de fin.  
  
 En outre, une entrée de journal pour l'événement **OnInformation** indique le nombre de travaux qui ont été transférés et une entrée de journal pour l'événement **OnWarning** est générée pour chaque travail remplacé à l'emplacement de destination.  
  
## <a name="security-and-permissions"></a>Sécurité et autorisations  
 Pour transférer des travaux, l'utilisateur doit être un membre du rôle serveur fixe sysadmin ou de l'un des rôles de base de données fixes de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur la base de données msdb à la fois sur les instances source et destination de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="configuration-of-the-transfer-jobs-task"></a>Configuration de la tâche de transfert de travaux  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition par programmation de ces propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.TransferJobsTask.TransferJobsTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="transfer-jobs-task-editor-general-page"></a>Éditeur de tâche de transfert de travaux (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de transfert de travaux** pour donner un nom et une description à la tâche de transfert de travaux.  
  
> [!NOTE]  
>  Seuls les membres du rôle serveur fixe **sysadmin** ou l'un des rôles de base de données fixe de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de destination peuvent y créer des travaux. Pour accéder à des travaux sur le serveur source, les utilisateurs doivent au moins y être membres du rôle de base de données fixe **SQLAgentUserRole** . Pour plus d’informations sur les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rôles de base de données fixe de l’Agent et leurs autorisations, consultez [Rôles de base de données fixe de l’Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique à la tâche de transfert de travaux. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche de transfert de travaux.  
  
## <a name="transfer-jobs-task-editor-jobs-page"></a>Éditeur de tâche de transfert de travaux (page Travaux)
  Utilisez la page **Travaux** de la boîte de dialogue **Éditeur de tâche de transfert de travaux** pour spécifier les propriétés de copie d’un ou plusieurs travaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à une autre.  
  
> [!NOTE]  
>  Pour accéder à des travaux sur le serveur source, les utilisateurs doivent au moins y être membres du rôle de base de données fixe **SQLAgentUserRole** . Pour créer des travaux sur le serveur de destination, l’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou de l’un des rôles de base de données fixe de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Pour plus d’informations sur les rôles de base de données fixe de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent et leurs autorisations, consultez [Rôles de base de données fixe de l’Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
### <a name="options"></a>Options  
 **SourceConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur source.  
  
 **DestinationConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste, ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur de destination.  
  
 **TransferAllJobs**  
 Déterminez si la tâche doit copier du serveur source au serveur de destination tous les travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou seulement ceux spécifiés.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Copie tous les travaux.|  
|**False**|Copie uniquement les travaux spécifiés.|  
  
 **JobsList**  
 Cliquez sur le bouton Parcourir **(…)** pour sélectionner les travaux à copier. Un travail au moins doit être sélectionné.  
  
> [!NOTE]  
>  Spécifiez **SourceConnection** avant de sélectionner les travaux à copier.  
  
 L’option **JobsList** n’est pas disponible quand **TransferAllJobs** a la valeur **True**.  
  
 **IfObjectExists**  
 Sélectionnez la façon dont la tâche doit gérer les travaux de même nom que ceux existant sur le serveur de destination.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**FailTask**|La tâche échoue si des travaux de même nom existent déjà sur le serveur de destination.|  
|**Remplacer**|La tâche remplace les travaux de même nom sur le serveur de destination.|  
|**Ignorer**|La tâche ignore les travaux de même nom qui existent sur le serveur de destination.|  
  
 **EnableJobsAtDestination**  
 Déterminez si les travaux copiés sur le serveur de destination doivent être activés.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Active les travaux sur le serveur de destination.|  
|**False**|Désactive les travaux sur le serveur de destination.|  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
  
