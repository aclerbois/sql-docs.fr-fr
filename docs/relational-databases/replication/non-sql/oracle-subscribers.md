---
title: Abonnés Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data types [SQL Server replication], non-SQL Server Subscribers
- Oracle Subscribers [SQL Server replication]
- non-SQL Server Subscribers, Oracle
- heterogeneous Subscribers, Oracle
- mapping data types [SQL Server replication]
ms.assetid: 591c0313-82ce-4689-9fc1-73752ff122cf
caps.latest.revision: 55
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ca824bafc48fac904e8be917b138adc503e97533
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="oracle-subscribers"></a>Abonnés Oracle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Depuis [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge les abonnements par émission de données à Oracle par le biais du fournisseur OLE DB Oracle fourni par Oracle.  
  
## <a name="configuring-an-oracle-subscriber"></a>Configuration d'un Abonné Oracle  
 Pour configurer un Abonné Oracle, procédez ainsi :  
  
1.  Installez et configurez le logiciel réseau client Oracle et le fournisseur OLE DB Oracle sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , afin que le serveur de distribution puisse se connecter à l'Abonné Oracle. Le logiciel réseau client Oracle doit être la version la plus récente. Oracle recommande aux utilisateurs d'installer les versions les plus récentes des logiciels clients. Par conséquent, il est fréquent que le logiciel client soit plus récent que le logiciel de base de données. La façon la plus simple d'installer le logiciel consiste à utiliser le programme d'installation Oracle Universal Installer sur le disque Oracle Client. Dans Oracle Universal Installer, vous devez fournir les informations suivantes :  
  
    |Informations|Description|  
    |-----------------|-----------------|  
    |Oracle Home|Chemin d'accès du répertoire d'installation des logiciels Oracle. Acceptez le chemin par défaut (C:\oracle\ora90 ou équivalent) ou entrez un autre chemin. Pour plus d'informations sur Oracle Home, consultez la section « Considérations sur Oracle Home » plus loin dans cette rubrique.|  
    |Nom d'Oracle Home|Alias pour le chemin du répertoire d'origine Oracle Home.|  
    |Type d'installation|Dans Oracle 10g, sélectionnez l'option d'installation **Runtime** ou **Administrator** .|  
  
2.  Créez un nom TNS pour l'Abonné. TNS (Transparent Network Substrate) est une couche de communication utilisée par les bases de données Oracle. Le nom du service TNS est le nom sous lequel une instance de base de données Oracle est connue sur un réseau. Vous attribuez un nom du service TNS quand vous configurez la connectivité pour une base de données Oracle. La réplication utilise le nom du service TNS pour identifier l'Abonné et établir les connexions.  
  
     Lorsque Oracle Universal Installer a terminé, utilisez Net Configuration Assistant pour configurer la connectivité réseau. Vous devez fournir quatre informations pour configurer la connectivité réseau. L'administrateur de base de données Oracle configure le réseau lorsqu'il installe la base de données et l'écouteur, et doit être en mesure de vous donner ces informations si vous ne les connaissez pas. Vous devez effectuer les opérations suivantes :  
  
    |Action|Description|  
    |------------|-----------------|  
    |Identifier la base de données|Il existe deux méthodes pour identifier la base de données. La première méthode utilise le SID (Oracle System Identifier) et est disponible dans chaque version d'Oracle. La seconde méthode utilise le nom de service, disponible à partir d'Oracle release 8.0 et versions ultérieures. Ces deux méthodes utilisent une valeur qui est configurée lors de la création de la base de données, et il est essentiel que la configuration réseau du client utilise la méthode de nommage déjà utilisée par l'administrateur lorsqu'il a configuré l'écouteur pour la base de données.|  
    |Identifier un alias réseau pour la base de données|Vous devez spécifier un alias réseau, utilisé pour accéder à la base de données Oracle. L'alias réseau est essentiellement un pointeur vers le SID distant ou le nom de service qui a été configuré lors de la création de la base de données ; il a reçu divers noms dans les différentes versions et produits d'Oracle, notamment Net Service Name et TNS Alias. SQL*Plus vous demande cet alias comme paramètre « Host String » lors de votre connexion.|  
    |Sélectionner le protocole réseau|Sélectionnez les protocoles que vous souhaitez prendre en charge. La plupart des applications utilisent TCP.|  
    |Spécifier les informations d'hôte pour identifier l'écouteur de la base de données|L'hôte est le nom ou l'alias DNS de l'ordinateur sur lequel s'exécute l'écouteur d'Oracle ; cet ordinateur est en général celui sur lequel réside la base de données. Pour certains protocoles, vous devez fournir des informations supplémentaires. Par exemple, si vous sélectionnez TCP, vous devez fournir le port sur lequel l'écouteur est à l'écoute des demandes de connexion sur la base de données cible. La configuration TCP par défaut utilise le port 1521.|  
  
3.  Créez une publication transactionnelle ou d'instantané, activez-la pour les Abonnés non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] puis créez un abonnement par émission de données pour l'Abonné. Pour plus d’informations, voir [Créer un abonnement pour un Abonné non-SQL Server](../../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  
  
### <a name="setting-directory-permissions"></a>Définition des autorisations sur les répertoires  
 Les comptes sous lesquels s'exécute le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur le serveur de distribution doivent détenir les autorisations de lecture et d'exécution pour le répertoire (et tous ses sous-répertoires) sur lequel le logiciel réseau client est installé.  
  
### <a name="testing-connectivity-between-the-sql-server-distributor-and-the-oracle-publisher"></a>Tester la connectivité entre le serveur de distribution SQL Server et le serveur de publication Oracle  
 Dans les dernières étapes de Net Configuration Assistant il peut y avoir une option pour tester la connexion au serveur d'abonnement Oracle. Avant de tester la connexion, assurez-vous que l'instance de base de données Oracle est en ligne et que l'écouteur Oracle est en cours d'exécution. Si le test échoue, contactez l'administrateur Oracle responsable de la base de données à laquelle vous essayez de vous connecter.  
  
 Lorsque vous avez réussi à vous connecter au serveur d'abonnement Oracle, essayez de vous connecter à la base de données à l'aide du même compte et du même mot de passe que vous avez configurés pour l'Agent de distribution de cet abonnement :  
  
1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
2.  Tapez `cmd` puis cliquez sur **OK**.  
  
3.  À l'invite de commandes, tapez :  
  
     `sqlplus <UserSchemaLogin>/<UserSchemaPassword>@<NetServiceName>`  
  
     Par exemple : `sqlplus replication/$tr0ngPasswerd@Oracle90Server`  
  
4.  Si la configuration du réseau a réussi, la connexion réussira et vous verrez une invite `SQL` .  
  
### <a name="considerations-for-oracle-home"></a>Considérations sur Oracle Home  
 Oracle prend en charge l'installation côte à côte des binaires d'application, mais la réplication ne peut utiliser qu'un seul jeu de binaires à un moment donné. Chaque jeu de binaires est associé à un répertoire d'origine Oracle Home ; les binaires se trouvent dans le répertoire %ORACLE_HOME%\bin. Vous devez vous assurer que le bon jeu de binaires (c'est-à-dire la dernière version du logiciel réseau client) est utilisé lorsque la réplication ouvre les connexions sur le serveur d'abonnement Oracle.  
  
 Ouvrez une session sur le serveur de distribution avec les comptes utilisés par le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et le service de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , et définissez les variables d'environnement appropriées. La variable %ORACLE_HOME% doit être définie de façon à faire référence au point d'installation que vous avez spécifié lors de l'installation du logiciel réseau client. Le %PATH% doit inclure le répertoire %ORACLE_HOME% \bin comme la première entrée Oracle rencontrée. Pour plus d'informations sur la définition des variables d'environnement, consultez la documentation Windows.  
  
> [!NOTE]  
>  Si vous avez plus d'un répertoire racine Oracle sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , assurez-vous que l'Agent de distribution utilise le fournisseur Oracle OLE DB le plus récent. Dans certains cas, par défaut, Oracle ne met pas à jour le fournisseur OLE DB lorsque vous mettez à jour les composants clients sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Désinstallez l'ancien fournisseur OLE DB et installez la dernière version. Pour plus d'informations sur l’installation et la désinstallation du fournisseur, consultez la documentation Oracle.  
  
## <a name="considerations-for-oracle-subscribers"></a>Points à prendre en compte pour les Abonnés Oracle  
 Outre les considérations exposées dans la rubrique [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md), réfléchissez aux points suivants lorsque vous répliquez vers les Abonnés Oracle :  
  
-   Oracle considère les chaînes vides aussi bien que les valeurs NULL comme NULL. Ceci est important si vous définissez une colonne [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] comme NOT NULL, et si vous répliquez cette colonne vers un serveur Abonné Oracle. Pour éviter les échecs lorsque vous appliquez des modifications sur l'Abonné Oracle, vous devez :  
  
    -   vous assurer qu'aucune chaîne vide n'est insérée dans la table publiée en tant que valeurs de colonne ;  
  
    -   utiliser le paramètre **–SkipErrors** pour l’Agent de distribution s’il vous est possible de recevoir les notifications d’échec dans le journal d’historique de l’Agent de distribution sans interrompre le traitement ; spécifier le code d’erreur Oracle 1400 (**-SkipErrors1400**) ;  
  
    -   modifier le script de création de table généré, en supprimant l'attribut NOT NULL de toutes les colonnes de caractères ayant des chaînes vides associées, et fournir le script modifié en tant que script de création personnalisé pour l'article en utilisant le paramètre @creation_script de [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
  
-   Les Abonnés Oracle prennent en charge une option de schéma de 0x4071. Pour plus d’informations sur les options de schéma, consultez [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
  
## <a name="mapping-data-types-from-sql-server-to-oracle"></a>Mappage des types de données à partir de SQL Server sur Oracle  
 La table qui suit montre les mappages de types de données qui sont utilisés lorsque les données sont répliquées sur un Abonné exécutant Oracle.  
  
|Type de données SQL Server|Type de données Oracle|  
|--------------------------|----------------------|  
|**bigint**|NUMBER(19,0)|  
|**binary(1-2000)**|RAW(1-2000)|  
|**binary(2001-8000)**|BLOB|  
|**bit**|NUMBER(1)|  
|**char(1-2000)**|CHAR(1-2000)|  
|**char(2001-4000)**|VARCHAR2(2001-4000)|  
|**char(4001-8000)**|CLOB|  
|**date**|DATE|  
|**datetime**|DATE|  
|**datetime2(0-7)**|TIMESTAMP(7) pour Oracle 9 et Oracle 10 ; VARCHAR(27) pour Oracle 8|  
|**datetimeoffset(0-7)**|TIMESTAMP(7) WITH TIME ZONE pour Oracle 9 et Oracle 10 ; VARCHAR(34) pour Oracle 8|  
|**decimal(1-38, 0-38)**|NUMBER(1-38, 0-38)|  
|**float(53)**|FLOAT|  
|**float**|FLOAT|  
|**geography**|BLOB|  
|**geometry**|BLOB|  
|**hierarchyid**|BLOB|  
|**image**|BLOB|  
|**Int**|NUMBER(10,0)|  
|**money**|NUMBER(19,4)|  
|**nchar(1-1000)**|CHAR(1-1000)|  
|**nchar(1001-4000)**|NCLOB|  
|**ntext**|NCLOB|  
|**numeric(1-38, 0-38)**|NUMBER(1-38, 0-38)|  
|**nvarchar(1-1000)**|VARCHAR2(1-2000)|  
|**nvarchar(1001-4000)**|NCLOB|  
|**nvarchar(max)**|NCLOB|  
|**real**|real|  
|**smalldatetime**|DATE|  
|**smallint**|NUMBER(5,0)|  
|**smallmoney**|NUMBER(10,4)|  
|**sql_variant**|Néant|  
|**sysname**|VARCHAR2(128)|  
|**texte**|CLOB|  
|**time(0-7)**|VARCHAR(16)|  
|**timestamp**|RAW (8)|  
|**tinyint**|NUMBER(3,0)|  
|**uniqueidentifier**|CHAR(38)|  
|**varbinary(1-2000)**|RAW(1-2000)|  
|**varbinary(2001-8000)**|BLOB|  
|**varchar(1-4000)**|VARCHAR2(1-4000)|  
|**varchar(4001-8000)**|CLOB|  
|**varbinary(max)**|BLOB|  
|**varchar(max)**|CLOB|  
|**xml**|NCLOB|  
  
## <a name="see-also"></a> Voir aussi  
 [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)   
 [S'abonner à des publications](../../../relational-databases/replication/subscribe-to-publications.md)  
  
  
