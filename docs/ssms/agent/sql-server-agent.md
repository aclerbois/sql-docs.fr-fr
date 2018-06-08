---
title: SQL Server Agent | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, about SQL Server Agent
- automatic administration steps
ms.assetid: 8d1dc600-aabb-416f-b3af-fbc9fccfd0ec
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 80b6776f555fd5bdaa8ed4c4977dc5193a27eba2
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708937"
---
# <a name="sql-server-agent"></a>SQL Server Agent
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent est un service Microsoft Windows qui exécute des tâches administratives planifiées appelées *travaux* dans [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)].  
  
**Dans cette rubrique**  
  
-   [Avantages de SQL Server Agent](#Benefits)  
  
-   [Composants de SQL Server Agent](#Components)  
  
-   [Sécurité pour l'administration de SQL Server Agent](#Security)  
  
## <a name="Benefits"></a>Avantages de SQL Server Agent  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour stocker les informations sur les travaux. Les travaux contiennent une ou plusieurs étapes de travail. Chaque étape contient sa propre tâche, par exemple, la sauvegarde d'une base de données.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent peut exécuter un travail sur la base d'une planification, en réponse à un événement spécifique ou sur demande. Par exemple, si vous sauvegardez l’ensemble des serveurs de l’entreprise chaque jour ouvrable après les horaires de bureau, vous pouvez automatiser cette tâche. Planifiez l'exécution de la sauvegarde après 22h00 du lundi au vendredi ; si la sauvegarde rencontre un problème, SQL Server Agent peut enregistrer l'événement et vous en avertir.  
  
> [!NOTE]  
> Par défaut, le service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent est désactivé lorsque [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] est installé, sauf si l'utilisateur choisit explicitement de démarrer automatiquement le service.  
  
## <a name="Components"></a>Composants de SQL Server Agent  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent utilise les composants ci-après pour définir les tâches à exécuter et quand les exécuter, et pour signaler si elles ont réussi ou échoué.  
  
### <a name="jobs"></a>travaux  
Un *travail* est une suite d'actions effectuées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Utilisez les travaux pour définir une tâche administrative, afin qu'elle soit exécutée une ou plusieurs fois et que son résultat (échec ou réussite) soit contrôlé. Un travail peut être exécuté sur un serveur local ou sur plusieurs serveurs distants.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Les travaux qui s'exécutent au moment d'un événement de basculement sur une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ne reprennent pas sur un autre nœud de cluster de basculement après le basculement. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent : Les travaux qui s’exécutent au moment où un nœud Hyper-V est suspendu ne reprennent pas si la pause provoque un basculement vers un autre nœud. Les travaux qui commencent mais ne peuvent pas se terminer à cause d'un événement de basculement sont enregistrés comme commencés, mais n'affichent pas d'entrées de journal supplémentaires pour l'achèvement ou l'échec. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] dans ces scénarios semblent ne s'être jamais terminés.  
  
Vous pouvez exécuter un travail :  
  
-   en fonction d'une ou plusieurs planifications ;  
  
-   en réponse à une ou plusieurs alertes ;  
  
-   en exécutant la procédure stockée sp_start_job.  
  
Chaque action d'un travail est appelée *étape du travail*. Par exemple, une étape du travail peut être l'exécution d'une instruction [!INCLUDE[tsql](../../includes/tsql_md.md)] , l'exécution d'un package [!INCLUDE[ssIS](../../includes/ssis_md.md)] ou l'envoi d'une commande à un serveur Analysis Services. Les étapes du travail sont gérées dans le cadre d'un travail.  
  
Chaque étape s'exécute dans un contexte de sécurité spécifique. Pour les étapes qui utilisent [!INCLUDE[tsql](../../includes/tsql_md.md)], utilisez l'instruction EXECUTE AS pour définir le contexte de sécurité. Pour les autres types d'étapes, utilisez un compte proxy.  
  
### <a name="schedules"></a>Planifications  
Une *planification* programme l'exécution d'un travail. Une même planification peut porter sur plusieurs travaux, et plusieurs planifications peuvent impliquer le même travail. Une planification peut prévoir l'exécution d'un travail :  
  
-   Au moment où [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent démarre.  
  
-   au moment où l'utilisation de l'UC atteint le niveau d'inactivité que vous avez défini ;  
  
-   ponctuellement, à une date et une heure spécifiques ;  
  
-   Selon une planification récurrente.  
  
Pour plus d’informations, consultez [Créer des planifications et les attacher à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
### <a name="alerts"></a>Alertes  
Une *alerte* est une réponse automatique à un événement donné. Par exemple, un événement peut être un travail qui démarre ou des ressources système qui atteignent un seuil spécifique. Vous définissez les conditions selon lesquelles une alerte est déclenchée.  
  
Une alerte peut être une réponse à :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Des événements.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Des conditions de performance.  
  
-   Des événements Windows Management Instrumentation sur l'ordinateur qui exécute SQL Server Agent.  
  
Une alerte peut :  
  
-   prévenir un ou plusieurs opérateurs ;  
  
-   exécuter un travail.  
  
Pour plus d’informations, consultez [Alertes](../../ssms/agent/alerts.md).  
  
### <a name="operators"></a>Opérateurs  
Un *opérateur* est une personne responsable de la maintenance d'une ou plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Dans certaines sociétés, les responsabilités d'opérateur sont affectées à une seule personne. Dans les entreprises qui ont de nombreux serveurs, plusieurs personnes peuvent se partager les responsabilités d'opérateur. Un opérateur ne contient pas d'informations de sécurité et ne définit pas de principal de sécurité.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] peut notifier les opérateurs à propos d’alertes par :  
  
-   Messagerie électronique  
  
-   Radiomessagerie (via e-mail)  
  
-   **net send**  
  
> [!NOTE]  
> Pour envoyer des notifications via **net send**, le service Windows Messenger doit être actif sur l’ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent réside.  
  
> [!IMPORTANT]  
> Les options du récepteur de radiomessagerie et **net send** seront supprimées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent dans une prochaine version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement.  
  
Pour envoyer des notifications aux opérateurs par e-mail ou radiomessagerie, vous devez configurer [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent afin qu’il utilise la messagerie de base de données. Pour plus d’informations, consultez [Messagerie de base de données](http://msdn.microsoft.com/9e4563dd-4799-4b32-a78a-048ea44a44c1).  
  
Un opérateur peut être l'alias d'un groupe d'individus. De cette façon, tous les membres de cet alias sont avertis en une seule fois. Pour plus d’informations, consultez [Opérateurs](../../ssms/agent/operators.md).  
  
## <a name="Security"></a>Sécurité pour l'administration de SQL Server Agent  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent utilise les rôles de base de données fixes **SQLAgentUserRole**, **SQLAgentReaderRole**et **SQLAgentOperatorRole** de la base de données **msdb** pour contrôler l'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent par les utilisateurs qui ne sont pas membres du rôle serveur fixe **sysadmin** . Outre ces rôles de base de données fixes, les sous-systèmes et les proxys aident les administrateurs de base de données à garantir que chaque étape de travail est exécutée avec les autorisations minimales nécessaires à la réalisation de cette tâche.  
  
### <a name="roles"></a>Rôles  
Les membres des rôles de base de données fixes **SQLAgentUserRole**, **SQLAgentReaderRole**et **SQLAgentOperatorRole** de **msdb**et les membres du rôle de serveur fixe **sysadmin** ont accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Un utilisateur qui n'appartient à aucun de ces rôles ne peut pas utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Pour plus d’informations sur les rôles utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
### <a name="subsystems"></a>Sous-systèmes  
Un sous-système est un objet prédéfini qui représente la fonctionnalité disponible pour une étape de travail. Chaque proxy a accès à un ou plusieurs sous-systèmes. Les sous-systèmes assurent la sécurité en délimitant l'accès aux fonctionnalités mises à la disposition d'un proxy. Chaque étape de travail s'exécute dans le contexte d'un proxy, à l'exception des étapes de travail [!INCLUDE[tsql](../../includes/tsql_md.md)] . Le travail de [!INCLUDE[tsql](../../includes/tsql_md.md)] utilise la commande EXECUTE AS pour définir le contexte de sécurité sur le propriétaire du travail.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] définit les sous-systèmes répertoriés dans le tableau suivant :  
  
|Nom du sous-système|Description|  
|--------------|-----------|  
|Script ActiveX Microsoft|Permet d'exécuter une étape de travail de script ActiveX<br /><br />**Avertissement** Le sous-système de création de scripts ActiveX sera supprimé de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent dans une version future de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.|  
|Système d’exploitation (**CmdExec**)|Permet de lancer un programme exécutable.|  
|PowerShell|Exécutez une étape de travail de scripts PowerShell.|  
|Serveur de distribution de réplication|Permet d'exécuter une étape de travail qui active l'Agent de distribution.|  
|Fusion de réplication|Permet d'exécuter une étape de travail qui active l'Agent de fusion.|  
|Agent de lecture de file d'attente de réplication|Permet d'exécuter une étape de travail qui active l'Agent de lecture de la file d'attente de réplication.|  
|Instantané de réplication|Permet d'exécuter une étape de travail qui active l'Agent d'instantané des réplications.|  
|Agent de lecture du journal des transactions de réplication|Permet d'exécuter une étape de travail qui active l'Agent de lecture du journal des réplications.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Command|Permet d'exécuter une commande [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] .|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Requête|Permet d'exécuter une requête [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] .|  
|[!INCLUDE[ssIS](../../includes/ssis_md.md)] Exécution du package|Permet d'exécuter un package [!INCLUDE[ssIS](../../includes/ssis_md.md)] .|  
  
> [!NOTE]  
> Comme les étapes de travail [!INCLUDE[tsql](../../includes/tsql_md.md)] n'utilisent pas de proxys, il n'existe aucun sous-système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent pour les étapes de travail [!INCLUDE[tsql](../../includes/tsql_md.md)] .  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent applique les restrictions des sous-systèmes même si le principal de sécurité du proxy a généralement l'autorisation d'exécuter cette tâche dans l'étape de travail. Par exemple, un proxy pour un utilisateur qui est membre du rôle de serveur fixe sysadmin ne peut pas exécuter d'étape de travail [!INCLUDE[ssIS](../../includes/ssis_md.md)] , à moins que le proxy ait accès au sous-système [!INCLUDE[ssIS](../../includes/ssis_md.md)] , même si l'utilisateur peut exécuter des packages [!INCLUDE[ssIS](../../includes/ssis_md.md)] .  
  
### <a name="proxies"></a>Proxys  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent utilise les proxys pour gérer les contextes de sécurité. Un proxy peut être utilisé dans plusieurs étapes de travail. Les membres du rôle de serveur fixe **sysadmin** peuvent créer des proxys.  
  
Chaque proxy correspond à des informations d'identification de sécurité. Chaque proxy peut être associé à un ensemble de sous-systèmes et de connexions. Le proxy peut être utilisé uniquement pour les étapes de travail qui utilisent un sous-système associé au proxy. Pour créer une étape de travail qui utilise un proxy spécifique, le propriétaire du travail doit utiliser une connexion associée à ce proxy ou être membre d'un rôle sans restriction d'accès aux proxys. Les membres du rôle de serveur fixe **sysadmin** disposent d'un accès sans restriction aux proxys. Les membres des rôles **SQLAgentUserRole**, **SQLAgentReaderRole**ou **SQLAgentOperatorRole** peuvent utiliser uniquement les proxys pour lesquels un accès spécifique leur a été accordé. Chaque utilisateur membre de l'un des rôles de base de données fixes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent doit disposer d'un accès à des proxys spécifiques de façon à pouvoir créer les étapes de travail qui les utilisent.  
  
## <a name="related-tasks"></a>Related Tasks  
Utilisez les étapes suivantes pour configurer [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent de manière à automatiser l'administration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] :  
  
1.  Identifiez les tâches administratives et les événements de serveur se produisant régulièrement et déterminez si ces tâches ou ces événements peuvent être administrés par programme. Une tâche convient à l'automatisation si elle implique une séquence d'étapes prévisibles et se produit à un moment spécifique ou en réponse à un événement particulier.  
  
2.  Définissez un ensemble de travaux, de planifications, d’alertes et d’opérateurs à l’aide de scripts [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)], [!INCLUDE[tsql](../../includes/tsql_md.md)] ou d’objets SMO [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] . Pour plus d’informations, consultez [Créer des travaux](../../ssms/agent/create-jobs.md).  
  
3.  Exécutez les travaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent que vous avez définis.  
  
> [!NOTE]  
> Pour l'instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] se nomme SQLSERVERAGENT. Pour les instances nommées, le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent se nomme SQLAgent$*nom_instance*.  
  
Si vous exécutez plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez employer l'administration multiserveur pour automatiser des tâches courantes dans toutes les instances. Pour plus d’informations, consultez [Administration automatisée à l’échelle d’une entreprise](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
Utilisez les tâches suivantes pour démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent :  
  
|Description|Rubrique|  
|-----------|-----|  
|Explique comment configurer SQL Server Agent.|[Configurer SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md)|  
|Explique comment démarrer, arrêter et interrompre le service SQL Server Agent.|[Démarrer, arrêter ou suspendre le service SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|  
|Décrit les considérations à prendre en compte lors de la spécification d'un compte pour le service SQL Server Agent.|[Sélectionner un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|  
|Explique comment utiliser le journal des erreurs de SQL Server Agent.|[Journal des erreurs de SQL Server Agent](../../ssms/agent/sql-server-agent-error-log.md)|  
|Explique comment utiliser des objets de performances.|[Utiliser des objets de performance](../../ssms/agent/use-performance-objects.md)|  
|Décrit l'Assistant Plan de maintenance qui est un utilitaire permettant de créer des travaux, des alertes et des opérateurs en vue d'automatiser l'administration d'une instance de SQL Server.|[Utiliser l'Assistant Plan de maintenance](http://msdn.microsoft.com/db65c726-9892-480c-873b-3af29afcee44)|  
|Explique comment automatiser des tâches administratives à l'aide de l'Agent SQL Server.|[Tâches d’administration automatisée &#40;SQL Server Agent&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|  
  
## <a name="see-also"></a> Voir aussi  
[Configuration de la surface d'exposition](http://msdn.microsoft.com/f741169c-1453-4ad2-830b-bf2be27d712f)  
  
