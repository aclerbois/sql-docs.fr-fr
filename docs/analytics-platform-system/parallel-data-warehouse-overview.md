---
title: Composants de l’entrepôt de données - système de plateforme d’Analytique en parallèle | Documents Microsoft
description: Cet article explique le logiciel et les composants non logicielle du système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 09813eecdcc933619955de8d94e83079cad0c68f
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="parallel-data-warehouse-components---analytics-platform-system"></a>Composants de l’entrepôt de données - système de plateforme d’Analytique en parallèle
Cet article explique le logiciel et les composants non logicielle du système de plateforme Analytique.  
  
<!-- MISSING LINKS

To learn more about Analytics Platform System, see:  
  
-   [Analytics Platform System architecture](architecture-overview.md)  
  
-   [Distributed and Replicated Tables &#40;SQL Server PDW&#41;](../sqlpdw/distributed-and-replicated-tables-sql-server-pdw.md)  
  
-   [PolyBase &#40;SQL Server PDW&#41;](../sqlpdw/polybase-sql-server-pdw.md)  
  
-   [Clustered Columnstore Indexes &#40;SQL Server PDW&#41;](../sqlpdw/clustered-columnstore-indexes-sql-server-pdw.md)  
  
-   [Query Process &#40;SQL Server PDW&#41;](../sqlpdw/query-process-sql-server-pdw.md)  
  
-   [Minimum and Maximum Values &#40;SQL Server PDW&#41;](../sqlpdw/minimum-and-maximum-values-sql-server-pdw.md)  

-->
   
  
![Logiciel de l’entrepôt de données en parallèle](media/parallel-data-warehouse-software.png "logiciel de Parallel Data Warehouse")  
  
## <a name="sec1"></a>Logiciel d’application – stockage de données utilisateur et le traitement de requête  
  
### <a name="control-node"></a>Nœud de contrôle  
Moteur MPP  
Le moteur de MPP est le cerveau du système massivement parallèles de traitement (MPP). Elle effectue les opérations suivantes :  
  
-   Crée des plans de requête parallèles et coordonne l’exécution des requêtes parallèles sur les nœuds de calcul.  
  
-   Stocke et coordonne les données de configuration et de métadonnées pour toutes les bases de données.  
  
-   Gère l’autorisation et authentification de base de données SQL Server PDW.  
  
-   Effectue le suivi d’état du matériel et logiciel.  
  
### <a name="data-movement-service-dms"></a>Service de déplacement de données DMS)  
Service de déplacement des données (DMS) fait partie de la « arme secrète » de PDW. Elle effectue les opérations suivantes :  
  
-   Transfère des données vers et depuis les nœuds SQL Server PDW.  
  
-   Processus des opérations nécessitant le transfert de données entre les nœuds de requête.  
  
-   Améliore les performances des requêtes en optimisant les vitesses de transfert de données.  
  
### <a name="admin-console"></a>Console Administration  
La Console d’administration est une application web qui présente l’état du matériel, d’intégrité et des informations de performances.  
  
### <a name="configuration-manager"></a>Gestionnaire de configuration  
Le Gestionnaire de Configuration (dwconfig.exe), est l’outil que les administrateurs de matériel permettent de configurer le système de plateforme d’Analytique.  
  
### <a name="control-node-databases"></a>Contrôle nœud bases de données  
SQL Server gère toutes les bases de données sur le nœud du contrôle.  
  
-   La base de données Shell qui gère les métadonnées pour toutes les bases de données utilisateur distribuée.  
  
-   TempDB contient les métadonnées de toutes les tables temporaires utilisateur sur l’appareil.  
  
-   Principale est la table principale pour SQL Server sur le nœud de contrôle.  
  
### <a name="compute-node"></a>Nœud de calcul  
Les nœuds de calcul sont le traitement parallèle des données et des unités de stockage. Ils ont le stockage en attachement direct et utilisent SQL Server pour gérer les données utilisateur.  
  
### <a name="data-movement-service-dms"></a>Service de déplacement de données DMS)  
Service de déplacement des données (DMS) s’exécute sur chaque nœud de calcul à effectuer les opérations suivantes :  
  
-   Dans le cadre du traitement de requêtes parallèles, DMS transférer des données vers et depuis d’autres nœuds de l’ordinateur et le nœud de contrôle.  
  
-   DMS, en cours d’exécution sur chaque nœud de calcul, reçoit les chargements de données en parallèle. Données sont chargées en parallèle directement depuis le serveur de chargement pour les nœuds de calcul  
  
-   DMS transfère des données à partir de chaque nœud de calcul directement au serveur de sauvegarde.  
  
-   À l’aide de PolyBase DMS transfère des données vers et depuis un cluster Hadoop externe ou la HDInsight région sur l’appareil.  
  
### <a name="compute-node-databases"></a>Bases de données de nœud de calcul 
Chaque nœud de calcul s’exécute une instance de SQL Server pour traiter les requêtes et de gérer les données utilisateur.  
  
## <a name="appliance-fabric"></a>Infrastructure d’application  
L’infrastructure d’application fournit l’infrastructure réseau, de services et de système d’exploitation de l’appareil.  
  
### <a name="domain-controller"></a>Contrôleur de domaine  
Services de domaine Active Directory (AD) (DS)  
Système de plateforme d’Analytique effectue l’authentification entre les nœuds du système de plateforme Analytique et gère l’authentification des connexions d’authentification Windows de SQL Server PDW.  
  
Service DNS  
Windows Service DNS (Domain Name) résout les noms de domaine pour les adresses IP de l’application système de plateforme Analytique.  
  
### <a name="windows-deployment-service"></a>Services de déploiement Windows  
Service de déploiement Windows (WDS) déploie le système d’exploitation Windows à l’appliance. Il est déployé sur chaque ordinateur hôte et l’ordinateur virtuel sur l’appareil.  
  
Le service DHCP crée des adresses IP afin que les ordinateurs hôtes au sein du domaine d’application peuvent joindre le matériel de réseau sans avoir une adresse IP préconfigurée.  
  
### <a name="virtual-machine-manager"></a>Virtual Machine Manager  
Système de plateforme Analytique utilise la virtualisation pour atteindre une haute disponibilité. Virtual Machine Manager héberge System Center pour déployer le système d’exploitation sur les ordinateurs hôtes physiques.  
  
Windows Server Update Services (WSUS) pour appliquer ou supprimer des mises à jour Windows sur tous les hôtes et les ordinateurs virtuels.  
  
### <a name="windows-server"></a>Windows Server  
Tous les hôtes et ordinateurs virtuels dans l’appliance exécutent le système d’exploitation Windows Server.  
  
### <a name="failover-clustering"></a>Clustering de basculement  
Clustering de basculement Windows permet de redémarrer les processus sur un ordinateur hôte passif dans le cas où un hôte échoue.  
  
### <a name="storage-spaces"></a>Espaces de stockage  
Espaces de stockage Windows gère les données de l’utilisateur en tant qu’un pool de stockage pour un petit groupe de nœuds de calcul. Si un nœud de calcul échoue, les données sont toujours accessibles via un autre nœud de calcul dans le groupe.  
  
### <a name="hyper-v"></a>Hyper-V  
Microsoft Hyper-V Server fournit une solution de virtualisation fiable et simple. Système de plateforme Analytique utilise virtualisation pour équilibrer les ressources du processeur et offre une haute disponibilité pour les nœuds PDW et d’un dispositif de composants d’infrastructure.  
  
## <a name="sec2"></a>Données non relationnelles
Technologie PolyBase intègre des données de SQL Server PDW avec des données Hadoop externes. Les données Hadoop peuvent être stockées sur une de ces sources de données Hadoop :  
  
-   Hortonworks pour Linux ou Windows Server  
  
-   Cloudera sur Linux  
  
-   HDInsight en cours d’exécution sur les points d’accès  
  
-   HDInsight les données stockées sur l’objet Blob de stockage Azure  
  
## <a name="query-tools"></a>Outils de requête   
  
Les requêtes sont écrites avec Transact\-SQL modifié pour s’ajuster à la nature MPP des requêtes. Toutes les requêtes sont envoyées au nœud de contrôle qui génère un plan de requête parallèle pour exécuter la requête sur les nœuds de calcul.  
  
### <a name="sql-server-data-tools-ssdt"></a>Outils de données SQL Server (SSDT)  
Outils de données SQL Server s’exécute à l’intérieur de Visual Studio et est notre outil GUI recommandé pour l’envoi de requêtes SQL Server PDW. Il est similaire à SQL Server Management Studio en vous permettant de naviguer dans un Explorateur d’objets.  
  
Si vous ne disposez pas de Visual Studio, vous pouvez télécharger les outils dont vous avez besoin gratuitement. 
<!-- MISSING LINKS
For more information, see [Install SQL Server database tooling  for Visual Studio &#40;SQL Server PDW&#41;](../sqlpdw/install-sql-server-database-tooling-for-visual-studio-sql-server-pdw.md).  
-->
  
### <a name="sqlcmd-command-line-query-tool"></a>Outil de requête de ligne de commande de SQLCMD  
SQLCMD est l’outil de ligne de commande de SQL Server pour l’exécution Transact\-SQL instructions et commandes du système. Il fonctionne avec SQL Server PDW et notre outil de ligne de commande recommandée pour l’interrogation de SQL Server PDW. Avec sqlcmd, vous pouvez exécuter Transact\-instructions SQL interactivement à partir de la ligne de commande, comme un fichier de commandes ou à partir de Windows PowerShell.  
  
<!-- MISSING LINKS

If you don’t have SQL Server, you can download this as a standalone package. For more information, see [Install sqlcmd Command-Line Client &#40;SQL Server PDW&#41;](../sqlpdw/install-sqlcmd-command-line-client-sql-server-pdw.md) 
--> 
  
### <a name="integration-services"></a>Integration Services  
Vous pouvez utiliser les Services d’intégration à la requête SQL Server PDW. 

<!-- MISSING LINKS
For more information, see [Connect With SQL Server Integration Services for Querying &#40;SQL Server PDW&#41;](../sqlpdw/connect-with-sql-server-integration-services-for-querying-sql-server-pdw.md). 

--> 
  
### <a name="linked-server"></a>Serveur lié  
En utilisant une connexion de serveur lié SQL Server, vous pouvez utiliser SQL Server pour envoyer Transact\-instructions SQL pour SQL Server PDW. 
<!-- MISSING LINKS
For more information, see [Connect With SQL Server Linked Server &#40;SQL Server PDW&#41;](../sqlpdw/connect-with-sql-server-linked-server-sql-server-pdw.md). 
--> 
  
## <a name="business-intelligence-tools"></a>Outils d’analyse décisionnelle
  
Analysis Services  
SQL Server PDW est une source de données valide pour les bases de données Analysis Services et les modèles de PowerPivot pour Excel. À l’aide du fournisseur OLE DB, vous pouvez configurer un cube Analysis Services pour utiliser le traitement analytique en ligne multidimensionnel (MOLAP) ou stockage de traitement analytique en ligne relationnel (ROLAP).  
  
<!-- MISSING LINKS
For more information, see [Connect With SQL Server Analysis Services &#40;SQL Server PDW&#41;](../sqlpdw/connect-with-sql-server-analysis-services-sql-server-pdw.md).  

-->
  
### <a name="report-builder"></a>Générateur de rapports  
Vous pouvez utiliser SQL Server PDW comme une source de données SQL Server pour les rapports que vous développez pour Reporting Services à l’aide du Générateur de rapports SQL Server. Vous pouvez également utiliser SQL Server PDW comme source de SQL Server pour les modèles de rapport. À l’aide du Gestionnaire de rapports ou de l’API du serveur de rapports, vous pouvez générer un modèle à partir d’une base de données SQL Server PDW.  
  
<!-- MISSING LINKS

For more information, see [Connect With SQL Server Report Builder &#40;SQL Server PDW&#41;](../sqlpdw/connect-with-sql-server-report-builder-sql-server-pdw.md) and [Connect With SQL Server Reporting Services &#40;SQL Server PDW&#41;](../sqlpdw/connect-with-sql-server-reporting-services-sql-server-pdw.md). 

--> 
  
### <a name="power-pivot-for-excel"></a>Power Pivot pour Excel  
Vous pouvez vous connecter à SQL Server PDW avec PowerPivot pour Excel, un téléchargement gratuit qui développe considérablement les capacités d’analyse de données d’Excel.  
  
<!-- MISSING LINKS

For more information, see [Connect With PowerPivot for Excel &#40;SQL Server PDW&#41;](../sqlpdw/connect-with-powerpivot-for-excel-sql-server-pdw.md).  

-->
  
## <a name="loading-tools"></a>Outils de chargement 
  
### <a name="integration-services"></a>Integration Services  
Installez les adaptateurs de destination PDW spécifiques qui vous permettent d’utiliser SQL ServerIntegration Services pour charger des données dans SQL Server PDW.  

<!-- MISSING LINKS
For more information, see [Install Integration Services Destination Adapters &#40;SQL Server PDW&#41;](../sqlpdw/install-integration-services-destination-adapters-sql-server-pdw.md). 
--> 
  
### <a name="dwloader-command-line-loader"></a>Chargeur de ligne de commande de dwloader  
dwloader est un outil de ligne de commande de chargement qui charge des données en parallèle à partir de votre serveur de chargement pour les nœuds de calcul de SQL Server PDW. 

<!-- MISSING LINKS
For more information, see [Install dwloader Command-Line Loader &#40;SQL Server PDW&#41;](../sqlpdw/install-dwloader-command-line-loader-sql-server-pdw.md)  
-->
  
### <a name="polybase-for-hadoop-integration"></a>PolyBase pour l’intégration de Hadoop  
Avec la technologie de PolyBase, vous pouvez charger des données non relationnelles à partir d’un Hadoop Cluster dans une table relationnelle dans SQL Server PDW. Les données Hadoop peuvent se trouver dans un externe du Hadoop Cluster, la région HDI APS, ou dans un objet Blob de stockage Azure.  

<!-- MISSING LINKS
For more information, see [PolyBase &#40;SQL Server PDW&#41;](../sqlpdw/polybase-sql-server-pdw.md).
-->  
  
## <a name="database-backup-and-restore"></a>Sauvegarde et restauration  
SQL Server PDW utilise Transact\-SQL de base de données de sauvegarde et restaurer les commandes pour sauvegarder et restaurer des bases de données utilisateur, en parallèle, vers et depuis un serveur de sauvegarde. SQL Server PDW écrit la sauvegarde dans un répertoire dans un partage de fichiers Windows et même restaure ensuite les données à partir d’un partage de fichiers Windows.  
  
Pour plus d’informations, consultez [planifier pour la sauvegarde et de chargement de matériel](backup-and-loading-hardware.md) et [sauvegarde et restaurer une vue d’ensemble](backup-and-restore-overview.md)  
  
## <a name="remote-table-copy"></a>Copie de la Table distante  
La fonctionnalité de copie de la Table distante vous permet de copier des tables de bases de données SQL Server PDW pour des bases de données (autre que l’appliance) SMP SQL Server distantes. Cela permet des scénarios de hub et spoke pour SQL Server PDW.  
  
<!-- MISSING LINKS

For more information, see [Remote Table Copy &#40;SQL Server PDW&#41;](../sqlpdw/remote-table-copy-sql-server-pdw.md).  

-->
  
## <a name="monitoring"></a>Surveillance  
Système de plateforme Analytique dispose de plusieurs méthodes pour surveiller l’activité de l’appliance  
  
### <a name="admin-console"></a>Console Administration  
La Console d’administration vous permet de vous permet d’afficher l’état actuel sur l’intégrité de l’application. Il s’exécute comme une application web sur le nœud de contrôle et est accessible via le protocole https.  
  
Pour plus d’informations, consultez [contrôler le matériel à l’aide de la Console d’administration &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-the-admin-console.md)  

### <a name="system-views"></a>Vues système  
La Console d’administration est basée sur les requêtes de vue système. Vous pouvez interroger les vues système individuellement afin d’obtenir la partie spécifique des informations dont vous avez besoin.  

Pour plus d’informations, consultez [contrôler le matériel en utilisant les vues système &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-system-views.md) 
  
### <a name="system-center-operations-manager"></a>System Center Operations Manager  
Il existe des packs d’administration de System Center Operations Manager (SCOM) pour SQL Server PDW. 

Pour configurer le matériel pour SCOM, consultez [contrôler le matériel à l’aide de System Center Operations Manager &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-system-center-operations-manager.md)  
  
