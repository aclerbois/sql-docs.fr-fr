---
title: Agent de fusion de réplication | Microsoft Docs
ms.custom: ''
ms.date: 08/24/2016
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
- Merge Agent, executables
- Merge Agent, parameter reference
- agents [SQL Server replication], Merge Agent
- command prompt [SQL Server replication]
ms.assetid: fe1e7f60-b0c8-45e9-a5e8-4fedfa73d7ea
caps.latest.revision: 64
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: f37984c25fb722245d433e18904b2d48de7707df
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-merge-agent"></a>Replication Merge Agent
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'Agent de fusion de réplication est un fichier exécutable d'utilitaire qui applique l'instantané initial contenu dans les tables de base de données aux Abonnés. Il fusionne également les modifications de données incrémentielles ayant eu lieu sur le serveur de publication après la création de l'instantané initial, puis harmonise les conflits soit en fonction des règles que vous configurez, soit à l'aide d'un programme de résolution personnalisé que vous créez.  
  
> [!NOTE]  
>  Les paramètres peuvent être spécifiés dans n'importe quel ordre. Lorsque les paramètres optionnels ne sont pas spécifiés, les valeurs des paramètres du Registre prédéfinis sur l'ordinateur local sont utilisées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
replmerg [-?]   
-Publisher server_name[\instance_name]  
-PublisherDB publisher_database  
-Publication publication  
-Subscriber server_name[\instance_name]  
-SubscriberDB subscriber_database  
[-AltSnapshotFolder alt_snapshot_folder_path]  
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-DestThreads number_of_destination_threads]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-DownloadGenerationsPerBatch download_generations_per_batch]  
[-DownloadReadChangesPerBatch download_read_changes_per_batch]  
[-DownloadWriteChangesPerBatch download_write_changes_per_batch]  
[-DynamicSnapshotLocation dynamic_snapshot_location]  
[-EncryptionLevel [0|1|2]]  
[-ExchangeType [1|2|3]]  
[-FastRowCount [0|1]]  
[-FileTransferType [0|1]]  
[-ForceConvergenceLevel [0|1|2 (Publisher|Subscriber|Both)]]  
[-FtpAddress ftp_address]  
[-FtpPassword ftp_password]  
[-FtpPort ftp_port]  
[-FtpUserNameftp_user_name]  
[-HistoryVerboseLevel [0|1|2|3]]  
[-Hostname host_name]  
[-InteractiveResolution [0|1]]  
[-InternetLogin internet_login]  
[-InternetPassword internet_password]  
[-InternetProxyLogin internet_proxy_login]  
[–InternetProxyPassword internet_proxy_password]  
[-InternetProxyServer internet_proxy_server]  
[-InternetSecurityMode [0|1]]  
[-InternetTimeout internet_timeout]  
[-InternetURL internet_url]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-MakeGenerationInterval make_generation_interval_seconds]  
[-MaxBcpThreads number_of_threads]  
[-MaxDownloadChanges number_of_download_changes]  
[-MaxUploadChanges number_of_upload_changes]  
[-MetadataRetentionCleanup [0|1]]  
[-Output]  
[-OutputVerboseLevel [0|1|2]]  
[-ParallelUploadDownload [0|1]]  
[-PacketSize packet_size]   
[-PollingInterval polling_interval]  
[-ProfileName profile_name]  
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]  
[-PublisherSecurityMode [0|1]]  
[-QueryTimeOut query_time_out_seconds]  
[-SrcThreads number_of_source_threads]  
[-StartQueueTimeout start_queue_timeout_seconds]  
[-SubscriberConflictClean [0|1]]  
[-SubscriberDatabasePath subscriber_path]  
[-SubscriberDBAddOption [0|1|2|3]]  
[-SubscriberLogin subscriber_login]  
[-SubscriberPassword subscriber_password   
[-SubscriberSecurityMode [0|1]]  
[-SubscriberType [0|1|2|3|4|5|6|7|8|9]]  
[-SubscriptionType [0|1|2]]  
[-SyncToAlternate [0|1]  
[-UploadGenerationsPerBatch upload_generations_per_batch]  
[-UploadReadChangesPerBatch upload_read_changes_per_batch]  
[-UploadWriteChangesPerBatch upload_write_changes_per_batch]  
[-UseInprocLoader]  
[-Validate [0|1|2|3]]  
[-ValidateInterval validate_interval]  
```  
  
## <a name="arguments"></a>Arguments  
 **-?**  
 Imprime tous les paramètres disponibles.  
  
 **-Publisher** *server_name*[**\\***instance_name*]  
 Nom du serveur de publication. Spécifiez *server_name* pour l'instance par défaut de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez *server_name***\\***instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur.  
  
 **-PublisherDB** *publisher_database*  
 Nom de la base de données du serveur de publication.  
  
 **-Publication** *publication*  
 Nom de la publication. Ce paramètre est uniquement valide si la publication est configurée de telle sorte qu'un instantané soit toujours disponible pour les nouveaux abonnements ou les abonnements réinitialisés.  
  
 **-Subscriber** *server_name*[**\\***instance_name*]  
 Nom de l'Abonné. Spécifiez *server_name* pour l'instance par défaut de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez *server_name***\\***instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur.  
  
 **-SubscriberDB** *subscriber_database*  
 Nom de la base de données de l'Abonné.  
  
 **-AltSnapshotFolder** *alt_snapshot_folder_path*  
 Chemin d'accès au dossier contenant l'instantané initial pour un abonnement.  
  
 **-Continuous**  
 Spécifie si l'agent tente d'interroger les transactions répliquées de manière continue. S'il est spécifié, l'Agent interroge les transactions répliquées de la source à des fréquences d'interrogation définies, même s'il n'y a pas de transactions en attente.  
  
 **-DestThreads** *number_of_destination_threads*  
 Spécifie le nombre de threads de destination utilisés par l'Agent de fusion pour appliquer les modifications à la destination. La destination est le serveur de publication au cours du téléchargement ascendant et l'Abonné au cours du téléchargement descendant. La valeur par défaut est 4.  
  
 **-DefinitionFile** *def_path_and_file_name*  
 Chemin d'accès du fichier de définition d'agent. Un fichier de définition d'agent contient des arguments d'invite de commandes pour l'agent. Le contenu du fichier est analysé en tant que fichier exécutable. Utilisez des guillemets doubles (") pour spécifier des valeurs d'argument qui contiennent des caractères arbitraires.  
  
 **-Distributor** *server_name*[**\\***instance_name*]  
 Nom du serveur de distribution. Spécifiez *server_name* pour l'instance par défaut de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez *server_name***\\***instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce serveur. Pour la distribution du serveur de distribution (transmission de type push), le nom a comme valeur par défaut l'instance par défaut de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur l'ordinateur local.  
  
 **-DistributorLogin** *distributor_login*  
 Nom de connexion du serveur de distribution.  
  
 **-DistributorPassword** *distributor_password*  
 Mot de passe du serveur de distribution.  
  
 **-DistributorSecurityMode** [ **0**| **1**]  
 Spécifie le mode de sécurité du serveur de distribution. La valeur **0** indique le mode d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (par défaut), tandis que la valeur **1** indique le mode d'authentification Windows.  
  
 **-DownloadGenerationsPerBatch** *download_generations_per_batch*  
 Nombre de générations à traiter dans un lot unique lors du téléchargement des modifications du serveur de publication vers l'Abonné. Une génération est définie comme un groupe logique de modifications par article. La valeur par défaut pour un lien de communication fiable est 100. La valeur par défaut pour un lien de communication non fiable est 10.  
  
 **-DownloadReadChangesPerBatch** *download_read_changes_per_batch*  
 Nombre de modifications à lire dans un lot unique lors du téléchargement des modifications du serveur de publication vers l'Abonné. La valeur par défaut est 100.  
  
 **-DownloadWriteChangesPerBatch** *download_write_changes_per_batch*  
 Nombre de modifications à appliquer dans un lot unique lors du téléchargement des modifications du serveur de publication vers l'Abonné. La valeur par défaut est 100.  
  
 **-DynamicSnapshotLocation** *dynamic_snapshot_location*  
 Emplacement des fichiers d'instantanés de données filtrées lorsque la publication utilise des filtres de lignes paramétrables.  
  
 **-EncryptionLevel** [ **0** | **1** | **2** ]  
 Niveau du chiffrement SSL (Secure Sockets Layer) utilisé par l'Agent de fusion lors de l'établissement de connexions.  
  
|Valeur EncryptionLevel|Description|  
|---------------------------|-----------------|  
|**0**|Spécifie que le chiffrement SSL n'est pas utilisé.|  
|**1**|Spécifie que le chiffrement SSL est utilisé, mais que l'agent ne vérifie pas si le certificat de serveur SSL est signé par un émetteur de confiance.|  
|**2**|Spécifie que le chiffrement SSL est utilisé et que le certificat est vérifié.|  
  
 Pour plus d’informations, consultez [Vue d’ensemble de la sécurité &#40;Réplication&#41;](../../../relational-databases/replication/security/security-overview-replication.md).  
  
 **-ExchangeType** [ **1**| **2**| **3**]  
 > [!WARNING]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)] Afin de limiter le téléchargement, utilisez **@subscriber_upload_options** de **sp_addmergearticle** à la place.  
  
 Spécifie le type d'échange de données au cours de la synchronisation parmi les types suivants :  
  
|Valeur ExchangeType|Description|  
|------------------------|-----------------|  
|**1**|L'agent doit télécharger les modifications de données de l'Abonné vers le serveur de publication.|  
|**2**|L'agent doit télécharger les modifications de données du serveur de publication vers l'Abonné.|  
|**3** (valeur par défaut)|L'agent doit tout d'abord télécharger les modifications de l'Abonné vers le serveur de publication, puis du serveur de publication vers l'Abonné. Vous devez utiliser cette option avec la synchronisation Web.|  
  
 Les articles en téléchargement uniquement vous permettent de contrôler le comportement de synchronisation d'articles individuels dans une publication, et peuvent aussi accroître les performances. Pour plus d’informations, consultez [Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
  
 Si vous utilisez **ExchangeType** pour séparer la phase de téléchargement (upload et download) de la réplication de fusion en sessions distinctes, vous devez exécuter l'agent de fusion avec **ExchangeType** défini à 1, puis réexécuter l'agent de fusion avec la valeur 2. Si vous n'arrivez pas à exécuter l'agent de fusion avec les deux paramètres, les métadonnées seront supprimées et vous devrez réinitialiser l'abonnement (sans téléchargement).  
  
 **-FastRowCount** [**0**|**1**]  
 Spécifie le type de méthode de calcul du nombre de lignes à utiliser pour la validation du nombre de lignes. La valeur **1** (valeur par défaut) indique la méthode rapide. La valeur **0** indique la méthode complète de calcul du nombre de lignes.  
  
 **-FileTransferType** [**0**|**1**]  
 Spécifie le type de transfert de fichier. La valeur **0** indique la convention d'affectation des noms (UNC), tandis que la valeur **1** indique le protocole de transfert de fichiers (FTP).  
  
 **-ForceConvergenceLevel** [**0**|**1**|**2** ( **Publisher**| **Subscriber**| **Both**)]  
 Spécifie le niveau de convergence que l'Agent de fusion doit utiliser. Il peut prendre l'une des valeurs suivantes :  
  
|Valeur ForceConvergenceLevel|Description|  
|---------------------------------|-----------------|  
|**0** (valeur par défaut)|Valeur par défaut. Effectue une fusion standard sans convergence supplémentaire.|  
|**1**|Force la convergence pour toutes les générations.|  
|**2**|Force la convergence pour toutes les générations et corrige les lignages endommagés. Lorsque vous spécifiez cette valeur, indiquez à quel niveau les lignages doivent être corrigés : le serveur de publication, l'Abonné ou bien les deux.|  
  
 **-FtpAddress** *ftp_address*  
 Adresse réseau du service FTP du serveur de distribution. S'il n'est pas spécifié, **Distributor** est utilisé.  
  
 **-FtpPassword** *ftp_password*  
 Mot de passe de l'utilisateur utilisé pour la connexion au service FTP.  
  
 **-FtpPort** *ftp_port*  
 Numéro de port du service FTP du serveur de distribution. S'il n'est pas spécifié, le numéro de port par défaut pour le service FTP (21) est utilisé.  
  
 **-FtpUserName** *ftp_user_name*  
 Nom d'utilisateur utilisé pour la connexion au service FTP. S'il n'est pas spécifié, anonymous est utilisé.  
  
 **-HistoryVerboseLevel** [**1**|**2**|**3**]  
 Spécifie la quantité d'informations d'historique journalisées pendant une opération de fusion. Vous pouvez réduire l'effet de la journalisation d'historique sur les performances en sélectionnant **1**.  
  
|Valeur HistoryVerboseLevel|Description|  
|-------------------------------|-----------------|  
|**0**|Journalise le dernier message d'état de l'agent, les derniers détails de session et les erreurs éventuelles.|  
|**1**|Journalise les détails incrémentiels de session à chaque état de session, y compris le pourcentage accompli, ainsi que le dernier message d'état de l'agent, les derniers détails de session et les erreurs éventuelles.|  
|**2**|Valeur par défaut. Journalise les détails incrémentiels de session à chaque état de session et les détails de session au niveau de l'article, y compris le pourcentage accompli, ainsi que le dernier message d'état de l'agent, les derniers détails de session et les erreurs éventuelles. Les messages de l'état de l'agent sont également journalisés.|  
|**3**|Identique à **-HistoryVerboseLevel** = **2**, à la différence qu'un nombre plus important de messages de progression de l'agent sont journalisés.|  
  
 **-Hostname** *host_name*  
 Nom de réseau de l'ordinateur local. La valeur par défaut est le nom de l'ordinateur local.  
  
 **-InteractiveResolution** [**0**|**1**]  
 Spécifie si la résolution de conflit interactive est utilisée lorsqu'un conflit se produit pendant la synchronisation. La valeur par défaut est **0**, indiquant que la résolution de conflit interactive n'est pas utilisée.  
  
 **-InternetLogin** *internet_login*  
 Spécifie le nom de connexion utilisé lors de la connexion à une DLL ISAPI d'écoute de réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui nécessite une authentification.  
  
 **-InternetPassword** *internet_password*  
 Spécifie le mot de passe utilisé lors de la connexion à une DLL ISAPI d'écoute de réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui nécessite une authentification.  
  
 **-InternetProxyLogin**  *internet_proxy_login*  
 Spécifie le nom de connexion utilisé lors de la connexion à un serveur proxy, défini dans *internet_proxy_server*, qui nécessite une authentification.  
  
 **–InternetProxyPassword**  *internet_proxy_password*  
 Spécifie le mot de passe utilisé lors de la connexion à un serveur proxy, défini dans *internet_proxy_server*, qui nécessite une authentification.  
  
 **-InternetProxyServer**  *internet_proxy_server*  
 Spécifie le serveur proxy à utiliser lors de l'accès à la ressource HTTP spécifiée dans *internet_url*.  
  
 **-InternetSecurityMode** [**0**|**1**]  
 Spécifie le mode de sécurité IIS utilisé lors de la connexion au serveur Web pendant la synchronisation Web. La valeur **0** indique l'authentification de base, tandis que la valeur **1** indique l'authentification intégrée Windows (par défaut).  
  
 **-InternetTimeout** *internet_timeout*  
 Nombre de secondes avant l'expiration d'une connexion à la DLL ISAPI d'écoute de réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 **-InternetURL** *internet_url*  
 Spécifie l'URL utilisée pour la connexion à la DLL ISAPI d'écoute de réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Cette propriété doit être spécifiée.  
  
 **-KeepAliveMessageInterval** *keep_alive_message_interval_seconds*  
 Durée en secondes au terme de laquelle le thread d'historique doit vérifier si l'une des connexions existantes attend une réponse du serveur. Vous pouvez réduire cette valeur pour éviter que l'agent de vérification ne marque l'Agent de fusion comme suspect lors de l'exécution d'un lot de longue durée. La valeur par défaut est **300** secondes.  
  
 **-LoginTimeOut** *login_time_out_seconds*  
 Nombre de secondes avant l'expiration de la connexion. La valeur par défaut est **15** secondes.  
  
 **-MakeGenerationInterval** *make_generation_interval_seconds*  
 Délai en secondes entre les créations des générations, ou lots de modifications, à télécharger vers le client. La valeur par défaut est **1** seconde.  
  
 Makegeneration est le processus qui prépare les modifications du serveur de publication à télécharger vers les abonnés et peut constituer un goulot d'étranglement lors des téléchargements. Si le processus makegeneration s'est déjà exécuté dans l'intervalle spécifié par **-MakeGenerationInterval**, le processus est ignoré pour la session de synchronisation actuelle. Cela peut profiter à la concurrence de synchronisation et s'avère particulièrement utile si les abonnés ne pensent pas télécharger les modifications.  
  
 **-MaxBcpThreads** *number_of_threads*  
 Spécifie le nombre d'opérations de copie en bloc pouvant être effectuées en parallèle. Le nombre maximal de threads et de connexions ODBC pouvant exister simultanément est, en privilégiant la valeur la plus petite, **MaxBcpThreads** ou le nombre de demandes de copie en bloc qui apparaissent dans la table système **sysmergeschemachange** dans la base de données de publication. **MaxBcpThreads** doit avoir une valeur supérieure à 0 et n'a aucune limite supérieure codée en dur. La valeur par défaut est **1**.  
  
 **-MaxDownloadChanges** *number_of_download_changes*  
 Spécifie le nombre maximal de lignes modifiées qui doivent être téléchargées du serveur de publication vers l'Abonné. Le nombre de lignes téléchargées peut être supérieur au nombre maximal spécifié. Cela est dû au fait que des générations complètes sont traitées et que des threads de destination parallèles peuvent s'exécuter, chacun traitant au moins 100 modifications lors du premier passage. Par défaut, toutes les modifications qui sont prêtes à être téléchargées sont envoyées.  
  
 **-MaxUploadChanges** *number_of_upload_changes*  
 Spécifie le nombre maximal de lignes modifiées qui doivent être téléchargées de l'Abonné au serveur de publication. Le nombre de lignes téléchargées peut être supérieur au nombre maximal spécifié. Cela est dû au fait que des générations complètes sont traitées et que des threads de destination parallèles peuvent s'exécuter, chacun traitant au moins 100 modifications lors du premier passage. Par défaut, toutes les modifications qui sont prêtes à être téléchargées sont envoyées.  
  
 **-MetadataRetentionCleanup** [**0**|**1**]  
 Spécifie si des métadonnées sont supprimées de [MSmerge_genhistory](../../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md), [MSmerge_contents](../../../relational-databases/system-tables/msmerge-contents-transact-sql.md), [MSmerge_tombstone](../../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md), [MSmerge_past_partition_mappings](../../../relational-databases/system-tables/msmerge-past-partition-mappings-transact-sql.md)et de [MSmerge_current_partition_mappings](../../../relational-databases/system-tables/msmerge-current-partition-mappings.md) en fonction de la période de rétention de publication. La valeur par défaut est **1**, indiquant que le nettoyage doit se produire. La valeur **0** indique que le nettoyage ne doit pas avoir lieu automatiquement.  
  
 **-Output** *output_path_and_file_name*  
 Chemin d'accès du fichier de sortie de l'agent. Si le nom du fichier n'est pas spécifié, la sortie est envoyée à la console. Si le nom de fichier spécifié existe, la sortie est ajoutée au fichier.  
  
 **-OutputVerboseLevel** [**0**|**1**|**2**]  
 Spécifie si la sortie doit être en clair. Si le niveau de détail est **0**, seuls les messages d'erreur sont imprimés. Si le niveau de détail est **1**, tous les messages du rapport de progression sont imprimés. Si le niveau de détail est **2** (valeur par défaut), tous les messages d'erreur et tous les messages du rapport de progression sont imprimés, ce qui peut s'avérer utile lors du débogage.  
  
 **-ParallelUploadDownload** [**0**|**1**]  
 Spécifie si l'Agent de fusion doit traiter en parallèle les modifications téléchargées vers le serveur de publication et celles téléchargées vers l'Abonné, ce qui peut s'avérer utile dans les environnements de grands volumes avec une bande passante réseau élevée. Si **ParallelUploadDownload** a la valeur **1**, le traitement parallèle est activé.  
  
 **-PacketSize**  
 Taille du paquet en octets. La valeur par défaut est 4 096 octets.  
  
 **-PollingInterval** *polling_interval*  
 Fréquence, en secondes, le serveur de publication ou l'Abonné est interrogé pour obtenir les modifications de données. La valeur par défaut est 60 secondes.  
  
 **-ProfileName** *profile_name*  
 Spécifie un profil d'agent à utiliser pour les paramètres d'agent. Si **ProfileName** a la valeur NULL, le profil d'agent est désactivé. Si **ProfileName** n'est pas spécifié, le profil par défaut du type d'agent est utilisé. Pour plus d’informations, consultez [Profils de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-PublisherFailoverPartner** *server_name*[**\\***instance_name*]  
 Spécifie l'instance du partenaire de basculement de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] participant à une session de mise en miroir de bases de données avec la base de données de publication. Pour plus d’informations, consultez [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
 **-PublisherLogin** *publisher_login*  
 Nom de connexion du serveur de publication. Si **PublisherSecurityMode** a la valeur **0** (pour l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ), ce paramètre doit être spécifié.  
  
 **-PublisherPassword** *publisher_password*  
 Mot de passe du serveur de publication. Si **PublisherSecurityMode** a la valeur **0** (pour l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ), ce paramètre doit être spécifié.  
  
 **-PublisherSecurityMode** [**0**|**1**]  
 Spécifie le mode de sécurité du serveur de publication. La valeur **0** indique le mode d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (par défaut), tandis que la valeur **1** indique le mode d'authentification Windows.  
  
 **-QueryTimeOut** *query_time_out_seconds*  
 Nombre de secondes avant l'expiration de la requête. La valeur par défaut est 300 secondes. L'Agent de fusion utilise également la valeur de **QueryTimeout** pour déterminer combien de temps il doit attendre la génération d'un instantané partitionné lorsque cette valeur est supérieure à 1 800.  
  
 **-SrcThreads** *number_of_source_threads*  
 Spécifie le nombre de threads source utilisés par l'Agent de fusion pour énumérer les modifications de la source. La source est l'Abonné au cours du téléchargement ascendant et le serveur de publication au cours du téléchargement descendant. La valeur par défaut est **3**.  
  
 **-StartQueueTimeout** *start_queue_timeout_seconds*  
 Durée maximale en secondes de l'attente de l'Agent de fusion lorsque le nombre de processus de fusion simultanés en cours d'exécution a atteint la limite définie par la propriété **@max_concurrent_merge** de **sp_addmergepublication**. Si le nombre maximal de secondes est atteint et que l'Agent de fusion attend toujours, celui-ci se ferme. La valeur 0 signifie que l'Agent attend indéfiniment, bien qu'il soit possible de l'annuler.  
  
 **-SubscriberDatabasePath** *subscriber_database_path*  
 Chemin d'accès à la base de données Jet (fichier .mdb) si **SubscriberType** a la valeur **2** (autorise une connexion à une base de données Jet sans nom de la source de données ODBC).  
  
 **-SubscriberDBAddOption** [**0**| **1**| **2**| **3**]  
 Indique si une base de données de l'Abonné existe.  
  
|Valeur SubscriberDBAddOption|Description|  
|---------------------------------|-----------------|  
|**0**|Utilise la base de données existante (valeur par défaut).|  
|**1**|Crée une base de données d'Abonné vide.|  
|**2**|Crée une base de données et la joint au fichier spécifié.|  
|**3**|Crée une base de données, joint la base de données et active tous les abonnements qui peuvent exister au niveau du fichier.|  
  
> [!NOTE]  
>  Lorsque vous utilisez les valeurs **2** et **3**, le chemin d'accès à la base de données pour l'Abonné doit être spécifié dans l'option **SubscriberDatabasePath** .  
  
 **-SubscriberLogin** *subscriber_login*  
 Nom de connexion de l'Abonné. Si **SubscriberSecurityMode** a la valeur **0** (pour l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ), ce paramètre doit être spécifié.  
  
 **-SubscriberPassword** *subscriber_password*  
 Mot de passe de l'Abonné. Si **SubscriberSecurityMode** a la valeur **0** (pour l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ), ce paramètre doit être spécifié.  
  
 **-SubscriberSecurityMode** [ **0**| **1**]  
 Spécifie le mode de sécurité de l'Abonné. La valeur **0** indique le mode d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (par défaut), tandis que la valeur **1** indique le mode d'authentification Windows.  
  
 **-SubscriberConflictClean** [ **0**| **1**]  
 Indique si les tables en conflit sont nettoyées au niveau de l'Abonné pendant le processus de synchronisation. La valeur **1** indique que les tables en conflit au niveau de l'Abonné sont nettoyées. Ce paramètre est utilisé uniquement pour les abonnements aux publications avec une journalisation des conflits décentralisée.  
  
 **-SubscriberType** [ **0**| **1**| **3**| **4**| **5**| **6**| **7**| **8**]  
 Spécifie le type de connexion d'Abonné utilisé par l'Agent de fusion. Seule la valeur par défaut **0** est prise en charge pour ce paramètre.  
  
 **-SubscriptionType**[ **0**| **1**| **2**]  
 Spécifie le type d'abonnement pour la distribution. La valeur **0** indique un abonnement par émission de données (valeur par défaut), la valeur **1** un abonnement par extraction et la valeur **2** un abonnement anonyme.  
  
 **-SyncToAlternate** [ **0|1**]  
 Spécifie si l'Agent de fusion se synchronise avec un Abonné ou un autre serveur de publication. La valeur **1** indique qu'il s'agit d'un autre serveur de publication. La valeur par défaut est **0**.  
  
 **-UploadGenerationsPerBatch** *upload_generations_per_batch*  
 Nombre de générations à traiter dans un lot unique lors du téléchargement des modifications de l'Abonné au serveur de publication. Une génération est définie comme un groupe logique de modifications par article. La valeur par défaut pour un lien de communication fiable est **100**. La valeur par défaut pour un lien de communication non fiable est **1**.  
  
 **-UploadReadChangesPerBatch** *upload_read_changes_per_batch*  
 Nombre de modifications à lire dans un lot unique lors du téléchargement des modifications de l'Abonné au serveur de publication. La valeur par défaut est **100**.  
  
 **-UploadWriteChangesPerBatch** *upload_write_changes_per_batch*  
 Nombre de modifications à appliquer dans un lot unique lors du téléchargement des modifications de l'Abonné au serveur de publication. La valeur par défaut est **100**.  
  
 **-UseInprocLoader**  
 Améliore les performances de l'instantané initial en forçant l'Agent de fusion à utiliser la commande BULK INSERT lors de l'application des fichiers d'instantanés à l'Abonné. Ce paramètre est déconseillé parce qu'il n'est pas compatible avec le type de données XML. Si vous ne répliquez pas de données XML, ce paramètre peut être utilisé. Ce paramètre ne peut pas être utilisé avec les instantanés en mode caractère. Si vous utilisez ce paramètre, le compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] au niveau de l'Abonné doit posséder des autorisations en lecture sur le répertoire où se trouvent les fichiers de données .bcp d'instantané. Lorsque ce paramètre n'est pas utilisé, le pilote ODBC chargé par l'agent lit les fichiers. Le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'est donc pas utilisé.  
  
 **-Validate** [**0**|**1**|**2**|**3**]  
 Spécifie si la validation doit être réalisée à la fin de la session de fusion et, le cas échéant, le type de validation à utiliser. La valeur **3** est recommandée.  
  
|Valeur Validate|Description|  
|--------------------|-----------------|  
|**0** (valeur par défaut)|Pas de validation|  
|**1**|Validation du décompte de lignes uniquement.|  
|**2**|Validation du nombre de lignes et de la somme de contrôle.|  
|**3**|Validation du nombre de lignes et de la somme de contrôle binaire.|  
  
> [!NOTE]  
>  Une validation utilisant la somme de contrôle binaire ou la somme de contrôle peut signaler de façon incorrecte un échec si les types de données sont différents entre l'Abonné et le serveur de publication. Pour plus d’informations, consultez la section « Considérations sur la validation des données » dans la rubrique [Valider des données répliquées](../../../relational-databases/replication/validate-replicated-data.md).  
  
 **-ValidateInterval** *validate_interval*  
 Fréquence, en minutes, de validation de l'abonnement en mode continu. La valeur par défaut est **60** minutes.  
  
## <a name="remarks"></a>Notes   
  
> [!IMPORTANT]  
>  Si vous avez installé l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour s'exécuter sous un compte système local plutôt que sous un compte d'utilisateur de domaine (option par défaut), le service peut uniquement accéder à l'ordinateur local. Si l'Agent de fusion qui s'exécute sous l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est configuré pour utiliser le mode d'authentification Windows lorsqu'il se connecte à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'Agent de fusion échoue. Le paramètre par défaut est l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Pour démarrer l'Agent de fusion, exécutez **replmerg.exe** à l'invite de commandes. Pour plus d'informations, consultez [Concepts des exécutables de l'agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
 L'historique de l'agent de fusion pour la session active n'est pas supprimé lors de l'exécution en mode continu. Un agent dont l'exécution est longue peut générer un grand nombre d'entrées dans les tables de l'historique de fusion, ce qui peut impacter les performances. Pour résoudre ce problème, passez en mode planifié, ou continuez à utiliser le mode continu mais créez une tâche dédiée pour redémarrer régulièrement l'agent de fusion, ou baissez le niveau de détail de l'historique pour réduire le nombre de lignes et, par conséquent, l'impact sur les performances.  
  
## <a name="see-also"></a> Voir aussi  
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
