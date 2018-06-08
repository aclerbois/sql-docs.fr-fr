---
title: Spécifications des capacités maximales pour SQL Server | Microsoft Docs
ms.date: 11/6/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: sql-non-specified
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- objects [SQL Server]
- number capacity specifications [SQL Server]
- size [SQL Server], capacity specifications
- number of objects
- capacity specifications [SQL Server]
- maximum capacity specifications [SQL Server]
- size [SQL Server]
- replication capacity specifications [SQL Server]
- objects [SQL Server], capacity specifications
- Database Engine [SQL Server], capacity specifications
ms.assetid: 13e95046-0e76-4604-b561-d1a74dd824d7
caps.latest.revision: 88
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 1cd2e8b76948a5f2d89b19157894d72de378f3ae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="maximum-capacity-specifications-for-sql-server"></a>Spécifications des capacités maximales pour SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
 > Pour du contenu relatif aux versions précédentes de SQL Server, consultez [Spécifications des capacités maximales pour SQL Server](https://msdn.microsoft.com/en-US/library/ms143432(SQL.120).aspx).

  Les tableaux suivants présentent la taille maximale et le nombre maximal des différents objets définis dans les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour naviguer jusqu'à la table d'une technologie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , cliquez sur son lien :  
  
 [Objets du moteur de base de données SQL Server](#Engine)  
  
 [Objets utilitaires SQL Server](#Utility)  
  
 [Objets d'application de la couche Données SQL Server](#DAC)  
  
 [Objets de réplication SQL Server](#Replication)  
  
##  <a name="Engine"></a> [!INCLUDE[ssDE](../includes/ssde-md.md)] Objets  
 Taille maximale et nombre maximal des différents objets définis dans les bases de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou référencés dans les instructions [!INCLUDE[tsql](../includes/tsql-md.md)] .  
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] objet||Tailles maximales/nombres maximaux [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64-bit)|Informations supplémentaires|  
|---------------------------------------------------------|-|------------------------------------------------------------------|----------------------------|  
|Taille du lot||65 536 * Taille des paquets réseau|La taille des paquets réseau représente la taille des paquets TDS (Tabular Data Stream) utilisés pour la communication entre des applications et le [!INCLUDE[ssDE](../includes/ssde-md.md)]relationnel. La taille par défaut s'élève à 4 Ko ; elle est contrôlée par l'option de configuration Taille du paquet réseau.|  
|Octets par colonne de chaîne courte||8,000||  
|Octets par clause GROUP BY, ORDER BY||8,060||  
|Octets par clé d’index||900 octets pour un index cluster. 1 700 pour un index non cluster.|Le nombre maximal d’octets dans une clé d’index cluster s’élève à 900 dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour une clé d’index non cluster, la valeur maximale est 1700 octets.<br /><br /> Vous pouvez définir une clé à l’aide de colonnes de longueur variable dont les tailles maximales, additionnées, dépasse la limite. Toutefois, la taille combinée des données de ces colonnes ne peut jamais dépasser la limite.<br /><br /> Dans un index non cluster, vous pouvez inclure des colonnes non-clés supplémentaires. Elles n’entrent pas en compte pour la limite de taille de la clé. Les colonnes non-clés peuvent améliorer les performances de certaines requêtes.|  
|Octets par clé d’index pour les tables optimisées en mémoire||2500 octets pour un index non cluster. Aucune limite pour un index de hachage, tant que toutes les clés d’index s’ajustent sur la ligne.|Sur une table optimisée en mémoire, un index non cluster ne peut pas avoir de colonnes clés dont la taille maximale déclarée dépasse 2500 octets. Peu importe que la taille des données réelles dans les colonnes clés soit inférieure aux tailles maximales déclarées.<br /><br /> Pour une clé d’index de hachage, il n’existe aucune limite de taille.<br /><br /> Pour les index sur les tables optimisées en mémoire, il n’existe pas de concept de colonnes incluses, puisque tous les index couvrent, par nature, toutes les colonnes.<br /><br /> Pour une table optimisée en mémoire, même si la taille de ligne est de 8060 octets, certaines colonnes de longueur variable peuvent être physiquement stockées en dehors de ces 8060 octets. Toutefois, les tailles maximales déclarés de toutes les colonnes clés pour tous les index sur une table, plus les éventuelles colonnes de longueur fixe supplémentaires dans la table, doivent tenir dans les 8060 octets.|  
|Octets par clé étrangère||900||  
|Octets par clé primaire||900||  
|Octets par ligne||8,060|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge le stockage avec dépassement de ligne, qui permet d’envoyer hors ligne les colonnes de longueur variable. Seule une racine de 24 octets est stockée dans l'enregistrement principal des colonnes de longueur variable envoyées hors ligne ; par conséquent, la limite effective par ligne est supérieure dans les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez la rubrique « Données de dépassement de ligne de plus de 8 Ko » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .|  
|Octets par ligne dans les tables optimisées en mémoire||8,060|À compter de [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] , les tables optimisées en mémoire prennent en charge le stockage hors ligne. Les colonnes de longueur variable sont envoyées hors ligne si la taille maximale de toutes les colonnes dans la table dépasse 8060 octets. Cette décision est prise au moment de la compilation. Seule une référence de huit octets est stockée dans la ligne pour les colonnes stockées hors ligne. Pour plus d’informations, consultez [Taille de la table et des lignes dans les tables optimisées en mémoire](../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md).|  
|Octets dans le texte source d'une procédure stockée||Taille de lot inférieure ou 250 Mo||  
|Octets par colonne **varchar(max)**, **varbinary(max)**, **xml**, **text**ou **image**||2^31-1||  
|Caractères par colonne **ntext** ou **nvarchar (max)**||2^30-1||  
|Index cluster par table|| 1||  
|Colonnes dans les clauses GROUP BY, ORDER BY||Limité uniquement par le nombre d'octets||  
|Colonnes ou expressions dans une instruction GROUP BY WITH CUBE ou WITH ROLLUP||10||  
|Colonnes par clé d’index||32|Si la table contient au moins un index XML, la clé de clustering de la table d’utilisateur est limitée à 31 colonnes, car la colonne XML est ajoutée à la clé de clustering du principal index XML. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous pouvez inclure des colonnes non-clés dans un index non cluster pour éviter la limitation à un maximum de 32 colonnes clés. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../relational-databases/indexes/create-indexes-with-included-columns.md).|  
|Colonnes par clé étrangère||32||  
|Colonnes par clé primaire||32||  
|Colonnes par tableau non large||1,024||  
|Colonnes par tableau large||30,000||  
|Colonnes par instruction SELECT||4,096||  
|Colonnes par instruction INSERT||4,096||  
|Connexions par client||Valeur maximale des connexions configurées||  
|Taille de la base de données||524 272 téraoctets||  
|Bases de données par instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]||32,767||  
|Groupes de fichiers par base de données||32,767||  
|Groupes de fichiers par base de données pour les données optimisées en mémoire|| 1||  
|Fichiers par base de données||32,767||  
|Taille de fichier (données)||16 téraoctets||  
|Taille de fichier (journal)||2 téraoctets||  
|Fichiers de données pour les données optimisées en mémoire par base de données||4 096 dans [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)]. Les versions ultérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’imposent pas une telle limite stricte.||  
|Fichier delta par fichier de données pour les données optimisées en mémoire|| 1||  
|Références de table de clé étrangère par table||Sortantes = 253. Entrantes = 10 000.|Pour connaître les restrictions associées, consultez [Create Foreign Key Relationships](../relational-databases/tables/create-foreign-key-relationships.md).|  
|Longueur d'identificateur (en caractères)||128||  
|Instances par ordinateur||50 instances sur un serveur autonome.<br /><br /> 25 instances sur un cluster de basculement si vous utilisez un disque de cluster partagé, car l'option stockée pour votre installation de cluster [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge 50 instances sur un cluster de basculement si vous choisissez les partages SMB comme option de stockage de votre installation de cluster.||  
|Index par table optimisée en mémoire||999 à partir de [!INCLUDE[ssSQL17](../includes/ssSQL17-md.md)] et dans [!INCLUDE[ssSDSFull](../includes/ssSDSFull-md.md)]<br/>8 dans [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)] et [!INCLUDE[ssSQL15](../includes/ssSQL15-md.md)]||  
|Longueur d’une chaîne contenant des instructions SQL (taille du traitement)||65 536 * Taille des paquets réseau|La taille des paquets réseau représente la taille des paquets TDS (Tabular Data Stream) utilisés pour la communication entre des applications et le [!INCLUDE[ssDE](../includes/ssde-md.md)]relationnel. La taille par défaut s'élève à 4 Ko ; elle est contrôlée par l'option de configuration Taille du paquet réseau.|  
|Verrous par connexion||Verrous maximaux par serveur||  
|Verrous par instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]||Limité uniquement par la mémoire|Cette valeur s’applique à l’allocation de verrouillage statique. Les verrous dynamiques sont uniquement limités par la mémoire.|  
|Niveaux d’imbrication des procédures stockées||32|Si une procédure stockée accède à plus de 64 bases de données ou à plus de 2 bases entrelacées, vous recevez un message d’erreur.|  
|Sous-requêtes imbriquées||32||  
|Niveaux de déclencheurs imbriqués||32||  
|Index non cluster par table||999||  
|Nombre d'expressions distinctes dans la clause GROUP BY lorsque l'un des éléments suivants est présent : CUBE, ROLLUP, GROUPING SETS, WITH CUBE, WITH ROLLUP||32||  
|Nombre de jeux de regroupement générés par les opérateurs dans la clause GROUP BY||4,096||  
|Paramètres par procédure stockée||2,100||  
|Paramètres par fonction définie par l'utilisateur||2,100||  
|REFERENCES par table||253||  
|Lignes par table||Limité par le stockage disponible||  
|Tables par base de données||Limité par le nombre d'objets dans une base de données|Les objets de base de données comprennent des tables, des vues, des procédures stockées, des fonctions définies par l’utilisateur, des déclencheurs, des règles, des valeurs par défaut et des contraintes. Au total, le nombre de tous les objets d'une base de données ne peut pas dépasser 2 147 483 647.|  
|Partitions par table ou index partitionné||15,000||  
|Statistiques sur les colonnes non indexées||30,000|| 
|Tables par instruction SELECT||Limité uniquement par les ressources disponibles||  
|Déclencheurs par table||Limité par le nombre d'objets dans une base de données|Les objets de base de données comprennent des tables, des vues, des procédures stockées, des fonctions définies par l’utilisateur, des déclencheurs, des règles, des valeurs par défaut et des contraintes. Au total, le nombre de tous les objets d'une base de données ne peut pas dépasser 2 147 483 647.|  
|Colonnes par instruction UPDATE (tableaux larges)||4096||  
|Connexions utilisateur||32,767||  
|Index XML||249||  
  
##  <a name="Utility"></a> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objets utilitaires  
 Taille maximale et nombre maximal des différents objets testés dans l’utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objet utilitaire||Tailles maximales/nombres maximaux [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64-bit)|  
|----------------------------------------------|-|------------------------------------------------------------------|  
|Ordinateurs (ordinateurs physique ou ordinateurs virtuels) par utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]||100|  
|Instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] par ordinateur||5|  
|Nombre total d'instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] par utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]||200*|  
|Bases de données utilisateur par instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], notamment les applications de la couche Données||50|  
|Nombre total de bases de données utilisateur par utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]||1,000|  
|Groupes de fichiers par base de données|| 1|  
|Fichiers de données par groupe de fichiers|| 1|  
|Fichiers journaux par base de données|| 1|  
|Volumes par ordinateur||3|  
  
 * Le nombre maximal d’instances gérées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prises en charge par l’utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut varier en fonction de la configuration matérielle du serveur. Pour obtenir des informations de prise en main, consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](https://msdn.microsoft.com/library/ee210548.aspx). [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’est pas disponible dans toutes les éditions de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx).    
  
##  <a name="DAC"></a> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objets d’application de la couche Données  
 Taille maximale et nombre maximal des différents objets testés dans les applications de la couche Données (DAC) [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Object DAC||Tailles maximales/nombres maximaux [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64-bit)|  
|------------------------------------------|-|------------------------------------------------------------------|  
|Bases de données par DAC|| 1|  
|Objets par DAC*||Limité par le nombre d'objets dans une base de données ou la mémoire disponible.|  
  
 * Les types d’objets inclus dans la limite sont des utilisateurs, des tables, des vues, des procédures stockées, des fonctions définies par l’utilisateur, des types de données définis par l’utilisateur, des rôles de base de données, des schémas et des types de table définis par l’utilisateur.  
  
##  <a name="Replication"></a> Objets de réplication  
 Taille maximale et nombre maximal des différents objets définis dans la réplication [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objet de réplication||Taille maximale/nombre maximal dans SQL Server (64 bits)|  
|--------------------------------------------------|-|---------------------------------------------------|  
|Articles (publication de fusion)||2048|  
|Articles (publication d'instantané ou transactionnelle)||32,767|  
|Colonnes dans une table* (publication de fusion)||246|  
|Colonnes dans une table** (publication d’instantané ou transactionnelle[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] )||1,000|  
|Colonnes dans une table** (publication d’instantané ou transactionnelle Oracle)||995|  
|Octets pour une colonne utilisée dans un filtre de lignes (publication de fusion)||1,024|  
|Octets pour une colonne utilisée dans un filtre de lignes (publication d'instantané ou transactionnelle)||8,000|  

 * Si le suivi de lignes est utilisé pour la détection de conflits (valeur par défaut), la table de base peut inclure 1 024 colonnes au maximum, mais les colonnes doivent être filtrées à partir de l’article afin que 246 colonnes au maximum soient publiées. Si le suivi de colonnes est utilisé, la table de base peut inclure 246 colonnes au maximum.  
  
 ** La table de base peut inclure le nombre maximal de colonnes autorisées dans la base de données de publication (1 024 pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]), mais les colonnes doivent être filtrées à partir de l’article si elles sont plus nombreuses que le maximum spécifié pour le type de publication.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Paramètres de l'outil d'analyse de configuration système](../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)   
 [Fonctionnalités et tâches de l’utilitaire SQL Server](../relational-databases/manage/sql-server-utility-features-and-tasks.md)  
  
  
