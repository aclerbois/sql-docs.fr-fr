---
title: Agent de lecture du journal des réplications | Microsoft Docs
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
- Log Reader Agent, executables
- Log Reader Agent, parameter reference
- agents [SQL Server replication], Log Reader Agent
- command prompt [SQL Server replication]
ms.assetid: 5487b645-d99b-454c-8bd2-aff470709a0e
caps.latest.revision: 51
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ed2f8686381e57dbac0b171ba28a2d1aaab77f11
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-log-reader-agent"></a>Agent de lecture du journal des réplications
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'Agent de lecture du journal des réplications est un fichier exécutable qui analyse le journal des transactions de chaque base de données configurée pour la réplication transactionnelle et qui copie les transactions devant être répliquées à partir du journal des transactions dans la base de données de distribution.  
  
> [!NOTE]  
>  Les paramètres peuvent être spécifiés dans n'importe quel ordre. Lorsque les paramètres optionnels ne sont pas spécifiés, les valeurs prédéfinies basées sur le profil d'agent par défaut sont utilisées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
logread [-?]   
-Publisher server_name[\instance_name]   
-PublisherDB publisher_database   
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-EncryptionLevel [0|1|2]]  
[-ExtendedEventConfigFile configuration_path_and_file_name]  
[-HistoryVerboseLevel [0|1|2]]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-LogScanThreshold scan_threshold]  
[-MaxCmdsInTran number_of_commands]  
[-MessageInterval message_interval]  
[-Output output_path_and_file_name]  
[-OutputVerboseLevel [0|1|2|3|4]]  
[-PacketSize packet_size]  
[-PollingInterval polling_interval]  
[-ProfileName profile_name]   
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherSecurityMode [0|1]]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]   
[-QueryTimeOut query_time_out_seconds]  
[-ReadBatchSize number_of_transactions]   
[-ReadBatchThreshold read_batch_threshold]  
[-RecoverFromDataErrors]  
```  
  
## <a name="arguments"></a>Arguments  
 **-?**  
 Affiche les informations d'utilisation.  
  
 **-Publisher** *server_name*[**\\***instance_name*]  
 Nom du serveur de publication. Spécifiez *server_name* pour l'instance par défaut de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez *server_name***\\***instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur.  
  
 **-PublisherDB** *publisher_database*  
 Nom de la base de données du serveur de publication.  
  
 **-Continuous**  
 Spécifie si l'Agent tente d'interroger les transactions répliquées de manière continue. S'il est spécifié, l'Agent interroge les transactions répliquées à partir de la source à des fréquences d'interrogation définies, même s'il n'y a pas de transactions en attente.  
  
 **-DefinitionFile** *def_path_and_file_name*  
 Chemin d'accès du fichier de définition d'agent. Un fichier de définition d'agent contient des arguments de ligne de commande pour l'agent. Le contenu du fichier est analysé en tant que fichier exécutable. Utilisez des guillemets doubles (") pour spécifier des valeurs d'argument qui contiennent des caractères arbitraires.  
  
 **-Distributor** *server_name*[**\\***instance_name*]  
 Nom du serveur de distribution. Spécifiez *server_name* pour l'instance par défaut de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez *server_name***\\***instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur.  
  
 **-DistributorLogin** *distributor_login*  
 Nom de connexion du serveur de distribution.  
  
 **-DistributorPassword** *distributor_password*  
 Mot de passe du serveur de distribution.  
  
 **-DistributorSecurityMode** [ **0**| **1**]  
 Spécifie le mode de sécurité du serveur de distribution. La valeur **0** indique le mode d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (par défaut), tandis que la valeur **1** indique le mode d'authentification [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows.  
  
 **-EncryptionLevel** [ **0** | **1** | **2** ]  
 Niveau du chiffrement SSL (Secure Sockets Layer) utilisé par l'Agent de lecture du journal lors de l'établissement de connexions.  
  
|Valeur EncryptionLevel|Description|  
|---------------------------|-----------------|  
|**0**|Spécifie que le chiffrement SSL n'est pas utilisé.|  
|**1**|Spécifie que le chiffrement SSL est utilisé, mais que l'agent ne vérifie pas si le certificat de serveur SSL est signé par un émetteur de confiance.|  
|**2**|Spécifie que le chiffrement SSL est utilisé et que le certificat est vérifié.|  
  
 Pour plus d’informations, consultez [Vue d’ensemble de la sécurité &#40;réplication&#41;](../../../relational-databases/replication/security/security-overview-replication.md).  
  
 **-ExtendedEventConfigFile** *configuration_path_and_file_name*  
 Spécifie le chemin d'accès et le nom du fichier de configuration XML d'événements étendus. Le fichier de configuration d'événements étendu vous permet de configurer des sessions et d'activer des événements pour le suivi.  
  
 **-HistoryVerboseLevel** [ **0**| **1**| **2**]  
 Spécifie la quantité d'informations d'historique journalisées pendant une opération du lecteur du journal. Vous pouvez réduire l'effet de la journalisation d'historique sur les performances en sélectionnant **1**.  
  
|Valeur HistoryVerboseLevel|Description|  
|-------------------------------|-----------------|  
|**0**||  
|**1**|Valeur par défaut. Met toujours à jour un message d'historique précédent du même état (démarrage, progression, succès, et ainsi de suite). Si aucun enregistrement précédent du même état n'existe, insère un nouvel enregistrement.|  
|**2**|Insère de nouveaux enregistrements d'historique, sauf s'il s'agit d'un enregistrement concernant notamment un message inactif ou un message de travail de longue durée, auquel cas les enregistrements précédents sont mis à jour.|  
  
 **-KeepAliveMessageInterval** *keep_alive_message_interval_seconds*  
 Durée en secondes au terme de laquelle le thread d'historique doit vérifier si l'une des connexions existantes attend une réponse du serveur. Vous pouvez réduire cette valeur pour éviter que l'agent de vérification ne marque l'Agent de lecture du journal comme suspect lors de l'exécution d'un lot de longue durée. La valeur par défaut est 300 secondes.  
  
 **-LoginTimeOut** *login_time_out_seconds*  
 Nombre de secondes avant l'expiration de la connexion. La valeur par défaut est 15 secondes.  
  
 **-LogScanThreshold** *scan_threshold*  
 À usage interne uniquement  
  
 **-MaxCmdsInTran** *number_of_commands*  
 Indique le nombre maximal d'instructions groupées dans une transaction lorsque le Lecteur du journal enregistre des commandes sur la base de données de distribution. L'utilisation de ce paramètre permet à l'Agent de lecture du journal et à l'Agent de distribution de scinder les transactions importantes (constituées de plusieurs commandes) sur le serveur de publication en plusieurs transactions plus petites lors de leur application sur l'Abonné. La spécification de ce paramètre permet de réduire les contentions sur le serveur de distribution et de réduire la latence entre le serveur de publication et l'Abonné. Du fait que la transaction d'origine est appliquée en plusieurs morceaux, l'Abonné peut accéder aux lignes d'une importante transaction logique du serveur de publication avant la fin de la transaction d'origine, ce qui rompt la stricte atomicité transactionnelle. La valeur par défaut est **0**, ce qui permet de conserver les limites de la transaction du serveur de publication.  
  
> [!NOTE]  
>  Ce paramètre est ignoré pour les publications non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d'informations, consultez la section « Configuration du travail du jeu de transactions » dans [Performance Tuning for Oracle Publishers](../../../relational-databases/replication/non-sql/performance-tuning-for-oracle-publishers.md).  
  
 **-MessageInterval** *message_interval*  
 Intervalle de temps utilisé pour la journalisation d'historique. Un événement d'historique est journalisé lorsque la valeur **MessageInterval** est atteinte une fois le dernier événement d'historique journalisé.  
  
 Si aucune transaction répliquée n'est disponible à la source, l'agent signale un message de non-transaction au serveur de distribution. Cette option spécifie combien de temps l'agent doit attendre avant de signaler un autre message de non-transaction. Les agents signalent toujours un message de non-transaction lorsqu'ils détectent qu'aucune transaction n'est disponible à la source après avoir précédemment traité des transactions répliquées. La valeur par défaut est 60 secondes.  
  
 **-Output** *output_path_and_file_name*  
 Chemin d'accès du fichier de sortie de l'agent. Si le nom du fichier n'est pas spécifié, la sortie est envoyée à la console. Si le nom de fichier spécifié existe, la sortie est ajoutée au fichier.  
  
 **-OutputVerboseLevel** [ **0**| **1**| **2** | **3** | **4** ]  
 Spécifie si la sortie doit être en clair.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Seuls les messages d'erreur sont imprimés.|  
|**1**|Tous les messages du rapport de progression de l'agent sont imprimés.|  
|**2** (par défaut)|Tous les messages d'erreur et tous les messages du rapport de progression sont imprimés.|  
|**3**|Les 100 premiers octets de chaque commande répliquée sont imprimés.|  
|**4**|Toutes les commandes répliquées sont imprimées.|  
  
 Les valeurs 2-4 sont utiles lors du débogage.  
  
 **-PacketSize** *packet_size*  
 Taille du paquet en octets. La valeur par défaut est 4 096 octets.  
  
 **-PollingInterval** *polling_interval*  
 Fréquence, en secondes, d'interrogation du journal pour l'obtention de transactions répliquées. La valeur par défaut est 5 secondes.  
  
 **-ProfileName** *profile_name*  
 Spécifie un profil d'agent à utiliser pour les paramètres d'agent. Si **ProfileName** a la valeur NULL, le profil d'agent est désactivé. Si **ProfileName** n'est pas spécifié, le profil par défaut du type d'agent est utilisé. Pour plus d’informations, consultez [Profils de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-PublisherFailoverPartner** *server_name*[**\\***instance_name*]  
 Spécifie l'instance du partenaire de basculement de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] participant à une session de mise en miroir de bases de données avec la base de données de publication. Pour plus d’informations, consultez [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
 **-PublisherSecurityMode** [ **0**| **1**]  
 Spécifie le mode de sécurité du serveur de publication. La valeur **0** indique le mode d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (par défaut), tandis que la valeur **1** indique le mode d'authentification Windows.  
  
 **-PublisherLogin** *publisher_login*  
 Nom de connexion du serveur de publication.  
  
 **-PublisherPassword** *publisher_password*  
 Mot de passe du serveur de publication.  
  
 **-QueryTimeOut** *query_time_out_seconds*  
 Nombre de secondes avant l'expiration de la requête. La valeur par défaut est 1800 secondes.  
  
 **-ReadBatchSize** *number_of_transactions*  
 Nombre maximal de transactions lues dans le journal des transactions de la base de données de publication par cycle de traitement, avec 500 comme valeur par défaut. L'agent continue à lire les transactions par lots jusqu'à ce que toutes les transactions aient été lues dans le journal. Ce paramètre n'est pas pris en charge pour les serveurs de publication Oracle.  
  
 **-ReadBatchThreshold** *number_of_commands*  
 Nombre de commandes de réplication à lire dans le journal des transactions avant que l'Agent de distribution ne les envoie à l'Abonné. La valeur par défaut est 0. Si ce paramètre n'est pas spécifié, l'Agent de lecture du journal lit la totalité du journal ou jusqu'au nombre spécifié **- ReadBatchSize** (nombre de transactions).  
  
 **-RecoverFromDataErrors**  
 Spécifie que l'Agent de lecture du journal poursuit son exécution lorsqu'il rencontre des erreurs dans les données de colonne publiées à partir d'un serveur de publication non-SQL Server. Par défaut, de telles erreurs provoquent l'échec de l'Agent de lecture du journal. Lorsque vous utilisez **-RecoverFromDataErrors**, les données de colonne erronées sont répliquées soit en tant que valeur NULL, soit en tant que valeur non nulle appropriée, et des messages d'avertissement sont journalisés dans la table [MSlogreader_history](../../../relational-databases/system-tables/mslogreader-history-transact-sql.md) . Ce paramètre est uniquement pris en charge pour les serveurs de publication Oracle.  
  
## <a name="remarks"></a>Notes   
  
> [!IMPORTANT]  
>  Si vous avez installé l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour s'exécuter sous un compte système local et non sous un compte d'utilisateur de domaine (valeur par défaut), le service peut uniquement accéder à l'ordinateur local. Si l'Agent de lecture du journal qui s'exécute sous l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est configuré pour utiliser le mode d'authentification Windows lorsqu'il se connecte à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'Agent de lecture du journal échoue. Le paramètre par défaut est l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d'informations sur la modification des comptes de sécurité, consultez [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 Pour démarrer l'Agent de lecture du journal, exécutez **logread.exe** à l'invite de commandes. Pour plus d’informations, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="change-history"></a>Historique des modifications  
  
|Mise à jour du contenu|  
|---------------------|  
|Ajout du paramètre **ExtendedEventConfigFile** .|  
  
## <a name="see-also"></a> Voir aussi  
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
