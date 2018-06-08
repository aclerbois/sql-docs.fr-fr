---
title: Transférer les messages d’erreur, tâche | Microsoft Docs
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
- sql13.dts.designer.transfererrormessagestask.f1
- sql13.dts.designer.transfererrormessagestask.general.f1
- sql13.dts.designer.transfererrormessagestask.errormessages.F1
helpviewer_keywords:
- Transfer Error Messages task [Integration Services]
ms.assetid: da702289-035a-4d14-bd74-04461fbfee1b
caps.latest.revision: 31
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 948f3fe8ae7603af9c64f21b974e97eea75da2b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-error-messages-task"></a>Tâche de transfert de messages d'erreur
  La tâche de transfert de messages d’erreur transfère un ou plusieurs messages d’erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définis par l’utilisateur entre des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les messages définis par l'utilisateur sont des messages avec un identificateur supérieur ou égal à 50 000. Les messages dont l'identificateur est inférieur à 50 000 sont des messages d'erreur système qui ne peuvent pas être transférés à l'aide de la tâche de transfert de messages d'erreur.  
  
 La tâche de transfert de messages d'erreur peut être configurée pour transférer tous les messages d'erreur ou uniquement les messages d'erreur spécifiés. Les messages d'erreur définis par l'utilisateur peuvent être disponibles en différentes langues et la tâche peut être configurée pour ne transférer que les messages dans des langues sélectionnées. Une version us_english du message qui utilise la page de codes 1033 doit exister sur le serveur de destination avant que vous ne puissiez transférer d'autres versions linguistiques du message vers ce serveur.  
  
 La table sysmessages dans la base de données master contient tous les messages d'erreur (système et définis par l'utilisateur) utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Les messages définis par l'utilisateur à transférer peuvent déjà exister à l'emplacement de destination. Un message d'erreur est défini comme message d'erreur dupliqué si l'identificateur et la langue sont identiques. La tâche de transfert de messages d'erreur peut être configurée pour traiter les messages d'erreur existants de différentes manières :  
  
-   Remplacer les messages d'erreur existants.  
  
-   Provoquer l'échec de la tâche lorsque des messages dupliqués existent.  
  
-   Ignorer les messages d'erreur dupliqués.  
  
 À l'exécution, la tâche de transfert de messages d'erreur se connecte aux serveurs source et destination en utilisant un ou deux gestionnaires de connexions SMO. Le gestionnaire de connexions SMO est configuré indépendamment de la tâche de transfert de messages d'erreur, puis il est référencé dans celle-ci. Le gestionnaire de connexions SMO spécifie le serveur et le mode d'authentification à utiliser lors de l'accès au serveur. Pour plus d'informations, consultez [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
 La tâche de transfert de messages d’erreur prend en charge une source et une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Chacune des versions peut être utilisée indifféremment comme source ou comme destination.  
  
## <a name="events"></a>Événements  
 La tâche génère un événement d'information qui indique le nombre de messages d'erreur transférés.  
  
 La tâche de transfert de messages d'erreur n'indique pas les stades intermédiaires de l'avancement du transfert des messages d'erreur : elle signale la tâche comme réalisée à 0 % ou à 100 %.  
  
## <a name="execution-value"></a>Valeur d'exécution  
 La valeur d’exécution, définie dans la propriété **ExecutionValue** de la tâche, retourne le nombre de messages d’erreur ayant été transférés. En affectant une variable définie par l’utilisateur à la propriété **ExecValueVariable** de la tâche de transfert de messages d’erreur, les informations sur le transfert de messages d’erreur peuvent être rendues disponibles aux autres objets du package. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
## <a name="log-entries"></a>Entrées du journal  
 La tâche de transfert de messages d'erreur comporte les entrées de journal personnalisées suivantes :  
  
-   TransferErrorMessagesTaskStartTransferringObjects    Cette entrée du journal indique que le transfert a commencé. L'entrée du journal inclut l'heure de début.  
  
-   TransferErrorMessagesTaskFinishedTransferringObjects   Cette entrée du journal indique que le transfert est terminé. L'entrée du journal inclut l'heure de fin.  
  
 De plus, une entrée de journal pour l’événement **OnInformation** indique le nombre de messages d’erreur qui ont été transférés, et une entrée de journal pour l’événement **OnWarning** est générée pour chaque message d’erreur remplacé à l’emplacement de destination.  
  
## <a name="security-and-permissions"></a>Sécurité et autorisations  
 Pour créer de nouveaux messages d'erreur, l'utilisateur qui exécute le package doit être un membre du rôle de serveur sysadmin ou serveradmin sur le serveur de destination.  
  
## <a name="configuration-of-the-transfer-error-messages-task"></a>Configuration de la tâche de transfert de messages d'erreur  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition par programmation de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.TransferErrorMessagesTask.TransferErrorMessagesTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="transfer-error-messages-task-editor-general-page"></a>Éditeur de tâche de transfert de messages d'erreur (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de transfert de messages d'erreur** pour donner un nom et une description à la tâche de transfert de messages d'erreur. La tâche de transfert de messages d’erreur transfère un ou plusieurs messages d’erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définis par l’utilisateur entre des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].   
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique à la tâche de transfert de messages d'erreur. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche de transfert de messages d'erreur.  
  
## <a name="transfer-error-messages-task-editor-messages-page"></a>Éditeur de tâche de transfert de messages d'erreur (page Messages)
  Utilisez la page **Messages** de la boîte de dialogue **Éditeur de tâche de transfert de messages d’erreur** pour spécifier les propriétés de copie d’un ou plusieurs messages d’erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définis par l’utilisateur d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à une autre. 
  
### <a name="options"></a>Options  
 **SourceConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur source.  
  
 **DestinationConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste, ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur de destination.  
  
 **IfObjectExists**  
 Déterminez si la tâche doit remplacer les messages d'erreur définis par l'utilisateur existants, ignorer les messages existants ou échouer si des messages d'erreur du même nom existent déjà sur le serveur de destination.  
  
 **TransferAllErrorMessages**  
 Déterminez si la tâche doit copier du serveur source au serveur de destination tous les messages définis par l'utilisateur ou seulement ceux spécifiés.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Copie tous les messages définis par l'utilisateur.|  
|**False**|Copie uniquement les messages définis par l'utilisateur spécifiés.|  
  
 **ErrorMessagesList**  
 Cliquez sur le bouton Parcourir **(…)** pour sélectionner les messages d’erreur à copier.  
  
> [!NOTE]  
>  Vous devez spécifier **SourceConnection** avant de pouvoir sélectionner les messages d’erreur à copier.  
  
 **ErrorMessageLanguagesList**  
 Cliquez sur le bouton Parcourir **(…)** pour sélectionner les langues pour lesquelles copier les messages d’erreur définis par l’utilisateur sur le serveur de destination. Une version us_english (page de codes 1033) du message doit exister sur le serveur de destination avant de pouvoir transférer vers ce serveur d'autres versions de langue du message.  
  
> [!NOTE]  
>  Vous devez spécifier **SourceConnection** avant de pouvoir sélectionner les messages d’erreur à copier.  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
  
