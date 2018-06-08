---
title: Éditions et fonctionnalités prises en charge de SQL Server 2017 | Microsoft Docs
ms.custom: ''
ms.date: 11/10/2017
ms.prod: sql
ms.prod_service: sql
ms.component: sql-non-specified
ms.reviewer: ''
ms.suite: sql
ms.technology:
- server-general
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
ms.assetid: ''
caps.latest.revision: 121
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 3cda2359f1e6ae92e49331bbd2bd3471107d2fdf
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="editions-and-supported-features-of-sql-server-2017"></a>Éditions et fonctionnalités prises en charge de SQL Server 2017
[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

Cette rubrique fournit des détails sur les fonctionnalités prises en charge par les différentes éditions de SQL Server 2017. 

Pour plus d’informations sur les versions antérieures, consultez :

* [SQL Server 2016](editions-and-components-of-sql-server-2016.md).  
* [SQL Server 2014](http://msdn.microsoft.com/library/cc645993(v=sql.120).aspx).

  
La configuration requise pour l'installation varie selon vos besoins applicatifs. Les différentes éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'adaptent aux exigences de chaque organisation et de chaque individu en termes de performances, d'exécution et de prix. Les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que vous installez dépendent également de vos exigences spécifiques. Les sections suivantes vous aident à choisir parmi les éditions et les composants disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

La version d’évaluation de SQL Server est disponible pendant une période d’évaluation de 180 jours.  
  
Pour obtenir les notes de publication les plus récentes et des informations sur les nouveautés, consultez les rubriques suivantes :
- [Notes de publication de SQL Server 2017](../sql-server/sql-server-2017-release-notes.md)
- [Nouveautés de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)

### <a name="try-sql-server"></a>Essayez SQL Server !    
    
> [![Télécharger à partir du Centre d’évaluation](../analysis-services/media/download.png)](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2017-ctp/) **[Télécharger SQL Server 2017 à partir du Centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-ctp/)**    

<!---    
> ![Azure Virtual Machine small](../analysis-services/media/azure-virtual-machine-small.png) **[Spin up a Virtual Machine with SQL Server 2016 already installed](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQL2016SP1-WS2016?tab=Overview?wt.mc_id=sqL16_vm)**   
--->

## <a name="includessnoversionincludesssnoversion-mdmd-editions"></a>Éditions de[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]   
 Le tableau ci-dessous décrit les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 
  
|Édition de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] |Définition|  
|---------------------------------------|----------------|  
|Enterprise|Offre Premium, l'édition [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise fournit des fonctions Datacenter avancées et complètes avec des performances ultra-rapides, une virtualisation illimitée et des fonctions Business Intelligence de bout en bout qui autorisent un haut niveau de service pour les charges de travail critiques et l'accès de l'utilisateur aux analyses de données.|  
|Standard|L'édition[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard permet la gestion des données de base et inclut une base de données Business Intelligence destinée aux services des grandes entreprises comme aux PME, leur permettant d'exécuter les applications et prenant en charge des outils de développement communs sur site et dans le cloud, pour une gestion efficace des bases de données avec des ressources informatiques minimales.|  
|Web|L'édition Web[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] est une option offrant un coût total de possession faible destinée aux hébergeurs Web et aux VAP Web, fournissant des fonctions évolutives, rentables et gérables aux propriétés Web à petite ou grande échelle.|  
|Développeur|L'édition[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer permet aux développeurs de créer des applications basées sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il inclut toutes les fonctionnalités de l'édition Enterprise, mais sa licence permet uniquement de l'utiliser comme un système de développement et de test, et non comme un serveur de production. L'édition [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer est la solution idéale pour le développement et le test d'applications.|  
|Éditions Express|L’édition Express est une édition de base comprenant une base de données gratuite, idéale pour découvrir et créer des applications bureautiques et de petites applications serveur pilotées par les données. C'est la solution idéale pour les éditeurs de logiciels, les développeurs et les amateurs de création d'applications clientes. Si vous avez besoin de fonctionnalités de base de données plus évoluées, vous pouvez mettre à niveau de manière transparente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express vers des versions plus sophistiquées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB, une version allégée d’Express qui conserve toutes les fonctions de programmabilité de ce dernier, s’exécute en mode utilisateur, s’installe rapidement sans aucune configuration et n’exige que peu de prérequis.|  
  
## <a name="using-includessnoversionincludesssnoversion-mdmd-with-an-internet-server"></a>Utilisation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec un serveur Internet  
 Sur un serveur Internet, comme un serveur exécutant les services Internet (IIS), vous installez généralement les outils clients [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Les outils clients comprennent les composants de connectivité clients utilisés par une application qui se connecte à une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
>[!NOTE]
>Bien que vous puissiez installer une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant les services Internet (IIS), ceci ne se fait en général que pour des sites Web de petite taille qui ne possèdent qu'un seul ordinateur serveur. La plupart des sites Web disposent de leur système IIS de niveau intermédiaire sur un serveur ou sur un cluster de serveurs, et de leurs bases de données sur un serveur distinct ou sur une fédération distincte de serveurs.  
  
## <a name="using-includessnoversionincludesssnoversion-mdmd-with-clientserver-applications"></a>Utilisation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec des applications client/serveur  
 Vous pouvez installer uniquement les composants clients de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant des applications client/serveur qui se connectent directement à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Une installation de composants clients est également un bon choix si vous administrez une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un serveur de base de données ou si vous prévoyez de développer des applications [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 L'option d'outils clients installe les fonctionnalités suivantes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] : composants de compatibilité descendante, [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], composants de connectivité, outils d'administration, kit de développement logiciel et composants de la documentation en ligne de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez  [Installer SQL Server](../database-engine/install-windows/install-sql-server.md).  
  
## <a name="deciding-among-includessnoversionincludesssnoversion-mdmd-components"></a>Choix des composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  
 Utilisez la page de sélection de composant de l'Assistant Installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour sélectionner les composants à inclure dans une installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Par défaut, aucune des fonctionnalités incluses dans l'arborescence n'est sélectionnée.  
  
 Utilisez les informations des tableaux suivants pour déterminer le jeu de fonctionnalités qui répond le mieux à vos besoins.  
  
|Composants serveur|Description|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Le[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] inclut le [!INCLUDE[ssDE](../includes/ssde-md.md)], le service principal de stockage, de traitement et de protection des données, la réplication, la recherche en texte intégral, les outils de gestion de données relationnelles et XML, l’intégration analytique dans la base de données et l’intégration de Polybase pour accéder à Hadoop et à d’autres sources de données hétérogènes, ainsi que le serveur [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS).|  
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] inclut les outils de création et de gestion d’applications de traitement analytique en ligne (OLAP, OnLine Analytical Processing) et d’exploration de données.|  
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] inclut les composants serveur et clients permettant de créer, de gérer et de déployer des rapports tabulaires, de matrice, graphiques et de forme libre. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est également une plateforme évolutive que vous pouvez utiliser pour développer des applications de création de rapports.|  
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] propose un ensemble d’outils graphiques et d’objets programmables permettant de déplacer, de copier et de transformer les données. Il inclut également le composant [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) pour [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) est la solution [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de gestion des données de référence. MDS peut être configuré pour gérer tout domaine (produits, clients, comptes) et inclut des hiérarchies, une sécurité granulaire, des transactions, la gestion des versions des données et les règles métier, ainsi que [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)], pouvant être utilisé pour gérer les données.|  
|Machine Learning Services (en base de données)|Machine Learning Services (en base de données) prend en charge les solutions Machine Learning distribuées et scalables avec des sources de données d’entreprise. Dans SQL Server 2016, le langage R était pris en charge. SQL Server 2017 prend en charge R et Python.|
|Machine Learning Server (autonome)|Machine Learning Server (autonome) prend en charge le déploiement de solutions Machine Learning distribuées et scalables sur plusieurs plateformes et avec plusieurs sources de données d’entreprise, notamment Linux, Hadoop et Teradata. Dans SQL Server 2016, le langage R était pris en charge. SQL Server 2017 prend en charge R et Python.|

  
|Outils d'administration|Description|  
|----------------------|-----------------|  
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] est un environnement intégré permettant d'accéder aux composants de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]en vue de les configurer, de les gérer, de les administrer et de les développer. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] permet aux développeurs et aux administrateurs de tous niveaux de compétence d'utiliser [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].<br /><br /> Téléchargez et installez <br />                [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] à partir de  [Télécharger SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx)|  
|Gestionnaire de configuration[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] |Le Gestionnaire de configuration[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permet de gérer la configuration de base des services [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , des protocoles serveur et clients et des alias clients.|  
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] fournit une interface utilisateur graphique qui permet d’analyser une instance du [!INCLUDE[ssDE](../includes/ssde-md.md)] ou d’ [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|  
|Assistant Paramétrage du[!INCLUDE[ssDE](../includes/ssde-md.md)] |L'Assistant Paramétrage du[!INCLUDE[ssDE](../includes/ssde-md.md)] permet de créer des ensembles d'index, des vues indexées et des partitions optimaux.|  
|Data Quality Client|Fournit une interface utilisateur graphique très facile et intuitive pour se connecter au serveur DQS et effectue des opérations de nettoyage des données. Il vous permet également de surveiller de façon centralisée différentes activités effectuées pendant l'opération de nettoyage des données.|  
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] fournissent un environnement de développement intégré (IDE) permettant de générer des solutions pour les composants Business Intelligence : [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], et [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Anciennement, Business Intelligence Development Studio.)<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] inclut également des « projets de base de données », qui offrent un environnement intégré aux développeurs, afin qu'ils puissent mener à bien leurs travaux de création de bases de données quelle que soit la plateforme [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (sur site et hors site) dans Visual Studio. Les développeurs de bases de données peuvent utiliser l'explorateur de serveurs amélioré de Visual Studio pour créer ou modifier facilement des objets de base de données et des données, ou exécuter des requêtes.|  
|Composants de connectivité|Installe des composants permettant la communication entre les clients et les serveurs, et des bibliothèques réseau pour DB-Library, ODBC et OLE DB.|  
  
|Documentation|Description|  
|-------------------|-----------------|  
|Documentation en ligne[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] |Documentation de base de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].| 

**Éditions Developer et Evaluation**  
Pour connaître les fonctionnalités prises en charge par les éditions Developer et Evaluation, consultez les fonctionnalités répertoriées pour SQL Server Entreprise Edition dans les tableaux ci-dessous.

L’édition Developer continue à prendre en charge seulement 1 client pour [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="Cross-BoxScaleLimits"></a> Limites d’échelle  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|Limité à moins de 1 socket ou 4 cœurs| 
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ou [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|Limité à moins de 1 socket ou 4 cœurs|  
|Mémoire maximale du pool de mémoires tampons par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum du système d’exploitation|128 Go|64 Go|1410 Mo|1410 Mo|
|Mémoire maximale du cache de segments columnstore par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go| 16 Go| 352 Mo| 352 Mo|  
|Taille maximale des données à mémoire optimisée par base de données dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go| 16 Go| 352 Mo| 352 Mo|  
|Mémoire maximale utilisée par instance de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Maximum du système d’exploitation|Tabulaire : 16 Go<br /><br /> MOLAP : 64 Go|Néant|Néant|Néant|  
|Mémoire maximale utilisée par instance de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d’exploitation|64 Go|64 Go|4 Go|Néant|
|Taille maximale de la base de données relationnelle|524 Po|524 Po|524 Po|10 GB|10 GB|  
  
<sup>1</sup> L’édition Enterprise avec serveur + licences d’accès client (CAL) (non disponibles pour les nouveaux contrats) est limitée à un maximum de 20 cœurs par instance SQL Server. Il n'existe aucune limite dans le mode de licence Serveur selon le nombre de cœurs. Pour plus d’informations, voir [Compute Capacity Limits by Edition of SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
 
##  <a name="RDBMSHA"></a> Haute disponibilité SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Prise en charge de Server Core <sup>1</sup>|Oui|Oui|Oui|Oui|Oui|  
|Copie des journaux de transaction|Oui|Oui|Oui|non|non|  
|Mise en miroir de bases de données|Oui|Oui<br /><br /> Sécurité complète uniquement|Témoin uniquement|Témoin uniquement|Témoin uniquement| 
|Compression de sauvegarde|Oui|Oui|non|non|non| 
|Instantané de base de données|Oui|Oui|Oui|Oui|Oui|
|Instances de cluster de basculement Always On<sup>2</sup>|Oui|Oui|non|non|non|  
|Groupes de disponibilité Always On<sup>3</sup>|Oui|non|non|non|non|
|Groupes de disponibilité de base<sup>4</sup>|non|Oui|non|non|non|
|Restauration en ligne de pages et de fichiers|Oui|non|non|non|non|
|Indexation en ligne|Oui|non|non|non|non|
|Reconstructions d’index en ligne pouvant être reprises|Oui|non|non|non|non|
|Modification de schéma en ligne|Oui|non|non|non|non|
|Récupération rapide|Oui|non|non|non|non|
|Sauvegardes en miroir|Oui|non|non|non|non|
|Ajout de mémoire et de processeur à chaud|Oui|non|non|non|non|
|Assistant de récupération de base de données|Oui|Oui|Oui|Oui|Oui|
|Sauvegarde chiffrée|Oui|Oui|non|non|non|
|Sauvegarde hybride vers Windows Azure (sauvegarde vers une URL)|Oui|Oui|non|non|non|
|Groupe de disponibilité sans cluster|Oui|Oui|non|non|non|non|
|Groupe de disponibilité à validation de réplica minimale|Oui|Oui|Oui|non|non|non|
  

<sup>1</sup> Pour plus d’informations sur l’installation de SQL Server sur Server Core, consultez [Installer SQL Server sur Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md). 

<sup>2</sup> Dans l’édition Entreprise, le nombre de nœuds correspond au maximum du système d’exploitation. L’édition Standard prend en charge deux nœuds. 

<sup>3</sup> L’édition Entreprise fournit la prise en charge de 8 réplicas secondaires maximum, y compris 2 réplicas secondaires synchrones. 

<sup>4</sup> L’édition Standard prend en charge les groupes de disponibilité de base. Un groupe de disponibilité de base prend en charge deux réplicas, avec une base de données. Pour plus d’informations sur les groupes de disponibilité de base, consultez [Groupes de disponibilité de base](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).  


##  <a name="RDBMSSP"></a> Scalabilité et performances SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Columnstore <sup>1</sup>|Oui|Oui|Oui|Oui|Oui|  
|Fichiers binaires LOB dans les index columnstore cluster|Oui|Oui|Oui|Oui|Oui|  
|Reconstruction d’index columnstore non cluster en ligne|Oui|non|non|non|non|
|OLTP en mémoire <sup>1</sup>|Oui|Oui|Oui|Oui, <sup>2</sup>|Oui|
|Stretch Database|Oui|Oui|Oui|Oui|Oui|
|Mémoire principale persistante|Oui|Oui|Oui|Oui|Oui|
|Prise en charge de plusieurs instances|50|50|50|50|50|
|Partitionnement des tables et des index|Oui|Oui|Oui|Oui|Oui|  
|Compression des données|Oui|Oui|Oui|Oui|Oui|
|gouverneur de ressources|Oui|non|non|non|non|  
|Parallélisme de tables partitionnées|Oui|non|non|non|non|
|Plusieurs conteneurs Filestream|Oui|Oui|Oui|Oui|Oui|
|Mémoire de pages de grande taille compatible NUMA et allocation de tableau de tampons|Oui|non|non|non|non|
|Buffer Pool Extension|Oui|Oui|non|non|non|
|Gouvernance des ressources d'E/S|Oui|non|non|non|non|  
|Durabilité différée|Oui|Oui|Oui|Oui|Oui|
|Paramétrage automatique|Oui|non|non|non|non|
|Jointures adaptatives en mode batch|Oui|non|non|non|non|
|Retour d’allocation de mémoire en mode batch|Oui|non|non|non|non|
|Exécution entrelacée pour les fonctions table à instructions multiples|Oui|Oui|Oui|Oui|Oui|
|Améliorations de l’insertion en bloc|Oui|Oui|Oui|Oui|Oui|


<sup>1</sup> La taille des données OLTP en mémoire et le cache de segments columnstore sont limités à la quantité de mémoire spécifiée par l’édition dans la section Limites d’échelle. Le Nombre maximal de degrés de parallélisme est limité. Le nombre de degrés de parallélisme maximal pour la création d’un index est limité à 2 pour l’Édition Standard et à 1 pour les éditions Web et Express. Ceci fait référence aux index columnstore créés sur des tables basées sur des disques et des tables à mémoire optimisée.

<sup>2</sup> Cette fonctionnalité n’est pas incluse dans l’option d’installation LocalDB.

##  <a name="RDBMSS"></a> Sécurité SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express|Express with Advanced Services|  
|-------------|----------------|--------------|---------|-------------|------------------------------------| 
|Sécurité au niveau des lignes|Oui|Oui|Oui|Oui|Oui|  
|Always Encrypted|Oui|Oui|Oui|Oui|Oui| 
|Masquage dynamique des données|Oui|Oui|Oui|Oui|Oui|   
|Audit de base|Oui|Oui|Oui|Oui|Oui| 
|Audit de granularité fine|Oui|Oui|Oui|Oui|Oui| 
|Chiffrement transparent de base de données|Oui|non|non|non|non|   
|Gestion de clés extensible|Oui|non|non|non|non| 
|Rôles définis par l’utilisateur|Oui|Oui|Oui|Oui|Oui| 
|Bases de données à relation contenant-contenu|Oui|Oui|Oui|Oui|Oui| 
|Chiffrement des sauvegardes|Oui|Oui|non|non|non|  

##  <a name="Replication"></a> Replication  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Abonnés hétérogènes|Oui|Oui|non|non|non|  
|Réplication de fusion|Oui|Oui|Oui (abonné uniquement)|Oui (abonné uniquement)|Oui (abonné uniquement)|   
|Publication Oracle|Oui|non|non|non|non| 
|Réplication transactionnelle d’égal à égal|Oui|non|non|non|non|   
|Réplication d'instantané|Oui|Oui|Oui (abonné uniquement)|Oui (abonné uniquement)|Oui (abonné uniquement)|   
|Suivi des modifications SQL Server|Oui|Oui|Oui|Oui|Oui| 
|Réplication transactionnelle|Oui|Oui|Oui (abonné uniquement)|Oui (abonné uniquement)|Oui (abonné uniquement)|   
|Réplication transactionnelle vers Azure|Oui|Oui|non|non|non|   
|Abonnement pouvant être mis à jour à la réplication transactionnelle|Oui|non|non|non|non|  
  
##  <a name="SSMS"></a> Outils d’administration  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Objets de gestion SQL (SMO)|Oui|Oui|Oui|Oui|Oui|  
|Gestionnaire de configuration SQL|Oui|Oui|Oui|Oui|Oui|   
|SQL CMD (outil d'invite de commandes)|Oui|Oui|Oui|Oui|Oui|      
|Distributed Replay - Outil d’administration|Oui|Oui|Oui|Oui|non|  
|Distribute Replay - Client|Oui|Oui|Oui|non|non|  
|Distributed Replay - Contrôleur|Oui (jusqu’à 16 clients)|Oui (1 client)|Oui (1 client)|non|non|   
|SQL Profiler|Oui|Oui|Non <sup>1</sup>|Non <sup>1</sup>|Non <sup>1</sup>|  
|SQL Server Agent|Oui|Oui|Oui|non|non| 
|Pack d'administration Microsoft System Center Operations Manager|Oui|Oui|Oui|non|non|  
|Assistant Paramétrage de base de données (DTA)|Oui|Oui <sup>2</sup>|Oui <sup>2</sup>|non|non|      
  
 
  <sup>1</sup> SQL Server Web, SQL Server Express, SQL Server Express with Tools et SQL Server Express with Advanced Services peuvent être profilés à l’aide de SQL Server Standard et SQL Server Entreprise.  
  
 <sup>2</sup> Paramétrage activé uniquement sur les fonctionnalités de l’édition Standard  
  
##  <a name="RDBMSM"></a> Simplicité de gestion SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Instances utilisateur|non|non|non|Oui|Oui| 
|LocalDB|non|non|non|Oui|non| 
|Connexion administrateur dédiée|Oui|Oui|Oui|Oui avec indicateur de trace|Oui avec indicateur de trace|   
|Prise en charge de SysPrep <sup>1</sup>|Oui|Oui|Oui|Oui|Oui| 
|Prise en charge de scripts PowerShell<sup>2</sup>|Oui|Oui|Oui|Oui|Oui| 
|Prise en charge des opérations des composants d’application du niveau Données : extraction, déploiement, mise à niveau, suppression|Oui|Oui|Oui|Oui|Oui| 
|Automation de stratégie (vérification selon la planification et sur modification)|Oui|Oui|Oui|non|non|   
|Collecteur de données de performances|Oui|Oui|Oui|non|non| 
|Possibilité d’inscription en tant qu’instance managée dans le cadre de la gestion de plusieurs instances|Oui|Oui|Oui|non|non|   
|Rapports de performances standard|Oui|Oui|Oui|non|non| 
|Repères de plan et gel de plan relatif|Oui|Oui|Oui|non|non|   
|Requête directe de vues d'index (à l'aide de l'indicateur NOEXPAND)|Oui|Oui|Oui|Oui|Oui| 
|Maintenance automatique des vues indexées|Oui|Oui|Oui|non|non| 
|Vues partitionnées distribuées|Oui|non|non|non|non| 
|Opérations d'index parallèles|Oui|non|non|non|non|  
|Utilisation automatique de vues indexées par l'optimiseur de requête|Oui|non|non|non|non| 
|Vérifications de cohérence parallèles|Oui|non|non|non|non| 
|Point de contrôle de l’utilitaire SQL Server|Oui|non|non|non|non|    
|Extension du pool de mémoires tampons|Oui|Oui|non|non|non| 
  
 <sup>1</sup> Pour plus d’informations, consultez [Considérations relatives à l’installation de SQL Server à l’aide de SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
 
 <sup>2</sup> Sur Linux, les scripts PowerShell sont pris en charge à partir d’ordinateurs Windows ciblant des serveurs SQL Server sur Linux. 
##  <a name="DevTools"></a> Outils de développement  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Intégration de Microsoft Visual Studio|Oui|Oui|Oui|Oui|Oui| 
|Intellisense (Transact-SQL et MDX)|Oui|Oui|Oui|Oui|Oui| 
|Outils de données SQL Server (SSDT)|Oui|Oui|Oui|Oui|non|    
|Outils de conception, de modification et de débogage MDX|Oui|Oui|non|non|non|   
  
##  <a name="Programmability"></a> Programmability  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Intégration R de base|Oui|Oui|Oui|Oui|non|   
|Intégration R avancée|Oui|non|non|non|non| 
|Intégration de Python de base|Oui|Oui|Oui|Oui|non|
|Intégration de Python avancée|Oui|non|non|non|non| 
|Machine Learning Server (autonome)|Oui|non|non|non|non|   
|Nœud de calcul Polybase|Oui|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup> | 
|Nœud principal Polybase|Oui|non|non|non|non| 
|JSON|Oui|Oui|Oui|Oui|Oui|   
|Magasin de requêtes|Oui|Oui|Oui|Oui|Oui|   
|Temporal|Oui|Oui|Oui|Oui|Oui|   
|Intégration du CLR (Common Language Runtime)|Oui|Oui|Oui|Oui|Oui|   
|Prise en charge XML native|Oui|Oui|Oui|Oui|Oui| 
|Indexation XML|Oui|Oui|Oui|Oui|Oui| 
|Fonctions MERGE & UPSERT|Oui|Oui|Oui|Oui|Oui|   
|prise en charge de FILESTREAM|Oui|Oui|Oui|Oui|Oui| 
|FileTable|Oui|Oui|Oui|Oui|Oui| 
|Types de données de date et d'heure|Oui|Oui|Oui|Oui|Oui|  
|Support d'internationalisation|Oui|Oui|Oui|Oui|Oui| 
|Recherche sémantique et en texte intégral|Oui|Oui|Oui|Oui|non| 
|Spécification d'une langue dans une requête|Oui|Oui|Oui|Oui|non|   
|Service Broker (messagerie)|Oui|Oui|Non (client uniquement)|Non (client uniquement)|Non (client uniquement)|   
|Transact-SQL, points de terminaison|Oui|Oui|Oui|non|non| 
|Graphique|Oui|Oui|Oui|Oui|Oui|  


<sup>1</sup> La montée en puissance parallèle avec plusieurs nœuds de calcul nécessite un nœud principal.

## <a name="IS"></a> Integration Services

Pour plus d’informations sur les fonctionnalités SQL Server Integration Services (SSIS) prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Integration Services prises en charge par les éditions de SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="MDS"></a> Master Data Services  
 Pour plus d’informations sur les fonctionnalités [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] et Data Quality Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Master Data Services et Data Quality Services prises en charge par les éditions de SQL Server](../master-data-services/master-data-services-and-data-quality-services-features-support.md). 

  
##  <a name="DW"></a> Entrepôt de données  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Création de cubes sans une base de données|Oui|Oui|non|non|non |   
|Génération automatique de la mise en lots et du schéma d'entrepôt de données|Oui|Oui|non|non|non| 
|Capture des données modifiées|Oui|Oui|non|non|non| 
|Optimisations de requêtes de jointure en étoile|Oui|non|non|non|non| 
|Configuration en lecture seule évolutive d'Analysis Services|Oui|non|non|non|non| 
|Traitement des requêtes parallèles sur les tables et les index partitionnés|Oui|non|non|non|non|   
|Agrégation globale des traitements|Oui|non|non|non|non| 

##  <a name="SSAS"></a> Analysis Services  
  
Pour plus d’informations sur les fonctionnalités Analysis Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md). 
  
##  <a name="BIMD"></a> Modèle sémantique BI (multidimensionnel)  
  
Pour plus d’informations sur les fonctionnalités Analysis Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).
   
##  <a name="BIT"></a> Modèle sémantique BI (tabulaire)  
  
Pour plus d’informations sur les fonctionnalités Analysis Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="PPSP"></a> Power Pivot for SharePoint  
  
Pour plus d’informations sur les fonctionnalités Power Pivot pour SharePoint prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="DM"></a> Exploration de données  
  
Pour plus d’informations sur les fonctionnalités d’exploration de données prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="SSRS"></a> Reporting Services  
  
Pour plus d’informations sur les fonctionnalités Reporting Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

##  <a name="BIC"></a> Clients Business Intelligence  

Pour plus d’informations sur les fonctionnalités de client Business Intelligence prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssNoVersion_md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md) ou [Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="SLS"></a> Services d’emplacement et spatiaux  
  
|Nom de la fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Index spatiaux|Oui|Oui|Oui|Oui|Oui|   
|Types de données planaires et géodésiques|Oui|Oui|Oui|Oui|Oui| 
|Bibliothèques spatiales avancées|Oui|Oui|Oui|Oui|Oui|   
|Importation/exportation de formats de données spatiales standard|Oui|Oui|Oui|Oui|Oui|   
  
##  <a name="ADS"></a> Services de base de données supplémentaires  
  
|Nom de la fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Assistant Migration|Oui|Oui|Oui|Oui|Oui|   
|Messagerie de base de données|Oui|Oui|Oui|non|non| 
  
##  <a name="Other"></a> Autres composants  
  
|Nom de la fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------|  
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|non|non| 
|StreamInsight HA|StreamInsight Premium Edition|non|non|non|non|   

> [![Télécharger SSMS](../analysis-services/media/download.png)](../ssms/download-sql-server-management-studio-ssms.md) **[Télécharger la dernière version de SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)**     
  
## <a name="next-steps"></a>Étapes suivantes 
 [Spécifications de produit pour SQL Server 2016](http://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)   
 [Installation de SQL Server 2016](../database-engine/install-windows/installation-for-sql-server-2016.md)  
 
 [!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
