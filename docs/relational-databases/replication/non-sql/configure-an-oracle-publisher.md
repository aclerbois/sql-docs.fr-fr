---
title: Configurer un serveur de publication Oracle | Microsoft Docs
ms.custom: ''
ms.date: 09/05/2017
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
- Oracle publishing [SQL Server replication], configuring
ms.assetid: 240c8416-c8e5-4346-8433-07e0f779099f
caps.latest.revision: 60
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: cec4bd133542e289bb0b6f682aaa7f37dd8a566b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-an-oracle-publisher"></a>Configurer un serveur de publication Oracle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les publications provenant des serveurs de publication Oracle sont créées de la même façon que les publications d'instantané et les publications transactionnelles standard. Toutefois, pour créer une publication provenant d'un serveur de publication Oracle, vous devez préalablement effectuer les étapes ci-après (les étapes un, trois et quatre sont décrites en détail dans cette rubrique).  
  
1.  Créez un utilisateur de réplication administratif dans la base de données Oracle à l'aide du script fourni.  
  
2.  S’agissant des tables à publier, attribuez directement l’autorisation SELECT sur chacune d’elles (et non un rôle) à l’utilisateur administratif Oracle que vous avez créé à l’étape 1.  
  
3.  Installez le logiciel client Oracle et le fournisseur OLE DB Oracle sur le serveur de distribution [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , puis arrêtez et redémarrez l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Si le serveur de distribution s’exécute sur une plateforme 64 bits, vous devez utiliser la version 64 bits du fournisseur OLE DB Oracle.  
  
4.  Configurez la base de données Oracle en tant que serveur de publication sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge les scénarios divers suivants pour la réplication transactionnelle et d'instantané :  
  
-   Publication de données de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers des Abonnés non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  

-   La publication de données sur et depuis Oracle présente les restrictions suivantes :  
  | |Version 2016 ou antérieure |Version 2017 ou ultérieure |
  |-------|-------|--------|
  |Réplication depuis Oracle |Prise en charge d’Oracle 10g ou version antérieure uniquement |Prise en charge d’Oracle 10g ou version antérieure uniquement |
  |Réplication vers Oracle |Jusqu’à Oracle 12c |Non pris en charge |

 La réplication hétérogène sur les abonnés non SQL Server est déconseillée. La publication Oracle est déconseillée. Pour déplacer des données, créez des solutions à l'aide de la Capture de données modifiées et [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  


 Pour obtenir la liste des objets pouvant être répliqués à partir d’une base de données Oracle, consultez [Problèmes et limitations de conception des serveurs de publication Oracle](../../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md).  
  
> [!NOTE]  
>  Vous devez être membre du rôle serveur fixe **sysadmin** pour activer un serveur de publication ou un serveur de distribution, et pour créer une publication ou un abonnement Oracle à partir d'une publication Oracle.  
  
## <a name="creating-the-replication-administrative-user-schema-within-the-oracle-database"></a>Création du schéma utilisateur administratif de réplication dans la base de données Oracle  
 Les agents de réplication se connectent à la base de données Oracle et effectuent des opérations dans le cadre d'un schéma utilisateur que vous devez créer. Ce schéma doit bénéficier d'autorisations qui sont répertoriées dans la section suivante. Le schéma utilisateur possède tous les objets créés par le processus de réplication [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur le serveur de publication Oracle, à l'exception d'un synonyme public, **MSSQLSERVERDISTRIBUTOR**. Pour plus d'informations sur les objets créés dans la base de données Oracle, consultez [Objects Created on the Oracle Publisher](../../../relational-databases/replication/non-sql/objects-created-on-the-oracle-publisher.md).  
  
> [!NOTE]  
>  La suppression du synonyme public **MSSQLSERVERDISTRIBUTOR** et de l’utilisateur de réplication Oracle configuré à l’aide de l’option **CASCADE** supprime tous les objets de réplication du serveur de publication Oracle.  
  
 Un exemple de script est fourni pour vous aider à configurer le schéma utilisateur de réplication Oracle. Ce script est disponible dans le répertoire suivant, après l’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] : *\<lecteur>*:\\\Program Files\Microsoft SQL Server\\*\<Nom_Instance>* \MSSQL\Install\oracleadmin.sql. Il est également fourni dans la rubrique [Script to Grant Oracle Permissions](../../../relational-databases/replication/non-sql/script-to-grant-oracle-permissions.md).  
  
 Connectez-vous à la base de données Oracle à l'aide d'un compte possédant des droits DBA, puis exécutez le script. Ce script vous invite à entrer le nom d'utilisateur et le mot de passe du schéma utilisateur administratif de réplication, ainsi que l'espace de table par défaut dans lequel créer les objets (l'espace de table doit déjà exister dans la base de données Oracle). Pour plus d’informations sur la spécification d’autres espaces de table pour des objets, consultez [Gérer des espaces disque logiques Oracle](../../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md). Choisissez un nom d’utilisateur et un mot de passe fort et n’oubliez pas de les noter, car vous devez fournir ces informations quand vous configurez la base de données Oracle en tant que serveur de publication. Il est recommandé de n'utiliser le schéma que pour les objets requis par la réplication et de ne pas y créer de tables à publier.  
  
### <a name="creating-the-user-schema-manually"></a>Création manuelle du schéma utilisateur  
 Si vous créez manuellement le schéma utilisateur administratif de réplication, vous devez lui attribuer les autorisations suivantes, soit directement, soit par le biais d'un rôle de base de données.  
  
-   CREATE PUBLIC SYNONYM et DROP PUBLIC SYNONYM  
  
-   CREATE PROCEDURE  
  
-   CREATE SEQUENCE  
  
-   CREATE SESSION  
  
 Vous devez également attribuer directement à l'utilisateur les autorisations suivantes (et non par le biais d'un rôle) :  
  
-   CREATE ANY TRIGGER Cela est requis uniquement pour la réplication de capture instantanée et la réplication transactionnelle.  
  
-   CREATE TABLE  
  
-   CREATE VIEW  
  
## <a name="installing-and-configuring-oracle-client-networking-software-on-the-sql-server-distributor"></a>Installation et configuration de logiciels réseau clients Oracle sur le serveur de distribution SQL Server  
 Vous devez installer et configurer les logiciels réseau clients Oracle et le fournisseur OLE DB Oracle sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] afin que celui-ci puisse se connecter au serveur de publication Oracle. Après l'installation des logiciels, attribuez les autorisations appropriées aux dossiers d'installation des logiciels, puis arrêtez et redémarrez l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour vous assurer de la mise à jour de tous les paramètres (les autorisations sont décrites ci-dessous dans la section « Définition des autorisations de répertoire »).  
  
> [!NOTE]  
>  La version des logiciels réseau clients Oracle doit être la plus récente. Oracle recommande aux utilisateurs d'installer les versions les plus récentes des logiciels clients. Par conséquent, il est fréquent que le logiciel client soit plus récent que le logiciel de base de données.  
  
 Le moyen le plus rapide d'installer et de configurer les logiciels réseau clients consiste à utiliser le programme d'installation universel (Universal Installer) d'Oracle ainsi que l'Assistant de configuration du réseau (Net Configuration Assistant) sur le disque client d'Oracle.  
  
 Dans Oracle Universal Installer, vous devez fournir les informations suivantes :  
  
|Informations|Description|  
|-----------------|-----------------|  
|Oracle Home|Chemin d'accès du répertoire d'installation des logiciels Oracle. Acceptez le chemin par défaut (C:\oracle\ora90 ou équivalent) ou entrez un autre chemin. Pour plus d'informations sur Oracle Home, consultez la section « Considérations sur Oracle Home » plus loin dans cette rubrique.|  
|Nom d'Oracle Home|Alias pour le chemin du répertoire d'origine Oracle Home.|  
|Type d'installation|Dans Oracle 10g, sélectionnez l'option d'installation **Administrator** .|  
  
 Lorsque Oracle Universal Installer a terminé, utilisez Net Configuration Assistant pour configurer la connectivité réseau. Vous devez fournir quatre informations pour configurer la connectivité réseau. L'administrateur de base de données Oracle configure le réseau lorsqu'il installe la base de données et l'écouteur, et doit être en mesure de vous donner ces informations si vous ne les connaissez pas. Vous devez effectuer les opérations suivantes :  
  
|Action|Description|  
|------------|-----------------|  
|Identifier la base de données|Il existe deux méthodes pour identifier la base de données. La première méthode utilise le SID (Oracle System Identifier) et est disponible dans chaque version d'Oracle. La seconde méthode utilise le nom de service, disponible à partir d'Oracle release 8.0 et versions ultérieures. Ces deux méthodes utilisent une valeur qui est configurée lors de la création de la base de données, et il est essentiel que la configuration réseau du client utilise la méthode de nommage déjà utilisée par l'administrateur lorsqu'il a configuré l'écouteur pour la base de données.|  
|Identifier un alias réseau pour la base de données|Vous devez spécifier un alias réseau, utilisé pour accéder à la base de données Oracle. Fournissez également cet alias lorsque vous identifiez la base de données Oracle en tant que serveur de publication sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . L'alias réseau est essentiellement un pointeur vers le SID distant ou le nom de service qui a été configuré lors de la création de la base de données ; il a reçu divers noms dans les différentes versions et produits d'Oracle, notamment Net Service Name et TNS Alias. SQL*Plus vous demande cet alias comme paramètre « Host String » lors de votre connexion.|  
|Sélectionner le protocole réseau|Sélectionnez les protocoles que vous souhaitez prendre en charge. La plupart des applications utilisent TCP.|  
|Spécifier les informations d'hôte pour identifier l'écouteur de la base de données|L'hôte est le nom ou l'alias DNS de l'ordinateur sur lequel s'exécute l'écouteur d'Oracle ; cet ordinateur est en général celui sur lequel réside la base de données. Pour certains protocoles, vous devez fournir des informations supplémentaires. Par exemple, si vous sélectionnez TCP, vous devez fournir le port sur lequel l'écouteur est à l'écoute des demandes de connexion sur la base de données cible. La configuration TCP par défaut utilise le port 1521.|  
  
### <a name="setting-directory-permissions"></a>Définition des autorisations de répertoire  
 le compte sous lequel le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute sur le serveur de distribution doit être doté d'autorisations en lecture et en écriture sur le répertoire (et tous les sous-répertoires) dans lequel les logiciels réseau clients Oracle sont installés.  
  
### <a name="testing-connectivity-between-the-sql-server-distributor-and-the-oracle-publisher"></a>Test de la connectivité entre le serveur de distribution SQL Server et le serveur de publication Oracle  
 L’Assistant de configuration du réseau propose en fin d’étapes une option de test de la connexion au serveur de publication Oracle. Avant de tester la connexion, assurez-vous que l'instance de base de données Oracle est en ligne et que l'écouteur Oracle est en cours d'exécution. Si le test échoue, contactez l'administrateur Oracle responsable de la base de données à laquelle vous essayez de vous connecter.  
  
 Après avoir établi une connexion au serveur de publication Oracle, essayez de vous connecter à la base de données à l'aide du compte et du mot de passe associés au schéma utilisateur administratif de réplication que vous avez créé. Les opérations suivantes doivent être effectuées lors de l'exécution sous le même compte Windows que celui utilisé par le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
2.  Tapez `cmd` puis cliquez sur **OK**.  
  
3.  À l'invite de commandes, tapez :  
  
     `sqlplus <UserSchemaLogin>/<UserSchemaPassword>@<NetServiceName>`  
  
     Par exemple : `sqlplus replication/$tr0ngPasswerd@Oracle90Server`  
  
4.  Si la configuration du réseau a réussi, la connexion réussit, puis vous voyez une invite `SQL`.  
  
5.  Si vous rencontrez des problèmes pour vous connecter à la base de données Oracle, consultez la section « Le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas se connecter à l'instance de base de données Oracle » dans [Troubleshooting Oracle Publishers](../../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).  
  
### <a name="considerations-for-oracle-home"></a>Considérations sur Oracle Home  
 Oracle prend en charge l'installation côte à côte des binaires d'application, mais la réplication ne peut utiliser qu'un seul jeu de binaires à un moment donné. Chaque jeu de binaires est associé à un répertoire d'origine Oracle Home ; les binaires se trouvent dans le répertoire %ORACLE_HOME%\bin. Vous devez vérifier que l'ensemble de binaires correct (notamment la dernière version des logiciels réseau clients) est utilisée lorsque la réplication établit des corrections avec le serveur de publication Oracle.  
  
 Ouvrez une session sur le serveur de distribution avec les comptes utilisés par le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et le service de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , et définissez les variables d'environnement appropriées. La variable %ORACLE_HOME% doit être définie de façon à faire référence au point d'installation que vous avez spécifié lors de l'installation du logiciel réseau client. Le %PATH% doit inclure le répertoire %ORACLE_HOME% \bin comme la première entrée Oracle rencontrée. Pour plus d'informations sur la définition des variables d'environnement, consultez la documentation Windows.  
  
## <a name="configuring-the-oracle-database-as-a-publisher-at-the-sql-server-distributor"></a>Configuration de la base de données Oracle en tant que serveur de publication sur le serveur de distribution SQL Server  
 Les serveurs de publication Oracle utilisent toujours un serveur de distribution distant ; vous devez configurer une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui agisse en tant que serveur de distribution pour votre serveur de publication Oracle (un serveur de publication Oracle ne peut utiliser qu'un seul serveur de distribution, mais un serveur de distribution unique peut servir plusieurs serveurs de publication Oracle). Une fois un serveur de distribution configuré, identifiez l'instance de base de données Oracle en tant que serveur de publication sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] via [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], Transact-SQL ou Replication Management Objects. Pour plus d’informations sur la configuration d’un serveur de distribution, consultez [Configurer la distribution](../../../relational-databases/replication/configure-distribution.md).  
  
> [!NOTE]  
>  Un serveur de publication Oracle ne peut pas porter le même nom que son serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou que les serveurs de publication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilisant le même serveur de distribution.  
  
 Lorsque vous identifiez la base de données Oracle en tant que serveur de publication, vous devez choisir une option de publication Oracle : Complète (Complete) ou Oracle Gateway. Une fois qu'un serveur de publication est identifié, vous ne pouvez pas modifier cette option sans supprimer et reconfigurer ce serveur. L'option Complète fournit aux publications d'instantané et aux publications transactionnelles l'ensemble complet des fonctionnalités prises en charge pour la publication Oracle. L'option Oracle Gateway fournit des optimisations de conception spécifiques, destinées à améliorer les performances lorsque la réplication sert de passerelle entre des systèmes.  
  
 Lorsque le serveur de publication Oracle a été identifié sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , la réplication crée un serveur lié portant le même nom que le service TNS de la base de données Oracle. Ce serveur lié ne peut être utilisé que par la réplication. Si vous devez vous connecter au serveur de publication Oracle par l’intermédiaire d’une connexion de serveur lié, créez un autre nom de service TNS puis utilisez ce nom lors de l’appel de [sp_addlinkedserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
 Pour configurer un serveur de publication Oracle et créer une publication, consultez [Create a Publication from an Oracle Database](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Considérations sur l’administration des serveurs de publication Oracle](../../../relational-databases/replication/non-sql/administrative-considerations-for-oracle-publishers.md)   
 [Mappage de type de données pour les serveurs de publication Oracle](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [Glossaire des termes de la publication Oracle](../../../relational-databases/replication/non-sql/glossary-of-terms-for-oracle-publishing.md)   
 [Vue d’ensemble de la publication Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)  
  
  
