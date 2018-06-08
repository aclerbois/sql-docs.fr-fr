---
title: Éditions et des fonctionnalités prises en charge de SQL Server 2017 ~ Linux | Documents Microsoft
ms.custom: sql-linux
ms.date: 09/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: linux
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- default components
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
ms.assetid: ''
caps.latest.revision: 121
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 8222c0a58c1dbaeeaa5bd3dffedcca254728aae6
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="editions-and-supported-features-of-sql-server-2017-on-linux"></a>Éditions et fonctionnalités prises en charge pour SQL Server 2017 sous Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article fournit des détails sur les fonctionnalités prises en charge par les différentes éditions de 2017 du serveur SQL sur Linux. Pour les éditions et les fonctionnalités prises en charge de SQL Server sur Windows, consultez [SQL Server 2017 - Windows](../sql-server/editions-and-components-of-sql-server-2017.md).  
  
La configuration requise pour l'installation varie selon vos besoins applicatifs. Les différentes éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'adaptent aux exigences de chaque organisation et de chaque individu en termes de performances, d'exécution et de prix. Les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que vous installez dépendent également de vos exigences spécifiques. Les sections suivantes vous aident à choisir parmi les éditions et les composants disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

Pour obtenir les notes de publication les plus récentes et des informations sur les nouveautés, consultez les rubriques suivantes :
- [Notes de publication de SQL Server sur Linux](sql-server-linux-release-notes.md)
- [Nouveautés de SQL Server sur Linux](sql-server-linux-whats-new.md)

Pour obtenir la liste des fonctionnalités de SQL Server non disponibles sur Linux, consultez [non pris en charge des fonctionnalités et services](sql-server-linux-release-notes.md#Unsupported).

### <a name="try-sql-server"></a>Essayez SQL Server !    
    
[Télécharger SQL Server 2017](http://www.microsoft.com/sql-server/sql-server-2017)

## <a name="includessnoversionincludesssnoversion-mdmd-editions"></a>Éditions de [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]  
 Le tableau ci-dessous décrit les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 
  
|Édition de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] |Définition|  
|---------------------------------------|----------------|  
|Enterprise|Offre premium, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise edition offre des fonctionnalités de centre de données haut de gamme complète avec des performances ultra-rapides-fast l’activation de haut niveau de service pour les charges de travail critiques.|  
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard edition propose une gestion de base de données pour les PME d’exécuter leurs applications et prend en charge des outils de développement courants pour locaux et cloud, pour une gestion efficace de la base de données avec un minimum de ressources informatiques.|  
|Web|L'édition Web[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] est une option offrant un coût total de possession faible destinée aux hébergeurs Web et aux VAP Web, fournissant des fonctions évolutives, rentables et gérables aux propriétés Web à petite ou grande échelle.|  
|Développeur|L'édition[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer permet aux développeurs de créer des applications basées sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il inclut toutes les fonctionnalités de l'édition Enterprise, mais sa licence permet uniquement de l'utiliser comme un système de développement et de test, et non comme un serveur de production. L'édition [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer est la solution idéale pour le développement et le test d'applications.|  
|Express Edition|L’édition Express est une édition de base comprenant une base de données gratuite, idéale pour découvrir et créer des applications bureautiques et de petites applications serveur pilotées par les données. C'est la solution idéale pour les éditeurs de logiciels, les développeurs et les amateurs de création d'applications clientes. Si vous avez besoin de fonctionnalités de base de données plus évoluées, vous pouvez mettre à niveau de manière transparente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express vers des versions plus sophistiquées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|  
  
## <a name="using-includessnoversionincludesssnoversion-mdmd-with-clientserver-applications"></a>Utilisation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec des applications client/serveur  

Vous pouvez installer uniquement les composants clients de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant des applications client/serveur qui se connectent directement à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Une installation de composants clients est également un bon choix si vous administrez une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un serveur de base de données ou si vous prévoyez de développer des applications [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="includessnoversionincludesssnoversion-mdmd-components"></a>Composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  

SQL Server 2017 sous Linux prend en charge le moteur de base de données SQL Server. Le tableau suivant décrit les fonctionnalités dans le moteur de base de données.   
  
|Composants serveur| Description|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] inclut le [!INCLUDE[ssDE](../includes/ssde-md.md)], le service principal de stockage, traitement et de sécurisation des données, réplication, recherche en texte intégral, outils de gestion relationnelles et XML et l’intégration de base de données analytique.|  

**Éditions Developer, Enterprise Core et d’évaluation**  
Pour les fonctionnalités prises en charge par l'édition Enterprise, Developer et les éditions d’évaluation, consultez les fonctionnalités répertoriées pour l’édition Enterprise de SQL Server  dans les tableaux ci-dessous.

L’édition Developer continue à prendre en charge seulement 1 client pour [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="Cross-BoxScaleLimits"></a> Limites d’échelle  
  
|Fonctionnalité|Enterprise|Standard|Web|Express| 
|-------------|----------------|--------------|---------|------------------------|
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs| 
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ou [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|
|Mémoire maximale du pool de mémoires tampons par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum du système d’exploitation|128 Go|64 Go|1410 Mo|
|Mémoire maximale du cache de segments columnstore par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go| 16 Go| 352 Mo|  
|Taille maximale des données à mémoire optimisée par base de données dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go| 16 Go| 352 Mo|
|Taille maximale de la base de données relationnelle|524 Po|524 Po|524 Po|10 GB|  
  
<sup>1</sup> Enterprise edition avec serveur + Client accès licences (CAL) (non disponible pour les nouveaux contrats) est limitée à un maximum de 20 cœurs par instance de SQL Server. Il n'existe aucune limite dans le mode de licence Serveur selon le nombre de cœurs. Pour plus d’informations, consultez [limites de capacité de calcul par l’édition de SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
 
##  <a name="RDBMSHA"></a> Haute disponibilité SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------|  
|Envoi des journaux de transaction|Oui|Oui|Oui|non|  
|Compression de sauvegarde|Oui|Oui|Non|non| 
|Instantané de base de données|Oui|Non|Non|non|
|Instance de cluster de basculement Always On<sup>1</sup>|Oui|Oui|Non|non| 
|Groupes de disponibilité Always On<sup>2</sup>|Oui|Non|Non|non|
|Groupes de disponibilité de base <sup>3</sup>|non|Oui|Non|non|
|Groupe de disponibilité à validation de réplica minimale|Oui|Oui|Non|non|
|Groupe de disponibilité sans cluster|Oui|Oui|Non|non|
|Restauration en ligne de pages et de fichiers|Oui|Non|Non|non|
|Indexation en ligne|Oui|Non|Non|non|
|Reconstructions d’index en ligne pouvant être reprises|Oui|Non|Non|non|
|Modification de schéma en ligne|Oui|Non|Non|non|
|Récupération rapide|Oui|Non|Non|non|
|Sauvegardes en miroir|Oui|Non|Non|non|
|Ajout de mémoire et de processeur à chaud|Oui|Non|Non|non|
|Sauvegarde chiffrée|Oui|Oui|Non|non|
|Sauvegarde hybride vers Windows Azure (sauvegarde vers une URL)|Oui|Oui|Non|non|
  
<sup>1</sup> sur Enterprise edition, le nombre de nœuds est au maximum du système d’exploitation. L’édition Standard prend en charge deux nœuds. 

<sup>2</sup> sur Enterprise edition prend en charge jusqu'à 8 réplicas secondaires - notamment 2 réplicas secondaires synchrones. 

<sup>3</sup> standard edition prend en charge les groupes de disponibilité de base. Un groupe de disponibilité de base prend en charge deux réplicas, avec une base de données. Pour plus d’informations sur les groupes de disponibilité de base, consultez [Groupes de disponibilité de base](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).    

##  <a name="RDBMSSP"></a> Scalabilité et performances SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------| 
|Columnstore <sup>1</sup>|Oui|Oui|Oui|Oui|  
|Fichiers binaires LOB dans les index columnstore cluster|Oui|Oui|Oui|Oui|  
|Reconstruction d’index columnstore non cluster en ligne|Oui|Non|Non|non|
|OLTP en mémoire <sup>1</sup>|Oui|Oui|Oui|Oui|
|Mémoire principale persistante|Oui|Oui|Oui|Oui|
|Partitionnement des tables et des index|Oui|Oui|Oui|Oui|  
|Compression des données|Oui|Oui|Oui|Oui|
|gouverneur de ressources|Oui|Non|Non|non|  
|Parallélisme de tables partitionnées|Oui|Non|Non|non|
|Mémoire de pages de grande taille compatible NUMA et allocation de tableau de tampons|Oui|Non|Non|non|
|Gouvernance des ressources d'E/S|Oui|Non|Non|non|  
|Durabilité différée|Oui|Oui|Oui|Oui|
|Paramétrage automatique|Oui|Non|Non|non|
|Jointures adaptatives en mode batch|Oui|Non|Non|non|
|Retour d’allocation de mémoire en mode batch|Oui|Non|Non|non|
|Exécution entrelacée pour les fonctions table à instructions multiples|Oui|Oui|Oui|Oui|
|Améliorations de l’insertion en bloc|Oui|Oui|Oui|Oui|


<sup>1</sup> La taille des données OLTP en mémoire et le cache de segments columnstore sont limités à la quantité de mémoire spécifiée par l’édition dans la section Limites d’échelle. Le Nombre maximal de degrés de parallélisme est limité. Les degrés de parallélisme de processus (DOP) pour la création d’un index est limitée à 2 de parallélisme maximal pour l’Édition Standard et 1 degré de parallélisme pour les éditions Web et Express. Ceci fait référence aux index columnstore créés sur des tables basées sur des disques et des tables à mémoire optimisée.

##  <a name="RDBMSS"></a> Sécurité SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express|
|-------------|----------------|--------------|---------|------------------------------------| 
|Sécurité au niveau des lignes|Oui|Oui|Oui|Oui|  
|Always Encrypted|Oui|Oui|Oui|Oui| 
|Masquage dynamique des données|Oui|Oui|Oui|Oui|   
|Audit de base|Oui|Oui|Oui|Oui| 
|Audit de granularité fine|Oui|Oui|Oui|Oui| 
|Chiffrement transparent de base de données|Oui|Non|Non|non|   
|Rôles définis par l’utilisateur|Oui|Oui|Oui|Oui| 
|Bases de données à relation contenant-contenu|Oui|Oui|Oui|Oui| 
|Chiffrement des sauvegardes|Oui|Oui|Non|non|  

##  <a name="RDBMSM"></a> Simplicité de gestion SGBDR  
  
|Fonctionnalité|Enterprise|Standard|Web|Express|   
|-------------|----------------|--------------|---------|------------------------|  
|Connexion administrateur dédiée|Oui|Oui|Oui|Oui avec indicateur de trace|Oui avec indicateur de trace|   
|Prise en charge de scripts PowerShell|Oui|Oui|Oui|Oui| 
|Prise en charge des opérations des composants d’application du niveau Données : extraction, déploiement, mise à niveau, suppression|Oui|Oui|Oui|Oui| 
|Automation de stratégie (vérification selon la planification et sur modification)|Oui|Oui|Oui|Non|non|   
|Collecteur de données de performances|Oui|Oui|Oui|Non|non| 
|Rapports de performances standard|Oui|Oui|Oui|Non|non| 
|Repères de plan et gel de plan|Oui|Oui|Oui|Non|non|   
|Requête directe de vues d'index (à l'aide de l'indicateur NOEXPAND)|Oui|Oui|Oui|Oui| 
|Maintenance automatique des vues indexées|Oui|Oui|Oui|Non|non| 
|Vues partitionnées distribuées|Oui|Non|Non|non| 
|Opérations d'index parallèles|Oui|Non|Non|non|  
|Utilisation automatique de vues indexées par l'optimiseur de requête|Oui|Non|Non|non| 
|Vérifications de cohérence en parallèle|Oui|Non|Non|non| 
|Point de contrôle de l’utilitaire SQL Server|Oui|Non|Non|non|    

##  <a name="Programmability"></a> Programmability  
  
|Fonctionnalité|Enterprise|Standard|Web|Express 
|-------------|----------------|--------------|---------|------------------------|  
|JSON|Oui|Oui|Oui|Oui|   
|Magasin de requêtes|Oui|Oui|Oui|Oui|   
|Temporal|Oui|Oui|Oui|Oui|   
|Prise en charge XML native|Oui|Oui|Oui|Oui| 
|Indexation XML|Oui|Oui|Oui|Oui| 
|Fonctions MERGE & UPSERT|Oui|Oui|Oui|Oui|   
|Types de données de date et d'heure|Oui|Oui|Oui|Oui|  
|Support d'internationalisation|Oui|Oui|Oui|Oui| 
|Recherche sémantique et en texte intégral|Oui|Oui|Oui|Oui|non| 
|Spécification d'une langue dans une requête|Oui|Oui|Oui|Oui|non|   
|Service Broker (messagerie)|Oui|Oui|Non (client uniquement)|Non (client uniquement)|Non (client uniquement)|   
|Transact-SQL, points de terminaison|Oui|Oui|Oui|Non|non| 
|Graphique|Oui|Oui|Oui|Oui|  


<sup>1</sup> La montée en puissance parallèle avec plusieurs nœuds de calcul nécessite un nœud principal.

## <a name="IS"></a> Integration Services

Pour plus d’informations sur les fonctionnalités d’Integration Services (SSIS) prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [fonctionnalités Integration Services pris en charge par les éditions de SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="SLS"></a> Services d’emplacement et spatiaux  
  
|Nom de la fonctionnalité|Enterprise|Standard|Web|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Index spatiaux|Oui|Oui|Oui|Oui|   
|Types de données planaires et géodésiques|Oui|Oui|Oui|Oui| 
|Bibliothèques spatiales avancées|Oui|Oui|Oui|Oui|   
|Importation/exportation de formats de données spatiales standard|Oui|Oui|Oui|Oui|   

  
## <a name="next-steps"></a>Étapes suivantes 
 [Éditions et des fonctionnalités prises en charge pour SQL Server 2017 - Windows](../sql-server/editions-and-components-of-sql-server-2017.md)  
 [Éditions et des fonctionnalités prises en charge pour SQL Server 2016 - Windows](../sql-server/editions-and-components-of-sql-server-2016.md)  
 [Éditions et des fonctionnalités prises en charge pour SQL Server 2014 - Windows](http://msdn.microsoft.com/library/cc645993(v=sql.120).aspx)  
 [Installation de SQL Server 2016](../database-engine/install-windows/installation-for-sql-server-2016.md)  
 [Spécifications de produit pour SQL Server](http://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb) 

  
  
