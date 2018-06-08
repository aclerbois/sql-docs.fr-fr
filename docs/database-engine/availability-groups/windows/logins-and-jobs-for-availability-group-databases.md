---
title: Connexions et travaux pour les bases de données de groupe de disponibilité | Documents Microsoft
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: d7da14d3-848c-44d4-8e49-d536a1158a61
caps.latest.revision: 16
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e6d704c10a90bf079b436c4923c6b21fff92319c
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770244"
---
# <a name="logins-and-jobs-for-availability-group-databases"></a>Connexions et travaux pour les bases de données de groupe de disponibilité
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous devez maintenir régulièrement le même ensemble de connexions utilisateur et de travaux de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent sur chaque base de données primaire d’un groupe de disponibilité Always On et les bases de données secondaires correspondantes. Les connexions et les travaux doivent être reproduits sur chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge un réplica de disponibilité pour le groupe de disponibilité.  
  
-   **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Travaux de l'Agent**  
  
     Vous devez copier manuellement les travaux appropriés de l'instance de serveur qui héberge le réplica principal d'origine vers les instances de serveur qui hébergent les réplicas secondaires d'origine. Pour toutes les bases de données, vous devez ajouter la logique au début de chaque travail approprié de façon à ce que le travail s'exécute uniquement sur la base de données primaire, c.-à-d., uniquement lorsque le réplica local est le réplica principal pour la base de données.  
  
     Les instances de serveur qui hébergent les réplicas de disponibilité d'un groupe de disponibilité peuvent être configurées différemment, avec des lettres de lecteurs de bande ou de lecteur différentes. Les travaux de chaque réplica de disponibilité doivent autoriser de telles différences.  
  
     Notez que les travaux de sauvegarde peuvent utiliser la fonction [sys.fn_hadr_is_preferred_backup_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) pour déterminer si le réplica local est celui qui est utilisé par défaut pour les sauvegardes, selon les préférences de sauvegarde du groupe de disponibilité. Les travaux de sauvegarde créés à l’aide de l’ [Assistant Plan de maintenance](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md) utilisent cette fonction en mode natif. Pour d'autres travaux de sauvegarde, nous vous recommandons d'utiliser cette fonction comme condition dans vos travaux de sauvegarde, de façon à ce qu'ils s'exécutent uniquement sur le réplica par défaut. Pour plus d’informations, consultez [Secondaires actifs : sauvegarde sur les réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
-   **Connexions**  
  
     Si vous utilisez des bases de données à relation contenant-contenu, vous pouvez configurer les utilisateurs contenus dans les bases de données, et pour ces utilisateurs, vous n'avez pas besoin de créer des connexions sur des instances de serveur qui hébergent un réplica secondaire. Pour une base de données de disponibilité sans relation contenant-contenu, vous devrez créer les utilisateurs pour les connexions sur les instances de serveur qui hébergent les réplicas de disponibilité. Pour plus d’informations, consultez [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md).  
  
     Si l’une de vos applications utilise l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou une connexion locale Windows, consultez [Connexions des applications qui utilisent l’authentification SQL Server ou une connexion locale Windows](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md#SSauthentication), plus loin dans cette rubrique.  
  
    > [!NOTE]  
    >  Un utilisateur de base de données pour lequel la connexion SQL Server n'est pas définie sur une instance de serveur, ou l'est de façon incorrecte, ne peut pas se connecter à cette instance. L'utilisateur devient donc un *utilisateur orphelin* de la base de données sur cette instance du serveur. Si un utilisateur est orphelin sur une instance de serveur donnée, vous pouvez configurer les connexions utilisateur à tout moment. Pour plus d’informations, consultez [Troubleshoot Orphaned Users &#40;SQL Server&#41;](../../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md).  
  
-   **Métadonnées supplémentaires**  
  
     Les connexions et les travaux ne sont pas les seules informations qui doivent être recréées sur chaque instance de serveur qui héberge un réplica secondaire d'un groupe de disponibilité donné. Par exemple, vous devrez peut-être recréer les paramètres de configuration du serveur, les informations d'identification, les données chiffrées, les autorisations, les paramètres de réplication, les applications de Service Broker, les déclencheurs (au niveau du serveur), etc. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="SSauthentication"></a> Connexions des applications qui utilisent l’authentification SQL Server ou une connexion locale Windows  
 Si une application utilise l'authentification SQL Server ou une connexion locale Windows, des SID incompatibles peuvent empêcher la résolution de la connexion de l'application sur une instance distante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. En cas de SID incompatibles, la connexion peut se solder par un utilisateur orphelin sur l'instance de serveur distante. Ce problème peut se produire lorsqu'une application se connecte à une base de données de copie des journaux de transaction ou une base de données mise en miroir suite à un basculement ou à une base de données d'abonné de réplication qui a été initialisée à partir d'une sauvegarde.  
  
 Pour éviter ce problème, nous vous recommandons de prendre des mesures préventives lorsque vous configurez une telle application de manière à utiliser une base de données hébergée par une instance distante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La prévention implique de transférer des connexions et des mots de passe de l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'instance distante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la manière d’éviter ce problème, consultez l’article 918992 de la base de connaissances Microsoft :[Comment transférer les connexions et les mots de passe entre des instances de SQL Server](http://support.microsoft.com/kb/918992/)).  
  
> [!NOTE]  
>  Ce problème affecte les comptes Windows locaux sur différents ordinateurs. Toutefois, ce problème ne se pose pas pour les comptes de domaine car le SID est identique sur chacun des ordinateurs.  
  
 Pour plus d’informations, consultez [Mise en miroir de bases de données et copie des journaux de transaction](http://blogs.msdn.com/b/sqlserverfaq/archive/2009/04/13/orphaned-users-with-database-mirroring-and-log-shipping.aspx) (blog du moteur de base de données).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer un compte de connexion](../../../relational-databases/security/authentication-access/create-a-login.md)  
  
-   [Créez un utilisateur de base de données](../../../relational-databases/security/authentication-access/create-a-database-user.md).  
  
-   [Créer un travail](http://msdn.microsoft.com/library/b35af2b6-6594-40d1-9861-4d5dd906048c)  
  
-   [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Contained Databases](../../../relational-databases/databases/contained-databases.md)   
 [Créer des travaux](http://msdn.microsoft.com/library/465fb7fc-7622-4252-a178-ea51691c935b)  
  
  
