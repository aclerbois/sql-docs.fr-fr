---
title: Inscrire une instance de SQL Server (utilitaire SQL Server) | Microsoft Docs
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
- sql13.SWB.makemanaged.agentaccount.F1
- sql13.SWB.makemanaged.welcome.F1
- sql13.SWB.makemanaged.enrolling.F1
- sql13.SWB.makemanaged.instancename.F1
- sql13.SWB.makemanaged.Summary.F1
- sql13.SWB.makemanaged.progress.F1
- sql13.SWB.makemanaged.validation.F1
helpviewer_keywords:
- Enroll instance
ms.assetid: a801c619-611b-4e82-a8d8-d1e01691b7a1
caps.latest.revision: 13
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ebe062a78caf3a0589afaef68b7cde988b8bcae4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enroll-an-instance-of-sql-server-sql-server-utility"></a>Inscrire une instance de SQL Server (utilitaire SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Inscrivez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existant pour surveiller ses performances et sa configuration comme une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le point de contrôle de l'utilitaire (UCP) recueille des informations sur la configuration et les performances des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] toutes les 15 minutes. Ces informations sont stockées dans l'entrepôt de données de gestion de l'utilitaire (UMDW) sur l'UCP ; le nom de fichier UMDW est sysutility_mdw. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont comparées aux stratégies afin d'aider à identifier les opportunités de consolidation et les goulots d'étranglement de performances.  
  
 Dans cette version, l'UCP et toutes les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent respecter les conditions suivantes :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être de version 10.50 ou ultérieure.  
  
-   L'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être du type [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   L'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit fonctionner dans un domaine Windows unique ou des domaines ayant des relations d'approbation bidirectionnelle.  
  
-   Les comptes de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'UCP et toutes les instances managées de SQL Server doivent disposer d'un accès en lecture aux utilisateurs dans Active Directory.  
  
-   L'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à inscrire ne peut pas être SQL Azure.  
  
 Dans cette version, l'UCP doit respecter les exigences suivantes :  
  
-   L’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être une édition prise en charge. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Nous recommandons d'héberger l'UCP sur une instance de SQL Server qui respecte la casse.  
  
-   Tenez compte des recommandations suivantes pour planifier la capacité sur l'ordinateur de l'UCP :  
  
    -   Dans un scénario classique, l'espace disque utilisé par la base de données UMDW (sysutility_mdw) sur l'UCP est d'environ 2 Go par instance gérée de SQL Server par an. Cette évaluation peut varier selon le nombre d'objets de base de données et système collectés par l'instance gérée. Le taux de croissance de l'espace disque de la base de données UMDW (sysutility_mdw) est plus élevé pendant les deux premiers jours.  
  
    -   Dans un scénario classique, l'espace disque utilisé par msdb sur l'UCP est d'environ 20 Mo par instance managée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Notez que cette évaluation peut varier selon les stratégies d'utilisation des ressources et le nombre de bases de données et d'objets système collectés par l'instance gérée. En général, l'utilisation de l'espace disque augmente en proportion de l'augmentation du nombre de violations de la stratégie et de l'augmentation de la durée de la fenêtre temporelle mobile des ressources volatiles.  
  
    -   Notez que la suppression d'une instance gérée de l'UCP ne réduira pas l'espace disque utilisé par les bases de données de l'UCP jusqu'à expiration des périodes de rétention des données pour l'instance gérée.  
  
 Dans cette version, toutes les instances gérées de SQL Server doivent respecter les conditions suivantes :  
  
-   Si l'UCP est hébergé par une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]insensible à la casse, les instances managées de SQL Server devront également être insensibles à la casse.  
  
-   Les données FILESTREAM ne sont pas prises en charge pour la surveillance de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour plus d’informations, consultez [Spécifications des capacités maximales pour SQL Server](../../sql-server/maximum-capacity-specifications-for-sql-server.md) et [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 Pour plus d’informations sur les concepts de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
> [!IMPORTANT]  
>  Le jeu d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est pris en charge côte à côte avec les jeux d'éléments de collecte d'utilitaires non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Autrement dit, une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être surveillée par d'autres jeux d'éléments de collecte bien qu'elle soit membre d'un utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Notez, toutefois, que tous les jeux d'éléments de collecte sur l'instance gérée téléchargent leurs données à l'entrepôt de données de gestion de l'utilitaire. Pour plus d’informations, consultez [Considérations sur l’exécution de jeux d’éléments de collecte de l’utilitaire et non-utilitaire sur la même instance de SQL Server](../../relational-databases/manage/run-utility-and-non-utility-collection-sets-on-same-sql-instance.md) et [Configurer votre entrepôt de données de point de contrôle de l’utilitaire &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/configure-your-utility-control-point-data-warehouse-sql-server-utility.md).  
  
## <a name="wizard-steps"></a>Étapes de l'Assistant  
 Les sections suivantes contiennent des informations détaillées sur chaque page du flux de travail de l'Assistant. Cliquez sur un lien pour accéder aux détails d'une page dans l'Assistant. Pour plus d'informations sur un script PowerShell de cette opération, consultez l' [exemple](#PowerShell_enroll)PowerShell.  
  
-   [Introduction à l'Assistant d'inscription d'instance](#Welcome)  
  
-   [Spécifiez l'instance de SQL Server.](#Instance_name)  
  
-   [Dialogue de connexion](#Connection_dialog)  
  
-   [Compte du jeu d'éléments de collecte de l'utilitaire](#Proxy_configuration)  
  
-   [Validation d'instance de SQL Server](#Validation_rules)  
  
-   [Résumé de l'inscription d'instance](#Summary)  
  
-   [Inscription de l'instance de SQL Server](#Enrolling)  
  
##  <a name="Welcome"></a> Introduction à l'Assistant d'inscription d'instance  
 Pour lancer l’Assistant, développez l’arborescence de l’Explorateur de l’utilitaire sur un point de contrôle d’utilitaire, cliquez avec le bouton droit sur **Instances managées**et sélectionnez **Ajouter une instance managée…**.  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Instance_name"></a> Spécifiez l'instance de SQL Server.  
 Pour sélectionner une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de la boîte de dialogue de connexion, cliquez sur **Se connecter…**. Indiquez le nom de l’ordinateur et le nom de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au format NomOrdinateur\NomInstance. Pour plus d’informations, consultez [Se connecter au serveur &#40;moteur de base de données&#41;](http://msdn.microsoft.com/library/ee9017b4-8a19-4360-9003-9e6484082d41).  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Connection_dialog"></a> Dialogue de connexion  
 Dans la boîte de dialogue Se connecter au serveur, vérifiez les informations type de serveur, nom de l'ordinateur et nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Se connecter au serveur &#40;moteur de base de données&#41;](http://msdn.microsoft.com/library/ee9017b4-8a19-4360-9003-9e6484082d41).  
  
> [!NOTE]  
>  Si la connexion est chiffrée, la connexion chiffrée est utilisée. Si la connexion n'est pas chiffrée, l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se reconnecte à l'aide d'une connexion chiffrée.  
  
 Pour continuer, cliquez sur **Se connecter…**.  
  
##  <a name="Proxy_configuration"></a> Compte du jeu d'éléments de collecte de l'utilitaire  
 Spécifiez un compte de domaine Windows pour exécuter le jeu d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce compte est utilisé comme compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le jeu d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez également utiliser le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent existant. Pour satisfaire aux exigences de validation, suivez les indications suivantes pour spécifier le compte.  
  
 Si vous indiquez l'option de compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent :  
  
-   Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être un compte de domaine Windows qui ne soit pas un compte intégré comme LocalSystem, NetworkService ou LocalService.  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Validation_rules"></a> Validation d'instance de SQL Server  
 Dans cette version finale, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à inscrire dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit remplir les conditions suivantes :  
  
|Condition|Action corrective|  
|---------------|-----------------------|  
|Vous devez avoir des privilèges administrateur sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et sur l'UCP.|Connectez-vous avec un compte disposant de privilèges administrateur sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et sur l'UCP.|  
|L'édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit prendre en charge l'inscription d'instance.|Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|TCP/IP doit être activé sur l'UCP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Activez TCP/IP sur l'UCP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être déjà inscrite avec un autre UCP de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Si l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous spécifiez est déjà managée dans le cadre d'un utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existant, vous ne pouvez pas l'inscrire avec un UCP différent.|  
|L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas déjà être un UCP.|Si l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous spécifiez est déjà un UCP différent de l'UCP auquel vous êtes connecté, vous ne pouvez pas l'inscrire dans cet UCP.|  
|L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir des jeux d'éléments de collecte de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installés.|Réinstallez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|Les jeux d'éléments de collecte sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent être interrompus.|Interrompez les jeux d'éléments de collecte préexistants sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le collecteur de données est désactivé, activez-le, interrompez tous jeux d'éléments de collecte en cours d'exécution, puis réexécutez des règles de validation pour l'opération Créer un UCP.<br /><br /> Pour activer le collecteur de données :<br /><br /> Dans l'Explorateur d'objets, développez le nœud **Gestion** .<br /><br /> Cliquez avec le bouton droit sur **Collecte de données**, puis cliquez sur **Activer la collecte de données**.<br /><br /> Pour arrêter un jeu d'éléments de collecte :<br /><br /> Dans l'Explorateur d'objets, développez le nœud Gestion et développez **Collecte de données**, puis **Jeux d'éléments de collecte de données système**.<br /><br /> Cliquez avec le bouton droit sur le jeu d’éléments de collecte à arrêter, puis cliquez sur **Arrêter le jeu d’éléments de collecte de données**.<br /><br /> Une zone de message affiche les résultats de cette action et un cercle rouge sur l'icône du jeu d'éléments de collecte indique que celui-ci s'est arrêté.|  
|Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être démarré.|Démarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , configurez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour démarrer manuellement. Sinon, configurez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour démarrer automatiquement.|  
|Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur l'UCP doit être démarré.|Démarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur l'UCP. Si l'UCP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , configurez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour démarrer manuellement. Sinon, configurez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour démarrer automatiquement.|  
|WMI doit être correctement configuré.|Pour résoudre les problèmes de configuration de WMI, consultez [Résolution des problèmes liés à l’utilitaire SQL Server](http://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453).|  
|Le compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être un compte de domaine Windows valide sur l'UCP.|Spécifiez un compte de domaine Windows valide. Pour vous assurer que le compte est valide, ouvrez une session sur l'UCP à l'aide du compte de domaine Windows.|  
|Si vous sélectionnez l'option de compte proxy, le compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être un compte de domaine Windows valide sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Spécifiez un compte de domaine Windows valide. Pour vérifier que le compte est valide, ouvrez une session sur l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du compte de domaine Windows.|  
|Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne peut pas être un compte intégré, comme Service réseau.|Réattribuez le compte à un compte de domaine Windows. Pour vérifier que le compte est valide, ouvrez une session sur l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du compte de domaine Windows.|  
|Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être un compte de domaine Windows valide sur l'UCP.|Spécifiez un compte de domaine Windows valide. Pour vous assurer que le compte est valide, ouvrez une session sur l'UCP à l'aide du compte de domaine Windows.|  
|Si vous sélectionnez l'option de compte service, le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être un compte de domaine Windows valide sur l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Spécifiez un compte de domaine Windows valide. Pour vérifier que le compte est valide, ouvrez une session sur l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du compte de domaine Windows.|  
  
 Si vous trouvez des erreurs dans les résultats de validation, corrigez les problèmes bloquants puis cliquez sur **Réexécuter la validation** pour vérifier la configuration de l'ordinateur.  
  
 Pour enregistrer le rapport de validation, cliquez sur **Enregistrer le rapport** , puis indiquez un emplacement pour le fichier.  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Summary"></a> Résumé de l'inscription d'instance  
 La page Résumé répertorie les informations concernant l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à ajouter à l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Paramètres de l'instance gérée :  
  
-   Nom de l'instance de SQL Server: ComputerName\InstanceName  
  
-   Compte de jeu d'éléments de collecte de l'utilitaire : DomainName\UserName  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Enrolling"></a> Inscription de l'instance de SQL Server  
 La page Inscription fournit l'état de l'opération :  
  
-   Préparation de l'instance pour l'inscription.  
  
-   Création du répertoire du cache pour les données collectées.  
  
-   Configuration du jeu d'éléments de collecte de l'utilitaire.  
  
 Pour enregistrer un rapport sur l'opération d'inscription, cliquez sur **Enregistrer le rapport** , puis indiquez un emplacement pour le fichier.  
  
 Pour mettre fin à l'Assistant, cliquez sur **Terminer**.  
  
> [!NOTE]  
>  Si vous utilisez l'authentification SQL Server pour vous connecter à l'instance de SQL Server à inscrire, et que vous spécifiez un compte proxy qui appartient à un domaine Active Directory différent du domaine où l'UCP se trouve, la validation d'instance réussit, mais l'opération d'inscription échoue avec le message d'erreur suivant :  
>   
>  Une exception s'est produite lors de l'exécution d'une instruction ou d'un lot Transact-SQL ou lot. (Microsoft.SqlServer.ConnectionInfo)  
>   
>  Informations supplémentaires : Impossible d’obtenir des informations sur l’utilisateur ou le groupe Windows NT « \<Nom_Domaine\Nom_Compte> », code d’erreur 0x5. (Microsoft SQL Server, erreur : 15404)  
>   
>  Pour plus d’informations sur la résolution de ce problème, consultez [Résolution des problèmes liés à l’utilitaire SQL Server](http://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453).  
  
> [!IMPORTANT]  
>  Ne modifiez pas les propriétés du jeu d'éléments de collecte « Informations sur l'utilitaire » sur une instance managée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et n'activez pas ou ne désactivez pas manuellement la collecte de données, car la collecte de données est contrôlée par un travail d'agent Utilitaire.  
  
 Après avoir effectué l'Assistant Inscription d'instance, cliquez sur le nœud **Instances gérées** dans le volet **Navigation de l'Explorateur de l'utilitaire** dans SSMS. Les instances inscrites de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont affichées en mode Liste dans le volet **Contenu de l'Explorateur de l'utilitaire** .  
  
 Le processus de collecte de données commence immédiatement, mais cela peut prendre jusqu'à 30 minutes pour que les données s'affichent d'abord dans le tableau de bord et les points de vue dans le volet Contenu de l'Explorateur de l'utilitaire. La collecte de données se poursuit une fois toutes les 15 minutes. Pour actualiser les données, cliquez avec le bouton droit sur le nœud **Instances gérées** dans le volet **Navigation de l’Explorateur de l’utilitaire** , puis sélectionnez **Actualiser**, ou cliquez avec le bouton droit sur le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le mode Liste, puis sélectionnez **Actualiser**.  
  
 Pour supprimer des instances gérées de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionnez **Instances gérées** dans le volet **Navigation de l’Explorateur de l’utilitaire** pour remplir le mode Liste d’instances gérées, cliquez avec le bouton droit sur le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans **Contenu de l’Explorateur de l’utilitaire** en mode Liste, puis sélectionnez **Rendre une instance non managée**.  
  
##  <a name="PowerShell_enroll"></a> Inscription d'une Instance de SQL Server à l'aide de PowerShell  
 Utilisez l'exemple suivant pour inscrire une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existant :  
  
```  
> $UtilityInstance = new-object -Type Microsoft.SqlServer.Management.Smo.Server "ComputerName\UCP-Name";  
> $SqlStoreConnection = new-object -Type Microsoft.SqlServer.Management.Sdk.Sfc.SqlStoreConnection $UtilityInstance.ConnectionContext.SqlConnectionObject;  
> $Utility = [Microsoft.SqlServer.Management.Utility.Utility]::Connect($SqlStoreConnection);  
> $Instance = new-object -Type Microsoft.SqlServer.Management.Smo.Server "ComputerName\ManagedInstanceName";  
> $InstanceConnection = new-object -Type Microsoft.SqlServer.Management.Sdk.Sfc.SqlStoreConnection $Instance.ConnectionContext.SqlConnectionObject;  
> $ManagedInstance = $Utility.EnrollInstance($InstanceConnection, "ProxyAccount", "ProxyPassword");  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Surveiller des instances de SQL Server dans l'utilitaire SQL Server](../../relational-databases/manage/monitor-instances-of-sql-server-in-the-sql-server-utility.md)   
 [Résolution des problèmes liés à l’utilitaire SQL Server](http://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453)  
  
  
