---
title: Transférer des connexions, tâche | Microsoft Docs
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
- sql13.dts.designer.transferloginstask.f1
- sql13.dts.designer.transferloginstask.general.f1
- sql13.dts.designer.transferloginstask.logins.f1
helpviewer_keywords:
- Transfer Logins task [Integration Services]
ms.assetid: 1df60fd6-c019-405d-8155-c330dbac2cc1
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 748eecc0759365057c1dbb2c2610e6307faf6a68
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-logins-task"></a>Tâche de transfert de connexions
  La tâche de transfert de connexions transfère une ou plusieurs connexions entre des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="transfer-logins-between-instances-of-sql-server"></a>Transférer des connexions entre des instances de SQL Server  
 La tâche de transfert de connexions prend en charge une source et une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="events"></a>Événements  
 La tâche génère un événement d'information qui indique le nombre de connexions transférées et un événement d'avertissement quand une connexion est remplacée.  
  
 La tâche de transfert des connexions n'indique pas les stades intermédiaires de l'avancement du transfert des connexions : elle signale la tâche comme réalisée à 0 % ou à 100 %.  
  
## <a name="execution-value"></a>Valeur d'exécution  
 La valeur d’exécution, définie dans la propriété **ExecutionValue** de la tâche, retourne le nombre de connexions transférées. En affectant une variable définie par l’utilisateur à la propriété **ExecValueVariable** de la tâche de transfert de connexions, les informations sur le transfert des connexions peuvent être rendues disponibles pour d’autres objets du package. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
## <a name="log-entries"></a>Entrées du journal  
 La tâche de transfert des connexions comporte les entrées du journal personnalisées suivantes :  
  
-   TransferLoginsTaskStarTransferringObjects   Cette entrée du journal indique que le transfert a commencé. L'entrée du journal inclut l'heure de début.  
  
-   TransferLoginsTaskFinishedTransferringObjects   Cette entrée du journal indique que le transfert est terminé. L'entrée du journal inclut l'heure de fin.  
  
 De plus, une entrée de journal pour l’événement **OnInformation** indique le nombre de connexions qui ont été transférées, et une entrée de journal pour l’événement **OnWarning** est générée pour chaque connexion remplacée à l’emplacement de destination.  
  
## <a name="security-and-permissions"></a>Sécurité et autorisations  
 Pour parcourir les connexions sur le serveur source et créer des connexions sur le serveur de destination, l'utilisateur doit être un membre du rôle de serveur sysadmin sur les deux serveurs.  
  
## <a name="configuration-of-the-transfer-logins-task"></a>Configuration de la tâche de transfert des connexions  
 La tâche de transfert de connexions peut être configurée pour transférer toutes les connexions, seulement les connexions spécifiées ou toutes les connexions ayant accès à une base de données spécifiée. La connexion sa ne peut pas être transférée. La connexion sa peut être renommée ; cependant, la connexion sa renommée ne peut pas être transférée non plus.  
  
 Vous pouvez également indiquer si la tâche copie les identificateurs de sécurité (SID) associés aux connexions. Si la tâche de transfert des connexions est utilisée en conjonction avec la tâche de transfert des bases de données, les SID doivent être copiés à l'emplacement de destination ; si ce n'est pas le cas, les connexions transférées ne sont pas reconnues par la base de données de destination.  
  
 À l'emplacement de destination, les connexions transférées sont désactivées et des mots de passe aléatoires leur sont affectés. Un membre du rôle sysadmin sur le serveur de destination doit modifier les mots de passe et activer les connexions avant que ces dernières puissent être utilisées.  
  
 Les connexions à transférer peuvent déjà exister à l'emplacement de destination. La tâche de transfert des connexions peut être configurée pour traiter les connexions existantes de différentes manières :  
  
-   Remplacer les connexions existantes  
  
-   Provoquer l'échec de la tâche lorsque des connexions dupliquées existent.  
  
-   Ignorer les connexions dupliquées  
  
 À l'exécution, la tâche de transfert des connexions se connecte aux serveurs source et destination en utilisant deux gestionnaires de connexions SMO. Les gestionnaires de connexions SMO sont configurés indépendamment de la tâche de transfert des connexions, puis référencés dans celle-ci. Les gestionnaires de connexions SMO spécifient le serveur et le mode d'authentification à utiliser lors de l'accès au serveur. Pour plus d'informations, consultez [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="programmatic-configuration-of-the-transfer-logins-task"></a>Configuration de la tâche de transfert des connexions par programme  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.TransferLoginsTask.TransferLoginsTask>  
  
## <a name="transfer-logins-task-editor-general-page"></a>Éditeur de tâche de transfert de connexions (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de transfert de connexions** pour donner un nom et une description à la tâche de transfert de connexions.  
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique à la tâche de transfert de connexions. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche de transfert de connexions.  
  
## <a name="transfer-logins-task-editor-logins-page"></a>Éditeur de tâche de transfert de connexions (page Connexions)
  Utilisez la page **Connexions** de la boîte de dialogue **Éditeur de tâche de transfert de connexions** pour spécifier les propriétés de copie d'une ou plusieurs connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à une autre.  
  
> [!IMPORTANT]  
>  Lors de l'exécution de la tâche de transfert de connexions, les connexions sont créées sur le serveur de destination avec des mots de passe aléatoires qui sont désactivés. Pour utiliser ces connexions, un membre du rôle serveur fixe **sysadmin** doit changer les mots de passe et les activer. La connexion **sa** ne peut pas être transférée.  
  
### <a name="options"></a>Options  
 **SourceConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur source.  
  
 **DestinationConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste, ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur de destination.  
  
 **LoginsToTransfer**  
 Sélectionnez les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à copier du serveur source au serveur de destination. Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**AllLogins**|Toutes les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur source seront copiées sur le serveur de destination.|  
|**SelectedLogins**|Seules les connexions spécifiées dans **LoginsList** seront copiées sur le serveur de destination.|  
|**AllLoginsFromSelectedDatabases**|Toutes les connexions des bases de données spécifiées dans **DatabasesList** seront copiées sur le serveur de destination.|  
  
 **LoginsList**  
 Sélectionnez les connexions sur le serveur source à copier sur le serveur de destination. Cette option est disponible uniquement lorsque **SelectedLogins** est sélectionné pour **LoginsToTransfer**.  
  
 **DatabasesList**  
 Sélectionnez les bases de données du serveur source qui contiennent les connexions à copier sur le serveur de destination. Cette option est disponible uniquement lorsque **AllLoginsFromSelectedDatabases** est sélectionné pour **LoginsToTransfer**.  
  
 **IfObjectExists**  
 Sélectionnez la façon dont la tâche doit gérer les connexions de même nom que celles existant sur le serveur de destination.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**FailTask**|La tâche échoue si des connexions de même nom existent déjà sur le serveur de destination.|  
|**Remplacer**|La tâche remplace les connexions de même nom sur le serveur de destination.|  
|**Ignorer**|La tâche ignore les connexions de même nom qui existent sur le serveur de destination.|  
  
 **CopySids**  
 Déterminez si les identificateurs de sécurité associés aux connexions doivent être copiés sur le serveur de destination. **CopySids** doit prendre la valeur **True** si la tâche de transfert de connexions est utilisée en parallèle à la tâche de transfert de bases de données. Dans le cas contraire, les connexions copiées ne seront pas reconnues par la base de données transférée.  
  
