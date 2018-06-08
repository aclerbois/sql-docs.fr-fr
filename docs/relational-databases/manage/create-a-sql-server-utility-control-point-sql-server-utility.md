---
title: Créer un point de contrôle de l’utilitaire SQL Server (Utilitaire SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.SWB.create.ucp.validation.F1
- sql13.SWB.create.ucp.Summary.F1
- sql13.SWB.create.ucp.progress.F1
- sql13.SWB.create.ucp.agentconfiguration.F1
- sql13.SWB.create.ucp.welcome.F1
- sql13.SWB.create.ucp.instancename.F1
helpviewer_keywords:
- Create UCP
- UCP
ms.assetid: d5335124-1625-47ce-b4ac-36078967158c
caps.latest.revision: 13
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 216599247aed33659c65f0b04293eff88a3d3692
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-sql-server-utility-control-point-sql-server-utility"></a>Créer un point de contrôle de l'utilitaire SQL Server (utilitaire SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une entreprise peut disposer de plusieurs utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et chaque utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut gérer de nombreuses instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et applications de la couche Données. Chaque utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détient un seul et unique point de contrôle de l’utilitaire (UCP). Vous devez créer un UCP pour chaque utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Chaque instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et chaque composant d’application de la couche Données est membre d’un seul et unique utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et est géré par un UCP unique.  
  
 L'UCP recueille des informations sur la configuration et les performances des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] toutes les 15 minutes. Ces informations sont stockées dans l'entrepôt de données de gestion de l'utilitaire (UMDW) sur l'UCP ; le nom de fichier UMDW est sysutility_mdw. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont comparées aux stratégies afin d'aider à identifier les opportunités de consolidation et les goulots d'étranglement de performances.  
  
## <a name="before-you-begin"></a>Avant de commencer  
 Avant de créer un point de contrôle de l'utilitaire (UCP), examinez les configurations requises et recommandations suivantes.  
  
 Dans cette version, l'UCP et toutes les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent respecter les conditions suivantes :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être de version 10.50 ou ultérieure.  
  
-   L'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être du type [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   L’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit fonctionner dans un domaine Windows unique ou des domaines ayant des relations d’approbation bidirectionnelle.  
  
-   Les comptes de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'UCP et toutes les instances managées de SQL Server doivent disposer d'un accès en lecture aux utilisateurs dans Active Directory.  
  
 Dans cette version, l'UCP doit respecter les exigences suivantes :  
  
-   L’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être une édition prise en charge. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Nous recommandons d'héberger l'UCP sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]qui respecte la casse.  
  
 Tenez compte des recommandations suivantes pour planifier la capacité sur l'ordinateur de l'UCP :  
  
-   Dans un scénario classique, l'espace disque utilisé par la base de données UMDW (sysutility_mdw) sur l'UCP est d'environ 2 Go par instance gérée de SQL Server par an. Cette évaluation peut varier selon le nombre d'objets de base de données et système collectés par l'instance gérée. Le taux de croissance de l'espace disque de la base de données UMDW (sysutility_mdw) est plus élevé pendant les deux premiers jours.  
  
-   Dans un scénario classique, l'espace disque utilisé par msdb sur l'UCP est d'environ 20 Mo par instance managée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Notez que cette évaluation peut varier selon les stratégies d'utilisation des ressources et le nombre de bases de données et d'objets système collectés par l'instance gérée. En général, l'utilisation de l'espace disque augmente en proportion de l'augmentation du nombre de violations de la stratégie et de l'augmentation de la durée de la fenêtre temporelle mobile des ressources volatiles.  
  
-   Notez que la suppression d'une instance gérée de l'UCP ne réduira pas l'espace disque utilisé par les bases de données de l'UCP jusqu'à expiration des périodes de rétention des données pour l'instance gérée.  
  
 Dans cette version, toutes les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent respecter les conditions suivantes :  
  
-   Si l’UCP est hébergé par une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sans respect de la casse, nous recommandons que les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient également sans respect de la casse.  
  
-   Les données FILESTREAM ne sont pas prises en charge pour la surveillance de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour plus d’informations, consultez [Spécifications des capacités maximales pour SQL Server](../../sql-server/maximum-capacity-specifications-for-sql-server.md) et [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
### <a name="remove-previous-utility-control-points-before-installing-a-new-one"></a>Supprimez les points de contrôle d'utilitaire précédents avant d'en installer un nouveau  
 Si vous installez un point de contrôle d'utilitaire (UCP) sur une instance de SQL Server qui a déjà été configurée comme UCP, vous devez, auparavant, supprimer toutes les instances gérées de SQL Server, ainsi que l'UCP. Pour ce faire, exécutez la procédure stockée **sp_sysutility_ucp_remove** .  
  
 Avant d'exécuter la procédure, notez la configuration requise :  
  
-   Cette procédure doit être exécutée sur un ordinateur qui est un UCP.  
  
-   Cette procédure doit être exécutée par un utilisateur disposant d'autorisations sysadmin, qui sont les mêmes autorisations requises pour créer un UCP.  
  
-   Toutes les instances gérées de SQL Server doivent être supprimées de l'UCP. Notez que l'UCP est une instance gérée de SQL Server. Pour plus d'informations, consultez [Procédure : supprimer une instance de SQL Server de l'utilitaire SQL Server](http://go.microsoft.com/fwlink/?LinkId=169392).  
  
 Utilisez cette procédure pour supprimer un UCP SQL Server de l'utilitaire SQL Server. Une fois l'opération terminée, il est possible de créer à nouveau un UCP sur l'instance de SQL Server.  
  
 Utilisez SQL Server Management Studio pour la connexion au point de contrôle de l'utilitaire, puis exécutez le script suivant :  
  
```  
EXEC msdb.dbo.sp_sysutility_ucp_remove;  
```  
  
> [!NOTE]  
>  Si l'instance de SQL Server de laquelle l'UCP est supprimé a un jeu d'éléments de collecte de données qui n'est pas d'utilitaire, la base de données sysutility_mdw n'est pas supprimée par la procédure. Si tel est le cas, la base de données sysutility_mdw doit être supprimée manuellement avant que le point de contrôle de l'utilitaire soit à nouveau créé.  
  
 Chaque instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et chaque composant d’application de la couche Données est membre d’un seul et unique utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et est géré par un UCP unique. Pour plus d’informations sur les concepts de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
 Un UCP est le point de raisonnement central de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . À l’aide de l’UCP, vous pouvez afficher les informations de configuration et de performances collectées à partir des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des applications de la couche Données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et effectuer des activités générales de planification de capacité. L'UCP est le point de lancement pour l'inscription et la suppression des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Après avoir inscrit des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez surveiller l’intégrité des ressources pour les instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les applications de la couche Données pour identifier les possibilités de consolidation et isoler les goulots d’étranglement des ressources. Pour plus d’informations, consultez [Surveiller des instances de SQL Server dans l’utilitaire SQL Server](../../relational-databases/manage/monitor-instances-of-sql-server-in-the-sql-server-utility.md).  
  
> [!IMPORTANT]  
>  Le jeu d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est pris en charge côte à côte avec les jeux d'éléments de collecte d'utilitaires non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Autrement dit, une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être surveillée par d'autres jeux d'éléments de collecte bien qu'elle soit membre d'un utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Notez, toutefois, que tous les jeux d'éléments de collecte sur l'instance gérée téléchargeront leurs données à l'entrepôt de données de gestion de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Considérations sur l’exécution de jeux d’éléments de collecte de l’utilitaire et non-utilitaire sur la même instance de SQL Server](../../relational-databases/manage/run-utility-and-non-utility-collection-sets-on-same-sql-instance.md) et [Configurer votre entrepôt de données de point de contrôle de l’utilitaire &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/configure-your-utility-control-point-data-warehouse-sql-server-utility.md).  
  
## <a name="wizard-steps"></a>Étapes de l'Assistant  
 ![](../../relational-databases/manage/media/create-ucp.gif "Create_UCP")  
  
 Les sections suivantes fournissent des informations sur chaque page du flux de travail de l'Assistant pour créer un UCP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour lancer l’Assistant pour créer un UCP, ouvrez le volet Explorateur de l’utilitaire dans le menu Affichage dans SSMS, puis cliquez sur le bouton ![](../../relational-databases/manage/media/create-ucp.gif "Créer un UCP") **Créer un UCP** en haut du volet Explorateur de l’utilitaire.  
  
 Cliquez sur un lien dans la liste ci-dessous pour accéder aux détails d'une page dans l'Assistant :  
  
 Pour plus d'informations sur un script PowerShell de cette opération, consultez l' [exemple](#PowerShell_create_UCP).  
  
-   [Introduction à l'Assistant Créer un UCP](#Welcome)  
  
-   [Spécifier une instance](#Instance_name)  
  
-   [Dialogue de connexion](#Connection_dialog)  
  
-   [Compte du jeu d'éléments de collecte de l'utilitaire](#Agent_configuration)  
  
-   [Règles de validation](#Validation_rules)  
  
-   [Résumé](#Summary)  
  
-   [Création du point de contrôle d'utilitaire](#Creating_UCP)  
  
##  <a name="Welcome"></a> Introduction à l'Assistant Créer un UCP  
 Si vous ouvrez l'Explorateur de l'utilitaire et qu'aucun point de contrôle d'utilitaire n'est connecté, vous devez en connecter un ou en créer un.  
  
 **Se connecter à un UCP existant** - Si un point de contrôle d’utilitaire existe déjà dans votre déploiement, vous pouvez vous y connecter en cliquant sur le bouton ![](../../relational-databases/manage/media/connect-to-utility.gif "Se connecter à l’utilitaire")**Se connecter à l’utilitaire** en haut du volet Explorateur de l’utilitaire. Pour se connecter à un UCP existant, vous devez disposer d'informations d'identification d'administrateur ou être membre du rôle de lecteur d'utilitaire. Notez qu'il ne peut y avoir qu'un seul UCP par utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et que vous ne pouvez être connecté qu'à un UCP d'une instance de SSMS.  
  
 **Créer un UCP** - Pour créer un point de contrôle d’utilitaire, cliquez sur le bouton ![](../../relational-databases/manage/media/create-ucp.gif "Créer un UCP")**Créer un UCP** en haut du volet Explorateur de l’utilitaire. Pour créer un UCP, vous devez spécifier le nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fournir des informations d'identification d'administrateur dans le dialogue de connexion. Notez qu'il ne peut y avoir qu'un seul UCP par utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="Instance_name"></a> Spécifier une instance  
 Spécifiez les informations suivantes à propos de l'UCP que vous créez :  
  
-   **Nom de l’instance** - Pour sélectionner une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir du dialogue de connexion, cliquez sur **Se connecter…**. Indiquez le nom de l’ordinateur et le nom de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au format NomOrdinateur\NomInstance.  
  
-   **Nom de l’utilitaire** - Spécifiez un nom qui sera utilisé pour identifier l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le réseau.  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Connection_dialog"></a> Dialogue de connexion  
 Dans la boîte de dialogue Se connecter au serveur, vérifiez les informations type de serveur, nom de l'ordinateur et nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Se connecter au serveur &#40;moteur de base de données&#41;](http://msdn.microsoft.com/library/ee9017b4-8a19-4360-9003-9e6484082d41).  
  
> [!NOTE]  
>  Si la connexion est chiffrée, la connexion chiffrée sera utilisée. Si la connexion n'est pas chiffrée, l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se reconnectera à l'aide d'une connexion chiffrée.  
  
 Pour continuer, cliquez sur **Se connecter…**.  
  
##  <a name="Agent_configuration"></a> Compte du jeu d'éléments de collecte de l'utilitaire  
 Spécifiez un compte de domaine Windows pour exécuter le jeu d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce compte est utilisé comme compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le jeu d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez également utiliser le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent existant. Pour satisfaire aux exigences de validation, suivez les indications suivantes pour spécifier le compte.  
  
 Si vous indiquez l'option de compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent :  
  
-   Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être un compte de domaine Windows qui ne soit pas un compte intégré comme LocalSystem, NetworkService ou LocalService.  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Validation_rules"></a> Règles de validation  
 Dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les conditions suivantes doivent être remplies sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où l'UCP sera créé :  
  
|Règle de validation|Action corrective|  
|---------------------|-----------------------|  
|Vous devez avoir des privilèges d'administrateur sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où le point de contrôle de l'utilitaire sera créé.|Connectez-vous avec un compte disposant de privilèges d'administrateur sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être de version 10.50 ou ultérieure.|Spécifiez une instance différente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour héberger l’UCP.|  
|L’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être une édition prise en charge. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|Spécifiez une instance différente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour héberger l’UCP.|  
|L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne doit pas être une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inscrite avec un autre UCP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Spécifiez une instance différente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour héberger l'UCP, ou inscrire l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de l'UCP où c'est actuellement une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être déjà l'hôte d'un point de contrôle de l'utilitaire.|Spécifiez une instance différente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour héberger l’UCP.|  
|L'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir TCP/IP activé.|Activez TCP/IP pour l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|L’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas avoir de base de données nommée « sysutility_mdw ».|L'opération créer un UCP créera un entrepôt de données de gestion de l'utilitaire (UMDW) nommé « sysutility_mdw ». L'opération exige que ce nom n'existe pas sur l'ordinateur au moment où les règles de validation sont exécutées. Pour continuer, vous devez supprimer ou renommer toute base de données nommée « sysutility_mdw ». Pour plus d’informations sur les opérations de modification de nom, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).|  
|Les jeux d'éléments de collecte sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent être interrompus.|Interrompez les jeux d'éléments de collecte préexistants lors de la création de l'UCP sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le collecteur de données est désactivé, activez-le, interrompez tous jeux d'éléments de collecte en cours d'exécution, puis réexécutez des règles de validation pour l'opération Créer un UCP.<br /><br /> Pour activer le collecteur de données :<br /><br /> Dans l'Explorateur d'objets, développez le nœud **Gestion** .<br /><br /> Cliquez avec le bouton droit sur **Collecte de données**, puis cliquez sur **Activer la collecte de données**.<br /><br /> Pour arrêter un jeu d'éléments de collecte :<br /><br /> Dans l'Explorateur d'objets, développez le nœud Gestion et développez **Collecte de données**, puis **Jeux d'éléments de collecte de données système**.<br /><br /> Cliquez avec le bouton droit sur le jeu d’éléments de collecte à arrêter, puis cliquez sur **Arrêter le jeu d’éléments de collecte de données**.<br /><br /> Une zone de message affiche les résultats de cette action et un cercle rouge sur l'icône du jeu d'éléments de collecte indique que celui-ci s'est arrêté.|  
|Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur l'instance spécifiée doit être démarré. Si l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être configuré pour démarrer manuellement. Sinon, le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être configuré pour démarrer automatiquement.|Démarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Si l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , configurez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour démarrer manuellement. Sinon, configurez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour démarrer automatiquement.|  
|WMI doit être correctement configuré.|Pour résoudre les problèmes de configuration de WMI, consultez [Résolution des problèmes liés à l’utilitaire SQL Server](http://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453).|  
|Le compte d’agent proxy de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être un compte intégré, comme Service réseau.|Si le compte proxy de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un compte intégré, comme Service réseau, réattribuez le compte à un compte de domaine Windows qui est sysadmin.|  
|Si vous sélectionnez l'option de compte proxy, le compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être un compte de domaine Windows valide.|Spécifiez un compte de domaine Windows valide. Pour vérifier que le compte est valide, ouvrez une session sur l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du compte de domaine Windows.|  
|Si vous sélectionnez l’option de compte de service, le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne peut pas être un compte intégré, comme Service réseau.|Si le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est un compte intégré, comme Service réseau, réattribuez le compte à un compte de domaine Windows.|  
|Si vous sélectionnez l'option de compte proxy, le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être un compte de domaine Windows valide.|Spécifiez un compte de domaine Windows valide. Pour vérifier que le compte est valide, ouvrez une session sur l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du compte de domaine Windows.|  
  
 Si vous trouvez des erreurs dans les résultats de validation, corrigez les problèmes bloquants puis cliquez sur **Réexécuter la validation** pour vérifier la configuration de l'ordinateur.  
  
 Pour enregistrer le rapport de validation, cliquez sur **Enregistrer le rapport** , puis indiquez un emplacement pour le fichier.  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Summary"></a> Résumé  
 La page Résumé affiche les informations que vous avez fournies à propos de l'UCP :  
  
-   le nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge l'UCP ;  
  
-   le nom de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   le nom du compte qui sera utilisé pour exécuter des travaux pour la collecte de données de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour modifier les paramètres de configuration de l'UCP, cliquez sur **Précédent**. Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Creating_UCP"></a> Création du point de contrôle d'utilitaire  
 Pendant l'opération de création d'un UCP, l'Assistant indiquera les étapes et l'état :  
  
-   Préparation de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la création d'un UCP  
  
-   Création de l'entrepôt de données de gestion de l'utilitaire (UMDW)  
  
-   Initialisation de l’UMDW [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le nom de fichier UMDW est sysutility_mdw.  
  
-   Configuration de l'UCP  
  
-   Configuration du jeu d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 Pour enregistrer un rapport sur l'opération créer un UCP, cliquez sur **Enregistrer le rapport** , puis indiquez un emplacement pour le fichier.  
  
 Pour mettre fin à l'Assistant, cliquez sur **Terminer**.  
  
 Une fois l'Assistant Créer un UCP terminé, le volet Navigation de l'Explorateur de l'utilitaire dans SSMS affiche un nœud pour l'UCP contenant des nœuds pour les Applications de la couche Données déployées, les Instances gérées et l'Administration de l'utilitaire. L'UCP devient automatiquement une instance gérée.  
  
 Le processus de collecte de données commence immédiatement, mais cela peut prendre jusqu'à 30 minutes pour que les données s'affichent d'abord dans le tableau de bord et les points de vue dans le volet Contenu de l'Explorateur de l'utilitaire. La collecte de données se poursuit une fois toutes les 15 minutes. Les données initiales proviendront de l'UCP lui-même. Autrement dit, l'UCP est la première instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour afficher le tableau de bord, cliquez sur **Afficher** , puis sélectionnez **Contenu de l'Explorateur de l'utilitaire** dans le menu SSMS. Pour actualiser les données, cliquez avec le bouton droit sur le nom de l’utilitaire dans le volet Explorateur de l’utilitaire, puis sélectionnez **Actualiser**.  
  
 Pour plus d’informations sur l’inscription d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md). Pour supprimer l’UCP en tant qu’instance gérée de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionnez **Instances gérées** dans le volet **Explorateur de l’utilitaire** pour remplir le mode Liste d’instances gérées, cliquez avec le bouton droit sur le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans **Contenu de l’Explorateur de l’utilitaire** en mode Liste, puis sélectionnez **Rendre une instance non managée**.  
  
##  <a name="PowerShell_create_UCP"></a> Créer un point de contrôle d'utilitaire à l'aide de PowerShell  
 Utilisez l'exemple suivant pour créer un point de contrôle d'utilitaire :  
  
```  
> $UtilityInstance = new-object –Type Microsoft.SqlServer.Management.Smo.Server "ComputerName\UCP-Name";  
> $SqlStoreConnection = new-object -Type Microsoft.SqlServer.Management.Sdk.Sfc.SqlStoreConnection $UtilityInstance.ConnectionContext.SqlConnectionObject;  
> $Utility = [Microsoft.SqlServer.Management.Utility.Utility]::CreateUtility("Utility", $SqlStoreConnection, "ProxyAccount", "ProxyAccountPassword");  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Résolution des problèmes liés à l’utilitaire SQL Server](http://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453)  
  
  
