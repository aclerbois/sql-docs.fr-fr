---
title: Options de démarrage du service moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 11/23/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- single-user mode [SQL Server], startup option
- overriding default startup options
- minimal configuration mode [SQL Server], startup option
- default startup options
- temporarily override default startup options [SQL Server]
- startup options [SQL Server]
- starting SQL Server, options
- single-user mode [SQL Server], startup parameter
- overriding default startup parameters
- minimal configuration mode [SQL Server], startup parameter
- default startup parameters
- temporarily override default startup parameters [SQL Server]
- startup parameters [SQL Server]
- starting SQL Server, parameters
ms.assetid: d373298b-f6cf-458a-849d-7083ecb54ef5
caps.latest.revision: 80
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 5cabb4763e85f8f1d052aa797ec6f868d9dac0aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-engine-service-startup-options"></a>Options de démarrage du service moteur de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les options de démarrage désignent certains emplacements de fichiers nécessaires lors du démarrage et spécifient certaines conditions applicables à l'échelle du serveur. La plupart des utilisateurs n'ont pas besoin de spécifier d'options de démarrage, à moins de dépanner le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou d'avoir un problème inhabituel et que le support technique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne leur demande d'utiliser une option de démarrage.  
  
> [!WARNING]  
>  Toute utilisation incorrecte des options de démarrage peut affecter les performances du serveur et empêcher le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
>
>  Démarrez SQL Server sur Linux avec l’utilisateur « mssql » afin d’éviter les problèmes de démarrage futurs. Exemple : `sudo -u mssql /opt/mssql/bin/sqlservr [STARTUP OPTIONS]` 
  
## <a name="about-startup-options"></a>À propos des options de démarrage  
 Lorsque vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le programme d'installation inscrit un ensemble d'options de démarrage par défaut dans le Registre de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Vous pouvez utiliser ces options de démarrage pour spécifier un autre fichier de base de données master, un autre fichier journal de base de données master ou un autre fichier journal des erreurs. Si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas localiser les fichiers nécessaires, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne démarre pas.  
  
 Il est possible de définir les options de démarrage en utilisant le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Configurer les options de démarrage du serveur &#40;SQL Server Configuration Manager&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  
  
## <a name="list-of-startup-options"></a>Liste des options de démarrage  
### <a name="default-startup-options"></a>Options de démarrage par défaut  
|Options|Description|  
|-----------------------------|-----------------|  
|**-d**  *master_file_path*|Représente le chemin d’accès complet au fichier de base de données MASTER (il s’agit généralement de C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\master.mdf). Si vous ne spécifiez pas cette option, les paramètres du Registre existant sont utilisés.|  
|**-e**  *error_log_path*|Représente le chemin d’accès complet au fichier journal des erreurs (il s’agit généralement de C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\LOG\ERRORLOG). Si vous ne spécifiez pas cette option, les paramètres du Registre existant sont utilisés.|  
|**-l**  *master_log_path*|Représente le chemin d’accès complet au fichier journal de la base de données MASTER (il s’agit généralement de C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\mastlog.ldf). Si vous ne spécifiez pas cette option, les paramètres de Registre existants sont utilisés.|  
  
### <a name="other-startup-options"></a>Autres options de démarrage   
|Options |Description|   
|---------------------------|-----------------|  
|**-c**|Raccourcit la durée nécessaire au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de l'invite de commandes. En général, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] démarre en tant que service en appelant le gestionnaire de contrôle de services. Comme [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ne démarre pas en tant que service pendant le démarrage à partir de l’invite de commandes, vous devez utiliser **-c** pour ignorer cette étape.|  
|**-f**|Démarre une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec une configuration minimale. Cette option est utile lorsqu'une valeur de configuration définie (espace mémoire insuffisant, par exemple) a empêché le serveur de démarrer. Le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode de configuration minimal place [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur. Pour plus d’informations, consultez la description de l’option **-m** ci-après.|  
|**g -**  *mémoire_à_réserver*|Spécifie un nombre entier de mégaoctets (Mo) de mémoire que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] garde disponible pour les allocations de mémoire dans le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais en dehors du pool de mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] défini par le paramètre serveur [max_server_memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md). La mémoire en dehors du pool de mémoire est la zone utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le chargement d'éléments, tels que les fichiers .dll des procédures stockées étendues, les fournisseurs OLE DB référencés par les requêtes distribuées et les objets automation référencés dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . La valeur par défaut est 256 Mo.<br /><br /> Cette option peut faciliter l'ajustement de l'allocation de mémoire, mais uniquement lorsque la mémoire physique dépasse la limite configurée définie par le système d'exploitation sur la mémoire virtuelle disponible pour les applications. L'utilisation de cette option peut s'avérer appropriée avec des configurations de mémoire importantes, dans lesquelles les besoins en utilisation de la mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas standard et l'espace d'adressage virtuel du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisé dans son intégralité. L'utilisation incorrecte de cette option peut mener à des situations dans lesquelles une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne démarre pas ou rencontre des erreurs lors de l'exécution.<br /><br /> Utilisez la valeur par défaut du paramètre **-g** , sauf si l’un des avertissements suivants figure dans le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :<br /> « Échec de l’allocation d’octets pour la réserve virtuelle : FAIL_VIRTUAL_RESERVE \<taille> »<br /> « Échec de l’allocation d’octets pour la réserve virtuelle : FAIL_VIRTUAL_COMMIT \<taille> »<br /><br /> Ces messages peuvent indiquer que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de libérer des parties du pool de la mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de trouver de l'espace pour des éléments tels que les fichiers .dll de procédure stockée étendue ou des objets Automation. Dans ce cas, envisagez d’augmenter la quantité de mémoire réservée par le commutateur **-g** .<br /><br /> L'utilisation d'une valeur inférieure à celle par défaut augmente la quantité de mémoire disponible dans le pool de mémoires géré par le Gestionnaire de mémoire de SQL Server ou les piles de threads ; cela permet de profiter de l'amélioration des performances pour les charges de travail qui ont recours de manière intensive à la mémoire dans les systèmes qui n'utilisent pas de nombreuses procédures stockées étendues, requêtes distribuées ou objets automation.|  
|**-m**|Démarre une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur. Lorsque vous démarrez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur, un seul utilisateur peut se connecter et le processus CHECKPOINT n'est pas démarré. CHECKPOINT garantit l'écriture régulière des transactions terminées de la mémoire cache du disque vers l'unité de base de données. (Cette option est généralement utilisée si vous rencontrez des problèmes avec des bases de données système qui doivent être réparées). Active l’option sp_configure allow updates. Par défaut, l'option allow updates est désactivée. Le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur permet à tout membre du groupe Administrateurs local de l'ordinateur de se connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en tant que membre du rôle serveur fixe sysadmin. Pour plus d’informations, consultez [Se connecter à SQL Server lorsque les administrateurs système n’y ont plus accès](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md). Pour plus d’informations sur le mode mono-utilisateur, consultez [Démarrer SQL Server en mode mono-utilisateur](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).|  
|**Nom de l’application -mClient**|Limite les connexions à une application cliente spécifiée. Par exemple, `-mSQLCMD`  limite les connexions à une connexion unique, laquelle doit s’identifier en tant que programme client SQLCMD. Utilisez cette option lorsque vous démarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur et qu'une application cliente inconnue utilise la seule connexion disponible. Utilisez `"Microsoft SQL Server Management Studio - Query" ` pour se connecter à l’éditeur de requête SSMS. L’option de l’éditeur de requête SSMS ne peut pas être configurée à l’aide de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Configuration Manager, car il comprend un tiret, qui est un caractère refusé par l’outil.<br /><br /> Le nom de l'application cliente respecte la casse. Des guillemets doubles sont requis si le nom de l’application contient des espaces ou des caractères spéciaux.<br /><br />**Exemples lors du démarrage à partir de la ligne de commande :**<br /><br />`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\sqlserver -s MSSQLSERVER -m"Microsoft SQL Server Management Studio - Query"` <br /><br />`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\sqlserver -s MSSQLSERVER -mSQLCMD` <br /><br /> **Note de sécurité :** N’utilisez pas cette option comme fonctionnalité de sécurité. L'application cliente fournit le nom d'application cliente et peut fournir un nom erroné dans la chaîne de connexion.|  
|**-n**|N'utilise pas le journal des applications Windows pour enregistrer les événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous démarrez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec **-n**, nous vous recommandons d’utiliser également l’option de démarrage **-e** . Sinon, les événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne seront pas consignés.|  
|**-s**|Permet de démarrer une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le paramètre **-s** n’est pas défini, l’instance par défaut va tenter de démarrer. Vous devez accéder au répertoire BINN de l’instance, dans l’invite de commandes, avant de démarrer **sqlservr.exe**. Par exemple, si Instance1 doit utiliser `\mssql$Instance1` pour ses fichiers binaires, l’utilisateur doit être dans le répertoire `\mssql$Instance1\binn` pour démarrer **sqlservr.exe -s instance1**.|  
|**-T**  *trace#*|Indique qu’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être démarrée avec un indicateur de trace spécifique (*trace#*) en vigueur. Les indicateurs de trace permettent de démarrer le serveur avec un comportement non standard. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).<br /><br /> **Important :** Lorsque vous spécifiez un indicateur de trace à l’aide de l’option **-T**, utilisez un « T » majuscule pour transmettre le numéro de l’indicateur de trace. Le caractère minuscule « t » est accepté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais il permet de définir d'autres indicateurs de trace internes qui ne servent qu'aux ingénieurs du support technique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . (Les paramètres spécifiés dans la fenêtre de démarrage du Panneau de configuration ne sont pas lus.)|  
|**-x**|Désactive les fonctionnalités d'analyse suivantes :<br /> - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les compteurs de l’Analyseur de performances<br /> - Le suivi des statistiques relatives au temps processeur et au taux d’accès au cache<br /> - La collecte d’informations pour la commande DBCC SQLPERF<br /> - La collecte d’informations pour certaines vues de gestion dynamique<br /> - De nombreux points d’événements étendus<br /><br /> **Avertissement :**  Lorsque vous utilisez l’option de démarrage **–x**, les informations dont vous disposez pour diagnostiquer les problèmes de performance et de fonctionnement avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont considérablement limitées.|  
|**-E**|Augmente le nombre d'étendues allouées à chaque fichier d'un groupe de fichiers. Cette option peut être utile pour les applications d'entrepôt de données qui ont un nombre limité d'utilisateurs exécutant des index ou des analyses de données. Elle ne doit pas être utilisée dans d'autres applications car elle peut diminuer les performances. Cette option n'est pas prise en charge dans les versions 32 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="using-startup-options-for-troubleshooting"></a>Utilisation des options de démarrage pour le dépannage  
 Certaines options de démarrage, telles que le mode mono-utilisateur et le mode de configuration minimale, sont principalement utilisées pour le dépannage. Le démarrage du serveur à des fins de dépannage avec les options **–m** ou **–f** est plus facilement réalisable sur la ligne de commande pendant le démarrage manuel de sqlservr.exe.  
  
> [!NOTE]  
>  Au moment d’un démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec l’instruction **net start**, les options de démarrage utilisent une barre oblique (/) au lieu d’un trait d’union (-).  
  
## <a name="using-startup-options-during-normal-operations"></a>Utilisation des options de démarrage au cours d'opérations normales  
 Il peut être intéressant d'utiliser certaines options de démarrage à chaque fois que vous démarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces options, telles que **–g** ou le démarrage avec un indicateur de trace, sont plus facilement mises en œuvre par la configuration des paramètres de démarrage au moyen du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cet outil enregistre les options de démarrage sous forme de clés du Registre, de sorte que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre toujours en tenant compte des options de démarrage.  
  
## <a name="compatibility-support"></a>Prise en charge de la compatibilité  
 Le paramètre **-h**  n’est pas pris en charge dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Ce paramètre a été utilisé dans les versions antérieures des instances 32 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour réserver l'espace d'adressage de mémoire virtuelle pour les métadonnées d'ajout de mémoire à chaud lorsque AWE est activé. Pour plus d’informations, consultez [Fonctionnalités SQL Server supprimées dans SQL Server 2016](http://msdn.microsoft.com/library/0678bfbc-5d3f-44f4-89c0-13e8e52404da).  
  
## <a name="related-tasks"></a>Related Tasks  
[Configurer l'option de configuration du serveur scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md)  
[Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)
[Configurer les options de démarrage du serveur &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
  
## <a name="see-also"></a> Voir aussi  
 [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [Application sqlservr](../../tools/sqlservr-application.md)  
  
  
