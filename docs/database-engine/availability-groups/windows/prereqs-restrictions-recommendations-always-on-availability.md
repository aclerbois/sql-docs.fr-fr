---
title: Prérequis, restrictions et recommandations - Groupes de disponibilité Always On | Microsoft Docs
ms.custom: ''
ms.date: 05/02/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], WSFC clusters
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], prerequisites and restrictions
- Availability Groups [SQL Server], Failover Cluster Instances
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: edbab896-42bb-4d17-8d75-e92ca11f7abb
caps.latest.revision: 151
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8d5b1b75df79f8422320089fe1a1a75fc890cfc8
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769735"
---
# <a name="prereqs-restrictions-recommendations---always-on-availability-groups"></a>Prérequis, restrictions et recommandations - Groupes de disponibilité Always On
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique décrit les considérations relatives au déploiement de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], notamment les prérequis, restrictions et recommandations concernant les ordinateurs hôtes, les clusters de basculement Windows Server (WSFC), les instances de serveur et les groupes de disponibilité. Pour chacun de ces composants, les considérations relatives à la sécurité et les autorisations requises (le cas échéant) sont indiquées.  
  
> [!IMPORTANT]  
>  Avant de déployer [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], nous vous recommandons de lire les sections de cette rubrique.  
    
##  <a name="DotNetHotfixes"></a> Correctifs logiciels .Net prenant en charge les groupes de disponibilité  
 Selon les composants et les fonctionnalités [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] que vous allez utiliser avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vous pourrez éventuellement avoir besoin d'installer des correctifs logiciels .Net supplémentaires identifiés dans le tableau suivant. Ces derniers peuvent être installés dans n'importe quel ordre.  
  
||Fonctionnalité dépendante|Correctif logiciel|Lien|  
|------|-----------------------|------------|----------|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]|Le correctif pour .Net 3.5 SP1 ajoute une prise en charge au client SQL concernant les fonctionnalités Always On d’intention de lecture (read-intent), de lecture seule (readonly) et de basculement à plusieurs sous-réseaux (multisubnetfailover). Le correctif doit être installé sur chaque serveur de rapports [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .|Article 2654347 de la base de connaissances : [Correctif pour .Net 3.5 SP1 pour l’ajout d’une prise en charge des fonctionnalités Always On](http://go.microsoft.com/fwlink/?LinkId=242896)|  
  
##  <a name="SystemReqsForAOAG"></a> Exigences et recommandations système de Windows  
 **Dans cette section :**  
  
-   [Liste de vérification des conditions requises](#SystemRequirements)  
  
-   [Recommandations pour les ordinateurs qui hébergent des réplicas de disponibilité (système Windows](#ComputerRecommendations)  
  
-   [Autorisations](#PermissionsWindows)  
  
-   [Tâches associées](#RelatedTasksWindows)  
  
-   [Contenu connexe](#RelatedContentWS)  
  
###  <a name="SystemRequirements"></a> Liste de vérification des conditions requises (système Windows)  
 Pour prendre en charge la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , vérifiez que chaque ordinateur devant participer à un ou plusieurs groupes de disponibilité respecte les conditions requises fondamentales suivantes :  
  
||Condition requise|Lien|  
|------|-----------------|----------|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Vérifiez que ce système n'est pas un contrôleur de domaine.|Les groupes de disponibilité ne sont pas pris en charge sur les contrôleurs de domaine.|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Vérifiez que chaque ordinateur exécute Windows Server 2012 ou versions ultérieures.|[Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Vérifiez que chaque ordinateur est un nœud dans un cluster WSFC.|[Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Vérifiez que le cluster WSFC contient suffisamment de nœuds pour prendre en charge vos configurations de groupe de disponibilité.|Un nœud de cluster peut héberger un seul réplica de disponibilité pour un groupe de disponibilité donné. Sur un nœud de cluster donné, une ou plusieurs instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peuvent héberger des réplicas de disponibilité pour de nombreux groupes de disponibilité.<br /><br /> Demandez à vos administrateurs de base de données le nombre de nœuds de cluster nécessaires pour prendre en charge les réplicas de disponibilité des groupes de disponibilité planifiés.<br /><br /> [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  
  
> [!IMPORTANT]  
>  Vérifiez que votre environnement est correctement configuré pour se connecter à un groupe de disponibilité. Pour plus d’informations, consultez [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
###  <a name="ComputerRecommendations"></a> Recommandations pour les ordinateurs qui hébergent des réplicas de disponibilité (système Windows)  
  
-   **Systèmes comparables :**  Pour un groupe de disponibilité donné, tous les réplicas de disponibilité doivent s'exécuter sur des systèmes comparables qui peuvent gérer des charges de travail identiques.  
  
-   **Cartes réseau dédiées :**  Pour des performances optimales, utilisez une carte d’interface réseau dédiée pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
-   **Espace disque suffisant :**  Chaque nœud WSFC sur lequel une instance de serveur héberge un réplica de disponibilité doit posséder l'espace disque suffisant pour toutes les bases de données du groupe de disponibilité. Gardez à l'esprit qu'à mesure que le volume des bases de données primaires croît, le volume des bases de données secondaires correspondantes augmente aussi.  
  
###  <a name="PermissionsWindows"></a> Autorisations (système Windows)  
 Pour administrer un cluster WSFC, l'utilisateur doit être administrateur système sur chaque nœud de cluster.  
  
 Pour plus d’informations sur le compte permettant d’administrer le cluster, consultez [Annexe A : conditions requises pour le cluster de basculement](http://technet.microsoft.com/library/dd197454.aspx).  
  
###  <a name="RelatedTasksWindows"></a> Tâches associées (système Windows)  
  
|Tâche|Lien|  
|----------|----------|  
|Définissez la valeur de HostRecordTTL.|[Modifiez HostRecordTTL (à l'aide de Windows PowerShell)](#ChangeHostRecordTTLps)|  
  
####  <a name="ChangeHostRecordTTLps"></a> Modifiez HostRecordTTL (à l'aide de Windows PowerShell)  
  
1.  Ouvrez la fenêtre PowerShell via **Exécuter en tant qu’administrateur**.  
  
2.  Importez le module FailoverClusters.  
  
3.  Utilisez l’applet de commande **Get-ClusterResource** pour rechercher la ressource de nom de réseau, puis utilisez l’applet de commande **Set-ClusterParameter** pour définir la valeur de **HostRecordTTL** , comme suit :  
  
     Get-ClusterResource "*NomRessourceRésesau>\<*" | Set-ClusterParameter HostRecordTTL *\<DuréeEnSecondes>*  
  
     L'exemple PowerShell suivant définit HostRecordTTL sur 300 secondes pour une ressource de nom réseau nommée `SQL Network Name (SQL35)`.  
  
    ```  
    Import-Module FailoverClusters  
  
    $nameResource = "SQL Network Name (SQL35)"  
    Get-ClusterResource $nameResource | Set-ClusterParameter ClusterParameter HostRecordTTL 300  
    ```  
  
    > [!TIP]  
    >  Chaque fois que vous ouvrez une nouvelle fenêtre PowerShell, vous devez importer le module **FailoverClusters** .  
  
##### <a name="related-content-powershell"></a>Contenu connexe (PowerShell)  
  
-   [Clustering and High-Availability](http://blogs.msdn.com/b/clustering/archive/2009/05/23/9636665.aspx) (Clustering et haute disponibilité) (Blog de l’équipe de clustering de basculement et d’équilibrage de la charge réseau)  
  
-   [Mise en route de Windows PowerShell sur un cluster de basculement](http://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Commandes de ressource de cluster et applets de commande Windows PowerShell équivalentes](http://msdn.microsoft.com/library/ee619744.aspx#BKMK_resource)  
  
###  <a name="RelatedContentWS"></a> Contenu connexe (système Windows)  
  
-   [Configurer les paramètres DNS dans un cluster de basculement multisite](http://technet.microsoft.com/library/dd197562\(WS.10\).aspx)  
  
-   [Enregistrement DNS avec ressource de nom réseau](http://blogs.msdn.com/b/clustering/archive/2009/07/17/9836756.aspx)  
  

##  <a name="ServerInstance"></a> Conditions préalables requises et restrictions pour une instance de SQL Server  
 Chaque groupe de disponibilité requiert un jeu de partenaires de basculement, appelés *réplicas de disponibilité*, qui sont hébergés par les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Une instance de serveur donnée peut être une *instance autonome* ou une [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]*instance de cluster de basculement* (FCI).  
  
 **Dans cette section :**  
  
-   [Liste de vérification : Conditions préalables requises](#PrerequisitesSI)  
  
-   [Utilisation de threads par les groupes de disponibilité](#ThreadUsage)  
  
-   [Autorisations](#PermissionsSI)  
  
-   [Tâches associées](#RelatedTasksSI)  
  
-   [Contenu connexe](#RelatedContentSI)  
  
###  <a name="PrerequisitesSI"></a> Liste de vérification : Conditions préalables requises (instance de serveur)  
  
||Condition préalable|Liens|  
|-|------------------|-----------|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Cet ordinateur hôte doit être un nœud WSFC. Les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergent les réplicas de disponibilité d’un groupe de disponibilité donné résident sur des nœuds distincts du cluster. Un groupe de disponibilité peut temporairement chevaucher deux clusters pendant sa migration vers un autre cluster. SQL Server 2016 introduit les groupes de disponibilité distribués. Dans un groupe de disponibilité distribué, deux groupes de disponibilité résident sur des clusters différents.|[Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)<br /><br /> [Clustering de basculement et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)<br/> <br/> [Groupes de disponibilité distribués (groupes de disponibilité Always On)](../../../database-engine/availability-groups/windows/distributed-availability-groups-always-on-availability-groups.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Si vous souhaitez qu'un groupe de disponibilité utilise Kerberos :<br /><br /> Toutes les instances de serveur qui hébergent un réplica de disponibilité pour le groupe de disponibilité doivent utiliser le même compte de service SQL Server.<br /><br /> L'administrateur de domaine doit inscrire manuellement un nom de principal de service (SPN) avec Active Directory sur le compte de service SQL Server pour le nom de réseau virtuel (VNN) de l'écouteur de groupe de disponibilité. Si le SPN est inscrit sur un compte différent du compte de service SQL Server, l'authentification échoue.<br /><br /> <br /><br /> <b>\*\* Important \*\*</b> Si vous modifiez le compte de service SQL Server, l’administrateur de domaine devra réinscrire le SPN manuellement.|[Inscrire un nom de principal du service pour les connexions Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)<br /><br /> **Brève explication :**<br /><br /> Kerberos et les SPN assurent une authentification mutuelle. Le SPN est mappé au compte Windows qui démarre les services SQL Server. Si l'inscription du SPN n'est pas effectuée correctement ou échoue, la couche de sécurité Windows ne peut pas déterminer le compte associé au SPN et l'authentification Kerberos ne peut pas être utilisée.<br /><br /> <br /><br /> Remarque : NTLM n’est pas soumis à cette condition.|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Si vous envisagez d'utiliser une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour héberger un réplica de disponibilité, assurez-vous de comprendre les restrictions relatives à l'instance de cluster de basculement et de respecter les conditions préalables requises pour cette dernière.|[Restrictions et conditions préalables requises pour l’utilisation d’une instance de cluster de basculement SQL Server afin d’héberger un réplica de disponibilité](#FciArLimitations) (plus loin dans cette rubrique)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Chaque instance de serveur doit exécuter [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]Enterprise Edition.|[Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../../sql-server/editions-and-supported-features-for-sql-server-2016.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Toutes les instances de serveur qui hébergent des réplicas de disponibilité pour un groupe de disponibilité doivent utiliser le même classement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Définir ou modifier le classement du serveur](../../../relational-databases/collations/set-or-change-the-server-collation.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Activez la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur chaque instance de serveur qui hébergera un réplica de disponibilité pour un groupe de disponibilité. Sur un ordinateur donné, vous pouvez activer autant d'instances de serveur que vous le souhaitez pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , dans la limite du nombre pris en charge par votre installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)<br /><br /> <br /><br /> <b>\*\* Important \*\*</b> Si vous supprimez et recréez un cluster WSFC, vous devez désactiver puis réactiver la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur chaque instance de serveur qui était activée pour les [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur le cluster d’origine.|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Chaque instance de serveur nécessite un point de terminaison de mise en miroir de bases de données. Notez que ce point de terminaison est partagé par tous les réplicas de disponibilité, ainsi que par tous les serveurs partenaires et témoins de mise en miroir de bases de données sur l'instance de serveur.<br /><br /> Si une instance de serveur que vous sélectionnez pour héberger un réplica de disponibilité s’exécute sous un compte d’utilisateur de domaine et n’a pas encore de point de terminaison de mise en miroir de bases de données, l’ [Assistant Nouveau groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md) (ou l’ [Assistant Ajouter un réplica au groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)) peut créer le point de terminaison et accorder l’autorisation CONNECT au compte de service de l’instance de serveur. Toutefois, si le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute en tant que compte intégré, tel que Système local, Service local ou Service réseau, ou comme compte qui n'appartient pas au domaine, vous devez utiliser des certificats pour l'authentification du point de terminaison, et l'Assistant ne peut pas créer un point de terminaison de mise en miroir de bases de données sur l'instance de serveur. Dans ce cas, nous recommandons de créer les points de terminaison de mise en miroir de bases de données manuellement avant de lancer l'Assistant.<br /><br /> <br /><br /> <b>\*\* Note de sécurité \*\*</b> La sécurité du transport pour les [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] est la même que pour la mise en miroir de bases de données.|[Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)<br /><br /> [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Si des bases de données utilisant FILESTREAM doivent être ajoutées à un groupe de disponibilité, vérifiez que FILESTREAM est activé sur chaque instance de serveur qui hébergera un réplica de disponibilité pour le groupe de disponibilité.|[Activer et configurer FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Si des bases de données à relation contenant-continu doivent être ajoutées à un groupe de disponibilité, vérifiez que l'option de serveur **contained database authentication** a la valeur **1** sur chaque instance de serveur qui hébergera un réplica de disponibilité pour le groupe de disponibilité.|[contained database authentication (option de configuration de serveur)](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Options de configuration de serveur &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
###  <a name="ThreadUsage"></a> Utilisation de threads par les groupes de disponibilité  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pose les conditions suivantes pour les threads de travail :  
  
-   Sur une instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]inactive, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] n'utilise aucun thread.  
  
-   Le nombre maximal de threads utilisés par les groupes de disponibilité est le paramètre configuré pour le nombre maximal de threads serveur («**max worker threads**») moins 40.  
  
-   Les réplicas de disponibilité hébergés sur une instance de serveur spécifique partagent le même pool de threads.  
  
     Les threads sont partagés à la demande, comme suit :  
  
    -   En général, il existe 3 à 10 threads partagés, mais ce nombre peut augmenter en fonction de la charge de travail du réplica principal.  
  
    -   Si un thread donné est inactif pendant un certain temps, il est remis à disposition dans le pool de threads [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à usage général. Normalement, un thread inactif est libéré après environ 15 secondes d'inactivité. Toutefois, selon la dernière activité, un thread inactif peut être conservé plus longtemps.  

    - Une instance de SQL Server utilise jusqu’à 100 threads de restauration par progression parallèle pour les réplicas secondaires. Chaque base de données utilise jusqu’à la moitié du nombre total de cœurs de processeur, mais pas plus de 16 threads par base de données. Si le nombre total de threads requis pour une seule instance est supérieur à 100, SQL Server utilise un thread redo unique pour chaque base de données restante. Les threads de restauration par progression sont libérés après environ 15 secondes d'inactivité. 

  
-   En outre, les groupes de disponibilité utilisent des threads non partagés, comme suit :  
  
    -   Chaque réplica principal utilise 1 thread de capture du journal pour chaque base de données principale. En outre, il utilise un thread d'envoi du journal pour chaque base de données secondaire. Les threads d'envoi du journal sont libérés après environ 15 secondes d'inactivité.    
  
    -   Une sauvegarde sur un réplica secondaire contient un thread sur le réplica principal pour la durée de l'opération de sauvegarde.  
  
 Pour plus d’informations, consultez [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](http://blogs.msdn.com/b/psssql/archive/2012/05/17/Always%20On-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases.aspx) (Always On - Série de formations HADRON : Utilisation du pool de travail pour les bases de données HADRON) (blog des ingénieurs du support technique [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ).  
  
###  <a name="PermissionsSI"></a> Autorisations (instance de serveur)  
  
|Tâche|Autorisations requises|  
|----------|--------------------------|  
|Création du point de terminaison de mise en miroir de bases de données|Requiert l'autorisation CREATE ENDPOINT ou l'appartenance au rôle serveur fixe **sysadmin** .  Requiert également l'autorisation CONTROL ON ENDPOINT. Pour plus d’informations, consultez [Autorisations de point de terminaison GRANT &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).|  
|Activation de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]|Nécessite l'appartenance au groupe **Administrateur** sur l'ordinateur local et le contrôle total sur le cluster WSFC.|  
  
###  <a name="RelatedTasksSI"></a> Tâches associées (instance de serveur)  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Détermination de l'existence du point de terminaison de mise en miroir de bases de données|[sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)|  
|Création du point de terminaison de mise en miroir de bases de données (s'il n'existe pas encore)|[Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)<br /><br /> [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)<br /><br /> [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)|  
|Activation des groupes de disponibilité|[Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)|  
  
###  <a name="RelatedContentSI"></a> Contenu connexe (instance serveur)  
  
-   [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](http://blogs.msdn.com/b/psssql/archive/2012/05/17/Always%20On-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases.aspx)  
  
##  <a name="NetworkConnect"></a> Recommandations relatives à la connectivité réseau  
 Nous vous recommandons vivement d'utiliser les mêmes liaisons réseau pour les communications entre les nœuds de cluster WSFC et les communications entre les réplicas de disponibilité.  L'utilisation de liaisons réseau distinctes peut provoquer des comportements inattendus en cas d'échec de certaines liaisons (et ce, même de façon intermittente).  
  
 Par exemple, pour qu'un groupe de disponibilité prenne en charge le basculement automatique, le réplica secondaire qui est le partenaire de basculement automatique doit être dans un état SYNCHRONIZED. Si la liaison réseau à ce réplica secondaire échoue (et ce, même de faon intermittente), le réplica passe à l'état UNSYNCHRONIZED et ne peut pas se resynchroniser tant que la liaison n'est pas restaurée. Si le cluster WSFC demande un basculement automatique alors que le réplica secondaire n'est pas synchronisé, le basculement automatique n'a pas lieu.  
  
##  <a name="ClientConnSupport"></a> Prise en charge de la connectivité client  
 Pour plus d’informations sur la prise en charge des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour la connectivité client, consultez [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
##  <a name="FciArLimitations"></a> Conditions préalables requises et restrictions pour l'utilisation d'une instance de cluster de basculement SQL Server afin d'héberger un réplica de disponibilité  
 **Dans cette section :**  
  
-   [Restrictions](#RestrictionsFCI)  
  
-   [Liste de vérification : Conditions préalables requises](#PrerequisitesFCI)  
  
-   [Tâches associées](#RelatedTasksFCIs)  
  
-   [Contenu connexe](#RelatedContentFCIs)  
  
###  <a name="RestrictionsFCI"></a> Restrictions (instances de cluster de basculement)  
  
> [!NOTE]  
> Les instances de cluster de basculement prennent en charge les volumes partagés de cluster. Pour plus d'informations sur les volumes partagés de cluster, consultez [Présentation des volumes partagés de cluster dans un cluster de basculement](http://technet.microsoft.com/library/dd759255.aspx).  
  
-   **Les nœuds de cluster d'une instance de cluster de basculement peuvent héberger un seul réplica pour un groupe de disponibilité donné :**  Si vous ajoutez un réplica de disponibilité sur une instance de cluster de basculement, les nœuds de cluster WSFC qui sont des propriétaires d'instance de cluster de basculement potentiels ne peuvent pas héberger un autre réplica pour le même groupe de disponibilité.  
  
     Par ailleurs, chacun des autres réplicas doit être hébergé par une instance de SQL Server 2016 qui réside sur un autre nœud de cluster dans le même cluster WSFC. La seule exception survient lors de la migration vers un autre cluster : un groupe de disponibilité peut temporairement chevaucher deux clusters.  
  
-   **Les instances de cluster de basculement ne prennent pas en charge le basculement automatique par les groupes de disponibilité :**  Les instances de cluster de basculement ne prennent pas en charge le basculement automatique par les groupes de disponibilité ; par conséquent, tout réplica de disponibilité hébergé par une instance de cluster de basculement peut uniquement être configuré pour le basculement manuel.  
  
-   **Modification du nom réseau d’une instance de cluster de basculement :**  si vous devez modifier le nom réseau d’une instance de cluster de basculement qui héberge un réplica de disponibilité, vous devez supprimer le réplica de son groupe de disponibilité, puis l’ajouter de nouveau au groupe de disponibilité. Étant donné que vous ne pouvez pas supprimer le réplica principal, si vous renommez une instance de cluster de basculement qui héberge le réplica principal, vous devez basculer vers un réplica secondaire, puis supprimer le réplica principal précédent avant de l'ajouter à nouveau. Notez que l'attribution d'un nouveau nom à une instance de cluster de basculement modifie l'URL de son point de terminaison de mise en miroir de bases de données. Lorsque vous ajoutez le réplica, veillez à spécifier l'URL du point de terminaison actuel.  
  
###  <a name="PrerequisitesFCI"></a> Liste de vérification : Conditions préalables (instances de cluster de basculement)  
  
||Condition préalable|Lien|  
|-|------------------|----------|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Vérifiez que chaque instance de cluster de basculement SQL Server dispose du stockage partagé requis, conformément à l'installation standard de l'instance de cluster de basculement SQL Server.||  
  
###  <a name="RelatedTasksFCIs"></a> Tâches associées (instances de cluster de basculement)  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Installation d'un cluster de basculement SQL Server|[Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Mise à niveau sur place de votre cluster de basculement SQL Server existant|[Mettre à niveau une instance de cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance-setup.md)|  
|Maintenance de votre cluster de basculement SQL Server existant|[Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)|  
  
###  <a name="RelatedContentFCIs"></a> Contenu connexe (instances de cluster de basculement)  
  
-   [Clustering de basculement et groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)  
  
-   [Guide de l’architecture Always On : Création d’une solution haute disponibilité et de récupération d’urgence en utilisant des instances de cluster de basculement et des groupes de disponibilité](http://technet.microsoft.com/library/jj215886.aspx)  
  
##  <a name="PrerequisitesForAGs"></a> Conditions préalables requises et restrictions pour les groupes de disponibilité  
 **Dans cette section :**  
  
-   [Restrictions](#RestrictionsAG)  
  
-   [Spécifications](#RequirementsAG)  
  
-   [Sécurité](#SecurityAG)  
  
-   [Tâches associées](#RelatedTasksAGs)  
  
###  <a name="RestrictionsAG"></a> Restrictions (groupes de disponibilité)  
  
-   **Les réplicas de disponibilité doivent être hébergés par différents nœuds d'un cluster WSFC :**  Pour un groupe de disponibilité donné, les réplicas de disponibilité doivent être hébergés par des instances de serveur qui s'exécutent sur différents nœuds du même cluster WSFC. La seule exception survient lors de la migration vers un autre cluster : un groupe de disponibilité peut temporairement chevaucher deux clusters.  
  
    > [!NOTE]  
    >  Les ordinateurs virtuels situés sur le même ordinateur physique peuvent chacun héberger un réplica de disponibilité pour le même groupe de disponibilité, étant donné que chaque ordinateur virtuel agit en tant qu'ordinateur distinct.  
  
-   **Nom de groupe de disponibilité unique :**  Chaque nom de groupe de disponibilité doit être unique sur le cluster WSFC. La longueur maximale d'un nom de groupe de disponibilité est de 128 caractères.  
  
-   **Réplicas de disponibilité :**  chaque groupe de disponibilité prend en charge un réplica principal et jusqu'à huit réplicas secondaires. Tous les réplicas peuvent s'exécuter en mode de validation asynchrone, ou au plus trois d'entre eux peuvent s'exécuter en mode de validation synchrone (un réplica principal et deux réplicas secondaires synchrones).  
  
-   **Nombre maximal de groupes de disponibilité et de bases de données de disponibilité par ordinateur :** le nombre réel de bases de données et de groupes de disponibilité que vous pouvez mettre sur un ordinateur (virtuel ou physique) dépend du matériel et de la charge de travail, mais il n’existe aucune limite imposée. Microsoft a largement testé des systèmes comportant 10 100 groupes de disponibilité et 100 bases de données par ordinateur physique. Les signes d’un système surchargé incluent, notamment, l’épuisement des threads de travail, des temps de réponse longs pour les vues système et les vues de gestion dynamique des groupes de disponibilité et/ou des vidages de système de répartiteur bloqués. Veillez à tester soigneusement votre environnement avec une charge de travail semblable à celle de production pour vous assurer qu'il peut gérer la capacité de pointe conformément au contrat de niveau de service de l'application. Lorsque vous choisissez les contrats de niveau de service, assurez-vous de prendre en compte la charge en conditions d'échec ainsi que les temps de réponse attendus.  
  
-   **N'utilisez pas le gestionnaire du cluster de basculement pour manipuler des groupes de disponibilité :**  
  
     Exemple :  
  
    -   Ne modifiez pas les propriétés du groupe de disponibilité, telles que les propriétaires possibles.  
  
    -   N'utilisez pas le gestionnaire du cluster de basculement pour basculer des groupes de disponibilité. Vous devez utiliser [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
###  <a name="RequirementsAG"></a> Conditions préalables requises (groupes de disponibilité)  
 Lors de la création ou de la reconfiguration d'un groupe de disponibilité, veillez à respecter les conditions préalables requises suivantes.  
  
||Condition préalable|Description|  
|-|------------------|-----------------|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Si vous envisagez d'utiliser une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour héberger un réplica de disponibilité, assurez-vous de comprendre les restrictions relatives à l'instance de cluster de basculement et de respecter les conditions préalables requises pour cette dernière.|[Conditions préalables requises et restrictions pour l’utilisation d’une instance de cluster de basculement SQL Server afin d’héberger un réplica de disponibilité](#FciArLimitations) (plus haut dans cette rubrique)|  
  
###  <a name="SecurityAG"></a> Sécurité (groupes de disponibilité)  
  
-   La sécurité est héritée du cluster WSFC. Le clustering de basculement Windows Server fournit deux niveaux de sécurité utilisateur avec la granularité de l’ensemble du cluster :  
  
    -   Accès en lecture seule  
  
    -   Contrôle total  
  
         [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nécessite un contrôle total, et l’activation de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lui donne le contrôle total du cluster (par le biais du SID du service).  
  
         Vous ne pouvez pas ajouter ou supprimer directement la sécurité d'une instance de serveur dans le Gestionnaire du cluster. Pour gérer les sessions de sécurité de cluster, utilisez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou le WMI équivalent de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit disposer des autorisations d'accès au Registre, cluster, etc.  
  
-   Nous vous recommandons d'utiliser le chiffrement pour les connexions entre des instances de serveur qui hébergent des réplicas de disponibilité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
#### <a name="permissions-availability-groups"></a>Autorisations (groupes de disponibilité)  
  
|Tâche|Autorisations requises|  
|----------|--------------------------|  
|Création d'un groupe de disponibilité|Requiert l’appartenance au rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.|  
|Modification d'un groupe de disponibilité|Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.<br /><br /> En outre, la jointure d’une base de données à un groupe de disponibilité nécessite l’appartenance au rôle de base de données fixe **db_owner** .|  
|Suppression d'un groupe de disponibilité|Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER. Pour supprimer un groupe de disponibilité qui n'est pas hébergé à l'emplacement du réplica local, vous avez besoin de l'autorisation CONTROL SERVER ou CONTROL sur ce groupe de disponibilité.|  
  
###  <a name="RelatedTasksAGs"></a> Tâches associées (groupes de disponibilité)  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Création d'un groupe de disponibilité|[Utiliser le groupe de disponibilité (Assistant Nouveau groupe de disponibilité)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)<br /><br /> [Créer un groupe de disponibilité (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)<br /><br /> [Créer un groupe de disponibilité (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)<br /><br /> [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)|  
|Modification du nombre de réplicas de disponibilité|[Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Supprimer un réplica secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
|Création d'un écouteur de groupe de disponibilité|[Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|Suppression d'un groupe de disponibilité|[Supprimer un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)|  
  
##  <a name="PrerequisitesForDbs"></a> Conditions préalables requises et restrictions pour les bases de données de disponibilité  
 Pour pouvoir être ajoutée à un groupe de disponibilité, une base de données doit respecter les Conditions préalables requises et restrictions suivantes.  
  
 **Dans cette section :**  
  
-   [Spécifications](#RequirementsDb)  
  
-   [Restrictions](#RestrictionsDb)  
  
-   [Recommandations pour les ordinateurs qui hébergent des réplicas de disponibilité (système Windows](#TDEdbs)  
  
-   [Autorisations](#PermissionsDbs)  
  
-   [Tâches associées](#RelatedTasksADb)  
  
###  <a name="RequirementsDb"></a> Liste de vérification : Conditions préalables requises (bases de données de disponibilité)  
 Pour pouvoir être ajoutée à un groupe de disponibilité, une base de données :  
  
||Spécifications|Lien|  
|-|------------------|----------|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|doit être une base de données utilisateur. Les bases de données système ne peuvent pas appartenir à un groupe de disponibilité ;||  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|doit résider sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] où vous créez le groupe de disponibilité et être accessible à l'instance de serveur ;||  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|doit être une base de données en lecture/écriture. Les bases de données en lecture seule ne peuvent pas être ajoutées à un groupe de disponibilité ;|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_read_only** = 0)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|doit être une base de données multi-utilisateur ;|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**user_access** = 0)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|ne doit pas utiliser AUTO_CLOSE ;|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_auto_close_on** = 0)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Utilisez le mode de récupération complète (également appelé modèle de récupération complète).|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**recovery_model** = 1)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Possédez au moins une sauvegarde complète de base de données.<br /><br /> Remarque : une fois que vous avez défini une base de données en mode de récupération complète, une sauvegarde complète est requise pour initialiser la séquence de journaux de récupération complète.|[Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|ne doit pas appartenir à un groupe de disponibilité existant ;|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**group_database_id** = NULL)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|ne doit pas être configurée pour la mise en miroir de base de données.|[sys.database_mirroring](../../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) (Si la base de données ne participe pas à la mise en miroir, toutes les colonnes qui utilisent le préfixe « mirroring » ont la valeur NULL.)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Avant d'ajouter une base de données qui utilise FILESTREAM à un groupe de disponibilité, vérifiez que FILESTREAM est activé sur chaque instance de serveur qui héberge ou hébergera un réplica de disponibilité pour le groupe de disponibilité.|[Activer et configurer FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Avant d'ajouter une base de données à relation contenant-continu à un groupe de disponibilité, vérifiez que l'option de serveur **contained database authentication** a la valeur **1** sur chaque instance de serveur qui héberge ou hébergera un réplica de disponibilité pour le groupe de disponibilité.|[contained database authentication (option de configuration de serveur)](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Options de configuration de serveur &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
> [!NOTE]  
>  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] fonctionne avec tous les niveaux de compatibilité de base de données pris en charge.  
  
###  <a name="RestrictionsDb"></a> Restrictions (bases de données de disponibilité)  
  
-   Si le chemin d'accès du fichier (notamment la lettre de lecteur) d'une base de données secondaire diffère du chemin d'accès de la base de données primaire correspondante, les restrictions suivantes s'appliquent :  
  
    -   **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]/[!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)]:**  L’option **Complète** n’est pas prise en charge (dans la page[Sélectionner la synchronisation de données initiale](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md) ).  
  
    -   **RESTORE WITH MOVE :**  Pour créer les bases de données secondaires, les fichiers de base de données doivent avoir l'état RESTORED WITH MOVE sur chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge un réplica secondaire.  
  
    -   **Incidence sur les opérations d’ajout de fichier :**  Une opération d’ajout de fichier ultérieure sur le réplica principal peut échouer sur les bases de données secondaires. Cet échec peut entraîner l'interruption des bases de données secondaires. Par voie de conséquence, les réplicas secondaires passent à l'état NOT SYNCHRONIZING.  
  
        > [!NOTE]  
        >  Pour plus d’informations sur la marche à suivre en cas d’échec d’une opération d’ajout de fichier, consultez [Résoudre une opération d’ajout de fichier ayant échoué &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md).  
  
-   Vous ne pouvez pas supprimer une base de données qui appartient à un groupe de service.  
  
###  <a name="TDEdbs"></a> Suivi des bases de données protégées par le chiffrement transparent des données  
 Si vous utilisez le chiffrement transparent des données (TDE), le certificat ou la clé asymétrique pour la création et le déchiffrement d'autres clés doit être identique sur chaque instance de serveur qui héberge un réplica de disponibilité pour le groupe de disponibilité. Pour plus d’informations, consultez [Déplacer une base de données protégée par le chiffrement transparent des données vers un autre serveur SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md).  
  
###  <a name="PermissionsDbs"></a> Autorisations (bases de données de disponibilité)  
 Nécessite l'autorisation ALTER sur la base de données.  
  
###  <a name="RelatedTasksADb"></a> Tâches associées (bases de données de disponibilité)  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Préparation d'une base de données secondaire (manuellement)|[Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)|  
|Jointure d'une base de données secondaire à un groupe de disponibilité (manuellement)|[Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)|  
|Modification du nombre de bases de données de disponibilité|[Ajouter une base de données à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)<br /><br /> [Supprimer une base de données secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)<br /><br /> [Supprimer une base de données primaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)|  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence)](http://go.microsoft.com/fwlink/?LinkId=227600)  
  
-   [Blog de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](https://blogs.msdn.microsoft.com/sqlalwayson/)  
  
-   [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](http://blogs.msdn.com/b/psssql/archive/2012/05/17/Always%20On-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases.aspx)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Clustering de basculement et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
    
  
--------------------------------------------------  

