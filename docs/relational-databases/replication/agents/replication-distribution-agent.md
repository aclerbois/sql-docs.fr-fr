---
title: Agent de distribution de réplication | Microsoft Docs
ms.custom: ''
ms.date: 02/23/2016
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
- Distribution Agent, executables
- agents [SQL Server replication], Distribution Agent
- Distribution Agent, parameter reference
- command prompt [SQL Server replication]
ms.assetid: 7b4fd480-9eaf-40dd-9a07-77301e44e2ac
caps.latest.revision: 64
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: fb6fd99b07d623e2fb3d63ddb3c25adfd01efbb0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-distribution-agent"></a>Agent de distribution de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'Agent de distribution de réplication est un fichier exécutable qui déplace l'instantané (pour la réplication d'instantané et la réplication transactionnelle) et les transactions contenues dans les tables de base de données de distribution (pour la réplication transactionnelle) vers les tables de destination au niveau des Abonnés.  
  
> [!NOTE]  
>  Les paramètres peuvent être spécifiés dans n'importe quel ordre. Lorsque les paramètres optionnels ne sont pas spécifiés, les valeurs des paramètres du Registre prédéfinis sur l'ordinateur local sont utilisées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
distrib [-?]  
-Publisher server_name[\instance_name]  
-PublisherDB publisher_database  
-Subscriber server_name[\instance_name]  
-SubscriberDB subscriber_database   
[-AltSnapshotFolder alt_snapshot_folder_path]   
[-BcpBatchSize bcp_batch_size]  
[-CommitBatchSize commit_batch_size]  
[-CommitBatchThreshold commit_batch_threshold]  
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-Distributor distributor]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-EncryptionLevel [0|1|2]]  
[-ErrorFile error_path_and_file_name]  
[-ExtendedEventConfigFile configuration_path_and_file_name]  
[-FileTransferType [0|1]]  
[-FtpAddress ftp_address]  
[-FtpPassword ftp_password]   
[-FtpPort ftp_port]  
[-FtpUserName ftp_user_name]  
[-HistoryVerboseLevel [0|1|2|3]]  
[-Hostname host_name]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-MaxBcpThreads]  
[-MaxDeliveredTransactions number_of_transactions]  
[-MessageInterval message_interval]  
[-OledbStreamThreshold oledb_stream_threshold]  
[-Output output_path_and_file_name]  
[-OutputVerboseLevel [0|1|2]]  
[-PacketSize packet_size]  
[-PollingInterval polling_interval]  
[-ProfileName profile_name]  
[-Publication publication]  
[-QueryTimeOut query_time_out_seconds]  
[-QuotedIdentifier quoted_identifier]  
[-SkipErrors native_error_id [:...n]]  
[-SubscriberDatabasePath subscriber_path]  
[-SubscriberLogin subscriber_login]  
[-SubscriberPassword subscriber_password]  
[-SubscriberSecurityMode [0|1]]  
[-SubscriberType [0|1|3]]  
[-SubscriptionStreams [1|2|...64]]  
[-SubscriptionTableName subscription_table]  
[-SubscriptionType [0|1|2]]  
[-TransactionsPerHistory [0|1|...10000]]  
[-UseDTS]  
[-UseInprocLoader]  
[-UseOledbStreaming]  
```  
  
## <a name="arguments"></a>Arguments  
 **-?**  
 Imprime tous les paramètres disponibles.  
  
 **-Publisher** *server_name*[**\\***i**nstance_name*]  
 Nom du serveur de publication. Spécifiez *server_name* pour l'instance par défaut de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez *server_name***\\***instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur.  
  
 **-PublisherDB** *publisher_database*  
 Nom de la base de données du serveur de publication.  
  
 **-Subscriber** *server_name*[**\\***instance_name*]  
 Nom de l'Abonné. Spécifiez *server_name* pour l'instance par défaut de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez *server_name***\\***instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur.  
  
 **-SubscriberDB** *subscriber_database*  
 Nom de la base de données de l'Abonné.  
  
 **-AltSnapshotFolder** *alt_snapshot_folder_path*  
 Chemin d'accès au dossier contenant l'instantané initial pour un abonnement.  
  
 **-BcpBatchSize** *bcp_batch_size*  
 Nombre de lignes à envoyer dans une opération de copie en bloc. Lorsque vous effectuez une opération **bcp in** , la taille du lot correspond au nombre de lignes à envoyer au serveur en une transaction, mais aussi au nombre de lignes à envoyer avant que l'Agent de distribution ne journalise un message de progression **bcp** . Lorsque vous effectuez une opération **bcp out** , une taille de lot fixe de **1000** est utilisée.  
  
 **-CommitBatchSize** *commit_batch_size*  
 Nombre de transactions à délivrer à l'Abonné avant l'émission d'une instruction COMMIT. La valeur par défaut est 100.  
  
 **-CommitBatchThreshold**  *commit_batch_threshold*  
 Nombre de commandes de réplication à délivrer à l'Abonné avant l'émission d'une instruction COMMIT. La valeur par défaut est 1000.  
  
 **-Continuous**  
 Spécifie si l'agent tente d'interroger les transactions répliquées de manière continue. S'il est spécifié, l'Agent interroge les transactions répliquées de la source à des fréquences d'interrogation définies, même s'il n'y a pas de transactions en attente.  
  
 **-DefinitionFile** *def_path_and_file_name*  
 Chemin d'accès du fichier de définition d'agent. Un fichier de définition d'agent contient des arguments d'invite de commandes pour l'agent. Le contenu du fichier est analysé en tant que fichier exécutable. Utilisez des guillemets doubles (") pour spécifier des valeurs d'argument qui contiennent des caractères arbitraires.  
  
 **-Distributor** *distributor*  
 Nom du serveur de distribution. Pour la distribution du serveur de distribution (transmission de type push), le nom a comme valeur par défaut le nom du serveur de distribution local.  
  
 **-DistributorLogin** *distributor_login*  
 Nom de connexion du serveur de distribution.  
  
 **-DistributorPassword** *distributor_password*  
 Mot de passe du serveur de distribution.  
  
 **-DistributorSecurityMode** [ **0**| **1**]  
 Spécifie le mode de sécurité du serveur de distribution. La valeur 0 indique le mode d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , tandis que la valeur 1 indique le mode d'authentification Windows (valeur par défaut).  
  
 **-EncryptionLevel** [ **0** | **1** | **2** ]  
 Niveau du chiffrement SSL (Secure Sockets Layer) utilisé par l'Agent de distribution lors de l'établissement de connexions.  
  
|Valeur EncryptionLevel|Description|  
|---------------------------|-----------------|  
|**0**|Spécifie que le chiffrement SSL n'est pas utilisé.|  
|**1**|Spécifie que le chiffrement SSL est utilisé, mais que l'agent ne vérifie pas si le certificat de serveur SSL est signé par un émetteur de confiance.|  
|**2**|Spécifie que le chiffrement SSL est utilisé et que le certificat est vérifié.|  
  
 Pour plus d’informations, consultez [Vue d’ensemble de la sécurité &#40;réplication&#41;](../../../relational-databases/replication/security/security-overview-replication.md).  
  
 **-ErrorFile** *error_path_and_file_name*  
 Chemin d'accès et nom du fichier d'erreurs généré par l'Agent de distribution. Ce fichier est généré là où une erreur s'est produite lors de l'application de transactions de réplication sur l'Abonné ; les erreurs se produisant sur le serveur de publication ou de distribution ne sont pas journalisées dans ce fichier. Ce fichier contient les transactions de réplication ayant échoué, avec les messages d'erreur associés. S'il n'est pas spécifié, le fichier d'erreurs est créé dans le répertoire même de l'Agent de distribution. Le nom du fichier d'erreurs est le nom de l'Agent de distribution avec une extension .err. Si le nom de fichier spécifié existe, les messages d'erreur sont ajoutés au fichier. Ce paramètre peut contenir jusqu'à 256 caractères Unicode.  
  
 **-ExtendedEventConfigFile** *configuration_path_and_file_name*  
 Spécifie le chemin d'accès et le nom du fichier de configuration XML d'événements étendus. Le fichier de configuration d'événements étendu vous permet de configurer des sessions et d'activer des événements pour le suivi.  
  
 **-FileTransferType** [ **0**| **1**]  
 Spécifie le type de transfert de fichier. La valeur **0** indique la convention d'affectation des noms (UNC), tandis que la valeur **1** indique le protocole de transfert de fichiers (FTP).  
  
 **-FtpAddress** *ftp_address*  
 Adresse réseau du service FTP du serveur de distribution. S'il n'est pas spécifié, **DistributorAddress** est utilisé. Si **DistributorAddress** n'est pas spécifié, **Distributor** est utilisé.  
  
 **-FtpPassword** *ftp_password*  
 Mot de passe de l'utilisateur utilisé pour la connexion au service FTP.  
  
 **-FtpPort** *ftp_port*  
 Numéro de port du service FTP du serveur de distribution. S'il n'est pas spécifié, le numéro de port par défaut pour le service FTP (21) est utilisé.  
  
 **-FtpUserName**  *ftp_user_name*  
 Nom d'utilisateur utilisé pour la connexion au service FTP. S'il n'est pas spécifié, **anonymous** est utilisé.  
  
 **-HistoryVerboseLevel** [ **0** | **1** | **2** | **3** ]  
 Spécifie la quantité d'informations d'historique journalisées pendant une opération de distribution. Vous pouvez réduire l'effet de la journalisation d'historique sur les performances en sélectionnant **1**.  
  
|Valeur HistoryVerboseLevel|Description|  
|-------------------------------|-----------------|  
|**0**|Les messages de progression sont écrits dans la console ou dans un fichier de sortie. Les enregistrements d'historique ne sont pas journalisés dans la base de données de distribution.|  
|**1**|Valeur par défaut. Met toujours à jour un message d'historique précédent du même état (démarrage, progression, succès, et ainsi de suite). Si aucun enregistrement précédent du même état n'existe, insère un nouvel enregistrement.|  
|**2**|Insère de nouveaux enregistrements d'historique, sauf s'il s'agit d'un enregistrement concernant notamment un message inactif ou un message de travail de longue durée, auquel cas les enregistrements précédents sont mis à jour.|  
|**3**|Insère toujours de nouveaux enregistrements, sauf s'il s'agit de messages inactifs.|  
  
 **-Hostname** *host_name*  
 Nom d'hôte utilisé pour la connexion au serveur de publication Ce paramètre peut contenir jusqu'à 128 caractères Unicode.  
  
 **-KeepAliveMessageInterval** *keep_alive_message_interval_seconds*  
 Durée en secondes au terme de laquelle le thread d'historique doit vérifier si l'une des connexions existantes attend une réponse du serveur. Vous pouvez réduire cette valeur pour éviter que l'agent de vérification ne marque l'Agent de distribution comme suspect lors de l'exécution d'un lot de longue durée. La valeur par défaut est **300** secondes.  
  
 **-LoginTimeOut** *login_time_out_seconds*  
 Nombre de secondes avant l'expiration de la connexion. La valeur par défaut est **15** secondes.  
  
 **-MaxBcpThreads** *number_of_threads*  
 Spécifie le nombre d'opérations de copie en bloc pouvant être effectuées en parallèle. Le nombre maximal de threads et de connexions ODBC pouvant exister simultanément est, en privilégiant la valeur la plus petite, **MaxBcpThreads** ou le nombre de demandes de copie en bloc qui apparaissent dans la transaction de synchronisation dans la base de données de distribution. **MaxBcpThreads** doit présenter une valeur supérieure à **0** et n’a aucune limite supérieure codée en dur. La valeur par défaut est **2** fois le nombre de processeurs, la valeur maximale étant de **8**. Lors de l’application d’une capture instantanée qui a été générée au niveau du serveur de publication à l’aide de l’option de capture instantanée simultanée, un seul thread est utilisé, quel que soit le nombre que vous spécifiez pour **MaxBcpThreads**.  
  
 **-MaxDeliveredTransactions** *number_of_transactions*  
 Nombre maximal de transactions par émission ou par extraction appliquées aux Abonnés dans le cadre d'une synchronisation. La valeur **0** indique que la valeur maximale est un nombre infini de transactions. D'autres valeurs peuvent être utilisées par les Abonnés pour réduire la durée d'une synchronisation qui est extraite d'un serveur de publication.  
  
> [!NOTE]  
>  Si - MaxDeliveredTransactions et - Continuous sont tous les deux spécifiés, l'Agent de distribution remet le nombre spécifié de transactions puis arrête (bien que - Continuous soit spécifié). Vous devez redémarrer l'Agent de distribution une fois le travail terminé.  
  
 **-MessageInterval**  *message_interval*  
 Intervalle de temps utilisé pour la journalisation d'historique. Un événement d'historique est journalisé lorsque l'un de ces paramètres est atteint :  
  
-   La valeur **TransactionsPerHistory** est atteinte après la journalisation du dernier événement d’historique.  
  
-   La valeur **MessageInterval** est atteinte après la journalisation du dernier événement d’historique.  
  
 Si aucune transaction répliquée n'est disponible à la source, l'agent signale un message de non-transaction au serveur de distribution. Cette option spécifie combien de temps l'agent doit attendre avant de signaler un autre message de non-transaction. Les agents signalent toujours un message de non-transaction lorsqu'ils détectent qu'aucune transaction n'est disponible à la source après avoir précédemment traité des transactions répliquées. La valeur par défaut est 60 secondes.  
  
 **-OledbStreamThreshold** *oledb_stream_threshold*  
 Spécifie la taille minimale des données des objets blob, en octets, au-delà de laquelle les données seront liées sous la forme d'un flux. Vous devez spécifier **–UseOledbStreaming** pour utiliser ce paramètre. Les valeurs peuvent être comprises entre 400 et 1 048 576 octets, la valeur par défaut étant de 16 384 octets.  
  
 **-Output** *output_path_and_file_name*  
 Chemin d'accès du fichier de sortie de l'agent. Si le nom du fichier n'est pas spécifié, la sortie est envoyée à la console. Si le nom de fichier spécifié existe, la sortie est ajoutée au fichier.  
  
 **-OutputVerboseLevel** [ **0**| **1**| **2**]  
 Spécifie si la sortie doit être en clair. Si le niveau de détail est **0**, seuls les messages d'erreur sont imprimés. Si le niveau de détail est **1**, tous les messages du rapport de progression sont imprimés. Si le niveau de détail est **2** (valeur par défaut), tous les messages d'erreur et tous les messages du rapport de progression sont imprimés, ce qui peut s'avérer utile lors du débogage.  
  
 **-PacketSize** *packet_size*  
 Taille du paquet en octets. La valeur par défaut est 4 096 octets.  
  
 **-PollingInterval** *polling_interval*  
 Fréquence, en secondes, à laquelle la base de données de distribution est interrogée au sujet des transactions répliquées. La valeur par défaut est 5 secondes.  
  
 **-ProfileName** *profile_name*  
 Spécifie un profil d'agent à utiliser pour les paramètres d'agent. Si **ProfileName** a la valeur NULL, le profil d'agent est désactivé. Si **ProfileName** n'est pas spécifié, le profil par défaut du type d'agent est utilisé. Pour plus d’informations, consultez [Profils de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-Publication**  *publication*  
 Nom de la publication. Ce paramètre est uniquement valide si la publication est configurée de telle sorte qu'un instantané soit toujours disponible pour les nouveaux abonnements ou les abonnements réinitialisés.  
  
 **-QueryTimeOut** *query_time_out_seconds*  
 Nombre de secondes avant l'expiration de la requête. La valeur par défaut est 1800 secondes.  
  
 **-QuotedIdentifier** *quoted_identifier*  
 Spécifie le caractère de l'identificateur entre guillemets à utiliser. Le premier caractère de la valeur indique la valeur utilisée par l'Agent de distribution. Si **QuotedIdentifier** est utilisé sans valeur, l’Agent de distribution utilise un espace. Si **QuotedIdentifier** n’est pas utilisé, l’Agent de distribution utilise n’importe quel identificateur entre guillemets pris en charge par l’Abonné.  
  
 **-SkipErrors** *native_error_id* [**:***...n*]  
 Liste séparée par des virgules spécifiant les numéros d'erreur à ignorer par cet agent.  
  
 **-SubscriberDatabasePath** *subscriber_database_path*  
 Chemin d'accès à la base de données Jet (fichier .mdb) si **SubscriberType** a la valeur **2** (autorise une connexion à une base de données Jet sans nom de la source de données ODBC).  
  
 **-SubscriberLogin** *subscriber_login*  
 Nom de connexion de l'Abonné. Si **SubscriberSecurityMode** a la valeur **0** (pour l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ), ce paramètre doit être spécifié.  
  
 **-SubscriberPassword** *subscriber_password*  
 Mot de passe de l'Abonné. Si **SubscriberSecurityMode** a la valeur **0** (pour l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ), ce paramètre doit être spécifié.  
  
 **-SubscriberSecurityMode** [ **0**| **1**]  
 Spécifie le mode de sécurité de l'Abonné. La valeur **0** indique le mode d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , tandis que la valeur **1** indique le mode d'authentification Windows (valeur par défaut).  
  
 **-SubscriberType** [ **0**| **1**| **3**]  
 Spécifie le type de connexion d'Abonné utilisé par l'Agent de distribution.  
  
|Valeur SubscriberType|Description|  
|--------------------------|-----------------|  
|**0**|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|**1**|Source de données ODBC|  
|**3**|Source de données OLE DB|  
  
 **-SubscriptionStreams** [**0**|**1**|**2**|...**64**]  
 Nombre de connexions autorisées par l'Agent de distribution afin d'appliquer des lots de modifications en parallèle à un Abonné, tout en conservant bon nombre des caractéristiques transactionnelles présentes lors de l'utilisation d'un thread unique. Pour un serveur de publication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , une plage de valeurs comprises entre 1 et 64 est prise en charge. Ce paramètre est uniquement pris en charge lorsque le serveur de publication et le serveur de distribution s'exécutent sur [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ou version ultérieure. Ce paramètre n'est pas pris en charge ou doit être égal à 0 pour les Abonnés non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou les abonnements d'égal à égal.  
  
> [!NOTE]  
>  Si l'une des connexions ne réussit pas à s'exécuter ou n'est pas validée, toutes les connexions abandonneront le lot actuel, et l'Agent utilisera un flux unique pour une nouvelle tentative sur les lots ayant échoué. Avant que cette phase de nouvelle tentative ne se termine, il peut se produire des incohérences transactionnelles temporaires sur l'Abonné. Une fois que les lots ayant échoué sont validés avec succès, l'Abonné retrouve un état de cohérence transactionnelle.  
  
> [!IMPORTANT]  
>  Lorsque vous spécifiez une valeur égale ou supérieure à 2 pour **-SubscriptionStreams**, l’ordre dans lequel les transactions sont reçues au niveau de l’Abonné peut différer de l’ordre dans lequel elles ont été créées au niveau du serveur de publication. Si ce comportement provoque des violations de contrainte pendant la synchronisation, vous devez utiliser l'option NOT FOR REPLICATION pour désactiver l'application de contraintes pendant la synchronisation. Pour plus d’informations, consultez [Contrôler le comportement de déclencheurs et de contraintes au cours de la synchronisation &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
> [!NOTE]  
>  Les flux d'abonnements ne fonctionnent pas pour les articles configurés pour fournir [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Pour utiliser les flux d'abonnements, configurez les articles afin qu'ils fournissent des appels de procédures stockées à la place.  
  
 **-SubscriptionTableName** *subscription_table*  
 Nom de la table d'abonnements générée ou a utilisée au niveau de l'Abonné donné. Lorsqu’il n’est pas spécifié, la table [MSreplication_subscriptions &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) est utilisée. Utilisez cette option pour les systèmes de gestion de base de données (SGBD) qui ne prennent pas en charge les noms de fichier longs.  
  
 **-SubscriptionType** [ **0**| **1**| **2**]  
 Spécifie le type d'abonnement pour la distribution. La valeur **0** indique un abonnement par émission de données, la valeur **1** un abonnement par extraction et la valeur **2** un abonnement anonyme.  
  
 **-TransactionsPerHistory** [ **0**| **1**|... **10000**]  
 Spécifie l'intervalle de transaction pour la journalisation d'historique. Si le nombre de transactions validées après la dernière instance de journalisation d'historique est supérieur à cette option, un message d'historique est journalisé. La valeur par défaut est 100. Une valeur de **0** indique une quantité infinie de **TransactionsPerHistory**. See the preceding **–MessageInterval**parameter.  
  
 **-UseDTS**  
 Doit être spécifié en tant que paramètre pour une publication qui autorise la transformation de données.  
  
 **-UseInprocLoader**  
 Améliore les performances de l'instantané initial en forçant l'Agent de distribution à utiliser la commande BULK INSERT lors de l'application des fichiers d'instantané à l'Abonné. Ce paramètre est déconseillé parce qu'il n'est pas compatible avec le type de données XML. Si vous ne répliquez pas de données XML, ce paramètre peut être utilisé. Ce paramètre ne peut être utilisé ni avec les instantanés en mode caractère ni avec les Abonnés non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Si vous utilisez ce paramètre, le compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] au niveau de l'Abonné doit posséder des autorisations en lecture sur le répertoire où se trouvent les fichiers de données .bcp d'instantané. Lorsque ce paramètre n'est pas utilisé, l'agent (pour les Abonnés non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ) ou le pilote ODBC chargé par l'agent (pour les Abonnés [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ) lit les fichiers. Le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'est donc pas utilisé.  
  
 **-UseOledbStreaming**  
 S'il est spécifié, active la liaison des données des objets blob sous la forme d'un flux. Utilisez **-OledbStreamThreshold** pour spécifier la taille, en octets, au-dessus de laquelle un flux sera utilisé. **UseOledbStreaming** est activé par défaut. **UseOledbStreaming** écrit dans le dossier **C:\Program Files\Microsoft SQL Server\\<version\>\COM**.  
  
## <a name="remarks"></a>Notes   
  
> [!IMPORTANT]  
>  Si vous avez installé l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour s'exécuter sous un compte système local plutôt que sous un compte d'utilisateur de domaine (paramètre par défaut), le service peut uniquement accéder à l'ordinateur local. Si l'Agent de distribution qui s'exécute sous l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est configuré pour utiliser le mode d'authentification Windows lorsqu'il se connecte à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'Agent de distribution échoue. Le paramètre par défaut est l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d'informations sur la modification des comptes de sécurité, consultez [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 Pour démarrer l’Agent de distribution, exécutez **distrib.exe** à l’invite de commandes. Pour plus d’informations, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="change-history"></a>Historique des modifications  
  
|Mise à jour du contenu|  
|---------------------|  
|Ajout du paramètre **ExtendedEventConfigFile** .|  
  
## <a name="see-also"></a> Voir aussi  
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
